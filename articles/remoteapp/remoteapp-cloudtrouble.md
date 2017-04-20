
<properties
    pageTitle="Problemen oplossen met RemoteApp-wolk collecties - maken | Microsoft Azure"
    description="Informatie over hoe u problemen met RemoteApp-wolk collectie maken"
    services="remoteapp"
    documentationCenter=""
    authors="vkbucha"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="troubleshoot-creating-remoteapp-cloud-collections"></a>Problemen oplossen met maken RemoteApp-wolk collecties

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Als u hebt met het maken van een collectie wolk problemen, Controleer de volgende gegevens.

## <a name="your-image-is-invalid"></a>Uw afbeelding is ongeldig ##
Als u een bericht zoals 'GoldImageInvalid' wanneer u wacht Azure uw collectie inrichten, betekent dit dat uw Sjabloonafbeelding voldoet niet aan de [vereisten image gedefinieerd](remoteapp-imagereqs.md). Dus ga lezen deze [vereisten](remoteapp-imagereqs.md), uw image te herstellen en maakt uw verzameling opnieuw.

## <a name="common-errors-seen-in-the-azure-management-portal"></a>Veelvoorkomende fouten weergegeven in de portal Management Azure

    DNS server could not be reached
    ProvisioningTimeout

Vaak wolk-collecties mislukt tijdens het maken van door u aangepaste installatiekopieën gebruikt.  Als u met behulp van een aangepaste afbeelding maken van de collectie ziet u een van de bovenstaande fouten optreden, controleert u de volgende zaken:

- Zorg ervoor dat de aangepaste installatiekopie die u hebt geüpload aan de afbeelding voldoet.
- Meest is voorkomende probleem dat de afbeelding niet correct syspreped is.  
- Controleer of de afbeelding kunt opstarten in Hyper-V of probeer een VM IAAS rechtstreeks in uw Azure abonnement met behulp van de afbeelding te maken. Als de VM niet opstarten en niet wordt gestart, wordt hier meestal dat de aangepaste installatiekopie niet goed is voorbereid.  Controleer of de aangepaste afbeelding is gebouwd volgens de procedure voor het maken van een afbeelding aangepaste sjabloon voor RemoteApp

Als u een van de Microsoft-afbeeldingen die deel uitmaken van uw abonnement, probeert u opnieuw de collectie maken. Als het probleem zich blijft voordoen vervolgens Neem contact op met Microsoft support.

    PlatformImageTrialModeOnly

Als u deze fout betekent dit meestal dat u naar een betaalde account is bijgewerkt, maar u probeert een afbeelding door Microsoft geleverd die geldig is tijdens de proefmodus van de service alleen te gebruiken. In dit geval proberen te maken uw cloud-collectie opnieuw, maar zorg ervoor dat u de juiste afbeelding opgeven.
