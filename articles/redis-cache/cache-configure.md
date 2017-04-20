<properties 
    pageTitle="Het configureren van Azure bestand Vgx. Cache | Microsoft Azure"
    description="De standaardconfiguratie bestand Vgx. voor Azure bestand Vgx. Cache begrijpen en informatie over het configureren van uw sessies Azure-Cache bestand Vgx."
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="douge"
    editor="tysonn" />
<tags 
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/25/2016"
    ms.author="sdanie" />

# <a name="how-to-configure-azure-redis-cache"></a>Het configureren van Azure-Cache bestand Vgx.

In dit onderwerp wordt beschreven hoe u bekijken en bijwerken van de configuratie van uw sessies Azure bestand Vgx. Cache en omvat standaard bestand Vgx. configuratie van de server voor Azure bestand Vgx. Cache exemplaren.

>[AZURE.NOTE] Zie voor meer informatie over het configureren en gebruiken van premium cache-functies, [persistentie voor een Premium Azure bestand Vgx. Cache configureren](cache-how-to-premium-persistence.md), [configureren voor een Premium Azure bestand Vgx. Cache clustering](cache-how-to-premium-clustering.md)en [het configureren van virtuele netwerk ondersteuning voor een Premium Azure bestand Vgx. Cache](cache-how-to-premium-vnet.md).

## <a name="configure-redis-cache-settings"></a>Bestand Vgx. cache-instellingen configureren

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Azure bestand Vgx. Cache bevat de volgende instellingen op het blad van de **Instellingen** .

![Cache-instellingen bestand Vgx.](./media/cache-configure/redis-cache-settings.png)



-   [Ondersteuning en probleemoplossing van de instellingen](#support-amp-troubleshooting-settings)
-   [Algemene instellingen](#general-settings)
    -   [Eigenschappen](#properties)
    -   [Toegangstoetsen](#access-keys)
    -   [Geavanceerde instellingen](#advanced-settings)
    -   [Advisor Cache bestand Vgx.](#redis-cache-advisor)
-   [Schaalinstellingen voor](#scale-settings)
    -   [Prijzen laag](#pricing-tier)
    -   [Clustergrootte bestand Vgx.](#cluster-size)
-   [Instellingen voor het beheer van gegevens](#data-management-settings)
    -   [Bestand Vgx persistentie van gegevens.](#redis-data-persistence)
    -   [Importeren/exporteren](#importexport)
-   [Beheerinstellingen](#administration-settings)
    -   [Opnieuw opstarten](#reboot)
    -   [Updates plannen](#schedule-updates)
-   [Diagnostische instellingen](#diagnostics-settings)
-   [Netwerkinstellingen](#network-settings)
-   [De instellingen voor bronnen](#resource-management-settings)

## <a name="support--troubleshooting-settings"></a>Ondersteuning en probleemoplossing van de instellingen

De instellingen in de sectie **Support + het oplossen van problemen** bieden u opties voor het oplossen van problemen met uw cache.

![Ondersteuning + oplossen](./media/cache-configure/redis-cache-support-troubleshooting.png)

Klik op **diagnosticeren en oplossen van problemen** te verstrekken met gemeenschappelijke problemen en strategieën voor het oplossen ervan.

Klik op **logboek voor faxactiviteit** om acties in de cache weer te geven. U kunt ook filteren gebruiken om deze weergave wilt opnemen, andere bronnen weer te geven. Zie voor meer informatie over het gebruik van controlelogboeken [weergeven gebeurtenissen en logboeken controleren](../monitoring-and-diagnostics/insights-debugging-with-events.md) en [bewerkingen met Resource Manager controleren](../resource-group-audit.md). Zie voor meer informatie over het controleren van gebeurtenissen Azure bestand Vgx. Cache [bewerkingen en waarschuwingen](cache-how-to-monitor.md#operations-and-alerts).

**Resource gezondheid** horloges van de resource en kunt u zien als deze wordt uitgevoerd zoals verwacht. Voor meer informatie over de Resource Azure health service overzicht [Azure Resource gezondheid](../resource-health/resource-health-overview.md).

>[AZURE.NOTE] De gezondheid van de resource kan momenteel om te rapporteren over de gezondheid van Azure bestand Vgx. Cache-exemplaren die worden gehost in een virtueel netwerk. Zie voor meer informatie [alle cache-functies werken als host van een cache in een VNET?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)

Klik op **nieuw ondersteuningsverzoek** als u wilt openen een verzoek om ondersteuning voor de cache.

## <a name="general-settings"></a>Algemene instellingen

De instellingen in de sectie **Algemeen** kunt u openen en de volgende instellingen voor de cache configureren.

![Algemene instellingen](./media/cache-configure/redis-cache-general-settings.png)

-   [Eigenschappen](#properties)
-   [Toegangstoetsen](#access-keys)
-   [Geavanceerde instellingen](#advanced-settings)
-   [Advisor Cache bestand Vgx.](#redis-cache-advisor)

### <a name="properties"></a>Eigenschappen

Klik op **Eigenschappen** om informatie over uw cache, met inbegrip van het eindpunt van de cache en de poorten weer te geven.

![Eigenschappen in Cache bestand Vgx.](./media/cache-configure/redis-cache-properties.png)

### <a name="access-keys"></a>Toegangstoetsen

Klik op **Access-toetsen** om te bekijken of de toegangstoetsen voor de cache genereren. Deze sleutels worden gebruikt en de host-naam en de **Eigenschappen** blade-poorten door de clients verbinding maken met uw cache.

![Toegangstoetsen Cache bestand Vgx.](./media/cache-configure/redis-cache-manage-keys.png)






### <a name="advanced-settings"></a>Geavanceerde instellingen

Op het blad **Geavanceerde instellingen** zijn de volgende instellingen configureren.

-   [-Poorten](#access-ports)
-   [Beleid voor Maxmemory en maxmemory-gereserveerd](#maxmemory-policy-and-maxmemory-reserved)
-   [Keyspace meldingen (geavanceerde instellingen)](#keyspace-notifications-advanced-settings)


### <a name="access-ports"></a>-Poorten

Niet-SSL-toegang is standaard uitgeschakeld voor nieuwe caches. Als u wilt dat de niet-SSL-poort, klikt u op **Nee** voor **toegang via SSL alleen toestaan** op de **blade geavanceerde instellingen** en klik op **Opslaan**.

![Cache-poorten bestand Vgx.](./media/cache-configure/redis-cache-access-ports.png)

### <a name="maxmemory-policy-and-maxmemory-reserved"></a>Beleid voor Maxmemory en maxmemory-gereserveerd

De instellingen **beleid voor Maxmemory** en **maxmemory gereserveerd** op de bladeserver **Geavanceerde instellingen** configureren voor de cache in het geheugen-beleid. De **maxmemory -** instelling configureert u het beleid voor verwijdering voor de cache en het geheugen gereserveerd voor niet-cache processen **maxmemory gereserveerd** configureert.

![Maxmemory beleid bestand Vgx.](./media/cache-configure/redis-cache-maxmemory-policy.png)

**Maxmemory-beleid** kunt u kiezen uit de volgende beleidsregels voor verwijdering.

-   vluchtige-recentelijk - dit is de standaardinstelling.
-   AllKeys recentelijk
-   volatile random
-   AllKeys willekeurige
-   vluchtige ttl
-   noeviction

Zie voor meer informatie over het beleid van maxmemory [beleid voor verwijdering](http://redis.io/topics/lru-cache#eviction-policies).

De instelling **maxmemory gereserveerd** configureert de hoeveelheid geheugen in MB die is gereserveerd voor niet-cache bewerkingen als replicatie tijdens failover. Het kan ook worden gebruikt wanneer er een verhouding hoog fragmentatie. Als u deze waarde kunt u een meer consistente bestand Vgx. server ervaring hebben als de belastingstoestand. Deze waarde moet worden ingesteld voor workloads die zijn zware schrijven hoger. Wanneer het geheugen is gereserveerd voor dergelijke bewerkingen is niet beschikbaar voor de opslag van gegevens in de cache.

>[AZURE.IMPORTANT] De instelling **maxmemory gereserveerd** is alleen beschikbaar voor Standard en Premium in cache opgeslagen.

### <a name="keyspace-notifications-advanced-settings"></a>Keyspace meldingen (geavanceerde instellingen)

Bestand Vgx. keyspace meldingen op het blad **Geavanceerde instellingen** zijn geconfigureerd. Keyspace meldingen kunnen clients meldingen ontvangen wanneer bepaalde gebeurtenissen optreden.

![Geavanceerde instellingen voor Cache bestand Vgx.](./media/cache-configure/redis-cache-advanced-settings.png)

>[AZURE.IMPORTANT] Keyspace meldingen en de instelling van de **kennis keyspace gebeurtenissen** zijn alleen beschikbaar voor Standard en Premium in cache opgeslagen.

Zie [Bestand Vgx. Keyspace meldingen](http://redis.io/topics/notifications)voor meer informatie. Zie het bestand [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) in de [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) voorbeeld voor voorbeeldcode.

<a name="recommendations"></a>
## <a name="redis-cache-advisor"></a>Advisor Cache bestand Vgx.

De blade **aanbevelingen** geeft aanbevelingen voor de cache. Tijdens normale bewerkingen worden geen aanbevelingen weergegeven. 

![Aanbevelingen](./media/cache-configure/redis-cache-no-recommendations.png)

Als de voorwaarden tijdens de werkzaamheden van de cache, zoals veel geheugen of netwerkbandbreedte belasting van de server optreden, wordt een waarschuwing weergegeven op het blad **Bestand Vgx. Cache** .

![Aanbevelingen](./media/cache-configure/redis-cache-recommendations-alert.png)

Meer informatie vindt u op de bladeserver **aanbevelingen** .

![Aanbevelingen](./media/cache-configure/redis-cache-recommendations.png)

U kunt deze gegevens op in de secties [Monitoring diagrammen](cache-how-to-monitor.md#monitoring-charts) en [grafieken voor gebruik](cache-how-to-monitor.md#usage-charts) van de **Cache bestand Vgx.** blade controleren.

Elke laag prijzen heeft verschillende beperkingen voor clientverbindingen, geheugen en bandbreedte. Als de cache nadert de maximale capaciteit voor deze gegevens gedurende een langere periode van tijd, wordt een aanbeveling gemaakt. Zie de volgende tabel voor meer informatie over de parameters en grenzen herzien door het hulpprogramma **aanbevelingen** .

| Metric Cache bestand Vgx.      | Zie voor meer informatie                                                  |
|-------------------------|---------------------------------------------------------------------------|
| Verbruik van de netwerkbandbreedte | [Cache prestaties - bandbreedte](cache-faq.md#cache-performance) |
| Aangesloten clients       | [Standaard bestand Vgx. serverconfiguratie - maxclients](#maxclients)            |
| Belasting van de server             | [Gebruik grafieken - Server laden bestand Vgx.](cache-how-to-monitor.md#usage-charts)  |
| Geheugengebruik            | [Prestaties van de cache - grootte](cache-faq.md#cache-performance)                |

Upgrade uw cache, klikt u op **Nu bijwerken** om de [prijzen laag](#pricing-tier) wijzigen en schalen van de cache. Zie voor meer informatie over het kiezen van een prijzen laag [Cache bestand Vgx. aanbod en de grootte moet ik gebruiken?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="scale-settings"></a>Schaalinstellingen voor

De instellingen in het gedeelte **schalen** kunt u openen en de volgende instellingen voor de cache configureren.

![Netwerk](./media/cache-configure/redis-cache-scale.png)

-   [Prijzen laag](#pricing-tier)
-   [Clustergrootte bestand Vgx.](#cluster-size)

### <a name="pricing-tier"></a>Prijzen laag

Klik op **prijzen laag** wilt bekijken of wijzigen van de prijzen laag voor de cache. Zie voor meer informatie over het schalen, [hoe schaal Azure bestand Vgx. Cache](cache-how-to-scale.md).

![Prijzen laag Cache bestand Vgx.](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>
### <a name="redis-cluster-size"></a>Clustergrootte bestand Vgx.

Klik op **(Voorbeeld) bestand Vgx. clustergrootte** om de clustergrootte voor een lopend premium cache terwijl clusteren is ingeschakeld.

>[AZURE.NOTE] Houd er rekening mee dat terwijl de Azure bestand Vgx. Cache Premium laag is vrijgegeven aan de algemene beschikbaarheid, de clustergrootte bestand Vgx. functie is momenteel in de voorvertoning.

![Clustergrootte bestand Vgx.](./media/cache-configure/redis-cache-redis-cluster-size.png)

Gebruik de schuifregelaar om de clustergrootte, of typ een getal tussen 1 en 10 in het tekstvak **aantal Shard** en klik op **OK** om op te slaan.

>[AZURE.IMPORTANT] Bestand Vgx. clustering is alleen beschikbaar voor Premium-caches. Zie voor meer informatie [het configureren van clusters voor een Premium Azure bestand Vgx. Cache](cache-how-to-premium-clustering.md).


## <a name="data-management-settings"></a>Instellingen voor het beheer van gegevens

De instellingen in de sectie **beheer** kunnen u toegang tot en de volgende instellingen configureren voor uw cache.

![Gegevensbeheer](./media/cache-configure/redis-cache-data-management.png)

-   [Bestand Vgx persistentie van gegevens.](#redis-data-persistence)
-   [Importeren/exporteren](#importexport)

### <a name="redis-data-persistence"></a>Bestand Vgx persistentie van gegevens.

Klik op **bestand Vgx. persistentie van gegevens** als u wilt inschakelen, uitschakelen of configureren van persistentie van gegevens voor de premium-cache.

![Bestand Vgx persistentie van gegevens.](./media/cache-configure/redis-cache-persistence-settings.png)

Klik op **ingeschakeld** als u de back-up van RDB (database bestand Vgx.) zodat het bestand Vgx. persistentie. Om het bestand Vgx. persistentie is uitgeschakeld, klikt u op **uitgeschakeld**.

Configureren van de back-upinterval een **Back-up frequentie** te selecteren uit de vervolgkeuzelijst. U kunt kiezen uit **15 minuten**, **30 minuten** **60 minuten**, **6 uur**, **12 uur**en **24 uur**. Dit interval begint te tellen ingedrukt nadat de vorige back-bewerking met succes is uitgevoerd en als het voorbij is een nieuwe back-up wordt gestart.

Klik op **Opslag Account** om het account opslag gebruiken te selecteren en kiest u de **primaire sleutel** of de **secundaire sleutel** gebruiken uit de **Opslag sleutel** vervolgkeuzelijst. Moet u een account voor opslag in hetzelfde gebied, als de cache en een rekening van de **Premie opslag** wordt aanbevolen omdat de premie opslag heeft een hogere doorvoersnelheid. Wanneer de sleutel opslag voor uw permanente account opnieuw wordt gegenereerd, moet u de gewenste toets opnieuw **Opslag sleutel** vervolgkeuzelijst kiezen.

Klik op **OK** om de permanente configuratie op te slaan.

>[AZURE.IMPORTANT] Persistentie van bestand Vgx. gegevens is alleen beschikbaar voor Premium-caches. Zie voor meer informatie [het configureren van persistentie van een Premium Azure bestand Vgx. Cache](cache-how-to-premium-persistence.md).

### <a name="importexport"></a>Importeren/exporteren

Importeren/exporteren is een Azure bestand Vgx. Cache management gegevensbewerking die u kunt gegevens importeren in Azure bestand Vgx. Cache of gegevens exporteren vanuit Azure bestand Vgx. Cache importeren en exporteren van een momentopname van een bestand Vgx. Cache Database (RDB) uit de cache van een premie aan een blob pagina in een opslag Azure Account. Hiermee kunt u migreren tussen verschillende exemplaren van het bestand Vgx. Azure-Cache of vullen van de cache met gegevens vóór gebruik.

Importeren kan worden gebruikt om bestanden compatibel RDB bestand Vgx. uit bestand Vgx. servers waarop wordt uitgevoerd in een wolk of het milieu, met inbegrip van bestand Vgx. uitgevoerd op Linux, Windows, of een wolk provider zoals Amazon Web Services en andere. Het importeren van gegevens is een eenvoudige manier om een cache te maken met vooraf ingestelde gegevens. Tijdens het importproces Azure bestand Vgx. Cache de RDB-bestanden uit de Azure opslag in het geheugen laadt en vervolgens voegt de sleutels in de cache.

Exporteren kunt u de gegevens in Azure bestand Vgx. Cache bestand Vgx. compatibel RDB-bestanden exporteren. U kunt deze functie gebruiken om gegevens te verplaatsen van de ene instantie van Azure bestand Vgx. Cache naar een andere of naar een andere server bestand Vgx.. Tijdens het exporteren die een tijdelijk bestand wordt gemaakt op de VM die fungeert als host voor het serverexemplaar Azure bestand Vgx. Cache en het bestand is geüpload naar de aangewezen opslag rekening. Als de exportbewerking is voltooid met status van slagen of mislukken, wordt het tijdelijke bestand verwijderd.

>[AZURE.IMPORTANT] Importeren/exporteren is alleen beschikbaar voor Premium-tier-caches. Zie [gegevens in Cache voor Azure bestand Vgx. importeren en exporteren](cache-how-to-import-export-data.md)voor meer informatie en instructies.


## <a name="administration-settings"></a>Beheerinstellingen

De instellingen in de sectie **beheer** kunt u de volgende beheertaken uitvoeren voor de premium-cache. 

![Beheer](./media/cache-configure/redis-cache-administration.png)

-   [Opnieuw opstarten](#reboot)
-   [Updates plannen](#schedule-updates)

>[AZURE.IMPORTANT] De instellingen in deze sectie zijn alleen beschikbaar voor Premium-tier-caches.

### <a name="reboot"></a>Opnieuw opstarten

Het blad **opnieuw opstarten** kunt u een of meer knooppunten van de cache opnieuw op te starten. Hiermee kunt u uw aanvraag voor tolerantie bij een storing testen.

![Opnieuw opstarten](./media/cache-configure/redis-cache-reboot.png)

Als u een cache premium terwijl clusteren is ingeschakeld, kunt u selecteren welke shards van de cache op te starten.

![Opnieuw opstarten](./media/cache-configure/redis-cache-reboot-cluster.png)

Opnieuw opstarten van een of meer knooppunten van de cache, selecteert u de gewenste knooppunten en klikt u op **opnieuw opstarten**. Als u een cache premium terwijl clusteren is ingeschakeld, selecteert u de shard(s) op te starten en klik vervolgens op **opnieuw opstarten**. Na een paar minuten weer de geselecteerde knooppunten opnieuw opstarten en er wordt on line een paar minuten later.

>[AZURE.IMPORTANT] Opnieuw opstarten is alleen beschikbaar voor Premium-tier-caches. Zie voor meer informatie en instructies [Azure bestand Vgx. Cache-beheer - opnieuw opstarten](cache-administration.md#reboot).

### <a name="schedule-updates"></a>Updates plannen

De blade **updates plannen** kunt u een onderhoudsvenster bestand Vgx. server updates voor uw cache aanwijzen. 

>[AZURE.IMPORTANT] Houd er rekening mee dat het onderhoudsvenster is alleen van toepassing op bestand Vgx. serverupdates, en niet op een Azure werkt of updates voor het besturingssysteem van de VMs die host zijn van de cache.

![Updates plannen](./media/cache-configure/redis-schedule-updates.png)

Als u een onderhoudsvenster, controleren de gewenste dagen geven het onderhoud venster start uur per dag en klik op **OK**. Houd er rekening mee dat het venster onderhoudstijd in UTC is. 

>[AZURE.IMPORTANT] Updates plannen is alleen beschikbaar voor Premium-tier-caches. Zie [Azure bestand Vgx. Cache-beheer - updates plannen](cache-administration.md#schedule-updates)voor meer informatie en instructies.

## <a name="diagnostics-settings"></a>Diagnostische instellingen

De **Diagnostische** sectie kunt u diagnostische gegevens configureren voor uw bestand Vgx. Cache.

![Diagnostische gegevens](./media/cache-configure/redis-cache-diagnostics.png)

Klik op **Diagnostische gegevens van** [de opslag-account configureren](cache-how-to-monitor.md#enable-cache-diagnostics) diagnostics in cache opgeslagen.

![Diagnostische gegevens van Cache bestand Vgx.](./media/cache-configure/redis-cache-diagnostics-settings.png)

Klik op **bestand Vgx. metrics** [Statistieken bekijken](cache-how-to-monitor.md#how-to-view-metrics-and-customize-charts) voor de cache en **waarschuwingsregels** [waarschuwingsregels](cache-how-to-monitor.md#operations-and-alerts)instellen.

Zie voor meer informatie over diagnostische gegevens van het bestand Vgx. Azure-Cache, [Azure bestand Vgx. Cache controleren](cache-how-to-monitor.md).


## <a name="network-settings"></a>Netwerkinstellingen

De instellingen in de sectie **netwerk** kunt u openen en de volgende instellingen voor de cache configureren.

![Netwerk](./media/cache-configure/redis-cache-network.png)

>[AZURE.IMPORTANT] Virtueel netwerkinstellingen zijn alleen beschikbaar voor premium caches die zijn geconfigureerd met de ondersteuning van VNET tijdens het maken van de cache. Ondersteuning voor informatie over het maken van een premium-cache met VNET en bijwerken van de instellingen, [het configureren van virtuele netwerk ondersteuning voor een Premium Azure bestand Vgx. Cache](cache-how-to-premium-vnet.md)bekijken.

## <a name="resource-management-settings"></a>De instellingen voor bronnen

![Bronbeheer](./media/cache-configure/redis-cache-resource-management.png)

De sectie **Tags** kunt u uw bronnen te ordenen. Zie [labels voor het ordenen van uw resources Azure gebruiken](../resource-group-using-tags.md)voor meer informatie.

De sectie **wordt vergrendeld** , kunt u een abonnement, resourcegroep of resource om te voorkomen dat andere gebruikers in uw organisatie per ongeluk verwijderen of wijzigen van kritieke bronnen vergrendelen. Zie [bronnen vergrendelen met Azure Resource Manager](../resource-group-lock-resources.md)voor meer informatie.

De sectie **gebruikers** biedt ondersteuning voor op rollen gebaseerde toegangscontrole (RBAC) in de Azure portal zodat organisaties die voldoen aan hun toegang beheer eenvoudig en nauwkeurig geredigeerd. Zie [op rollen gebaseerde toegangscontrole in Azure portal](../active-directory/role-based-access-control-configure.md)voor meer informatie.

Klik op **sjabloon exporteren** te exporteren van een sjabloon van de gedistribueerde bronnen voor toekomstige implementaties. Zie [bronnen distribueren met sjablonen Azure Resource Manager](../resource-group-template-deploy.md)voor meer informatie over het werken met sjablonen.

## <a name="default-redis-server-configuration"></a>Standaard configuratie bestand Vgx.

Nieuwe exemplaren van het bestand Vgx. Azure-Cache zijn geconfigureerd met het volgende bestand Vgx. standaardconfiguratiewaarden.

>[AZURE.NOTE] De instellingen in deze sectie kunnen niet worden gewijzigd met behulp van de `StackExchange.Redis.IServer.ConfigSet` methode. Als deze methode wordt aangeroepen met een van de opdrachten in deze sectie, wordt de volgende uitzondering gegenereerd:  
>
>`StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
>  
>Alle waarden die kunnen geconfigureerd, zoals **geheugen van max-beleid worden**worden geconfigureerd met de Azure portal of opdrachtregel beheerprogramma's zoals Azure CLI of PowerShell.

|Instelling|Standaardwaarde|Beschrijving|
|---|---|---|
|databases|16|Het aantal databases is 16, maar kunt u een ander nummer op basis van de prijzen laag. <sup>1</sup> de standaarddatabase DB 0 is, kunt u een op een per verbinding basis met `connection.GetDatabase(dbid)` waarbij database-id is een getal tussen `0` en `databases - 1`.|
|maxclients|Afhankelijk van de prijzen laag<sup>2</sup>|Dit is het maximum aantal verbonden clients gelijktijdig zijn toegestaan. Als de limiet is bereikt wordt bestand Vgx. nieuwe verbindingen voor het verzenden van een fout 'maximum aantal clients bereikt' gesloten.|
|maxmemory-beleid|vluchtige recentelijk|Maxmemory-beleid is de instelling voor het bestand Vgx. selecteren wat u zal wilt verwijderen als maxmemory (de grootte van de cache die dat u hebt geselecteerd tijdens het maken van de cache) wordt bereikt. De standaardinstelling is met Azure bestand Vgx. Cache vluchtige-recentelijk, waarmee u de toetsen met een verlopen instellen met behulp van een algoritme Recentelijk verwijdert. Deze instelling kan worden geconfigureerd in de portal Azure. Zie voor meer informatie [Maxmemory beleid en maxmemory](#maxmemory-policy-and-maxmemory-reserved).|
|Voorbeelden van maxmemory|3|Recentelijk en minimale TTL-algoritmen niet nauwkeurig algoritmen, maar worden benaderd algoritmen (om geheugen te besparen), zodat u kunt ook de grootte van de steekproef te controleren. Bijvoorbeeld voor standaard zal bestand Vgx. drie sleutels controleren en kies de regellijn die is minder recent.|
|LUA-termijn|5.000|Maximale uitvoeringstijd van een Lua script in milliseconden. Als u de maximale verwerkingstijd wordt bereikt wordt bestand Vgx. vastgelegd dat een script nog worden uitgevoerd nadat de maximaal toegestane tijd en beginnen zal met het reageren op query's met een fout.|
|LUA-gebeurtenis-limit|500|Dit is de maximale grootte van de gebeurtenissenwachtrij script.|
|client-output-buffer-limit normalclient-output-buffer-limit pubsub|0 0 032mb 8mb 60|De client output buffer grenzen kunnen dwingen ontkoppeling van clients die niet van gegevens van de server snel genoeg om een bepaalde reden lezen zijn (een veel voorkomende oorzaak is dat een client Pub/Sub zo snel als de uitgever deze produceren kan berichten niet meer verbruiken) worden gebruikt. Zie [http://redis.io/topics/clients](http://redis.io/topics/clients)voor meer informatie.|

<a name="databases"></a>
<sup>1</sup> De limiet voor de `databases` is verschillend voor elke Azure bestand Vgx. Cache prijzen laag en kan worden ingesteld bij het maken van de cache. Als er geen `databases` instelling is opgegeven tijdens het maken van de cache, de standaardwaarde is 16.

-   Basic en Standard caches
    -   C0-cache (250 MB) - tot 16-databases
    -   C1-cache (1 GB) - tot 16-databases
    -   C2-cache (2,5 GB) - tot 16-databases
    -   C3 (6 GB) cache - tot 16-databases
    -   C4 (13 GB) cache - tot 32-databases
    -   C5-cache (26 GB) - tot 48 databases
    -   C6 (53 GB) cache - tot 64-databases
-   Premium caches
    -   P1 (6 GB tot 60 GB) - tot 16-databases
    -   P2 (13 GB - 130 GB) - tot 32-databases
    -   P3 (26 GB - 260 GB) - tot 48 databases
    -   P4 (53 GB - 530 GB) - tot 64-databases
    -   Alle premium caches met bestand Vgx. cluster ingeschakeld - bestand Vgx. cluster ondersteunt alleen gebruik van database 0 zodat de `databases` beperken voor elke cache premium met bestand Vgx. cluster ingeschakeld is in feite 1 en de opdracht [Select](http://redis.io/commands/select) is niet toegestaan. Zie voor meer informatie [moet ik wijzigingen aanbrengen in de clienttoepassing clustering?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)


>[AZURE.NOTE] De `databases` instelling kan worden geconfigureerd alleen tijdens het maken van de cache en alleen met PowerShell CLI of andere clients management. Voor een voorbeeld van de configuratie van `databases` tijdens het maken van de cache met PowerShell, Zie [Nieuw AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).


<a name="maxclients"></a>
<sup>2</sup> `maxclients` is verschillend voor elke Azure bestand Vgx. Cache prijzen laag.

-   Basic en Standard caches
    -   C0-cache (250 MB) - tot 256 verbindingen
    -   C1-cache (1 GB) - tot 1000 verbindingen
    -   C2 (2,5 GB) cache - maximaal 2.000 verbindingen
    -   C3 (6 GB) cache - maximaal 5000 verbindingen
    -   C4 (13 GB) cache - maximaal 10.000 verbindingen
    -   C5-cache (26 GB) - maximaal 15.000 verbindingen
    -   C6 (53 GB) cache - maximaal 20.000 verbindingen
-   Premium caches
    -   P1 (6 GB tot 60 GB) - maximaal 7.500 verbindingen
    -   P2 (13 GB - 130 GB) - maximaal 15.000 verbindingen
    -   P3 (26 GB - 260 GB) - maximaal 30.000 verbindingen
    -   P4 (53 GB - 530 GB) - maximaal 40.000 verbindingen

## <a name="redis-commands-not-supported-in-azure-redis-cache"></a>Opdrachten die niet worden ondersteund in Azure bestand Vgx. Cache bestand Vgx.

>[AZURE.IMPORTANT] De volgende opdrachten zijn uitgeschakeld omdat de configuratie en het beheer van Azure bestand Vgx. Cache exemplaren wordt beheerd door Microsoft. Als u probeert aan te roepen ze ontvangt u een foutbericht vergelijkbaar met `"(error) ERR unknown command"`.
>
>-  BGREWRITEAOF
>-  BGSAVE
>-  CONFIG
>-  FOUTOPSPORING
>-  MIGREREN
>-  OPSLAAN
>-  AFSLUITEN
>-  SLAVEOF
>-  CLUSTER - schrijven zijn uitgeschakeld, maar alleen-lezen Cluster opdrachten zijn toegestaan.

Zie voor meer informatie over opdrachten bestand Vgx. [http://redis.io/commands](http://redis.io/commands).

## <a name="redis-console"></a>Console bestand Vgx.

U kunt opdrachten veilig uitgeven aan uw Azure bestand Vgx. Cache exemplaren met het **Bestand Vgx. Console**, die beschikbaar voor de standaard is en Premium in cache opgeslagen.

>[AZURE.IMPORTANT] Het bestand Vgx. Console werkt niet met VNET, clustering, en databases van andere dan 0. 
>
>-  [VNET](cache-how-to-premium-vnet.md) - wanneer de cache is een onderdeel van een VNET, alleen voor clients in de VNET toegang tot de cache. Omdat het bestand Vgx. Console het bestand Vgx. cli.exe client gehost op VMs die geen deel uitmaken van de VNET gebruikt, kan het verbinden met de cache.
>-  [Clustering](cache-how-to-premium-clustering.md) - het bestand Vgx. Console gebruikt de client bestand Vgx. cli.exe clustering op dit moment niet worden ondersteund. Het bestand Vgx. cli-hulpprogramma in de [instabiele](http://redis.io/download) structuur van het bestand Vgx. opslagplaats op GitHub implementeert basisondersteuning wanneer gestart met de `-c` overschakelen. Zie [afspelen met het cluster](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) op [http://redis.io](http://redis.io) in het [bestand Vgx. cluster-zelfstudie](http://redis.io/topics/cluster-tutorial)voor meer informatie.
>-  Het bestand Vgx. Console kunt u een nieuwe verbinding met de database 0 elke keer dat u een opdracht indienen. U kunt niet gebruiken de `SELECT` opdracht voor het selecteren van een andere database omdat de database wordt teruggezet op 0 bij iedere opdracht. Zie voor meer informatie over het uitvoeren van opdrachten bestand Vgx., met inbegrip van wijzigen naar een andere database [hoe kan ik bestand Vgx. opdrachten uitvoeren?](cache-faq.md#how-can-i-run-redis-commands)

Voor toegang tot de Console bestand Vgx., klik op de **Console** van de **Cache bestand Vgx.** blade.

![Console bestand Vgx.](./media/cache-configure/redis-console-menu.png)

Voor het verlenen van opdrachten exemplaar van uw cache typt in de gewenste opdracht in de console.

![Console bestand Vgx.](./media/cache-configure/redis-console.png)

Zie de vorige sectie voor [opdrachten die niet worden ondersteund in Azure bestand Vgx. Cache bestand Vgx.](#redis-commands-not-supported-in-azure-redis-cache) lijst van bestand Vgx. opdrachten zijn niet beschikbaar voor Azure bestand Vgx. Cache. Zie voor meer informatie over opdrachten bestand Vgx. [http://redis.io/commands](http://redis.io/commands). 

## <a name="move-your-cache-to-a-new-subscription"></a>Verplaats de cache naar een nieuw abonnement

U kunt uw cache verplaatsen naar een nieuw abonnement door te klikken op **verplaatsen**.

![Verplaats de Cache bestand Vgx.](./media/cache-configure/redis-cache-move.png)

Zie [bronnen aan de nieuwe resourcegroep of abonnement verplaatsen](../resource-group-move-resources.md)voor meer informatie over het verplaatsen van resources uit een resourcegroep naar het andere en van een abonnement naar de andere.

## <a name="next-steps"></a>Volgende stappen
-   Zie voor meer informatie over het werken met de opdrachten bestand Vgx. [hoe kan ik bestand Vgx. opdrachten uitvoeren?](cache-faq.md#how-can-i-run-redis-commands).
