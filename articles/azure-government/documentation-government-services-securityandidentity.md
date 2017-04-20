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
    ms.date="10/12/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-security-and-identity"></a>Beveiliging op Azure en identiteit

##  <a name="key-vault"></a>Sleutel kluis
Zie voor meer informatie over deze service en het gebruik ervan, de <a href="https://azure.microsoft.com/documentation/services/key-vault">openbare documentatie Azure sleutel kluis.</a>

### <a name="data-considerations"></a>Overwegingen met betrekking tot gegevens
De volgende informatie geeft aan de rand van de overheid Azure voor Azure sleutel kluis:

| Geregeld/gecontroleerde gegevens toegestaan | Geregeld/gecontroleerde gegevens niet toegestaan |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alle gegevens versleuteld met een sleutel Azure sleutel kluis kan Regulated/gecontroleerde gegevens bevatten. | Azure sleutel kluis metagegevens mag niet exporteren gecontroleerde gegevens bevatten. Deze metagegevens omvatten alle configuratiegegevens hebt opgegeven bij het maken en onderhouden van uw sleutel kluis.  Voer geen gegevens Regulated/geregeld in de volgende velden: groep resourcenamen, sleutel kluis namen, de naam van abonnement |

Sleutel kluis is in Azure overheid in het algemeen beschikbaar. Als in het openbaar is er geen uitbreiding, sleutel kluis is alleen beschikbaar via PowerShell en CLI.

## <a name="next-steps"></a>Volgende stappen

Voor aanvullende informatie en updates abonneren op de <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure overheid Blog.</a>
