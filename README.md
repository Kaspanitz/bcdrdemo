# Concepts

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
	

# [Zone Concepts](https://learn.microsoft.com/en-us/azure/reliability/availability-zones-service-support?view=azuresql#azure-services-with-availability-zone-support)

- **Zonal services**: A resource can be deployed to a specific, **self-selected availability zone** to achieve more stringent latency or performance requirements. Resiliency is self-architected by replicating applications and data to one or more zones within the region. Resources are aligned to a selected zone. For example, virtual machines, managed disks, or standard IP addresses can be aligned to a same zone, which allows for increased resiliency by having multiple instances of resources deployed to different zones.
- **Zone-redundant services**: Resources are replicated or distributed across zones **automatically**. For example, zone-redundant services replicate the data across multiple zones so that a failure in one zone doesn't affect the high availability of the data. 
- **Always-available services:** Always available across all Azure geographies and are resilient to zone-wide outages and region-wide outages. For a complete list of always-available services, also called non-regional services, in Azure, see Products available by region

# [Reliability by service](https://learn.microsoft.com/en-us/azure/reliability/reliability-guidance-overview?view=azuresql)

- [Availability options for Azure VMs](https://learn.microsoft.com/en-us/azure/virtual-machines/availability)
- [...](https://learn.microsoft.com/en-us/azure/reliability/reliability-guidance-overview?view=azuresql)