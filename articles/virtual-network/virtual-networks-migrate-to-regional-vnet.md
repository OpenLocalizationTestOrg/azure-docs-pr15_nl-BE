<properties 
   pageTitle="Affiniteit groepen migreren met regionale virtueel netwerk (VNet)"
   description="Meer informatie over het affiniteit groepen migreren naar regionale vnets"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-migrate-from-affinity-groups-to-a-regional-virtual-network-vnet"></a>Affiniteit groepen migreren met regionale virtueel netwerk (VNet)

U kunt een groep affiniteit ervoor te zorgen dat resources gemaakt binnen dezelfde groep affiniteit fysiek worden gehost door servers die dicht bij elkaar deze bronnen zijn voor snellere communicatie inschakelen. Affiniteit groepen waren in het verleden een vereiste voor het maken van virtuele netwerken (VNets). Op dat moment werkt de network manager-service beheerd VNets kan alleen binnen een reeks of schaaleenheid van fysieke servers. De reikwijdte van beheer van het netwerk naar een regio toegenomen architecturale verbeteringen.

Als gevolg van deze architecturale verbeteringen zijn affiniteit groepen niet langer aanbevolen of vereist voor virtuele netwerken. Het gebruik van de affiniteit voor VNets wordt vervangen door de regio's. Regionale VNets, VNets die gekoppeld aan de regio's zijn worden genoemd.

Bovendien is het raadzaam u affiniteit groepen in het algemeen niet gebruikt. Afgezien van de vereiste VNet waren affiniteit groepen ook belangrijk om te gebruiken, zodat bronnen, zoals het berekenen en opslag, dicht bij elkaar zijn geplaatst. Met de huidige Azure netwerkarchitectuur zijn niet langer deze plaatsing eisen nodig. Zie [affiniteit groepen en VMs](#Affinity-groups-and-VMs) paar resterende specifieke gevallen waar u kunt een groep affiniteit gebruikt.

## <a name="creating-and-migrating-to-regional-vnets"></a>Maken en migreren naar regionale VNets

*Regio*in de toekomst zullen, bij het maken van nieuwe VNets worden gebruikt. U ziet dit als een optie in de Portal beheren. Houd er rekening mee dat in het bestand met configuratie als *locatie geeft*.

>[AZURE.IMPORTANT] Hoewel het nog steeds technisch mogelijk is voor het maken van een virtueel netwerk die is gekoppeld aan een groep affiniteit, is er geen dwingende redenen zijn om dit te doen. Veel nieuwe voorzieningen, zoals netwerk-beveiligingsgroepen, zijn alleen beschikbaar wanneer u een regionale VNet en zijn niet beschikbaar voor virtuele netwerken die gekoppeld aan de affiniteit groepen zijn.

### <a name="about-vnets-currently-associated-with-affinity-groups"></a>Over de VNets die momenteel is gekoppeld aan de affiniteit groepen

VNets die gekoppeld aan de affiniteit groepen zijn zijn ingeschakeld voor de migratie naar regionale VNets. Als u wilt migreren naar een regionale VNet, als volgt te werk:

1. Het netwerk configuratiebestand exporteren. U kunt de PowerShell of de Portal beheren. Zie voor instructies met behulp van de Portal voor beheer van [uw VNet met een configuratiebestand netwerk configureren](virtual-networks-using-network-configuration-file.md).

1. Bewerk het configuratiebestand netwerk, de oude waarden vervangen door de nieuwe waarden. 

    > [AZURE.NOTE] De **locatie** is de regio die u hebt opgegeven voor de affiniteit groep die is gekoppeld aan uw VNet. Bijvoorbeeld, als uw VNet gekoppeld aan een groep affiniteit die in de Verenigde Staten West is, bevindt zich wanneer u migreert, moet de locatie verwijzen naar West VS. 
    
    De volgende regels in het configuratiebestand van uw netwerk, de waarden te vervangen door uw eigen bewerken: 

    **Oude waarde:** \<VirtualNetworkSitename = "VNetUSWest" AffinityGroup = "VNetDemoAG"\> 

    **Nieuwe waarde:** \<VirtualNetworkSitename = "VNetUSWest" Location = "US West"\>

1. Sla uw wijzigingen en [importeert u](virtual-networks-using-network-configuration-file.md) de netwerkconfiguratie op Azure.

>[AZURE.NOTE] Deze migratie leidt niet tot enige uitvaltijd voor uw service.

## <a name="affinity-groups-and-vms"></a>Affiniteit groepen en VMs

Zoals eerder vermeld, zijn niet meer in het algemeen affiniteit groepen aanbevolen voor VMs. Alleen als een set van VMs absoluut laagste netwerkvertraging tussen de VMs moet hebben, moet u een groep affiniteit gebruiken. VMs plaatst in een groep affiniteit, worden het VMs alle geplaatst in dezelfde compute cluster of schaal eenheid.

Het is belangrijk dat u met een groep affiniteit kan twee, dat mogelijk negatief is, consequenties:

- De set van VM omvang worden beperkt tot de VM formaten aangeboden door de schaal compute eenheid.

- Er is een grotere kans dat er een nieuwe VM toekennen. Dit gebeurt wanneer de specifieke schaaleenheid voor de groep affiniteit onvoldoende capaciteit heeft.

### <a name="what-to-do-if-you-have-a-vm-in-an-affinity-group"></a>Wat te doen als u een VM in een groep affiniteit hebt

VMs die zich momenteel in een groep affiniteit hoeft niet te worden verwijderd uit de groep affiniteit.

Zodra een VM is geïmplementeerd, wordt geïmplementeerd op een van één schaaleenheid. Affiniteit groepen de groep beschikbare VM formaten voor de implementatie van een nieuwe VM kunt beperken, maar is al een bestaande VM wordt geïmplementeerd met beperkte toegang tot de VM-formaten beschikbaar zijn in de schaaleenheid waarin de VM wordt geïmplementeerd. Om die reden heeft een VM verwijderen uit de groep affiniteit geen effect.
 
