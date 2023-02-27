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
	- Latency-sensitive workloads
	- Best effort to fail over and fail back VMs into a PPG
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
	- ARM template support for declarative and repeatable creation of Azure resources
	- PowerShell support to setup vaults and policies using scripts
	- Azure Policy integration for governance
- Auto upgrade of ASR agents
- `RPO: 'minutes'`
- SLA-backed RTO
- Continuous replication
- Hot-Add Disk
	- Exclude undesired
	- Include new disks on a machine that is already replicating
- Compression
- Capacity Reservation
	- Purchase capacity in DR region  and failover to that capacity
	- Create new or use existing Capacity Reservation Group (CRG)
	- Assign a CRG to VMs. On failover, new VM will be created in the assigned CRG
- Recovery Plans
	- Model app around its dependencies
	- Test failovers and calculate RTO
	- Parallelism and sequencing of VM start up
	- Azure Automation runbooks to automate tasks outside and inside recovered VMs
	- Add manual actions to validate recovered application aspects that cannot be automated
- Deployment Planner
	- Profile churn on VMs with no production impact
	- Estimate network bandwidth for initial and delta replication
	- Identify Azure storage type (standard or premium) required for VMs
	- Estimate number of standard and premium storage accounts
	- Estimate number of Configuration and Process Servers 
	- VM eligibility assessment based on number of disks, size and IOPS
- Private Link
	- Access Site Recovery Vault and Storage Account over Private Endpoints in your network
	- Replication traffic stays on Microsoft backbone
	- Secure connectivity from on-premises to Azure
	- Meet compliance requirements
- Encryption
	- In-transit and at-rest
	- Azure Disk Encryption (ADE) enabled VMs supported for replication
	- Customer Managed Keys (CMK) enabled managed disk VMs supported
- Zone to Zone DR
	- Metro strategy
	- Compliance requirements of data residency

#### Best Practices
- Generate csv file with target details like SKU, resource group, target network
- Execute PowerShell script to enable replication and track progress
- Leverage scripts to perform test and planned failovers
- Create hub-spoke topology of all workloads with shared resources like AD in hub and web/app tier in spoke 
- Pre-create network components in target region to minimize recovery time 
- Plan network/subnet configuration to ensure IP retention post failover and avoid conflicts during steady-state
- Integrate with Azure Log Analytics for monitoring
- Configure automatic updates so that new ASR update gets installed as and when it is released
- Run deployment planner to profile workloads and understand storage & network capacity requirements  
- Profile disk churn on source machine and validate supported limits
- Ensure subscription has enough resources in target region
- Perform Test failover of protected VMs to validate successful replication
- Leverage Recovery Plans to orchestrate the entire application failover of web tier, middle tier and data tier to achieve low RTO
- Run DR drills every 3 months to validate the RPO & RTO targets and meet the business and compliance needs

#### Useful Links
- [What is newly GA, in private/public preview?](https://azure.microsoft.com/en-au/updates/?query=site%20recovery)
- [Azure Site Recovery Pricing](https://azure.microsoft.com/en-gb/pricing/details/site-recovery/)
- [Azure Pricing Calculator](https://azure.microsoft.com/en-gb/pricing/calculator/Azure%20Site%20Recovery)