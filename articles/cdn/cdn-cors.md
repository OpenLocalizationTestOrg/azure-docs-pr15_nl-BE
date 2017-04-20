<properties
    pageTitle="Met Azure CDN met CORS | Microsoft Azure"
    description="Informatie over de Azure Content Delivery Network (CDN) te gebruiken met Cross-oorsprong Resource Sharing (CORS)."
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
    ms.date="09/30/2016"
    ms.author="casoper"/>
    
# <a name="using-azure-cdn-with-cors"></a>Met behulp van Azure CDN met CORS     

## <a name="what-is-cors"></a>Wat is CORS?

CORS (Cross-oorsprong resources delen) is een HTTP-functie waarmee u een webtoepassing in een domein met toegang tot bronnen in een ander domein. Alle moderne webbrowsers implementeren ter vermindering van de kans op aanvallen via cross-site scripting, een beveiligingsbeperking [dezelfde oorsprong beleid](http://www.w3.org/Security/wiki/Same_Origin_Policy)genoemd.  Hiermee voorkomt u dat een webpagina vanuit de oproepende API's in een ander domein.  CORS biedt een veilige manier om een domein (het domein van oorsprong) API's aanroepen in een ander domein.
 
## <a name="how-it-works"></a>Hoe het werkt
1.  Het OPTIONS-verzoek met een **oorsprong** in de HTTP-header, verzendt de browser. De waarde van deze header is het domein die de bovenliggende pagina. Wanneer u een pagina uit de https://www.contoso.com probeert te krijgen tot de gegevens van een gebruiker in het domein fabrikam.com, zou de volgende verzoek-header worden verzonden naar fabrikam.com: 
    
    `Origin: https://www.contoso.com`
 
2.  De server reageert met het volgende:
    - Een **Access Control-leiden-oorsprong** header in een antwoord dat aangeeft welke sites oorsprong zijn toegestaan. Bijvoorbeeld:
        
        `Access-Control-Allow-Origin: https://www.contoso.com`
        
    - Als de server kan de aanvraag cross oorsprong geen foutpagina
    - Een **Access Control-leiden-oorsprong** -header met een jokerteken, waarmee alle domeinen:
        
        `Access-Control-Allow-Origin: *`
 
Voor complexe HTTP-aanvragen, er is een "preflight" aanvraag eerst gedaan om te bepalen of er toestemming voor de gehele aanvraag verzenden.
 
## <a name="wildcard-or-single-origin-scenarios"></a>Jokerteken of oorsprong enkele scenario 's

CORS van Azure CDN werken automatisch met geen aanvullende configuratie als de kop van het **Access-besturingselement-leiden-oorsprong** is ingesteld op het jokerteken (*) of een enkele oorsprong.  Het eerste antwoord de CDN cache en volgende aanvragen gebruikt dezelfde kop.
 
Als u reeds verzoeken zijn aangebracht in de CDN voor CORS wordt ingesteld op de de oorsprong, moet u inhoud op de inhoud van uw eindpunt te laden de inhoud met de kop van het **Access-besturingselement-leiden-oorsprong** leegmaken.
 
## <a name="multiple-origin-scenarios"></a>Meerdere scenario's van oorsprong

Als u toestaan dat een specifieke lijst van oorsprong wilt moet worden toegestaan voor CORS, krijgen dingen een beetje ingewikkelder. Het probleem treedt op wanneer de CDN de kop **Access Control-leiden-oorsprong** voor de eerste CORS oorsprong slaat.  Wanneer een andere CORS herkomst een volgende aanvraag indient, wordt de CDN betekend de cache **Access Control-leiden-oorsprong** kop niet overeen met.  Er zijn verschillende manieren om dit te corrigeren.
 
### <a name="azure-cdn-premium-from-verizon"></a>Azure CDN premie van Verizon

De beste manier om deze is **Azure CDN premie van Verizon**, waarmee sommige geavanceerde functies gebruiken. 
 
Moet u [een regel](cdn-rules-engine.md) maken om de kop van de **oorsprong** van de aanvraag.  Als het een geldige oorsprong is, wordt de regel de kop van het **Access-besturingselement-leiden-oorsprong** waarvan de oorsprong in de aanvraag verstrekte ingesteld.  Als de oorsprong die zijn opgegeven in de header van **oorsprong** niet is toegestaan, moet de regel de kop **Access Control-leiden-oorsprong** , waardoor de browser om de aanvraag af te wijzen weglaten. 
 
Er zijn twee manieren om dit te doen met de regels-engine.  In beide gevallen wordt de koptekst van de **Access Control-leiden-oorsprong** van de bestandsserver oorsprong volledig wordt genegeerd, volledig beheert de toegestane CORS oorsprongen van het CDN regels-engine.

#### <a name="one-regular-expression-with-all-valid-origins"></a>Een reguliere expressie met alle geldige oorsprong
 
In dit geval maakt u een reguliere expressie waarin alle van de oorsprong die u wilt toestaan: 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$
 
> [AZURE.TIP] [Perl compatibele reguliere expressies](http://pcre.org/) **Azure CDN van Verizon** gebruikt als de engine voor reguliere expressies.  U kunt een hulpprogramma zoals [101 van reguliere expressies](https://regex101.com/) voor het valideren van uw reguliere expressie.  Het teken '/' is geldig in reguliere expressies en hoeft niet te worden voorafgegaan, ontsnappen dat teken wordt beschouwd als de beste en sommige regex validatiefuncties te verwachten is.

Als de reguliere expressie komt overeen met, uw regel worden vervangen door de kop van het **Access-besturingselement-leiden-oorsprong** (indien aanwezig) van de oorsprong bij de oorsprong die de aanvraag heeft verzonden.  U kunt ook extra kopteksten van CORS, zoals **Access-besturingselement-leiden-methoden**toevoegen.

![Voorbeeld van de regels met de reguliere expressie](./media/cdn-cors/cdn-cors-regex.png)
 
#### <a name="request-header-rule-for-each-origin"></a>Verzoek-header regel voor elke oorsprong.

U kunt in plaats daarvan een afzonderlijke regel voor elke oorsprong die u wilt toestaan via de **Request Header jokertekens** [overeenkomt met de voorwaarde](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1)maken in plaats van reguliere expressies. Net als bij de methode van de reguliere expressie, wordt de motor regels de headers CORS. 
  
![Voorbeeld van de regels zonder de reguliere expressie](./media/cdn-cors/cdn-cors-no-regex.png)

> [AZURE.TIP] In het bovenstaande voorbeeld wordt het gebruik van het jokerteken * vertelt de regels overeenkomt met de HTTP- en HTTPS-engine.
 
### <a name="azure-cdn-standard"></a>Azure CDN-standaard

Op Azure CDN standaard profielen is het enige mechanisme voor meerdere oorsprong zonder gebruik te maken van de oorsprong van het jokerteken met [caching van de query-tekenreeks](cdn-query-string.md).  U moet de query string is ingeschakeld voor het eindpunt van de CDN en vervolgens met een unieke query-tekenreeks voor het aanvragen van toegestane domeinen. Dit resulteert in de cache van een afzonderlijk object voor elke unieke query-tekenreeks CDN. Deze aanpak is echter niet ideaal, omdat hierdoor meerdere exemplaren van hetzelfde bestand in de cache op de CDN.  

