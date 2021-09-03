# Synapse Features FAQ

### **Q**: _When will we review backup and restore scenarios?_

**A**: This is a great topic we'll explore throughout the next 12 months as part of an ongoing architecture design/review series.

---

### **Q**: _Any best practices of using Spark compute vs. SQL Provisioned compute to query SQL POOL storage?_

**A**: Querying the SQL pool data via the SQL analytics connector in Spark has a particular overhead when compared to directly querying the SQL pool. When that overhead is acceptable, the advantage of not needing to leave the Spark environment takes precedence. In addition to that, the RDD features of Spark kick in and allow you do perform highly parallelized data processing.

---

### **Q**: _Is Synapse using a form of Azure Notebooks?_

**A**: They are similar. Synapse uses Juypter Notebooks (like Azure Notebooks) with nteract. But it's just a convention enforced in the UI. For Spark notebooks, the Apache Livy API is in charge, so you can create sessions and submit statements from anywhere. Livy provides a REST API to manage and execute Spark sessions.

---

### **Q**: _I saw error trying to save/publish Notebook stating that it exceed the 2Mb limit? Is this a workspace configuration where we can change that limit or a hard limit for now? Seems rather smallish?_

**A**: Not currently.

---

### **Q**: _Is there a public doc link for the 90+ inbuilt connectors handy?_

**A**: https://docs.microsoft.com/en-us/azure/data-factory/connector-overview

---

### **Q**: _Can Synapse pipelines create a table automatically in the sink without one being there before?_

**A**: Yes, it can be done, but this is not recommended for production. There is a generate schema option.

---

### **Q**: _Do the latest features in ADF automatically show up in Synapse studio since they're the same thing?_

**A**: Yes, ADF and Synapse pipelines will be identical as they share the same code base. Same bits deployed at the same time (except the SSIS runtime).

---

### **Q**: _Can I run my ADF pipelines from Synapse?_

**A**: Not directly. You can, for example call an Azure Function that would triger an ADF pipeline.

---

### **Q**: _How can the data flow from Azure Databricks to Synapse be encrypted?_

**A**: Here is a link that can help https://docs.microsoft.com/en-us/azure/azure-databricks/databricks-extract-load-sql-data-warehouse#load-data-into-azure-synapse

---

### **Q**: _Is Copy here a Copy Activity of ADF?_

**A**: Copy is an option in a DW Sink of the Copy activity in ADF.

---

### **Q**: _I thought SSIS was not supported in Synapse, how can we do the alternative from On-Prem suggesting to use this?_

**A**: You would use a self-hosted integration runtime that is installed on-premises.

---

### **Q**: _What is technically behind this serverless SQL endpoint? A shared cross-customer SQL Pool?_

**A**: The resources are on-demand but dedicated to the customer at the workspace level. That's why the first query takes ~25-30 seconds. It allocates the resources.

---

### **Q**: _When will Spark 3.0 come in?_

**A**: We will do it, but we need it to GA first. There will be a gap between it going GA and us making it available.

---

### **Q**: _Will you guys support a similar concept to Runtimes? DB does a great job with this. It is not perfect, but customers love it._

**A**: There are currently no plans for different runtimes, given that the current ML and Platform runtimes will merge. We need to see what the future looks like.

---

### **Q**: _Can the spark pools be modified based on need so that I can go up and down as needed?_

**A**: We expect people will create many spark pools as they are just metadata when you connect we spin a cluster to run the job it scales as needed and then you hit the time to live on the cluster (15 mins by default, 30 mins from a notebook) which you can change and when the job is complete the cluster will shut down.

---

### **Q**: _Different Spark pools can have different Spark versions, right?_

**A**: Eventually, yes, we only support one version right now, and we will be very aggressive about keeping the version matrix small vs. say HDI.

---

### **Q**: _What about Data Wrangling?_

**A**: Data Wrangling is something still being worked on.

---

### **Q**: _Is there a baked in Hive store for supporting SQL within the Spark layer, or is all SQL expected to be within the SQL pools and entirely separate from the Spark layer? I'm asking about combining PySpark and SQL...and the ability to create temporary views etc., switching between PySpark, Spark SQL, and back in a notebook._

**A**: Yes this is fully supported. You can see Hive in the attached Data Lake system storage if you know where to look and have the right permissions.

---

### **Q**: _How compatible will the notebooks be with standard Jupyter notebooks? thinking about things like support for nbextensions?_

**A**: The notebooks use nteract and can be imported/exported as ipynb. nbextensions don't work, yet. We are working on a pure jupyter compatibility layer for the AML team so their jupyter notebooks can just use Synapse Spark as a remote kernel, so we will be able to support classic jupyter, but it will not be included in the Studio at least for now.

---

### **Q**: _What about Polybase?_

**A**: There will be lots of improvements to reading and writing external data from Synapse SQL.

---

### **Q**: _I am currently working with a customer presently using AWS Redshift and looking to make a TCO comparison on Azure Synapse (SQL option) Analytics._

**A**: You can find our TCO calculator and Redshift compete-materials on aka.ms/azure_synapse_overview.

---

### **Q**: _Does Azure Synapse replace entirely (and surpasses) Azure Databricks?_

**A**: No, Azure Synapse is complimentary to Azure Databricks. Customers who are already using Azure Databricks can benefit from Synapse capabilities while continuing to use Azure Databricks in their solution.

---

### **Q**: _Can I link a Cosmos DB account?_

**A**: Yes. This capability is made available thru the [Azure Synapse Link for Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/configure-synapse-link) feature.  

---

### **Q**: _If I want to run ML models, I have to resort to Spark, not SQL, right?_

**A**: Today, for scoring we have the Predict function, which is there in SQL Pools. Use Predict to score data against an ONNX model.
Training models in SQL is something that is being talked about but not available. To train models you can use Synapse Notebooks. 

---

### **Q**: _Can I set up Spark queues or multiple pools with different permissions?_

**A**: Yes, each Spark Pool is securable. So you can have different permissions.

---

### **Q**: _Does it support EBCDIC (mainframe) unstructured files with metadata, including audio, video and visual files?_

**A**: Not directly, but via Spark you can process virtually any kind of file format/structure.

---

### **Q**: _Will the Spark notebooks have a parameter interface? Widgets and ADF are super helpful._

**A**: You will be able to have a parameter cell and then bind to that from ADF etc. 
We currently don't plan to provide UI widgets as a way of interacting with those.

---

### **Q**: _Does it support delta lake since it's now open-sourced?_

**A**: Yes, we can run the open-source version of Delta Lake.

---

### **Q**: _Is Spark R supported?_

**A**: No.

---

### **Q**: _Are there options other than memory-optimized machines, like GPU?_

**A**: Currently, no other options, our Spark team decided to prioritize memory-optimized machines since they're best suited to most workloads.

---

### **Q**: _Why not provide an on-demand pool option on Spark as well?_

**A**: We're working on bringing the Spark pool deployment time to be quite fast, minimizing the need for an on-demand pool option. Most GPU workloads can be parallelized without Spark reasonably quickly, so you'd be able to use an AML Compute cluster.

---

### **Q**: _Is there a way to prevent the user from importing any library? Or route the user to a private repo?_

**A**: Not currently, but that sort of fine-grained package management is something other customers have requested and something we're looking at.

---

### **Q**: _Do we have access control at Notebook level like DataBricks?_

**A**: Currently, no. It's just at the Spark admin level. But, many customers have requested securable folders and files similar to Databricks, so it's something we are working on.

---

### **Q**: _What is the notebook platform, Jupyter, or something else?_

**A**: Jupyter + nteract

---

### **Q**: _How does AML service fit into all of this?_

**A**: AML integrates with Synapse spark through packages and connecting to an AML workspace where you can register models, run AutoML, etc. 

---

### **Q**: _Do you think that Workload Management will still be relevant with v3 / multi-cluster (i.e., one cluster by user or type of tasks)? If so, can you give examples of why?_

**A**: Yes! We see workload management as a toolbox, where there are different tools for different use-cases. For example, workload isolation is excellent at handling fast variations in resource utilization at low overhead. Auto-scale can be better for opening up more resources or lowering costs, but you don't want to continually scale up or down as it evicts the cache and affects performance. Multi-cluster is effective at providing chargeback and isolating more predictable jobs, such as daily loads. Each has its best use-case, and the goal of Synapse is to provide customers with the entire toolbox.

---

### **Q**: _What is Gen 3?_

**A**: The set of "gen 3" improvements (multi-cluster, online scale, etc.) are not tied to the general synapse preview/GA - it's a separate development effort. 

---

### **Q**: _Mapping Dataflows in Synapse workspace assume they spin up Synapse Spark Pools behind the scenes, unlike ADF (DataBricks)... correct?_

**A**: The first edition did use Azure Databricks, now mapping Data Flows run directly on Synapse Spark Pools.

---

### **Q**: _What is the AML integration roadmap?_

**A**: Can't say too much besides that as of May 2020, it's definitely on our roadmap. Given the importance of ML to a lot of analytics use-cases, expanding the ML integration is something the team is actively looking into.

---

### **Q**: _What's going to happen for our customers already using SQL DW? Is the cost going to change for them? How are we going to notify the new features to them like the Studio?_

**A**: We are aiming for minimal disruption to our existing SQL Pool customers. The price will not change as the feature set is the same, and there's no overhead cost from Synapse. As we move closer to GA, we will have a more precise story for how customers will be able to access all the new features with as little disruption as possible.

---

### **Q**: _All that we can do with the new Synapse Studio, will we be able to do with Azure Data Studio or SQL Management Studio?_

**A**: You can connect to the serverless SQL and SQL Pool endpoints in Azure Data Studio and SSMS to submit queries. Further integration is on the roadmap right now. (May 2020)

---

### **Q**: _How do I do version control on the SQL Scripts?_

**A**: Version control/file control is one of the most significant asks we've gotten from customers and something we're working on as we move towards GA. (May 2020)

---

### **Q**: _How do folders and RBAC work for SQL Scripts and the like, If I have 200 devs how do they segment their work?_

**A**: Currently does not exist in the Synapse preview. (May 2020).

---

### **Q**: _Is GPU supported in Synapse Spark?_

**A**: No GPU support. Synapse Spark is for data engineering, not ML/deep learning. Data scientists should use Azure Machine Learning.

---

### **Q**: _Will Azure ML support Synapse Spark pool as compute?_

**A**: Yes, Synapse Spark as AML compute is on the roadmap and will have deeper integration than the current Databricks integration (May 2020).

---

### **Q**: _Is three a concept similar to ADB Interactive Cluster?_

**A**: No interactive cluster. For non-production, high concurrency is used for cost-saving (dev sharing environments) - in Synapse the price point will be lower so each dev can have their cluster at a similar price point than ADB high concurrency. For production, point the report tools to the SQL endpoint in Synapse instead of the Spark endpoint.

---

### **Q**: _Any GitHub integration or plans?_

**A**: Yes for GA (May 2020)

---

### **Q**: _Is there a view for understanding jobs that were submitted, deep-dive what steps were executed?_

**A**: This will be located in the Monitoring Hub of Synapse Studio.

---

### **Q**: _Can we migrate the data in one go?_

**A**: Recommended solution is to move data from source to data lake or blob using copy statement and then push it into Synapse SQL database.

---

### **Q**: _Is there a typical data set size at which azure synapse becomes relevant and appropriate?_

**A**: It depends on the business requirements whether you need a warehouse in which data can grow or you need a fast data load ingestion like spark. You should also think of integrations also because synapse is a unified platform.

---

### **Q**: _When I saw Power BI Integration in synapse the version is very old, is it automatically synced with latest version?_

**A**: Power BI service is always up to date.

---

### **Q**: _Can I link more than one Power BI workspace to a single Azure Synapse Workspace?_

**A**: Currently, you can only link a single Power BI workspace to an Azure Synapse Workspace.

---

### **Q**: _Does Azure Synapse workspace Support CI/CD?_

**A**: Yes! All Pipeline artifacts, notebooks, SQL scripts, and Spark job definitions will reside in Github or Azure Repos.

---

### **Q**: _How do I migrate existing pipelines from Azure Data Factory to an Azure Synapse workspace?_

**A**: At this time, you must manually recreate your Azure Data Factory pipelines and related artifacts by exporting the JSON from the original pipeline and importing it into your Synapse workspace.

---

### **Q**: _What is the default deployment of dedicated SQL pools now?_

**A**: By Default, all new dedicated SQL pools will be deployed to a workspace; however, if you need to you can still create a dedicated SQL pool (formerly SQL DW) in a standalone form factor.

---

### **Q**: _Can I 'aquire lease' on a folder using pyspark notebook? If yes, is there any Microsoft spark utilities api for me to help?_

**A**: If concurrency is an issue, might I suggest using a Tumbling Window Trigger?

---

### **Q**: _synapse workspace via mobile phone hotspot_

**A**: While connected using Mobile hotspot, just try to go to Synapse > Networking > Firewall rules and click on Add Client IP button. See if that works.

---

### **Q**: _Azure Synapse: Managed Private Endpoints Deployment_

**A**: Below Rest API allows you to create managed end points programmatically. You can utilize this REST API calls to automate your process. Please refer the [link](https://docs.microsoft.com/en-us/rest/api/synapse/data-plane/managed-private-endpoints/create) for more info.

---

### **Q**: _Copying Data from Blob to Azure Synapse_

**A**: Please refer the [Link](https://docs.microsoft.com/en-us/azure/data-factory/tutorial-copy-data-tool)

---

### **Q**: _Is it possible to share metadata between spark pool and dedicated sql pool?_

**A**: No, currently it can only be shared spark pools and serverless pools.

---

### **Q**: _Is it possible to share metadata between spark pool and dedicated sql pool?_

**A**: No, currently it can only be shared spark pools and serverless pools.

---

### **Q**: _Is it possible to share metadata related to delta format files between spark/sql pool and dedicated sql pool?_

**A**: It is not possible currently and only databases and their contained external and managed tables that uses Parquet Storage format can be shared with workspace SQL engine. Please refer to the [documentation](https://docs.microsoft.com/en-us/azure/synapse-analytics/metadata/overview) for more info.

---

### **Q**: _What does Azure Synapse offer for data security?_

**A**: Azure Synapse offers several solutions for protecting data such as TDE and auditing. For more information, see [reference](https://docs.microsoft.com/en-us/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-manage-security).

---

### **Q**: _Where can I find out what legal or business standards Azure Synapse is compliant with?_

**A**: Visit the Microsoft Compliance [page](https://www.microsoft.com/trustcenter/compliance/complianceofferings) for various compliance offerings by product such as SOC and ISO.

---

### **Q**: _Can I connect Power BI?_

**A**: Yes! Though Power BI supports direct query with Azure Synapse, it's not intended for a large number of users or real-time data. To optimize Power BI performance further, consider using Power BI on top of Azure Analysis Services or Analysis Service IaaS.

---

### **Q**: _Does dedicated SQL pool (formerly SQL DW) support REST APIs?_

**A**: Yes. Most REST functionality that can be used with SQL Database is also available with dedicated SQL pool (formerly SQL DW).

---

