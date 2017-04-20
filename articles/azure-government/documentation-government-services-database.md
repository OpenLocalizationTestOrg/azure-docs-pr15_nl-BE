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
    ms.date="10/18/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-databases"></a>Azure overheid Databases

##  <a name="sql-database"></a>SQL-Database

Zie de<a href="https://msdn.microsoft.com/en-us/library/bb510589.aspx"> Microsoft Security Center voor SQL-Database-Engine</a> en [Azure SQL Database openbare documentatie](https://azure.microsoft.com/documentation/services/sql-database/) voor extra ondersteuning voor metagegevens zichtbaarheid configuratie en aanbevolen procedures voor beveiliging.

### <a name="variations"></a>Variaties

V12-Database SQL is over het algemeen beschikbaar in Azure regering.

Het adres van de SQL-Servers Azure in Azure overheid verschilt:

Servicetype|Azure publiek|Azure overheid
---|---|---
SQL-Database|*. database.windows.net|*. database.usgovcloudapi.net

### <a name="considerations"></a>Overwegingen met betrekking tot

De volgende informatie geeft aan de rand van de overheid Azure voor Azure SQL:

| Geregeld/gecontroleerde gegevens toegestaan | Geregeld/gecontroleerde gegevens niet toegestaan |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alle gegevens opgeslagen en verwerkt in Microsoft Azure SQL kunnen Azure overheid geregeld gegevens bevatten. Hulpmiddelen voor databases moet u voor de gegevensoverdracht van Azure overheid geregeld gegevens gebruiken. | Azure SQL metagegevens mag niet exporteren gecontroleerde gegevens bevatten. Deze metagegevens omvatten alle configuratiegegevens hebt opgegeven bij het maken en onderhouden van uw opslagproduct.  Voer geen gegevens geregeld/geregeld in de volgende velden: Database naam, naam van abonnement, resourcegroepen, naam van de Server, Server admin login, implementatie namen, resourcenamen, bron codes

## <a name="azure-redis-cache"></a>Cache bestand Vgx Azure.

Zie voor meer informatie over deze service en het gebruik van het [openbare documentatie Azure bestand Vgx. Cache](https://azure.microsoft.com/documentation/services/redis-cache/).

### <a name="variations"></a>Variaties

De URL's voor toegang tot en beheer van Azure bestand Vgx. Cache in Azure overheid zijn verschillend:

Servicetype|Azure publiek|Azure overheid
---|---|---
Cache-eindpunt|*. redis.cache.windows.net|*. redis.cache.usgovcloudapi.net
Azure portal|https://Portal.Azure.com|https://Portal.Azure.us

>[AZURE.NOTE] Alle code en scripts moet rekening gehouden met eindpunten en omgevingen. Zie [verbinding maken met Azure overheid Cloud](../redis-cache/cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-azure-government-cloud-or-azure-china-cloud)voor meer informatie.


### <a name="considerations"></a>Overwegingen met betrekking tot

De volgende informatie geeft aan de rand van de overheid Azure voor Azure bestand Vgx. Cache:

| Geregeld/gecontroleerde gegevens toegestaan | Geregeld/gecontroleerde gegevens niet toegestaan |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alle gegevens opgeslagen en verwerkt in Azure bestand Vgx. Cache kunnen Azure overheid geregeld gegevens bevatten. | Azure bestand Vgx. metagegevens mag niet exporteren gecontroleerde gegevens bevatten. Voer geen gegevens geregeld/geregeld in de volgende velden: naam, VNET naam, naam van abonnement, resourcegroepen, codes van de bron, bestand Vgx. eigenschappen in de Cache.  

##  <a name="next-steps"></a>Volgende stappen

Voor aanvullende informatie en updates abonneren op de <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure overheid Blog.</a>
