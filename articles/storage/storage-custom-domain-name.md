<properties
    pageTitle="Een domeinnaam voor uw eindpunt Blob-opslag | Microsoft Azure"
    description="Informatie over het toewijzen van een aangepaste domein naar het eindpunt van de Blob storage Azure opslag account in de klassieke Azure-Portal."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="configure-a-custom-domain-name-for-your-blob-storage-endpoint"></a>Een aangepaste domeinnaam configureren voor uw eindpunt Blob-opslag

## <a name="overview"></a>Overzicht

U kunt een aangepast domein voor toegang tot blobgegevens in uw account opslag Azure. Het standaardeindpunt voor Blob-opslag is `<storage-account-name>.blob.core.windows.net`. Als u toewijzen een aangepaste domein en een subdomein bijvoorbeeld **www.contoso.com** eindpunt van de blob voor uw opslag-account, wordt uw gebruikers kunnen ook toegang blob-gegevens in uw opslag account met dat domein.

>[AZURE.IMPORTANT] Azure opslag ondersteunt nog geen HTTPS met aangepaste domeinen. We zijn op de hoogte dat klanten geïnteresseerd in deze functie bent en het in een toekomstige release beschikbaar zal zijn.

Er zijn twee manieren om uw aangepaste domein wijst u het eindpunt van de blob voor uw opslag-account. De eenvoudigste manier is voor het maken van een CNAME-record uw aangepaste domein en een subdomein toe te wijzen aan het eindpunt van de blob. Een CNAME-record is een DNS-functie die is toegewezen een brondomein naar een doeldomein. In dit geval het brondomein is uw aangepaste domein en een subdomein--nota van het subdomein is altijd vereist. Het doeldomein is de Blob-service-eindpunt.

Het proces van uw aangepaste domein koppelen aan het eindpunt van de blob kan, echter resulteren in een korte periode van downtime voor het domein terwijl u het domein in de [Klassieke Azure-Portal registreert](https://manage.windowsazure.com). Als uw aangepaste domein een toepassing met een serviceniveau overeenkomst (SLA) die vereist dat er geen downtime worden momenteel ondersteunt, kunt u het subdomein Azure **asverify** gebruiken om een tussenliggende registratiestap zodat gebruikers toegang kunnen krijgen tot uw domein tijdens de DNS-server de toewijzing plaatsvindt.

De volgende tabel ziet voorbeeld-URL's voor toegang tot de blob-gegevens in een opslag-account met de naam **mystorageaccount**. Het aangepaste domein geregistreerd voor de account opslag is **www.contoso.com**:

Resourcetype|URL-indelingen
---|---
Opslag account|**Standaard-URL:** http://mystorageaccount.blob.core.windows.net<p>**Aangepaste domein URL:** http://www.contoso.com</td>
BLOB|**Standaard-URL:** http://mystorageaccount.blob.core.windows.net/mycontainer/myblob<p>**Aangepaste domein URL:** http://www.contoso.com/mycontainer/myblob
Root-container|**Standaard-URL:** http://mystorageaccount.blob.core.windows.net/myblob of http://mystorageaccount.blob.core.windows.net/$ hoofdmap/myblob<p>**Aangepaste domein URL:** http://www.contoso.com/myblob of http://www.contoso.com/$ hoofdmap/myblob

## <a name="register-a-custom-domain-for-your-storage-account"></a>Een aangepaste domein voor uw opslag account registreren

Gebruik deze procedure om uw aangepaste domein registreren als er geen problemen met het domein worden kort niet beschikbaar voor gebruikers, of als uw aangepaste domein is een toepassing die momenteel niet beheert.

Als uw aangepaste domein een toepassing die elke uitvaltijd kan niet hebben momenteel ondersteunt is, gebruikt u de procedure die wordt beschreven in <a href="#register-a-custom-domain-for-your-storage-account-using-the-intermediary-asverify-subdomain">een aangepaste domein voor uw opslag-account met behulp van het subdomein tussenliggende asverify registreren</a>.

Een aangepaste domeinnaam configureren, moet u een nieuwe CNAME-record maken met uw domeinregistratieservice. De CNAME-record bevat een alias voor een domeinnaam; in dit geval wordt het adres van uw aangepaste domein naar het eindpunt van de Blob opslag voor uw opslag account toegewezen.

Elke registrator heeft een soortgelijke, maar enigszins verschillende manier een CNAME-record, maar het concept is hetzelfde. Houd er rekening mee dat veel basic domein registratie pakketten geen DNS-configuratie, bieden dus u uw domein registratiepakket bijwerken moet voordat u de CNAME-record kunt maken.

1.  Ga naar het tabblad **opslag** in de [Klassieke Azure-Portal](https://manage.windowsazure.com).

2.  Klik op de naam van de account van de opslag die u wilt aangepaste domein toewijzen op het tabblad **opslag** .

3.  Klik op het tabblad **configureren** .

4.  Klik onderaan in het scherm **Domein beheren** om het dialoogvenster **Aangepaste domein beheren** weer te geven. In de tekst boven aan het dialoogvenster ziet u informatie over het maken van de CNAME-record. Voor deze procedure negeren de tekst die naar het subdomein **asverify verwijst** .

5.  Meld u aan bij uw DNS-domeinregistratie en Ga naar de pagina voor het beheren van DNS. Wellicht vindt u dit in een sectie **Domeinnaam**, **DNS**of **Name Server Management**.

6.  Zoek de sectie voor het beheren van CNAME-records. Mogelijk moet naar een pagina voor geavanceerde instellingen Ga en zoek naar de woorden **CNAME**, **Alias**of **subdomeinen**.

7.  Een nieuwe CNAME-record maken en geef de alias van een subdomein, zoals **www** of **foto's**. Geef een host-naam, de Blob service-eindpunt in de notatie **mystorageaccount.blob.core.windows.net** (waarbij **mystorageaccount** staat voor de naam van de account van uw opslag). Gebruik van de host-naam wordt opgegeven in de tekst van het dialoogvenster **Aangepaste domein beheren** .

8.  Nadat u de CNAME-record hebt gemaakt, terug te keren naar het dialoogvenster **Aangepaste domein beheren** en typ de naam van uw aangepaste domein, met inbegrip van het subdomein, in het veld **Aangepaste domeinnaam** . Als het domein **contoso.com** en een subdomein **www**, invoeren, bijvoorbeeld **www.contoso.com**; Geef **photos.contoso.com**op als een subdomein **foto's**. Houd er rekening mee dat het subdomein vereist is.

9. Klik op de knop **registreren** om uw aangepaste domein registreren.

    Als de registratie voltooid is, verschijnt het bericht dat **uw aangepaste domein actief is**. Gebruikers kunnen nu beeld blob-gegevens op uw aangepaste domein zolang ze de juiste machtigingen hebt.

## <a name="register-a-custom-domain-for-your-storage-account-using-the-intermediary-asverify-subdomain"></a>Een aangepaste domein voor uw opslag-account met behulp van het subdomein tussenliggende asverify registreren

Gebruik deze procedure voor het registreren van uw aangepaste domein als uw aangepaste domein ondersteunt momenteel een toepassing met een SLA die dat vereist er geen downtime. Door het maken van een CNAME die wijst van asverify. &lt;subdomein&gt;. &lt;customdomain&gt; te asverify. &lt;storageaccount&gt;. blob.core.windows.net, kunt u uw domein bij Azure vooraf registreren. Vervolgens kunt u een tweede CNAME wijst van maken &lt;subdomein&gt;. &lt;customdomain&gt; te &lt;storageaccount&gt;. blob.core.windows.net, waardoor verkeer naar uw aangepaste domein wordt doorgestuurd naar het eindpunt van de blob.

Het subdomein asverify is een speciale subdomein herkend door Azure. Door prepending **asverify** voor uw eigen subdomein, toestaan dat Azure herkent uw aangepaste domein zonder het wijzigen van de DNS-record voor het domein. Wanneer u wijzigingen in de DNS-record voor het domein aanbrengt, wordt het eindpunt van de blob met geen uitvaltijd toegewezen.

1.  Ga naar het tabblad **opslag** in de [Klassieke Azure-Portal](https://manage.windowsazure.com).

2.  Klik op de naam van de account van de opslag die u wilt aangepaste domein toewijzen op het tabblad **opslag** .

3.  Klik op het tabblad **configureren** .

4.  Klik onderaan in het scherm **Domein beheren** om het dialoogvenster **Aangepaste domein beheren** weer te geven. In de tekst boven aan het dialoogvenster ziet u informatie over het maken van de CNAME-record met behulp van het subdomein **asverify** .

5.  Meld u aan bij uw DNS-domeinregistratie en Ga naar de pagina voor het beheren van DNS. Wellicht vindt u dit in een sectie **Domeinnaam**, **DNS**of **Name Server Management**.

6.  Zoek de sectie voor het beheren van CNAME-records. Mogelijk moet naar een pagina voor geavanceerde instellingen Ga en zoek naar de woorden **CNAME**, **Alias**of **subdomeinen**.

7.  Een nieuwe CNAME-record maken en bieden een subdomein van het subdomein asverify met alias. Het subdomein dat u opgeeft zal bijvoorbeeld in de indeling **asverify.www** of **asverify.photos**. Geef een host-naam, de Blob service-eindpunt in de notatie **asverify.mystorageaccount.blob.core.windows.net** (waarbij **mystorageaccount** staat voor de naam van de account van uw opslag). Gebruik van de host-naam wordt opgegeven in de tekst van het dialoogvenster **Aangepaste domein beheren** .

8.  Nadat u de CNAME-record hebt gemaakt, terug te keren naar het dialoogvenster **Aangepaste domein beheren** en typ de naam van uw aangepaste domein in het veld **Aangepaste domeinnaam** . Als het domein **contoso.com** en een subdomein **www**, invoeren, bijvoorbeeld **www.contoso.com**; Geef **photos.contoso.com**op als een subdomein **foto's**. Houd er rekening mee dat het subdomein vereist is.

9.  Klik op het selectievakje dat zegt **Geavanceerd: het subdomein 'asverify' gebruiken om mijn aangepaste domein preregister**.

10. Klik op de knop **registreren** om uw aangepaste domein preregister.

    Als de voorafgaande registratie voltooid is, verschijnt het bericht dat **uw aangepaste domein actief is**.

11. Op dit moment uw aangepaste domein is geverifieerd door Azure, maar het verkeer naar uw domein nog niet aan uw account voor de opslag worden gerouteerd. Terug naar de DNS-domeinregistratie om het te voltooien, en een andere CNAME-record die aan de Blob-service-eindpunt een subdomein maken. Geef bijvoorbeeld het subdomein **www** of **foto's**en de hostnaam als **mystorageaccount.blob.core.windows.net** (waarbij **mystorageaccount** staat voor de naam van de account van uw opslag). Met deze stap is de registratie van uw aangepaste domein voltooid.

12. Ten slotte kunt u de CNAME-record die u hebt gemaakt met behulp van **asverify**, verwijderen, alleen als een tussenliggende stap noodzakelijk is.

Gebruikers kunnen nu beeld blob-gegevens op uw aangepaste domein zolang ze de juiste machtigingen hebt.

## <a name="verify-that-the-custom-domain-references-your-blob-service-endpoint"></a>Controleer of het aangepaste domein verwijst naar de Blob-service-eindpunt

Om te controleren of uw aangepaste domein inderdaad is toegewezen aan de Blob-service-eindpunt, een blob te maken in een openbare container in uw account voor opslag. Vervolgens in een webbrowser, gebruiken een URI in de volgende notatie voor toegang tot de blob:

-   http://<*subdomain.customdomain*>/<*mycontainer*>/<*myblob*>

U kunt bijvoorbeeld de volgende URI voor toegang tot een webformulier via een aangepast subdomein van **photos.contoso.com** die is toegewezen aan een blob in de container **myforms** :

-   http://Photos.contoso.com/myforms/applicationform.htm

## <a name="unregister-a-custom-domain-from-your-storage-account"></a>De registratie van een aangepast domein van uw opslag account

Ga als volgt te werk om een aangepast domein register, te verwijderen: 

1. Aanmelden bij de [Azure klassieke Portal](https://manage.windowsazure.com). 

2. Klik in het navigatiedeelvenster op **opslag**. 

3. Klik op de pagina **opslag** op de naam van de account van de opslag om het dashboard weer te geven. 

5. Klik op het lint op **Domein beheren**. 

6. In het dialoogvenster **Aangepaste domein beheren** , klikt u op **registratie**. 


## <a name="additional-resources"></a>Aanvullende bronnen

-   [Het aangepaste domein toewijzen aan Content Delivery Network (CDN) eindpunt](../cdn/cdn-map-content-to-custom-domain.md)
