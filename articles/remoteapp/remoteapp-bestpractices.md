<properties
    pageTitle="Azure RemoteApp-beste praktijken | Microsoft Azure"
    description="Aanbevolen procedures voor het configureren en gebruiken van Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="best-practices-for-configuring-and-using-azure-remoteapp"></a>Aanbevolen procedures voor het configureren en gebruiken van Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

De volgende informatie kunt u configureren en productief Azure RemoteApp gebruiken.

## <a name="connectivity"></a>Connectiviteit


- Gebruik altijd de meest recente clientversie. Met behulp van oudere clients kan leiden tot verbindingsproblemen en andere slechtere ervaringen. Inschakelen automatisch updates voor uw apparaat zal zorgen ervoor dat altijd de meest recente client is geïnstalleerd.
- Gebruik altijd de meest stabiele en betrouwbare internetverbinding beschikbaar.  
- Proxyverbindingen ondersteund gebruik alleen voor connectiviteit voor optimale prestaties.  De SOCKS-proxy wordt niet ondersteund.

## <a name="applications"></a>Toepassingen


- Opslaan en sluiten van RemoteApp-toepassingen als u klaar bent met de toepassing. De toepassing niet wordt gesloten, kan dit leiden tot gegevensverlies.
- Aangepaste toepassingen voordat u ze gebruikt in Azure RemoteApp valideren. Dit omvat het ervoor te zorgen dat ze werken op een multisessie-platform en niet onnodig bronnen zoals geheugen en CPU die een andere gebruiker in dezelfde collectie kan beroven van processortijd verbruiken. Download en bekijk de [Application Compatibility aanbevolen procedures voor extern bureaublad-Services](http://www.dabcc.com/resources/Application%20Compatibility%20Best%20Practices%20for%20Remote%20Desktop%20Services.pdf)voor informatie.

## <a name="configuration-and-management"></a>Configuratie en beheer


- De sjabloon-afbeeldingen up-to-date te houden, software-updates en andere essentiële fixes installeren wanneer dat nodig is. Op deze manier als Azure RemoteApp auto-schalen om te voldoen aan uw capaciteit, wordt elke instantie hersteld.  
- Zorg ervoor dat uw implementatie van Active Directory Federation Services (AD FS) is veilig en betrouwbaar. Anders mislukken client verificatie, te voorkomen dat gebruikers toegang krijgen tot RemoteApp Azure.
- Configureren met de geïnstalleerde toepassingen, functies of onderdelen sjabloon afbeeldingen zodanig dat ze ' stateless zijn '. Ze moeten niet blind op alle exemplaren van de virtuele machines in een RemoteApp-service worden in een permanente status.
    - Alle gebruikersgegevens opslaan in gebruikersprofielen of andere locaties buiten de service, zoals aandelen of OneDrive op gebouwen-bestand.
    - Gedeelde gegevens in opslaglocaties buiten de service, opslaan, zoals aandelen of OneDrive op gebouwen-bestand.
    - Algemene instellingen configureren in de Sjabloonafbeelding, in plaats van op afzonderlijke virtuele machines in een service.
    - Uitschakelen automatische software-updates voor uitgegeven toepassingen - in plaats daarvan handmatig toepassen op de Sjabloonafbeelding en te testen voordat u de sjabloon implementeert.
