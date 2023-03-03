## Disaster Recovery
`Recovery Point Objective (RPO):` How much data can be lost?

`Recovery Time Objective (RTO):` How long can a service be down? The **expected** recovery time.

`Maximum Tolerable Downtime (MTD):` Total downtime before significant impact to the business. The **required** recovery time.

### Azure Site Recovery

#### Scenarios (9)
- [Azure to Azure](https://learn.microsoft.com/en-us/azure/site-recovery/azure-to-azure-how-to-enable-replication)
	- [Support matrix](https://learn.microsoft.com/en-us/azure/site-recovery/azure-to-azure-support-matrix)

[![](https://learn.microsoft.com/en-us/azure/site-recovery/media/concepts-azure-to-azure-architecture/enable-replication-step-2-v2.png)](https://learn.microsoft.com/en-us/azure/site-recovery/media/concepts-azure-to-azure-architecture/enable-replication-step-2-v2.png)
	
- [VMware to Azure](https://learn.microsoft.com/en-us/azure/site-recovery/vmware-physical-large-deployment)
	- [Support matrix](https://learn.microsoft.com/en-us/azure/site-recovery/vmware-physical-azure-support-matrix)
	
[![](https://learn.microsoft.com/en-us/azure/site-recovery/media/vmware-azure-architecture-modernized/architecture-modernized.png#lightbox)](https://learn.microsoft.com/en-us/azure/site-recovery/media/vmware-azure-architecture-modernized/architecture-modernized.png#lightbox)

- [Physical to Azure](https://learn.microsoft.com/en-us/azure/site-recovery/vmware-physical-large-deployment)
    - [Support matrix](https://learn.microsoft.com/en-us/azure/site-recovery/vmware-physical-azure-support-matrix)

[![](https://learn.microsoft.com/en-us/azure/site-recovery/media/physical-azure-architecture/v2a-architecture-henry.png)](https://learn.microsoft.com/en-us/azure/site-recovery/media/physical-azure-architecture/v2a-architecture-henry.png)

- [Hyper-V to Azure](https://learn.microsoft.com/en-us/azure/site-recovery/site-recovery-manage-network-interfaces-on-premises-to-azure)
	- [Support matrix](https://learn.microsoft.com/en-us/azure/site-recovery/hyper-v-azure-support-matrix)

[![](https://learn.microsoft.com/en-us/azure/site-recovery/media/hyper-v-azure-architecture/arch-onprem-azure-hypervsite.png)](https://learn.microsoft.com/en-us/azure/site-recovery/media/hyper-v-azure-architecture/arch-onprem-azure-hypervsite.png)
[![](https://learn.microsoft.com/en-us/azure/site-recovery/media/hyper-v-azure-architecture/arch-onprem-onprem-azure-vmm.png)](https://learn.microsoft.com/en-us/azure/site-recovery/media/hyper-v-azure-architecture/arch-onprem-onprem-azure-vmm.png)

- [Azure Stack to Azure](https://learn.microsoft.com/en-us/azure/site-recovery/azure-stack-site-recovery)
- [Proximity Placement Groups](https://learn.microsoft.com/en-us/azure/site-recovery/how-to-enable-replication-proximity-placement-groups)
	- Latency-sensitive workloads
	- Best effort to fail over and fail back VMs into a PPG
- [On-premises Apps](https://learn.microsoft.com/en-us/azure/site-recovery/site-recovery-workload)
	- App-agnostic replication for any workload running on a supported machine
	- E.g. AD, DNS, SQL, SharePoint, Dynamics AX, RDS, Exchange, SAP, File Server, IIS, Citrix XenApp and XenDesktop, etc
- [Zone to Zone DR](https://learn.microsoft.com/en-us/azure/site-recovery/azure-to-azure-how-to-enable-zone-to-zone-disaster-recovery)
	- Metro DR strategy
	- Compliance requirements for data residency
	- If deploying VMs in a highly available availability zone configuration is not desired - cost tradeoff
	- Where a paired region is not in the same jurisdiction e.g. Southeast Asia
	- Reduces networking complexity (use the same VNET, etc)
- Site-to-Site Disaster Recovery ([VMware](https://learn.microsoft.com/en-us/azure/site-recovery/vmware-physical-secondary-disaster-recovery) and [Hyper-V](https://learn.microsoft.com/en-us/azure/site-recovery/site-to-site-deprecation))
	- Deprecated
	
#### Features
- [Automation](https://learn.microsoft.com/en-us/azure/site-recovery/azure-to-azure-powershell)
	- Bicep/ARM/PowerShell for creation of Azure resources
	- PowerShell to test failover
	- [Azure Automation Runbooks](https://learn.microsoft.com/en-us/azure/site-recovery/site-recovery-runbook-automation) to automate tasks outside and inside recovered VMs as part of a recovery plan
	- [Azure Policy](https://learn.microsoft.com/en-us/azure/site-recovery/azure-to-azure-how-to-enable-policy) integration for governance and at scale operations
- [Deployment Planner](https://learn.microsoft.com/en-us/azure/site-recovery/site-recovery-deployment-planner)
	- Command-line tool for both Hyper-V to Azure and VMware to Azure disaster recovery scenarios
	- Profile churn on VMs with no production impact
	- Estimate network bandwidth for initial and delta replication
	- Identify Azure storage type (standard or premium) required for VMs
	- Estimate number of standard and premium storage accounts
	- Estimate number of Configuration and Process Servers 
	- VM eligibility assessment based on number of disks, size and IOPS
	- Excel report
	[![](https://learn.microsoft.com/en-us/azure/site-recovery/media/site-recovery-vmware-deployment-planner-analyze-report/recommendations-v2a.png)](https://learn.microsoft.com/en-us/azure/site-recovery/media/site-recovery-vmware-deployment-planner-analyze-report/recommendations-v2a.png)
- [Private Link](https://learn.microsoft.com/en-us/azure/site-recovery/azure-to-azure-how-to-enable-replication-private-endpoints)
	- Access Site Recovery Vault and Storage Account over Private Endpoints in your network
	- Replication traffic stays on Microsoft backbone
	- Meet compliance requirements
- [Encryption](https://learn.microsoft.com/en-us/azure/site-recovery/site-recovery-faq#does-site-recovery-encrypt-replication-)
	- In-transit and at-rest
	- Azure Disk Encryption (ADE) enabled VMs supported for replication
	- Customer Managed Keys (CMK) enabled managed disk VMs supported
- Consistency
	- [Crash](https://learn.microsoft.com/en-us/azure/site-recovery/azure-to-azure-common-questions#whats-a-crash-consistent-recovery-point)
		- On-disk data (no data from in-memory)
		- `RPO: 5 minutes` (automatically)
		- Appropriate for app servers, file servers
		- Not for databases servers
	- [Application](https://learn.microsoft.com/en-us/azure/site-recovery/azure-to-azure-common-questions#whats-an-application-consistent-recovery-point)
		- On-disk + data in memory
		- `RPO: 1-12 hours` (configurable)
		- For databases
	- [Multi-VM](https://learn.microsoft.com/en-us/azure/site-recovery/azure-to-azure-common-questions#multi-vm-consistency)
		- Consistent recovery points across multiple VMs
		- Replication group of all enabled VMs created
		- Shared crash-consistent and app-consistent recovery points
		- Single VM failover is not allowed
		- Up to 16 VMs
	- [Recovery points](https://learn.microsoft.com/en-us/azure/site-recovery/azure-to-azure-common-questions#how-far-back-can-i-recover)
		- Recovery points are prunded after two hours, saving one point per hour
		- Last two hours have 24 crash-consistent (2 hours*60 minutes/5 minutes) and two app-consistent points (1/hour) available
		- `Retention: 15 days (managed disk), 3 days (unmanaged disk)` ([configurable, and cost consideration](https://learn.microsoft.com/en-us/azure/site-recovery/azure-to-azure-common-questions#how-does-the-pruning-of-recovery-points-happen))
- [Recovery Plans](https://learn.microsoft.com/en-us/azure/site-recovery/recovery-plan-overview)
	- Model app around its dependencies
	- Test failovers and calculate RTO
	- Parallelism and sequencing of VM start up
	- Add manual actions to validate recovered application aspects that cannot be automated
	- Up to 100 protected items/plan
	- Machines can be referenced in multiple recovery plans, in which subsequent plans skip the deployment/startup of a machine if it was previously deployed using another recovery plan
[![](https://learn.microsoft.com/en-us/azure/site-recovery/media/recovery-plan-overview/rp.png)](https://learn.microsoft.com/en-us/azure/site-recovery/media/recovery-plan-overview/rp.png)
[![](https://learn.microsoft.com/en-us/azure/site-recovery/media/recovery-plan-overview/rptest.png)](https://learn.microsoft.com/en-us/azure/site-recovery/media/recovery-plan-overview/rptest.png)
[Example video](https://youtu.be/1KUVdtvGqw8)
- [Capacity Reservation](https://learn.microsoft.com/en-us/azure/site-recovery/azure-to-azure-common-questions#how-do-we-ensure-capacity-in-the-target-region)
	- Purchase capacity in DR region  and failover to that capacity
	- Create new or use existing Capacity Reservation Group (CRG)
	- Assign a CRG to VMs. On failover, new VM will be created in the assigned CRG
- Hot-Add Disk
	- Exclude undesired
	- Include new disks on a machine that is already replicating
- IP Addresses
	- [Public IP addresses](https://learn.microsoft.com/en-us/azure/site-recovery/azure-to-azure-common-questions#can-i-keep-a-public-ip-address-after--failover) cannot be retained after a failover
		- [Set up public IP addresses after failover](https://learn.microsoft.com/en-us/azure/site-recovery/concepts-public-ip-address-with-site-recovery)
			- Traffic Manager to reduce RTO
	- [Private IP addresses](https://learn.microsoft.com/en-us/azure/site-recovery/azure-to-azure-common-questions#can-i-keep-a-private-ip-address-after-failover) can be retained
		- ASR tries to provision the same IP address for the target VM for source VMs with static IP addresses
		- [Retaining IP address examples](https://learn.microsoft.com/en-us/azure/site-recovery/site-recovery-retain-ip-azure-vm-failover)
		- [Use a different IP address](https://learn.microsoft.com/en-us/azure/site-recovery/hyper-v-vmm-networking#use-a-different-ip-address)
			- Drawback: DNS and cache entries may need to be updated resulting in downtime - use short TTL values to mitigate
			- Can be useful to do subnet-level failovers i.e. subset of application - [example](https://learn.microsoft.com/en-us/azure/site-recovery/site-recovery-retain-ip-azure-vm-failover#resources-in-azure-isolated-app-failover)

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
