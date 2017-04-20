Domain Name System (DNS) wordt gebruikt om te zoeken naar dingen op het internet. Bijvoorbeeld wanneer u een adres in uw browser invoeren of klik op een koppeling op een webpagina, gebruikt het DNS voor het domein vertalen naar een IP-adres. Het IP-adres is ongeveer zoals een straat, maar is niet erg menselijke friendly. Bijvoorbeeld, is het veel gemakkelijker te onthouden van een DNS-naam zoals **contoso.com** dan te onthouden IP-adres zoals 192.168.1.88 of 2001:0:4137:1f67:24a2:3888:9cce:fea3.

Het DNS-systeem is gebaseerd op *records*. Een specifieke *naam*, bijvoorbeeld **contoso.com**, koppelen records aan een IP-adres of een andere DNS-naam. Wanneer een toepassing, zoals een webbrowser, een naam in DNS opgezocht, wordt de record wordt gevonden en wat verwijst naar het adres gebruikt. Als de waarde naar verwijst een IP-adres is, wordt die waarde gebruiken in de browser. Als het verwijst naar een andere DNS-naam, heeft de toepassing opnieuw resolutie doen. Alle naamomzetting eindigt uiteindelijk in een IP-adres.

Wanneer u een Website Azure maakt, wordt automatisch een DNS-naam toegewezen aan de site. Deze naam heeft de vorm van ** &lt;yoursitename&gt;. azurewebsites.net**. Als u uw website als een eindpunt Azure verkeer Manager toevoegt, uw website is toegankelijk via de ** &lt;yourtrafficmanagerprofile&gt;. trafficmanager.net** domein.

> [AZURE.NOTE] Als uw website is geconfigureerd als een eindpunt van het beheer van verkeer, gebruikt u de **. trafficmanager.net** adres bij het maken van DNS-records.

> U kunt alleen gebruiken CNAME-records met verkeer Manager

Ook zijn er meerdere typen records, elk met hun eigen functies en beperkingen, maar voor websites die zijn geconfigureerd als Manager verkeer eindpunten, alleen belangrijk over een bepaald; *CNAME-* records.

###<a name="cname-or-alias-record"></a>Record of een CNAME-Alias

*Specifieke* DNS-naam **mail.contoso.com** of **www.contoso.com**, koppelt een CNAME-record aan een andere (canoniek) domeinnaam. Azure Websites met behulp van beheer van netwerkverkeer en de canonieke naam is de ** &lt;Mijntoep >. trafficmanager.net** -naam van uw serviceprofiel verkeer. Eenmaal gemaakt, de CNAME een alias gemaakt voor de ** &lt;Mijntoep >. trafficmanager.net** domeinnaam. De CNAME-post wordt omgezet in het IP-adres van uw ** &lt;Mijntoep >. trafficmanager.net** domeinnaam automatisch, zodat als het IP-adres van de website wijzigt, hoeft u geen actie te ondernemen.

Wanneer netwerkverkeer Manager op het verkeer binnenkomt, stuurt het verkeer vervolgens naar uw website met behulp van de methode die is geconfigureerd voor voor taakverdeling. Dit is volledig transparant voor de bezoekers van uw website. Zij zien alleen de aangepaste domeinnaam in hun browser.

> [AZURE.NOTE] Sommige registratieservices domein alleen kunnen u subdomeinen toewijzen wanneer u een CNAME-record, bijvoorbeeld **www.contoso.com**en geen basis-namen, zoals **contoso.com**. Zie de documentatie bij uw registratieservice, <a href="http://en.wikipedia.org/wiki/CNAME_record">de Wikipedia-post op de CNAME-record</a>of het <a href="http://tools.ietf.org/html/rfc1035">IETF-domeinnamen - implementatie en specificatie</a> document voor meer informatie over CNAME-records.
