<properties
    pageTitle="Azure DevTest Labs Veelgestelde vragen | Microsoft Azure"
    description="Antwoorden vinden op veelgestelde vragen van Azure DevTest Labs"
    services="devtest-lab,virtual-machines"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="tarcher"/>

# <a name="azure-devtest-labs-faq"></a>Azure DevTest Labs Veelgestelde vragen

Dit artikel bevat antwoorden op enkele van de meest gestelde vragen over Azure DevTest Labs.

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="general"></a>Algemeen
- [Wat gebeurt er als mijn vraag hier niet wordt beantwoord?](#what-if-my-question-isnt-answered-here)
- [Waarom gebruikt u Azure DevTest Labs?](#why-should-i-use-azure-devtest-labs) 
- [Wat betekent "zorgeloze, self-service"?](#what-does-quotworry-free-self-servicequot-mean)
- [Hoe kan ik de Azure DevTest Labs gebruiken?](#how-can-i-use-azure-devtest-labs) 
- [Hoe ben ik gefactureerd voor Azure DevTest Labs](#how-am-i-billed-for-azure-devtest-labs) 
 
## <a name="security"></a>Beveiliging 
- [Wat zijn de verschillende beveiligingsniveaus in Azure DevTest Labs?](#what-are-the-different-security-levels-in-azure-devtest-labs) 
- [Hoe maak ik een functie waarmee gebruikers een specifieke taak uit te voeren?](#how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task) 
 
## <a name="cicd-integration--automation"></a>CI/CD integratie en automatisering 
 
- [Is Azure DevTest Labs geïntegreerd met mijn CI/CD toolchain?](#does-azure-devtest-labs-integrate-with-my-cicd-toolchain) 
 
## <a name="virtual-machines"></a>Virtuele Machines 
 
- [Waarom kan ik bepaalde VMs in de blade Azure virtuele Machines die in Azure DevTest Labs niet zien?](#why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs) 
- [Wat is het verschil tussen de aangepaste afbeeldingen en formules?](#what-is-the-difference-between-custom-images-and-formulas) 
- [Hoe maak ik meerdere VMs met dezelfde sjabloon tegelijk?](#how-do-i-create-multiple-vms-from-the-same-template-at-once) 
- [Hoe verplaats ik mijn bestaande Azure VMs in mijn lab Azure DevTest Labs](#how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab) 
- [Kan ik meerdere schijven koppelen aan mijn VMs?](#can-i-attach-multiple-disks-to-my-vms) 
- [Hoe ik het automatiseren van VHD-bestanden te maken van aangepaste afbeeldingen uploaden?](#how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images) 
- [Hoe kan ik het automatiseren van het VMs in mijn lab verwijderen?](#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab)
 
## <a name="artifacts"></a>Artefacten 
 
- [Wat zijn onderdelen?](#what-are-artifacts) 
 
## <a name="lab-configuration"></a>Configuratie van een testlab 
 
- [Hoe maak ik een lab van een sjabloon Azure Resource Manager](#how-do-i-create-a-lab-from-an-azure-resource-manager-template) 
- [Waarom worden mijn VMs in verschillende resourcegroepen met willekeurige namen gemaakt? Kan ik wijzigen of deze brongroepen wijzigen?](#why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups) 
- [Hoeveel labs kan ik maken onder het hetzelfde abonnement?](#how-many-labs-can-i-create-under-the-same-subscription)
- [Hoeveel VMs kan ik per lab maken?](#how-many-vms-can-i-create-per-lab)
- [Hoe kan ik een directe link naar Mijn lab delen?](#how-do-i-share-a-direct-link-to-my-lab)
- [Wat is een Microsoft-account?](#what-is-a-microsoft-account)
 
## <a name="troubleshooting"></a>Het oplossen van problemen 
 
- [Mijn artefact is mislukt tijdens het maken van VM. Hoe kan ik dit oplossen?](#my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it) 
- [Waarom is niet mijn bestaande virtuele netwerk correct opslaan?](#why-isnt-my-existing-virtual-network-saving-properly)  

### <a name="what-if-my-question-isnt-answered-here"></a>Wat gebeurt er als mijn vraag hier niet wordt beantwoord?
Als uw vraag hier niet wordt weergegeven, laat het ons weten en wij helpen u een antwoord te vinden.

- Een vraag posten in de [thread Disqus](#comments) aan het einde van deze Veelgestelde vragen en oefenen met het team de Cache Azure en andere leden van de community over dit artikel.
- Als u wilt een grotere doelgroep bereiken, een vraag posten in het [forum MSDN voor Azure DevTest Labs](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs)en oefenen met het team Azure DevTest Labs en andere leden van de Gemeenschap.
- Als u een aanvraag voor een functie, dien uw aanvragen en ideeën op de [Azure DevTest Labs gebruiker Voice](https://feedback.azure.com/forums/320373-azure-devtest-labs).

### <a name="why-should-i-use-azure-devtest-labs"></a>Waarom gebruikt u Azure DevTest Labs? 
Azure DevTest Labs uw team tijd en geld bespaart. Ontwikkelaars kunnen maken van uw eigen omgeving met diverse verschillende grondslagen en artefacten gebruiken om snel te implementeren en configureren van toepassingen. Met behulp van aangepaste afbeeldingen en formules, kunnen virtuele machines worden opgeslagen als sjabloon en eenvoudig gereproduceerd. Bovendien bieden labs verschillende configureerbare beleidsvormen waarmee beheerders lab voor het verminderen van afval en omgevingen van een team beheren. Deze beleidsregels zijn auto-shutdown, drempel, kosten, maximale VMs per gebruiker en de maximale grootte van de VM. Lees het [Overzicht](devtest-lab-overview.md) voor een uitgebreidere uitleg van Azure DevTest Labs of bekijk de [inleidende video](/documentation/videos/videos/what-is-azure-devtest-labs). 

### <a name="what-does-worry-free-self-service-mean"></a>Wat betekent "zorgeloze, self-service"?
"Self-service zorgeloze," betekent dat ontwikkelaars en testers hun eigen omgeving maken als nodig is, en beheerders de beveiliging hebben van Azure DevTest Labs helpt te minimaliseren kent afval en kosten te bepalen. Beheerders kunnen opgeven welke groottes VM zijn toegestaan, wordt het maximum aantal VMs, en wanneer de VMs worden gestart en afgesloten. Azure DevTest Labs ook gemakkelijk kosten controleren en instellen van waarschuwingen om te blijven op de hoogte van hoe resources in een testomgeving worden gebruikt. 

### <a name="how-can-i-use-azure-devtest-labs"></a>Hoe kan ik de Azure DevTest Labs gebruiken? 
Azure DevTest Labs is handig wanneer u vereisen dev- of testomgeving, en u wilt ze snel reproduceren en/of ze met kostenbesparende beleid beheren. 

Hier volgen enkele scenario's die onze klanten met Azure DevTest Labs voor: 

- Dev- en testomgevingen voor op één plaats beheren, met behulp van beleid om kosten en aangepaste images te delen binnen het team is gebaseerd.
- Een toepassing met behulp van aangepaste afbeeldingen op te slaan van de schijf-staat gedurende de ontwikkelingsstadia ontwikkelen.
- De kosten met betrekking tot de voortgang bijhouden. 
- Het maken van massale testomgevingen voor het testen van quality assurance.
- Artefacten en formules gebruiken om gemakkelijk te configureren en een toepassing in verschillende omgevingen te reproduceren. 
- VMs voor hackathons (dev of test samenwerkingsmogelijkheden) distribueren en vervolgens gemakkelijk verwijderen van gegevens van deze wanneer de gebeurtenis wordt beëindigd. 

### <a name="how-am-i-billed-for-azure-devtest-labs"></a>Hoe ben ik gefactureerd voor Azure DevTest Labs 
Azure DevTest Labs is een gratis service, wat betekent dat labs maken en configureren van het beleid, sjablonen en artefacten vrij is. U betaalt alleen voor de Azure bronnen binnen uw labs, zoals virtuele machines, rekeningen voor opslag en virtuele netwerken gebruikt. Lees voor meer informatie over de kosten van resources lab, over [Azure DevTest Labs prijzen](https://azure.microsoft.com/pricing/details/devtest-lab/). 

### <a name="what-are-the-different-security-levels-in-azure-devtest-labs"></a>Wat zijn de verschillende beveiligingsniveaus in Azure DevTest Labs?  
Toegang wordt bepaald door [Azure_Role-Based Access Control RBAC ()](../active-directory/role-based-access-built-in-roles.md). Om te begrijpen hoe access werkt, is het handig om het verschil tussen een machtiging, een rol en een scope die door RBAC.

- **Machtiging** : een machtiging is een gedefinieerde toegang tot een specifieke actie. Een machtiging kan bijvoorbeeld leestoegang tot alle virtuele machines. 
- **Rol** - een rol is een verzameling machtigingen die kunnen worden gegroepeerd en worden toegewezen aan een gebruiker. 'Abonnement eigenaar' heeft bijvoorbeeld toegang tot alle bronnen binnen een abonnement. 
- **Scope** - een scope is een niveau binnen de hiërarchie van de Azure. Een scope kan bijvoorbeeld een resourcegroep of een enkele lab of het hele abonnement. 
 
Binnen het bereik van Azure DevTest Labs, er zijn twee soorten rollen om gebruikersmachtigingen te definiëren: lab-eigenaar en gebruiker lab.

- De **eigenaar lab** - eigenaar van een lab heeft de toegang tot bronnen in het lab. Dus kunnen ze beleid wijzigen, lezen en schrijven van een VMs, het virtuele netwerk wijzigen, enzovoort. 
- **Lab-gebruiker** - een lab-gebruiker kan alle lab-bronnen, zoals VMs, beleid en virtuele netwerken bekijken maar niet wijzigen beleid of een VMs gemaakt door andere gebruikers. Het is ook mogelijk te maken van aangepaste rollen in Azure DevTest Labs en leert u hoe u in het artikel, [gebruikersmachtigingen verlenen aan specifieke lab-beleid](devtest-lab-grant-user-permissions-to-specific-lab-policies.md). 
 
Omdat de scopes zijn hiërarchisch, wanneer een gebruiker machtigingen op een bepaald bereik heeft, worden ze automatisch machtigingen die op elk niveau bereik vallen. Bijvoorbeeld, als een gebruiker wordt toegewezen aan de rol van de eigenaar van het abonnement, hebben vervolgens ze toegang tot alle bronnen in een abonnement. Deze bronnen zijn alle virtuele machines en alle virtuele netwerken alle labs. De eigenaar van een abonnement neemt dus automatisch de rol van de eigenaar van het lab. Het omgekeerde is echter niet het geval. De eigenaar van een lab heeft toegang tot een lab een lagere bereik dan het soort abonnement is. Eigenaar van een lab dus niet kunnen zien van alle bronnen die het lab buiten of virtuele machines of virtuele netwerken. 

### <a name="how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task"></a>Hoe maak ik een functie waarmee gebruikers een specifieke taak uit te voeren?
Een uitgebreid artikel over het maken van aangepaste rollen en machtigingen toewijzen aan die rol kunt u hier vinden. Hier volgt een voorbeeld van een script dat wordt gemaakt van de rol "DevTest Labs geavanceerde gebruiker", die gemachtigd is om te starten en stoppen alle VMs in het lab:
 
    $policyRoleDef = Get-AzureRmRoleDefinition "DevTest Labs User" 
    $policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*') 
    $policyRoleDef.Id = $null 
    $policyRoleDef.Name = "DevTest Labs Advance User" 
    $policyRoleDef.IsCustom = $true 
    $policyRoleDef.AssignableScopes.Clear() 
    $policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>") 
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action") 
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action") 
    $policyRoleDef = New-AzureRmRoleDefinition -Role $policyRoleDef  
 
### <a name="does-azure-devtest-labs-integrate-with-my-cicd-toolchain"></a>Is Azure DevTest Labs geïntegreerd met mijn CI/CD toolchain? 
Als u VSTS gebruikt, is een [uitbreiding van Azure DevTest Labs taken](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) waarmee u uw pipeline release in Azure DevTest Labs automatiseren. Enkele van de toepassingen van deze uitbreiding zijn:

- Maken en een VM automatisch implementeren en configureren met de laatste build met Azure bestanden kopieert of PowerShell VSTS taken. 
- Automatisch vastleggen van de toestand van een VM na het testen om een bug op dezelfde VM voor verder onderzoek. 
- De VM aan het einde van de pijpleiding release verwijderen wanneer het niet langer nodig is. 

De volgende blog-updates bieden begeleiding en informatie over het gebruik van de extensie VSTS:
 
- [Azure DevTest Labs – VSTS extensie](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/) 
- [Implementatie van een nieuwe VM in een bestaande AzureDevTestLab van VSTS](http://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS) 
- [Met VSTS Release Management voor continue implementaties te AzureDevTestLabs](http://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs) 
 
Voor andere toolchains CI/CD worden alle eerder vermelde scenario's die kunnen worden bereikt via de uitbreiding van de taken VSTS op dezelfde manier bereikt via [Azure Resource Manager sjablonen](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) met [Azure PowerShell-cmdlets](../resource-group-template-deploy.md) en [.NET SDK's](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/)implementeren. U kunt ook [REST API's voor DevTest Labs](http://aka.ms/dtlrestapis) te integreren met uw toolchain.  

### <a name="why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs"></a>Waarom kan ik bepaalde VMs in de blade Azure virtuele Machines die in Azure DevTest Labs niet zien?
Wanneer een VM in Azure DevTest Labs is gemaakt, krijgt toestemming voor toegang tot die VM. U zijn kunt weergeven in de blade labs en de blade **virtuele Machines** . Gebruikers met de rol van DevTest Labs ziet alle virtuele machines die worden gemaakt in het lab via het lab van **alle virtuele Machines** blade. Echter, in de rol van DevTest Labs automatisch krijgt geen leestoegang tot de VM-bronnen die anderen hebben gemaakt. Deze VMs worden daarom niet weergegeven in de blade **virtuele Machines** . 

### <a name="what-is-the-difference-between-custom-images-and-formulas"></a>Wat is het verschil tussen de aangepaste afbeeldingen en formules? 
Een aangepaste afbeelding is een VHD (virtuele harde schijf), terwijl een formule is een afbeelding die u kunt configureren met extra instellingen die u kunt opslaan en reproduceren. Een aangepaste afbeelding wellicht beter als u wilt snel verschillende omgevingen te maken met dezelfde afbeelding elementaire, onveranderbaar zijn. Een formule kan worden beter als u de configuratie van uw VM met de meest recente bits, een virtueel netwerk-en subnet of een specifieke grootte te reproduceren. Zie het artikel, [Comparing aangepaste afbeeldingen en formules in Labs DevTest](devtest-lab-comparing-vm-base-image-types.md)voor een uitgebreidere uitleg. 
 
### <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>Hoe maak ik meerdere VMs met dezelfde sjabloon tegelijk? 
Kunt u de [VSTS taken extensie](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) of [een sjabloon Azure Resource Manager genereren](devtest-lab-add-vm-with-artifacts.md#save-arm-template) tijdens het maken van een VM en [implementeren van de sjabloon Azure Resource Manager van Windows PowerShell](../resource-group-template-deploy.md). 
 
### <a name="how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab"></a>Hoe verplaats ik mijn bestaande Azure VMs in mijn lab Azure DevTest Labs 
Wij ontwerpen een oplossing direct VMs verplaatsen naar Azure DevTest Labs, maar op dit moment kunt u uw bestaande VMs naar Azure DevTest Labs als volgt: 

1. Kopieer het VHD-bestand van uw bestaande VM met behulp van dit [Windows PowerShell-script](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1) 
1. Een [aangepaste afbeelding maken](devtest-lab-create-template.md) in uw testomgeving Azure DevTest Labs. 
1. Een VM in het lab van uw aangepaste installatiekopie maken 
 
### <a name="can-i-attach-multiple-disks-to-my-vms"></a>Kan ik meerdere schijven koppelen aan mijn VMs? 
Meerdere schijven koppelen aan VMs wordt ondersteund.  
 
### <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>Hoe ik het automatiseren van VHD-bestanden te maken van aangepaste afbeeldingen uploaden? 
Er zijn twee opties:

- [Azure-AzCopy](../storage/storage-use-azcopy.md#blob-upload) kan worden gebruikt om te kopiëren of VHD-bestanden uploaden naar de opslag-account die is gekoppeld aan het lab.
- [Microsoft Azure Opslagverkenner](../vs-azure-tools-storage-manage-with-storage-explorer.md) is een zelfstandige toepassing die wordt uitgevoerd op Windows, OSX en Linux.   
 
Ga als volgt te werk om de bestemming opslag account die is gekoppeld aan uw lab:

1. Log in om de [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). 
1. Selecteer **Resourcegroepen** in het linkerpaneel. 
1. Zoek en selecteer de resourcegroep die is gekoppeld aan uw lab. 
1. Selecteer een van de rekeningen van de opslag op het blad **Overzicht** . 
1. **BLOB's**selecteren.
1. Uploaden van bestanden in de lijst zoekt. Als er geen bestaat, terug te keren naar stap 4 en probeer een andere account voor opslag.
1. Gebruik de **URL** als de bestemming in de opdracht AzCopy.


### <a name="how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>Hoe kan ik het automatiseren van het VMs in mijn lab verwijderen?

Naast het VMs verwijderen uit uw lab in Azure portal, kunt u alle van het VMs in uw testomgeving met een PowerShell script verwijderen. Wijzigen in het volgende voorbeeld wordt de parameterwaarden onder de opmerking **waarden te wijzigen** . U kunt ophalen van de `subscriptionId`, `labResourceGroup`, en `labName` waarden uit het lab blade in Azure portal. 


    # Delete all the VMs in a lab
    
    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Login to your Azure account
    Login-AzureRmAccount
    
    # Select the Azure subscription that contains the lab. This step is optional
    # if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    
    # Get the lab that contains the VMs to delete.
    $lab = Get-AzureRmResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    
    # Get the VMs from that lab.
    $labVMs = Get-AzureRmResource | Where-Object { 
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.ResourceName -like "$($lab.ResourceName)/*"}
    
    # Delete the VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzureRmResource -ResourceId $labVM.ResourceId -Force
    }




### <a name="what-are-artifacts"></a>Wat zijn onderdelen? 
Artefacten zijn aanpasbare elementen die kunnen worden gebruikt voor de implementatie van uw meest recente bits of uw dev tools op een VM. Ze zijn gekoppeld aan uw VM tijdens het maken van een paar eenvoudige klikken en zodra de VM wordt ingericht, de artefacten implementeren en configureren van de VM. Er zijn diverse reeds bestaande artefacten in onze [opslagplaats voor openbare Github](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts), maar u kunt ook gemakkelijk [uw eigen artefacten auteur](devtest-lab-artifact-author.md). 

### <a name="how-do-i-create-a-lab-from-an-azure-resource-manager-template"></a>Hoe maak ik een lab van een sjabloon Azure Resource Manager 
We hebben een [opslagplaats van Github lab Azure Resource Manager-sjablonen](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) die u als implementeren kunt verstrekt-is of te wijzigen om aangepaste sjablonen voor uw labs. Elk van deze sjablonen is een koppeling waarop u klikken kunt als u wilt implementeren in het lab als-onder uw eigen Azure abonnement is of u kunt de sjabloon en [implementeren met behulp van PowerShell of Azure CLI](../resource-group-template-deploy.md).
 
### <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>Waarom worden mijn VMs in verschillende resourcegroepen met willekeurige namen gemaakt? Kan ik wijzigen of deze brongroepen wijzigen? 
Resourcegroepen worden gemaakt op deze manier Azure DevTest Labs voor het beheer van de machtigingen van de gebruiker en de toegang tot virtuele machines. Terwijl de VM u naar een andere groep met de gewenste naam verplaatsen kunt, wordt als zodanig niet aanbevolen. Wij werken aan deze ervaring om meer flexibiliteit te verbeteren.   
 
### <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>Hoeveel labs kan ik maken onder het hetzelfde abonnement? 
Er is geen specifieke beperking van het aantal labs die kunnen worden gemaakt per abonnement. De middelen zijn echter beperkt per abonnement. U kunt lezen over de [grenzen en de quota's ingesteld op de Azure abonnementen](../azure-subscription-service-limits.md) en [hoe deze limieten verhogen](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests). 
 
### <a name="how-many-vms-can-i-create-per-lab"></a>Hoeveel VMs kan ik per lab maken? 
Er is geen specifieke beperking van het aantal VMs die per laboratorium kan worden gemaakt. Op dit moment ondersteunt in het lab echter slechts ongeveer 40 VMs uitgevoerd op hetzelfde moment in de standaard opslag en 25 VMs tegelijkertijd worden uitgevoerd in de premium-opslag. Wij werken momenteel aan deze limieten verhogen. 

### <a name="how-do-i-share-a-direct-link-to-my-lab"></a>Hoe kan ik een directe link naar Mijn lab delen?

Als u wilt delen, een directe koppeling naar uw gebruikers lab kunt u de volgende procedure uitvoeren.

1. Ga naar het lab in Azure portal.
2. De lab-URL van uw browser kopiëren en delen met uw gebruikers lab. 

>[AZURE.NOTE] Als uw gebruikers lab externe gebruikers met een [MSA account zijn](#what-is-a-microsoft-account) en zij niet tot Active directory van uw bedrijf behoren, wordt een fout bij het navigeren naar de onderstaande koppeling. Als ze een foutbericht ontvangt, moet op hun naam in de rechterbovenhoek van de Azure portal en selecteer de map waar het lab uit de **map** -sectie van het menu bestaat.

### <a name="what-is-a-microsoft-account"></a>Wat is een Microsoft-account?

Een Microsoft-account wordt gebruikt voor bijna alles wat die u met Microsoft-apparaten en services doet. Het is een e-mailadres en wachtwoord die u gebruikt voor aanmelding bij Skype, Outlook.com, OneDrive, Windows Phone en Xbox LIVE- en betekent dit dat uw bestanden, foto's, contactpersonen en instellingen kunnen u volgen op elk apparaat. 

>[AZURE.NOTE] Microsoft-account gebruikt voor "Windows Live ID" worden genoemd.
 
### <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>Mijn artefact is mislukt tijdens het maken van VM. Hoe kan ik dit oplossen? 
Verwijzen naar het blogbericht [problemen met mislukte artefacten in AzureDevTestLabs](http://www.visualstudiogeeks.com/blog/DevOps/How-to-troubleshoot-failing-artifacts-in-AzureDevTestLabs) - geschreven door een van onze MVP's - meer informatie over het verkrijgen van logboeken over de mislukte artefact. 
 
### <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Waarom is niet mijn bestaande virtuele netwerk correct opslaan?  
Eén mogelijkheid is dat de virtuele-netwerknaam punten bevat. Als dit het geval is, probeert de perioden verwijderen of vervangen door koppeltekens en probeer het vervolgens opnieuw het virtuele netwerk op te slaan.
