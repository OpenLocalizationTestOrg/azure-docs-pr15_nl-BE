<properties
   pageTitle="DNS-record sets en records beheren via de portal Azure | Microsoft Azure"
   description="Records op Azure DNS en DNS-record sets beheren wanneer uw Azure DNS-domein hosting. Alle PowerShell-opdrachten voor bewerkingen op records en recordsets."
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

# <a name="manage-dns-records-and-record-sets-by-using-powershell"></a>DNS-records en recordsets via PowerShell beheren



> [AZURE.SELECTOR]
- [Azure Portal](dns-operations-recordsets-portal.md)
- [Azure CLI](dns-operations-recordsets-cli.md)
- [PowerShell](dns-operations-recordsets.md)



In dit artikel wordt beschreven hoe u records voor uw DNS-zone en recordsets beheren met Windows PowerShell.

Het is belangrijk te begrijpen het verschil tussen de DNS-record sets en afzonderlijke DNS-records. Een verzameling is de verzameling van records in een zone met dezelfde naam en hetzelfde type. Zie [records met behulp van de portal Azure en recordsets maken DNS](dns-getstarted-create-recordset-portal.md)voor meer informatie.

U moet de meest recente versie van de Azure Resource Manager PowerShell-cmdlets voor het beheren van uw recordsets en records. Zie voor meer informatie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md). Zie voor meer informatie over het werken met PowerShell [Azure PowerShell Azure Resource Manager gebruiken](../powershell-azure-resource-manager.md).



## <a name="create-a-new-record-set-and-a-record"></a>Een nieuwe record en een record maken

Zie [records met PowerShell en recordsets maken van DNS-](dns-getstarted-create-recordset.md)record instellen via PowerShell maken.

## <a name="get-a-record-set"></a>Een verzameling ophalen

U haalt een bestaande record set met `Get-AzureRmDnsRecordSet`. Geeft u dat de record relatieve naam, het type en de zone ingesteld.

    $rs = Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

Net als bij `New-AzureRmDnsRecordSet`, de recordnaam moet een relatieve naam, wat betekent dat de zonenaam van de moet worden uitgesloten.

U kunt de zone opgeven met de zonenaam en de Resourcegroepnaam of met behulp van een zone-object:

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $rs = Get-AzureRmDnsRecordSet -Name www –RecordType A -Zone $zone

`Get-AzureRmDnsRecordSet`Deze eigenschap retourneert een lokaal object dat de set record die is gemaakt in Azure DNS vertegenwoordigt.

## <a name="list-record-sets"></a>Lijst met recordsets

Ook kunt u`Get-AzureRmDnsRecordSet` aan recordsets lijst als u geen waarde opgeeft de *– de naam* en/of *– RecordType* parameters.

### <a name="to-list-all-record-sets"></a>Overzicht van alle recordsets

In het volgende voorbeeld wordt alle recordsets, ongeacht de naam of het type record:

    $list = Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

### <a name="to-list-record-sets-of-a-given-record-type"></a>Aan recordsets van een bepaald type lijst

In het volgende voorbeeld wordt alle recordsets die overeenkomen met het opgegeven type. De recordset wordt is geretourneerd in dit geval 'A'-records:

    $list = Get-AzureRmDnsRecordSet –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

De zone kan worden opgegeven via de *Zonenaam –* en *ResourceGroupName –* parameters (zoals), of een zone-object op te geven:

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $list = Get-AzureRmDnsRecordSet -Zone $zone

## <a name="add-a-record-to-a-record-set"></a>Een record toevoegen aan een verzameling

U records toevoegen aan een record wordt ingesteld met behulp van de `Add-AzureRmDnsRecordConfig` cmdlet. Dit is een off line wordt gewerkt. Alleen het lokale object dat staat voor de verzameling is gewijzigd.

De parameters voor het toevoegen van records aan een verzameling variëren afhankelijk van het type van de recordset. Bijvoorbeeld wanneer u een set record van het type "A", kunt u alleen opgeven records met de parameter *-IPv4Address*.

Als u meer records kunnen worden toegevoegd aan elke record instellen voor extra aanroepen voor `Add-AzureRmDnsRecordConfig`. U kunt maximaal 20 records toevoegen aan een verzameling. Recordsets van het type 'BNAAM' kunnen maximaal één record bevatten, en een verzameling kan niet twee identieke records bevat. Leeg record sets (met nul records) kunnen worden gemaakt, maar worden niet weergegeven op de Azure DNS-naamservers.

Nadat de record de gewenste verzameling records bevat, moet u deze doorvoeren met behulp van de `Set-AzureRmDnsRecordSet` cmdlet. Nadat u een verzameling is toegewezen, vervangt de bestaande record in Azure DNS instellen.

### <a name="to-create-an-a-record-set-with-a-single-record"></a>Voor het maken van een A-record met een enkele record instellen

    $rs = New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

De volgorde van bewerkingen om een record te maken kan ook worden *doorgesluisd*, wat betekent dat u het Recordset-object met behulp van de pijp in plaats van deze wordt doorgegeven als parameter doorgeven. Bijvoorbeeld:

    New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Add-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### <a name="additional-record-type-examples"></a>Voorbeelden van extra recordtype

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]

## <a name="modify-existing-record-sets"></a>Bestaande records wijzigen

De stappen voor het wijzigen van een bestaande record set zijn vergelijkbaar met de stappen bij het maken van records. De volgorde van de bewerkingen is als volgt:

1.  Ophalen van de bestaande record instellen met behulp van `Get-AzureRmDnsRecordSet`.

2.  Wijzigen van de record of records toe te voegen records, te verwijderen als u de recordparameters instellen of wijzigen van de record tijd ingesteld op live (TTL). Dit is een off line wordt gewerkt. Alleen het lokale object dat staat voor de verzameling is gewijzigd.

3.  Uw wijzigingen doorvoeren met behulp van de `Set-AzureRmDnsRecordSet` cmdlet. Deze optie vervangt de bestaande record in Azure DNS instellen.


### <a name="to-update-a-record-in-an-existing-record-set"></a>Een record in een bestaande groep records bij te werken

In dit voorbeeld wijzigen is het IP-adres van een bestaande A-record:

    $rs = Get-AzureRmDnsRecordSet -name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Ipv4Address = "134.170.185.46"
    Set-AzureRmDnsRecordSet -RecordSet $rs

De `Set-AzureRmDnsRecordSet` cmdlet etag cheques worden gebruikt om ervoor te zorgen dat de wijzigingen niet worden overschreven. Gebruik de *-overschrijven* vlag onderdrukken van deze controles. Zie voor meer informatie [over labels en etags](dns-getstarted-create-dnszone.md#tagetag).

### <a name="to-modify-an-soa-record"></a>Een SOA-record wijzigen

U kan toevoegen of verwijderen van records uit de automatisch gemaakte SOA-record ingesteld op de top van de zone (naam = "@"). Echter, kunt u een van de parameters in de SOA-record (met uitzondering van "Host") en de record set TTL.

In het volgende voorbeeld ziet u hoe de eigenschap *e-mailadres* van de SOA-record te wijzigen:

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Email = "admin.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="to-modify-ns-records-at-the-zone-apex"></a>NS-records op de top van de zone wijzigen

U kan toevoegen, verwijderen of de records in de automatisch gemaakte NS record ingesteld op de top van de zone wijzigen (naam = "@"). De enige wijziging die toegestaan, is het wijzigen van de record set TTL.

In het volgende voorbeeld ziet u hoe de eigenschap TTL-waarde van de NS-record set wijzigen:

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Ttl = 300
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="to-add-records-to-an-existing-record-set"></a>Records toevoegen aan een bestaande record set

In dit voorbeeld toevoegen we twee extra MX-records aan de bestaande record set:

    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail2.contoso.com" -Preference 10
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail3.contoso.com" -Preference 20
    Set-AzureRmDnsRecordSet -RecordSet $rs

## <a name="remove-a-record-from-an-existing-record-set"></a>Een record verwijderen uit een bestaande record set

Records kunnen worden verwijderd uit een record instellen met behulp van `Remove-AzureRmDnsRecordConfig`. De record die u wilt verwijderen moet exact overeenkomen met een bestaande record over alle parameters. Wijzigingen moeten worden doorgevoerd via `Set-AzureRmDnsRecordSet`.

De verzameling worden niet verwijderd als u de laatste record verwijderen uit een verzameling. Zie [een record set verwijderen](#delete-a-record-set) hieronder voor meer informatie.


    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

De volgorde van bewerkingen in een record verwijderen uit een verzameling kan ook worden doorgesluisd, wat betekent dat u het Recordset-object met behulp van de pijp in plaats van deze wordt doorgegeven als parameter doorgeven. Bijvoorbeeld:

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### <a name="remove-an-aaaa-record-from-a-record-set"></a>Een AAAA-record verwijderen uit een verzameling

    $rs = Get-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv6Address "2607:f8b0:4009:1803::1005"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-a-cname-record-from-a-record-set"></a>Een CNAME-record verwijderen uit een verzameling

Een CNAME-record set kan maximaal één record bevatten, verlaat die record verwijderen een lege record set.

    $rs =  Get-AzureRmDnsRecordSet -name "test-cname" -RecordType CNAME -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Cname "www.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-mx-record-from-a-record-set"></a>Een MX-record verwijderen uit een verzameling

    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail.contoso.com" -Preference 5
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-ns-record-from-record-set"></a>Een NS-record verwijderen uit een Recordset

    $rs = Get-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname "ns1.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-srv-record-from-a-record-set"></a>Een SRV-record verwijderen uit een verzameling

    $rs = Get-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs –Priority 0 –Weight 5 –Port 8080 –Target "sip.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-a-txt-record-from-a-record-set"></a>Een TXT-record verwijderen uit een verzameling

    $rs = Get-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Value "This is a TXT record"
    Set-AzureRmDnsRecordSet -RecordSet $rs

## <a name="delete-a-record-set"></a>Een verzameling verwijderen

Recordsets kunnen worden verwijderd met behulp van de `Remove-AzureRmDnsRecordSet` cmdlet. U kunt het SOA niet verwijderen en de NS-record ingesteld op de top van de zone (naam = "@") die automatisch zijn gemaakt als de zone is gemaakt. Ze worden automatisch verwijderd als de zone wordt verwijderd.

Gebruik een van de volgende drie methoden voor het verwijderen van een verzameling:

### <a name="specify-all-the-parameters-by-name"></a>Alle parameters opgeven met de naam

De optionele *-Force* switch kan worden gebruikt om de bevestigingsprompt.

    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]


### <a name="specify-the-record-set-by-name-and-type-and-specify-the-zone-by-object"></a>De verzameling met name en type opgeven en geeft u de zone door object

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Zone $zone [-Force]

### <a name="specify-the-record-set-by-object"></a>Geef de Recordset object

    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet –RecordSet $rs [-Overwrite] [-Force]

Wanneer u de record instellen met behulp van een object opgeeft, kan deze etag gecontroleerd om ervoor te zorgen dat de wijzigingen niet worden verwijderd. De optionele *-overschrijven* vlag onderdrukt deze controles. Zie [Etags en labels](dns-getstarted-create-dnszone.md#tagetag) voor meer informatie.

Het Recordset-object kan ook worden doorgesluisd in plaats van als parameter worden doorgegeven:

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordSet [-Overwrite] [-Force]

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over DNS Azure [Azure DNS-overzicht](dns-overview.md). Zie [maken van DNS-zones en record wilt instellen met de .NET SDK](dns-sdk.md)voor meer informatie over het automatiseren van DNS.

Zie voor meer informatie over reverse DNS-records [beheren reverse DNS-records voor uw services met PowerShell](dns-reverse-dns-record-operations-ps.md).
