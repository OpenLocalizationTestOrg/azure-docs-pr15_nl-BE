<properties
    pageTitle="Continue integratie in VS Team Services met Azure resourcegroep projecten | Microsoft Azure"
    description="Beschrijving van het instellen van de continue integratie in Visual Studio Team Services via Azure resourcegroep implementatieprojecten in Visual Studio."
    services="visual-studio-online"
    documentationCenter="na"
    authors="mlearned"
    manager="erickson-doug"
    editor="" />

 <tags
    ms.service="azure-resource-manager"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/01/2016"
    ms.author="mlearned" />

# <a name="continuous-integration-in-visual-studio-team-services-using-azure-resource-group-deployment-projects"></a>Continue integratie in Visual Studio Team Services implementatieprojecten Azure resourcegroep gebruiken

Voor de implementatie van een Azure-sjabloon die u nodig hebt voor het uitvoeren van taken via de verschillende fasen: bouwen, testen, kopiëren naar Azure (ook wel "Staging"), en de sjabloon distribueren.  Er zijn twee manieren om dit te doen in Visual Studio Team Services (VS Team Services). Beide methoden geven dezelfde resultaten, dus kies degene die het beste past bij uw workflow.

-   Voeg één stap in de definitie van de build die de PowerShell-script dat opgenomen in het project in Azure resourcegroep deployment (implementatie-AzureResourceGroup.ps1) wordt uitgevoerd. Het script artefacten gekopieerd en vervolgens de sjabloon implementeert.
-   Voeg dat meerdere VS Team Services ontwikkelen stappen elk uitvoeren van een taak van het werkgebied.

Dit artikel wordt beschreven hoe u de eerste optie (gebruik een definitie build de PowerShell-script uit te voeren). Een voordeel van deze optie is dat het script dat wordt gebruikt door ontwikkelaars in Visual Studio hetzelfde script dat wordt gebruikt door de VS Team Services. Deze procedure wordt ervan uitgegaan dat u al een Visual Studio-implementatieproject ingecheckt in de VS Team Services.

## <a name="copy-artifacts-to-azure"></a>Artefacten kopiëren naar Azure 

Ongeacht het scenario, als u alle onderdelen die nodig zijn voor de sjabloonimplementatie hebt moet u Azure Resource Manager toegang te verlenen tot deze. Deze artefacten kunnen u de volgende bestanden:

-   Geneste sjablonen
-   Van configuratiescripts en DSC-scripts
-   Binaire bestanden van toepassing

### <a name="nested-templates-and-configuration-scripts"></a>Geneste sjablonen en configuratiescripts
Wanneer u de sjablonen die worden geleverd door Visual Studio gebruikt (of gebouwd met Visual Studio fragmenten), het PowerShell-script fasen niet alleen de artefacten, dat ook de URI voor de middelen voor de verschillende installaties parameterizes. Het script en de artefacten kopieert naar een beveiligde container in Azure, een SaS-token voor de container die wordt gemaakt en vervolgens doorgegeven dat informatie over de sjabloonimplementatie. Zie [de sjabloonimplementatie van een maken](https://msdn.microsoft.com/library/azure/dn790564.aspx) voor meer informatie over geneste sjablonen.

## <a name="set-up-continuous-deployment-in-vs-team-services"></a>Doorlopende implementatie in VS Team Services instellen

De PowerShell-script aanroepen in VS Team Services, moet u uw definitie build bijwerken. De stappen zijn in het kort: 

1.  Bewerk de definitie van de build.
1.  Azure vergunning in de VS Team Services instellen.
1.  Een Azure PowerShell build stap die verwijst naar het PowerShell-script in de resourcegroep Azure implementatieproject toevoegen.
1.  Stel de waarde van de parameter *- ArtifactsStagingDirectory* om te werken met projecten in de VS Team Services is gebaseerd.

### <a name="detailed-walkthrough"></a>Gedetailleerde scenario

De volgende stappen helpt u bij de noodzakelijke stappen voor het continuous deployment in VS Team Services configureren 

1.  De definitie van de build VS Team Services bewerken en toevoegen van een stap Azure PowerShell build. Kies de build definitie onder de categorie **definities maken** en kies vervolgens de koppeling **bewerken** .

    ![][0]

1.  Een nieuwe stap voor **Azure PowerShell** build toevoegen aan de definitie van de build en kies vervolgens **Add bouwen stap...** de knop.

    ![][1]

1.  Kies de categorie **implementeren taak** , selecteert u de taak **Azure PowerShell** en kies vervolgens de knop **toevoegen** .

    ![][2]

1.  Kies de **Azure PowerShell** build stap en vul vervolgens de waarden.

    1.  Als u al een Azure-service-eindpunt dat is toegevoegd aan de VS Team Services, het abonnement te kiezen in de keuzelijst, vervolgkeuzelijst **Azure abonnement** en gaat u verder met de volgende sectie. 

        Als er geen een Azure-service-eindpunt in VS Team Services, moet u een toe te voegen. Deze onderafdeling gaat u door het proces. Als uw account Azure gebruikmaakt van een Microsoft-account (zoals Hotmail), moet u de volgende stappen uit om de verificatie van een Service Principal.

    1.  Kies de koppeling **beheren** naast het **Abonnement Azure** het vak vervolgkeuzelijst.

        ![][3]

    1. **Azure** kiezen in de keuzelijst, vervolgkeuzelijst **Nieuwe Service-eindpunt** .

        ![][4]

    1.  Selecteer in het dialoogvenster **Azure-abonnement toevoegen** de optie **Service Principal** .

        ![][5]

    1.  Uw abonnement Azure informatie toevoegen aan het dialoogvenster **Azure-abonnement toevoegen** . U moet de volgende items opgeven:
        -   Abonnement-Id
        -   De naam van abonnement
        -   Service Principal-Id
        -   Sleutel Service Principal
        -   Huurder-Id

    1.  Een naam van uw keuze aan het vak **abonnement** toevoegen. Deze waarde wordt weergegeven in de **Azure abonnement** vervolgkeuzelijst in VS Team Services later. 

    1.  Als u uw Azure abonnements-ID niet weet, kunt u een van de volgende opdrachten te werk.
        
        PowerShell-scripts gebruiken:

        `Get-AzureRmSubscription`

        Azure-CLI gebruiken:

        `azure account show`
    

    1.  Volg de procedure in de [portal met behulp van Active Directory maken toepassing en service principal](resource-group-create-service-principal-portal.md) of [verificatie van een service principal Azure Resource Manager](resource-group-authenticate-service-principal.md)als u een Service Principal-ID, Service Principal sleutel en ID van de huurder.

    1.  De Service Principal-ID en sleutel Service Principal-ID huurder waarden toevoegen aan het dialoogvenster **Azure-abonnement toevoegen** en klik vervolgens op **OK** .

        U hebt nu een geldig Service Principal met behulp van de Azure PowerShell script uitvoert.

1.  De build-definitie bewerken en kies de **Azure PowerShell** build stap. Selecteer het abonnement in de keuzelijst, vervolgkeuzelijst **Azure-abonnement** . (Als het abonnement niet wordt weergegeven, kiest u de knop **vernieuwen** vervolgens de koppeling **beheren** .) 

    ![][8]

1.  Geef het pad op het distribueren AzureResourceGroup.ps1 PowerShell-script. Hiertoe kiest u de knop met het weglatingsteken (...) naast het vak **Pad naar Script** navigeren naar het distribueren AzureResourceGroup.ps1 PowerShell script in de map **Scripts** van het project, selecteert u het en klik op **OK** . 

    ![][9]

1. Nadat u het script hebt geselecteerd, moet u het pad naar het script bijwerken zodat het wordt uitgevoerd vanuit de Build.StagingDirectory (dezelfde map die *ArtifactsLocation* is ingesteld op). U kunt dit doen door toevoeging van '$(Build.StagingDirectory)/ "aan het begin van het pad naar het script.

    ![][10]

1.  Voer de volgende parameters (op één regel) in het vak **Scriptparameters** . Als u het script in Visual Studio uitvoeren, kunt u zien hoe VS maakt gebruik van de parameters in het venster **uitvoer** . U kunt dit gebruiken als uitgangspunt voor het instellen van de parameterwaarden in de stap opbouwen.

  	| Parameter | Beschrijving|
  	|---|---|
  	| -ResourceGroupLocation           | De waarde voor geo-locatie waar de resourcegroep zich bevindt, zoals **eastus** of **Oost-Verenigde Staten**. (Enkele aanhalingstekens toevoegen als de naam een spatie). Zie [Azure regio's](https://azure.microsoft.com/en-us/regions/) voor meer informatie.|                                                                                                                                                                                                                              |
  	| -ResourceGroupName               | De naam van de resourcegroep die wordt gebruikt voor deze installatie.|                                                                                                                                                                                                                                                                                                                                                                                                                |
  	| -UploadArtifacts                 | Deze parameter, indien aanwezig, geeft aan dat artefacten moeten worden geüpload naar Azure uit het lokale systeem. U hoeft alleen deze schakeloptie instellen als uw sjabloonimplementatie vereist extra onderdelen die u wilt voorbereiden met de PowerShell-script (zoals configuratiescripts of geneste sjablonen).                                                                                                                                                                 |
  	| -StorageAccountName              | De naam van de account van de opslag in fase artefacten gebruikt voor deze installatie. Deze parameter is alleen vereist als u onderdelen naar Azure kopiëren wilt. Deze opslag account worden niet automatisch gemaakt door de implementatie, moet deze al bestaan.|                                                                                                                                                                                                                     |
  	| -StorageAccountResourceGroupName | De naam van de resourcegroep die is gekoppeld aan de account van de opslag. Deze parameter is alleen vereist als u onderdelen naar Azure kopiëren wilt.|                                                                                                                                                                                                                                                                                                                               |
  	| -Sjabloonbestand                    | Het pad naar het sjabloonbestand in het project resourcegroep Azure-implementatie. Om te zorgen voor meer flexibiliteit, een pad te gebruiken voor deze parameter die is gebaseerd op de locatie van het PowerShell-script in plaats van een absoluut pad.|
  	| -TemplateParametersFile          | Het pad naar het parameterbestand in het project resourcegroep Azure-implementatie. Om te zorgen voor meer flexibiliteit, een pad te gebruiken voor deze parameter die is gebaseerd op de locatie van het PowerShell-script in plaats van een absoluut pad.|
  	| -ArtifactStagingDirectory        | Met deze parameter kunt het PowerShell script weet in welke map van waaruit de binaire bestanden van het project moeten worden gekopieerd. Met deze waarde negeert de standaardwaarde die wordt gebruikt door het PowerShell-script. VS Team Services gebruiken, stelt u de waarde op: $(Build.StagingDirectory) - ArtifactStagingDirectory                                                                                                                                                                                              |

    Hier is een voorbeeld van script argumenten (regel gebroken voor leesbaarheid):

    ``` 
    -ResourceGroupName 'MyGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\azuredeploy.json' 
    -TemplateParametersFile '..\templates\azuredeploy.parameters.json' -UploadArtifacts -StorageAccountName 'mystorageacct' 
    –StorageAccountResourceGroupName 'Default-Storage-EastUS' -ArtifactStagingDirectory '$(Build.StagingDirectory)' 
    ```

    Wanneer u klaar bent, moet het vak **Scriptparameters** uitzien.

    ![][11]

1.  Nadat u de vereiste artikelen voor de Azure PowerShell bouwen stap hebt toegevoegd, kiest u de **wachtrij** maken om het project te maken. Het **bouwen van** scherm ziet u de uitvoer van het PowerShell-script.

## <a name="next-steps"></a>Volgende stappen

Lees [Azure Resource-beheer-overzicht](azure-resource-manager/resource-group-overview.md) voor meer informatie over bronbeheer Azure en Azure resourcegroepen.


[0]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough1.png
[1]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough2.png
[2]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough3.png
[3]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough4.png
[4]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough5.png
[5]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough6.png
[8]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough9.png
[9]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough10.png
[10]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough11b.png
[11]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough12.png
