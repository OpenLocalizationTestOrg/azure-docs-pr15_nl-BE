<properties
   pageTitle="Resource Manager en klassieke implementatie | Microsoft Azure"
   description="Beschrijving van de verschillen tussen het implementatiemodel Resource Manager en het klassieke (of Management Service) implementatiemodel."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/27/2016"
   ms.author="tomfitz"/>

# <a name="azure-resource-manager-vs-classic-deployment-understand-deployment-models-and-the-state-of-your-resources"></a>Azure Resource Manager versus klassiek implementatie: implementatiemodellen en de status van uw resources begrijpen

In dit onderwerp leert u Azure Resource Manager en klassieke implementatiemodellen, de status van uw resources, en waarom uw resources zijn geïmplementeerd met één of de andere. De resourcemanager en klassieke implementatiemodellen vertegenwoordigen op twee manieren implementeren en beheren van uw Azure-oplossingen. U ermee via twee verschillende API-sets en de gedistribueerde bronnen kunnen bevatten belangrijke verschillen. De twee modellen zijn niet volledig compatibel met elkaar. In dit onderwerp worden deze verschillen beschreven.

Om te vereenvoudigen de implementatie en het beheer van hulpbronnen, wordt aangeraden u Resource Manager gebruiken voor alle nieuwe resources. Indien mogelijk, wordt aangeraden dat u bestaande bronnen via bronnenbeheerder opnieuw implementeren.

Als u nieuwe Resource Manager te zijn, kunt u lees eerst de terminologie in het [overzicht van Azure Resource Manager](azure-resource-manager/resource-group-overview.md)worden gedefinieerd.

## <a name="history-of-the-deployment-models"></a>Geschiedenis van de implementatiemodellen

Azure die oorspronkelijk het klassieke implementatiemodel. In dit model wordt bestonden elke resource onafhankelijk; Er is geen manier om te groeperen van verwante bronnen. In plaats daarvan moest u handmatig bijhouden welke resources bestaat uit een oplossing of -toepassing, en vergeet niet te beheren in een gecoördineerde benadering. Een oplossing wordt geïmplementeerd, moest u elke bron afzonderlijk via de klassieke portal maken of een script maken dat de middelen in de juiste volgorde geïmplementeerd. Als u wilt verwijderen van een oplossing, moest u elke bron afzonderlijk verwijderen. U kunt niet gemakkelijk toepassen en opstellen voor verwante resources bijwerken. Ten slotte kunt u niet toepassen codes bronnen label met voorwaarden die u helpen uw bronnen bewaken en beheren van facturering.

Azure geïntroduceerd in 2014, Resource Manager, die het concept van een groep worden toegevoegd. Een resourcegroep is een container voor resources die de levensduur van een gemeenschappelijke delen. Het implementatiemodel Resource Manager biedt verschillende voordelen:

- U kunt implementeren, beheren en controleren de services voor uw oplossing als een groep, in plaats van afzonderlijk verwerken van deze services.
- Als u herhaaldelijk implementeren de oplossing gedurende de gehele levenscyclus en vertrouwen die uw resources worden ingezet in een consistente status hebben.
- U kunt toegangsbeheer toepassen op alle resources in de resourcegroep en de beleidsregels worden automatisch toegepast wanneer u nieuwe resources worden toegevoegd aan de resourcegroep.
- U kunt labels toepassen op de bronnen logisch ordenen van alle resources in uw abonnement.
- U kunt JavaScript Object Notation (JSON) gebruiken voor het definiëren van de infrastructuur voor uw oplossing. De JSON-bestand staat bekend als een bronnenbeheerder sjabloon.
- U kunt de afhankelijkheden tussen resources, zodat deze kunnen worden gebruikt in de juiste volgorde definiëren.

Als bronbeheer is toegevoegd, worden alle resources zijn met terugwerkende kracht aan resourcegroepen standaard toegevoegd. Als u een bron via de klassieke implementatie nu maakt, wordt de resource automatisch gemaakt binnen een groep standaard bron voor die service, zelfs als u niet hebt opgegeven die bronnengroep op implementatie. Echter betekent alleen binnen een groep bestaande niet dat de resource aan de bronnenbeheerder model is geconverteerd. Bekijken we hoe elke service omgaat met de van twee implementatiemodellen in de volgende sectie. 

## <a name="understand-support-for-the-models"></a>Ondersteuning voor de modellen begrijpen 

Bij het bepalen van welk implementatiemodel te gebruiken voor uw resources zijn er drie scenario's rekening houden met:

1. De service ondersteunt bronnenbeheer en biedt slechts één type.
2. De service, Resource Manager ondersteunt maar biedt twee typen - één Resource Manager en één voor klassieke. Dit scenario geldt alleen voor virtuele machines, rekeningen voor opslag en virtuele netwerken.
3. De service biedt geen ondersteuning voor Resource Manager.

Om te ontdekken of een service Resource Manager ondersteunt, Zie [Bronbeheer providers ondersteund](resource-manager-supported-services.md).

Als de service die u wilt gebruiken niet Resource Manager ondersteunt, moet u de klassieke implementatie met blijven.

Als de service Resource Manager en **niet** een virtuele machine, opslag-account of een virtueel netwerk ondersteunt, kunt u Resource Manager zonder enige complicaties.

Voor virtuele machines, rekeningen voor opslag en virtuele netwerken, als de bron is gemaakt door middel van klassieke implementatie, moet u verder via de klassieke bewerkingen worden uitgevoerd. Als de virtuele machine opslag account, of virtueel netwerk via de implementatie van bronbeheer is gemaakt, moet u de bewerkingen van de bronnenbeheerder blijven. Dit onderscheid kunt krijgen verwarrend wanneer uw abonnement een combinatie van resources die zijn gemaakt met behulp van bronbeheer en klassieke implementatie bevat. Deze combinatie van resources kunnen onverwachte resultaten kunt maken omdat de bronnen niet bewerkingen ondersteunen.

In sommige gevallen een bronnenbeheerder opdracht gegevens over een resource die is gemaakt via de klassieke implementatie kunt ophalen of een administratieve taken zoals het verplaatsen van een klassieke resource aan een andere resource kan uitvoeren. Maar deze gevallen geeft niet de indruk dat het type Resource Manager-bewerkingen ondersteunt. Stel dat u een groep met een virtuele machine die is gemaakt met de klassieke implementatie. Als u de volgende Resource Manager PowerShell-opdracht uitvoert:

    Get-AzureRmResource -ResourceGroupName ExampleGroup -ResourceType Microsoft.ClassicCompute/virtualMachines

Wordt de virtuele machine:
    
    Name              : ExampleClassicVM
    ResourceId        : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.ClassicCompute/virtualMachines/ExampleClassicVM
    ResourceName      : ExampleClassicVM
    ResourceType      : Microsoft.ClassicCompute/virtualMachines
    ResourceGroupName : ExampleGroup
    Location          : westus
    SubscriptionId    : {guid}

De bronnenbeheerder cmdlet **Get-AzureRmVM** retourneert echter alleen virtuele machines via bronnenbeheerder geïmplementeerd. De volgende opdracht resulteert niet in de virtuele machine gemaakt via de klassieke implementatie.

    Get-AzureRmVM -ResourceGroupName ExampleGroup

Alleen de resources gemaakt met behulp van bronbeheer ondersteuning tags. U kunt labels toepassen op klassieke bronnen.

## <a name="resource-manager-characteristics"></a>Kenmerken van de Resource Manager

Om u te helpen begrijpen van de twee modellen, bekijken we de kenmerken van de Resource Manager typen:

- Via de [portal Azure](https://portal.azure.com/)gemaakt.

     ![Azure portal](./media/resource-manager-deployment-model/portal.png)

     Voor Compute, Storage en netwerk bronnen hebt u de mogelijkheid van het gebruik van Resource Manager of klassieke implementatie. Selecteer de **Resource Manager**.

     ![Resource Manager distributie](./media/resource-manager-deployment-model/select-resource-manager.png)

- Gemaakt door de Resource Manager versie van de Azure PowerShell-cmdlets. Deze opdrachten hebben de indeling *Werkwoord-AzureRmNoun*.

        New-AzureRmResourceGroupDeployment

- Via de [Azure Resource Manager REST API](https://msdn.microsoft.com/library/azure/dn790568.aspx) voor overige bewerkingen gemaakt.

- Gemaakt door middel van Azure CLI-opdrachten uitvoeren in de modus van de **arm** .

        azure config mode arm
        azure group deployment create 

- Het brontype is niet **(klassiek)** opnemen in de naam. De volgende afbeelding ziet het type account voor **opslag**.

    ![Web app.](./media/resource-manager-deployment-model/resource-manager-type.png)

## <a name="classic-deployment-characteristics"></a>Klassieke implementatie kenmerken

U kan ook de klassieke implementatiemodel kennen als het model voor het beheer van de Service.

Gemaakt in het model Klassiek deployment resources delen de volgende kenmerken:

- Gemaakt door middel van de [klassieke portal](https://manage.windowsazure.com)

     ![Klassieke portal](./media/resource-manager-deployment-model/classic-portal.png)

     Of de Azure portal en geeft u **klassieke** implementatie (voor Compute, Storage en netwerken).

     ![Klassieke implementatie](./media/resource-manager-deployment-model/select-classic.png)

- Gemaakt door middel van de Service Management-versie van de Azure PowerShell-cmdlets. De namen van deze opdrachten hebben de indeling *Werkwoord-AzureNoun*.

        New-AzureVM 

- Via de [Service Management REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx) voor overige bewerkingen gemaakt.
- Gemaakt door middel van Azure CLI-opdrachten uitvoeren in de modus voor **asm** .

        azure config mode asm
        azure vm create 

- Het resourcetype bevat **(klassiek)** in de naam. De volgende afbeelding ziet het type **opslag**account (klassiek).

    ![klassieke type](./media/resource-manager-deployment-model/classic-type.png)

U kunt de Azure portal voor het beheren van bronnen die zijn gemaakt via de klassieke implementatie.

## <a name="changes-for-compute-network-and-storage"></a>Wijzigingen voor compute, netwerk en opslag

In het volgende diagram wordt weergegeven compute-, netwerk- en bronnen via bronnenbeheerder geïmplementeerd.

![Resource Manager-architectuur](./media/virtual-machines-azure-resource-manager-architecture/arm_arch3.png)

Houd rekening met de volgende relaties tussen de bronnen:

- Alle bronnen bestaan binnen een resourcegroep.
- De virtuele machine is afhankelijk van een specifieke opslag-account die is gedefinieerd in de opslag resource voorziening voor het opslaan van de schijven in de blobopslag (vereist).
- De virtuele machine wordt verwezen naar een specifieke NIC is gedefinieerd in de netwerkprovider resource (vereist) en de beschikbaarheid instellen in de Compute resource provider gedefinieerde (optioneel).
- De NIC verwijst naar de virtuele machine toegewezen IP-adres (vereist), het subnet van het virtuele netwerk voor de virtuele machine (vereist) en aan een beveiligingsgroep netwerk (optioneel).
- Het subnet in een virtueel netwerk verwijst naar een netwerk beveiliging (optioneel).
- De load balancer-instantie verwijst naar de back-end groep IP-adressen die verwijst naar een load balancer openbare of particuliere IP-adres (optioneel) en bevatten de NIC van een virtuele machine (optioneel).

Hier vindt u de onderdelen en de bijbehorende relaties voor klassieke implementatie:

![klassieke architectuur](./media/virtual-machines-azure-resource-manager-architecture/arm_arch1.png)

De klassieke oplossing voor het hosten van een virtuele machine bevat:

- Een vereiste cloud-service als een container die fungeert als host voor virtuele machines (berekenen). Virtuele machines worden automatisch voorzien van een netwerk-interfacekaart (NIC) en een IP-adres toegewezen door Azure. De cloud-service bevat tevens een exemplaar van externe load balancer, een openbaar IP-adres en Standaardeindpunten toestaan van extern bureaublad en externe PowerShell verkeer voor Windows-gebaseerde virtuele machines en Secure Shell (SSH) verkeer voor Linux-gebaseerde virtuele machines.
- Een account vereist opslag waarin de VHD's voor een virtuele machine, met inbegrip van het besturingssysteem, tijdelijke en aanvullende gegevensschijven (opslag).
- Een optioneel virtueel netwerk dat fungeert als een extra container kunt u een structuur in een subnet maken en instellen van het subnet waarop de virtuele machine bevindt (netwerk).

De volgende tabel beschrijft de wijzigingen in de interactie tussen Compute-, netwerk- en resource-providers:

 Artikel | Klassiek | Resource Manager
 ---|---|---
| Cloud Service voor virtuele Machines |  Cloud-Service is een container voor het houden van de virtuele machines die de beschikbaarheid van het platform en Load Balancing vereist. | Cloud-Service is niet langer een object is vereist voor het maken van een virtuele Machine met behulp van het nieuwe model. |
| Virtuele netwerken | Een virtueel netwerk is optioneel voor de virtuele machine. Als opgenomen, kan niet het virtuele netwerk met Resource Manager worden geïmplementeerd. | Virtuele machine moet een virtueel netwerk die is geïmplementeerd met Resource Manager. |
| Opslag-Accounts | De virtuele machine moet een opslag-account waarin de VHD's voor het besturingssysteem, tijdelijke en aanvullende gegevensschijven opgeslagen. | De virtuele machine moet een opslag-account voor het opslaan van de schijven in de blobopslag. |
| Beschikbaarheid van Sets | Beschikbaarheid van het platform is aangegeven door de dezelfde 'AvailabilitySetName' op de virtuele Machines configureren. Het maximum aantal domeinen veroorzaakt is 2. | Beschikbaarheid is een bron die door de Microsoft.Compute Provider. Virtuele Machines die hoge beschikbaarheid nodig moeten worden opgenomen in het beschikbaar stellen. Het maximum aantal domeinen veroorzaakt is nu 3. |
| Affiniteit groepen | Affiniteit groepen zijn nodig voor het maken van virtuele netwerken. Met de introductie van regionale virtuele netwerken die was echter niet nodig meer. |Om te vereenvoudigen, bestaat het begrip affiniteit groepen niet in de API's blootgesteld via Azure Resource Manager. |
| Load Balancing    | Het maken van een Cloud-Service biedt een impliciete taakverdeling voor de virtuele Machines geïmplementeerd. | De taakverdeling is een bron die door de provider Microsoft.Network. De taakverdeling moet worden verwezen naar de primaire netwerkinterface van de virtuele Machines die moet worden verdeeld. Netwerktaakverdeling kunnen intern of extern zijn. Een load balancer-instantie verwijst naar de back-end groep IP-adressen die verwijst naar een load balancer openbare of particuliere IP-adres (optioneel) en bevatten de NIC van een virtuele machine (optioneel). [Meer informatie.](../articles/resource-groups-networking.md) |
|Virtuele IP-adres | Cloud Services krijgt u een standaard VIP (virtuele IP-adres) bij een VM wordt toegevoegd aan een cloud-service. Het virtuele IP-adres is het adres dat is gekoppeld aan de impliciete taakverdeling.    | Openbare IP-adres is een bron die door de provider Microsoft.Network. Openbare IP-adres kan (gereserveerd) statisch of dynamisch zijn. Dynamische openbare IP-adressen kunnen worden toegewezen aan een taakverdeling. Openbare IP-adressen kan worden beveiligd met behulp van beveiligingsgroepen. |
|Gereserveerde IP-adres|   U kunt een IP-adres in Azure reserveren en te koppelen aan een Cloud Service om ervoor te zorgen dat het IP-adres sticky is.   | Openbare IP-adres kan worden gemaakt in de modus 'Static' en biedt dezelfde mogelijkheid als een ' gereserveerde IP-adres'. Statische openbare IP-adressen kunnen alleen worden toegewezen aan een Load balancer nu. |
|Openbare IP-adres (PIP) per VM | Openbare IP-adressen kunnen ook worden gekoppeld aan een VM rechtstreeks. | Openbare IP-adres is een bron die door de provider Microsoft.Network. Openbare IP-adres kan (gereserveerd) statisch of dynamisch zijn. Echter worden alleen dynamische openbare IP-adressen toegewezen aan een netwerkinterface om nu een openbaar IP-adres per VM. |
|Eindpunten| Invoer eindpunten moest worden geconfigureerd op een virtuele Machine Connectivity voor bepaalde poorten open zijn. Een van de gemeenschappelijke modi van een verbinding met virtuele machines die zijn gedaan door het instellen van invoer eindpunten. | NAT regels voor binnenkomende verbindingen kunnen worden geconfigureerd voor netwerktaakverdeling te bereiken dezelfde mogelijkheid van de eindpunten op specifieke poorten voor het aansluiten van het VMs. |
|DNS-naam| Een cloud-service krijgt een impliciete globaal unieke DNS-naam. Bijvoorbeeld: `mycoffeeshop.cloudapp.net`. | DNS-namen zijn optionele parameters die kunnen worden opgegeven voor een resource openbare IP-adres. De FQDN-naam wordt in de volgende indeling: - `<domainlabel>.<region>.cloudapp.azure.com`. |
|Netwerkinterfaces | Primaire en secundaire netwerk-Interface en de bijbehorende eigenschappen zijn gedefinieerd als de configuratie van een virtuele machine. | Netwerk-Interface is een bron die door de Microsoft.Network Provider. De levenscyclus van de netwerkinterface is niet gebonden aan een virtuele Machine. Het verwijst naar de virtuele machine toegewezen IP-adres (vereist), het subnet van het virtuele netwerk voor de virtuele machine (vereist) en aan een beveiligingsgroep netwerk (optioneel). |

Meer informatie over virtuele netwerken met elkaar verbinden van verschillende modellen, Zie [virtuele netwerken verbinden van verschillende modellen in de portal](./vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="migrate-from-classic-to-resource-manager"></a>Migreren van klassiek tot Resource Manager

Als u klassieke deployment resources migreren naar deployment Resource Manager, Zie:

1. [Technische diepe kennismaking op platform ondersteund migratie van klassiek Azure Resource Manager](./virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
2. [Ondersteund platform migratie van IaaS bronnen van klassiek Azure Resource Manager](./virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
3. [IaaS resources migreren van klassiek Azure Resource Manager met Azure PowerShell](./virtual-machines/virtual-machines-windows-ps-migration-classic-resource-manager.md)
4. [IaaS resources migreren van klassiek Azure Resource Manager met behulp van de CLI Azure](./virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md)

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Kan ik een virtuele machine met behulp van bronbeheer Azure te implementeren in een virtueel netwerk of opslag account via klassieke implementatie maken?**

Dit wordt niet ondersteund. U kunt Azure Resource Manager niet gebruiken voor de implementatie van een virtuele machine in een virtueel netwerk die is gemaakt met de implementatie van klassieke.

**Kan ik een virtuele machine met Azure Resource Manager van een gebruiker-installatiekopie die is gemaakt met de Azure Service API's maken?**

Dit wordt niet ondersteund. U kunt echter de VHD-bestanden kopiëren vanaf een account van opslag die is gemaakt met API's voor de Service en deze toevoegen aan een nieuwe account die via Azure Resource Manager.

**Wat is de impact op het contingent voor mijn abonnement?**

De quota voor de virtuele machines, virtuele netwerken en opslag-accounts die zijn gemaakt via de bronnenbeheerder Azure staan los van andere quota. Elk abonnement haalt quota's maken met behulp van de API's voor nieuwe bronnen. U kunt meer lezen over de aanvullende quota's [hier](../articles/azure-subscription-service-limits.md).

**Kan ik doorwerken op mijn geautomatiseerde scripts voor het inrichten van virtuele machines, virtuele netwerken en opslag rekeningen via de API's voor bronbeheer gebruiken?**

Blijven werken voor bestaande virtuele machines, virtuele netwerken die zijn gemaakt onder de Azure Service Management-modus de automatisering en scripts die u hebt gemaakt. De scripts moeten echter worden bijgewerkt zodat het nieuwe schema voor het maken van dezelfde bronnen via de modus Resource Manager.

**Waar vind ik voorbeelden van bronbeheer Azure sjablonen**

Een uitgebreide verzameling sjablonen voor starter kunt op [Azure Resource Manager QuickStart sjablonen](https://azure.microsoft.com/documentation/templates/)vinden.

## <a name="next-steps"></a>Volgende stappen

- Zie [Resource Manager sjabloon scenario](resource-manager-template-walkthrough.md)u helpt bij het maken van een sjabloon met een definitie van een virtuele machine, rekening voor opslag en virtuele netwerken.
- Zie [een toepassing met Azure Resource Manager sjabloon distribueren](resource-group-template-deploy.md)de opdrachten voor het implementeren van een sjabloon.
