#Task 3
#step 1

{
    "name": "SourceDatabaseLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": "Data Source=source_server;Initial Catalog=source_db;Integrated Security=True"
        }
    }
}
#step2 
{
    "name": "SourceDataset",
    "properties": {
        "linkedServiceName": {
            "referenceName": "SourceDatabaseLinkedService",
            "type": "LinkedServiceReference"
        },
        "type": "SqlServerTable",
        "typeProperties": {
            "tableName": "source_table"
        }
    }
}
#step 3
{
    "name": "IncrementalLoadPipeline",
    "properties": {
        "activities": [
            {
                "name": "GetLastLoadDate",
                "type": "Lookup",
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
                        "type": "AzureSqlSource",
                        "query": "SELECT MAX(load_date) AS LastLoadDate FROM destination_table"
                    }
                },
                "dataset": {
                    "referenceName": "DestinationDataset",
                    "type": "DatasetReference"
                }
            },
            {
                "name": "IncrementalLoad",
                "type": "Copy",
                "dependsOn": [

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
                        "sqlReaderQuery": {
                            "value": "SELECT * FROM source_table WHERE last_modified_date > '@{activity('GetLastLoadDate').output.firstRow.LastLoadDate}'"
                        }
                    },
                    "sink": {
                        "type": "SqlSink",
                        "writeBatchSize": 1000
                    }
                },
                "inputs": [

            }
        ]
    }
}
#step 4

{
    "name": "DailyTrigger",
    "properties": {
        "type": "ScheduleTrigger",
        "typeProperties": {
            "recurrence": {
                "frequency": "Day",
                "interval": 1
            }
        },
        "pipelines": [
        {
                "pipelineReference": {
                    "referenceName": "IncrementalLoadPipeline",
                    "type": "PipelineReference"
                }
            }
        ]
    }
}






