## VCCR - Disaster Recovery Prep

Updated: 2020-12-29

Now that the Replication job has either completed once successfully or has been running on a scheduled cadence via a [Simple Replication job to vCD](https://mlwiles.github.io/vmwaresolutions/vccr/vcd-job/), its now time to prepare for failover testing and/or execution.

First we re-evaluate that the CCR job is running successfully... 

<img src="images/01-replication-job.png" width="1000" style="border: 1px solid black">

and has created the VM in the vDC.

<img src="images/02-replication-job.png" width="1000" style="border: 1px solid black">

Behind the scenes, what is not necessarily known to the user is that there are snapshots created by Veeam that map one-to-one to the Restore Points on the replication job.  During replication (or sometimes when there is a network failure) the service provider can observe a snapshot with the name `Veeam Replica Working Snapshot`.  In a normal replication, this is fine, but when something causes the replication to be interrupted, its up to the service provider to notify or proactively cleanup the orphaned snapshot.

<img src="images/03-replication-job.png" width="1000" style="border: 1px solid black">

Once a successful replication has been completed, the snapshot will be renamed to the format `Restore Point MM-DD-YYYY HH:mm:ss XM` and this will corollate to the restore points referenced from the Customer VBR on-premises.

<img src="images/04-replication-job.png" width="1000" style="border: 1px solid black">

Now to either create a temporary test or execute a failover permanently, the customer will need to create a `Failover Plan` from the on-premises VBR.

<img src="images/05-failover-vbr.png" width="1000" style="border: 1px solid black">

Provide the Failover Plan a unique and meaningful name.

<img src="images/06-failover-vbr.png" width="1000" style="border: 1px solid black">

Select the VM(s) from the available list ...

<img src="images/07-failover-vbr.png" width="1000" style="border: 1px solid black">

In this case, we select the previously replicated VM `VCCRTEST-mwiles1`

<img src="images/08-failover-vbr.png" width="1000" style="border: 1px solid black">

Add the VM(s).

<img src="images/09-failover-vbr.png" width="1000" style="border: 1px solid black">

Summary 

<img src="images/10-failover-vbr.png" width="1000" style="border: 1px solid black">

To begin the Test or the Migration, select the plan and then `Start`

<img src="images/11-failover-vbr.png" width="1000" style="border: 1px solid black">

You will see the Plan has began and update for the on-prem VBR.  This will power on the VM.

<img src="images/12-failover-vbr.png" width="1000" style="border: 1px solid black">

One thing to note, in the even of a complete disaster of the on-premises VBR, the Failover plan can also be started from the VCCR Self-Service Portal.  One place the link can be found is from the vCD portal.

<img src="images/22-failover-ssp.png" width="1000" style="border: 1px solid black">

Log-in with the `ORGID\Username` and respective password.

<img src="images/23-failover-ssp.png" width="1000" style="border: 1px solid black">

Select the `Failover Plan` and `Start`

<img src="images/24-failover-ssp.png" width="1000" style="border: 1px solid black">

Status will be reflected in the portal UI.

<img src="images/25-failover-ssp.png" width="1000" style="border: 1px solid black">

Next would be to either stop the Failover Plan, or begin the Permanent Failover to fully [Migration to vCD](https://mlwiles.github.io/vmwaresolutions/vccr/vcd-migration/)

_Note the information described in this example are guidelines.  There are multiple ways to configure the various parts of the example.  Please adjust accordingly for your needs._

[Veeam Cloud Connect Replication](https://mlwiles.github.io/vmwaresolutions/vccr/)<br/>
[Main Page](https://mlwiles.github.io/vmwaresolutions)

