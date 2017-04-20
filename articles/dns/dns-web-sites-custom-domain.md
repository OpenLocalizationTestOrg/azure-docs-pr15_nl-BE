<properties
   pageTitle="Maak aangepaste DNS-records voor een web app | Microsoft Azure  "
   description="Het maken van aangepaste domein DNS-records voor web app met Azure DNS."
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-dns-records-for-a-web-app-in-a-custom-domain"></a>DNS-records voor een web app in een aangepast domein te maken

Azure DNS kunt u een aangepast domein host voor uw web-apps. Zo maakt u een Azure web app en u wilt dat gebruikers toegang tot het door een van beide contoso.com of www.contoso.com gebruiken als een FQDN-naam.

Hiervoor hebt u twee records te maken:

- Een hoofdmap 'A'-record contoso.com aan te wijzen
- Een "CNAME" record voor de WWW-naam die verwijst naar de A-record

Houd er rekening mee dat als u een A-record voor een web app in Azure maakt, de A-record handmatig moet worden bijgewerkt als het onderliggende IP-adres voor de wijzigingen op het web app.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint, moet u eerst een DNS-zone maakt in Azure DNS en overdragen van de zone in uw registratieservice Azure DNS.

1. Als een DNS-zone maakt, volg de stappen in het [maken van een DNS-zone](dns-getstarted-create-dnszone.md).
2. Als u wilt overdragen naar Azure DNS DNS, volg de stappen in de [overdracht van DNS-domein](dns-domain-delegation.md).

Na het maken van een zone en het overdragen van Azure DNS, kunt u de records vervolgens uw aangepaste domein maken.


## <a name="1-create-an-a-record-for-your-custom-domain"></a>1. Maak een A-record voor uw aangepaste domein

Een A-record wordt gebruikt om een naam toewijzen aan het bijbehorende IP-adres. In het volgende voorbeeld zullen we wijzen @ als een A-record met een IPv4-adres:

### <a name="step-1"></a>Stap 1

Een A-record maken en toewijzen aan een variabele $rs

    $rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600

### <a name="step-2"></a>Stap 2

De IPv4-waarde toevoegen aan de eerder gemaakte record set "@" met de $rs variabele toegewezen. Het toegewezen IPv4-waarde worden het IP-adres voor uw web app.

Volg de stappen in [een aangepaste domeinnaam in Azure App-Service configureren](../web-sites-custom-domain-name.md#Find-the-virtual-IP-address)om het IP-adres voor een web app.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address <your web app IP address>

### <a name="step-3"></a>Stap 3

De wijzigingen in de recordreeks vastleggen. Gebruik `Set-AzureRMDnsRecordSet` de wijzigingen uploaden naar de record die is ingesteld op Azure DNS:

    Set-AzureRMDnsRecordSet -RecordSet $rs

## <a name="2-create-a-cname-record-for-your-custom-domain"></a>2. Maak een CNAME-record voor uw aangepaste domein

Als uw domein al wordt beheerd door DNS Azure (Zie [DNS-domein delegeren](dns-domain-delegation.md), kunt u het volgende in het voorbeeld voor het maken van een CNAME-record voor contoso.azurewebsites.net.

### <a name="step-1"></a>Stap 1

PowerShell openen en een nieuwe CNAME-record set maken en toewijzen aan een variabele $rs. In dit voorbeeld wordt een recordset van het type CNAME-met "time to live" maken van 600 seconden in de DNS-zone met de naam 'contoso.com'.

    $rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600

    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### <a name="step-2"></a>Stap 2

Wanneer de CNAME-record set is gemaakt, moet u de waarde van een alias die naar de web app verwijst maken.

Met behulp van de eerder toegewezen variabele "$rs" kunt u de opdracht PowerShell hieronder de alias maken voor de contoso.azurewebsites.net web app.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"

    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {contoso.azurewebsites.net}
    Tags              : {}

### <a name="step-3"></a>Stap 3

De wijzigingen die met behulp van de `Set-AzureRMDnsRecordSet` cmdlet:

    Set-AzureRMDnsRecordSet -RecordSet $rs

Valideren van de record correct is gemaakt met het opvragen van de "www.contoso.com" met nslookup, zoals hieronder wordt weergegeven:

    PS C:\> nslookup
    Default Server:  Default
    Address:  192.168.0.1

    > www.contoso.com
    Server:  default server
    Address:  192.168.0.1

    Non-authoritative answer:
    Name:    <instance of web app service>.cloudapp.net
    Address:  <ip of web app service>
    Aliases:  www.contoso.com
    contoso.azurewebsites.net
    <instance of web app service>.vip.azurewebsites.windows.net

## <a name="create-an-awverify-record-for-web-apps"></a>Maak een 'awverify' record voor web apps


Als u een A-record voor uw web app gebruikt, moet u een verificatieprocedure om ervoor te zorgen dat u eigenaar bent van het aangepaste domein doorlopen. Deze controlestap gebeurt door een speciale CNAME-record met de naam 'awverify' te maken. Deze sectie geldt voor A-records alleen.


### <a name="step-1"></a>Stap 1

De "awverify"-record maken. In het volgende voorbeeld maken we de 'aweverify' record voor contoso.com te verifiëren dat voor het aangepaste domein.

    $rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "awverify" -RecordType "CNAME" -Ttl 600

    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### <a name="step-2"></a>Stap 2

Nadat de recordset 'awverify' is gemaakt, wijzen de CNAME-record alias instellen. In het volgende voorbeeld wijst de CNAME-record alias ingesteld op awverify.contoso.azurewebsites.net toe.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"

    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {awverify.contoso.azurewebsites.net}
    Tags              : {}

### <a name="step-3"></a>Stap 3

De wijzigingen die met behulp van de `Set-AzureRMDnsRecordSet cmdlet`, zoals aangegeven in de opdracht hieronder.

    Set-AzureRMDnsRecordSet -RecordSet $rs



## <a name="next-steps"></a>Volgende stappen

Volg de stappen in [een aangepaste domeinnaam voor App-Service configureren](../app-service-web/web-sites-custom-domain-name.md) voor het configureren van uw web app voor het gebruik van een aangepaste domein.








