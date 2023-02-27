## Disaster Recovery
`Recovery Point Objective (RPO):` How much data can be lost?

`Recovery Time Objective (RTO):` How long can a service be down? The **expected** recovery time.

`Maximum Tolerable Downtime (MTD):` Total downtime before significant impact to the business. The **required** recovery time.

### Azure Site Recovery

#### Scenarios (8)
- [Azure to Azure](https://learn.microsoft.com/en-us/azure/site-recovery/azure-to-azure-how-to-enable-replication)
- [VMware to Azure](https://learn.microsoft.com/en-us/azure/site-recovery/vmware-physical-large-deployment)
- [Physical to Azure](https://learn.microsoft.com/en-us/azure/site-recovery/vmware-physical-large-deployment)
- [Azure Stack to Azure](https://learn.microsoft.com/en-us/azure/site-recovery/azure-stack-site-recovery)
- [Hyper-V to Azure](https://learn.microsoft.com/en-us/azure/site-recovery/site-recovery-manage-network-interfaces-on-premises-to-azure)
- [Proximity Placement Groups](https://learn.microsoft.com/en-us/azure/site-recovery/how-to-enable-replication-proximity-placement-groups)
	- Best effort to fail over and fail back VMs into a proximity placement group
- [On-premises Apps](https://learn.microsoft.com/en-us/azure/site-recovery/site-recovery-workload)
	- App-agnostic replication for any workload running on a supported machine
- Site-to-Site Disaster Recovery ([VMware](https://learn.microsoft.com/en-us/azure/site-recovery/vmware-physical-secondary-disaster-recovery) and [Hyper-V](https://learn.microsoft.com/en-us/azure/site-recovery/site-to-site-deprecation))
	- Deprecated

#### Features
- Consistency
	- Crash
		- Snapshot of all data on disk
		- Every 5 minutes
		- Up to 15 days 
		- For app servers
	- Application
		- Snapshot of data on disk + data in memory
		- Configurable: Every 1-12 hours
		- Up to 15 days
		- For databases
	- Multi VM
		- Consistent recovery points across multiple VMs
		- Replication group of all enabled VMs created
		- Shared crash & app consistent recovery points for all VMs in a group
- No-impact DR drill
- Recovery points up to 15 days
- Point and click Graphical UI
- Centralized monitoring and alerting
- Automation support
- Auto upgrade of ASR agents
- RPO of minutes
- SLA-backed RTO
- Continuous replication
- Compression
- Recovery Plans
	- Model app around its dependencies
	- Test failovers
- Deployment Planner
	- Profile actual churn on VMs
	- Estimate network bandwidth
	- No impact on production workloads
- Private Link
	- Simpler Networking setup
	- Secure connectivity
	- ASR services in your private virtual network

#### Useful Links
- [What is newly GA, in private/public preview?](https://azure.microsoft.com/en-au/updates/?query=site%20recovery)
- [Azure Site Recovery Pricing](https://azure.microsoft.com/en-gb/pricing/details/site-recovery/)
- [Azure Pricing Calculator](https://azure.microsoft.com/en-gb/pricing/calculator/Azure%20Site%20Recovery)