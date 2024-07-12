# Basics-of-ADF
#Task 1

 #step 1:
{
    "name": "LocalSQLServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": "Data Source=localhost;Initial Catalog=your_local_db;Integrated Security=True"
        },
        "connectVia": {
            "referenceName": "SelfHostedIntegrationRuntime",
            "type": "IntegrationRuntimeReference"
        }
    }
}

#Step 2

{
    "name": "AzureSQLDatabaseLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Server=tcp:your_azure_server.database.windows.net,1433;Database=your_azure_db;User ID=your_username;Password=your_password;Encrypt=True;Connection Timeout=30;"
        }
    }
}

step 3:

{
    "name": "LocalSQLServerDataset",
    "properties": {
        "linkedServiceName": {
            "referenceName": "LocalSQLServerLinkedService",
            "type": "LinkedServiceReference"
        },
        "type": "SqlServerTable",
        "typeProperties": {
            "tableName": "your_local_table"
        }
    }
}

step 4:

{
    "name": "AzureSQLDatabaseDataset",
    "properties": {
        "linkedServiceName": {
            "referenceName": "AzureSQLDatabaseLinkedService",
            "type": "LinkedServiceReference"
        },
        "type": "AzureSqlTable",
        "typeProperties": {
            "tableName": "your_azure_table"
        }
    }
}
step 5:

{
    "name": "CopyLocalToAzurePipeline",
    "properties": {
        "activities": [
            {
                "name": "CopyData",
                "type": "Copy",
                "dependsOn": [],
                "policy": {
                    "timeout": "7.00:00:00",
                    "retry": 0,
                    "retryIntervalInSeconds": 30,
                    "secureOutput": false,
                    "secureInput": false
                },
                "typeProperties": {
                    "source": {
                        "type": "SqlSource",
                        "sqlReader






















