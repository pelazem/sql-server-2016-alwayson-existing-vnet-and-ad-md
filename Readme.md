# Create a SQL Server 2016 AlwaysOn Availability Group (AOAG) in an _existing_ Azure VNet and Active Directory domain.

This template will create a SQL Server 2016 AlwaysOn Availability Group and configure the VMs using Powershell DSC extensions. 

This template creates the following resources:
+	One internal load balancer (ILB)
+	Three VMs in a Windows Server Failover Cluster (WSFC)
	+	Two VMs run SQL Server 2016 (primary and synchronous secondary replica)
	+	The third VM is the File Share Witness for the cluster
	+	VMs use Managed Disks
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

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fpelazem%2Fsql-server-2016-alwayson-existing-vnet-and-ad%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

## Deploying

You can deploy these samples directly through the Azure Portal (see "Deploy to Azure" button above) or by using the deploy.ps1 script supplied in the root of the repo.
