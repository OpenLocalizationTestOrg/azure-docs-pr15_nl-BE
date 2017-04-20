<properties
   pageTitle="Maak een recordset en records voor een DNS-zone met PowerShell | Microsoft Azure"
   description="Het maken van host-records voor DNS Azure. Instellen van de record wordt ingesteld en de records met PowerShell"
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



# <a name="create-dns-record-sets-and-records-by-using-powershell"></a>DNS-record sets en records maken met PowerShell


> [AZURE.SELECTOR]
- [Azure Portal](dns-getstarted-create-recordset-portal.md)
- [PowerShell](dns-getstarted-create-recordset.md)
- [Azure CLI](dns-getstarted-create-recordset-cli.md)

Dit artikel begeleidt u door het proces van het maken van records en records wordt ingesteld met behulp van Windows PowerShell. Nadat u de DNS-zone maakt, kunt u de DNS-records voor uw domein toevoegen. Hiertoe moet u eerst begrijpen DNS-records en recordsets.

[AZURE.INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

## <a name="verify-that-you-have-the-latest-version-of-powershell"></a>Controleer of u de nieuwste versie van PowerShell

Controleren of u de nieuwste versie van de Azure Resource Manager PowerShell-cmdlets hebt geïnstalleerd. [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) Zie voor meer informatie over het installeren van de PowerShell-cmdlets.

## <a name="create-a-record-set-and-record"></a>Een recordset en een record maken

Deze sectie wordt beschreven hoe u een recordset en een record maakt.


### <a name="1-connect-to-your-subscription"></a>1. Maak verbinding met uw abonnement

De PowerShell-console openen en verbinding maken met uw account. Met het volgende voorbeeld kunt u verbinding maken:

    Login-AzureRmAccount

Controleer de abonnementen voor de account.

    Get-AzureRmSubscription

Geef het abonnement dat u wilt gebruiken.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

Zie voor meer informatie over het werken met PowerShell, [Met behulp van Windows PowerShell met Resource Manager](../powershell-azure-resource-manager.md).


### <a name="2-create-a-record-set"></a>2. Maak een verzameling

Recordsets te maken met de `New-AzureRmDnsRecordSet` cmdlet. Bij het maken van een verzameling, moet u de record instellen de naam, de zone, de tijd voor het live (TTL) en het type record opgeven.

Voor het maken van een recordset in de top van de zone (in dit geval 'contoso.com'), gebruikt u de naam van de "@", met inbegrip van de aanhalingstekens. Dit is een algemene DNS-Verdrag.

In het volgende voorbeeld wordt een recordset met de relatieve naam 'www' in de DNS-Zone 'contoso.com' gemaakt. De FQDN-naam van de recordset is "www.contoso.com". Het recordtype is "A", en de TTL-waarde is 60 seconden. Na deze stap is voltooid, hebt u een set van "www" lege record die is toegewezen aan de variabele *$rs*.

    $rs = New-AzureRmDnsRecordSet -Name "www" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 60

#### <a name="if-a-record-set-already-exists"></a>Als een record al bestaat

Als een record instellen al bestaat, mislukt de opdracht als de *-overschrijven* schakeloptie wordt gebruikt. De *-overschrijven* optie activeert een bevestiging wordt gevraagd, die kan worden onderdrukt met behulp van de *-Force* overschakelen.


    $rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 -ZoneName contoso.com -ResouceGroupName MyAzureResouceGroup [-Tag $tags] [-Overwrite] [-Force]


In dit voorbeeld geeft u de zone met de zonenaam en Resourcegroepnaam. Ook kunt u een zone-object als resultaat wordt gegeven door `Get-AzureRmDnsZone` of `New-AzureRmDnsZone`.

    $zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup
    $rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 –Zone $zone [-Tag $tags] [-Overwrite] [-Force]

`New-AzureRmDnsRecordSet`Deze eigenschap retourneert een lokaal object dat de set record die is gemaakt in Azure DNS vertegenwoordigt.

### <a name="3-add-a-record"></a>3. een record toevoegen

De zojuist gemaakte 'www' record als set wilt gebruiken, moet u de records aan toe te voegen. U kunt toevoegen IPv4 *A* -records aan de 'www' record instellen met het volgende voorbeeld. In het volgende voorbeeld is afhankelijk van de variabele *$rs* die u hebt ingesteld in de vorige stap.

Records toevoegen aan een record instellen met behulp van `Add-AzureRmDnsRecordConfig` is een off line bewerking. Alleen de lokale variabele *$rs* wordt bijgewerkt.


    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.185.46
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.188.221

### <a name="4-commit-the-changes"></a>4. de wijzigingen doorvoeren

De wijzigingen in de recordreeks vastleggen. Gebruik `Set-AzureRmDnsRecordSet` de wijzigingen uploaden naar de record die is ingesteld op Azure DNS.

    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="5-retrieve-the-record-set"></a>5. de recordreeks ophalen

U kunt de record uit DNS Azure instellen met behulp van ophalen `Get-AzureRmDnsRecordSet` zoals in het volgende voorbeeld wordt getoond.


    Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup


    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : MyAzureResourceGroup
    Ttl               : 3600
    Etag              : 68e78da2-4d74-413e-8c3d-331ca48246d9
    RecordType        : A
    Records           : {134.170.185.46, 134.170.188.221}
    Tags              : {}


U kunt ook het hulpprogramma nslookup of andere hulpprogramma's voor DNS-query uitvoeren op de nieuwe set records.

Als u nog niet het domein met de naam Azure DNS-servers zijn overgedragen, moet u expliciet de naam, de server en het adres voor uw zone opgeven.


    nslookup www.contoso.com ns1-01.azure-dns.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    Name:    www.contoso.com
    Addresses:  134.170.185.46
                134.170.188.221

## <a name="create-a-record-set-of-each-type-with-a-single-record"></a>Een recordset maken van elk type met één record


De volgende voorbeelden laten zien hoe een recordset maken van elk type record. Elke record bevat één record.

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]


## <a name="next-steps"></a>Volgende stappen

[Het beheren van DNS-zones met PowerShell](dns-operations-dnszones.md)

[DNS-records en recordsets via PowerShell beheren](dns-operations-recordsets.md)

[Azure bewerkingen met .NET SDK automatiseren](dns-sdk.md)
