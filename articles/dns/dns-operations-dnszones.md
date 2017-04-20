<properties
   pageTitle="Beheren van DNS-zones met PowerShell | Microsoft Azure"
   description="U kunt DNS-zones met Azure Powershell beheren. Het bijwerken, verwijderen en maken van DNS-zones op DNS Azure"
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

# <a name="how-to-manage-dns-zones-using-powershell"></a>Het beheren van DNS-Zones met PowerShell

> [AZURE.SELECTOR]
- [Azure CLI](dns-operations-dnszones-cli.md)
- [PowerShell](dns-operations-dnszones.md)



In dit artikel wordt beschreven hoe u voor het beheren van uw DNS-zone met PowerShell. Om deze stappen te gebruiken, moet u de nieuwste versie van de Azure Resource Manager PowerShell-cmdlets (1.0 of hoger) installeren. [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) Zie voor meer informatie over het installeren van de PowerShell-cmdlets.


## <a name="create-a-new-dns-zone"></a>Een nieuwe DNS-zone maken

Zie een DNS-zone maken [maken van een DNS-zone met PowerShell](dns-getstarted-create-dnszone.md).

## <a name="get-a-dns-zone"></a>Ophalen van een DNS-zone

U haalt een DNS-zone met de `Get-AzureRmDnsZone` cmdlet. Deze bewerking wordt een DNS-zone-object overeenkomt met een bestaande zone in DNS Azure. Het object bevat gegevens over de zone (zoals het aantal sets record), maar bevat niet de recordsets zelf.

    $zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup

## <a name="list-dns-zones"></a>Lijst met DNS-zones

Door de zonenaam van de weg te `Get-AzureRmDnsZone`, kunt u alle zones in een resourcegroep opsommen. Deze bewerking retourneert een array met objecten van de zone.

    $zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup

## <a name="update-a-dns-zone"></a>Een DNS-zone-update

De bron van een DNS-zone kan worden gewijzigd met behulp van `Set-AzureRmDnsZone`. Dit werkt niet bij een van de DNS-record stelt binnen de zone (Zie [het beheren van DNS-records](dns-operations-recordsets.md)). Het wordt alleen gebruikt voor het bijwerken van de eigenschappen van de bron van de zone zelf. Dit is momenteel beperkt tot Azure Resource Manager 'tags' voor de bron voor de zone. Zie [Etags en labels](dns-getstarted-create-dnszone.md#Etags-and-tags) voor meer informatie.

Een van de volgende update DNS-zone op twee manieren gebruiken:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group"></a>De zone met behulp van de zone en de bron de groep opgeven

    Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Tag $tags]

### <a name="specify-the-zone-using-a-zone-object"></a>Geeft u de zone een $zone-object gebruiken

Geef de zone met behulp van een $zone-object `Get-AzureRmDnsZone`. Bij het gebruik van `Set-AzureRmDnsZone` Etag controles met een $zone-object wordt gebruikt om ervoor te zorgen niet gelijktijdig wijzigingen worden overschreven. U kunt de optionele *-overschrijven* overschakelen naar deze controles te onderdrukken. Zie [Etags en labels](dns-getstarted-create-dnszone.md#Etags-and-tags) voor meer informatie.


    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    <..modify $zone.Tags here...>
    Set-AzureRmDnsZone -Zone $zone [-Overwrite]


## <a name="delete-a-dns-zone"></a>Verwijderen van een DNS-Zone

DNS-zones kunnen worden verwijderd met de cmdlet verwijderen AzureRmDnsZone.

Voordat u een DNS-zone in DNS Azure verwijdert, moet u het verwijderen van alle records sets, met uitzondering van de NS- en SOA-records in de hoofdmap van de zone die automatisch zijn gemaakt als de zone is gemaakt.

Gebruik een van de volgende twee manieren voor het verwijderen van een DNS-zone:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a>De zone met de zonenaam en de Resourcegroepnaam opgeven

Deze bewerking is een optionele *-Force* -switch wordt onderdrukt de prompt die u wilt verwijderen van de DNS-zone.

    Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]

### <a name="specify-the-zone-using-a-zone-object"></a>Geeft u de zone een $zone-object gebruiken

Geef de zone met behulp van een $zone-object `Get-AzureRmDnsZone`. Deze bewerking is een optionele *-Force* -switch wordt onderdrukt de prompt die u wilt verwijderen van de DNS-zone. Net als bij `Set-AzureRmDnsZone`, geven de zone met een $zone-object kunt Etag controles zodat de wijzigingen worden niet verwijderd. <BR>
De optionele *-overschrijven* vlag onderdrukt deze controles. Zie [Etags en labels](dns-getstarted-create-dnszone.md#Etags-and-tags) voor meer informatie.

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsZone -Zone $zone [-Force] [-Overwrite]



De zone-object kan ook worden doorgesluisd in plaats van als parameter worden doorgegeven:

    Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone [-Force] [-Overwrite]

## <a name="next-steps"></a>Volgende stappen

Na het maken van een DNS-zone maken [Recordsets en records](dns-getstarted-create-recordset.md) als u wilt beginnen met het omzetten van namen voor uw Internet-domein.