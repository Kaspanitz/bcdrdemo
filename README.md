# Business Continuity in Azure
`FTA Preview`

*Applications do not necessarily have the availability (or recoverability) that we need, just because they are in the cloud. We do have more options to build resilience into our applications though.*

[![](https://learn.microsoft.com/en-us/azure/reliability/media/shared-responsibility-model.png)](https://learn.microsoft.com/en-us/azure/reliability/media/shared-responsibility-model.png)

*Many of the offerings Azure provides require customers to architect for availability, and set up disaster recovery in multiple regions and are not the responsibility of Microsoft.*

*The many availability and disaster recovery options provide customers with the flexibility to choose an implementation that aligns to their application criticality requirements and budget considerations.*

[Shared responsibility](https://learn.microsoft.com/en-us/azure/reliability/overview#shared-responsibility)

## Agenda
- [Availability](availability.md)
	- Azure Infrastructure
	- Azure Reliability
- Disaster Recovery
	- [Backup Center](drbackupcenter.md)
	- [Azure Backup](drbackup.md)
	- [Azure Site Recovery](drreplicate.md)
	- Built-in Features (e.g., Replication, Backup/Restore, Export/Import, Soft Delete) e.g.,
		- [Microsoft Entra ID (Soft Delete)](https://learn.microsoft.com/en-us/azure/active-directory/architecture/recover-from-deletions)
		- [Azure App Service (Backup and Restore)](https://learn.microsoft.com/en-us/azure/app-service/manage-backup?tabs=portal)
		- [Azure DNS (Import and Export)](https://learn.microsoft.com/en-us/azure/dns/dns-import-export)
		- [Azure Service Bus (Geo-DR)](https://learn.microsoft.com/en-us/azure/service-bus-messaging/service-bus-geo-dr)
		- [Azure SQL Database (Active Geo-replication)](https://learn.microsoft.com/en-us/azure/azure-sql/database/active-geo-replication-overview?view=azuresql)
		- [Azure SQL Database (Automated Backups)](https://learn.microsoft.com/en-us/azure/azure-sql/database/automated-backups-overview?view=azuresql)
		- [Azure SQL Database (Auto-failover Groups)](https://learn.microsoft.com/en-us/azure/azure-sql/database/auto-failover-group-sql-db?view=azuresql&tabs=azure-powershell)
		- [Key Vault (Soft Delete)](https://learn.microsoft.com/en-us/azure/key-vault/general/soft-delete-change)
		- [Blob (Versioning)](https://learn.microsoft.com/en-us/azure/storage/blobs/versioning-overview)
		- [Blob (Soft Delete)](https://learn.microsoft.com/en-us/azure/storage/blobs/soft-delete-blob-overview)
		- [Containers (Soft Delete)](https://learn.microsoft.com/en-us/azure/storage/blobs/soft-delete-container-overview)
- [Azure Business Continuity Guide (Guided Journey with Templates)](https://aka.ms/abcg)