<properties 
    pageTitle="Het beheren van Azure bestand Vgx. Cache | Microsoft Azure"
    description="Informatie over het uitvoeren van beheertaken uitvoeren, zoals het opnieuw opstarten en schema-updates voor Azure-Cache bestand Vgx."
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
    ms.date="09/27/2016"
    ms.author="sdanie" />

# <a name="how-to-administer-azure-redis-cache"></a>Het beheren van Azure-Cache bestand Vgx.

In dit onderwerp wordt beschreven hoe uit te voeren zoals opnieuw starten en updates voor uw Azure bestand Vgx. Cache exemplaren plannen.

>[AZURE.IMPORTANT] De instellingen en functies die in dit artikel worden beschreven, zijn alleen beschikbaar voor Premium tier caches.


## <a name="administration-settings"></a>Beheerinstellingen

De instellingen voor Azure bestand Vgx. Cache- **beheer** kunt u de volgende beheertaken uitvoeren voor de premium-cache. Om beheerinstellingen te openen, klikt u op **Instellingen** of **alle instellingen** uit het bestand Vgx. Cache blade en blader naar het gedeelte **beheer** in de blade **Instellingen** .

![Beheer](./media/cache-administration/redis-cache-administration.png)

-   [Opnieuw opstarten](#reboot)
-   [Updates plannen](#schedule-updates)

## <a name="reboot"></a>Opnieuw opstarten

Het blad **opnieuw opstarten** kunt u een of meer knooppunten van de cache opnieuw op te starten. Hiermee kunt u uw aanvraag voor tolerantie bij een storing testen.

![Opnieuw opstarten](./media/cache-administration/redis-cache-reboot.png)

Als u een cache premium terwijl clusteren is ingeschakeld, kunt u selecteren welke shards van de cache op te starten.

![Opnieuw opstarten](./media/cache-administration/redis-cache-reboot-cluster.png)

Selecteer de gewenste knooppunten opnieuw opstarten van een of meer knooppunten van de cache, en klik op **opnieuw opstarten**. Als u een cache premium terwijl clusteren is ingeschakeld, selecteert u de shard(s) op te starten en klik vervolgens op **opnieuw opstarten**. Na een paar minuten weer de geselecteerde knooppunten opnieuw opstarten en er wordt on line een paar minuten later.

De invloed op toepassingen die afhankelijk is van de knooppunten die u opnieuw opstarten.

-   **Hoofd** - als de master node opnieuw wordt opgestart, Azure bestand Vgx. Cache wordt overgenomen door het knooppunt replica en bevordert het model. Tijdens deze failover, kan er een kort interval waarin verbindingen naar de cache kunnen mislukken.
-   **Slave** - als de slave-knooppunt opnieuw is opgestart, is meestal geen invloed op clients van de cache.
-   **Beide master en slave** - als beide knooppunten cache opnieuw worden opgestart, wordt alle gegevens verloren verbindingen naar de cache mislukt in de cache, totdat het primaire knooppunt weer on line komt. Als u de [persistentie van gegevens](cache-how-to-premium-persistence.md)hebt geconfigureerd, wordt de meest recente back-up teruggezet wanneer de cache weer on line komt. Merk op dat een cache schrijft die na de meest recente back-up verloren.
-   **Knooppunten van een cache premium terwijl clusteren is ingeschakeld** - wanneer u de knooppunten van een premium cache opnieuw opstart terwijl clusteren is ingeschakeld, het gedrag is hetzelfde als wanneer u de knooppunten van een niet-geclusterde cache opnieuw opstarten.


>[AZURE.IMPORTANT] Opnieuw opstarten is alleen beschikbaar voor Premium-tier-caches.

## <a name="reboot-faq"></a>Veelgestelde vragen over opnieuw opstarten

-   [Welk knooppunt moet ik opnieuw opstarten als u wilt testen van mijn toepassing?](#which-node-should-i-reboot-to-test-my-application)
-   [Kan ik de cache Schakel clientverbindingen opnieuw opstarten?](#can-i-reboot-the-cache-to-clear-client-connections)
-   [Ik verliest gegevens uit de cache als opnieuw opstarten?](#will-i-lose-data-from-my-cache-if-i-do-a-reboot)
-   [Kan ik mijn cache met PowerShell CLI of andere beheerprogramma's opnieuw opstarten?](#can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools)
-   [Welke prijzen lagen kunt de functionaliteit voor opnieuw opstarten?](#what-pricing-tiers-can-use-the-reboot-functionality)


### <a name="which-node-should-i-reboot-to-test-my-application"></a>Welk knooppunt moet ik opnieuw opstarten als u wilt testen van mijn toepassing?

Test de betere beveiliging van uw toepassingen tegen fouten van het primaire knooppunt van de cache, het **Master** knooppunt opnieuw op te starten. Test de betere beveiliging van uw toepassingen tegen fouten van het tweede knooppunt, start opnieuw op het knooppunt **Slave** . Als u wilt testen de tolerantie van de toepassing tegen een totale uitval van de cache, start opnieuw op **beide** knooppunten.

### <a name="can-i-reboot-the-cache-to-clear-client-connections"></a>Kan ik de cache Schakel clientverbindingen opnieuw opstarten?

Ja, als u de cache opnieuw alle clientverbindingen gewist. Dit kan handig zijn in het geval waarbij alle clientverbindingen worden gebruikt, bijvoorbeeld als gevolg van een fout of een fout in de clienttoepassing. Elke laag prijzen heeft verschillende [client verbindingslimieten](cache-configure.md#default-redis-server-configuration) voor de verschillende formaten en als deze limieten zijn bereikt, geen verbindingen meer worden geaccepteerd. De cache opnieuw opstarten kunt u wissen alle clientverbindingen.

>[AZURE.IMPORTANT] Als uw verbindingen van clients worden gebruikt om door een logische fout of fout in de clientcode, houd er rekening mee dat StackExchange.Redis automatisch opnieuw verbonden als het bestand Vgx. knooppunt weer on line is. Als het onderliggende probleem niet is opgelost, blijven de verbindingen van clients worden gebruikt.

### <a name="will-i-lose-data-from-my-cache-if-i-do-a-reboot"></a>Ik verliest gegevens uit de cache als opnieuw opstarten?

Als u de **Master** - en **Slave** -knooppunten opnieuw alle gegevens in de cache (of die shard als u met een cache premium clusteren is ingeschakeld) is verloren. Als u de [persistentie van gegevens](cache-how-to-premium-persistence.md)hebt geconfigureerd, wordt de meest recente back-up wordt teruggezet wanneer de cache weer on line komt. Houd er rekening mee dat een cache-schrijfbewerkingen die hebben plaatsgevonden nadat de back-up is gemaakt, gaan verloren.

Als u slechts een van de knooppunten opnieuw gegevens zijn meestal verloren, maar nog steeds mogelijk. Bijvoorbeeld als de master node opnieuw wordt opgestart en het schrijven van een cache wordt uitgevoerd, de gegevens uit de cache schrijven is verloren gegaan. Een ander scenario voor verlies van gegevens zou zijn als u een knooppunt opnieuw opstarten en het andere knooppunt gebeurt vanwege een fout op hetzelfde moment gaan. Zie voor meer informatie over mogelijke oorzaken van verlies van gegevens, [Wat is er gebeurd met mijn gegevens in het bestand Vgx.?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md).

### <a name="can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools"></a>Kan ik mijn cache met PowerShell CLI of andere beheerprogramma's opnieuw opstarten?

Ja, voor PowerShell instructies Zie [een cache bestand Vgx. opnieuw opstarten](cache-howto-manage-redis-cache-powershell.md#to-reboot-a-redis-cache).

### <a name="what-pricing-tiers-can-use-the-reboot-functionality"></a>Welke prijzen lagen kunt de functionaliteit voor opnieuw opstarten?

Opnieuw opstarten is alleen beschikbaar in de premie prijzen laag.

## <a name="schedule-updates"></a>Updates plannen

De blade **updates plannen** kunt u een onderhoudsvenster voor uw cache aanwijzen. Wanneer het onderhoudsvenster is opgegeven, wordt een bestand Vgx. server bijgewerkt tijdens dit venster. Houd er rekening mee dat het onderhoudsvenster is alleen van toepassing op bestand Vgx. serverupdates, en niet op een Azure werkt of updates voor het besturingssysteem van de VMs die host zijn van de cache.

![Updates plannen](./media/cache-administration/redis-schedule-updates.png)

Als u een onderhoudsvenster, controleren de gewenste dagen geven het onderhoud venster start uur per dag en klik op **OK**. Houd er rekening mee dat het venster onderhoudstijd in UTC is. 

>[AZURE.NOTE] Het venster standaard onderhoud voor updates is 5 uur. Deze waarde kan niet worden geconfigureerd via de portal Azure, maar kunt u deze configureren in PowerShell met behulp van de `MaintenanceWindow` -parameter van de cmdlet [New-AzureRmRedisCacheScheduleEntry](https://msdn.microsoft.com/library/azure/mt763833.aspx) . Zie voor meer informatie [kan ik de geplande updates met PowerShell CLI of andere beheerprogramma's beheerd?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)

## <a name="schedule-updates-faq"></a>Veelgestelde vragen over updates plannen

-   [Wanneer updates worden uitgevoerd als ik de functie van schema-updates niet gebruik?](#when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature)
-   [Welke soorten updates worden uitgevoerd tijdens het venster geplande onderhoud?](#what-type-of-updates-are-made-during-the-scheduled-maintenance-window)
-   [Kan ik beheerde geplande updates met PowerShell CLI of andere beheerprogramma 's](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)
-   [Welke prijzen lagen, kan de functionaliteit van schema-updates gebruiken?](#what-pricing-tiers-can-use-the-schedule-updates-functionality)

### <a name="when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature"></a>Wanneer updates worden uitgevoerd als ik de functie van schema-updates niet gebruik?

Als u een onderhoudsvenster niet opgeeft, kunnen updates op elk gewenst moment worden gemaakt.

### <a name="what-type-of-updates-are-made-during-the-scheduled-maintenance-window"></a>Welke soorten updates worden uitgevoerd tijdens het venster geplande onderhoud?

Alleen bestand Vgx. server wordt bijgewerkt tijdens het venster geplande onderhoud. Het onderhoudsvenster is niet van toepassing op Azure updates of updates voor het besturingssysteem van de VM.

### <a name="can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools"></a>Kan ik beheerde geplande updates met PowerShell CLI of andere beheerprogramma 's

Ja, kunt u uw geplande updates met behulp van de volgende PowerShell cmdlets beheren.

-   [Get-AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763835.aspx)
-   [Nieuwe AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763834.aspx)
-   [Nieuwe AzureRmRedisCacheScheduleEntry](https://msdn.microsoft.com/library/azure/mt763833.aspx)
-   [Verwijderen AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763837.aspx)

### <a name="what-pricing-tiers-can-use-the-schedule-updates-functionality"></a>Welke prijzen lagen, kan de functionaliteit van schema-updates gebruiken?

Updates plannen is alleen beschikbaar in de premie prijzen laag.

## <a name="next-steps"></a>Volgende stappen

-   Ontdek meer functies van [Azure bestand Vgx. Cache premium laag](cache-premium-tier-intro.md) .





