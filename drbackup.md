## Disaster Recovery
`Recovery Point Objective (RPO):` How much data can be lost?
`Recovery Time Objective (RTO):` How long can a service be down? The **expected** recovery time.
`Maximum Tolerable Downtime (MTD):` Total downtime before significant impact to the business. The **required** recovery time.

### [Azure Backup](https://learn.microsoft.com/en-us/azure/backup/)
![](https://learn.microsoft.com/en-us/azure/backup/media/guidance-best-practices/azure-backup-architecture.png)

#### Vaults (2)
- [Recovery Services Vault](https://learn.microsoft.com/en-us/azure/backup/backup-azure-recovery-services-vault-overview)
	- Configuration information and backup data
	- [Redundancy](https://learn.microsoft.com/en-us/azure/backup/backup-create-recovery-services-vault#set-storage-redundancy)
		- **LRS** (low cost option recommended when Azure vault is not the primary/only store), **GRS** (6 copies across 2 paired regions), **ZRS**
		- Must be configured before protecting any workloads. Once a workload is protected in Recovery Services vault, the setting is locked and can't be changed.
		*Storage replication settings for vaults are not relevant for Azure file share backup, because the current solution is snapshot based and no data is transferred to the vault. Snapshots are stored in the same storage account as the backed-up file share.*
	- [Key features](https://learn.microsoft.com/en-us/azure/backup/backup-azure-recovery-services-vault-overview#key-features), including:
		- [Cross Region Restore](https://learn.microsoft.com/en-us/azure/backup/backup-azure-arm-restore-vms#cross-region-restore): Restore Azure VMs in a secondary (paired) region. By enabling this feature at the vault level, you can restore the replicated data in the secondary region any time. This enables you to restore the secondary region data for audit-compliance, and during outage scenarios, without waiting for Azure to declare a disaster (unlike the GRS settings of the vault).
			- Must be enabled and incurs charges
			- `RPO: 36 hours`
		- **Cross-region backup** (i.e. back up a resource to a recovery services vault in a different region) is not supported for any Azure workload. 
- [Backup Vault](https://learn.microsoft.com/en-us/azure/backup/backup-vault-overview)
	- Backup data for certain newer Azure Backup workloads e.g. Azure Disks, Azure Blobs, PostgreSQL
	- Redundancy
		- **LRS**, **ZRS**, **GRS**

#### Security (6)
- [Encryption](https://learn.microsoft.com/en-us/azure/backup/backup-azure-recovery-services-vault-overview#encryption-settings-in-the-recovery-services-vault) of data in transit and at rest
	- Within Azure, data in transit between Azure storage and the vault is protected by HTTPS. This data remains within the Azure network.
	- Backup data
		- Platform-managed keys (by default)
		- Customer-managed keys (stored in key vault)
	- [Azure VMs OS/data disks encrypted with Azure Disk Encryption (ADE) is supported](https://learn.microsoft.com/en-us/azure/backup/backup-azure-vms-encryption)
- [Soft delete (enhanced in preview)](https://learn.microsoft.com/en-us/azure/backup/backup-azure-enhanced-soft-delete-about)
	- Deleted data is retained for a specified duration (14-180 days)
	- [Enhanced soft delete features](https://learn.microsoft.com/en-us/azure/backup/backup-azure-enhanced-soft-delete-about#whats-enhanced-soft-delete) e.g. Always-on
- [RBAC](https://learn.microsoft.com/en-us/azure/backup/backup-rbac-rs-vault)
	- **Backup Contributor** - Permissions to create and manage backup except deleting Recovery Services vault and giving access to others. "Admin of backup operations"
	- **Backup Operator** - Contributor permissions except for removing backup and managing backup policies. Can't perform destructive operations such as stop backup with delete data or remove registration of on-premises resources.
	- **Backup Reader** - View all backup management operations. "Monitoring role"
- [Multi-user authorization](https://learn.microsoft.com/en-us/azure/backup/multi-user-authorization-concept?tabs=backup-vault#how-does-mua-for-backup-work)
	- Additional protection on Recovery Services vaults and Backup vaults. Azure Backup uses another Azure resource called the Resource Guard to ensure critical operations are performed only with applicable authorization. Resource Guard must be owned by a different user.
[![](https://github.com/Kaspanitz/bcdr/blob/main/images/MUA.png)](https://github.com/Kaspanitz/bcdr/blob/main/images/MUA.png)
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

#### Tiers (2)
- Standard
- [Archive](https://learn.microsoft.com/en-us/azure/backup/archive-tier-support)
	- Backup Long-Term (monthly or yearly) retention points in the archive tier
	- [Support matrix](https://learn.microsoft.com/en-us/azure/backup/archive-tier-support#support-matrix)

#### [Backup Policy](https://learn.microsoft.com/en-us/azure/backup/guidance-best-practices#backup-policy-considerations)
1. [Schedule (when?)](https://learn.microsoft.com/en-us/azure/backup/guidance-best-practices#schedule-considerations)
2. [Retention (how long?)](https://learn.microsoft.com/en-us/azure/backup/guidance-best-practices#retention-considerations)
	- Short-term (daily, weekly)
	- Long-term (monthly, yearly)
	- On-demand (not scheduled via backup policy)

[Configure backup policy to automate vault-archive tier for Azure VMs, SQL Server/SAP HANA in Azure VMs]( https://azure.microsoft.com/en-au/updates/general-availability-smart-tiering-to-vaultarchive-tier-for-azure-backup/)

#### [Supported Workloads (8+)](https://learn.microsoft.com/en-us/azure/backup/backup-overview#what-can-i-back-up)
- **[On-premises (3 options)](https://learn.microsoft.com/en-us/azure/backup/backup-support-matrix#on-premises-backup-support)**
	- Windows machine with MARS agent
		- Can also use for Azure VMs e.g. to get three backups/day to vault instead of one backup/day `RPO of 8 hours vs. 24 hours`
	- [DPM/MABS](https://learn.microsoft.com/en-us/azure/backup/backup-support-matrix-mabs-dpm)
		- [DPM (Server, System Center License, Tape/Azure)](https://learn.microsoft.com/en-us/azure/backup/backup-azure-dpm-introduction)
		- [MABS (Server, No System Center License, Azure)](https://learn.microsoft.com/en-us/azure/backup/backup-mabs-protection-matrix)
- **[Azure VMs](https://learn.microsoft.com/en-us/azure/backup/backup-support-matrix-iaas)**
	- Backup process takes a snapshot, data is transferred to a Recovery Services vault with no impact on production workloads
	- Snapshot levels of [consistency](https://learn.microsoft.com/en-us/azure/backup/backup-azure-vms-introduction#snapshot-consistency):
		- App-consistent
		- File system-consistent
		- Crash-consistent
	- [Quiesce applications using pre-post scripts](https://learn.microsoft.com/en-us/azure/backup/backup-azure-linux-database-consistent-enhanced-pre-post)
	- [Selective disk backup/restore](https://learn.microsoft.com/en-us/azure/backup/selective-disk-backup-restore)
	- [Backup encrypted Azure VM](https://learn.microsoft.com/en-us/azure/backup/backup-azure-vms-encryption)
		- Storage service encryption (SSE) with platform-managed or customer-managed keys
		- Azure Disk Encryption (ADE)
	- Policies: 
		- Standard
			- `RPO: 24 hours` (once a day)
			- [Trusted Azure VM](https://learn.microsoft.com/en-us/azure/virtual-machines/trusted-launch) not supported
			- Will not support newer Azure offerings
		- Enhanced
			- Snapshot based (Instant Restore) reduces backup and restore times
[![](https://learn.microsoft.com/en-us/azure/backup/media/backup-azure-vms/instant-rp-flow.png)](https://learn.microsoft.com/en-us/azure/backup/media/backup-azure-vms/instant-rp-flow.png)
			- One snapshot per day is transferred to vault
			- `RPO: 4 hours` (up to six per day)
			- `Snapshot Retention: Select 1 to 17 days` (hourly frequency)
			- `Snapshot Retention: Select 1 to 30 days` (daily frequency)
			- Does not support Ultra SSD
		- `Daily Backup Point Retention: 7-9999 days`
		- `Weekly Backup Point Retention: 1-5163 days`
		- `Monthly Backup Point Retention: 1-1188 days`
		- `Yearly Backup Point Retention: 1-99 days`
		- Restore
			- Create a VM
			- Restore disk (attach to existing or new VM)
			- Replace VM disk
			- Restore item level (files/folders)
			- Cross region restore
- **[Azure Managed Disks](https://learn.microsoft.com/en-us/azure/backup/disk-backup-support-matrix)**
	- Agentless (useful in highly secure environments where agents cannot be deployed)
	- Incremental snapshots
	- `RPO: 1 hour`
	- `Retention: 1-282 days (7 by default)`
		Choose between first successful backup per day or week
	- Multiple backups per day (every 1, 2, 4, 6, 8, or 12 hours)
	- Ultra and Premium SSD v2 incremental snapshots are in public preview (as at 25 and 23 January 2023 respectively)
	- Shared disk support
	- Restore
		- Only supports Alternate-Location Recovery (ALR) currently (Original-Location Recovery (OLR) is not supported)
- **[SQL Server in Azure VMs](https://learn.microsoft.com/en-us/azure/backup/sql-support-matrix)**
	- `Full backup one/day (at most) scheduled`
	- `Full backup three/day on-demand` (hard limit of 9 to retry failed)
	- `RPO: 15 minutes` (transaction log backups)
	- `Transaction Log Retention: 7-35 days`
	- `Daily Full Backup Retention: 7-9999 days`
	- `Weekly Full Backup Retention: 1-5163 days`
	- `Monthly Full Backup Retention: 1-1188 days`
	- `Yearly Full Backup Retention: 1-99 days`
	- Database level
	- Backup can also be configured at server level with option to restore individual databases.
	- Native SQL backup compression support
	- Always On Availability Group awareness
	- Also consider [SQL Server Managed Backup](https://learn.microsoft.com/en-us/sql/relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure?view=sql-server-ver16) and Manual Backup 
- **[Azure Files shares](https://learn.microsoft.com/en-us/azure/backup/azure-file-share-support-matrix)**
	- Full share restore
	- Item level restore
	- Snapshot-based
- **[Azure Blobs](https://learn.microsoft.com/en-us/azure/backup/blob-backup-support-matrix)**
- **[SAP HANA databases in Azure VMs](https://learn.microsoft.com/en-us/azure/backup/sap-hana-backup-support-matrix)**
- **[Azure Database for PostgreSQL servers](https://learn.microsoft.com/en-us/azure/backup/backup-azure-database-postgresql-support-matrix)**
- **[[Private preview: Azure Kubernetes Service (AKS) Backup]](https://azure.microsoft.com/en-au/updates/private-preview-aks-backup/)**

#### Azure Policy (4)
- Compliance: [Auto-Enable Backup on VM creation](https://learn.microsoft.com/en-us/azure/backup/backup-azure-auto-enable-backup)
- [Supported VM SKUs](https://learn.microsoft.com/en-us/azure/backup/backup-azure-policy-supported-skus)
- [Supported scenarios](https://learn.microsoft.com/en-us/azure/backup/backup-azure-auto-enable-backup#supported-scenarios)
- Built-in Policies
	- [Policy 1 - Configure backup on VMs without a given tag to an existing recovery services vault in the same location](https://learn.microsoft.com/en-us/azure/backup/backup-azure-auto-enable-backup#policy-1---configure-backup-on-vms-without-a-given-tag-to-an-existing-recovery-services-vault-in-the-same-location)
	- [Policy 2 - Configure backup on VMs with a given tag to an existing recovery services vault in the same location](https://learn.microsoft.com/en-us/azure/backup/backup-azure-auto-enable-backup#policy-2---configure-backup-on-vms-with-a-given-tag-to-an-existing-recovery-services-vault-in-the-same-location)
	- [Policy 3 - Configure backup on VMs without a given tag to a new recovery services vault with a default policy](https://learn.microsoft.com/en-us/azure/backup/backup-azure-auto-enable-backup#policy-3---configure-backup-on-vms-without-a-given-tag-to-a-new-recovery-services-vault-with-a-default-policy)
	- [Policy 4 - Configure backup on VMs with a given tag to a new recovery services vault with a default policy](https://learn.microsoft.com/en-us/azure/backup/backup-azure-auto-enable-backup#policy-4---configure-backup-on-vms-with-a-given-tag-to-a-new-recovery-services-vault-with-a-default-policy)

#### Useful Links
- [What is newly GA, in private/public preview?](https://azure.microsoft.com/en-au/updates/?query=backup&Page=2)
- [Azure Backup Pricing](https://azure.microsoft.com/en-us/pricing/details/backup/)
- [Azure Pricing Calculator](https://azure.microsoft.com/en-au/pricing/calculator/Backup%20demo)
- [Azure Backup Detailed Estimates](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdownload.microsoft.com%2Fdownload%2F0%2Fb%2F7%2F0b7c4140-24b4-4eff-9b2b-64ecee97d667%2FAzureBackupDetailedEstimatesV9.1.xlsx&data=05%7C01%7Cjohanv%40microsoft.com%7Ce86eb91501514ab12a5408db157a54a7%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C638127387941003210%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C1000%7C%7C%7C&sdata=UVsZ9MHbdAxW4GpgkG%2FfPGhYeDuhFQGe5JIDP%2Fv7Bxs%3D&reserved=0 "Azure Backup Detailed Estimates")
[(how to use the calculator)](https://learn.microsoft.com/en-us/azure/backup/azure-backup-pricing#estimate-costs-for-backing-up-azure-vms-or-on-premises-servers)