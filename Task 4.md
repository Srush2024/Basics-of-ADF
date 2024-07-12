#Task 4
#step 1

{
    "name": "MyPipeline",
    "properties": {
        "activities": [
            {
                "name": "MyActivity",
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
                        "sqlReaderQuery": "SELECT * FROM source_table WHERE last_modified_date > @last_run_date"
                    },
                    "sink": {
                        "type": "SqlSink"
                    }
                },
                "inputs": [
                    {
                        "referenceName": "SourceDataset",
                        "type": "DatasetReference"
                    }
                ],
                "outputs": [
                    {
                        "referenceName": "DestinationDataset",
                        "type": "DatasetReference"
                    }
                ]
            }
        ]
    }
}
#step 2
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "name": "[concat(parameters('dataFactoryName'), '/LastSaturdayTrigger')]",
            "type": "Microsoft.DataFactory/factories/triggers",
            "apiVersion": "2018-06-01",
            "properties": {
                "type": "ScheduleTrigger",
                "pipelines": [
                    {
                        "pipelineReference": {
                            "referenceName": "MyPipeline",
                            "type": "PipelineReference"
                        }
                    }
                ],
                "typeProperties": {
                    "recurrence": {
                        "frequency": "Month",
                        "interval": 1,
                        "startTime": "2021-01-01T00:00:00Z",
                        "schedule": {
                            "weekDays": ["Saturday"],
                            "monthDays": [-1]
                        }
                    }
                }
            }
        }
    ],
    "parameters": {
        "dataFactoryName": {
            "type": "string"
        }
    }
}


