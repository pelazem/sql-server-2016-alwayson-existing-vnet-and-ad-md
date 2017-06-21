# Create a SQL Server 2016 AlwaysOn Availability Group (AOAG) in an _existing_ Azure VNet and Active Directory domain.

This template will create a SQL Server 2016 AlwaysOn Availability Group and configure the VMs using Powershell DSC extensions. 

This template creates the following resources:
+	One internal load balancer (ILB)
+	Three storage accounts
+	Three VMs in a Windows Server Failover Cluster (WSFC)
	+	Two VMs run SQL Server 2016 (primary and synchronous secondary replica)
	+	The third VM is the File Share Witness for the cluster
+	One Availability Set for the SQL and Witness VMs

The ILB points at the AOAG Listener.

If the required Azure VNet and Active Directory infrastructure is not yet in place, it can be deployed using <a href="https://github.com/Azure/azure-quickstart-templates/tree/master/active-directory-new-domain-ha-2-dc">this template</a>.

## Notes

+	The default settings for storage are to deploy using **premium storage**.  The SQL VMs use two P30 disks each (for data and log).  These sizes can be changed by changing the relevant variables. In addition there is a P10 Disk used for each VM OS Disk.

+ 	The default settings for compute require that you have at least 9 cores of free quota to deploy.

+ 	The images used to create this deployment are
	+ 	SQL Server - Latest SQL Server 2016 on Windows Server 2016 Image
	+ 	Witness - Latest Windows Server 2016 Image

+ 	The image configuration is defined in variables, but the scripts that configure this deployment have only been tested with these versions and may not work on other images.

+	To successfully deploy this template:
	+	Ensure the subnet to which these VMs will be deployed already exists
	+	Ensure the subnet has network communication with the AD domain controllers, including checking VNet-to-VNet peering and any applicable Network Security Group (NSG) rules
	+	Deploy to a subnet that does NOT already have an AOAG (including the ILB) deployed to it
	+	Recommended: ensure that the subnet to be deployed to is defined in Active Directory Sites and Services


Click the button below to deploy from the portal

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fpelazem%2Fsql-server-2016-alwayson-existing-vnet-and-ad-md%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>


## Deploying Sample Templates

You can deploy these samples directly through the Azure Portal or by using the scripts supplied in the root of the repo.

To deploy a sample using the Azure Portal, click the **Deploy to Azure** button found in the README.md of each sample.

To deploy the sample via the command line (using [Azure PowerShell or the Azure CLI](https://azure.microsoft.com/en-us/downloads/)) you can use the scripts.

Simple execute the script and pass in the folder name of the sample you want to deploy.  For example:

```PowerShell (using the deploy.ps1 script in the root of this repo)
.\deploy.ps1 -subscriptionId '[Your Azure subscription ID]' -resourceGroupName '[The Resource Group name to deploy to]' -resourceGroupLocation '[Azure region name]'
```
```bash
azure-group-deploy.sh -l eastus -u
```

Tags: ``cluster, ha, sql, alwayson``