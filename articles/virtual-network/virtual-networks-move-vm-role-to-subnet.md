<properties 
   pageTitle="Een rol of VM-instantie verplaatsen naar een ander subnet"
   description="Meer informatie over het VMs en rol exemplaren verplaatsen naar een ander subnet"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/22/2016"
   ms.author="jdial" />

# <a name="how-to-move-a-vm-or-role-instance-to-a-different-subnet"></a>Een rol of VM-instantie verplaatsen naar een ander subnet

PowerShell kunt u uw VMs uit één subnet verplaatsen naar een andere in hetzelfde virtuele netwerk (VNet). Rol exemplaren kunnen worden verplaatst door het bewerken van het CSCFG in plaats van met behulp van PowerShell.

>[AZURE.NOTE] Dit artikel bevat informatie die is gebaseerd op alleen Azure klassieke implementaties.

Waarom VMs verplaatsen naar een ander subnet? Migratie van subnet is handig wanneer het oudere subnet te klein is en kan niet worden uitgebreid als gevolg van de bestaande actieve VMs in dat subnet. In dat geval kunt u een nieuwe, grotere subnet maken en de VMs migreren naar het nieuwe subnet en nadat de migratie is voltooid, kunt u de oude leeg subnet verwijderen.

## <a name="how-to-move-a-vm-to-another-subnet"></a>Een VM verplaatsen naar een ander subnet

Als u wilt verplaatsen in een VM, de Set AzureSubnet PowerShell-cmdlet, met het volgende voorbeeld als sjabloon worden uitgevoerd. In het volgende voorbeeld zijn we TestVM verplaatsen van een subnet aanwezig, met Subnet 2. Zorg ervoor dat het voorbeeld aan uw omgeving bewerken. Houd er rekening mee dat wanneer u de cmdlet Update-AzureVM als onderdeel van een procedure uitvoert, wordt dit gedaan uw VM als onderdeel van het updateproces.

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
  	| Set-AzureSubnet –SubnetNames Subnet-2 `
  	| Update-AzureVM

Als u een statische intern particuliere IP-adres hebt opgegeven voor uw VM, hebt u deze instelling uitschakelen voordat u de VM naar een nieuw subnet verplaatsen kunt. In dat geval gebruikt u het volgende:

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
  	| Remove-AzureStaticVNetIP `
  	| Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
  	| Set-AzureSubnet -SubnetNames Subnet-2 `
  	| Update-AzureVM

## <a name="to-move-a-role-instance-to-another-subnet"></a>Een exemplaar van de functie verplaatsen naar een ander subnet

Een exemplaar van de functie verplaatsen, het CSCFG-bestand te bewerken. In het volgende voorbeeld verplaatst we 'Role0' in een virtueel netwerk *VNETName* van een subnet aanwezig met *Subnet 2*. Omdat het exemplaar van de rol is al geïmplementeerd, wijzigt u alleen de naam van het Subnet = Subnet 2. Zorg ervoor dat het voorbeeld aan uw omgeving bewerken.

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
