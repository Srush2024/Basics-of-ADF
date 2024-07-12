#Task 2
#step 1
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "ftp.example.com",
            "port": 21,
            "username": "your_username",
            "password": {
                "type": "SecureString",
                "value": "your_password"
            }
        }
    }
}

#step 2
{
    "name": "SFTPLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "sftp.example.com",
            "port": 22,
            "username": "your_username",
            "password": {
                "type": "SecureString",
                "value": "your_password"
            }
        }
    }
}
step 3
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=your_account_name;AccountKey=your_account_key;EndpointSuffix=core.windows.net"
        }
    }
}
step 4
{
    "name": "CopyFTPToAzurePipeline",
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
                        "type": "FileSource"
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "inputs": [

            }
        ]
    }
}

step 5

{
    "name": "SFTPSourceDataset",
    "properties": {
        "linkedServiceName": {
            "referenceName": "SFTPLinkedService",
            "type": "LinkedServiceReference"
        },
        "type": "FileShare",
        "typeProperties": {
            "location": {
                "type": "SftpLocation",
                "folderPath": "/path/to/source/folder",
                "fileName": "source_file.csv"
            },
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n",
                "encodingName": "UTF-8"
            }
        }
    }
}




