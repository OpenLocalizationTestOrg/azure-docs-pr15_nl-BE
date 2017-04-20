<properties 
   pageTitle="Het instellen van een statische particuliere IP-adres in de klassieke modus ausing de CLI | Microsoft Azure"
   description="Wat zijn statische particuliere IP-adressen (Spanningsdips) en hoe u ze kunt beheren in de klassieke modus met behulp van de CLI"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-azure-cli"></a>Het instellen van een statisch IP-adres persoonlijke Azure CLI (klassiek)

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dit artikel heeft betrekking op de klassieke implementatiemodel. U kunt ook [een statische particuliere IP-adres in het implementatiemodel Resource Manager beheren](virtual-networks-static-private-ip-arm-cli.md).

Het onderstaande voorbeeld Azure CLI opdrachten verwachten een eenvoudige omgeving al gemaakt. Als u wilt dat de opdrachten uitvoeren zoals deze worden weergegeven in dit document, eerst de testomgeving wordt beschreven in het [maken van een vnet](virtual-networks-create-vnet-classic-cli.md)te bouwen.

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Een statische particulier IP-adres opgeven bij het maken van een VM
U maakt een nieuwe VM met de naam *DNS01* in een nieuwe wolk service genaamd *TestService* op basis van het bovenstaande scenario, als volgt te werk:

1. Als u nooit CLI Azure gebruikt nog, Zie [installeren en configureren van de CLI Azure](../xplat-cli-install.md) en volg de instructies tot aan het punt waar u uw Azure-account en abonnement.
1. Voer de opdracht **azure service maken** voor het maken van de cloud-service.

        azure service create TestService --location uscentral

    Verwachte output:

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name TestService
        info:    service create command OK
    
2. Voer de opdracht **azure vm maken** voor het maken van de VM. Let op de waarde voor een statische particulier IP-adres. De lijst die wordt weergegeven nadat u de parameters die worden gebruikt door de uitvoer wordt uitgelegd.

        azure vm create -l centralus -n DNS01 -w TestVNet -S "192.168.1.101" TestService bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 adminuser AdminP@ssw0rd

    Verwachte output:

        info:    Executing command vm create
        warn:    --vm-size has not been specified. Defaulting to "Small".
        info:    Looking up image bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
        info:    Looking up virtual network
        info:    Looking up cloud service
        warn:    --location option will be ignored
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Retrieving storage accounts
        info:    Creating VM
        info:    OK
        info:    vm create command OK

    - **-l (of--locatie)**. Azure regio waar de VM worden gemaakt. Voor ons scenario *centralus*.
    - **-n (of--vm-naam)**. De naam van de VM worden gemaakt.
    - **-w (of--virtuele-netwerknaam)**. De naam van de VNet waar de VM worden gemaakt. 
    - **-S (of--static ip)**. Statische particuliere IP-adres voor de VM.
    - **TestService**. De naam van de cloud-service waar de VM worden gemaakt.
    - **bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2**. De afbeelding die wordt gebruikt voor het maken van de VM.
    - **adminuser**. Lokale administrator voor de Windows-VM.
    - **AdminP@ssw0rd**. Lokale administrator-wachtwoord voor de Windows-VM.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Het statische particuliere IP-adresgegevens ophalen voor een VM
Als u de statische persoonlijke IP-adresgegevens voor de VM gemaakt met het bovenstaande script, voert u de opdracht Azure CLI en houd rekening met de waarde voor *StaticIP netwerk*:

    azure vm static-ip show DNS01

Verwachte output:

    info:    Executing command vm static-ip show
    info:    Getting virtual machines
    data:    Network StaticIP "192.168.1.101"
    info:    vm static-ip show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Een statische particulier IP-adres van een VM verwijderen
Toegevoegd aan de VM in het bovenstaande script de volgende Azure CLI-opdracht voor het verwijderen van de statische particulier IP-adres:
    
    azure vm static-ip remove DNS01

Verwachte output:

    info:    Executing command vm static-ip remove
    info:    Getting virtual machines
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip remove command OK

## <a name="how-to-add-a-static-private-ip-to-an-existing-vm"></a>Een statische particuliere IP-adres toevoegen aan een bestaande VM
Toevoegen van een statische particuliere IP-adres voor de VM gemaakt met behulp van het script hierboven runt hij de volgende opdracht:

    azure vm static-ip set DNS01 192.168.1.101

Verwachte output:

    info:    Executing command vm static-ip set
    info:    Getting virtual machines
    info:    Looking up virtual network
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip set command OK

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [gereserveerde openbare IP-](virtual-networks-reserved-public-ip.md) adressen.
- Informatie over adressen [instantieniveau openbare IP-(ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Neem contact op met het [gereserveerde IP-REST API's](https://msdn.microsoft.com/library/azure/dn722420.aspx).
