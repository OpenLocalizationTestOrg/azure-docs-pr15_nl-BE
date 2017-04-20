<properties
    pageTitle="Maak een virtuele Machine schaal instellen met behulp van de portal Azure | Microsoft Azure"
    description="Schaal sets met Azure portal implementeren."
    keywords="virtuele machine schaal sets" 
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="gatneil"/>

# <a name="create-a-virtual-machine-scale-set-using-the-azure-portal"></a>Maak een virtuele Machine schaal instellen met behulp van de portal Azure

Deze zelfstudie toont hoe eenvoudig het is voor het maken van een virtuele Machine schaal instellen in een paar minuten met behulp van de portal Azure. Als u geen abonnement op een Azure, een [gratis account](https://azure.microsoft.com/free/) maken voordat u begint.

## <a name="choose-the-vm-image-from-the-marketplace"></a>Kies de VM-afbeelding in de markt

Vanuit de portal kunt u eenvoudig een schaal instelt met CentOS, CoreOS, Debian, Suse Open, Red Hat Enterprise Linux, SUSE Linux Enterprise Server, Ubuntu Server of installatiekopieën van Windows-servers implementeren.

Eerst, Ga naar de [Azure portal](https://portal.azure.com) in een webbrowser. Klik op `New`, zoeken naar `scale set`, en selecteer vervolgens de `Virtual machine scale set` post:

![ScaleSetPortalOverview](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalOverview.PNG)

## <a name="create-the-linux-virtual-machine"></a>De Linux virtuele machine maken

Nu kunt u de standaardinstellingen gebruiken en snel maken van de virtuele machine.

* Op de `Basics` blade, voer een naam voor de set schaal. Deze naam wordt de basis van de FQDN-naam van de verdeling van de belasting voor de schaal set, dus zorg ervoor dat de naam uniek is voor alle Azure.

* Selecteer het gewenste besturingssysteem typt, geef de gewenste gebruikersnaam en selecteer welke verificatiemethode typt u de voorkeur. Als u een wachtwoord, moet ten minste 12 tekens lang, en drie van de vier volgende complexiteitsvereisten voldoen: één kleine letter, één hoofdletter, één nummer en één speciaal teken. Zie voor meer informatie over de [vereisten voor gebruikersnaam en wachtwoord](../virtual-machines/virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm). Als u `SSH public key`, worden ervoor dat u alleen plakken in uw openbare sleutel, niet uw persoonlijke sleutel:

![ScaleSetPortalBasics](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalBasics.PNG)

* Voer de gewenste resource groepsnaam en locatie en klik vervolgens op `OK`.

* Op de `Virtual machine scale set service settings` blade: Voer het gewenste domein naam etiket (de basis van de FQDN-naam voor de verdeling van de belasting voor de set schaal). Dit label moet uniek zijn voor alle Azure.

* Kies uw gewenste besturingssysteem schijfkopie, exemplaar aantal en grootte van de machine.

* In- of uitschakelen automatisch schalen en configureren als ingeschakeld:

![ScaleSetPortalService](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalService.PNG)

* Op de `Summary` blade, wanneer de validatie is voltooid, klikt u op `OK`.

* Ten slotte op de `Purchase` blade, klikt u op `Purchase` start de schaal instellen voor implementatie.

## <a name="connect-to-a-vm-in-the-scale-set"></a>Verbinding maken met een VM in de schaal

Zodra uw set schaal geïmplementeerd is, gaat u naar de `Inbound NAT Rules` tabblad van de verdeling van de belasting voor de schaal:

![ScaleSetPortalNatRules](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalNatRules.PNG)

U kunt verbinding maken met elke VM in de schaal instellen met behulp van deze NAT-regels. Bijvoorbeeld voor een set Windows schaal, als er een NAT regel op inkomende poort 50000 u kan verbinding maken met deze computer via RDP op `<load-balancer-ip-address>:50000`. Voor een set van de schaal Linux u verbinding te maken met de opdracht `ssh -p 50000 <username>@<load-balancer-ip-address>`.

## <a name="next-steps"></a>Volgende stappen

Zie [deze documentatie](./virtual-machine-scale-sets-cli-quick-create.md)voor documentatie over het implementeren van schaal sets uit de CLI.

Zie [deze documentatie](./virtual-machine-scale-sets-windows-create.md)voor documentatie over het implementeren van schaal sets van PowerShell.

Zie [deze documentatie](./virtual-machine-scale-sets-vs-create.md)voor documentatie over het implementeren van de schaal instellen vanuit Visual Studio.

Voor algemene documentatie, bekijk de [overzichtspagina van documentatie voor schaal wordt ingesteld](./virtual-machine-scale-sets-overview.md).

Bekijk de [belangrijkste portaalpagina voor schaal sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/)voor algemene informatie.

