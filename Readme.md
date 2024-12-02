# Cloud Architecture

![Screenshot 2024-11-30 005820](https://github.com/user-attachments/assets/37e51a9e-e5a5-4f1a-ab7a-3d3f4508b719)

Flow of the architecture

1.	Frontend Request Handling:

   * The Cloud CDN receives user requests and caches content to lower latency.
   
   * The Cloud Load Balancer receives non-cached requests and directs them to the relevant backend service (GKE or GCE).

2.	Application Processing:

   * GKE handles requests for updated services (such ERP microservices or e-commerce).
   
   * Requests that have not yet been modernized or containerized are handled by legacy workloads hosted on GCE.

   * GKE and mainframe modernization services work together to migrate ERP.

 3.	Data Flow:

   * Cloud SQL is used by applications to exchange relational transactional data.
   	
   * For a smooth transfer, Datastream replicates real-time data between on-premises databases and Cloud SQL or Cloud Spanner.

   * BigQuery receives transactional or operational data for analytics.

4.	Storage & Backup:

   * Cloud storage is where backups and application data are kept.
   
   * Filestore offers shared file systems to applications that require long-term storage.

5.	Networking:

   * Cloud VPN offers safe communication between the Google Cloud environment and on-premises infrastructure.
     
   * VPC manages service segmentation and traffic routing.

6.	Monitoring & Security:

   * The Operations Suite keeps an eye on every service for alerts and performance.
     
   * DLP is used to ensure data compliance, and IAM is used to handle access control.

7.	Disaster Recovery:

   * For high availability, backups (Cloud Storage) and critical databases (Cloud SQL, Spanner) include failover systems or secondary replicas in several locations.






 





To begin with migration plan we are currently working on the IT landscape, we are dealing with monolithic ERP system which are characterized by the performance issue, scalability limitation, security risk and so on.

So our migration strategy is to perform either a refactoring or a rehosting

## Migration phases
### Phase 1.	Planning and assessment
### Inventory of Resources:
Tools: Use Google Cloud Migrate for  all on-premises workload

Analyze current workloads with Google Cloud Migrate, including the SQL database cluster, mainframe, e-commerce apps, monolithic ERP, and 150 virtual machines.

VMs: Examine the 150 virtual machines (Windows Server and Linux) to record information about installed apps, CPU, memory, and storage usage, and OS versions.

Monolithic ERP System: Determine the database connections, system components, and particular dependencies (such as on other virtual machines or apps).

Determine the web servers, application servers, and backend connections that make up e-commerce applications.

SQL Database Cluster: Compile data on dependencies, transaction rates, schema, and database size.

### Phase 2: Planning & Design
a. 150 virtual machines (a combination of Linux and Windows) 

b. A monolithic ERP system that controls inventories and finances

c. Applications for e-commerce (must remain online)

d. A SQL database that houses private information

e. Payroll and reports are handled via the legacy mainframe system.

### Migration Startegy Overview

| **System**         | **Migration Approach**                        | **Target GCP Service**         |
|---------------------|-----------------------------------------------|---------------------------------|
| Virtual Machines    | Lift and Shift (move as-is)                   | Compute Engine                 |
| ERP System          | Refactor (modernize into smaller services)    | Google Kubernetes Engine (GKE) |
| E-commerce Apps     | Lift and Shift, then improve later            | Compute Engine or GKE          |
| SQL Database        | Migrate and enhance (keep it secure)          | Cloud SQL or BigQuery          |


### Phase 3: Migration Execution

#### 1. VM Migration

The 150 virtual machines (Windows and Linux) will be transferred from on-premises to GCP.

Approach  
• To "lift and shift" (transfer them as-is), use Google Cloud Migrate.

#### 2.	Migrate the SQL Database
   
What’s Happening?

•	Move in-house SQL database to Cloud SQL or BigQuery for better security and performance.

Approach?

•	Use Database Migration Service to move the data without stopping operations.

#### 3. Modernize the ERP system

We will disassemble the ERP system into more manageable, smaller components rather than retaining it as a single entity.

Approach?

Transform it into microservices and use Google Kubernetes Engine (GKE) to deploy them.

### Phase 4: Post Migration

It's time to ensure that everything is operating optimally and correctly after transferring everything to the cloud. Checking, adjusting, and making sure everything is optimum for long-term use are all part of this step.

#### 1. Test Performance and Scalability

We have to determine whether the systems can manage the load and scale up as necessary.

To replicate real-world usage and high traffic, do load testing and stress testing (particularly for e-commerce apps).

#### 2. Ensure Security Compilance

The cloud needs to be set up in a way that protects your data, meets regulations (like GDPR or HIPAA), and ensures secure access.

•  Data encryption (making sure customer and business data is protected).

•  Identity and access management (who can access what, and ensuring proper authentication).

#### 3. Cost Optimization

Use GCP’s cost management tools to analyze your usage and see where money can be saved

#### 4. Monitor System Health and Performance Continuously

Use Google Cloud Monitoring and Logging to set up automated alerts and dashboards that track system performance, uptime, and errors.
