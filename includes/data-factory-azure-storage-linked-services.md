## <a name="azure-storage-linked-service"></a>Azure Storage Service gekoppeld

De **opslag van Azure service gekoppeld** , kunt u een opslag Azure-account te koppelen aan een fabriek Azure gegevens met de **sleutel van de account**. Dit biedt de fabriek gegevens globale toegang naar de opslag van Azure. De volgende tabel bevat een beschrijving van JSON-elementen die specifiek zijn voor gekoppelde Azure-Storage-service.

| Eigenschap | Beschrijving | Vereist |
| :-------- | :----------- | :-------- |
| type | De eigenschap type moet worden ingesteld op: **AzureStorage** | Ja |
| connectionString | Geef de gegevens die nodig zijn voor de verbinding met Azure opslag voor de eigenschap connectionString. | Ja |

Raadpleeg het volgende artikel voor stapsgewijze instructies voor het weergeven of kopiëren van de sleutel van de account voor de opslag van een Azure: [weergeven, kopiëren en opnieuw genereren opslag toegangstoetsen](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

**Voorbeeld:**  
  
    {  
        "name": "StorageLinkedService",  
        "properties": {  
            "type": "AzureStorage",  
            "typeProperties": {  
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"  
            }  
        }  
    }  


## <a name="azure-storage-sas-linked-service"></a>Azure Storage, Sas Service gekoppeld  
Een gedeelde toegang handtekening (SAS) biedt een gedelegeerde toegang tot bronnen in uw account voor opslag. Dit betekent dat u een client beperkte machtigingen op objecten in uw account opslag gedurende een bepaalde tijd en met een opgegeven reeks machtigingen, zonder dat u voor het delen van uw account toegangstoetsen kunt verlenen. De SAS is een URI die in de query-parameters omvat alle gegevens die nodig zijn voor geverifieerde toegang tot een resource opslag. Als u opslagbronnen met de SAS, hoeft de client alleen in de SA's doorgeven aan de methode of de juiste constructor. Zie voor gedetailleerde informatie over SA's [gedeelde toegang handtekeningen: informatie over het Model SAS](../articles/storage/storage-dotnet-shared-access-signature-part-1.md)
  
De service Azure opslag SAS gekoppeld, kunt u een Azure opslag-Account te koppelen aan een fabriek Azure gegevens met behulp van een gedeelde Access handtekening (SAS). Dit biedt de fabriek gegevens beperkt/tijd-gebonden toegang tot alle/specifieke bronnen (blob/container) in de opslag. De volgende tabel bevat een beschrijving van JSON-elementen die specifiek zijn voor de service Azure opslag SAS gekoppeld. 

| Eigenschap | Beschrijving | Vereist |
| :-------- | :----------- | :-------- |
| type | De eigenschap type moet worden ingesteld op: **AzureStorageSas**  | Ja |
| sasUri | Gedeelde toegang handtekening URI op de Azure opslagmiddelen zoals blob, de recipiënt of de tabel opgeven. Zie de opmerkingen hieronder voor details. | Ja | 


**Voorbeeld:**
  
    {  
        "name": "StorageSasLinkedService",  
        "properties": {  
            "type": "AzureStorageSas",  
            "typeProperties": {  
                "sasUri": "<storageUri>?<sasToken>"   
            }  
        }  
    }  

Bij het maken van een **URI SAS**, de volgende:  

- Azure Data Factory ondersteunt alleen **Service SAS**, SAS voor niet-Account. Zie [Typen van gedeelde toegang handtekeningen](../articles/storage/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures) voor meer informatie over deze twee typen.
- Juiste lezen/schrijven- **machtigingen** moeten worden ingesteld voor objecten die op basis van hoe de gekoppelde service (lezen, schrijven, lezen/schrijven) in de fabriek van uw gegevens worden gebruikt.
- **Verlooptijd** moet correct worden ingesteld. Zorg ervoor dat de toegang tot objecten voor gegevensopslag Azure verloopt niet binnen de actieve periode van de pijpleiding.
- URI moet worden gemaakt op de juiste container/blob of tabel op basis van de noodzaak. Een Uri SAS om een Azure blob kan de Data Factory service toegang te krijgen tot die bepaalde blob. Een Uri SAS aan een container Azure blob kan de service Data Factory doorlopen BLOB's in die container. Als u nodig hebt voor toegang later meer/minder objecten bieden of de URI SAS bijwerken, moet u de gekoppelde service bijwerken met de nieuwe URI.   
