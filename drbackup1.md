## Disaster Recovery
`Recovery Point Objective (RPO):` How much data can be lost?

`Recovery Time Objective (RTO):` How long can a service be down? The **expected** recovery time.

`Maximum Tolerable Downtime (MTD):` Total downtime before significant impact to the business. The **required** recovery time.

------------

### [Azure Backup](https://learn.microsoft.com/en-us/azure/backup/)
![](https://learn.microsoft.com/en-us/azure/backup/media/guidance-best-practices/azure-backup-architecture.png)


1. [Vaults (2)](vaults.md)
2. [Security (6)](security.md)
3. [Tiers (2)](tiers.md)
4. [Backup Policy](backuppolicy.md)
5. [Workloads](workloads.md)
6. [Azure Policy (4)](azurepolicy.md)

#### Useful Links
- [What is newly GA, in private/public preview?](https://azure.microsoft.com/en-au/updates/?query=backup&Page=2)
- [Azure Backup Pricing](https://azure.microsoft.com/en-us/pricing/details/backup/)
- [Azure Pricing Calculator](https://azure.microsoft.com/en-au/pricing/calculator/Backup%20demo)
- [Azure Backup Detailed Estimates](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdownload.microsoft.com%2Fdownload%2F0%2Fb%2F7%2F0b7c4140-24b4-4eff-9b2b-64ecee97d667%2FAzureBackupDetailedEstimatesV9.1.xlsx&data=05%7C01%7Cjohanv%40microsoft.com%7Ce86eb91501514ab12a5408db157a54a7%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C638127387941003210%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C1000%7C%7C%7C&sdata=UVsZ9MHbdAxW4GpgkG%2FfPGhYeDuhFQGe5JIDP%2Fv7Bxs%3D&reserved=0 "Azure Backup Detailed Estimates")
[(how to use the calculator)](https://learn.microsoft.com/en-us/azure/backup/azure-backup-pricing#estimate-costs-for-backing-up-azure-vms-or-on-premises-servers)