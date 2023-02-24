# Business Continuity in Azure

## Key Concepts

- [The Azure Global Infrastructure](https://infrastructuremap.microsoft.com/explore)
	- [Geographies](https://azure.microsoft.com/en-au/explore/global-infrastructure/geographies/#overview)
	- [Regions](https://learn.microsoft.com/en-us/azure/reliability/availability-zones-overview#regions)


- [Azure Reliability](https://learn.microsoft.com/en-us/azure/reliability/?view=azuresql "Azure Reliability")
	- [Shared responsibility](https://learn.microsoft.com/en-us/azure/reliability/overview#shared-responsibility): Architectures are not automatically 100% available or recoverable just because they are in the cloud
	- [Region Pairs](https://learn.microsoft.com/en-us/azure/reliability/cross-region-replication-azure#azure-cross-region-replication-pairings-for-all-geographies)
	- [Availability Sets](https://learn.microsoft.com/en-us/azure/virtual-machines/availability-set-overview)
	- [Availability Zones](https://learn.microsoft.com/en-us/azure/reliability/availability-zones-overview#availability-zones)
	- [Subscriptions](https://learn.microsoft.com/en-us/azure/reliability/availability-zones-overview#availability-zones) and [checkZonePeers](https://learn.microsoft.com/en-us/rest/api/resources/subscriptions/check-zone-peers)
	- [Infographic](https://azure.microsoft.com/mediahandler/files/resourcefiles/infographic-reliability-with-microsoft-azure/InfographicRC2.pdf)
	

- [Zone Definitions](https://learn.microsoft.com/en-us/azure/reliability/availability-zones-service-support?view=azuresql#azure-services-with-availability-zone-support)
	- **Zonal services**: A resource can be deployed to a specific, **self-selected availability zone** to achieve more stringent latency or performance requirements. Resiliency is self-architected by replicating applications and data to one or more zones within the region. Resources are aligned to a selected zone. For example, virtual machines, managed disks, or standard IP addresses can be aligned to a same zone, which allows for increased resiliency by having multiple instances of resources deployed to different zones.
	- **Zone-redundant services**: Resources are replicated or distributed across zones **automatically**. For example, zone-redundant services replicate the data across multiple zones so that a failure in one zone does not affect the high availability of the data. 
	- **Always-available services:** Always available across all Azure geographies and are resilient to zone-wide outages and region-wide outages. For a complete list of always-available services, also called non-regional services, in Azure, see Products available by region

## Availability

Azure Service Level Agreement

[Reliability by service](https://learn.microsoft.com/en-us/azure/reliability/reliability-guidance-overview?view=azuresql)
- [Availability options for Azure VMs](https://learn.microsoft.com/en-us/azure/virtual-machines/availability)
- [More services...](https://learn.microsoft.com/en-us/azure/reliability/reliability-guidance-overview?view=azuresql)

## Disaster Recovery

### Azure Backup
[![](https://learn.microsoft.com/en-us/azure/backup/media/backup-overview/azure-backup-overview.png)](https://learn.microsoft.com/en-us/azure/backup/media/backup-overview/azure-backup-overview.png)

#### Vaults
- [Recovery Services](https://learn.microsoft.com/en-us/azure/backup/backup-azure-recovery-services-vault-overview)
	- Configuration information and backup data
	- [Redundancy](https://learn.microsoft.com/en-us/azure/backup/backup-create-recovery-services-vault#set-storage-redundancy)
		- LRS, GRS, ZRS
	- [Key features](https://learn.microsoft.com/en-us/azure/backup/backup-azure-recovery-services-vault-overview#key-features) including:
		- [Cross Region Restore](https://learn.microsoft.com/en-us/azure/backup/backup-azure-arm-restore-vms#cross-region-restore): Restore Azure VMs in a secondary (paired) region. By enabling this feature at the vault level, you can restore the replicated data in the secondary region any time. This enables you to restore the secondary region data for audit-compliance, and during outage scenarios, without waiting for Azure to declare a disaster (unlike the GRS settings of the vault).
			- Enabling incurs charges
			- RPO:36 hours
- [Backup](https://learn.microsoft.com/en-us/azure/backup/backup-vault-overview)
	- Houses backup data for certain newer workloads that Azure Backup supports e.g., PostgreSQL

#### Security
- [Soft delete (enhanced in preview)](https://learn.microsoft.com/en-us/azure/backup/backup-azure-enhanced-soft-delete-about)
	- Deleted data is retained for a specified duration (14-180 days)
	- [Enhanced soft delete features](https://learn.microsoft.com/en-us/azure/backup/backup-azure-enhanced-soft-delete-about#whats-enhanced-soft-delete)
- [RBAC](https://learn.microsoft.com/en-us/azure/backup/backup-rbac-rs-vault)
	- Backup Contributor - Permissions to create and manage backup except deleting Recovery Services vault and giving access to others. "Admin of backup operations"
	- Backup Operator - Contributor permissions except for removing backup and managing backup policies. Can't perform destructive operations such as stop backup with delete data or remove registration of on-premises resources.
	- Backup Reader - View all backup management operations. "Monitoring role"
- [Multi-user authorization](https://learn.microsoft.com/en-us/azure/backup/multi-user-authorization-concept?tabs=backup-vault#how-does-mua-for-backup-work)
	- Additional protection on Recovery Services vaults and Backup vaults. Azure Backup uses another Azure resource called the Resource Guard to ensure critical operations are performed only with applicable authorization. Resource Guard must be owned by a different user.
- [Immutable vault (preview)](https://learn.microsoft.com/en-us/azure/backup/backup-azure-immutable-vault-concept?tabs=recovery-services-vault)
	- Blocks specific operations on the vault and its protected items.
	- Disable/Enable/Enable and Lock (cannot be disabled)
	- Restricted operations e.g., Stop protection with delete data, Modify backup policy to reduce retention, Change backup policy to reduce retention
- Private endpoints
	- Bring backup service into VNET
	- [V1](https://learn.microsoft.com/en-us/azure/backup/private-endpoints-overview) and 
	- [V2](https://learn.microsoft.com/en-us/azure/backup/backup-azure-private-endpoints-concept) experience
		- Create private endpoints without managed identities.
		- No private endpoints are created for the blob and queue services.
		- Use of fewer private IPs.
		[Key enhancements](https://learn.microsoft.com/en-us/azure/backup/backup-azure-private-endpoints-concept#key-enhancements)

#### [Supported Workloads](https://learn.microsoft.com/en-us/azure/backup/backup-overview#what-can-i-back-up)
1. On-premises
2. Azure VMs
3. Azure Managed Disks
4. Azure Files shares
5. SQL Server in Azure VMs
6. SAP HANA databases in Azure VMs
7. Azure Database for PostgreSQL servers
8. Azure Blobs

#### Links
- [Azure Pricing Calculator](https://azure.microsoft.com/en-au/pricing/calculator/Backup%20demo)
- [Azure Backup Detailed Estimates](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdownload.microsoft.com%2Fdownload%2F0%2Fb%2F7%2F0b7c4140-24b4-4eff-9b2b-64ecee97d667%2FAzureBackupDetailedEstimatesV9.1.xlsx&data=05%7C01%7Cjohanv%40microsoft.com%7Ce86eb91501514ab12a5408db157a54a7%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C638127387941003210%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C1000%7C%7C%7C&sdata=UVsZ9MHbdAxW4GpgkG%2FfPGhYeDuhFQGe5JIDP%2Fv7Bxs%3D&reserved=0 "Azure Backup Detailed Estimates")
[(how to use the calculator)](https://learn.microsoft.com/en-us/azure/backup/azure-backup-pricing#estimate-costs-for-backing-up-azure-vms-or-on-premises-servers)

### Azure Site Recovery































































