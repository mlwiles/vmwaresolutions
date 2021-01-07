## VAS - Restore from backup

<!-- 
Updated: 2021-01-05
--> 

### Backup and Restore matrix

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

- *Supported* = The use case and is expected to work, it not working please open a ticket.<br>
- ***NOT** Applicable* = no vApp visible to delete<br>
- ***NOT** Supported* = VM cannot be restored to the original location, because it is no longer available.<br>


### Restore and Keep

### Restore and Replace



_Note the information described in this example are guidelines.  There are multiple ways to configure the various parts of the example.  Please adjust accordingly for your needs._

[Veeam Availability Suite](https://mlwiles.github.io/vmwaresolutions/vas/)<br/>
[Main Page](https://mlwiles.github.io/vmwaresolutions)