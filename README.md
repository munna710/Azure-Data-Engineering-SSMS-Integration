

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

## SQL Server
![ssms](https://github.com/munna710/Azure-Data-Engineering-SSMS-Integration/blob/main/images/ssms.png)
The SQL scripts and queries that I used to create a login and add a database in Microsoft SQL Server Management Studio. The database is AdventureWorksLT2012, a sample database that contains data about a fictional company that sells bicycles and bicycle accessories. The database is used for learning and practicing SQL Server features and functionalities.

## Steps
- To create a login, I used the CREATE LOGIN statement with the name, password, and default database options. I also granted the login the db_owner role on the master database using the ALTER SERVER ROLE statement.
- To add a database, downloaded the AdventureWorksLT2022.bak file from [Microsoft SQL Server Samples](https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms).
- Created username and password as secrets in keyvault
- Created and configured a [self-hosted integration runtime](https://learn.microsoft.com/en-us/azure/data-factory/create-self-hosted-integration-runtime?tabs=data-factory).

# Ingestion
## Data Factory-copy-one-table

![ADF](https://github.com/munna710/Azure-Data-Engineering-SSMS-Integration/blob/main/images/ADF.png)

The Azure Data Factory pipeline that I created to copy address table data from on premise to data lake storage gen2. The pipeline uses the Copy Data activity to perform the data movement operation.
## Steps
- To create the pipeline, I used the Azure Data Factory web interface and dragged and dropped the Copy Data activity from the **author>>pipeline>>newpipeline**to the canvas.
- To configure the source and sink of the Copy Data activity, I used the New Dataset option and selected the **data lake storage gen2**and **parquet** format for each.
- To run the pipeline, I used the Debug option and monitored the activity run in the Output window.

## Data Factory-copy-all-tables

![ADF](https://github.com/munna710/Azure-Data-Engineering-SSMS-Integration/blob/main/images/copy-all-tables.png)
the Azure Data Factory pipeline that I created to copy data from multiple tables in a source database to a data lake storage gen2. The pipeline uses the following activities:

- **Lookup**: This activity queries the source database and returns a list of table names that match a certain condition.
- **Foreach**: This activity iterates over the list of table names and passes each one as an item to the next activity.
- **Copy Data**: This activity copies the data from each table in the source database to a corresponding table in the sink database.

## Steps
- To create the pipeline, I used the Azure Data Factory web interface and dragged and dropped the activities from the Author menu to the canvas.
- To configure the lookup activity, I used the New Dataset option and selected the source database as the dataset. I also specified the query to get the table names from the database.
- To configure the foreach activity, I used the Items option and selected the output of the lookup activity as the source. I also enabled the sequential option to run the iterations one by one.
- To configure the copy data activity, I used the New Dataset option and selected the source and sink databases as the datasets. I also used dynamic content to specify the table names for each dataset using the item variable from the foreach activity.
- To run the pipeline, I used the Debug option and monitored the activity runs in the Output window.
![ADF](https://github.com/munna710/Azure-Data-Engineering-SSMS-Integration/blob/main/images/query1.png)
![ADF](https://github.com/munna710/Azure-Data-Engineering-SSMS-Integration/blob/main/images/query3.png)
![ADF](https://github.com/munna710/Azure-Data-Engineering-SSMS-Integration/blob/main/images/query4.png)
![ADF](https://github.com/munna710/Azure-Data-Engineering-SSMS-Integration/blob/main/images/query5.png)
![ADF](https://github.com/munna710/Azure-Data-Engineering-SSMS-Integration/blob/main/images/query6.png)
![ADF](https://github.com/munna710/Azure-Data-Engineering-SSMS-Integration/blob/main/images/query7.png)

# Transformation
- [Bronze to Silver](https://github.com/munna710/Azure-Data-Engineering-SSMS-Integration/blob/main/bronze%20to%20silver.ipynb)
- [Silver to Gold](https://github.com/munna710/Azure-Data-Engineering-SSMS-Integration/blob/main/silver%20to%20gold.ipynb)
- Run a Databricks notebook with the Databricks Notebook Activity in Azure Data Factory.
![ADF](https://github.com/munna710/Azure-Data-Engineering-SSMS-Integration/blob/main/images/ADF-DB.png)


# Loading
### created view for address table

![ADF](https://github.com/munna710/Azure-Data-Engineering-SSMS-Integration/blob/main/images/m1.png)
- The SQL scripts that I used to create and query data in Azure Data Lake Storage Gen2. Azure Data Lake Storage Gen2 is a scalable and secure data lake service that supports the Azure Synapse Analytics platform. I used the Azure Synapse Studio web interface to write and execute SQL scripts on the data lake.

## Steps
- To create a view, I used the CREATE VIEW statement with the name and query of the view. The query used the OPENROWSET function to read data from a Delta file stored in the data lake.
- To query the view, I used the SELECT statement with the columns and conditions of the query. The query returned the address data from the view in a tabular format.
![ADF](https://github.com/munna710/Azure-Data-Engineering-SSMS-Integration/blob/main/images/m1.png)

### created view for all tables
 ![ADF](https://github.com/munna710/Azure-Data-Engineering-SSMS-Integration/blob/main/images/m2.png)
- The SQL scripts that I used to create and query data in Azure Data Lake Storage Gen2.
- 
## Steps
- To create a stored procedure, I used the CREATE OR ALTER PROC statement with the name and parameters of the procedure. The procedure creates or alters a view using the OPENROWSET function to read data from a Delta file stored in the data lake.
- To execute the stored procedure, I used the EXEC statement with the name and arguments of the procedure. The procedure returns the data from the view in a tabular format.

### the Azure Data Factory pipeline that I created to perform various data operations using stored procedures. The pipeline uses the following activities:

- **Get Metadata**: This activity retrieves the metadata of a dataset, such as the file name, size, and type.
- **Get Tablenames**: This activity executes a SQL query on a database and returns a list of table names that match a certain condition.
- **Foreach**: This activity iterates over the list of table names and passes each one as an item to the next activity.
- **Stored procedure**: This activity executes a stored procedure on a database using the item variable as an argument. The stored procedure creates or alters a view using the OPENROWSET function to read data from a Delta file stored in the data lake.

## Steps
- To create the pipeline, I used the Azure Synapse Studio web interface and dragged and dropped the activities from the Activities menu to the canvas.
- To configure the activities, I used the Settings option and specified the properties and parameters for each activity, such as the dataset, the query, the stored procedure name, and the item variable.
- To monitor the pipeline, I used the Monitor option and viewed the activity runs in the List or Gantt view. I also exported the activity runs to a CSV file for further analysis.
![ADF](https://github.com/munna710/Azure-Data-Engineering-SSMS-Integration/blob/main/images/m3.png)
![ADF](https://github.com/munna710/Azure-Data-Engineering-SSMS-Integration/blob/main/images/m4.png)
