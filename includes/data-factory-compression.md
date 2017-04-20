### <a name="compression-support"></a>Compressieondersteuning  
Verwerken van grote gegevenssets veroorzaken I/O en netwerk knelpunten. Daarom kunnen gecomprimeerde gegevens in winkels niet alleen snellere overdracht van gegevens via het netwerk en schijfruimte besparen, maar ook brengen aanzienlijke prestatieverbeteringen in grote gegevens verwerken. Compressie wordt momenteel ondersteund voor winkels zoals Azure Blob of On-premises bestandssysteem gegevens op basis van een bestand.  

> [AZURE.NOTE] Compressie-instellingen worden niet ondersteund voor gegevens in de **AvroFormat**, **OrcFormat**of **ParquetFormat**. 

Compressie voor een dataset gebruikt u de eigenschap **compressie** in de dataset JSON zoals in het volgende voorbeeld:   

    {  
        "name": "AzureBlobDataSet",  
        "properties": {  
            "availability": {  
                "frequency": "Day",  
                "interval": 1  
            },  
            "type": "AzureBlob",  
            "linkedServiceName": "StorageLinkedService",  
            "typeProperties": {  
                "fileName": "pagecounts.csv.gz",  
                "folderPath": "compression/file/",  
                "compression": {  
                    "type": "GZip",  
                    "level": "Optimal"  
                }  
            }  
        }  
    }  
 
Het gedeelte **compressie** heeft twee eigenschappen:  
  
- **Type:** de compressiecodec **GZIP**, **Deflate** of **BZIP2**.  
- **Niveau:** de compressieratio die **optimale** of **snelst**kan zijn. 
    - **Snelste:** De bewerking compressie Voltooi zo spoedig mogelijk, zelfs als het bestand niet optimaal is gecomprimeerd. 
    - **Optimale**: de compressie-bewerking moet worden optimaal gecomprimeerd, zelfs als de bewerking een langere tijd in beslag neemt. 
    
    Zie [Compressieniveau](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) onderwerp voor meer informatie. 

Stel dat het bovenstaande voorbeeld dataset wordt gebruikt als de uitvoer van een activiteit kopiëren, de kopie de uitvoergegevens worden gecomprimeerd met GZIP-codec met optimale verhouding en vervolgens de gecomprimeerde gegevens schrijven naar een bestand met de naam pagecounts.csv.gz in de Azure Blob-opslag.   

Wanneer u de eigenschap compressie in een input-dataset JSON opgeeft, kunt de pijpleiding gecomprimeerde gegevens kan lezen uit de bron en wanneer u de eigenschap in een dataset uitvoer JSON opgeeft, de activiteit van de kopie schrijven gecomprimeerde gegevens naar de bestemming. Hier volgen enkele voorbeelden van scenario's: 

- Gegevens uit een Azure blob lezen GZIP gecomprimeerd decomprimeren en resultaatgegevens schrijven naar Azure SQL-database. Definieert u in dit geval de invoer dataset Azure Blob met de eigenschap JSON-compressie. 
- Gegevens lezen uit een bestand met tekst zonder opmaak van het bestandssysteem voor de lokalen en de gecomprimeerde gegevens schrijven naar een Azure blob comprimeren met GZip-indeling. Definieert u in dit geval een dataset uitvoer Azure Blob met de eigenschap JSON-compressie.  
- Een gecomprimeerd GZIP gegevens lezen uit een Azure blob, decomprimeren en gecomprimeerd met BZIP2 resultaatgegevens schrijven naar een Azure blob. Met compressie soort GZIP en de dataset uitvoer met compressietype ingesteld op BZIP2 in dit geval definieert u de invoer Azure Blob dataset.   
