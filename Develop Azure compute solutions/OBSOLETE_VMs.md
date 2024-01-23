VMS ARE OBSOLETE FOR AZ-204]
## Virtual Machine

#### Availability Sets

##### Availability Set
**Availability se**t is a preferred grouping service that gives an insight how your application is built to provide for redundancy and availability. It is recommended to have two or more VMs within an **availability set**. 

==Fault Domain== is a feature that can be utilized for the availability set to make sure there is always one VM up and running in case of a shutdown/power failure or the VM server going down.

==Update Domain== is a feature like ==Fault Domain== but instead makes sure that not all VMs are updated at the same time, spreading out the update process so that at least one server is running in each availability set to at least one VM server still running during updates.

*When creating Availability Sets, Azure ensures that the VMs are spread across these domains. If you have 3 VMs in an Availability Set with 3 FDs and 3 UDs, each VM would be in a separate FD and UD.*


**VM TYPES**

| VM Type                | Use Case                                                                                                                                              | Typical Scenario                                                                                          |
|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| General Purpose        | Balanced CPU-to-memory ratio. Good for testing, development, and small to medium databases.                                                            | Running web servers, small to medium databases, and development environments.                              |
| Compute Optimized      | High CPU-to-memory ratio. Suitable for compute-bound applications that need high performance from the CPU.                                              | Running batch processing, analytics, gaming servers, and scientific simulations.                            |
| Memory Optimized       | High memory-to-CPU ratio. Suitable for applications that require a large amount of RAM.                                                                | Running large in-memory databases like SAP HANA, or other memory-intensive applications.                   |
| Storage Optimized      | Optimized for high disk throughput and IO. Suitable for data-heavy tasks that require high read and write speeds.                                        | Running big data, SQL, and NoSQL databases, data warehousing, and large transactional databases.           |
| GPU                    | Equipped with GPU resources. Suitable for graphics-intensive applications, deep learning, and parallel computing.                                        | Running deep learning models, graphics rendering, video editing, and other GPU-intensive tasks.            |
| High Performance Compute | High CPU and network performance. Suitable for complex computational tasks that require high-throughput and low-latency networking.                         | Running high-performance simulations, scientific research, financial modeling, and other HPC applications. |

##### VM Just-in-time access
It is a ==security== feature that you can enable for your Virtual Machine to make sure that it only accepts connection from pre-assigned IP addresses for a limited time.