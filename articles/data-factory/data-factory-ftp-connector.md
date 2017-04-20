<properties 
    pageTitle="Gegevens verplaatsen van de FTP-server | Microsoft Azure" 
    description="Informatie over het verplaatsen van gegevens vanaf een FTP-server met behulp van Azure Data Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/20/2016" 
    ms.author="spelluru"/>

# <a name="move-data-from-an-ftp-server-using-azure-data-factory"></a>Gegevens verplaatsen van een FTP-server met behulp van Azure Data Factory
Dit artikel geeft een overzicht van de activiteit kopiëren in Azure Data Factory gebruiken om gegevens te verplaatsen van een FTP-server naar een gegevensarchief ondersteunde sink. Dit artikel is gebaseerd op het artikel [gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) die activiteit kopiëren en de lijst van opgeslagen gegevens ondersteund als bronnen/putten een algemeen overzicht van de verplaatsing van gegevens biedt. 

Data factory ondersteunt momenteel alleen verplaatsen gegevens vanaf een FTP-server naar de andere opgeslagen gegevens, maar niet voor het verplaatsen van gegevens uit andere opgeslagen gegevens naar een FTP-server. Ondersteunt zowel op gebouwen en FTP-servers van de wolk. 

Als u gegevens van een **in de lokalen** van FTP-server naar een gegevensarchief wolk verplaatst (voorbeeld: Azure Blob Storage) installeren en Data Management Gateway. De Data Management Gateway is een client-agent die is geïnstalleerd op uw computer op gebouwen waarmee cloud services verbinding maken met de bron voor gebouwen. Zie [Data Management Gateway](data-factory-data-management-gateway.md) voor meer informatie over de gateway. Zie artikel [verplaatst gegevens tussen de locaties op gebouwen en wolken](data-factory-move-data-between-onprem-and-cloud.md) voor stapsgewijze instructies op de gateway instellen en gebruiken. De gateway kunt u verbinding maken met een FTP-server, zelfs als de server zich op een IaaS Azure VM (virtual machine). 

Als de FTP-server kunt u de gateway op de machine op locatie of de IaaS Azure VM installeren. We raden echter aan de gateway te installeren op een afzonderlijke computer of een aparte Azure IaaS VM bronconflicten voorkomen en betere prestaties. Wanneer u de gateway hebt geïnstalleerd op een afzonderlijke computer, wordt de computer moet toegang kunnen krijgen tot de FTP-server. 

## <a name="copy-data-wizard"></a>Wizard gegevens kopiëren
De gemakkelijkste manier voor het maken van een pijpleiding die gegevens worden gekopieerd vanaf een FTP-server is de wizard kopiëren gebruiken. Zie [Zelfstudie: maken van een pijplijn met behulp van de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met behulp van de wizard kopiëren. 

De volgende voorbeelden geven monster JSON definities die u gebruiken kunt voor het maken van een pijplijn met [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). 

## <a name="sample-copy-data-from-ftp-server-to-azure-blob"></a>Voorbeeld: Gegevens van FTP-server kopiëren naar Azure blob

In dit voorbeeld ziet u hoe gegevens van een FTP-server kopiëren naar een Azure Blob-opslag. Gegevens kunnen wel gekopieerde **rechtstreeks** naar een van de PUT vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) met behulp van de activiteit van de kopie in Azure Data Factory.  
 
Het monster heeft de volgende data factory entiteiten:

- Een gekoppelde service van het type [FtpServer](#ftp-linked-service-properties).
- Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Een invoer [dataset](data-factory-create-datasets.md) van het type [bestandsshare](#fileshare-dataset-type-properties).
- Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- Een [pijpleiding](data-factory-create-pipelines.md) met kopie activiteit die gebruikmaakt van [FileSystemSource](#ftp-copy-activity-type-properties) en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Het monster kopieert gegevens van een FTP-server naar een Azure blob elk uur. De JSON-eigenschappen in deze voorbeelden gebruikt worden in voorbeelden van de volgende secties beschreven. 

**FTP service gekoppeld** In dit voorbeeld gebruikt de basisverificatie met gebruikersnaam en wachtwoord in tekst zonder opmaak. U kunt ook een van de volgende manieren gebruiken: 

- Anonieme verificatie 
- Basisverificatie met gecodeerde referenties
- FTP via SSL/TLS (FTPS)

Zie [FTP service gekoppeld](#ftp-linked-service-properties) sectie voor verschillende soorten verificatie die u kunt gebruiken. 

    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",          
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
      }
    }

**Azure gekoppeld opslagservice**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**FTP-invoer dataset** Deze dataset verwijst naar de FTP-map `mysharedfolder` en `test.csv`. De pijpleiding wordt het bestand gekopieerd naar de bestemming. 

Instelling 'externe': 'true' informeert de Data Factory-service dat de dataset buiten de fabriek gegevens is en niet wordt geproduceerd door een activiteit in de fabriek van gegevens.
    
    {
      "name": "FTPFileInput",
      "properties": {
        "type": "FileShare",
        "linkedServiceName": "FTPLinkedService",
        "typeProperties": {
          "folderPath": "mysharedfolder",
          "fileName": "test.csv",
          "useBinaryTransfer": true
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }


**Azure Blob uitvoer dataset**

Gegevens worden weggeschreven naar een nieuwe blob elk uur (frequentie: uur, interval: 1). Het pad naar de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het pad naar de map wordt gebruikt voor jaar, maand, dag en uur delen van de begintijd.

    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/ftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "MM"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "dd"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**Pipeline-exemplaar activiteit**

De pijplijn bevat een kopie activiteit die is geconfigureerd voor het gebruik van de invoer en uitvoer datasets en elk uur is gepland. **Het type** is ingesteld op **FileSystemSource** in de pijplijn JSON definitie en **sink** -type is ingesteld op **BlobSink**. 
    
    {
        "name": "pipeline",
        "properties": {
            "activities": [{
                "name": "FTPToBlobCopy",
                "inputs": [{
                    "name": "FtpFileInput"
                }],
                "outputs": [{
                    "name": "AzureBlobOutput"
                }],
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "FileSystemSource"
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "00:05:00"
                }
            }],
            "start": "2016-08-24T18:00:00Z",
            "end": "2016-08-24T19:00:00Z"
        }
    }

## <a name="ftp-linked-service-properties"></a>Eigenschappen van FTP-Service gekoppelde

De volgende tabel bevat een beschrijving van JSON-elementen die specifiek zijn voor de FTP-service gekoppelde.

| Eigenschap | Beschrijving | Vereist | Standaard |
| -------- | ----------- | -------- | ------- | 
| type | De eigenschap type moet worden ingesteld op de FtpServer | Ja | &nbsp;
| host | Naam of IP-adres van de FTP-Server | Ja | &nbsp;
| authenticationType | Verificatietype opgeven | Ja | Basic, anoniem |
| gebruikersnaam | Gebruikers die toegang hebben tot de FTP-server | Nee | &nbsp;
| wachtwoord | Wachtwoord voor de gebruiker (gebruikersnaam) | Nee | &nbsp;
| encryptedCredential | Gecodeerde referenties toegang te krijgen tot de FTP-server | Nee | &nbsp;
| gatewayName | De naam van de Data Management Gateway-gateway verbinding kunnen maken met een FTP-server op locatie | Nee    | &nbsp;
| poort | Poort waarop de FTP-server luistert | Nee | 21 |
| enableSsl | Opgeven of het gebruik van FTP via SSL/TLS-kanaal | Nee | True | 
| enableServerCertificateValidation | Geef aan of de server SSL certificaat validatie inschakelen via FTP over SSL/TLS-kanaal | Nee | True | 

### <a name="using-anonymous-authentication"></a>Anonieme verificatie gebruikt

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {     
                "authenticationType": "Anonymous",
                "host": "myftpserver.com"
            }
        }
    }

### <a name="using-username-and-password-in-plain-text-for-basic-authentication"></a>Gebruikersnaam en wachtwoord in tekst zonder opmaak gebruikt voor basisverificatie
    
    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "username": "Admin",
                "password": "123456"
            }
        }
    }


### <a name="using-port-enablessl-enableservercertificatevalidation"></a>Met behulp van poort, enableSsl, enableServerCertificateValidation

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",  
                "username": "Admin",
                "password": "123456",
                "port": "21",
                "enableSsl": true,
                "enableServerCertificateValidation": true
            }
        }
    }

### <a name="using-encryptedcredential-for-authentication-and-gateway"></a>Met behulp van encryptedCredential voor verificatie en gateway

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "encryptedCredential": "xxxxxxxxxxxxxxxxx",
                "gatewayName": "mygateway"
            }
        }
    }

Zie [referenties voor instelling en beveiliging](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) voor meer informatie over het instellen van referenties voor een gegevensbron op ruimten FTP.

[AZURE.INCLUDE [data-factory-file-share-dataset](../../includes/data-factory-file-share-dataset.md)]   
[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="ftp-copy-activity-type-properties"></a>Eigenschappen van FTP-kopie activiteit

Zie het artikel [Pijpleidingen maken](data-factory-create-pipelines.md) voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. Eigenschappen van de naam, beschrijving, input en output-tabellen en beleidsregels zijn beschikbaar voor alle typen activiteiten. 

Eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren aan de andere kant met elk activiteitstype. Voor activiteit kopiëren variëren de eigenschappen afhankelijk van de typen bronnen en putten.

[AZURE.INCLUDE [data-factory-file-system-source](../../includes/data-factory-file-system-source.md)]   

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Prestatie en afstemming  
Zie de [uitvoering van de activiteit & Tuning Guide](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties gevolgen van verplaatsing van gegevens (kopiëren activiteit) in Azure Data Factory en verschillende manieren optimaliseren.

## <a name="next-steps"></a>Volgende stappen
Raadpleeg de volgende artikelen: 

- [Zelfstudie kopiëren activiteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een activiteit kopiëren. 
