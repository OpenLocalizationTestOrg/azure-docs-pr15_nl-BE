<properties 
    pageTitle="Voorbeelden van NoSQL Python voor DocumentDB | Microsoft Azure" 
    description="Zoeken naar NoSQL Python voorbeelden op github voor veelvoorkomende taken in DocumentDB, met inbegrip van de CRUD-bewerkingen voor JSON-documenten in NoSQL databases." 
    keywords="Python-voorbeelden"
    services="documentdb" 
    authors="moderakh" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter="python"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/18/2016" 
    ms.author="moderakh"/>


# <a name="documentdb-python-examples"></a>Voorbeelden van DocumentDB Python

> [AZURE.SELECTOR]
- [Voorbeelden van .NET](documentdb-dotnet-samples.md)
- [Voorbeelden van node.js](documentdb-nodejs-samples.md)
- [Python-voorbeelden](documentdb-python-samples.md)
- [Galerie met voorbeelden van Code Azure](https://azure.microsoft.com/documentation/samples/?service=documentdb)

Monsteroplossingen die CRUD bewerkingen en andere algemene bewerkingen op Azure DocumentDB bronnen uitvoeren zijn opgenomen in de opslagplaats [azure-documentdb-python](https://github.com/Azure/azure-documentdb-python/tree/master/samples) GitHub. Dit artikel bevat:

- Koppelingen naar taken in elk van de bestanden Python voorbeeld project. 
- Koppelingen naar de bijbehorende API verwijzen inhoud.

**Vereisten**

1. U moet een account Azure Python voorbeelden gebruiken:
    - U kunt [gratis een Azure account openen](https://azure.microsoft.com/pricing/free-trial/): krijgt u credits kunt u uitproberen betaalde Azure services en zelfs nadat ze gebruikt omhoog kunt u de account en gebruik gratis Azure services, zoals Websites. Uw creditcard wordt nooit afgeschreven, tenzij u expliciet de instellingen wijzigen en ten laste van vragen.
   - U kunt [Visual Studio abonnee voordelen te activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/): Visual Studio uw abonnement geeft u credits elke maand die u voor betaalde Azure services gebruiken kunt.
2. U moet ook de [Python SDK](documentdb-sdk-python.md). 

    > [AZURE.NOTE] Elk monster wordt op zichzelf staand, stelt zichzelf en opgeschoond. De monsters geven als zodanig meerdere aanroepen van [document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html). Telkens wanneer dit uw abonnement gebeurt wordt gefactureerd voor 1 uur gebruik per laag van de prestaties van de collectie wordt gemaakt. 

## <a name="database-examples"></a>Database-voorbeelden

Het bestand [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement/Program.py) van het project [DatabaseManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement) ziet u hoe u de volgende taken uitvoeren.

Taak | API-Naslaggids
--- | ---
[Een database maken](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L65-L76) | [document_client. CreateDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Een account voor een database query](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L49-L62) | [document_client. QueryDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Een database met Id lezen](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L79-L96) | [document_client. ReadDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Lijst met databases voor een account](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L99-L110) | [document_client. ReadDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Een database verwijderen](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L113-L126) | [document_client. DeleteDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)

## <a name="collection-examples"></a>Verzameling voorbeelden 

Het bestand [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement/Program.py) van het project [CollectionManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement) ziet u hoe u de volgende taken uitvoeren.

Taak | API-Naslaggids
--- | ---
[Een collectie maken](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L84-L135) | [document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Lees een lijst van alle collecties in een database](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L198-L225) | [document_client. ListCollections](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Een collectie met een Id ophalen](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L178-L195) | [document_client. ReadCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Niveau van de prestaties van een collectie ophalen](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L139-L161) | [DocumentQueryable.QueryOffers](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Niveau van de prestaties van een collectie wijzigen](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L163-L175) | [document_client. ReplaceOffer](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Een verzameling verwijderen](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L212-L225) | [document_client. DeleteCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
