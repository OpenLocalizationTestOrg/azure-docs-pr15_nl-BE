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
    ms.date="09/29/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-compute"></a>Azure overheid berekenen

##  <a name="virtual-machines"></a>Virtuele Machines

Zie voor meer informatie over deze service en het gebruik van deze [Formaten van Azure Virtual Machines](../virtual-machines/virtual-machines-windows-sizes.md).

### <a name="variations"></a>Variaties

De volgende VM SKU's zijn in Azure overheid algemeen beschikbaar (GA):

VM SKU|Amerikaanse beurs VA|Amerikaanse beurs IA|Notities
---|---|---|---
A|NH|NH|Geen
Dv1|NH|-|Geen
DSv1|NH|-|Geen
Dv2|NH|NH|15 is binnenkort verkrijgbaar
F|NH|NH|Geen
G|Gepland|-|Geen

###  <a name="data-considerations"></a>Overwegingen met betrekking tot gegevens

De volgende informatie geeft aan de rand van de overheid Azure voor Azure virtuele Machines:

| Geregeld/gecontroleerde gegevens toegestaan | Geregeld/gecontroleerde gegevens niet toegestaan |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Gegevens ingevoerd, opgeslagen en verwerkt in een VM geregeld exportgegevens kan bevatten. De binaire bestanden in Azure virtuele Machines uitgevoerd. Statische verificaties, zoals wachtwoorden en pincodes voor toegang tot de platformonderdelen Azure smartcard. Persoonlijke sleutels, certificaten gebruikt om platformonderdelen van Azure te beheren. SQL-verbindingsreeksen.  Andere beveiliging informatie/geheimen, zoals certificaten, coderingssleutels hoofdsleutels en opslag sleutels opgeslagen in Azure services.  | Metagegevens mag niet exporteren gecontroleerde gegevens bevatten. Deze metagegevens omvatten alle configuratiegegevens hebt opgegeven bij het maken en onderhouden van uw Azure Virtual Machine.  Voer geen gegevens Regulated/geregeld in de volgende velden: pachters rolnamen, resourcegroepen, implementatie namen, resourcenamen, bron codes  

## <a name="next-steps"></a>Volgende stappen

Voor aanvullende informatie en updates abonneren op de <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure overheid Blog.</a>
