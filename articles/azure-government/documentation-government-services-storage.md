<properties
    pageTitle="Azure overheid documentatie | Microsoft Azure"
    description="Dit zorgt voor een vergelijking van functies en hulp op het ontwikkelen van toepassingen voor de overheid Azure"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/13/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-storage"></a>Azure overheid opslag

##  <a name="azure-storage"></a>Azure opslag

Zie [openbare opslag Azure-documentatie](https://azure.microsoft.com/documentation/services/storage/)voor meer informatie over deze service en het gebruik ervan.

### <a name="variations"></a>Variaties

De URL's voor opslag rekeningen in Azure overheid zijn verschillend:

Servicetype|Azure publiek|Azure overheid
---|---|---
BLOB-opslag|*. blob.core.windows.net|*. blob.core.usgovcloudapi.net
Queue Storage|*. queue.core.windows.net|*. queue.core.usgovcloudapi.net
Tabelopslag|*. table.core.windows.net| *. table.core.usgovcloudapi.net

>[AZURE.NOTE] Alle code en scripts moet rekening gehouden met de eindpunten.  Zie [opslag Azure verbindingsreeksen configureren](../storage-configure-connection-string.md#creating-a-connection-string-to-the-explicit-storage-endpoint). 

Zie voor meer informatie over API's de <a href="https://msdn.microsoft.com/en-us/library/azure/mt616540.aspx">Cloud opslag Account Constructor</a>.

Het achtervoegsel eindpunt te gebruiken in deze overbelastingen is core.usgovcloudapi.net 

### <a name="considerations"></a>Overwegingen met betrekking tot

De volgende informatie geeft aan de rand van de overheid Azure voor Azure opslag:

| Geregeld/gecontroleerde gegevens toegestaan | Geregeld/gecontroleerde gegevens niet toegestaan |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Gegevens die zijn ingevoerd, opgeslagen en verwerkt in een opslag Azure product exporteren gecontroleerde gegevens kan bevatten. Statische verificaties, zoals wachtwoorden en pincodes voor toegang tot de platformonderdelen Azure smartcard. Persoonlijke sleutels, certificaten gebruikt om platformonderdelen van Azure te beheren. Andere beveiliging informatie/geheimen, zoals certificaten, coderingssleutels hoofdsleutels en opslag sleutels opgeslagen in Azure services. | Azure opslag metagegevens mag niet exporteren gecontroleerde gegevens bevatten. Deze metagegevens omvatten alle configuratiegegevens hebt opgegeven bij het maken en onderhouden van uw opslagproduct.  Voer geen gegevens Regulated/geregeld in de volgende velden: resourcegroepen, namen van implementatie, resourcenamen, bron codes  

##  <a name="premium-storage"></a>Premium-opslagruimte

Zie voor meer informatie over deze service en het gebruik van het [premie opslag: krachtige opslag voor een standaardwerkbelasting Azure Virtual Machine](../storage/storage-premium-storage.md).

###  <a name="variations"></a>Variaties

Premium-opslag is in het algemeen beschikbaar zijn in de USGov Virginia. Dit omvat de DS serie virtuele Machines. 

### <a name="considerations"></a>Overwegingen met betrekking tot

Opslag gegevens gelden dezelfde overwegingen als bovenstaand premium opslag rekeningen. 

##  <a name="next-steps"></a>Volgende stappen

Voor aanvullende informatie en updates abonneren op de <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure overheid Blog.</a>
