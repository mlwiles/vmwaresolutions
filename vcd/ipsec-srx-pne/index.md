## vCD - IPSec Tunnel over IBM Private Network Endpoint (PNE) using SRX

Updated: 2021-02-09

Based on the initial example of [IPSec Tunnel over IBM Private Network Endpoint (PNE) using ESG](https://mlwiles.github.io/vmwaresolutions/vcd/ipsec-esg-pne/), this modification of the example will change the inbound 'external' or 'inbound' side of the IPSec tunnel which was using ESG and to now be using SRX.

In this example we replace the vCloud Directory with ESG in Frankfurt ...
<img src="images/1-vdc-pne-ipsec.png" width="1000" style="border: 1px solid black">

with a vCenter and an Juniper SRX.
<img src="images/2-vdc-pne-ipsec.png" width="1000" style="border: 1px solid black">

To do so, we have deployed an SRX appliance in the vCenter in FRA (Juniper appliance image - vsrx_20.4r1.12_scsi)

### The Datacenter information on the left:
- Physical datacenter: Dallas, TX, USA
- vCloud Director virtual datacenter: mwiles-dal-v10
- Edge Service Gateway address: 52.117.132.152
- VM network: 172.16.0.0/16
- VM (centos-dal): 172.16.0.2
- PNE address: 166.9.48.94

### The Datacenter information on the right:
- Physical datacenter: Frankfurt, Germany
- vCenter instance with VM: vsrx
- IBM Cloud IP of the vsrx: 10.240.232.10
- VM network: 172.15.0.0/16
- VM (centos-fra): 172.15.0.2

## Configuring the Dallas vDC.
### Same network in Dallas vDC from the referenced example, no changes...

## Configuring the Frankfurt vCenter instance (SRX)

The configuration below provides the FRA side of the tunnel.  This descibes the interfaces and the routes for the example.

```
interfaces {
    ge-0/0/0 {
        unit 0 {
            family inet {
                address 172.15.0.1/16;
            }
        }
    }
    ge-0/0/1 {
        unit 0 {
            family inet {
                address 10.240.232.10/24;
            }
        }
    }
    fxp0 {
        unit 0;
    }
    st0 {
        unit 1 {
            family inet;
        }
    }
}
routing-options {
    static {
        route 10.0.0.0/8 next-hop 10.240.232.1;
        route 166.8.0.0/14 next-hop 10.240.232.1;
        route 172.16.0.0/16 next-hop st0.1;
    }
}
```

Next we will define the IPsec configuration per our example.

```
security {
    ike {
        proposal cse-test-proposal {
            authentication-method pre-shared-keys;
            dh-group group5;
            authentication-algorithm sha1;
            encryption-algorithm aes-128-cbc;
            lifetime-seconds 900;
        }
        policy cse-test-policy {
            mode main;
            proposals cse-test-proposal;
            pre-shared-key ascii-text "$9$.PznuOIyrK4an9AuREws24JU3n9CpOxNaG"; ## SECRET-DATA
        }
        gateway cse-test-gateway {
            ike-policy cse-test-policy;
            address 166.9.48.94;
            local-identity hostname ipsec-fra-SRX;
            remote-identity hostname ipsec-dal;
            external-interface ge-0/0/1.0;
        }
    }
    ipsec {
        proposal cse-test-proposal {
            protocol esp;
            authentication-algorithm hmac-sha1-96;
            encryption-algorithm aes-128-cbc;
            lifetime-seconds 300;
        }
        policy cse-test-policy {
            proposals cse-test-proposal;
        }
        vpn cse-test-vpn {
            bind-interface st0.1;
            ike {
                gateway cse-test-gateway;
                proxy-identity {
                    local 172.15.0.0/16;
                    remote 172.16.0.0/16;
                    service any;
                }
                ipsec-policy cse-test-policy;
            }
            establish-tunnels immediately;
        }
    }
}
```

Finally we will define the security zones and policies per our example.

```
security {
    policies {
        from-zone trust to-zone trust {
            policy default-permit {
                match {
                    source-address any;
                    destination-address any;
                    application any;
                }
                then {
                    permit;
                }
            }
            policy VPN-IN {
                match {
                    source-address NET-172.16.0.0/16;
                    destination-address NET-172.15.0.0/16;
                    application any;
                }
                then {
                    permit;
                }
            }
            policy VPN-OUT {
                match {
                    source-address NET-172.15.0.0/16;
                    destination-address NET-172.16.0.0/16;
                    application any;
                }
                then {
                    permit;
                }
            }
        }
    }
    zones {
        security-zone trust {
            address-book {
                address NET-172.15.0.0/16 172.15.0.0/16;
                address NET-172.16.0.0/16 172.16.0.0/16;
            }
            host-inbound-traffic {
                system-services {
                    all;
                }
            }
            interfaces {
                ge-0/0/0.0;
                ge-0/0/1.0;
                st0.1;
            }
        }
    }
}

```

You can check the status of the IPsec tunnel on the SRX device by checking for ike and ipsec security associations. Typically you should expect to see one `ike SA`, and one *pair* of `ipsec SAs` for each remote-subnet/local-subnet combination.
```
show security ike security-associations
show security ipsec security-associations
```
<img src="images/3-vdc-pne-ipsec.png" width="1000" style="border: 1px solid black">

## Test the tunnel

From the web-console, we log into the VMs in each side.  Then ensure we can ssh/ping the other VM as shown in the screenshot.

<img src="images/4-vdc-pne-ipsec.png" width="1000" style="border: 1px solid black">


_Note the information described in this example are guidelines.  There are multiple ways to configure the various parts of the example.  Please adjust accordingly for your needs._

_Additional note this SRX is not secured/locked down as would be recommended in real life. So bear that in mind when using as an example_

Credit:  [Nigel Vander Houwen](https://www.linkedin.com/in/nigel-vander-houwen-1a0a1626/) for his Network expertise!

[VMWare vCloud Director](https://mlwiles.github.io/vmwaresolutions/vcd/)<br/>
[Main Page](https://mlwiles.github.io/vmwaresolutions)