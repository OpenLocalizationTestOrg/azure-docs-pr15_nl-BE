<properties 
   pageTitle="Adapter voor SharePoint StorSimple | Microsoft Azure"
   description="Beschrijving van het installeren en configureren of verwijderen van de Adapter StorSimple voor SharePoint in een SharePoint-serverfarm."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="07/11/2016"
   ms.author="v-sharos" />

# <a name="install-and-configure-the-storsimple-adapter-for-sharepoint"></a>Installeer en configureer de StorSimple Adapter voor SharePoint

## <a name="overview"></a>Overzicht

De StorSimple Adapter voor SharePoint is een onderdeel waarmee u Microsoft Azure StorSimple flexibele opslag en beveiliging van gegevens naar een SharePoint server-farms. U kunt de adapter binaire groot Object (BLOB) inhoud verplaatsen van de SQL Server-databases voor inhoud op Microsoft Azure StorSimple hybride cloud opslagapparaat.

De StorSimple Adapter voor SharePoint fungeert als een externe BLOB-opslag (RBS)-provider en de SQL-Server externe BLOB-opslag-functie gebruikt voor het opslaan van ongestructureerde SharePoint-inhoud (in de vorm van BLOB's) op een bestandsserver die wordt ondersteund door een StorSimple apparaat.

>[AZURE.NOTE] De StorSimple Adapter voor SharePoint ondersteunt SharePoint Server 2010 Remote BLOB Storage (RBS). SharePoint Server 2010 externe BLOB Storage (EBS) worden niet ondersteund.

- Download de StorSimple Adapter for SharePoint, Ga naar [StorSimple Adapter voor SharePoint] [ 1] in het Microsoft Download Center.

- Voor informatie over het plannen van de Resourcestructuur en RBS beperkingen, gaat u naar [Gebruik gebruik van RBS in SharePoint 2013] [ 2] of [plannen voor de Resourcestructuur (SharePoint Server 2010)][3].

De rest van dit overzicht wordt een korte beschrijving van de rol van de StorSimple Adapter voor SharePoint en SharePoint capaciteit en prestaties grenzen die u rekening houden met moet voordat u installeren en configureren van de adapter. Nadat u deze gegevens bekijken, gaat u naar [StorSimple Adapter voor de installatie van SharePoint](#storsimple-adapter-for-sharepoint-installation) om te beginnen met het instellen van de adapter.

### <a name="storsimple-adapter-for-sharepoint-benefits"></a>StorSimple Adapter voor de voordelen van SharePoint

Inhoud is opgeslagen in een SharePoint-site als ongestructureerde BLOB-gegevens in een of meer inhoudsdatabases. Standaard worden deze databases worden gehost op computers waarop SQL Server wordt uitgevoerd en bevinden zich in de serverfarm. BLOB's toenemen snel in grootte, verbruikt veel opslag voor gebouwen. Om deze reden is het raadzaam om een andere, minder opslagoplossing te vinden. SQL Server biedt een technologie voor externe Blob Storage (RBS) waarmee u de BLOB inhoud opslaan in het bestandssysteem buiten de SQL Server-database. BLOB's kunnen zich bevinden in het bestandssysteem op de computer waarop SQL Server wordt uitgevoerd met de Resourcestructuur, of ze kunnen worden opgeslagen in het bestandssysteem op een andere server-computer.

RBS vereist het gebruik van een provider Resourcestructuur, zoals de StorSimple Adapter voor SharePoint, opdat de Resourcestructuur in SharePoint. De StorSimple Adapter voor SharePoint werkt met RBS, zodat u BLOB's verplaatsen naar een server een back-up van het systeem Microsoft Azure StorSimple. Microsoft Azure StorSimple vervolgens slaat de BLOB-gegevens lokaal of in de cloud, op basis van gebruik. BLOB's die zeer actief zijn (meestal aangeduid als hot gegevens of Tier 1) staan lokaal. Minder actieve en archivering van gegevens bevinden zich in de cloud. Nadat u een inhoudsdatabase maken voor RBS, wordt alle nieuwe BLOB inhoud gemaakt in SharePoint opgeslagen op het apparaat StorSimple en niet in de inhoudsdatabase.

De toepassing Microsoft Azure StorSimple van RBS biedt de volgende voordelen:

- Bewegende BLOB inhoud op een afzonderlijke server, kunt u de query belasting van SQL Server, waarmee u kunt SQL Server reactievermogen verbeteren beperken. 

- Azure StorSimple gebruikt deduplication en compressie gegevens verkleinen.

- Azure StorSimple biedt bescherming van gegevens in de vorm van lokale en momentopnamen van de wolk. Ook als u de database zelf op de StorSimple apparaat plaatst, kunt u back-up de inhoudsdatabase en de BLOB's samen in een vastlopen consistente manier. (De database met inhoud te verplaatsen naar het apparaat wordt alleen ondersteund voor het apparaat StorSimple 8000-serie. Deze functie wordt niet ondersteund voor de 5000 of 7000-serie.)

- Azure StorSimple bevat disaster recovery functies inclusief failover-, bestands- en herstel (met inbegrip van de herstelprocedure) en snel herstel van gegevens.

- Data recovery-software, zoals Kroll Ontrack PowerControls, kunt u met StorSimple momentopnamen van BLOB-gegevens uit te voeren op itemniveau herstel van SharePoint-inhoud. (Deze data recovery software is een afzonderlijke inkoop).

- De StorSimple-Adapter voor SharePoint wordt in het Centraal beheer van SharePoint portal, zodat u de volledige SharePoint-oplossing te beheren vanaf een centrale locatie.

BLOB inhoud verplaatsen naar het bestandssysteem, kan andere kostenbesparingen en voordelen bieden. Bijvoorbeeld met behulp van de Resourcestructuur kunt verminderen de noodzaak voor dure Tier 1-opslag en, omdat de inhoudsdatabase wordt deze kleiner, Resourcestructuur kunt beperken het aantal databases dat vereist is in de SharePoint-serverfarm. Echter andere factoren, zoals de maximale grootte van database en het bedrag van de niet-RBS-inhoud kunnen ook van invloed op opslagvereisten. Zie voor meer informatie over de kosten en voordelen van het gebruik van RBS, [plannen voor de Resourcestructuur (SharePoint Foundation 2010)] [ 4] en [Gebruik gebruik van RBS in SharePoint 2013][5].

### <a name="capacity-and-performance-limits"></a>Maximale capaciteit en prestaties

Voordat u RBS in uw SharePoint-oplossing overwegen, moet u op de hoogte van de geteste prestaties en beperkingen van de capaciteit van SharePoint Server 2010 en SharePoint Server 2013 en hoe deze beperkingen hebben betrekking op acceptabel zijn. Zie voor meer informatie, [Software, grenzen en grenzen voor SharePoint 2013](https://technet.microsoft.com/library/cc262787.aspx).

Lees het volgende voordat u de Resourcestructuur configureren:

- Zorg ervoor dat de totale grootte van de inhoud (de grootte van een inhoudsdatabase) plus de grootte van de bijbehorende externalized BLOB's niet meer dan de maximale grootte van RBS ondersteund door SharePoint. Deze limiet is 200 GB. 

    **De inhoudsdatabase maatregel en blobgrootte**

     1. Deze query uitvoeren op de centrale administratie WFE. Start de SharePoint Management Shell en voert u de volgende Windows PowerShell-opdracht voor de grootte van de inhoudsdatabases:

        `Get-SPContentDatabase | Select-Object -ExpandProperty DiskSizeRequired`

         Deze stap wordt met deze eigenschap wordt de grootte van de database met inhoud op de schijf.

     2. Een van de volgende SQL-query's uitvoeren in SQL Management Studio op in het vak SQL server voor elke database met inhoud en het resultaat toevoegen aan het aantal verkregen in stap 1.

        Voer op inhoudsdatabases voor SharePoint 2013:

        `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[DocStreams] WHERE [Content] IS NULL`

        Voer op inhoudsdatabases voor SharePoint 2010:

        `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[AllDocs] WHERE [Content] IS NULL`

        Deze stap wordt de grootte van de BLOB's die externalized hebben zijn opgehaald.

- Wij raden aan dat u alle BLOB en database-inhoud lokaal opslaan op het apparaat StorSimple. Het apparaat StorSimple is een cluster met twee knooppunten voor hoge beschikbaarheid. Het plaatsen van de inhoudsdatabases en BLOB's op het apparaat StorSimple biedt een hoge beschikbaarheid.

    Met traditionele SQL Server migratie beste praktijken kunt u de inhoudsdatabase StorSimple apparaat. Verplaats de database pas nadat alle BLOB inhoud uit de database is verplaatst naar de bestandsshare via de Resourcestructuur. Als u de database met inhoud naar het apparaat StorSimple verplaatsen, wordt u aangeraden de inhoudsdatabase opslagruimte op het apparaat als een primaire partitie te configureren.

- In Microsoft Azure StorSimple, als met trapsgewijs geschakelde volumes, is er geen manier om te garanderen dat lokaal is opgeslagen op het apparaat zal niet worden doorverbonden naar Microsoft Azure cloud opslag StorSimple inhoud. Daarom is het raadzaam StorSimple lokaal vastgemaakt volumes gebruiken in combinatie met SharePoint RBS. Hierdoor worden alle BLOB inhoud lokaal op het apparaat StorSimple blijft en niet wordt verplaatst naar Microsoft Azure.

- Als u de inhoudsdatabases op de StorSimple-apparaat niet opslaan, gebruiken de traditionele SQL Server beschikbaarheid aanbevolen procedures die ondersteuning bieden voor Resourcestructuur. RBS clustering van SQL Server worden ondersteund, terwijl SQL Server mirroring niet. 

>[AZURE.WARNING] Als u de Resourcestructuur niet hebt ingeschakeld, wordt niet aanbevolen de inhoudsdatabase te verplaatsen naar de StorSimple-apparaat. Dit is een niet-geteste configuratie.
 
## <a name="storsimple-adapter-for-sharepoint-installation"></a>StorSimple Adapter voor de installatie van SharePoint

Voordat u de Adapter StorSimple voor SharePoint installeert, moet u de StorSimple apparaat configureren en zorg ervoor dat het starten van SQL Server en SharePoint-serverfarm voldoen aan alle vereisten. In deze zelfstudie wordt beschreven configuratievereisten, alsmede procedures voor het installeren en upgraden van de StorSimple Adapter voor SharePoint. 

## <a name="configure-prerequisites"></a>Vereisten configureren

Voordat u de StorSimple-Adapter voor SharePoint installeren kunt, ervoor dat de StorSimple apparaat, instantiëring van SQL Server en SharePoint-serverfarm voldoen aan de volgende vereisten.

### <a name="system-requirements"></a>Systeemvereisten

De StorSimple Adapter voor SharePoint werkt met de volgende hardware en software:

- Ondersteund besturingssysteem: Windows Server 2008 R2 SP1, Windows Server 2012 of Windows Server 2012 R2 

- Ondersteunde versies van SharePoint versies – SharePoint Server 2010 of SharePoint Server 2013

- Ondersteunde versies van SQL Server – SQL Server 2008, Enterprise Edition, SQL Server 2008 R2 Enterprise Edition of Enterprise Edition van SQL Server 2012

- Ondersteunde apparaten StorSimple – StorSimple 8000-serie, StorSimple 7000-serie of StorSimple 5000-reeks.

### <a name="storsimple-device-configuration-prerequisites"></a>StorSimple apparaat configuratie vereisten

Het apparaat StorSimple is een blok en als zodanig vereist een bestandsserver waarop de gegevens kunnen worden gehost. Wij raden aan dat u een afzonderlijke server in plaats van een bestaande server uit de SharePoint-farm gebruiken. Deze bestandsserver moet zich op hetzelfde lokale netwerk (LAN) als de SQL Server-computer waarop de inhoudsdatabases. 

>[AZURE.TIP] 
>
>- Als u uw SharePoint-farm voor maximale beschikbaarheid configureert, moet u ook de bestandsserver voor hoge beschikbaarheid implementeren.
>
>- Als u de database met inhoud niet bij de StorSimple-apparaat bewaren, gebruikt traditionele hoge beschikbaarheid aanbevolen procedures die ondersteuning bieden voor Resourcestructuur. RBS clustering van SQL Server worden ondersteund, terwijl SQL Server mirroring niet. 

Zorg ervoor dat het apparaat StorSimple correct is geconfigureerd en dat adequate volumes ter ondersteuning van uw SharePoint-implementatie worden geconfigureerd en is toegankelijk vanuit de SQL Server-computer. Ga naar [uw apparaat op gebouwen StorSimple implementeren](storsimple-deployment-walkthrough.md) als u nog niet hebt geïmplementeerd en de StorSimple apparaat geconfigureerd. Noteer het IP-adres van het apparaat StorSimple; u moet deze tijdens de StorSimple Adapter voor de installatie van SharePoint. 

Zorg bovendien dat het volume moet worden gebruikt voor BLOB externalization voldoet aan de volgende eisen voldoen:

- Het volume moet worden geformatteerd met een clustergrootte van 64 KB.

- Uw web-front-end (WFE) en toepassingsservers moet toegang kunnen krijgen tot het volume via een pad (Universal Naming Convention). 

- De SharePoint server-farm moet worden geconfigureerd om te schrijven naar het volume.

>[AZURE.NOTE] Na het installeren en configureren van de adapter alle BLOB-externalization moet gaan via de StorSimple-apparaat (het apparaat zal de volumes voor SQL Server en de opslag lagen beheren). Kunt u andere doelen voor BLOB-externalization.
 
Als u StorSimple Snapshot Manager om momentopnamen van de database en BLOB-gegevens te gebruiken wilt, moet u StorSimple Snapshot Manager installeren op de databaseserver, zodat deze de Service SQL Writer gebruiken kunt voor het implementeren van Windows Volume Shadow Copy Service (VSS). 

>[AZURE.IMPORTANT] StorSimple Snapshot Manager biedt geen ondersteuning voor de VSS-schrijver van SharePoint en consistente toepassing momentopnamen kunnen niet van de SharePoint-gegevens. In een scenario met SharePoint biedt StorSimple Snapshot Manager alleen crash consistent back-ups. 
 
## <a name="sharepoint-farm-configuration-prerequisites"></a>Vereisten voor SharePoint-farm configureren

Zorg ervoor dat uw SharePoint-serverfarm correct is geconfigureerd, als volgt:

- Controleer of uw SharePoint-serverfarm in orde zijn en controleert u het volgende: 

- Alle SharePoint WFE en toepassingsservers die zijn geregistreerd in de farm worden uitgevoerd en kunnen worden gepingd vanaf de server waarop u de Adapter StorSimple voor SharePoint installeert.

- De SharePoint-timerservice (SPTimerV3 of SPTimerV4) wordt uitgevoerd op elke server WFE en de application server.

- Zowel de SharePoint-timerservice en de IIS-toepassingen waaronder de site voor Centraal beheer van SharePoint wordt uitgevoerd hebben beheerdersrechten. 

- Zorg ervoor dat Internet Explorer Enhanced Security Context (verbeterde) is uitgeschakeld. Volg deze stappen om te schakelen van de verbeterde beveiliging:

    1. Sluit alle exemplaren van Internet Explorer.

    2. Start de Server Manager.

    3. Klik op de **Lokale Server**in het linkerdeelvenster.

    4. Klik in het rechterdeelvenster, naast **Verbeterde beveiliging van Internet Explorer**, **op**.

    5. **Administrators**, klik op **uitschakelen**.

    6. Klik op **OK**.

## <a name="remote-blob-storage-rbs-prerequisites"></a>Vereisten voor Remote BLOB Storage (RBS)

Let erop dat u een ondersteunde versie van SQL Server. Alleen de volgende versies worden ondersteund en gebruikt dit veld kan:

- SQL Server 2008, Enterprise Edition

- SQL Server 2008 R2 Enterprise Edition

- Enterprise Edition van SQL Server 2012

BLOB's kunnen worden externalized voor volumes die het apparaat StorSimple SQL Server. Geen andere doelen voor BLOB-externalization worden ondersteund.

Wanneer u alle vereiste stappen hebt uitgevoerd, gaat u naar [de StorSimple Adapter voor SharePoint installeren](#install-the-storsimple-adapter-for-sharepoint).

## <a name="install-the-storsimple-adapter-for-sharepoint"></a>De StorSimple-Adapter installeren voor SharePoint

Gebruik de volgende stappen de StorSimple-Adapter installeren voor SharePoint. Als u de software opnieuw installeert, raadpleegt u [bijwerken of opnieuw installeren van de Adapter StorSimple voor SharePoint](#upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint). De tijd die nodig is voor de installatie is afhankelijk van het totale aantal SharePoint-databases in uw SharePoint-serverfarm.

[AZURE.INCLUDE [storsimple-install-sharepoint-adapter](../../includes/storsimple-install-sharepoint-adapter.md)]


## <a name="configure-rbs"></a>RBS configureren

Na installatie van de Adapter StorSimple voor SharePoint configureren Resourcestructuur zoals beschreven in de volgende procedure.

>[AZURE.TIP] De StorSimple Adapter voor SharePoint kunt aansluiten op de pagina Centraal beheer van SharePoint, waardoor RBS worden ingeschakeld of uitgeschakeld voor elke database met inhoud in de SharePoint-farm. In- of uitschakelen van RBS in de inhoudsdatabase wordt echter een reset van IIS, die, afhankelijk van uw farmconfiguratie tijdelijk de beschikbaarheid van de SharePoint web-front-end (WFE verstoren kan). (Factoren zoals het gebruik van een front-end-taakverdeling, de huidige werklast van de server, enzovoort kunnen beperken of deze onderbrekingen te voorkomen.) Als u wilt voorkomen dat gebruikers een onderbreking, het is raadzaam dat u in- of Resourcestructuur alleen tijdens een gepland onderhoudsvenster uitschakelen.

[AZURE.INCLUDE [storsimple-sharepoint-adapter-configure-rbs](../../includes/storsimple-sharepoint-adapter-configure-rbs.md)]
 

## <a name="configure-garbage-collection"></a>Configureer de garbage collector

Wanneer u objecten uit een SharePoint-site worden verwijderd, worden ze niet automatisch uit de Resourcestructuur store-volume verwijderd. In plaats daarvan een asynchrone, achtergrond onderhoud wordt verwijderd zwevende BLOB's uit het bestandsarchief. Systeembeheerders kunnen dit proces uit te voeren periodiek plannen of zij deze indien nodig kunnen starten.

Dit onderhoudsprogramma (Microsoft.Data.SqlRemoteBlobs.Maintainer.exe) wordt automatisch geïnstalleerd op alle SharePoint WFE webservers en toepassingsservers wanneer u Resourcestructuur. Het programma is geïnstalleerd op de volgende locatie: *opstartstation*: externe SQL-blobopslag-10.50\Maintainer\ \Program Files\Microsoft

Voor meer informatie over de configuratie en het gebruik van het onderhoudsprogramma [Resourcestructuur onderhouden in SharePoint Server 2013]Zie[8].

>[AZURE.IMPORTANT] Het programma Resourcestructuur maintainer is veel bronnen. U moet het alleen tijdens perioden van lichte activiteit wordt uitgevoerd op de SharePoint-farm te plannen.

### <a name="delete-orphaned-blobs-immediately"></a>Zwevende BLOB's onmiddellijk te verwijderen

Als u zwevende BLOB's onmiddellijk te verwijderen, kunt u de volgende instructies. Houd er rekening mee dat deze instructies een voorbeeld zijn van hoe dit kan worden gedaan in een omgeving met SharePoint 2013 met de volgende onderdelen:

- De naam van de inhoudsdatabase is WSS_Content.
- De SQL Server-naam is SHRPT13 SQL12\SHRPT13.
- De naam van de webtoepassing is SharePoint-80.

[AZURE.INCLUDE [storsimple-sharepoint-adapter-garbage-collection](../../includes/storsimple-sharepoint-adapter-garbage-collection.md)]


## <a name="upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint"></a>Upgraden of installeer de Adapter StorSimple voor SharePoint

Met de volgende procedure kunt u SharePoint server upgrade en installeer StorSimple Adapter voor SharePoint of gewoon upgraden of installeer de adapter in een bestaande SharePoint-serverfarm. 

>[AZURE.IMPORTANT] Lees de volgende informatie voordat u upgrade uitvoert van uw SharePoint-software en/of een upgrade of de StorSimple-Adapter installeren voor SharePoint:
>
>- Alle bestanden die eerder zijn verplaatst naar de externe opslag via de Resourcestructuur niet mogelijk totdat de installatie is voltooid en de Resourcestructuur functie opnieuw ingeschakeld. Alleen de impact van de gebruiker uitvoeren een upgrade of herinstallatie tijdens een geplande onderhoudsvenster.
>
>- De tijd die nodig is voor de upgrade/installatie kan variëren, afhankelijk van het totale aantal SharePoint-databases in de serverfarm.
>
>- Nadat de upgrade/installatie voltooid is, moet u de Resourcestructuur inschakelen voor de inhoudsdatabases. Zie [Resourcestructuur configureren](#configure-rbs) voor meer informatie.
>
>- Als u de Resourcestructuur voor een SharePoint-farm met een zeer groot aantal databases (meer dan 200) configureert, kan de pagina **Centraal beheer van SharePoint** -time-out. Als dat gebeurt, kunt u de pagina vernieuwen. Dit heeft geen invloed op het configuratieproces.

[AZURE.INCLUDE [storsimple-upgrade-sharepoint-adapter](../../includes/storsimple-upgrade-sharepoint-adapter.md)]
 
## <a name="storsimple-adapter-for-sharepoint-removal"></a>StorSimple Adapter voor SharePoint verwijderen

De volgende procedures wordt beschreven hoe u de BLOB's terug naar de inhoud SQL Server-databases en verwijdert u de Adapter StorSimple voor SharePoint. 

>[AZURE.IMPORTANT] U hebt de BLOB's om terug te gaan naar de inhoudsdatabases voordat u de adaptersoftware verwijderen. 

### <a name="before-you-begin"></a>Voordat u begint 

Verzamel de volgende gegevens voordat u de gegevens terug naar de SQL Server-databases voor inhoud en beginnen met het verwijderingsproces adapter:

- De namen van alle databases waarvoor Resourcestructuur is ingeschakeld.
- Het UNC-pad van de geconfigureerde BLOB-opslag

### <a name="move-the-blobs-back-to-the-content-databases"></a>De BLOB's terug naar de inhoudsdatabases

Voordat u de Adapter StorSimple voor SharePoint-software verwijdert, moet u alle van de BLOB's die zijn externalized terug naar de SQL Server-databases voor inhoud migreren. Als u probeert de StorSimple Adapter voor SharePoint verwijderen voordat u de BLOB's terug naar de inhoudsdatabases, verschijnt de volgende waarschuwing weergegeven.

![Waarschuwingsbericht](./media/storsimple-adapter-for-sharepoint/sasp1.png)
 
####<a name="to-move-the-blobs-back-to-the-content-databases"></a>De BLOB's om terug te gaan naar de inhoudsdatabases 

1. Downloaden van de externalized objecten.

2. Open de pagina **Centraal beheer van SharePoint** en Ga naar **Systeeminstellingen**. 

3. Klik onder **Azure StorSimple**, **StorSimple-Adapter configureren**.

4. Klik op de knop **uitschakelen** onder elk van de inhoudsdatabase die u wilt verwijderen uit de externe BLOB-opslag op de pagina **StorSimple Adapter configureren** . 

5. De objecten van SharePoint verwijderen en vervolgens opnieuw uploaden.

Ook kunt u de Microsoft` RBS Migrate()` opgenomen met SharePoint PowerShell-cmdlet. Zie voor meer informatie, [inhoud migreren naar of uit de Resourcestructuur](https://technet.microsoft.com/library/ff628255.aspx).

Nadat u de BLOB's terug naar de database met inhoud verplaatst, gaat u naar de volgende stap: [verwijderen van de adapter](#uninstall-the-adapter).

### <a name="uninstall-the-adapter"></a>De adapter verwijderen

Nadat u de BLOB's terug naar de inhoudsdatabases voor SQL Server verplaatst, gebruikt u een van de volgende opties de StorSimple Adapter voor SharePoint verwijderen.

#### <a name="to-use-the-installation-program-to-uninstall-the-adapter"></a>Gebruik het installatieprogramma voor het verwijderen van de adapter 

1. Gebruik een account met administrator-bevoegdheden voor aanmelding bij de server front-end (WFE).
2. Dubbelklik op de StorSimple Adapter voor SharePoint installer. De installatiewizard wordt gestart.

    ![Wizard Setup](./media/storsimple-adapter-for-sharepoint/sasp2.png)

3. Klik op **volgende**. De volgende pagina wordt weergegeven.

    ![Setup-pagina van de wizard verwijderen](./media/storsimple-adapter-for-sharepoint/sasp3.png)

4. Klik op **verwijderen** om het verwijderingsproces te selecteren. De volgende pagina wordt weergegeven.

    ![Setup-wizard bevestigingspagina](./media/storsimple-adapter-for-sharepoint/sasp4.png)

5. Klik op **verwijderen** om het verwijderen te bevestigen. De volgende pagina van de voortgang wordt weergegeven.

    ![Voortgang wizardpagina](./media/storsimple-adapter-for-sharepoint/sasp5.png)

6. Wanneer het verwijderen voltooid is, verschijnt de voltooiingspagina. Klik op **Voltooien** om de Setup Wizard te sluiten.

#### <a name="to-use-the-control-panel-to-uninstall-the-adapter"></a>Het Configuratiescherm gebruiken voor het verwijderen van de adapter 

1. Open het Configuratiescherm en klik vervolgens op **programma's en onderdelen**.

2. **Adapter voor SharePoint StorSimple**selecteren en klik vervolgens op **verwijderen**. 

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over StorSimple](storsimple-overview.md).

<!--Reference links-->
[1]: https://www.microsoft.com/download/details.aspx?id=44073
[2]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[3]: https://technet.microsoft.com/library/ff628583(v=office.14).aspx
[4]: https://technet.microsoft.com/library/ff628569(v=office.14).aspx
[5]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[8]: https://technet.microsoft.com/en-us/library/ff943565.aspx
