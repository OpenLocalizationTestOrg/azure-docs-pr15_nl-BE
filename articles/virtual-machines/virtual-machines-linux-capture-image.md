<properties
    pageTitle="Vastleggen van een Linux VM te gebruiken als een sjabloon | Microsoft Azure"
    description="Informatie over het opnemen en een afbeelding van een Linux-gebaseerde Azure virtual machine (VM) gemaakt met het implementatiemodel Azure Resource Manager generaliseren."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="iainfou"/>


# <a name="capture-a-linux-virtual-machine-running-on-azure"></a>Vastleggen van een Linux virtuele machine op Azure

Volg de stappen in dit artikel om te generaliseren en vastleggen uw Azure Linux virtuele machine (VM) in het implementatiemodel Resource Manager. Wanneer u de VM generalize, kunt u persoonlijke gegevens verwijderen en voorbereiden van de VM als een afbeelding moet worden gebruikt. Vervolgens legt u een afbeelding van het algemene virtuele harde schijf (VHD) voor het besturingssysteem, gekoppelde gegevensschijven VHD's en een [bronnenbeheerder sjabloon](../azure-resource-manager/resource-group-overview.md) voor nieuwe VM-implementaties. 

Netwerk resources instellen voor elke nieuwe VM wilt maken met behulp van de afbeelding VMs, en de sjabloon (een JavaScript Object Notation of JSON, bestand) gebruiken voor het implementeren van de vastgelegde beelden van de VHD. Op deze manier kunt u een VM met de huidige configuratie, vergelijkbaar met de manier waarop u afbeeldingen op Marketplace Azure repliceren.

>[AZURE.TIP]Als u een kopie maken van uw bestaande Linux VM met de gespecialiseerde staat voor back-up of het opsporen van fouten, Zie [maken een kopie van een Linux virtuele machine op Azure](virtual-machines-linux-copy-vm.md). En als u een VHD Linux vanuit een VM op ruimten te uploaden wilt, Zie [uploaden en een Linux VM van aangepaste schijfkopie maken](virtual-machines-linux-upload-vhd.md).  

## <a name="before-you-begin"></a>Voordat u begint

Zorg ervoor dat u aan de volgende vereisten voldoen:

* **Azure VM gemaakt in het implementatiemodel Resource Manager** - als u een Linux VM hebt gemaakt, kunt u de [portal](virtual-machines-linux-quick-create-portal.md), de [Azure CLI](virtual-machines-linux-quick-create-cli.md)of [Resource Manager-sjablonen](virtual-machines-linux-cli-deploy-templates.md). 

    Configureer de VM als nodig is. Bijvoorbeeld [gegevensschijven toevoegen](virtual-machines-linux-add-disk.md), updates toepassen en toepassingen installeren. 
* **Azure CLI** - de [Azure CLI](../xplat-cli-install.md) op een lokale computer installeren.

## <a name="step-1-remove-the-azure-linux-agent"></a>Stap 1: De agent Azure Linux verwijderen

Voer eerst de opdracht **waagent** met de parameter **identiteitsgegevens** op Linux VM. Met deze opdracht verwijdert u bestanden en gegevens om de VM klaar voor algemeen gebruik geschikt maken. Zie voor meer informatie de [gebruikershandleiding Azure Linux Agent](virtual-machines-linux-agent-user-guide.md).

1. Verbinding maken met uw Linux VM met behulp van een SSH-client.

2. Typ de volgende opdracht in het venster SSH:

    ```
    sudo waagent -deprovision+user
    ```
    >[AZURE.NOTE] Deze opdracht alleen uitvoeren op een VM die u wilt vastleggen als een afbeelding. Dit garandeert niet dat de afbeelding van alle gevoelige informatie is uitgeschakeld of geschikt voor distributie is.

3. Typ **j** om door te gaan. U kunt toevoegen de **-force** parameter om te voorkomen dat dit bevestigen.

4. Nadat de opdracht is voltooid, typt u **exit**. In deze stap sluit u de SSH-client.

    
## <a name="step-2-capture-the-vm"></a>Stap 2: Vastleggen van de VM

Gebruik de CLI Azure generaliseren en vastleggen van de VM. In de volgende voorbeelden kunt u voorbeeld parameternamen vervangen door uw eigen waarden. Voorbeeld van de parameternamen zijn **myResourceGroup**, **myVnet**en **myVM**.

5. Open de CLI Azure en [Meld u aan bij uw abonnement Azure](../xplat-cli-connect.md)van uw lokale computer. 

6. Zorg ervoor dat bronbeheer modus.

    ```
    azure config mode arm
    ```

7. Sluit de VM die u al deprovisioned met de volgende opdracht:

    ```
    azure vm deallocate -g MyResourceGroup -n myVM
    ```

8. Generalize VM met de volgende opdracht:

    ```
    azure vm generalize -g MyResourceGroup -n myVM
    ```

9. Nu voert u de opdracht **azure vm vastleggen** waarop de VM worden vastgelegd. In het volgende voorbeeld wordt de VHD image zijn opgenomen met namen die beginnen met **MyVHDNamePrefix**en de optie **-t** geeft u een pad naar de sjabloon **MyTemplate.json**. 

    ```
    azure vm capture MyResourceGroup MyResourceGroup MyVHDNamePrefix -t MyTemplate.json
    ```

    >[AZURE.IMPORTANT]In dezelfde account opslag die de oorspronkelijke VM gebruikt standaard de VHD afbeeldingsbestanden gemaakt. *Dezelfde opslag account* gebruiken voor het opslaan van de VHD's voor elke nieuwe VMs u van de afbeelding maakt. 

6. Om de locatie van een vastgelegde afbeelding de JSON-sjabloon te openen in een teksteditor. Zoek in de **storageProfile**, de **uri** van de **afbeelding** zich in de container **system** . Bijvoorbeeld is de URI van de installatiekopie van het besturingssysteem schijf vergelijkbaar met`https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`

## <a name="step-3-create-a-vm-from-the-captured-image"></a>Stap 3: Maak een VM uit de vastgelegde afbeelding
Nu de afbeelding met een sjabloon gebruiken voor het maken van een Linux VM. Deze stappen laten zien hoe u met de CLI Azure en de sjabloon voor het JSON die u vastgelegd om de VM in een nieuw virtueel netwerk maken.

### <a name="create-network-resources"></a>Netwerk resources maken

De sjabloon wilt gebruiken, moet u eerst een virtueel netwerk en NIC instellen voor uw nieuwe VM. Het is raadzaam dat u een bronnengroep voor deze bronnen maken op de locatie waar de VM-installatiekopie is opgeslagen. De opdrachten uitvoeren is vergelijkbaar met de volgende, vervangen door namen voor uw resources en juiste Azure locatie ("centralus" in deze opdrachten):

    azure group create MyResourceGroup1 -l "centralus"

    azure network vnet create MyResourceGroup1 myVnet -l "centralus"

    azure network vnet subnet create MyResourceGroup1 myVnet mySubnet

    azure network public-ip create MyResourceGroup1 myIP -l "centralus"

    azure network nic create MyResourceGroup1 myNIC -k mySubnet -m myVnet -p myIP -l "centralus"

### <a name="get-the-id-of-the-nic"></a>De id van het NIC

Als u wilt een VM uit de installatiekopie implementeren met behulp van de JSON die u hebt opgeslagen tijdens het opnemen, moet u de Id van de netwerkadapterkaart. Dit alsnog doen door de volgende opdracht uit te voeren:

    azure network nic show MyResourceGroup1 myNIC

De **Id** van de uitvoer is vergelijkbaar met`/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`



### <a name="create-a-vm"></a>Een VM maken
Nu de volgende opdracht uit te maken uw VM vanaf de vastgelegde afbeelding voor VM. Gebruik de parameter **-f** geeft het pad naar de opgeslagen sjabloon JSON-bestand.

    azure group deployment create MyResourceGroup1 MyDeployment -f MyTemplate.json

In de uitvoer van de opdracht, wordt u gevraagd om een nieuwe VM-naam, de gebruikersnaam admin en wachtwoord en de Id van de Netwerkkaart die u eerder hebt gemaakt.

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    vmName: myNewVM
    adminUserName: myAdminuser
    adminPassword: ********
    networkInterfaceId: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resource Groups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic

In het volgende voorbeeld ziet u wat er voor een geslaagde implementatie:

    + Het initialiseren van sjabloon configuraties en parameters
    + Maken van een distributie-info: sjabloon implementatie xxxxxxx gemaakt
    + Wacht op implementatie tot de gegevens: DeploymentName: MyDeployment gegevens: ResourceGroupName: MyResourceGroup1-gegevens: ProvisioningState: gegevens is voltooid: tijdstempel: xxxxxxx gegevens: modus: incrementele gegevens: naamwaarde


    data:    ------------------  ------------  -------------------------------------

    gegevens: vmName String myNewVM


    gegevens: String Standard_D1 vmSize


    gegevens: adminUserName String myAdminuser


    gegevens: adminPassword SecureString niet gedefinieerd


    Data: networkInterfaceId, String /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic info: groep implementatie OK maken, opdracht

### <a name="verify-the-deployment"></a>Controleer of de implementatie

Nu SSH naar de virtuele machine die u hebt gemaakt om te controleren of de implementatie en begin met de nieuwe VM. Zoeken via SSH verbinding wordt het IP-adres van de VM die u hebt gemaakt met de volgende opdracht:

    azure network public-ip show MyResourceGroup1 myIP

Het openbare IP-adres wordt vermeld in de uitvoer van de opdracht. Standaard verbinding met de Linux VM door SSH op poort 22.

## <a name="create-additional-vms"></a>Extra VMs maken
Gebruik de vastgelegde afbeelding en sjabloon wilt implementeren extra VMs met behulp van de stappen in de vorige sectie. Andere opties VMs maken van de afbeelding bevatten met behulp van een sjabloon quickstart of uitvoeren van de opdracht **maken van azure vm** .

### <a name="use-the-captured-template"></a>Gebruik de sjabloon opgenomen

Als u de vastgelegde afbeelding en sjabloon, als volgt (Zie de vorige sectie):

* Zorg ervoor dat uw afbeelding VM op dezelfde rekening opslag die fungeert als van uw VM VHD host.
* Kopieer de sjabloon JSON-bestand en geef een unieke naam voor de OS-schijf of de nieuwe VM VHD (VHD's). Bijvoorbeeld in de **storageProfile**, onder de **vhd**in **uri**, Geef een unieke naam voor de **osDisk** VHD vergelijkbaar met`https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`
* Maak een NIC in dezelfde of een andere virtuele netwerk.
* Maak een implementatie met de gewijzigde sjabloon JSON-bestand in de bronnengroep waarin u het virtuele netwerk hebt ingesteld.

### <a name="use-a-quickstart-template"></a>Gebruik een sjabloon quickstart

Als u wilt dat het netwerk automatisch bij het maken van een VM uit de afbeelding instellen, kunt u de resources in een sjabloon. Zie bijvoorbeeld de [sjabloon 101-vm-van-gebruiker-image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) van GitHub. Met deze sjabloon maakt een VM uit uw aangepaste installatiekopie en de benodigde virtuele netwerk openbare IP-adres en NIC-bronnen. Zie voor een overzicht van het gebruik van de sjabloon in de portal Azure, [het maken van een virtuele machine van een aangepaste installatiekopie met behulp van de sjabloon voor een Resource Manager](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/).

### <a name="use-the-azure-vm-create-command"></a>Gebruik de azure vm maken, opdracht

Meestal is het eenvoudigste is een sjabloon Resource Manager gebruiken voor het maken van een VM uit de afbeelding. Echter, kunt u de VM _imperatively_ met behulp van de opdracht **create azure vm** met de **Q-** (**--afbeelding urn**) parameter. Als u deze methode gebruikt, worden ook de parameter **-d** (**--os-schijf-vhd**) als u de locatie opgeven van het OS VHD-bestand voor de nieuwe VM wilt doorgeven. Dit bestand moet zich in de container VHD's van de opslag account waar u de afbeelding VHD-bestand is opgeslagen. De opdracht de VHD voor de nieuwe VM automatisch gekopieerd naar de container **VHD's** .

Voordat u de **azure vm maken** met de afbeelding, moet u de volgende stappen uitvoeren:

1.  Een resourcegroep maken of een bestaande brongroep voor de distributie te identificeren.

2.  Een openbaar IP-adresbron en een NIC-bron maken voor de nieuwe VM. Zie voor stapsgewijze instructies voor het maken van een virtueel netwerk, NIC en openbare IP-adres met behulp van de CLI, eerder in dit artikel. (**azure vm maken** een NIC ook kunt maken, maar moet u aanvullende parameters voor een virtueel netwerk en subnet doorgeven.)


Voer een opdracht waarmee URI's worden doorgegeven aan zowel het nieuwe OS VHD-bestand en de bestaande afbeelding. In dit voorbeeld wordt een grootte VM Standard_A1 wordt gemaakt in de regio Oost-VS.

    azure vm create MyResourceGroup1 myNewVM eastus Linux -d "https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix.vhd" -Q "https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd" -z Standard_A1 -u myAdminname -p myPassword -f myNIC

Voor extra opties, voert u `azure help vm create`.

## <a name="next-steps"></a>Volgende stappen

Zie de taken in uw VMs met de CLI beheren, [implementeren en beheren van virtuele machines met behulp van bronbeheer Azure sjablonen en de CLI Azure](virtual-machines-linux-cli-deploy-templates.md).
