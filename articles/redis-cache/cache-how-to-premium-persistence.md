<properties 
    pageTitle="Het configureren van de persistentie van gegevens voor een Cache Premium Azure bestand Vgx." 
    description="Meer informatie over het configureren en beheren van permanente gegevens uw sessies Premium tier Azure-Cache bestand Vgx." 
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
    ms.date="09/30/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-data-persistence-for-a-premium-azure-redis-cache"></a>Het configureren van de persistentie van gegevens voor een Cache Premium Azure bestand Vgx.

Azure Cache bestand Vgx. levert verschillende cache-oplossingen die zorgen voor flexibiliteit in de keuze van de grootte van de cache en functies, waaronder de nieuwe Premium-laag.

De premium Azure bestand Vgx. Cache laag bevat functies zoals clustering, persistentie en ondersteuning voor virtuele netwerken. In dit artikel wordt beschreven hoe u van persistentie in een instantie premium Azure bestand Vgx. Cache.

Zie voor informatie over andere functies van de premium-cache, [Inleiding tot de Azure bestand Vgx. Cache Premium laag](cache-premium-tier-intro.md).

## <a name="what-is-data-persistence"></a>Wat is de persistentie van gegevens?
Bestand Vgx. persistentie kunt u gegevens die zijn opgeslagen in het bestand Vgx. aanhouden. U kunt ook momentopnamen en back-up van de gegevens die u in het geval van een hardwarestoring laden kunt. Dit is een enorm voordeel ten opzichte van Basic of Standard laag waar alle gegevens in het geheugen opgeslagen en kan er gegevensverlies in geval van een storing waar Cache knooppunten niet beschikbaar zijn. 

Azure bestand Vgx. Cache biedt bestand Vgx. persistentie met behulp van het [RDB-model](http://redis.io/topics/persistence), waarin de gegevens worden opgeslagen in een account Azure opslag. Als persistentie is geconfigureerd, blijft een momentopname van de cache bestand Vgx. in een bestand Vgx. binaire indeling naar de schijf op basis van een configureerbare back-upfrequentie bestaan in Azure bestand Vgx. Cache. Als er een catastrofale gebeurtenis optreedt die zowel de primaire als de replica cache uitschakelt, wordt de cache opnieuw opgebouwd met de meest recente snapshot.

Persistentie kan worden geconfigureerd vanuit de **Nieuwe Cache bestand Vgx.** blade tijdens het maken van de cache en op het blad van de **Instellingen** voor bestaande premium-caches.

## <a name="create-a-premium-cache"></a>Een premium-cache maken

Om een cache te maken en configureren van persistentie, aanmelden bij de [Azure portal](https://portal.azure.com) en klik op **Nieuw**->**Data- +**>**Bestand Vgx. Cache**.

![Maak een Cache bestand Vgx.][redis-cache-new-cache-menu]

Persistentie configureren, selecteert u een van de **Premium** -caches in de blade **kiest de prijzen laag** .

![Kies uw prijzen laag][redis-cache-premium-pricing-tier]

Zodra een premium prijzen laag is geselecteerd, klikt u op **bestand Vgx. persistentie**.

![Persistentie bestand Vgx.][redis-cache-persistence]

De stappen in de volgende sectie wordt beschreven hoe u een bestand Vgx. persistentie configureren op uw nieuwe premium-cache. Zodra het bestand Vgx. persistentie is geconfigureerd, klikt u op **maken** om het maken van uw nieuwe premium-cache met bestand Vgx. persistentie.

## <a name="configure-redis-persistence"></a>Configureren van persistentie bestand Vgx.

Bestand Vgx. persistentie is geconfigureerd op het blad **bestand Vgx. persistentie van gegevens** . Deze blade is geopend tijdens het maken van cache voor nieuwe caches, zoals beschreven in de vorige sectie. Voor bestaande caches, is de **persistentie van gegevens bestand Vgx.** blade toegankelijk vanuit de blade **Instellingen** voor de cache.

![Instellingen bestand Vgx.][redis-cache-settings]

Klik op **ingeschakeld** als u de back-up van RDB (database bestand Vgx.) zodat het bestand Vgx. persistentie. Als het bestand Vgx. persistentie in een eerder geactiveerde premium cache uitschakelt, klikt u op **uitgeschakeld**.

Configureren van de back-upinterval een **Back-up frequentie** te selecteren uit de vervolgkeuzelijst. U kunt kiezen uit **15 minuten**, **30 minuten** **60 minuten**, **6 uur**, **12 uur**en **24 uur**. Dit interval begint te tellen ingedrukt nadat de vorige back-bewerking met succes is uitgevoerd en als het voorbij is een nieuwe back-up wordt gestart.

Klik op **Opslag Account** om het account opslag gebruiken te selecteren en kiest u de **primaire sleutel** of de **secundaire sleutel** gebruiken uit de **Opslag sleutel** vervolgkeuzelijst. Moet u een account voor opslag in hetzelfde gebied, als de cache en een rekening van de **Premie opslag** wordt aanbevolen omdat de premie opslag heeft een hogere doorvoersnelheid. 

>[AZURE.IMPORTANT] Als de sleutel opslag voor uw permanente account opnieuw wordt gegenereerd, moet u de gewenste toets rechoose uit de vervolgkeuzelijst **Storage Key** .

![Persistentie bestand Vgx.][redis-cache-persistence-selected]

Klik op **OK** om de permanente configuratie op te slaan.

De volgende back-up (of de eerste back-up voor nieuwe caches) wordt gestart zodra de back-up frequentie-interval is verstreken.



## <a name="persistence-faq"></a>Veelgestelde vragen over persistentie

De volgende lijst bevat antwoorden op enkele veelgestelde vragen over persistentie Azure bestand Vgx. Cache.

-   [Kan ik de persistentie van een eerder gemaakte cache inschakelen?](#can-i-enable-persistence-on-a-previously-created-cache)
-   [Kan ik de back-up frequentie wijzigen nadat ik de cache gemaakt?](#can-i-change-the-backup-frequency-after-i-create-the-cache)
-   [Waarom als er een back-up frequentie van 60 minuten er meer dan 60 minuten tussen back-ups is?](#why-if-i-have-a-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups)
-   [Wat gebeurt er met de oude back-ups wanneer een nieuwe back-up wordt gemaakt?](#what-happens-to-the-old-backups-when-a-new-backup-is-made)
-   [Wat gebeurt er als ik naar een ander formaat hebt geschaald en een back-up die is gemaakt voordat de bewerking schalen wordt hersteld?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)

### <a name="can-i-enable-persistence-on-a-previously-created-cache"></a>Kan ik de persistentie van een eerder gemaakte cache inschakelen?

Ja, bestand Vgx. persistentie kan worden geconfigureerd op cache maken en bestaande premium-caches.

### <a name="can-i-change-the-backup-frequency-after-i-create-the-cache"></a>Kan ik de back-up frequentie wijzigen nadat ik de cache gemaakt?

Ja, kunt u de back-up frequentie op het blad **bestand Vgx. persistentie van gegevens** . Zie [configureren bestand Vgx. persistentie](#configure-redis-persistence)voor meer informatie.

### <a name="why-if-i-have-a-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups"></a>Waarom als er een back-up frequentie van 60 minuten er meer dan 60 minuten tussen back-ups is?

De frequentie van de back-up start pas als de vorige back-upproces is voltooid. Als de back-up frequentie 60 minuten is en een back-upproces 15 minuten duurt te voltooien, start niet de volgende back-up tot 75 minuten na de begintijd van de vorige back-up.

### <a name="what-happens-to-the-old-backups-when-a-new-backup-is-made"></a>Wat gebeurt er met de oude back-ups wanneer een nieuwe back-up wordt gemaakt?

Alle back-ups, met uitzondering van de meest recente bewerking worden automatisch verwijderd. Deze verwijdering niet onmiddellijk kan gebeuren, maar oudere back-ups zijn niet voor onbepaalde tijd bewaard.

### <a name="what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation"></a>Wat gebeurt er als ik naar een ander formaat hebt geschaald en een back-up die is gemaakt voordat de bewerking schalen wordt hersteld?

-   Als u hebt geschaald naar een groter formaat, is er geen effect.
-   Als u op een kleinere grootte hebt aangepast en u hebt een aangepaste [databases](cache-configure.md#databases) die is groter dan het [beperken van de databases](cache-configure.md#databases) voor de nieuwe grootte, gegevens in deze databases niet wordt hersteld. Zie voor meer informatie [Is mijn aangepaste databases instellen waarin dit probleem optreedt tijdens het schalen?](cache-how-to-scale.md#is-my-custom-databases-setting-affected-during-scaling)
-   Als u op een kleinere grootte hebt aangepast, en er niet genoeg ruimte in het kleinere formaat voor het opslaan van alle gegevens van de laatste back-up, wordt tijdens het herstelproces, meestal met behulp van het beleid [allkeys recentelijk](http://redis.io/topics/lru-cache) een verwijdering sleutels worden verwijderd.

## <a name="next-steps"></a>Volgende stappen
Leer meer cache premium-functies gebruiken.

-   [Inleiding tot het niveau van de premie van Azure Cache bestand Vgx.](cache-premium-tier-intro.md)
  
<!-- IMAGES -->

[redis-cache-new-cache-menu]: ./media/cache-how-to-premium-persistence/redis-cache-new-cache-menu.png

[redis-cache-premium-pricing-tier]: ./media/cache-how-to-premium-persistence/redis-cache-premium-pricing-tier.png

[redis-cache-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-persistence.png

[redis-cache-persistence-selected]: ./media/cache-how-to-premium-persistence/redis-cache-persistence-selected.png

[redis-cache-settings]: ./media/cache-how-to-premium-persistence/redis-cache-settings.png
