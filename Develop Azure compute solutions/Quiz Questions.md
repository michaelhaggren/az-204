##### If you need to make sure that the client application is routed to the same instance for the life of the session, what should you choose?
<input type="checkbox"> ARR Infinity 
<input type="checkbox"> Always On 
<input type="checkbox"> HTTP Version
<details> <summary>Click to reveal the answer</summary> Answer: ARR Infinity.<br> Bottom line: Enable ARR Affinity if your app stores state on the instance, otherwise **disable it**.. </details>

##### What is the Always On feature within Azure App Services?

<details> <summary>Click to reveal the answer</summary> Always On is a feature in Azure App Services that makes sure your web app is always running, even if no one is visiting it. It's like having a caretaker for your website who keeps an eye on it all the time, making sure everything is working correctly. </details>


##### What timertrigger is this cron expression set to ( 0 15 8-10  * * * ) and when will it run?

<details> <summary>Click to reveal the answer</summary> It will run every 4 hours, every day for the rest of eternity, starting at midnight. </details>

##### Name three different options for adding certificates in an App Service
<details> <summary>Click to reveal the answer</summary> Import a certificate from a Key Vault <br>Upload a private certificate <br>Upload a public certificate </details>

##### True or False: A Standard_DS1_v2 VM is a good choice for a web server with low to moderate traffic?
<details> <summary>Click to reveal the answer</summary> Yes <br>
Explanation: The Standard_DS1_v2 VM is a good choice for a web server with low to moderate traffic as it offers a balance between CPU and memory resources. </details>

##### You need to deploy a Windows-based VM that will run a SQL Server database. Which VM size should you choose? 
A. Standard_D2_v3 
B. Standard_DS14_v2 
C. Standard_E4-2s_v3 
D. Standard_GS5
<details> <summary>Click to reveal the answer</summary> Answer: B. Standard_DS14_v2 Explanation: The Standard_DS14_v2 VM is a good choice for running a SQL Server database on a Windows-based VM as it offers high memory and storage capacity, and is optimized for I/O-intensive workloads. </details>

##### You need to deploy a VM that will run a SQL Server database. Which VM size should you choose? 
A. Standard_D2_v3 
B. Standard_D4_v3 
C. Standard_D8s_v3 
D. Standard_D16s_v3
<details> <summary>Click to reveal the answer</summary> Answer: B. Standard_D4_v3 Explanation: The Standard_D4_v3 VM is a good choice for running a SQL Server database as it provides a balance between CPU, memory, and storage resources. </details>

True or False: An availability set is a logical grouping of VMs within a data center region that ensures that they are deployed across multiple fault domains and update domains.
<details> <summary>Click to reveal the answer</summary> Answer: True Explanation: An availability set is a logical grouping of VMs that allows you to ensure that your VMs are deployed across multiple fault domains and update domains to minimize the impact of any single point of failure. </details>

You need to ensure high availability for your application by deploying VMs across multiple physical locations within a region. Which Azure feature should you use? A. Availability zones B. Availability sets C. Virtual networks D. Load balancers
<details> <summary>Click to reveal the answer</summary> Answer: A. Availability zones Explanation: Availability zones allow you to deploy VMs across multiple physical locations within a region to provide high availability and fault tolerance. </details>

True or False: Each availability zone is a separate physical location with its own power source, network, and cooling.
<details> <summary>Click to reveal the answer</summary> Answer: True Explanation: Each availability zone is a separate physical location within a region that is isolated from other zones with its own power source, network, and cooling. </details>

##### You need to deploy a mission-critical workload in Azure that requires high availability and fault tolerance across multiple geographic regions. Which Azure feature should you use? 
A. Availability zones
B. Availability sets 
C. Virtual networks
D. Azure Site Recovery
<details> <summary>Click to reveal the answer</summary> Answer: D. Azure Site Recovery is a disaster recovery solution that allows you to replicate your VMs and data across multiple geographic regions to ensure high availability and fault tolerance in case of a regional outage. </details>

True or False: Autoscaling is a feature in Azure that allows you to automatically adjust the number of instances of an application based on predefined rules.
<details> <summary>Click to reveal the answer</summary> Answer: True Explanation: Autoscaling in Azure allows you to automatically adjust the number of instances of an application based on predefined rules, such as CPU usage or queue length. </details>

##### What is a scale condition in Azure? 
A. A threshold that triggers an autoscaling action 
B. A specific time period during which an autoscaling action occurs 
C. A set of rules that define how many instances to add or remove during an autoscaling action 
D. A limit on the maximum number of instances that can be created during an autoscaling action
<details> <summary>Click to reveal the answer</summary> Answer: A. A threshold that triggers an autoscaling action Explanation: A scale condition is a threshold that triggers an autoscaling action, such as adding or removing instances based on resource utilization or other metrics. </details>

##### What is a scale rule in Azure? 
A. A threshold that triggers an autoscaling action 
B. A specific time period during which an autoscaling action occurs 
C. A set of rules that define how many instances to add or remove during an autoscaling action
D. A limit on the maximum number of instances that can be created during an autoscaling action
<details> <summary>Click to reveal the answer</summary> Answer: C. A set of rules that define how many instances to add or remove during an autoscaling action Explanation: A scale rule is a set of rules that define how many instances to add or remove during an autoscaling action, based on the scale conditions that are defined. </details>

##### What is the primary use of a load balancer in Azure? 
A. To distribute traffic evenly across multiple backend servers 
B. To provide a secure tunnel between on-premises and cloud resources 
C. To monitor the health of backend servers and perform automatic failover 
D. To provide a centralized location for managing virtual networks and subnets
<details> <summary>Click to reveal the answer</summary> Answer: A. To distribute traffic evenly across multiple backend servers Explanation: The primary use of a load balancer in Azure is to distribute incoming traffic evenly across multiple backend servers, improving application performance, availability, and scalability. </details>

##### You are managing a set of Linux-based virtual machines (VMs) in Azure. You need to automate the deployment of software updates and manage the configuration of these VMs. What should you use? 
A. Azure Automation State Configuration 
B. Azure DevTest Labs 
C. Azure Resource Manager Templates 
D. Azure Site Recovery
<details> <summary>Click to reveal the answer</summary> Answer: A. Azure Automation State Configuration Link to Documentation </details>

##### You are developing an application that requires a scalable container orchestration solution on Azure. The application needs to manage containers, provide automated updates, and allow for monitoring and diagnostics. Which of the following is the option you should use? 
A. Azure Container Instances (ACI) 
B. Azure Kubernetes Service (AKS)
C. Azure Container Registry 
D. Azure Virtual Machines with Docker
<details> <summary>Click to reveal the answer</summary> Answer: B. Azure Kubernetes Service (AKS) <br>It's designed for precisely the requirements you mentioned: container orchestration, automated updates, and robust monitoring and diagnostics capabilities. With your proficiency in Azure, diving into AKS should be a logical next step. </details>

##### You are tasked with improving the performance of a virtual machine (VM) that hosts a database. The VM experiences slow disk I/O. Solution: You change the VM's disk type to Premium SSD. Does the solution meet the goal? 
A. Yes
B. No
<details> <summary>Click to reveal the answer</summary> Answer: A. Yes Link to Documentation </details>

##### You are responsible for managing virtual machines (VMs) in Azure for a financial services company. You need to ensure data encryption both at rest and in transit and comply with industry regulations. What should you use? 
A. Azure Disk Encryption and Azure VPN Gateway
B. Azure Information Protection and Azure ExpressRoute 
C. Azure Disk Encryption and Azure Information Protection 
D. Azure VPN Gateway and Azure ExpressRoute
<details> <summary>Click to reveal the answer</summary> Answer: A. Azure Disk Encryption and Azure VPN Gateway Link to Documentation for Azure Disk Encryption Link to Documentation for Azure VPN Gateway </details>

##### Your company wants to deploy a containerized application in Azure and needs to pull container images from a private registry. You must ensure that the container instances have access to the images in the private registry without storing the credentials in the application code. What should you use?
A. Azure Container Registry with managed identity 
B. Azure Kubernetes Service (AKS) with Azure Active Directory (AD) integration
C. Azure Container Instances (ACI) with managed identity 
D. Azure Container Registry with Azure Key Vault
<details> <summary>Click to reveal the answer</summary> Answer: C. Azure Container Instances (ACI) with managed identity </details>