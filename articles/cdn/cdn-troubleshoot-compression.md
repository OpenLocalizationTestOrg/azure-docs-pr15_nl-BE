<properties
    pageTitle="Bestandscompressie in Azure CDN Troubleshooting | Microsoft Azure"
    description="Problemen oplossen met Azure CDN bestandscompressie."
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
    ms.date="09/01/2016"
    ms.author="casoper"/>
    
# <a name="troubleshooting-cdn-file-compression"></a>Bestandscompressie CDN oplossen

Dit artikel helpt u bij het oplossen van problemen met [CDN bestandscompressie](cdn-improve-performance.md).

Als u meer Help-informatie op elk punt in dit artikel, kunt u contact opnemen met de Azure experts op [de MSDN Azure en de forums stackoverloop](https://azure.microsoft.com/support/forums/). Ook kunt u ook een incident voor ondersteuning van Azure bestand. Ga naar de [website voor ondersteuning van Azure](https://azure.microsoft.com/support/options/) en op **Ondersteuning krijgen**.

## <a name="symptom"></a>Symptoom

Compressie voor het eindpunt is ingeschakeld, maar de bestanden gedecomprimeerd worden geretourneerd.

>[AZURE.TIP] Om te controleren dat of de bestanden zijn gecomprimeerd worden geretourneerd, moet u een hulpprogramma als [Fiddler](http://www.telerik.com/fiddler) of van uw browser [ontwikkelprogramma's](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/)gebruiken.  Controle van de HTTP-antwoordheaders geretourneerd met de cache CDN inhoud.  Als er een header met de naam is `Content-Encoding` met een waarde van het **type gzip**, **bzip2**of **verkleinen**, de inhoud wordt gecomprimeerd.
>
>![Content-Encoding-header](./media/cdn-troubleshoot-compression/cdn-content-header.png)

## <a name="cause"></a>Oorzaak

Er zijn verschillende mogelijke oorzaken, waaronder:

- De aangevraagde inhoud komt niet in aanmerking voor compressie.
- Compressie is niet ingeschakeld voor het opgegeven bestandstype.
- De HTTP-aanvraag bevat geen koptekst die het compressietype voor een geldige aanvragen.

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing

> [AZURE.TIP] Net als bij het implementeren van nieuwe eindpunten, configuratiewijzigingen CDN doorvoeren via het netwerk enige tijd.  Meestal worden toegepast binnen 90 minuten.  Als dit de eerste keer dat u compressie hebt ingesteld voor uw eindpunt CDN, kunt u overwegen 1 tot 2 uur wachten om er zeker van te zijn dat de instellingen zijn doorgegeven aan de POP's compressie. 

### <a name="verify-the-request"></a>Controleer of de aanvraag

Eerst doen we een snelle controle van de aanvraag.  Van uw browser [ontwikkelprogramma's](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) kunt u de aanvragen worden weergeven.

- Controleer of de aanvraag wordt verzonden naar de URL van het eindpunt, `<endpointname>.azureedge.net`, en niet de oorsprong.
- Controleer of de aanvraag bevat een **Accept-Encoding** -header en de waarde van die header **gzip**, **verkleinen**of **bzip2**.

> [AZURE.NOTE] **Azure CDN van Akamai** profielen ondersteunen alleen de **gzip** -codering.

![Aanvraagheaders CDN](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profile"></a>Controleer of de compressie-instellingen (profiel standaard CDN)

> [AZURE.NOTE] Deze stap is alleen van toepassing als uw CDN-profiel een profiel **Azure CDN standaard van Verizon** of **Azure CDN standaard van Akamai is** . 

Ga naar het eindpunt in [Azure portal](https://portal.azure.com) en klik op de knop **configureren** .

- Controleer of de compressie is ingeschakeld.
- Controleer of het MIME-type voor de inhoud wordt gecomprimeerd is opgenomen in de lijst met gecomprimeerde indelingen.

![CDN compressie-instellingen](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profile"></a>Controleer of de compressie-instellingen (Premium CDN-profiel)

> [AZURE.NOTE] Deze stap is alleen van toepassing als uw CDN-profiel een profiel **Azure CDN premie van Verizon is** .

Ga naar het eindpunt in [Azure portal](https://portal.azure.com) en klik op de knop **beheren** .  De aanvullende portal wordt geopend.  Wijs het tabblad **Http-groot** en vervolgens zweven over het doel van de **Cache-instellingen** .  Klik op **compressie**. 

- Controleer of de compressie is ingeschakeld.
- Controleer of dat de lijst **Bestandstypen** bevat een lijst met door komma's gescheiden (geen spaties) van MIME-typen.
- Controleer of het MIME-type voor de inhoud wordt gecomprimeerd is opgenomen in de lijst met gecomprimeerde indelingen.

![CDN premium compressie-instellingen](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-the-content-is-cached"></a>Controleer of dat de inhoud in de cache is opgeslagen

> [AZURE.NOTE] Deze stap is alleen van toepassing als uw CDN-profiel een profiel **Azure CDN van Verizon** (Standard of Premium is).

Met behulp van uw browser ontwikkelprogramma's, Controleer de antwoordheaders zodat het bestand wordt opgeslagen in de regio waar deze wordt aangevraagd.

- Controleer of de **Server** response-header.  De kop moet de indeling **Platform (POP-Server-ID)**, zoals in het volgende voorbeeld.
- Controleer of de **X-Cache** response-header.  De kop dient **Druk**te lezen.  

![CDN antwoordheaders](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-the-file-meets-the-size-requirements"></a>Controleer of dat het bestand voldoet aan de grootte

> [AZURE.NOTE] Deze stap is alleen van toepassing als uw CDN-profiel een profiel **Azure CDN van Verizon** (Standard of Premium is).

Als u in aanmerking voor compressie, moet een bestand voldoen aan volgende grootte:

- Groter zijn dan 128 bytes.
- Kleiner dan 1 MB.

### <a name="check-the-request-at-the-origin-server-for-a-via-header"></a>Controleer het verzoek op de oorspronkelijke server op een kop **Via**

De **Via** HTTP-header geeft de webserver de aanvraag wordt door een proxy-server worden doorgegeven.  Microsoft IIS-webservers standaard comprimeren niet antwoorden wanneer de aanvraag een kop **Via bevat** .  Ga als volgt te werk om dit gedrag negeren:

- **IIS 6**: [HcNoCompressionForProxies instellen = "FALSE" in de IIS-Metabase-eigenschappen](https://msdn.microsoft.com/library/ms525390.aspx)
- **IIS 7 en hoger**: [ **noCompressionForHttp10** en de **noCompressionForProxies** op False in de configuratie van de server instellen](http://www.iis.net/configreference/system.webserver/httpcompression)

