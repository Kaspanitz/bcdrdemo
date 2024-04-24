## Vaults (2)
- [Recovery Services Vault](https://learn.microsoft.com/en-us/azure/backup/backup-azure-recovery-services-vault-overview)
	- Configuration information and backup data
	- [Redundancy](https://learn.microsoft.com/en-us/azure/backup/backup-create-recovery-services-vault#set-storage-redundancy)
		- **LRS** (low cost option recommended when Azure vault is not the primary/only store), **GRS** (6 copies across 2 paired regions), **ZRS** (high data durability along with data residency)
		- Must be configured before protecting any workloads. Once a workload is protected in Recovery Services vault, the setting is locked and can't be changed.
		*Storage replication settings for vaults are not relevant for Azure file share backup, because the current solution is snapshot based and no data is transferred to the vault. Snapshots are stored in the same storage account as the backed-up file share.*
	- [Key features](https://learn.microsoft.com/en-us/azure/backup/backup-azure-recovery-services-vault-overview#key-features), including:
		- [Cross Region Restore](https://learn.microsoft.com/en-us/azure/backup/backup-azure-arm-restore-vms#cross-region-restore): Restore Azure VMs in a secondary (paired) region. By enabling this feature at the vault level, you can restore the replicated data in the secondary region any time. This enables you to restore the secondary region data for audit-compliance, and during outage scenarios, without waiting for Azure to declare a disaster (unlike the GRS settings of the vault).
			- Must be enabled and incurs charges
			- `RPO: 36 hours`
		- **Cross-region backup** (i.e. back up a resource to a recovery services vault in a different region) is not supported for any Azure workload.
  - Cross-subscription restore GA'd Aug 2023 - [info and limitations](https://learn.microsoft.com/en-us/azure/backup/backup-azure-arm-restore-vms#restore-options)
  - No cross-subscription backup
- [Backup Vault](https://learn.microsoft.com/en-us/azure/backup/backup-vault-overview)
	- Backup data for certain newer Azure Backup workloads e.g. Azure Disks, Azure Blobs, PostgreSQL
	- Redundancy
		- **LRS**, **ZRS**, **GRS**
