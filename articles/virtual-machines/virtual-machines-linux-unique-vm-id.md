<properties
   pageTitle="Toegang tot de VM-ID"
   description="Beschrijving van de toegang tot en gebruik van de unieke ID voor Azure VM"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="kmouss"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="02/08/2016"
   ms.author="kmouss"/>
   
# <a name="accessing-and-using-azure-vm-unique-id"></a>Toegang tot en gebruik van de unieke ID voor Azure VM

De unieke ID voor Azure VM is een 128 bits id gecodeerd en opgeslagen in alle Azure IaaS VM van SMBIOS en kan momenteel worden gelezen met behulp van BIOS-opdrachten platform.

De unieke ID voor Azure VM is een alleen-lezen eigenschap. Unieke Azure VM-ID verandert niet na opnieuw opstarten, afsluiten (ofwel gepland voor niet-gepland), Start/Stop opgeheven toewijzen service retoucheren of herstellen in plaats. Als de VM is een momentopname kopiëren om een nieuw exemplaar maken, nieuwe Azure VM-ID is geconfigureerd.

> [AZURE.NOTE] Als u oudere VMs gemaakt en uitgevoerd, aangezien deze nieuwe functie kreeg uitgerold (18 September 2014), voer opnieuw uw VM om automatisch een Azure unieke-id.


Toegang tot Azure unieke VM ID vanuit de VM:


## <a name="create-a-vm"></a>Een VM maken
 

Zie [virtuele machines maken](virtual-machines-linux-creation-choices.md) voor meer informatie.


## <a name="connect-to-the-vm"></a>Verbinding maken met de VM
 

Zie voor meer informatie, [SSH van Linux](virtual-machines-linux-mac-create-ssh-keys.md)


## <a name="query-vm-unique-id"></a>De unieke ID voor query-VM

Opdracht (wordt **Ubuntu**):

    sudo dmidecode | grep UUID
    
Voorbeeld van de verwachte resultaten:

    UUID: 090556DA-D4FA-764F-A9F1-63614EDA019A
    
Door Big Endian bit te bestellen, is de werkelijke unieke VM-ID in dit geval:

    DA 56 05 09 – FA D4 – 4f 76 - A9F1-63614EDA019A
    
    
Azure VM unieke ID kan worden gebruikt in verschillende scenario's of de VM op Azure wordt uitgevoerd of op gebouwen en kunt u uw licentie, rapportage of algemene bijhouden vereisten die moet u in uw IaaS Azure-implementaties wellicht. Veel onafhankelijke softwareleveranciers toepassingen bouwen en certificeren ze op Azure verlangen een Azure VM gedurende de gehele levenscyclus te identificeren en kunt u zien of de VM wordt uitgevoerd op Azure, on-Premises of op andere cloud-providers. Dit platform-id kan bijvoorbeeld helpen detecteren als de software is een geldig gebruiksrecht of help om gegevens naar de bron, zoals VM te helpen bij het instellen van de juiste statistieken voor het juiste platform te houden en deze gegevens onder andere gebruik correleren correleren.
