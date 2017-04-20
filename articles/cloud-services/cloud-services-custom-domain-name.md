<properties
    pageTitle="Een aangepaste domeinnaam in een Cloud-Services configureren | Microsoft Azure"
    description="Informatie over hoe u uw Azure toepassing of gegevens in een aangepaste domein weergeven door de DNS-instellingen configureren."
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="adegeo"/>

# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a>Een aangepaste domeinnaam voor een Azure cloud-service configureren

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-custom-domain-name-portal.md)
- [Azure klassieke portal](cloud-services-custom-domain-name.md)


Als u een Cloud-Service maakt, Azure wordt toegewezen aan een subdomein van cloudapp.net. Bijvoorbeeld, als uw Cloud-Service met de naam 'contoso', uw gebruikers kunnen toegang tot uw toepassing op een URL zoals http://contoso.cloudapp.net. Azure wordt ook een virtuele IP-adres toegewezen.

U kunt uw toepassing echter ook blootstellen op uw eigen domeinnaam, bijvoorbeeld contoso.com. In dit artikel wordt uitgelegd hoe reserveren of een aangepaste domeinnaam configureren voor Cloud Service web-rollen.

Wilt u al undestand de records CNAME en A worden? [Sprong voorbij de verklaring](#add-a-cname-record-for-your-custom-domain).

> [AZURE.NOTE]
> Alles gaat sneller! Gebruik de Azure [begeleide scenario](http://support.microsoft.com/kb/2990804). Er wordt een aangepaste domeinnaam koppelen en communicatie (SSL) met Azure Cloud Services of Websites Azure een handomdraai beveiligen.

<p/>

> [AZURE.NOTE]
> De procedures in deze taak van toepassing op Azure Cloud Services. Zie voor App-Services, [deze](../app-service-web/web-sites-custom-domain-name.md). Zie voor accounts, opslag, [deze](../storage/storage-custom-domain-name.md).


## <a name="understand-cname-and-a-records"></a>Begrijpen van de records CNAME en A

CNAME (aliasrecords of) en een records kunnen u een domeinnaam te koppelen aan een specifieke server (of service in dit geval) maar werken verschillend. Er zijn ook enkele specifieke overwegingen bij het gebruik van A-records met Azure Cloud-services waarmee u rekening houden moet voordat u besluit om te gebruiken.

### <a name="cname-or-alias-record"></a>Record of een CNAME-Alias

Een *specifiek* domein, zoals **contoso.com** of **www.contoso.com**, koppelt een CNAME-record aan een canonieke naam. In dit geval de canonieke naam wordt de **[Mijntoep] .cloudapp .net** domeinnaam van uw Azure gehoste toepassing. Eenmaal gemaakt, de CNAME een alias gemaakt voor de **[Mijntoep] .cloudapp .net**. De CNAME-post wordt omgezet in het IP-adres van uw **[Mijntoep] .cloudapp .net** service automatisch, zodat als het IP-adres van de cloud-dienst wordt gewijzigd, u hoeft geen actie te ondernemen.

> [AZURE.NOTE]
> Sommige registratieservices domein alleen kunnen u subdomeinen toewijzen wanneer u een CNAME-record, bijvoorbeeld www.contoso.com en geen basis-namen, zoals contoso.com. Zie de documentatie bij uw registratieservice, [de Wikipedia-post op de CNAME-record](http://en.wikipedia.org/wiki/CNAME_record)of het [IETF-domeinnamen - implementatie en specificatie](http://tools.ietf.org/html/rfc1035) document voor meer informatie over CNAME-records.

### <a name="a-record"></a>Een record

Een A-record koppelt een domein, bijvoorbeeld **contoso.com** of **www.contoso.com**, *of in een jokertekendomein* , zoals ** \*. contoso.com**, een IP-adres. In het geval van een Azure Cloud-Service, het virtuele IP van de service. Dus het belangrijkste voordeel van een A-record via een CNAME-record is dat u kunt een post met een jokerteken, zoals \* **. contoso.com**, die zou verwerken van aanvragen voor meerdere subdomeinen, zoals **mail.contoso.com**, **login.contoso.com**of **www.contso.com**.

> [AZURE.NOTE]
> Aangezien een A-record is toegewezen aan een statisch IP-adres, kan het automatisch omzetten in wijzigingen het IP-adres van uw Cloud-Service. Het IP-adres dat wordt gebruikt door de Service Cloud is toegewezen voor het eerst implementeren van een lege sleuf (productie of staging.) Als u de implementatie van de sleuf verwijderen, het IP-adres is vrijgegeven door Azure en eventuele toekomstige implementaties in de sleuf kunnen een nieuw IP-adres worden gegeven.
>
> Het IP-adres van een bepaalde implementatie-sleuf (productie- of staging) is gemakkelijk, behouden bij het wisselen tussen de fasering en productie-implementaties of het uitvoeren van een interne upgrade van een bestaande installatie. Zie voor meer informatie over het uitvoeren van deze acties, [cloud services beheren](cloud-services-how-to-manage.md).


## <a name="add-a-cname-record-for-your-custom-domain"></a>Een CNAME-record voor uw aangepaste domein toevoegen

Om een CNAME-record maakt, moet u een nieuwe vermelding in de DNS-tabel voor uw aangepaste domein toevoegen met behulp van de hulpprogramma's die door uw registratieservice. Elke registrator heeft een soortgelijke, maar enigszins verschillende manier een CNAME-record, maar de concepten zijn hetzelfde.

1. Een van deze methoden gebruiken om te zoeken naar de **. cloudapp.net** domeinnaam toegewezen aan de service cloud.

    * Aanmelden bij de [Azure klassieke portal], selecteer uw service cloud **Dashboard**selecteren en vervolgens de vermelding van de **URL van de Site** te vinden in de sectie **Overzicht** .
    
        ![overzicht sectie met de site-URL][csurl]
    
        **OR**  
    
    * Installeren en configureren van [Azure Powershell](../powershell-install-configure.md)en gebruikt u de volgende opdracht:
        
        ```powershell
        Get-AzureDeployment -ServiceName yourservicename | Select Url
        ```
    
    De domeinnaam die wordt gebruikt in de URL die wordt geretourneerd door de methode, moet u deze bij het maken van een CNAME-record opslaan.

1.  Meld u aan bij uw DNS-domeinregistratie en Ga naar de pagina voor het beheren van DNS. Zoek naar koppelingen of gedeelten van de site aangeduid als **Domeinnaam**, **DNS**of **Name Server Management**.

2.  Nu kunt u selecteren of geef de CNAME-zoeken. Mogelijk moet naar beneden selecteert u het recordtype dat in een vervolgkeuzelijst of gaat u naar een pagina voor geavanceerde instellingen. U moet zoeken naar woorden **CNAME**, **Alias**of **subdomeinen**.

3.  Ook moet u de alias domein of subdomein voor de CNAME, zoals **www** opgeven als u wilt een alias maken voor **www.customdomain.com**. Als u een alias maken voor het hoofddomein, kan deze worden vermeld als de "**@**" symbool in een van uw registratieservice DNS-hulpprogramma's.

4. Vervolgens moet u een canonieke hostnaam, in dit geval van uw toepassing **cloudapp.net** domein is opgeven.

Bijvoorbeeld doorstuurt de volgende CNAME-record al het verkeer van **www.contoso.com** naar **contoso.cloudapp.net**, de naam van het aangepaste domein van de gedistribueerde toepassing:

| Alias/Host-naam/subdomein | Canonieke domein     |
| ------------------------- | -------------------- |
| www                       | Contoso.cloudapp.NET |

Een bezoeker van **www.contoso.com** zien nooit de waarde true host (contoso.cloudapp.net), zodat het doorstuurproces onzichtbaar voor de eindgebruiker is.

> [AZURE.NOTE]
> Het bovenstaande voorbeeld is alleen van toepassing op het verkeer bij het subdomein **www** . Aangezien u jokertekens met CNAME-records gebruiken kunt, moet u een CNAME voor elk domein/subdomein maken. Als u wilt direct verkeer van subdomeinen, zoals \*. contoso.com, aan het adres van uw cloudapp.net, kunt u een post **URL omgeleid** of **Volgende URL** in uw DNS-instellingen configureren of een A-record maken.


## <a name="add-an-a-record-for-your-custom-domain"></a>Een A-record voor uw aangepaste domein toevoegen

Een A-record maken, moet u eerst het virtuele IP-adres van uw cloud-service zoeken. Vervolgens een nieuwe vermelding in de DNS voor uw aangepaste domein toevoegen met behulp van de hulpprogramma's die door uw registratieservice. Elke registrator heeft een soortgelijke, maar enigszins verschillende methode voor het opgeven van een A-record, maar de concepten zijn hetzelfde.

1. Een van de volgende methoden gebruiken om het IP-adres van uw cloud-service.
    
    * aanmelden bij de [Azure klassieke portal], selecteer uw service cloud **Dashboard**selecteren en zoekt u de **Openbare virtuele IP-(VIP)** invoeren in de sectie **Overzicht** .
    
        ![overzicht sectie met de VIP][vip]
    
        **OR**  
    
    * Installeren en configureren van [Azure Powershell](../powershell-install-configure.md)en gebruikt u de volgende opdracht:
    
        ```powershell
        get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
        ```
    
    Als u meerdere endpoints die zijn gekoppeld aan uw cloud-service hebt, ontvangt u meerdere regels met het IP-adres, maar alle hetzelfde adres moet worden weergegeven.
    
    Sla het IP-adres, moet u deze bij het maken van een A-record.

1.  Meld u aan bij uw DNS-domeinregistratie en Ga naar de pagina voor het beheren van DNS. Zoek naar koppelingen of gedeelten van de site aangeduid als **Domeinnaam**, **DNS**of **Name Server Management**.

2.  Nu kunt u selecteren of invoeren van een record zoeken. Mogelijk moet naar beneden selecteert u het recordtype dat in een vervolgkeuzelijst of gaat u naar een pagina voor geavanceerde instellingen.

3. Selecteer of typ het domein of subdomein dat deze A-record wordt gebruikt. Selecteer bijvoorbeeld **www** als u wilt een alias maken voor **www.customdomain.com**. Als u een jokerteken vermelding maken voor alle subdomeinen, voer '__*__'. Hieronder vallen alle subdomeinen, zoals **mail.customdomain.com**, **login.customdomain.com**en **www.customdomain.com**.

    Als u maken voor het hoofddomein van een A-record wilt, kan deze worden vermeld als de "**@**" symbool in een van uw registratieservice DNS-hulpprogramma's.

4. Voer het IP-adres van uw cloud-service in het opgegeven veld. Hiermee wordt de domein-vermelding in de A-record met het IP-adres van uw implementatie van cloud-service gebruikt.

Bijvoorbeeld, de volgende die record al het verkeer van **contoso.com** naar **137.135.70.239**, het IP-adres van de gedistribueerde toepassing doorstuurt:

| Host-naam/subdomein | IP-adres     |
| ------------------- | -------------- |
| @                   | 137.135.70.239 |



In dit voorbeeld wordt het maken van een A-record voor het hoofddomein. Als u een jokerteken vermelding wilt ter dekking van alle subdomeinen maken, typt u '__*__' als de subdomein.

>[AZURE.WARNING]
>IP-adressen in Azure zijn standaard dynamisch. Waarschijnlijk zult u een [gereserveerd IP-adres](../virtual-network/virtual-networks-reserved-public-ip.md) gebruiken om ervoor te zorgen dat uw IP-adres niet verandert.

## <a name="next-steps"></a>Volgende stappen

* [Cloud-Services beheren](cloud-services-how-to-manage.md)
* [CDN inhoud toewijzen aan een aangepast domein](../cdn/cdn-map-content-to-custom-domain.md)
* [Algemene configuratie van de service cloud](cloud-services-how-to-configure.md).
* Meer informatie over het [implementeren van een cloud-service](cloud-services-how-to-create-deploy.md).
* [Ssl-certificaten](cloud-services-configure-ssl-certificate.md)configureren.




[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: http://msdn.microsoft.com/library/ee517253.aspx
[Create a CNAME record that associates the subdomain with the storage account]: #create-cname
[Azure klassieke portal]: https://manage.windowsazure.com
[Validate Custom Domain dialog box]: http://i.msdn.microsoft.com/dynimg/IC544437.jpg
[vip]: ./media/cloud-services-custom-domain-name/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name/csurl.png
 