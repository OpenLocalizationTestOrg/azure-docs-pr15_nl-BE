<properties 
    pageTitle="Importeren en exporteren van gegevens in de Cache van Azure bestand Vgx. | Microsoft Azure" 
    description="Meer informatie over het importeren en exporteren van gegevens van en naar een blob-opslag met de exemplaren van de premie Azure-Cache bestand Vgx." 
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

# <a name="import-and-export-data-in-azure-redis-cache"></a>Gegevens importeren en exporteren in Azure-Cache bestand Vgx.

Importeren/exporteren is een Azure bestand Vgx. Cache data management bewerking die u kunt gegevens importeren in Azure bestand Vgx. Cache of gegevens exporteren vanuit Azure bestand Vgx. Cache importeren en exporteren van een momentopname van een bestand Vgx. Cache Database (RDB) uit de cache van een premie aan een blob pagina in een opslag Azure Account. Importeren/exporteren kunt u migreren tussen verschillende exemplaren van het bestand Vgx. Azure-Cache of vullen van de cache met gegevens vóór gebruik.

Dit artikel bevat richtlijnen voor het importeren en exporteren van gegevens met Azure bestand Vgx. Cache en bevat de antwoorden op veelgestelde vragen.

>[AZURE.IMPORTANT] Importeren/exporteren in de voorvertoning en is alleen beschikbaar voor [premium-tier](cache-premium-tier-intro.md) -caches.

## <a name="import"></a>Importeren

Importeren kan worden gebruikt om bestanden compatibel RDB bestand Vgx. uit bestand Vgx. servers waarop wordt uitgevoerd in een wolk of het milieu, met inbegrip van bestand Vgx. uitgevoerd op Linux, Windows, of een wolk provider zoals Amazon Web Services en andere. Het importeren van gegevens is een eenvoudige manier om een cache te maken met vooraf ingestelde gegevens. Tijdens het importproces Azure bestand Vgx. Cache de RDB-bestanden uit de Azure opslag in het geheugen laadt en vervolgens voegt de sleutels in de cache.

>[AZURE.NOTE] Voordat u de importbewerking start, moet u ervoor zorgen dat uw bestand Vgx. Database (RDB) of bestanden worden geüpload naar pagina BLOB's in Azure opslag, in dezelfde regio en abonnement als uw exemplaar van het bestand Vgx. Azure-Cache. Zie [aan de slag met Azure Blob-opslag](../storage/storage-dotnet-how-to-use-blobs.md)voor meer informatie. Als u het gebruik van de functie voor het [Exporteren van Azure bestand Vgx. Cache](#export) RDB-bestand hebt geëxporteerd, uw RDB bestand al is opgeslagen in een blob pagina en klaar om te importeren.

1. Een of meer geëxporteerde cache BLOB's, [Blader naar de cache](cache-configure.md#configure-redis-cache-settings) in Azure portal importeren en klik op **gegevens importeren** in het blad van de **Instellingen** van uw exemplaar van de cache.

    ![Gegevens importeren][cache-import-data]

2. Klik op **Blob(s) kiezen** en selecteert u de account voor opslag met de gegevens te importeren.

    ![Opslag account kiezen][cache-import-choose-storage-account]

3. Klik op de container waarin de te importeren gegevens.

    ![Kies container][cache-import-choose-container]

4. Selecteer een of meer BLOB's importeren door te klikken op het gebied links van de naam van de blob en klik vervolgens op **selecteren**.

    ![BLOB's kiezen][cache-import-choose-blobs]

5. Klik op **importeren** om het importproces te starten.

    >[AZURE.IMPORTANT] De cache is niet toegankelijk voor clients van de cache tijdens het importeren en alle bestaande gegevens in de cache is verwijderd.

    ![Importeren][cache-import-blobs]

    De voortgang van de importbewerking kunt u controleren aan de hand van de kennisgevingen van de Azure portal of door het bekijken van gebeurtenissen in het [audit-logboek](cache-configure.md#support-amp-troubleshooting-settings).

    ![Voortgang importeren][cache-import-data-import-complete] 


## <a name="export"></a>Exporteren

Exporteren kunt u de gegevens in Azure bestand Vgx. Cache bestand Vgx. compatibel RDB-bestanden exporteren. U kunt deze functie gebruiken om gegevens te verplaatsen van de ene instantie van Azure bestand Vgx. Cache naar een andere of naar een andere server bestand Vgx.. Een tijdelijk bestand wordt gemaakt op de VM waarop de Azure bestand Vgx. Cache serverinstantie tijdens het exporteren en het bestand is geüpload naar de aangewezen opslag rekening. Als de exportbewerking is voltooid met status van slagen of mislukken, wordt het tijdelijke bestand verwijderd.

1. De huidige inhoud van de cache van opslag, [bladert u naar de cache](cache-configure.md#configure-redis-cache-settings) in het portal voor Azure exporteren en klik op **gegevens exporteren** uit het blad van de **Instellingen** van uw exemplaar van de cache.

    ![Opslag container kiezen][cache-export-data-choose-storage-container]

2. Klik op **Opslag Container kiezen** en selecteer de gewenste opslag. De rekening voor de opslag moet het abonnement en de regio als de cache.

    >[AZURE.IMPORTANT] Werken met pagina-blobs die worden ondersteund door zowel klassieke als ARM opslag rekeningen, maar worden niet ondersteund door de [Blob-opslag rekeningen](../storage/storage-blob-storage-tiers.md#blob-storage-accounts) op dit moment importeren/exporteren.

    ![Opslag account][cache-export-data-choose-account]

3. Kies de gewenste blob-container en klik op **selecteren**. Nieuwe container, klik op **Toevoegen Container** eerst toevoegen en selecteer vervolgens in de lijst.

    ![Opslag container kiezen][cache-export-data-container]

4. Typ een **voorvoegsel Blob** en klik op **exporteren** om het exportproces te starten. Voorvoegsel van de blob wordt gebruikt als voorvoegsel voor de namen van bestanden die zijn gegenereerd door deze exportbewerking.

    ![Exporteren][cache-export-data]

    De voortgang van de exportbewerking kunt u controleren aan de hand van de kennisgevingen van de Azure portal of door het bekijken van gebeurtenissen in het [audit-logboek](cache-configure.md#support-amp-troubleshooting-settings).

    ![][cache-export-data-export-complete]

    Caches blijven beschikbaar voor gebruik tijdens het exporteren.


## <a name="importexport-faq"></a>Veelgestelde vragen over het importeren/exporteren

Deze sectie bevat veelgestelde vragen over de functie importeren/exporteren.

-   [Welke prijzen lagen kunt importeren/exporteren?](#what-pricing-tiers-can-use-importexport)
-   [Kan ik gegevens importeren vanaf een willekeurige server bestand Vgx.](#can-i-import-data-from-any-redis-server)
-   [Mijn cache is beschikbaar tijdens importeren/exporteren?](#will-my-cache-be-available-during-an-importexport-operation)
-   [Kan ik importeren/exporteren gebruiken bij cluster bestand Vgx.](#can-i-use-importexport-with-redis-cluster)
-   [Hoe werkt importeren/exporteren met een aangepaste databases instellen](#how-does-importexport-work-with-a-custom-databases-setting)
-   [Hoe verschilt Import/Export bestand Vgx. persistentie?](#how-is-importexport-different-from-redis-persistence)
-   [Kan ik importeren/exporteren met PowerShell CLI of andere clients management automatiseren?](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
-   [Ik heb ontvangen een time-outfout tijdens de Import/exportbewerking. Wat betekent het?](#i-received-a-timeout-error-during-my-importexport-operation.-what-does-it-mean)
-   [Ik krijg een fout bij het exporteren van gegevens naar Azure Blob-opslag. Wat is er gebeurd?](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage.-what-happened)


### <a name="what-pricing-tiers-can-use-importexport"></a>Welke prijzen lagen kunt importeren/exporteren?

Importeren/exporteren is alleen beschikbaar in de premie prijzen laag.

### <a name="can-i-import-data-from-any-redis-server"></a>Kan ik gegevens importeren vanaf een willekeurige server bestand Vgx.

Ja, naast het importeren van gegevens die zijn geëxporteerd vanuit Azure bestand Vgx. Cache exemplaren als u RDB-bestanden importeren uit bestand Vgx. servers waarop wordt uitgevoerd in een wolk of de omgeving, zoals Linux, Windows, of cloud providers zoals Amazon Web Services. Hiertoe de RDB-bestand van het gewenste bestand Vgx. server uploaden naar een blob pagina in een opslag Azure Account en vervolgens importeren in uw exemplaar van premium Azure bestand Vgx. Cache. U wilt bijvoorbeeld de gegevens uit de cache productie exporteren en importeren in een cache gebruikt als onderdeel van een testomgeving voor testdoeleinden of migratie. 

### <a name="will-my-cache-be-available-during-an-importexport-operation"></a>Mijn cache is beschikbaar tijdens importeren/exporteren?

-   **Exporteren** - Caches beschikbaar blijven en u kunt doorgaan met het gebruik van de cache tijdens een exportbewerking.
-   **Importeren** - Caches zijn niet beschikbaar wanneer een importbewerking wordt gestart en beschikbaar komen voor gebruik als de importbewerking is voltooid.


### <a name="can-i-use-importexport-with-redis-cluster"></a>Kan ik importeren/exporteren gebruiken bij cluster bestand Vgx.

Ja, en u kunt importeren/exporteren tussen een geclusterde cache en een niet-geclusterde cache. Sinds het bestand Vgx. cluster [ondersteunt alleen database 0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), worden gegevens in databases behalve 0 won't worden geïmporteerd. Wanneer geclusterde cache-gegevens worden geïmporteerd, zijn de toetsen opnieuw verdeeld tussen de shards van het cluster. 

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a>Hoe werkt importeren/exporteren met een aangepaste databases instellen

Sommige prijzen lagen hebben verschillende [databases grenzen](cache-configure.md#databases), er zijn enkele overwegingen bij het importeren als u een aangepaste waarde voor de `databases` instellen tijdens het maken van de cache.

-   Bij het importeren van een prijzen laag met een lagere `databases` limiet dan de laag die u hebt geëxporteerd:
    -   Als u het aantal `databases` 16 voor alle lagen van de prijzen is, geen gegevens verloren zijn gegaan.
    -   Als u een aangepaste aantal `databases` binnen de grenzen die valt voor de laag die u importeren wilt, geen gegevens verloren zijn gegaan.
    -   Als de geëxporteerde gegevens gegevens in een database die groter is dan de grenzen van de nieuwe laag opgenomen, de gegevens van die hogere databases niet geïmporteerd.

### <a name="how-is-importexport-different-from-redis-persistence"></a>Hoe verschilt Import/Export bestand Vgx. persistentie?

Azure Cache bestand Vgx. persistentie kunt u gegevens die zijn opgeslagen in het bestand naar de opslag van Azure Vgx.. Als persistentie is geconfigureerd, blijft een momentopname van de cache bestand Vgx. in een bestand Vgx. binaire indeling naar de schijf op basis van een configureerbare back-upfrequentie bestaan in Azure bestand Vgx. Cache. Als een catastrofale gebeurtenis die zowel de primaire als de replica cache uitschakelt plaatsvindt, wordt de cachegegevens hersteld automatisch met de meest recente snapshot. Zie de [persistentie van gegevens voor een Premium Azure bestand Vgx. Cache configureren](cache-how-to-premium-persistence.md)voor meer informatie.

Importeren / exporteren kunt u gegevens in of exporteren uit een bestand Vgx. Azure-Cache. Deze niet back-up configureren en herstellen met behulp van bestand Vgx. persistentie.


### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a>Kan ik importeren/exporteren met PowerShell CLI of andere clients management automatiseren?

Ja, PowerShell instructies Zie voor [een cache bestand Vgx. importeren](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) en [exporteren van een bestand Vgx. cache](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).



### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a>Ik heb ontvangen een time-outfout tijdens de Import/exportbewerking. Wat betekent het?

Als u op het blad **gegevens importeren** of **exporteren van gegevens** langer dan 15 minuten voordat de bewerking wordt gestart, ontvangt u een foutbericht zoals het volgende.

    The request to import data into cache 'contoso55' failed with status 'error' and error 'One of the SAS URIs provided could not be used for the following reason: The SAS token end time (se) must be at least 1 hour from now and the start time (st), if given, must be at least 15 minutes in the past.

Om dit probleem oplossen, start de import- of exportbewerking vóór 15 minuten zijn verstreken.

### <a name="i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened"></a>Ik krijg een fout bij het exporteren van gegevens naar Azure Blob-opslag. Wat is er gebeurd?

Import/Export werkt alleen met RDB-bestanden die zijn opgeslagen als BLOB's pagina. Andere typen blob worden niet ondersteund op dit moment, met inbegrip van de blob storage rekeningen bij hot en cool lagen.


## <a name="next-steps"></a>Volgende stappen
Leer meer cache premium-functies gebruiken.

-   [Inleiding tot het niveau van de premie van Azure Cache bestand Vgx.](cache-premium-tier-intro.md)    

  
<!-- IMAGES -->
[cache-settings-import-export-menu]: ./media/cache-how-to-import-export-data/cache-settings-import-export-menu.png
[cache-export-data-choose-account]: ./media/cache-how-to-import-export-data/cache-export-data-choose-account.png
[cache-export-data-choose-storage-container]: ./media/cache-how-to-import-export-data/cache-export-data-choose-storage-container.png
[cache-export-data-container]: ./media/cache-how-to-import-export-data/cache-export-data-container.png
[cache-export-data-export-complete]: ./media/cache-how-to-import-export-data/cache-export-data-export-complete.png
[cache-export-data]: ./media/cache-how-to-import-export-data/cache-export-data.png
[cache-import-data]: ./media/cache-how-to-import-export-data/cache-import-data.png
[cache-import-choose-storage-account]: ./media/cache-how-to-import-export-data/cache-import-choose-storage-account.png
[cache-import-choose-container]: ./media/cache-how-to-import-export-data/cache-import-choose-container.png
[cache-import-choose-blobs]: ./media/cache-how-to-import-export-data/cache-import-choose-blobs.png
[cache-import-blobs]: ./media/cache-how-to-import-export-data/cache-import-blobs.png
[cache-import-data-import-complete]: ./media/cache-how-to-import-export-data/cache-import-data-import-complete.png








