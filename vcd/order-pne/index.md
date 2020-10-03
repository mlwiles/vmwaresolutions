## Order a Private Network Endpoint (PNE) to connect your IBM account to your IBM VMWare Solutions Shared virtual datacenter

There is an offering from IBM VMWare Solutions that allows communication between an IBM Cloud account and IBM VMWare Solutions Shared taking advantage of the IBM Cloud backbone.  This provides a fast connection between the accounts as well as avoids any ingress/egress charges.  To enabled this, from the IBM VMWare Solutions Shared virtual data center select 'Create a private network endpoint' as seen in the screenshot below.

<img src="images/1-pne.png" width="1000" style="border: 1px solid black">

For the free version, select Multi-tentant.  This option runs on shared hardware and the throughput is not gurarenteed.  For faster and guarenteed throughput, check out the Dedicated options.
In order to allow the IBM account to have access, you will need to identify which IP(s) or subnet(s) can have access.  This does not automatically allow all of IBM Cloud to have access to your vDC, this just allows them to pass through the PNE.  The default firewall rules on your edge gateway prevents inbound traffic.  So in this example we allow all of IBM Cloud private (10.0.0.0/8) but will later show how to use the firewall rules to allow only endpoints we desire.

<img src="images/2-pne.png" width="200" style="border: 1px solid black">

Once created, the PNE will show the information that is required for firewall rules.  Note the 52. ip address is the internal address that is used by IBM VMWare Solutions Shared.  This is NOT a public IP.  Also the 166.9 IP will be your PNE address will be important for your firewall rules.  Additionally the IP/subnet that you whitelisted at ordertime.  We do not allow changes via the UI, this will be ticket based at this time.

<img src="images/3-pne.png" width="1000" style="border: 1px solid black">

Once ordered and configured you will be able to take advantage of the unlimited access from your whitelisted accounts into the virtual datacenter. 