---
title: "Mendix Data Loader"
url: /appstore/modules/snowflake/mendix-data-loader/
description: "Describes the configuration and usage of the Mendix Data Loader application from the Snowflake Marketplace."
weight: 20
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details. 
---

## 1 Introduction

The [Mendix Data Loader](https://app.snowflake.com/marketplace/listing/GZTDZHHIE0/mendix-mendix-data-loader) allows for seamless data ingestion from operational Mendix applications by using an exposed OData service into Snowflake, enhancing an organization's business intelligence and reporting capabilities.

### 1.1 Typical Use Cases

The Mendix Data Loader supports a range of data ingestion tasks, enabling organizations to leverage their operational data within Snowflake for analytical purposes. The key functionalities include ingesting data dynamically from Mendix applications (only needing an OData endpoint and credentials) to Snowflake. The ingested data is stored in the target schema of the target database specified by the user and created by the Mendix Data Loader application. This target schema in the target database serves as a staging area. The user should copy the tables of the target schema into a database and schema where they want to store the ingested data. This should be done after every ingestion.

### 1.2 Prerequisites {#prerequisites}

To use the Mendix Data Loader, you must have the following:

* A Mendix application with a [published OData service](https://docs.mendix.com/refguide/published-odata-services/) that includes exposed entities. 
* A Snowflake environment.

### 1.3 Licensing and Cost

The Mendix Data Loader is covered under the Mendix EULA. While the loader itself does not incur additional costs, operating within Snowflake may incur a usage cost. For more information, refer to the [Snowflake pricing documentation](https://www.snowflake.com/en/data-cloud/pricing-options/).

Depending on your use case, your deployment environment, and the type of app that you want to build, you may also need a license for your Mendix app. For more information, refer to [Licensing Apps](/developerportal/deploy/licensing-apps-outside-mxcloud/).

## 2 Installation

Follow instructions in [Install an app from a listing](https://other-docs.snowflake.com/en/native-apps/consumer-installing) to add the component to your Snowflake environment.

## 3 Configuration

Upon installation, configure the Mendix Data Loader as follows:

1. Open the application by accessing the Mendix Data Loader from the **MENDIX_DATA_LOADER** tab inside your Snowflake environment.
2. Ensure that the application has the `CREATE DATABASE` privilege in Snowflake. On first initialization, you will be prompted to grant the application this privilege.
3. Configure each data ingestion job by providing the following information:

    * **Endpoint** - The URL to the OData service of your Mendix application.
    * **Authentication** - The username and password for accessing the OData service.
    * **Target Database Name** - The Snowflake database target for the data ingestion.
    * **Target Schema Name** - The name of the target schema into which all the data will be ingested. Every time an ingestion is performed, all data already present in the target schema will be removed or replaced.
    * **Generate and Execute SQL Script** - Required only once, when configuring the data ingestion endpoint for the first time. Click the **Generate Script** button to produce and execute the required SQL scripts with necessary privileges.

4. Click **Ingest Data** to start the data transfer. All data exposed by the Odata service will be ingested and all ingested data will be stored in [transient tables](https://docs.snowflake.com/en/user-guide/tables-temp-transient#transient-tables).
5. To view the results, check the job ID and verify the data in the specified target database.

## 4 Technical Reference

### 4.1 Current Limitations

* At present, the Mendix Data Loader supports username and password authentication. Make sure to use username and password authentication when setting up your Odata service.
* Exposing an association in an Odata service is as a link is not supported yet by the Mendix Data Loader. Instead, choose the **As an associated object id** option in your Odata settings. This option will store the associated object ID in the table, but not explicitly as foreign key.
* The Mendix Data Loader supports single endpoint (OData) ingestion. If you want to ingest data from multiple endpoint, you can do this by ingesting the data from each endpoint separately one by one. Make sure to assign a different staging schema for every ingestion you do, or the previous ingestions will be overwritten. The ability to ingest data from multiple endpoints in one go will be added in a future release.
* At the moment the Mendix Data Loader does not support the scheduling of ingestion jobs. However, you can still achieve this by using a Snowflake worksheet. The ability to schedule ingestion jobs in your Mendix application will be added in a future release.
* The Mendix Data Loader always ingests all the data exposed by the OData published by your Mendix application. If you do not want to ingest all of the data inside the exposed entities, you must filter the data at the Mendix/OData side. 

### 4.2 Troubleshooting

If you encounter any issues while using the Mendix Data Loader, use the following troubleshooting tips to help you solve them.

For any additional troubleshooting, contact the [development team](mailto:sa_dev_team@mendix.com).

#### 4.2.1 Error Parsing JSON: Document Is Too Large

When ingesting data, the Mendix Data Loader shows an error similar to the following: `net.snowflake.client.jdbc.SnowflakeSQLException: Error parsing JSON: document is too large, max size 16777216 bytes`.

##### 4.2.1.1 Cause

The amount of data being ingested is so large that the JSON file has become too large to parse.

##### 4.2.1.2 Solution

To solve this issue, configure the exposed OData entities to have pagination. For the best performance, make the pages as large as possible while still ensuring that the JSON does not become too large to parse. 

{{% alert color="info" %}}
Pagination of exposed entities through OData is not availabl in Mendix version 10.10.0. This is a known issue that will be resolved in a future release.
{{% /alert %}}

### 4.3 Contact Information

For support or queries regarding the Mendix Data Loader, email the development team at [SA_Dev_Team@mendix.com](mailto:sa_dev_team@mendix.com).
