

In dit artikel wordt beschreven hoe u een Azure Virtual Machine schaal instellen met behulp van een Visual Studio Resource groep implementatie implementeren.


[Azure Virtual Machine schaal Sets](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) zijn een middel Azure berekenen om te implementeren en beheren van een collectie van soortgelijke virtuele machines met eenvoudig geïntegreerde opties voor automatisch schalen en taakverdeling. U kunt inrichten en implementeren van VM schaal wordt ingesteld met behulp van [sjablonen Azure Resource Manager (ARM)](https://github.com/Azure/azure-quickstart-templates). ARM-sjablonen kunnen worden geïmplementeerd met behulp van de CLI Azure, PowerShell, REST en ook rechtstreeks vanuit Visual Studio. Visual Studio biedt een aantal voorbeeld sjablonen die kunnen worden geïmplementeerd als onderdeel van een project Azure Resource groep implementatie.

Resourcegroep Azure implementaties zijn een manier om te groeperen en een reeks verwante Azure bronnen publiceren in een implementatie met één bewerking. Voor meer informatie over deze hier: [maken en implementeren van Azure resourcegroepen via Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy/).

## <a name="pre-requisites"></a>Minimumvereisten

Implementeren van VM schaal stelt in Visual Studio aan de slag moet u het volgende:

- Visual Studio 2013 of 2015
- Azure SDK 2.7 of 2.8

Opmerking: Deze instructies wordt ervan uitgegaan dat u Visual Studio 2015 in combinatie met [Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

## <a name="creating-a-project"></a>Een Project maken

1. Maak een nieuw project in Visual Studio 2015 door het bestand **kiezen | Nieuw | Project**.

    ![Nieuw bestand][file_new]

2. Onder **Visual C# | Cloud**, kies **Azure Resource Manager** een project voor de implementatie van een ARM-sjabloon te maken.

    ![Project maken][create_project]

3.  Selecteer in de lijst met sjablonen de Linux of Windows virtuele Machine schaal instellen sjabloon.

    ![Selecteer sjabloon][select_Template]

4. Zodra het project is gemaakt ziet u PowerShell-scripts voor implementatie, een sjabloon Azure Resource Manager en een parameterbestand voor de virtuele Machine schaal ingesteld.

    ![Solution Explorer][solution_explorer]

## <a name="customize-your-project"></a>Uw project aanpassen

Nu kunt u de sjabloon wilt aanpassen aan de behoeften van uw toepassing, zoals VM extensie-eigenschappen toevoegen of bewerken van regels voor de taakverdeling. De VM schaal instellen sjablonen zijn standaard geconfigureerd voor de implementatie van de AzureDiagnostics-uitbreiding die eenvoudig automatisch schalen regels toevoegen. Het ook wordt uitgevoerd, implementeert een taakverdeling met een openbaar IP-adres, geconfigureerd met binnenkomende NAT regels welke kunt u verbinding maken met de VM-exemplaren met SSH (Linux) of RDP (Windows) – het poortbereik front-end begint bij 50000, wat betekent dat in het geval van Linux, als u SSH naar poort 50000 van openbare IP-adres (of de naam van het domein) wordt u doorgestuurd naar poort 22 van de eerste VM in de schaal instellen. Verbinding maken met poort 50001 wordt doorgestuurd naar poort 22 van de tweede VM enzovoort.

 Een goede manier om uw sjablonen met Visual Studio bewerken is de JSON-overzicht gebruiken voor het ordenen van de parameters, variabelen en bronnen. Met een goed begrip van het schema kunt Visual Studio aanwijzen fouten in de Template voordat u het implementeert.

![JSON Explorer][json_explorer]

## <a name="deploy-the-project"></a>Implementatie van het project

6. De sjabloon ARM implementeren op Azure te maken van de resource VM schaal ingesteld. Klik met de rechtermuisknop op het projectknooppunt, kiest u **implementeren | Implementatie van nieuwe**.

    ![Sjabloon distribueren][5deploy_Template]

7. Selecteer uw abonnement in het dialoogvenster 'Implementeren op Resource Group'.

    ![Sjabloon distribueren][6deploy_Template]

8. Hier kunt u ook een nieuwe bronnengroep Azure voor de implementatie van de sjabloon te maken.

    ![Nieuwe resourcegroep][new_resource]

9. Volgende klik op **Parameters bewerken** om op te geven van parameters die worden doorgegeven aan de sjabloon, worden bepaalde waarden, zoals de gebruikersnaam en wachtwoord voor het besturingssysteem zijn vereist voor het maken van de implementatie.

    ![Parameters bewerken][edit_parameters]

10. Klik op **Deploy**. Het venster **uitvoer** wordt de voortgang van de installatie weergegeven. Houd er rekening mee dat het de actie het script **Implementeren-AzureResourceGroup.ps1** wordt uitgevoerd.

    ![Uitvoervenster][output_window]

## <a name="exploring-your-vm-scale-set"></a>Verkennen van uw VM schaal instellen

Nadat de installatie is voltooid, ziet u de nieuwe VM schaal instellen in Visual Studio **Cloud Explorer** (de lijst vernieuwen). Cloud Explorer kunt u Azure bronnen in Visual Studio tijdens het ontwikkelen van toepassingen te beheren. U kunt ook uw VM schaal instellen bekijken in het Portal Azure en Azure Resource Explorer.

![Cloud Explorer][cloud_explorer]

 De portal biedt de beste manier om visueel beheren uw Azure infrastructuur met een webbrowser, terwijl Azure Resource Explorer een eenvoudige manier om explorer biedt en foutopsporing Azure resources, zodat een venster in de weergave' exemplaar' en waarin ook PowerShell-opdrachten voor de resources die u bekijkt. VM schaal Sets zijn in het afdrukvoorbeeld, wordt de Resource Explorer het meest gedetailleerd voor uw VM schaal wordt weergegeven.

## <a name="next-steps"></a>Volgende stappen

Zodra u hebt geïmplementeerd VM schaal stelt door middel van Visual Studio kunt u uw project aanpassen aan de toepassingsvereisten van uw verder aanpassen. Zo stelt automatisch schalen door het toevoegen van een bron van inzicht, infrastructuur toe te voegen aan uw sjabloon als zelfstandige VMs of distribueren van toepassingen met de extensie aangepast script. Een goede bron van voorbeeld sjablonen vindt u in de [Sjablonen van Azure Quickstart](https://github.com/Azure/azure-quickstart-templates) GitHub opslagplaats (zoeken naar "vmss").

[file_new]: ./media/virtual-machines-common-scale-sets-visual-studio/1-FileNew.png
[create_project]: ./media/virtual-machines-common-scale-sets-visual-studio/2-CreateProject.png
[select_Template]: ./media/virtual-machines-common-scale-sets-visual-studio/3b-SelectTemplateLin.png
[solution_explorer]: ./media/virtual-machines-common-scale-sets-visual-studio/4-SolutionExplorer.png
[json_explorer]: ./media/virtual-machines-common-scale-sets-visual-studio/10-JsonExplorer.png
[5deploy_Template]: ./media/virtual-machines-common-scale-sets-visual-studio/5-DeployTemplate.png
[6deploy_Template]: ./media/virtual-machines-common-scale-sets-visual-studio/6-DeployTemplate.png
[new_resource]: ./media/virtual-machines-common-scale-sets-visual-studio/7-NewResourceGroup.png
[edit_parameters]: ./media/virtual-machines-common-scale-sets-visual-studio/8-EditParameter.png
[output_window]: ./media/virtual-machines-common-scale-sets-visual-studio/9-Output.png
[cloud_explorer]: ./media/virtual-machines-common-scale-sets-visual-studio/12-CloudExplorer.png