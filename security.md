## Security (6)
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