

![Project Architecture](https://github.com/munna710/Azure-Data-Engineering-SSMS-Integration/blob/main/images/project-architecture.png)
## Description
This repository contains the implementation of our data processing and analytics architecture using Azure services. The architecture is designed to ingest, process, analyze, and visualize data efficiently and securely.

## Architecture Overview
The architecture consists of several components including an on-prem SQL Server Database, Azure Data Factory (ADF), Azure Data Lake Gen2, Azure Databricks, Azure Synapse Analytics, Power BI for visualization, and security & governance tools like Azure Active Directory and Azure Key Vault.

### Components
- **On-prem SQL Server Database**: The source of our raw data.
- **Azure Data Factory (ADF)**: Responsible for orchestrating and automating the data workflows.
- **Azure Data Lake Gen2**: Stores large amounts of structured and unstructured data.
    - **Bronze Layer**: Raw data storage layer.
    - **Silver Layer**: Contains cleaned and processed data.
    - **Gold Layer**: Stores the finalized datasets ready for analysis.
- **Azure Databricks**: Used for big data analytics and artificial intelligence workloads.
- **Azure Synapse Analytics**: Integrates analytics into the architecture to generate insights from the processed data.
- **Power BI**: A business analytics tool used for visualizing the analytical results.

### Security & Governance
- **Azure Active Directory**: Offers identity and access management services to secure resources.
- **Azure Key Vault**: Used to safeguard cryptographic keys and other secrets used by cloud services.

The SQL scripts and queries that I used to create a login and add a database in Microsoft SQL Server Management Studio. The database is AdventureWorksLT2012, a sample database that contains data about a fictional company that sells bicycles and bicycle accessories. The database is used for learning and practicing SQL Server features and functionalities.

## Steps
- To create a login, I used the CREATE LOGIN statement with the name, password, and default database options. I also granted the login the db_owner role on the master database using the ALTER SERVER ROLE statement.
- To add a database, downloaded the AdventureWorksLT2012.bak file from [Microsoft SQL Server Samples](https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms).



