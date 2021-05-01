## vCD - Secure inbound access

Updated: 2021-04-25

### <a name="toc"></a>Table of Contents:
  - [Overview](#overview)
  - [Finished Picture](#finished)
  - [Data Collection](#data)
  - [Public IP](#publicip)
  - [vDC Network](#vdcnetwork)
  - [Virtual Machine IP](#vmip)
  - [Allow Access inbound](#allow)
  - [Test the rule](#test)

###  <a name="overview"></a>Overview

If you are finding that the Web Console in your vDC is not quite meeting your needs:<br>
Compute > Virtual Machine > MACHINE > Actions > Launch Web Console

<img src="images/0-webconsole.png" style="border: 1px solid black">

then you can configure SECURE inbound access to your VM.  This is a great way to take advantage of copy / paste, larger resolution, or just working on your environment without the requirement of the admin portal.

For more details on allowing your VMs Internet or IBM Cloud services access see [Internet and IBM Cloud services access](https://mlwiles.github.io/vmwaresolutions/vcd/outbound/).

Back to: [Menu](#toc)

### <a name="finished"></a>Finished Picture

The end goal is to open your Edge Service Gateway (ESG) to securely enable you to connect either via Secure Shell (SSH), Remote Desktop (RDP), etc ... to your VM(s).

Below is a use case where we will configure inbound SSH from a whitelisted (HOME) ip address to our vm in our vDC.

<img src="images/1-complete.png" style="border: 1px solid black">

Back to: [Menu](#toc)

### <a name="data"></a>Data Collection 

For this example, we need the following information:
- Home or remote IP address (e.g. [What's my IP](https://whatismyipaddress.com/)): `75.183.214.216`
- Public IP address for my ESG (see below): `169.59.231.66`
- Virtual Machine IP: (see below): `172.16.10.22`

Back to: [Menu](#toc)

### <a name="publicip"></a>Public IP address range can be found on your ESG

Networking > Edges > EDGENAME > External Networks > IP Allocations<br>
There will be 5 IP addresses assigned to your ESG at creation time.  Any of these will work for this purpose.  Select one of the IPs to use as inbound IP:  169.59.231.66

<img src="images/2-esg-publicips.png" style="border: 1px solid black">

Back to: [Menu](#toc)

### <a name="vdcnetwork"></a>vDC Network

You must create a network if one does not already exist that will be used to route the inbound traffic.

For this example, I created a network with the following criteria:
- Gateway CIDR: `172.16.10.1/24`
- Type: `Routed`
- Connection Type: `Subinterface`
- Static IP Pool: `172.16.10.10-172.16.10.20`

Review [vCD -   Networks made easy](https://mlwiles.github.io/vmwaresolutions/vcd/network101/) for additional information on how to create a network.

<img src="images/3-network.png" style="border: 1px solid black">

If using vApps, the network must be attached to the vApp.  

Compute > vApps > Networks > New

<img src="images/4-vapp-network.png" style="border: 1px solid black">

OrgVDC Network > 172.16.10.0/24 (in the case of this example)

<img src="images/5-vapp-network.png" style="border: 1px solid black">

Back to: [Menu](#toc)

### <a name="vmip"></a>Virtual Machine IP

Make sure you have a VM attached to the Network and assign an IP

Compute > vApps > Virtual Machine > MACHINE > Hardware > NICs > Edit

<img src="images/6-vm-network.png" style="border: 1px solid black">

Select the NIC
- Primary NIC
- Connected
- Network (`172.16.10.0/24` in the case of this example)
- IP Mode (`Static - Manual` for this example)
- IP (`172.16.10.22` in the case of this example)

<img src="images/7-vm-network.png" style="border: 1px solid black">

Once set, Force Customization on the VM to have VMWare Tools setup the networking.

<img src="images/8-vm-poweron.png" style="border: 1px solid black">

Back to: [Menu](#toc)

### <a name="allow"></a>Allow Access inbound

Create the ESG Firewall rule to allow the inbound traffic.  In this case we are going to allow traffic from the Remote IP on Port 2222

Networking > Edges > EDGENAME > Services<br>

<img src="images/9-esg-firewall.png" style="border: 1px solid black">

From the Firewall tab, select the `+` to add a new rule.  Edit the contents of the rule:
- Name: `Inbound Secure`
- Source: Remote IP address (for this example `75.183.214.216`)
- Destination: Where to allow traffic (for this example `Any`)
- Service: What service to allow (for this example TCP with a destination port of `2222`)

Don't forget to `Save changes`

<img src="images/10-esg-firewall.png" style="border: 1px solid black">

Create the ESG DNAT rule to change the destination of traffic from port 2222 on the external network to 22 on the internal network, select `+ DNAT` button to add a new DNAT rule:
- Applied on: `tenant external network`
- Original IP Address: Public IP address for my ESG: `169.59.231.66`
- Original Port: `2222`
- Translated IP Address: IP of the VM: `172.16.10.22`
- Translated Port: `22`

Don't forget to `Save changes`

<img src="images/11-esg-dnat.png" style="border: 1px solid black">

Back to: [Menu](#toc)

### <a name="test"></a>Test the rule

To test the rule, I will open a terminal on my local machine and try to ssh to the machine:<br>
`ssh root@169.59.231.66 -p 2222`

<img src="images/12-test.png" style="border: 1px solid black">

For more details on deploying VMs see [vCD - Simple Deploy of a VM](https://mlwiles.github.io/vmwaresolutions/vcd/vm101/).

Back to: [Menu](#toc)

_Note the information described in this example are guidelines.  There are multiple ways to configure the various parts of the example.  Please adjust accordingly for your needs._

[VMWare vCloud Director](https://mlwiles.github.io/vmwaresolutions/vcd/)<br/>
[Main Page](https://mlwiles.github.io/vmwaresolutions)