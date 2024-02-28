## [Supported Workloads (8+)](https://learn.microsoft.com/en-us/azure/backup/backup-overview#what-can-i-back-up)

------------

- **[On-premises (3 options)](https://learn.microsoft.com/en-us/azure/backup/backup-support-matrix#on-premises-backup-support)**
	- Windows (Workstation or Server) with MARS agent
		- In the past often also used for Azure VMs e.g. to get three backups/day to vault instead of one backup/day `RPO of 8 hours vs. 24 hours`
		([Multiple backups per day for Azure VMs is now GA](https://azure.microsoft.com/en-us/updates/multiple-backups-per-day/))
		- Linux is not supported
		- The MARS agent doesn't require a separate backup server
		- The MARS agent isn't application-aware. You can restore files and folders from backups, or do a volume-level restore
	- [DPM or MABS](https://learn.microsoft.com/en-us/azure/backup/backup-support-matrix-mabs-dpm)
		- [DPM (Server, System Center License, Tape/Azure)](https://learn.microsoft.com/en-us/azure/backup/backup-azure-dpm-introduction)
		- [MABS (Server, No System Center License, Azure)](https://learn.microsoft.com/en-us/azure/backup/backup-mabs-protection-matrix)
		- Linux support

Note: [Data backed up from Azure Backup Agent, DPM, and Azure Backup Server is compressed and encrypted before being transferred. With compression and encryption applied, the data in the vault is 30-40% smaller](https://learn.microsoft.com/en-us/azure/backup/backup-azure-backup-faq#why-is-the-size-of-the-data-transferred-to-the-recovery-services-vault-smaller-than-the-data-selected-for-backup-)

------------

- **[Azure VMs](https://learn.microsoft.com/en-us/azure/backup/backup-support-matrix-iaas)**
	- Azure VM backup happens in two phases:
		- In phase 1, the snapshot taken is stored along with the disk. This is referred to as `snapshot tier`. Snapshot tier restores are faster (than restore from vault) because they eliminate the wait time for snapshots to copy to the vault before triggering the restore. So restore from the snapshot tier is also referred as Instant Restore.
		- In phase 2, the snapshot is transferred and stored in the vault managed by the Azure Backup service. This is referred to as `vault tier`.
	- As the backup process takes a snapshot, data is transferred to a Recovery Services vault with no impact on production workloads
	- Snapshot levels of [consistency](https://learn.microsoft.com/en-us/azure/backup/backup-azure-vms-introduction#snapshot-consistency):
		- App-consistent
		- File system-consistent
		- Crash-consistent
	- [Database-consistent (Quiesce applications using pre-post scripts)](https://learn.microsoft.com/en-us/azure/backup/backup-azure-linux-database-consistent-enhanced-pre-post)
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
			- [Cross subscription restore (GA: Aug 2023)](https://azure.microsoft.com/en-us/updates/generally-available-cross-subscription-restore-for-azure-virtual-machines/)
				- To another subscription within the same tenant of the subscription where source VM is present
				- Requires relevant permissions to restore in that secondary subscription
				- Feature is only allowed if you have the Cross Subscription Restore property enabled for the Recovery Services vault

_Note: Azure VM backup options include 1. Azure VM Backup, Azure site Recovery, Azure Managed Disk Snapshot Backup and Azure Managed Disk Image Backup_

Things to consider when creating images versus snapshots:
- Consider images. With Azure managed disks, you can take an image of a generalized virtual machine that's been deallocated. The image includes all of the disks attached to the virtual machine. You can use the image to create a virtual machine that includes all of the disks.

- Consider snapshots. A snapshot is a copy of a disk at the point in time the snapshot is taken. The snapshot applies to one disk only, and doesn't have awareness of any disk other than the one it contains. Snapshot backups are problematic for configurations that require the coordination of multiple disks, such as striping. In this case, the snapshots need to coordinate with each other, but this functionality isn't currently supported.

- Consider operating disk backups. If you have a virtual machine with only one disk (the operating system disk), you can take a snapshot or an image of the disk. You can create a virtual machine from either a snapshot or an image.

------------

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

------------

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
	- [Backup Always On Availability Groups](https://learn.microsoft.com/en-us/azure/backup/backup-sql-server-on-availability-groups)
		- [Basic Always On AG](https://learn.microsoft.com/en-us/sql/database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups?view=sql-server-ver16) is not supported
		- Considerations apply for the region, subscription, primary and secondary replicas
		- How various types of backup (full, differential, etc) will be handled, if there is a failover to a secondary node, needs to be considered
	- Also consider [SQL Server Managed Backup](https://learn.microsoft.com/en-us/sql/relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure?view=sql-server-ver16) and Manual Backup 

------------

- **[Azure Files](https://learn.microsoft.com/en-us/azure/backup/azure-file-share-support-matrix)**
	- Snapshot-based: No need to configure the storage replication type. Azure Files backup is snapshot-based, and no data is transferred to the vault. Snapshots are stored in the same Azure storage account as your backed-up file share.
	- Hourly or Daily Frequency
	- `RPO: 4 hours` (hourly frequency)
	- `RPO: 24 hours` (daily frequency)
	- Hourly frequency, every 4 hours schedule (refer to portal for 6, 8 and 12 hour schedules)
		- `Daily Backup Point Retention: 1-49 days`
		- `Weekly Backup Point Retention: 1-200 days`
		- `Monthly Backup Point Retention: 1-120 days`
		- `Yearly Backup Point Retention: 1-10 days`
	- Daily frequency
		- `Daily Backup Point Retention: 1-200 days`
		- `Weekly Backup Point Retention: 1-200 days`
		- `Monthly Backup Point Retention: 1-120 days`
		- `Yearly Backup Point Retention: 1-10 days`
	- Restore
		- Full share restore
		- Item level restore

------------

- **[Azure Blobs](https://learn.microsoft.com/en-us/azure/backup/blob-backup-support-matrix)**
	- Backup Center integration for single pane of glass
	- Managed, local data protection
	- Protect block blobs from data loss scenarios like corruptions, blob deletions, and accidental storage account deletion
	- Data stored locally within source storage account itself
	- Recover to a selected point in time
	- Continuous backup, no need to schedule
	- `Retention: 1-360 days or 1-51 weeks or 1-11 months`
	- [Point-in-time restore](https://learn.microsoft.com/en-us/azure/storage/blobs/point-in-time-restore-overview)

------------

- **[SAP HANA databases in Azure VMs](https://learn.microsoft.com/en-us/azure/backup/sap-hana-backup-support-matrix)**

------------

- **[Azure Database for PostgreSQL servers](https://learn.microsoft.com/en-us/azure/backup/backup-azure-database-postgresql-support-matrix)**

------------

- **[Azure Kubernetes Service Backup](https://learn.microsoft.com/en-us/azure/backup/azure-kubernetes-service-backup-overview)**

------------
