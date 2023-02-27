## Disaster Recovery
`Recovery Point Objective (RPO):` How much data can be lost?

`Recovery Time Objective (RTO):` How long can a service be down? The **expected** recovery time.

`Maximum Tolerable Downtime (MTD):` Total downtime before significant impact to the business. The **required** recovery time.

### [Backup Center](https://learn.microsoft.com/en-us/azure/backup/backup-center-overview)
- Unified management experience
- [Govern](https://learn.microsoft.com/en-us/azure/backup/backup-center-govern-environment), [monitor](https://learn.microsoft.com/en-us/azure/backup/backup-center-monitor-operate), [report](https://learn.microsoft.com/en-us/azure/backup/backup-center-obtain-insights), and operate **Azure Backups** at scale 
	- across various dimensions: job status, policies, backup instances
	- across various scopes: vaults, resource groups, subscriptions, regions, and tenants (using Azure Lighthouse) 
- Monitor and manage **Azure Site Recovery** at scale
- [Support matrix](https://learn.microsoft.com/en-us/azure/backup/backup-center-support-matrix)