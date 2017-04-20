<properties
    pageTitle="Implementeren van een App op de virtuele Machine schaal Sets | Microsoft Azure"
    description="Implementeren van een app op de virtuele Machine schaal Sets"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="guybo"/>

# <a name="deploy-an-app-on-virtual-machine-scale-sets"></a>Implementeren van een App op de virtuele Machine schaal Sets

Een toepassing die wordt uitgevoerd op een VM schaal ingesteld is gewoonlijk geïmplementeerd op drie manieren:

- Nieuwe software installeren op een besturingssysteemkopie bij de implementatie. Een besturingssysteemkopie in deze context is een installatiekopie van het besturingssysteem van de markt Azure, zoals Ubuntu, 16.04, Windows Server 2012 R2, enz.

U kunt nieuwe software installeren op een besturingssysteemkopie [VM-extensie](../virtual-machines/virtual-machines-windows-extensions-features.md). Een VM-extensie is software die wordt uitgevoerd wanneer een VM wordt geïmplementeerd. U kunt elke code die u graag bij de implementatie met de extensie van een aangepast script uitvoeren. [Hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-lapstack-autoscale) is een voorbeeld van Azure Resource Manager sjabloon met twee uitbreidingen voor VM: een Linux aangepast Script extensie Apache en PHP installeren en een diagnostische extensie uitzenden van prestatiegegevens door Azure automatisch schalen gebruikt.

Een voordeel van deze aanpak is een scheiding tussen de toepassing en het OS hebben en u uw toepassing afzonderlijk kunt onderhouden. Natuurlijk kunnen betekent dat er zijn ook meer bewegende delen en de implementatietijd VM niet langer zijn als er veel om het script te downloaden en te configureren.

**Als u gevoelige informatie in uw opdracht aangepast Script extensie (bijvoorbeeld een wachtwoord) doorgeeft, moet u opgeven de `commandToExecute` in de `protectedSettings` kenmerk van het aangepaste Script extensie in plaats van de `settings` kenmerk.**

- Maak een aangepaste installatiekopie VM met zowel het besturingssysteem als de toepassing in een enkele VHD. Hier wordt de schaal set bestaat uit een set van VMs gekopieerd van een afbeelding die is gemaakt door u, die u hebt te onderhouden. Hiervoor is geen extra configuratie bij de implementatie VM. Echter, in de `2016-03-30` versie van VM schaal Sets (en eerdere versies), de OS-diskettes voor de VMs in de schaal zijn beperkt tot een opslag op één account. U kunt dus maximaal 40 VMs hebben in een schaal, in plaats van de 100 VM per schaal limiet met platform afbeeldingen instellen. Overzicht [Schaal instellen ontwerpen](./virtual-machine-scale-sets-design-overview.md) voor meer informatie.

- Een platform of een aangepaste afbeelding die in feite een container host implementeren en installeren van uw toepassing als een of meer containers die u kunt met een hulpprogramma orchestrator of configuratie beheren. Het leuke van deze aanpak is dat u hebt uw cloud-infrastructuur van de toepassingslaag geabstraheerd en ze afzonderlijk kunt onderhouden.

## <a name="what-happens-when-a-vm-scale-set-scales-out"></a>Wat gebeurt er tijdens een VM schaal instellen van schalen?

De toepassing wordt automatisch geïnstalleerd wanneer u een of meer VMs aan een schaal instelt toevoegt door het vergroten van de capaciteit – of handmatig of via automatisch schalen –. Voor voorbeeld extensies gedefinieerd heeft als de schaal, worden ze uitgevoerd op een nieuwe VM telkens wanneer die deze wordt gemaakt. Als de set schaal is gebaseerd op een aangepaste afbeelding, is een nieuwe VM een kopie van de aangepaste bronafbeelding. Als de schaal VMs zijn container hosts, vervolgens moet u mogelijk de opstartcode te laden van de containers in een aangepast Script extensie of een extensie installeert een agent die door een cluster orchestrator (zoals Azure Container Service) geregistreerd.

## <a name="how-do-you-manage-application-updates-in-vm-scale-sets"></a>Hoe beheer ik updates voor toepassingen in VM schaal Sets?

Voor updates voor toepassingen in VM schaal Sets volgen drie belangrijkste methoden van de drie voorgaande implementatiemethoden van toepassing:

* Met de VM-extensies bijwerken. Een VM-extensies die zijn gedefinieerd voor een VM schaal ingesteld worden uitgevoerd telkens wanneer een nieuwe VM is geïmplementeerd, een bestaande VM is reimaged of een VM-extensie wordt bijgewerkt. Als uw toepassing, moet u rechtstreeks bijwerken van een toepassing via extensies is een levensvatbare benadering: de definitie van de extensie hoeft te werken. Een eenvoudige manier om dit te doen is door het wijzigen van de fileUris om te verwijzen naar de nieuwe software.

* De aanpak van onveranderbare aangepaste afbeelding. Wanneer u de toepassing (of onderdelen van app) Fancy in een VM-afbeelding kunt u zich concentreren op het opbouwen van een betrouwbare pijplijn bouwen, testen en implementatie van de afbeeldingen automatiseren. U kunt uw architectuur te vergemakkelijken snel verwisselen van een gefaseerde schaal in productie ontwerpen. Een goed voorbeeld van deze benadering is de [Azure Spinnaker-stuurprogramma werkt](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).

Verpakker en Terraform ook ondersteuning voor Azure Resource Manager, zodat u kunt ook afbeeldingen 'als code"definiëren en ze in Azure bouwen, gebruikt u de VHD in uw set schaal. Hierdoor zou dus worden echter problemen veroorzaken voor afbeeldingen op Marketplace waar extensions/aangepaste scripts belangrijker geworden omdat de bits uit de markt niet rechtstreeks bewerken.

* Containers worden bijgewerkt. Het beheer van de levenscyclus op een niveau boven de cloud infrastructuur, bijvoorbeeld door app onderdelen en toepassingen die abstracte in containers en deze containers via orchestrators container en managers als chef-kok/Puppet configuratie beheren.

De schaal ingesteld VMs is een stabiele substraat voor de containers en moet alleen worden incidenteel beveiligingsupdates en updates van OS gerelateerd. Zoals gezegd, is de Container Azure Service een goed voorbeeld van deze aanpak en bouwen van een service eromheen.

## <a name="how-do-you-roll-out-an-os-update-across-update-domains"></a>Hoe implementatie een OS-update in update domeinen?

Stel dat u de installatiekopie van het besturingssysteem bijwerken terwijl het uitvoeren van VM schaal ingesteld. Een manier om dit te doen is afbeeldingen voor het bijwerken van de VM VM één tegelijk. U kunt dit doen met PowerShell of Azure CLI. Er zijn afzonderlijke opdrachten voor het bijwerken van het model VM schaal instellen (hoe de configuratie is gedefinieerd) en 'handmatige upgrade' aanroepen op afzonderlijke VMs.

[Hier](https://github.com/gbowerman/vmsstools) is een voorbeeld van Python script automatiseren van het proces van het bijwerken van een domein één update VM schaal instellen op een tijdstip. (Voorbehoud: het is meer een bewijs van concept dan een harde productie-ready-oplossing – u wilt toevoegen sommige fout controle enz.).
