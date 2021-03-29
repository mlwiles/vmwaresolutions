## VAS - Restore from backup

Updated: 2021-01-13

Once you have [Setup a simple backup job](https://mlwiles.github.io/vmwaresolutions/vas/backup/index.md), the next set of steps will be to test or restore from the backup.  If you recall in the backup job we focused on two types of items to add.

1. a vApp (which is the vApp and *ALL* of the Virtual Machines in it)
2. a VM (this will also include information about the vApp the VM is contained in) 

Knowing the types of backups that we have in place, we can now look at the use cases in which we can restore.  We can confirm our requirements are met, or take an action item to modify existing backups or add additional.  

### Backup and Restore matrix

The following is a table that reflects what use cases are covered when trying to restore from a backup job.

The header describes the type of VM/vApp backup job was created as well as the types of restores that are compatible with the backup jobs.

----------------

| vCD Action<br>Create  | Veeam<br>Job Type | vCD Action<br>Delete | Restore<br>VM Keep | Restore<br>VM Overwrite | Restore<br>vApp Keep	| Restore<br>vApp Overwrite |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Create vApp & VM** |  |  |  |  |  |  |
|  | **Backed up VM**  |  |  |  |  |  |
|  |  | **Nothing** | *Supported* | *Supported* | *Supported* | *Supported* |    
|  |  | **VM (only)** | *Supported* | *Supported* | *Supported* | *Supported* |
|  |  | **vApp and VM** | ***NOT** Supported* | ***NOT** Supported* | *Supported* | *Supported* |
|  | **Backed up vApp<br>(includes VM)**  |  |  |  |  |  |
|  |  | **Nothing** | *Supported* | *Supported* | *Supported* | *Supported* |
|  |  | **VM (only)** | *Supported* | *Supported* | *Supported* | *Supported* |
|  |  | **vApp and VM** | ***NOT** Supported* | ***NOT** Supported* | *Supported* | *Supported* |
| **Create VM Only<br>(no explicit vApp)** |  |  |  |  |  |  |
|  | **Backed up VM**  |  |  |  |  |  |
|  |  | **Nothing** | *Supported* | *Supported* | *Supported* | *Supported* |
|  |  | **VM (only)** | ***NOT** Supported* | ***NOT** Supported* | *Supported* | *Supported* |
|  |  | **vApp and VM** | ***NOT** Applicable* | ***NOT** Applicable* | ***NOT** Applicable* | ***NOT** Applicable* |
|  | **Backed up vApp<br>(includes VM)**  |  |  |  |  |  |
|  |  | **Nothing** | *Supported* | *Supported* | *Supported* | *Supported* |
|  |  | **VM (only)** | ***NOT** Supported* | ***NOT** Supported* | *Supported* | *Supported* |
|  |  | **vApp and VM** | ***NOT** Applicable* | ***NOT** Applicable* | ***NOT** Applicable* | ***NOT** Applicable* |

----------------

- *Supported* = The use case and is expected to work, it not working please open a ticket.<br>
- ***NOT** Applicable* = no vApp visible to delete<br>
- ***NOT** Supported* = VM cannot be restored to the original location, because it is no longer available.<br>
----------------
Once you have had a chance to review this table, lets look at a couple of examples on how to restore successfully.

### Restore and Keep



### Restore and Replace



_Note the information described in this example are guidelines.  There are multiple ways to configure the various parts of the example.  Please adjust accordingly for your needs._

[Veeam Availability Suite](https://mlwiles.github.io/vmwaresolutions/vas/)<br/>
[Main Page](https://mlwiles.github.io/vmwaresolutions)