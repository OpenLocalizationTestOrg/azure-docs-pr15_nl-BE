<properties 
    pageTitle="Inleiding tot de laag Azure bestand Vgx. Cache Premium | Microsoft Azure" 
    description="Informatie over het maken en beheren van bestand Vgx. persistentie, bestand Vgx. clustering en VNET ondersteuning voor uw sessies Premium tier Azure-Cache bestand Vgx." 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="introduction-to-the-azure-redis-cache-premium-tier"></a>Inleiding tot het niveau van de premie van Azure Cache bestand Vgx.
Azure bestand Vgx. Cache is een cache gedistribueerde, beheerde waarmee u uiterst schaalbare en responsieve toepassingen bouwen door middel van supersnelle toegang tot uw gegevens. 

De nieuwe Premium-tier is een onderneming klaar laag, inclusief alle functies van de standaard-tier en nog veel meer, zoals betere prestaties, grotere werklast noodherstel, importeren/exporteren en verbeterde beveiliging. Verder lezen voor meer informatie over de extra functies van de Premium-cache laag.

## <a name="better-performance-compared-to-standard-or-basic-tier"></a>Betere prestaties in vergelijking met standaard of Basic laag
**Betere prestaties via standaard of elementaire laag.** Caches in de premie laag worden op hardware die snellere processors en biedt hogere prestaties in vergelijking met de Basic- of Standard-Tier geïmplementeerd. Premium-tier Caches hebben hogere doorvoer en lagere vertragingstijden. 

**Doorvoer voor de Cache met dezelfde grootte is in de premie hoger in vergelijking met standaard laag.** Bijvoorbeeld: de doorvoer van een 53 P4 GB cache (Premium) is 250K-aanvragen per seconde in vergelijking met 150 K voor C6 (standaard).

Zie voor meer informatie over de grootte, doorvoer en bandbreedte met premium caches [Azure bestand Vgx. Cache Veelgestelde vragen](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)

## <a name="redis-data-persistence"></a>Bestand Vgx persistentie van gegevens.
De Premium-laag kunt u de gegevens van de cache op een rekening Azure opslag. In een Basic-/ Standard-cache alle gegevens alleen in het geheugen opgeslagen. In geval van een onderliggende infrastructuur kunnen er problemen worden potentieel gegevensverlies. Wij raden aan tolerantie tegen gegevensverlies verhoogd met de functie bestand Vgx. persistentie in de Premium-laag. Azure Cache bestand Vgx. biedt RDB en AOF (binnenkort) opties in het [bestand Vgx. persistentie](http://redis.io/topics/persistence). 

Zie voor instructies over het configureren van persistentie [persistentie voor een Premium Azure bestand Vgx. Cache configureren](cache-how-to-premium-persistence.md).

##<a name="redis-cluster"></a>Cluster bestand Vgx.
Als u wilt maken caches groter zijn dan 53 GB of shard gegevens over meerdere bestand Vgx. knooppunten wilt, kunt u bestand Vgx. clustering is beschikbaar in de premie laag. Elk knooppunt bestaat uit een combinatie van een primaire replica/cache beheerd door Azure voor hoge beschikbaarheid. 

**Bestand Vgx. clustering biedt u maximale schaal en de doorvoer.** Doorvoer lineair wordt verhoogd naarmate u het aantal shards (knooppunten) in het cluster. Bv. Als u een cluster P4 van 10 shards maakt, is de beschikbare doorvoer 250K * 10 = 2,5 miljoen-aanvragen per seconde. Zie de [Azure bestand Vgx. Cache Veelgestelde vragen](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) voor meer informatie over de grootte, doorvoer en bandbreedte met premium-caches.

Om te beginnen met clusters Zie [clusters voor een Premium Azure bestand Vgx. Cache configureren](cache-how-to-premium-clustering.md).

##<a name="enhanced-security-and-isolation"></a>Verbeterde beveiliging en isolatie

Caches gemaakt in de Basic- of Standard-laag zijn op het openbare internet toegankelijk. Toegang tot de Cache wordt beperkt op basis van de toegangstoets. Met de Premium-laag kunt u verdere ervoor zorgen dat alleen clients binnen een bepaald netwerk toegang hebben tot de Cache. U kunt de Cache in [Azure Virtual Network (VNet)](https://azure.microsoft.com/services/virtual-network/)van een bestand Vgx. implementeren. U kunt alle functies van VNet zoals subnetten, opstellen, en andere functies die verder beperken de toegang tot het bestand Vgx..

Zie [ondersteuning voor een Premium Azure bestand Vgx. Cache virtueel netwerk configureren](cache-how-to-premium-vnet.md)voor meer informatie.

## <a name="importexport"></a>Importeren/exporteren

Importeren/exporteren is een Azure bestand Vgx. Cache management gegevensbewerking die u kunt gegevens importeren in Azure bestand Vgx. Cache of gegevens exporteren vanuit Azure bestand Vgx. Cache importeren en exporteren van een momentopname van een bestand Vgx. Cache Database (RDB) uit de cache van een premie aan een blob pagina in een opslag Azure Account. Hiermee kunt u migreren tussen verschillende exemplaren van het bestand Vgx. Azure-Cache of vullen van de cache met gegevens vóór gebruik.

Importeren kan worden gebruikt om bestanden compatibel RDB bestand Vgx. uit bestand Vgx. servers waarop wordt uitgevoerd in een wolk of het milieu, met inbegrip van bestand Vgx. uitgevoerd op Linux, Windows, of een wolk provider zoals Amazon Web Services en andere. Het importeren van gegevens is een eenvoudige manier om een cache te maken met vooraf ingestelde gegevens. Tijdens het importproces Azure bestand Vgx. Cache de RDB-bestanden uit de Azure opslag in het geheugen laadt en vervolgens voegt de sleutels in de cache.

Exporteren kunt u de gegevens in Azure bestand Vgx. Cache bestand Vgx. compatibel RDB-bestanden exporteren. U kunt deze functie gebruiken om gegevens te verplaatsen van de ene instantie van Azure bestand Vgx. Cache naar een andere of naar een andere server bestand Vgx.. Een tijdelijk bestand wordt gemaakt op de VM waarop de Azure bestand Vgx. Cache serverinstantie tijdens het exporteren en het bestand is geüpload naar de aangewezen opslag rekening. Als de exportbewerking is voltooid met status van slagen of mislukken, wordt het tijdelijke bestand verwijderd.

Zie [gegevens importeren in en exporteren van gegevens uit de Cache van Azure bestand Vgx.](cache-how-to-import-export-data.md)voor meer informatie.

## <a name="reboot"></a>Opnieuw opstarten

De premium-laag kunt u een of meer knooppunten van de cache op verzoek opnieuw op te starten. Hiermee kunt u uw aanvraag voor tolerantie bij een storing testen. U kunt de volgende knooppunten opnieuw opstarten.

-   Het hoofdknooppunt van de cache
-   Slave-knooppunt van de cache
-   Master- en slave-knooppunten van de cache
-   Als u een cache premium met clustering, kunt u opstarten de master, slave of beide knooppunten voor afzonderlijke shards in de cache

Voor meer informatie, Zie [opnieuw opstarten](cache-administration.md#reboot) en [Veelgestelde vragen over het opnieuw opstarten](cache-administration.md#reboot-faq).

## <a name="schedule-updates"></a>Updates plannen

De geplande updates-functie kunt u een onderhoudsvenster voor uw cache aanwijzen. Wanneer het onderhoudsvenster is opgegeven, wordt een bestand Vgx. server bijgewerkt tijdens dit venster. Venster starten om een onderhoudsvenster aanwijzen, selecteert u de gewenste dagen en geeft u het onderhoud uur per dag. Houd er rekening mee dat het venster onderhoudstijd in UTC is. 

Zie [schema-updates](cache-administration.md#schedule-updates) en [updates plannen Veelgestelde vragen](cache-administration.md#schedule-updates-faq)voor meer informatie.

>[AZURE.NOTE] Alleen bestand Vgx. server wordt bijgewerkt tijdens het venster geplande onderhoud. Het onderhoudsvenster is niet van toepassing op Azure updates of updates voor het besturingssysteem van de VM.

## <a name="to-scale-to-the-premium-tier"></a>Voor de premium-laag

Als u wilt schalen naar de premie laag, kies een lagere premie in het blad **wijzigen prijzen laag** . Ook kunt u de cache van de premie laag met PowerShell en CLI schalen. Zie voor stapsgewijze instructies [hoe u schaal Azure bestand Vgx. Cache](cache-how-to-scale.md) en [een schaal bewerking automatiseren](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

## <a name="next-steps"></a>Volgende stappen

Maak een cache en ontdek de nieuwe functies van de premium-laag.

-   [Het configureren van persistentie van een Cache Premium Azure bestand Vgx.](cache-how-to-premium-persistence.md)
-   [Het configureren van virtuele netwerk ondersteuning voor een Cache Premium Azure bestand Vgx.](cache-how-to-premium-vnet.md)
-   [Het configureren van clusters voor een Cache Premium Azure bestand Vgx.](cache-how-to-premium-clustering.md)
-   [Hoe u de gegevens importeren in en exporteren uit de Cache van Azure bestand Vgx.](cache-how-to-import-export-data.md)
-   [Het beheren van Azure-Cache bestand Vgx.](cache-administration.md)
  

