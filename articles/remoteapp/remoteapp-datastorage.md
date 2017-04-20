
<properties
    pageTitle="Nooit gevoelige gegevens opslaan in aangepaste afbeeldingen voor Azure RemoteApp | Microsoft Azure"
    description="Informatie over de richtlijnen voor het opslaan van gegevens in aangepaste images in Azure RemoteApp"
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


# <a name="never-store-sensitive-data-on-custom-images"></a>Nooit vertrouwelijke gegevens op te slaan op een aangepaste images

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Wanneer u uw eigen toepassing in Azure RemoteApp-host, is de eerste stap voor het maken van een aangepaste installatiekopie. We gebruiken die aangepaste afbeelding VM instanties die voldoen aan uw apps voor uw gebruikers maken. De aangepaste afbeelding mag alleen toepassingen en nooit vertrouwelijke gegevens die verloren gaan, zoals SQL-databases, personeelsbestanden of speciale bestanden zoals QuickBooks bedrijfsbestanden bevatten. Alle vertrouwelijke gegevens moet zich bevinden op Azure RemoteApp extern op een bestandsserver, een andere Azure VM of in SQL Azure. De afbeelding moet de toepassing die verbinding maakt met de gegevensbron en worden de gegevens alleen host. Bekijk de [vereisten voor Azure RemoteApp afbeeldingen](remoteapp-imagereqs.md) voor meer informatie. 

Om te begrijpen waarom u geen gevoelige gegevens moet opslaan, moet u begrijpen hoe werkt RemoteApp Azure. Wanneer een collectie wordt gemaakt of bijgewerkt, achter de schermen meerdere klonen of exemplaren van de afbeelding worden gemaakt. Alle exemplaren van deze VM zijn exacte replica's van de afbeelding aangepast. Wanneer gebruikers toepassingen starten zijn ze verbonden met een van deze exemplaren VM. Maar dezelfde instantie is niet gegarandeerd en moet niet van belang omdat ze niet permanent zijn. De VM-exemplaren die als host fungeert voor de toepassingen zijn niet permanent en kunnen worden vernietigd of verwijderd op basis van, bijvoorbeeld tijdens het bijwerken van de collectie. 

Zodra de collectie is ingericht en gebruikers starten een verbinding met het VMs, gebruikersgegevens is permanent en beveiligd omdat het is opgeslagen op een aparte opslag binnen een VHD noemen we een [schijf van de gebruiker profiel (UPD)](remoteapp-upd.md), wordt het gebruikersprofiel in c:\users\<userprofile >. Wanneer een toepassing wordt gestart, wordt de UPD gemonteerd en beschouwd als een lokaal gebruikersprofiel door het besturingssysteem. Lees meer over hoe u [RemoteApp Azure gebruikersgegevens en -instellingen opgeslagen](remoteapp-upd.md).

Van de voorbeeldgegevens die niet in de afbeelding moet staan:

- Gedeelde gegevens van gebruikers voor toegang tot
- SQL DB of QuickBooks DB
- Alle gegevens in D:\

Van voorbeeldgegevens die kunnen worden ondergebracht in het profiel moeten worden gekopieerd naar alle gebruikers UPD:

- Configuratiebestanden per gebruiker
- Scripts die gebruikers nodig zouden hebben bewaard in hun UPD

Belangrijkste punten:

- Nooit vertrouwelijke gegevens op te slaan die gaan op de afbeelding verloren kunnen bij het maken van een aangepaste installatiekopie.
- Gevoelige gegevens moeten bevinden zich altijd op een afzonderlijke bestandsserver, afzonderlijke Azure VM op een wolk, en altijd buiten de VM-exemplaren die als host fungeert voor de Azure RemoteApp-toepassingen. 
- Gegevens van de gebruiker opgeslagen en blijft bestaan in de schijf van de gebruiker profiel (UPD)


