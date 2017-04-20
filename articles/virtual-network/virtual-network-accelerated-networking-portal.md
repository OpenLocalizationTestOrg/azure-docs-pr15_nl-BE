<properties 
   pageTitle="Versnelde toegang voor een virtuele machine - Portal | Microsoft Azure"
   description="Informatie over het versnelde netwerken configureren voor een Azure virtuele machine met behulp van de Portal Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="jdial" />

# <a name="accelerated-networking-for-a-virtual-machine"></a>Versnelde netwerken voor een virtuele machine

> [AZURE.SELECTOR]
- [Azure Portal](virtual-network-accelerated-networking-portal.md)
- [PowerShell](virtual-network-accelerated-networking-powershell.md)

Versnelde netwerken kan één basis-i/o-virtualisatie (SR-IOV) met een virtuele machine (VM), de netwerkprestaties aanzienlijk verbeteren. Dit pad krachtige omzeilt de host van het gegevenspad latentie en jitter voor gebruik met de meest veeleisende werklast netwerk op ondersteunde VM typen CPU-gebruik verminderen. In dit artikel wordt uitgelegd hoe u met behulp van de Portal Azure versnelde netwerken in het implementatiemodel Azure Resource Manager configureren. U kunt ook een VM maken met behulp van Azure PowerShell versnelde toegang. Voor meer informatie over hoe, klikt u op het vak PowerShell aan het begin van dit artikel.

In de volgende afbeelding ziet u de communicatie tussen de twee virtuele machines (VM) met en zonder versnelde toegang:

![Vergelijking](./media/virtual-network-accelerated-networking-portal/image1.png)

Alle netwerkverkeer van en naar de VM moet zonder versnelde netwerken via de host en de virtual switch. De virtual switch biedt alle afdwingen, zoals netwerk-beveiligingsgroepen, access control lists, isolatie en andere gevirtualiseerde services voor het netwerkverkeer. Meer informatie leest u het artikel [Hyper-V-netwerk virtualisatie en Virtual Switch](https://technet.microsoft.com/library/jj945275.aspx) .

Versnelde netwerken, netwerkverkeer aankomt bij de netwerkkaart (NIC) en wordt vervolgens doorgestuurd naar de VM. Alle netwerkbeleid dat de virtual switch wordt toegepast zonder versnelde netwerken zijn overgenomen en toegepast in de hardware. Toepassen van beleid in hardware, kunt de NIC voor doorsturen van netwerkverkeer rechtstreeks naar de VM, overslaan van de host en de virtual switch, terwijl het beleid die het toegepast in de host.

De voordelen van versnelde toegang zijn alleen van toepassing op de VM die is ingeschakeld. Voor de beste resultaten is het ideaal om deze functie inschakelen voor ten minste twee VMs verbonden met de dezelfde VNet. Tijdens het communiceren via VNets of aansluitende op gebouwen, heeft deze functie een minimale invloed op de algehele latentie.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

##<a name="benefits"></a>Voordelen

- **Lagere latentie / hogere pakketten per seconde (pps):** De virtual switch van het gegevenspad verwijderen verwijdert de tijd pakketten besteden in de host voor de verwerking van beleid en neemt het aantal pakketten dat kan worden verwerkt binnen de VM.
- **Verlaagd jitter:** Virtual switch verwerking, is afhankelijk van het bedrag van het beleid dat moet worden toegepast en de belasting van de CPU die bezig is met de verwerking. Het afdwingen van beleid aan de hardware-offloading verwijdert die variabiliteit door het afleveren van pakketten rechtstreeks naar de VM, verwijderen van de host de VM-communicatie- en alle software-interrupts en Contextomschakelingen.
- **Verminderd CPU-gebruik:** De virtual switch in de host overslaat leidt tot minder CPU-gebruik voor het verwerken van netwerkverkeer.

## <a name="limitations"></a>Beperkingen

De volgende beperkingen bestaan bij het gebruik van deze mogelijkheid:
 
- **Network interface maken:** Versnelde netwerken kan alleen worden ingeschakeld voor een nieuwe netwerkinterface.  Deze kan niet worden ingeschakeld op een bestaand netwerkinterface.
- **VM maken:** Een netwerkinterface met versnelde netwerken ingeschakeld kan alleen worden gekoppeld aan een VM wanneer de VM wordt gemaakt. De netwerk-interface kan niet worden gekoppeld aan een bestaande VM.
- **Regio's:** Aangeboden in alleen de regio West Centraal US en West-Europa Azure. De set van de regio's wordt in de toekomst uitgebreid.
- **Ondersteund besturingssysteem:** Microsoft Windows Server 2012 R2 en Windows Server 2016 technische voorbeeld 5. Ondersteuning voor Linux en Windows Server 2012 zal spoedig worden toegevoegd.
- **VM-grootte:** Standard_D15_v2 en Standard_DS15_v2 zijn dat de enige ondersteunde formaten voor VM-exemplaar. Zie het artikel [Windows VM formaten](../virtual-machines/virtual-machines-windows-sizes.md) voor meer informatie. Het aantal ondersteunde formaten voor VM-exemplaar wordt in de toekomst uitgebreid.

Wijzigingen in deze beperkingen wordt aangekondigd via de pagina [updates Azure virtuele netwerken](https://azure.microsoft.com/updates/accelerated-networking-in-preview) .

## <a name="create-a-windows-vm-with-accelerated-networking"></a>Maak een Windows VM met versnelde netwerken

1. Voor de voorvertoning registreren door een e-mail te sturen naar [Abonnementen voor versnelde toegang](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) met uw abonnement-ID en het beoogde gebruik. Voer de overige stappen tot niet nadat u een e-mailbericht de mededeling dat u hebt geaccepteerd in de voorvertoning hebt ontvangen.
2. Aanmelden bij de Azure Portal op http://portal.azure.com.
3. Een VM maken met het voltooien van de stappen in het [maken van een Windows VM](../virtual-machines/virtual-machines-windows-hero-tutorial.md) -artikel de volgende opties selecteren:
    - Selecteer een besturingssysteem dat wordt vermeld in de sectie beperkingen van dit artikel.
    - Selecteer een locatie (regio), vermeld in de sectie beperkingen van dit artikel.
    - Selecteer een grootte VM is vermeld in de sectie beperkingen van dit artikel. Als een van de ondersteunde formaten niet wordt weergegeven, klikt u op **Alles weergeven** in de blade **Kies een formaat** een formaat selecteren in een uitgevouwen lijst.
    - In het blad **Instellingen** klikt u op *ingeschakeld* voor **Accelerated netwerken**, zoals in de volgende afbeelding:

        ![Instellingen](./media/virtual-network-accelerated-networking-portal/image3.png)

    >[AZURE.NOTE] De optie voor versnelde toegang alleen zijn zichtbaar als u hebt:
    >
    >- In het voorbeeld is geaccepteerd
    >- Ondersteund besturingssysteem, de locatie en de VM-grootten die zijn vermeld in de sectie beperkingen van dit artikel hebt geselecteerd.

5. Zodra de VM is gemaakt, downloadt u het [stuurprogramma versnelde netwerken](https://gallery.technet.microsoft.com/Azure-Accelerated-471b5d84), verbinding maken en de VM en uitvoeren van het installatieprogramma van het stuurprogramma in de VM-aanmelding.
6. Klik met de rechtermuisknop op de Windows-knop en klik op **Apparaatbeheer**. Controleer dat de **Mellanox ConnectX 3 virtuele functie Ethernet-Adapter** wordt weergegeven onder de optie **netwerk** als het is uitgevouwen, zoals in de volgende afbeelding:

    ![Apparaatbeheer](./media/virtual-network-accelerated-networking-portal/image2.png)