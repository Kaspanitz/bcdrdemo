# Business Continuity in Azure

## Key Concepts

- [The Azure Global Infrastructure](https://infrastructuremap.microsoft.com/explore)
	- [Geographies](https://azure.microsoft.com/en-au/explore/global-infrastructure/geographies/#overview)
	- [Regions](https://learn.microsoft.com/en-us/azure/reliability/availability-zones-overview#regions)


- [Azure Reliability](https://learn.microsoft.com/en-us/azure/reliability/?view=azuresql "Azure Reliability")
	- Architectures are not automatically 100% available or recoverable when they are in the cloud
	- [Shared responsibility](https://learn.microsoft.com/en-us/azure/reliability/overview#shared-responsibility)
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

[Reliability by service](https://learn.microsoft.com/en-us/azure/reliability/reliability-guidance-overview?view=azuresql)
- [Availability options for Azure VMs](https://learn.microsoft.com/en-us/azure/virtual-machines/availability)
- [More services...](https://learn.microsoft.com/en-us/azure/reliability/reliability-guidance-overview?view=azuresql)

## Disaster Recovery

### Azure Backup
[![](https://learn.microsoft.com/en-us/azure/backup/media/backup-overview/azure-backup-overview.png)](https://learn.microsoft.com/en-us/azure/backup/media/backup-overview/azure-backup-overview.png)

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
