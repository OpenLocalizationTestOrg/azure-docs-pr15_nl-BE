<properties
    pageTitle="Het oplossen van 404 status retourneren Azure CDN-eindpunten | Microsoft Azure"
    description="Problemen oplossen met 404 responscodes met Azure CDN eindpunten."
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
    
# <a name="troubleshooting-cdn-endpoints-returning-404-statuses"></a>Het oplossen van CDN eindpunten 404 status opvragen

Dit artikel helpt u bij het oplossen van problemen met 404-fouten retourneren voor [CDN eindpunten](cdn-create-new-endpoint.md) .

Als u meer Help-informatie op elk punt in dit artikel, kunt u contact opnemen met de Azure experts op [de MSDN Azure en de forums stackoverloop](https://azure.microsoft.com/support/forums/). Ook kunt u ook een incident voor ondersteuning van Azure bestand. Ga naar de [website voor ondersteuning van Azure](https://azure.microsoft.com/support/options/) en klik op **Ondersteuning krijgen**.

## <a name="symptom"></a>Symptoom

U hebt gemaakt met een CDN-profiel en een eindpunt, maar de inhoud niet lijkt te zijn beschikbaar op de CDN.  Gebruikers die proberen toegang te krijgen tot de inhoud via de URL CDN ontvangen 404 HTTP statuscodes. 

## <a name="cause"></a>Oorzaak

Er zijn verschillende mogelijke oorzaken, waaronder:

- De oorsprong van het bestand niet zichtbaar voor de CDN
- Het eindpunt is onjuist geconfigureerd, waardoor de CDN te zoeken op de verkeerde plaats
- De host is de host-header van de CDN weigeren
- Het eindpunt nog niet heeft doorgegeven in de CDN

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing

> [AZURE.IMPORTANT] Na het maken van een eindpunt CDN, zal ze niet onmiddellijk beschikbaar zijn voor gebruik, als het duurt om de inschrijving doorgeven via de CDN.  Voor <b>Azure CDN van Akamai</b> profielen voltooit het doorgeven van meestal binnen één minuut.  <b>Azure CDN van Verizon</b> profielen, het doorgeven van meestal voltooit binnen 90 minuten, maar in sommige gevallen kan langer duren.  Als u de stappen in dit document en u nog steeds 404 antwoorden krijgt, kunt u nog een paar uur om te controleren voordat u opnieuw een support ticket openen.

### <a name="check-the-origin-file"></a>Controleer het bestand oorsprong

We moeten Controleer eerst de die het bestand we in de cache willen beschikbaar is op onze oorsprong en is toegankelijk voor het publiek.  De snelste manier om dat te doen is een browser in een In privé- of Incognito-sessie openen en blader naar het bestand rechtstreeks.  Alleen Typ of plak de URL in het adresvak en als die in het bestand dat u verwacht dat het resultaat zien.  In dit voorbeeld ga ik gebruik van een bestand dat ik heb in een opslag Azure account toegankelijk op `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`.  Zoals u ziet, wordt de test is doorgegeven.

![Succes!](./media/cdn-troubleshoot-endpoint/cdn-origin-file.png)

> [AZURE.WARNING] Dit is de snelste en eenvoudigste manier om te controleren of dat het bestand is openbaar, kunnen bepaalde netwerkconfiguraties in uw organisatie bieden u de illusie dat dit bestand openbaar is wanneer het is in feite alleen zichtbaar voor gebruikers van uw netwerk (ook als deze wordt gehost in Azure).  Als u een externe browser waaruit u testen kunt, zoals een mobiel apparaat dat niet is verbonden met het netwerk van uw organisatie of een virtuele machine in Azure, zou die worden aanbevolen.

### <a name="check-the-origin-settings"></a>Controleer de instellingen van de oorsprong

Nu dat het bestand is openbaar beschikbaar op het internet hebt gecontroleerd, moeten we onze oorsprong-instellingen te controleren.  Ga naar uw profiel CDN in [Azure Portal](https://portal.azure.com)en klikt u op het eindpunt dat u wilt oplossen.  Klik in de resulterende **eindpunt** blade, de oorsprong.  

![Eindpunt-blade met oorsprong gemarkeerd](./media/cdn-troubleshoot-endpoint/cdn-endpoint.png)

Het blad van de **oorsprong** wordt weergegeven. 

![Oorsprong blade](./media/cdn-troubleshoot-endpoint/cdn-origin-settings.png)

#### <a name="origin-type-and-hostname"></a>Type van de oorsprong en de hostnaam

Controleer of dat het **type van oorsprong** juist is en controleer of de **hostnaam van de oorsprong**.  In mijn voorbeeld, `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, het gedeelte van de hostnaam van de URL is `cdndocdemo.blob.core.windows.net`.  Zoals u in het screenshot ziet, dit klopt.  Het veld **hostnaam oorsprong** is voor opslag van Azure Web App en Cloud Service oorsprong, een vervolgkeuzelijst, zodat we niet hoeven te hoeven maken over het juiste spelling.  Als u een aangepaste oorsprong, is het echter *absoluut kritiek belang* die de hostnaam correct is gespeld.

#### <a name="http-and-https-ports"></a>HTTP- en HTTPS-poorten

Verder is het handig om te controleren, hier is uw **http-** en **HTTPS-poorten**.  In de meeste gevallen 80 en 443 juist zijn en u geen wijzigingen nodig.  Echter, als de oorspronkelijke server op een andere poort luistert, die moet worden hier weergegeven.  Als u niet zeker weet, kijk eens naar de URL voor het bestand oorsprong.  De specificaties van HTTP- en HTTPS opgeven-poorten 80 en 443 als de standaardwaarden. In de URL, `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, een poort niet wordt opgegeven, wordt uitgegaan van de standaardwaarde van 443 en de instellingen zijn correct.  

Echter, zegt de URL voor de oorsprong-bestand dat u eerder hebt getest is `http://www.contoso.com:8080/file.txt`.  Opmerking de `:8080` aan het einde van het segment van de hostnaam.  Dat vertelt de browser voor gebruik van poort `8080` verbinding maken met de webserver op `www.contoso.com`, dus u moet 8080 invoeren in het veld **HTTP-poort** .  Het is belangrijk te weten dat deze poortinstellingen alleen van invloed op welke poort het eindpunt wordt gebruikt om informatie te halen uit de oorsprong.

> [AZURE.NOTE] Het volledige bereik van TCP-poort voor oorsprong toegestaan **Azure CDN van Akamai** eindpunten niet.  Zie voor een lijst met poorten van oorsprong die niet zijn toegestaan, [Azure CDN van Akamai toegestane oorsprong poorten](https://msdn.microsoft.com/library/mt757337.aspx).  
  
### <a name="check-the-endpoint-settings"></a>Controleer de instellingen voor eindpunt

Terug op het **eindpunt** blade, klikt u op de knop **configureren** .

![Blade eindpunt met de knop configureren is gemarkeerd](./media/cdn-troubleshoot-endpoint/cdn-endpoint-configure-button.png)

**Configureren** -blade van het endpoint wordt weergegeven.

![Blade configureren](./media/cdn-troubleshoot-endpoint/cdn-configure.png)

#### <a name="protocols"></a>Protocollen

Voor **protocollen**, Controleer of het protocol dat wordt gebruikt door de clients wordt geselecteerd.  Hetzelfde protocol dat door de client gebruikt zal worden gebruikt voor toegang tot de oorsprong, dus het is belangrijk dat de poorten zijn oorsprong juist geconfigureerd in de vorige sectie.  Het eindpunt wordt alleen op de standaard HTTP- en HTTPS-poorten (80 en 443), ongeacht de oorsprong poorten luistert.

Laten we terug naar onze hypothetisch voorbeeld met `http://www.contoso.com:8080/file.txt`.  Als u onthouden wilt, Contoso opgegeven `8080` als de HTTP-poort, maar ook eens deze opgegeven `44300` als de HTTPS-poort.  Als deze een eindpunt met de naam gemaakt `contoso`, hun hostnaam CDN-eindpunt zou zijn `contoso.azureedge.net`.  Een aanvraag voor `http://contoso.azureedge.net/file.txt` is een HTTP-aanvraag, waarmee het eindpunt zou HTTP op poort 8080 te halen vanaf de oorsprong.  Beveiligde aanvragen via HTTPS, `https://contoso.azureedge.net/file.txt`, het eindpunt te gebruiken HTTPS op poort 44300 zou veroorzaken wanneer het bestand vanaf de oorsprong bij het ophalen.

#### <a name="origin-host-header"></a>Oorsprong host-header

De **host-header van oorsprong** is de waarde van de host-header verzonden naar de oorsprong bij elke aanvraag.  In de meeste gevallen moet dit gelijk zijn aan de **oorsprong hostname** we eerder gecontroleerd.  Een onjuiste waarde in dit veld niet in het algemeen 404 statussen toe leiden dat, maar het is waarschijnlijk een andere statussen 4xx, afhankelijk van wat de oorsprong verwacht.

#### <a name="origin-path"></a>Pad van oorsprong

Ten slotte moeten we onze **oorsprong pad**verifiëren.  Dit is standaard leeg.  U moet dit veld alleen gebruiken als u wilt dat de oorsprong gehost bronnen wilt beschikbaar maken op het CDN isoleren.  

Bijvoorbeeld in mijn eindpunt wilde ik alle resources op mijn account opslag beschikbaar te zijn, zodat ik leeg **pad van oorsprong** .  Dit betekent dat een verzoek tot `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` resulteert in een verbinding van mijn eindpunt op `cdndocdemo.core.windows.net` aanvragen `/publicblob/lorem.txt`.  Ook een aanvraag voor `https://cdndocdemo.azureedge.net/donotcache/status.png` leidt tot het eindpunt aanvragen `/donotcache/status.png` van de oorsprong.

Maar wat gebeurt er als ik niet nodig de CDN voor elk pad op mijn oorsprong?  Zeggen I wilde slechts tonen de `publicblob` pad.  Als ik mijn veld **oorsprong pad** */publicblob* , waardoor het eindpunt te */publicblob* voor elke aanvraag gedaan om de oorsprong te plaatsen.  Betekent dit dat de aanvraag voor `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` nu daadwerkelijk gaat het verzoek deel van de URL, `/publicblob/lorem.txt`, en `/publicblob` naar het begin. Dit resulteert in een aanvraag voor `/publicblob/publicblob/lorem.txt` van de oorsprong.  Als dat het pad naar een bestand dat niet wordt opgelost, wordt de oorsprong status 404 geretourneerd.  De juiste URL voor het ophalen van lorem.txt in dit voorbeeld zou eigenlijk zijn `https://cdndocdemo.azureedge.net/lorem.txt`.  Opmerking dat wij niet het pad */publicblob* , opnemen omdat het gedeelte van de aanvraag van de URL is `/lorem.txt` en het eindpunt wordt toegevoegd `/publicblob`, hetgeen in `/publicblob/lorem.txt` wordt de aanvraag doorgegeven aan de oorsprong.
