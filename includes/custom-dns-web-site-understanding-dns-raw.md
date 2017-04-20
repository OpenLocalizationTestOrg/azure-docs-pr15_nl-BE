Domain Name System (DNS) wordt gebruikt om te zoeken naar bronnen op het internet. Bijvoorbeeld wanneer u een web app-adres in uw browser invoeren of klik op een koppeling op een webpagina, gebruikt het DNS voor het domein vertalen naar een IP-adres. Het IP-adres is ongeveer zoals een straat, maar is niet erg menselijke friendly. Bijvoorbeeld, is het veel gemakkelijker te onthouden van een DNS-naam zoals **contoso.com** dan te onthouden IP-adres zoals 192.168.1.88 of 2001:0:4137:1f67:24a2:3888:9cce:fea3.

Het DNS-systeem is gebaseerd op *records*. Een specifieke *naam*, bijvoorbeeld **contoso.com**, koppelen records aan een IP-adres of een andere DNS-naam. Wanneer een toepassing, zoals een webbrowser, een naam in DNS opgezocht, wordt de record wordt gevonden en wat verwijst naar het adres gebruikt. Als de waarde naar verwijst een IP-adres is, wordt die waarde gebruiken in de browser. Als het verwijst naar een andere DNS-naam, heeft de toepassing opnieuw resolutie doen. Alle naamomzetting eindigt uiteindelijk in een IP-adres.

Wanneer u een web app in de App-Service maakt, wordt automatisch een DNS-naam toegewezen aan het web app. Deze naam heeft de vorm van ** &lt;yourwebappname&gt;. azurewebsites.net**. Er is ook een virtueel IP-adres beschikbaar is voor gebruik bij het maken van DNS-records, zodat u records die wijzen maken kunt naar de **. azurewebsites.net**, of kunt u verwijzen naar het IP-adres.

> [AZURE.NOTE] Het IP-adres van uw web app verandert als u verwijderen en opnieuw maken van uw web app of de App Service plan modus op **vrij** wijzigen nadat deze is ingesteld op **standaard**, **gedeeld**of **standaard**.

Ook zijn er meerdere typen records, elk met hun eigen functies en beperkingen, maar voor web apps alleen belangrijk voor ons twee, *A* - en *CNAME* -records.

###<a name="address-record-a-record"></a>Adresrecord (record)

Een A-record koppelt een domein, bijvoorbeeld **contoso.com** of **www.contoso.com**, *of in een jokertekendomein* , zoals ** \*. contoso.com**, een IP-adres. In het geval van een web app in de App-Service, het virtuele IP van de service of een specifiek IP-adres het adres die u hebt aangeschaft voor uw web app.

De belangrijkste voordelen van een A-record via een CNAME-record zijn:

* U kunt een hoofddomein bijvoorbeeld **contoso.com** toewijzen aan een IP-adres; veel registrators alleen toestaan deze met behulp van een records

* U kunt hebt die gebruikmaakt van een jokerteken, zoals ** \*. contoso.com**, die zou verwerken van aanvragen voor meerdere subdomeinen, zoals **mail.contoso.com**, **blogs.contoso.com**of **www.contso.com**.

> [AZURE.NOTE] Aangezien een A-record is toegewezen aan een statisch IP-adres, kan het automatisch omzetten in wijzigingen het IP-adres van uw web app. Een IP-adres voor gebruik met een records weergegeven wanneer u aangepaste domein naam instellingen voor uw web app configureren; Deze waarde kan echter wijzigen als u verwijderen en opnieuw maken van uw web app of de modus App Service plan maken op **gratis**.

###<a name="alias-record-cname-record"></a>Record alias (CNAME-record)

*Specifieke* DNS-naam **mail.contoso.com** of **www.contoso.com**, koppelt een CNAME-record aan een andere (canoniek) domeinnaam. In het geval van App Service Web Apps de canonieke naam wordt de ** &lt;yourwebappname >. azurewebsites.net** de domeinnaam van uw web app. Eenmaal gemaakt, de CNAME een alias gemaakt voor de ** &lt;yourwebappname >. azurewebsites.net** domeinnaam. De CNAME-post wordt omgezet in het IP-adres van uw ** &lt;yourwebappname >. azurewebsites.net** domeinnaam automatisch, zodat als het IP-adres van de web-app wordt gewijzigd, hoeft u geen actie te ondernemen.

> [AZURE.NOTE] Sommige registratieservices domein alleen kunnen u subdomeinen toewijzen wanneer u een CNAME-record, bijvoorbeeld **www.contoso.com**en geen basis-namen, zoals **contoso.com**. Zie de documentatie bij uw registratieservice, <a href="http://en.wikipedia.org/wiki/CNAME_record">de Wikipedia-post op de CNAME-record</a>of het <a href="http://tools.ietf.org/html/rfc1035">IETF-domeinnamen - implementatie en specificatie</a> document voor meer informatie over CNAME-records.

###<a name="web-app-dns-specifics"></a>Web app-specifieke DNS-informatie

Met Web Apps met behulp van een A-record, moet u eerst een van de volgende TXT-records maken:

* **Voor het hoofddomein** - A DNS TXT record van **@** te ** &lt;yourwebappname&gt;. azurewebsites.net**.

* **Voor een bepaald subdomein** - een DNS-naam van ** &lt;subdomein >** naar ** &lt;yourwebappname&gt;. azurewebsites.net**. Bijvoorbeeld **blogs** als de A-record voor **blogs.contoso.com**.

* **Voor het jokerteken sub-dodmains** - A DNS TXT record van *** naar ** &lt;yourwebappname&gt;. azurewebsites.net**.

Deze TXT-record wordt gebruikt om te verifiëren dat u eigenaar bent van het domein dat u probeert te gebruiken. Dit is niet alleen een A-record aan te wijzen aan het virtuele IP-adres van uw web app maken.

Vindt u het IP-adres en **. azurewebsites.net** namen voor uw web app door de volgende stappen uit te voeren:

1. Open in de browser de [Azure-Portal](https://portal.azure.com).

2. Klik op de naam van uw web app in de blade **Web Apps** en selecteer **Custom domains** vanaf de onderkant van de pagina.

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

3. In het blad **Custom domains** ziet u het virtuele IP-adres. Deze informatie, zoals het wordt gebruikt bij het maken van DNS-records opslaan

    ![](./media/custom-dns-web-site/virtual-ip-address.png)

    > [AZURE.NOTE] U aangepaste domeinnamen met een **gratis** web-app niet gebruiken en moet het plan App Service upgraden naar **gedeelde**, **Basic**, **Standard**of **Premium** laag. Prijzen voor meer informatie over de App serviceplan van lagen, zoals het wijzigen van de prijzen laag van uw web app, hoe [web apps schaal](../articles/web-sites-scale.md).
