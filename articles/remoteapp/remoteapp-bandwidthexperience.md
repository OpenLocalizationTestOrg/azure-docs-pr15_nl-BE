<properties 
    pageTitle="Azure RemoteApp - hoe doet bandbreedte van het netwerk en de kwaliteit van zich werk samen? | Microsoft Azure"
    description="Informatie over welke gevolgen de netwerkbandbreedte in Azure RemoteApp-kwaliteit van de ervaring van de gebruiker."
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

# <a name="azure-remoteapp---how-do-network-bandwidth-and-quality-of-experience-work-together"></a>Azure RemoteApp - hoe doet bandbreedte van het netwerk en de kwaliteit van zich werk samen?

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Als u naar de [totale bandbreedte van het netwerk](remoteapp-bandwidth.md) voor RemoteApp Azure kijkt, moet u rekening houden met de volgende factoren - dit zijn allemaal deel uitmaken van een dynamisch systeem dat van invloed op de algehele gebruikservaring. 

- **Beschikbare netwerkbandbreedte en de huidige netwerkomstandigheden** - een set parameters (verlies, vertraging, jitter) op hetzelfde netwerk op een bepaald moment kan invloed hebben op de toepassing ervaring met streaming, wat betekent dat een verlaagde algemene gebruikerservaring. De bandbreedte die beschikbaar is in het netwerk is een functie van congestie, willekeurige verlies, vertraging omdat al deze parameters zijn van invloed op controlemechanisme congestie die op hun beurt besturingselementen de overdrachtssnelheid om te voorkomen dat conflicten.  Bijvoorbeeld brengt een netwerk met verlies of een netwerk met lange wachttijden de gebruiker onjuiste zelfs op een netwerk met 1000 MB bandbreedte. Het verlies en de wachttijden zijn afhankelijk van het aantal gebruikers die zich op hetzelfde netwerk en wat deze gebruikers doen (bijvoorbeeld video's bekijken, downloaden en uploaden van grote bestanden afdrukken).
- **Gebruiksscenario** - de ervaring is afhankelijk van wat de gebruikers als individuen en als een groep op hetzelfde netwerk doen. Lezen van een dia moet bijvoorbeeld één frame moet worden geactualiseerd; Als de gebruiker skims en over de inhoud van een tekstdocument schuift, moeten ze een hoger aantal frames per seconde worden bijgewerkt. De communicatie heen en weer naar de server in dit scenario verbruiken uiteindelijk meer netwerkbandbreedte. Houd ook rekening met een extreem voorbeeld: meerdere gebruikers zijn HD-video's (zoals 4K resolutie) bekijken, HD-vergaderingen houden, 3D-videogames spelen of CAD-systemen werkt. Deze kan zelfs een echt hoge bandbreedte netwerk vrijwel onbruikbaar maken.
- **Schermresolutie en het aantal schermen** - meer bandbreedte is vereist voor volledige update groter scherm dan de kleinere schermen. De onderliggende technologie is een vrij goed in te coderen en verzenden van alleen de regio's van de schermen die zijn bijgewerkt, maar eens aan staan, het hele scherm moet worden bijgewerkt. Wanneer de gebruiker heeft een hogere Resolutiescherm (bijvoorbeeld 4K resolutie), moet die update meer bandbreedte dan een scherm met een lagere resolutie (zoals 1024x768px). Deze dezelfde logica van toepassing als u meer dan één scherm voor omleiding. Bandbreedte moet toenemen met het aantal schermen.
- **Omleiding van het Klembord en apparaat** - dit is een probleem niet erg duidelijk, maar in veel gevallen als een gebruiker een grote hoeveelheid gegevens naar het Klembord, slaat het vergt nogal wat tijd voor die gegevens overbrengen van de extern bureaublad-client naar de server. De downstream-ervaring kan negatieve gevolgen hebben voor de ervaring van het verzenden van de inhoud van het Klembord stroomopwaarts. Hetzelfde geldt voor apparaatomleiding - als een scanner of webcam produceert een grote hoeveelheid gegevens die moeten worden verzonden naar de server upstream, of een printer voor het ontvangen van een groot document moet lokale opslag beschikbaar moet zijn voor een toepassing die wordt uitgevoerd in de cloud een groot bestand te kopiëren, gebruikers tegenkomen gedropte frames of tijdelijk "bevroren" video omdat de gegevens die nodig zijn voor het omleiden van het apparaat is verhoging van de bandbreedte van het netwerk moet. 

Bij de evaluatie van de behoeften voor uw netwerk bandbreedte, moet u rekening houden met al deze factoren die als een systeem.

Nu gaat u terug naar de [belangrijkste netwerk bandbreedte artikel](remoteapp-bandwidth.md)of gaat u verder met de [bandbreedte van het netwerk](remoteapp-bandwidthtests.md)testen.

## <a name="learn-more"></a>Meer informatie
- [Schatting verbruik van de netwerkbandbreedte Azure RemoteApp](remoteapp-bandwidth.md)

- [Azure RemoteApp - het verbruik van de netwerkbandbreedte met enkele algemene scenario's testen](remoteapp-bandwidthtests.md)

- [Netwerkbandbreedte Azure RemoteApp - algemene richtlijnen (als u niet zelf testen)](remoteapp-bandwidthguidelines.md)