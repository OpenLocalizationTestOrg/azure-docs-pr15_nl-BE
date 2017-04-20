<properties
    pageTitle="De prestaties verbeteren door het comprimeren van bestanden in Azure CDN | Microsoft Azure"
    description="Meer informatie over het bestandsoverdracht snelheid verbeteren en verhoogt de prestaties van de pagina laden door het comprimeren van bestanden in Azure CDN."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="improve-performance-by-compressing-files"></a>De prestaties verbeteren door bestanden te comprimeren

Compressie is een eenvoudige en effectieve methode voor het bestandsoverdracht snelheid verbeteren en de pagina laden prestaties verbeteren door de bestandsgrootte te verkleinen voordat deze wordt verzonden vanaf de server. Het verlaagt de bandbreedtekosten en biedt een meer responsieve ervaring voor uw gebruikers.

Er zijn twee manieren compressie inschakelen:

- U kunt compressie inschakelen op de bronserver, waarin de CDN zal passeren de gecomprimeerde bestanden en gecomprimeerde bestanden leveren aan clients die deze aanvragen.
- U kunt compressie rechtstreeks op CDN servers aan de periferie, waarin de CDN zullen de bestanden comprimeren en dienen aan eindgebruikers, zelfs als ze worden niet gecomprimeerd door de originele server inschakelen.

> [AZURE.IMPORTANT] CDN configuratiewijzigingen doorvoeren via het netwerk enige tijd.  Voor <b>Azure CDN van Akamai</b> profielen voltooit het doorgeven van meestal onder een minuut.  Voor <b>Azure CDN van Verizon</b> profielen ziet u meestal uw wijzigingen toepassen binnen 90 minuten.  Als dit de eerste keer dat u compressie hebt ingesteld voor uw eindpunt CDN, u moet rekening houden met 1 tot 2 uur wachten om er zeker van te zijn dat de compressie instellingen zijn doorgegeven aan de POP's voor het oplossen van problemen

## <a name="enabling-compression"></a>Compressie inschakelen

> [AZURE.NOTE] De Standard en Premium CDN niveaus bieden dezelfde functionaliteit compressie, maar verschilt van de gebruikersinterface.  Zie voor meer informatie over de verschillen tussen de Standard en Premium CDN lagen [Azure CDN-overzicht](cdn-overview.md).

### <a name="standard-tier"></a>Standaard laag

> [AZURE.NOTE] Deze sectie is van toepassing op **Azure CDN standaard van Verizon** - en **Azure CDN van Akamai** profielen.

1. Klik op het CDN eindpunt dat u wilt beheren vanuit de bladeserver CDN-profiel.

    ![CDN profiel blade eindpunten](./media/cdn-file-compression/cdn-endpoints.png)

    Hiermee opent u het eindpunt CDN blade.

2. Klik op de knop **configureren** .

    ![Knop CDN profiel blade beheren](./media/cdn-file-compression/cdn-config-btn.png)

    Hiermee opent u de configuratie van CDN blade.

3. **Compressie**inschakelen.

    ![Compressieopties CDN](./media/cdn-file-compression/cdn-compress-standard.png)

4. De standaardtypen gebruiken of de lijst wijzigen door te verwijderen of toe te voegen bestandstypen.
    
    > [AZURE.TIP] Mogelijk is, terwijl het niet verdient compressie toepassen op gecomprimeerde indelingen, zoals ZIP, MP3, MP4, JPG, enz.
    
5. Wanneer u klaar bent, klikt u op de knop **Opslaan** .

### <a name="premium-tier"></a>Premium-laag

> [AZURE.NOTE] Deze sectie is van toepassing op **Azure CDN premie van Verizon** profielen.

1. Klik op de knop **beheren** van de bladeserver CDN-profiel.

    ![Knop CDN profiel blade beheren](./media/cdn-file-compression/cdn-manage-btn.png)

    De portal CDN management wordt geopend.

2. Wijs het tabblad **Http-groot** en vervolgens zweven over het doel van de **Cache-instellingen** .  Klik op **compressie**.

    Compressieopties weergegeven.

    ![Bestandscompressie](./media/cdn-file-compression/cdn-compress-files.png)

3. Compressie inschakelen door te klikken op het keuzerondje **Compressie ingeschakeld** .  Voer de MIME-typen die u wilt comprimeren als door komma's gescheiden lijst (zonder spaties) in het tekstvak **Typen** .
        
    > [AZURE.TIP] Mogelijk is, terwijl het niet verdient compressie toepassen op gecomprimeerde indelingen, zoals ZIP, MP3, MP4, JPG, enz. 

4. Wanneer u klaar bent, klikt u op de knop **bijwerken** .


## <a name="compression-rules"></a>Compressie-regels

Deze tabellen Azure CDN compressie gedrag voor elk scenario wordt beschreven.

> [AZURE.IMPORTANT] Voor **Azure CDN van Verizon** (Standard en Premium), zijn alleen in aanmerking komende bestanden gecomprimeerd.  Als u in aanmerking voor compressie, moet een bestand:
>
> - Groter zijn dan 128 bytes.
> - Kleiner zijn dan 1 MB.
> 
> Voor **CDN van Akamai Azure**, alle bestanden in aanmerking komen voor compressie.
>
> Voor alle producten van Azure CDN moet een bestand een MIME-type dat [geconfigureerd voor compressie is](#enabling-compression).
>
> **Azure CDN van Verizon** profielen (Standard en Premium) ondersteuning **gzip**, **verkleinen**of **bzip2** -codering.  **Azure CDN van Akamai** profielen ondersteunen alleen de **gzip** -codering.
>
> **Gzip** gecodeerde bestanden aanvragen **Azure CDN van Akamai** eindpunten altijd bij de oorsprong, ongeacht de aanvraag van de client.

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a>Compressie is uitgeschakeld of het bestand is niet in aanmerking voor compressie

|De client vraagt een indeling (via de Accept-Encoding-header)|Cache-bestandsindeling|CDN antwoord naar de client|Notities|
|----------------|-----------|------------|-----|
|Gecomprimeerd|Gecomprimeerd|Gecomprimeerd|   |
|Gecomprimeerd|Niet-gecomprimeerd|Niet-gecomprimeerd|    | 
|Gecomprimeerd|Niet in cache|Wel of niet gecomprimeerd|Afhankelijk is van oorsprong antwoord|
|Niet-gecomprimeerd|Gecomprimeerd|Niet-gecomprimeerd|    |
|Niet-gecomprimeerd|Niet-gecomprimeerd|Niet-gecomprimeerd|    |   
|Niet-gecomprimeerd|Niet in cache|Niet-gecomprimeerd|     |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a>Compressie is ingeschakeld en het bestand komt in aanmerking voor compressie

|De client vraagt een indeling (via de Accept-Encoding-header)|Cache-bestandsindeling|CDN antwoord naar de client|Notities|
|----------------|-----------|------------|-----|
|Gecomprimeerd|Gecomprimeerd|Gecomprimeerd|CDN transcodes tussen ondersteunde indelingen|
|Gecomprimeerd|Niet-gecomprimeerd|Gecomprimeerd|CDN compressie wordt uitgevoerd|
|Gecomprimeerd|Niet in cache|Gecomprimeerd|CDN uitvoert compressie als oorsprong gedecomprimeerd wordt.  **Azure CDN van Verizon** wordt het niet-gecomprimeerde bestand op het eerste verzoek doorgeven en vervolgens laten comprimeren en het bestand voor de volgende aanvragen in de cache.  Bestanden met `Cache-Control: no-cache` kop nooit worden gecomprimeerd. 
|Niet-gecomprimeerd|Gecomprimeerd|Niet-gecomprimeerd|CDN decompressie wordt uitgevoerd|
|Niet-gecomprimeerd|Niet-gecomprimeerd|Niet-gecomprimeerd|     |  
|Niet-gecomprimeerd|Niet in cache|Niet-gecomprimeerd|     |

## <a name="media-services-cdn-compression"></a>Media Services CDN-compressie

Voor Media Services CDN ingeschakeld streaming eindpunten, compressie is standaard ingeschakeld voor de volgende inhoudstypen: application/vnd.ms-sstr + xml, application/dash+xml,application/vnd.apple.mpegurl, application/f4m + xml. U kan niet in-/ uitschakelen compressie van de genoemde typen met behulp van de portal Azure.  

## <a name="see-also"></a>Zie ook
- [Bestandscompressie CDN oplossen](cdn-troubleshoot-compression.md)    
