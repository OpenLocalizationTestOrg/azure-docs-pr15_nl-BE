<properties
     pageTitle="Azure Content Delivery Network (CDN) inhoud toewijzen aan een aangepast domein | Microsoft Azure"
     description="In dit onderwerp wordt beschreven hoe een CDN inhoud toewijzen aan een aangepaste domein."
     services="cdn"
     documentationCenter=""
     authors="camsoper"
     manager="erikre"
     editor=""/>
<tags
     ms.service="cdn"
     ms.workload="media"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="article"
    ms.date="07/28/2016"
     ms.author="casoper"/>

# <a name="how-to-map-custom-domain-to-content-delivery-network-cdn-endpoint"></a>Het aangepaste domein toewijzen aan Content Delivery Network (CDN) eindpunt
U kunt een aangepast domein toewijzen aan een eindpunt CDN om uw eigen domeinnaam gebruiken in URL's met inhoud in cache in plaats van met behulp van een subdomein van azureedge.net.

Er zijn twee manieren om uw aangepaste domein worden toegewezen aan een eindpunt CDN:

1. [Een CNAME-record maken met uw domeinregistratieservice en uw aangepaste domein en een subdomein toewijzen aan het eindpunt CDN](#register-a-custom-domain-for-an-azure-cdn-endpoint)

    Een CNAME-record is een DNS-functie die is toegewezen een brondomein zoals `www.contosocdn.com` of `cdn.contoso.com`, naar een doeldomein. In dit geval is het brondomein het aangepaste domein en een subdomein (een subdomein, zoals **www** of **cdn** altijd vereist is). Het doeldomein is uw eindpunt CDN.  

    Het proces van uw aangepaste domein toewijzen aan uw eindpunt CDN kan, echter resulteren in een korte periode van downtime voor het domein terwijl u het domein in de Portal Azure registreert.

2. [Een van tussenliggende registratiestap met **cdnverify** toevoegen](#register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain)

    Als uw aangepaste domein een toepassing met een serviceniveau overeenkomst (SLA) die vereist dat er geen downtime worden momenteel ondersteunt, kunt u het subdomein Azure **cdnverify** gebruiken om een tussenliggende registratiestap zodat gebruikers toegang kunnen krijgen tot uw domein tijdens de DNS-server de toewijzing plaatsvindt.  

Nadat u uw aangepaste domein met behulp van een van de bovenstaande procedures registreert, wilt u [controleren of de aangepaste subdomein verwijst naar uw eindpunt CDN](#verify-that-the-custom-subdomain-references-your-cdn-endpoint).

> [AZURE.NOTE] U moet een CNAME-record maken met uw domeinregistratieservice uw domein koppelen aan het eindpunt CDN. CNAME-records toewijzen van specifieke subdomeinen, zoals `www.contoso.com` of `cdn.contoso.com`. Het is niet mogelijk een CNAME-record om aan te wijzen een hoofddomein, zoals `contoso.com`.
>    
> Een subdomein kan alleen worden gekoppeld aan één CDN eindpunt. De CNAME-record die u maakt, wordt alle verkeer is geadresseerd aan het subdomein tot aan het eindpunt van de opgegeven route.  Als u koppelt bijvoorbeeld `www.contoso.com` met uw eindpunt CDN, vervolgens u kan niet koppelen aan andere Azure eindpunten, zoals een eindpunt opslag account of een cloud-service-eindpunt. U kunt echter verschillende subdomeinen uit hetzelfde domein gebruiken voor verschillende eindpunten. U kunt ook verschillende subdomeinen toewijzen aan hetzelfde eindpunt CDN.
>
> Voor eindpunten **Azure CDN van Verizon** (Standard en Premium), houd er rekening mee dat het beslag tot **90 minuten** voor aangepaste domein wijzigingen doorgeven aan CDN rand knooppunten.

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint"></a>Een aangepaste domein voor een eindpunt Azure CDN registreren

1.  Meld u aan bij de [Azure Portal](https://portal.azure.com/).
2.  Klik op **Bladeren**, vervolgens **CDN profielen**, vervolgens het profiel met het eindpunt dat u wilt toewijzen aan een aangepast domein CDN.  
3.  Klik op het CDN-eindpunt dat u wilt koppelen, het subdomein in de blade **CDN-profiel** .
4.  Klik op de knop **Aangepaste domein toevoegen** aan de bovenkant van het blad van het eindpunt.  Het **toevoegen van een aangepaste domein** blade ziet u de hostnaam eindpunt is afgeleid van het eindpunt CDN, te gebruiken bij het maken van een nieuwe CNAME-record. De indeling van het adres van de host-naam wordt weergegeven als ** &lt;EndpointName >. azureedge.net**.  U kunt deze hostnaam te gebruiken bij het maken van de CNAME-record kopiëren.  
5.  Ga naar de website van uw domeinregistratieservice en Ga naar de sectie voor het maken van DNS-records. Wellicht vindt u dit in een sectie **Domeinnaam**, **DNS**of **Name Server Management**.
6.  Zoek de sectie voor het beheren van CNAME-records. Mogelijk moet u Ga naar een pagina voor geavanceerde instellingen en zoek naar de woorden CNAME-Alias of subdomeinen.
7.  Een nieuwe CNAME-record waarmee u uw gekozen subdomein (bijvoorbeeld **www** of **cdn**) maken met de host-naam die in het blad **een aangepaste domein toevoegen** .
8.  Terug naar de blade **een aangepaste domein toevoegen** en geef uw aangepaste domein, met inbegrip van het subdomein, in het dialoogvenster. De domeinnaam bijvoorbeeld invoeren in de notatie `www.contoso.com` of `cdn.contoso.com`.   

    Azure zal controleren of de CNAME-record bestaat voor de domeinnaam die u hebt ingevoerd. Als de CNAME correct is, wordt uw aangepaste domein gevalideerd.  Voor de eindpunten **Azure CDN van Verizon** (Standard en Premium) duurt tot 90 minuten voor aangepaste domeininstellingen doorgeven aan alle knooppunten van CDN rand, echter.  

    Houd er rekening mee dat in sommige gevallen tijd voor de CNAME-record wellicht doorgeven aan naamservers op het Internet de. Als uw domein niet onmiddellijk worden gevalideerd en u denkt dat de CNAME-record correct is, wacht een paar minuten en probeer het opnieuw.


## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain"></a>Een aangepaste domein voor een CDN Azure eindpunt met behulp van het subdomein tussenliggende cdnverify registreren  

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/).
2. Klik op **Bladeren**, vervolgens **CDN profielen**, vervolgens het profiel met het eindpunt dat u wilt toewijzen aan een aangepast domein CDN.  
3. Klik op het CDN-eindpunt dat u wilt koppelen, het subdomein in de blade **CDN-profiel** .
4. Klik op de knop **Aangepaste domein toevoegen** aan de bovenkant van het blad van het eindpunt.  Het **toevoegen van een aangepaste domein** blade ziet u de hostnaam eindpunt is afgeleid van het eindpunt CDN, te gebruiken bij het maken van een nieuwe CNAME-record. De indeling van het adres van de host-naam wordt weergegeven als ** &lt;EndpointName >. azureedge.net**.  U kunt deze hostnaam te gebruiken bij het maken van de CNAME-record kopiëren.
5. Ga naar de website van uw domeinregistratieservice en Ga naar de sectie voor het maken van DNS-records. Wellicht vindt u dit in een sectie **Domeinnaam**, **DNS**of **Name Server Management**.
6. Zoek de sectie voor het beheren van CNAME-records. Mogelijk moet naar een pagina voor geavanceerde instellingen Ga en zoek naar de woorden **CNAME**, **Alias**of **subdomeinen**.
7. Een nieuwe CNAME-record maken en bieden een subdomein van het subdomein **cdnverify** met alias. Het subdomein dat u opgeeft zal bijvoorbeeld in de indeling **cdnverify.www** of **cdnverify.cdn**. Geef de hostnaam, uw eindpunt CDN in de notatie **is cdnverify.&lt; EndpointName >. azureedge.net**.
8. Terug naar de blade **een aangepaste domein toevoegen** en geef uw aangepaste domein, met inbegrip van het subdomein, in het dialoogvenster. De domeinnaam bijvoorbeeld invoeren in de notatie `www.contoso.com` of `cdn.contoso.com`. Houd er rekening mee dat in deze stap niet hoeft te beginnen het subdomein met **cdnverify**.  

    Azure zal controleren of de CNAME-record bestaat voor de cdnverify-domeinnaam die u hebt ingevoerd.
9. Op dit moment uw aangepaste domein is geverifieerd door Azure, maar het verkeer naar uw domein nog niet wordt gerouteerd naar het eindpunt van de CDN. Na lang genoeg gewacht op dat de aangepaste domeininstellingen doorgeven aan de CDN rand knooppunten (90 minuten voor **Azure CDN van Verizon**, **CDN van Akamai Azure**1-2 minuten), terug naar de website van uw registratieservice DNS en een andere CNAME-record maken die worden toegewezen aan een subdomein aan uw eindpunt CDN. Geef bijvoorbeeld het subdomein als **www** of **cdn**en de hostnaam als ** &lt;EndpointName >. azureedge.net**. Met deze stap is de registratie van uw aangepaste domein voltooid.
10. Ten slotte kunt u de CNAME-record die u hebt gemaakt met behulp van **cdnverify**, verwijderen, alleen als een tussenliggende stap noodzakelijk is.  


## <a name="verify-that-the-custom-subdomain-references-your-cdn-endpoint"></a>Controleer of de aangepaste subdomein verwijst naar uw eindpunt CDN

- Na de registratie van uw aangepaste domein kunt u inhoud die is opgeslagen op uw CDN eindpunt met het aangepaste domein openen.
Ten eerste zorgen ervoor dat u openbare inhoud die in de cache op het eindpunt geplaatst. Als uw CDN-eindpunt gekoppeld aan een account voor opslag is, slaat de CDN inhoud in openbare blob containers. Test het aangepaste domein, ervoor te zorgen dat de container is ingesteld dat de toegang van het publiek en bevat ten minste één blob.
- Ga naar het adres van de blob met behulp van het aangepaste domein in uw browser. Als uw aangepaste domein bijvoorbeeld `cdn.contoso.com`, de URL naar een blob in de cache is vergelijkbaar met de volgende URL: http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg

## <a name="see-also"></a>Zie ook

[Het inschakelen van de Content Delivery Network (CDN) voor Azure](./cdn-create-new-endpoint.md)  
