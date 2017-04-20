<properties
   pageTitle="Ontwikkel- en testomgevingen | Microsoft Azure"
   description="Leren werken met Azure Resource Manager sjablonen snel en consistent maken en verwijderen van ontwikkeling en testen van omgevingen."
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
   ms.date="01/22/2016"
   ms.author="tomfitz"/>

# <a name="development-and-test-environments-in-microsoft-azure"></a>Ontwikkel- en testomgevingen in Microsoft Azure

Aangepaste toepassingen zijn gewoonlijk geïmplementeerd op meerdere ontwikkel- en testomgevingen voor implementatie op productie. Wanneer omgevingen in gebouwen worden gemaakt, worden computerbronnen aangeschaft of toegewezen voor elke omgeving voor elke toepassing. De omgevingen bevatten vaak verschillende fysieke of virtuele machines met specifieke configuraties die handmatig zijn geïmplementeerd of complexe automatiseringsscripts. Implementaties vaak uren in beslag nemen en leiden tot inconsistente configuraties in omgevingen.

## <a name="scenario"></a>Scenario ##

Wanneer u gegevens leveren voor ontwikkel- en testomgevingen in Microsoft Azure, betaalt je alleen voor de resources die u gebruikt.  In dit artikel wordt uitgelegd hoe snel en consistent kunt maken, onderhouden, en verwijderen van ontwikkeling en testomgeving met behulp van bronbeheer Azure sjablonen en parameterbestanden, zoals hieronder wordt geïllustreerd.

![Scenario](./media/solution-dev-test-environments/scenario.png)

Ontwikkel- en testomgevingen voor drie zijn hierboven weergegeven.  Elke heeft webtoepassing en SQL database resources die zijn opgegeven in een sjabloon gemaakt.  De namen van de toepassing en de database in elke omgeving verschillen en zijn opgegeven in de unieke parameterbestanden voor elke omgeving.  

Als u niet vertrouwd met concepten Azure Resource Manager bent, het raadzaam het overzichtsartikel [Azure Resource Manager](azure-resource-manager/resource-group-overview.md) te lezen voordat u dit artikel leest.

U kunt eerst Doorloop de stappen in dit artikel vermelde zonder te lezen van de artikelen waarnaar wordt verwezen enige ervaring met behulp van bronbeheer Azure sjablonen snel krijgen. Nadat u één keer door de stappen hebt hebt, kun je steeds om te antwoorden op de vragen die uw eerste ontstaan de meeste tijd door met experimenteren verder met de stappen en door het lezen van de artikelen waarnaar wordt verwezen.

## <a name="plan-azure-resource-use"></a>Gebruik van Azure bronnen plannen
Zodra u een hoog niveau ontwerp voor uw toepassing hebt, kunt u definiëren:

- Welke bronnen Azure uw toepassing opgenomen. U kunt uw toepassing maken en deze implementeren als een Azure Web App met Azure SQL-Database.  U kunt uw toepassing in de virtuele machines met behulp van PHP en MySQL of IIS en SQL Server of andere onderdelen kan maken. Het artikel [Azure App Service, Cloud Services en vergelijking van virtuele Machines]( app-service-web/choose-web-site-cloud-service-vm.md) kunt u bepalen welke Azure resources u wilt gebruiken voor uw toepassing.
- Welke serviceniveauvereisten zoals beschikbaarheid, beveiliging en de schaal die voldoen aan uw toepassing.

## <a name="download-an-existing-template"></a>Een bestaande sjabloon downloaden
Een sjabloon Azure Resource Manager bepaalt alle Azure bronnen die gebruikmaakt van uw toepassing. Een aantal sjablonen bestaan al dat u kunt rechtstreeks in het portal voor Azure implementeren of downloaden, wijzigen en opslaan in een bronbeheersysteem met uw toepassingscode.  Voer de volgende stappen om een bestaande sjabloon te downloaden.

1. Bestaande sjablonen in de opslagplaats van de GitHub [Azure Quickstart sjablonen](https://github.com/Azure/azure-quickstart-templates/) zoeken. In de lijst ziet u een map '[201-web-app-sql-database](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database)'. Aangezien veel aangepaste toepassingen een webtoepassing en een SQL-database opnemen, wordt deze sjabloon gebruikt als een voorbeeld in de rest van dit artikel om te weten hoe u kunt sjablonen gebruiken. Het valt buiten het bestek van dit artikel volledig uitleggen van alles wat met deze sjabloon maakt en configureert, maar als u van plan bent naar werkelijke omgeving in uw organisatie te maken, zult u begrijpen dat door het lezen van het artikel [dient een web app met een SQL-Database](app-service-web/app-service-web-arm-with-sql-database-provision.md) .
Opmerking: dit artikel is geschreven voor de December 2015 versie van de sjabloon [201-web-app-sql-database](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database) . De koppelingen hieronder die verwijzen naar de sjabloon en parameterbestanden in die versie van de sjabloon.
2. Klik op het bestand [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.json) in de map 201-web-app-sql-database om de inhoud ervan weer te geven. Dit is het sjabloonbestand Azure Resource Manager. 
3. Klik op de knop "[Raw](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.json)" in de weergavemodus. 
4. De inhoud van dit bestand selecteren met de muis, en op uw computer opslaan als een bestand met de naam "TestApp1-Template.json". 
5. Bekijk de sjablooninhoud en u ziet het volgende:
 - Sectie **bronnen** : in dit gedeelte definieert de typen Azure bronnen die door deze sjabloon zijn gemaakt. Met deze sjabloon maakt onder andere brontypen [Azure Web App](app-service-web/app-service-web-overview.md) en [Azure SQL-Database](sql-database/sql-database-technical-overview.md) . Als u liever wilt uitvoeren en beheren van web- en SQL-servers in virtuele machines, kunt u de sjablonen "[iis-2vm-sql-1vm](https://github.com/Azure/azure-quickstart-templates/tree/master/iis-2vm-sql-1vm)" of "[licht-app](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app)", maar de instructies in dit artikel zijn gebaseerd op de sjabloon [201-web-app-sql-database](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database) .
 - De sectie **parameters** : in dit gedeelte definieert u de parameters die elke resource kan worden geconfigureerd met. Sommige van de parameters die zijn opgegeven in de sjabloon hebben 'standaardwaarde'-eigenschappen en andere niet. Bij het implementeren van Azure resources met een sjabloon, moet u waarden opgeven voor alle parameters waarvoor geen standaardwaarde-eigenschappen die zijn opgegeven in de sjabloon.  Als u geen waarden voor parameters met de eigenschappen standaardwaarde opgeeft, wordt de waarde die is opgegeven voor de parameter standaardwaarde in de sjabloon gebruikt.

Een sjabloon bepaalt welke Azure resources worden gemaakt en de parameters elke resource kan worden geconfigureerd met. U kunt meer informatie over sjablonen en het ontwerpen van uw eigen door de [Aanbevolen procedures voor het ontwerpen van Azure Resource Manager sjablonen](best-practices-resource-manager-design-templates.md) artikel lezen.

## <a name="download-and-customize-an-existing-parameter-file"></a>Downloaden en aanpassen van een bestaand parameterbestand

Hoewel u waarschijnlijk zult u de *dezelfde* Azure bronnen in elke omgeving hebt gemaakt, kunt u de configuratie van middelen van de *verschillende* in elke omgeving.  Dit is waar de parameterbestanden komen. Parameterbestanden maken die unieke waarden in elke omgeving bevatten door de volgende stappen uit te voeren.   

1. De inhoud van het bestand [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.parameters.json) in de map 201-web-app-sql-database bekijken. Dit is het parameterbestand voor het sjabloonbestand dat u in de vorige sectie hebt opgeslagen. 
2. Klik op de knop "[Raw](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.parameters.json)" in de weergavemodus. 
3. Met de muis, selecteert u de inhoud van dit bestand en sla ze op op drie afzonderlijke bestanden op uw computer met de volgende namen:
 - TestApp1-Parameters-Development.json
 - TestApp1-Parameters-Test.json
 - TestApp1-Parameters-Pre-Production.json

3. Met behulp van een tekst- of JSON editor bewerken de ontwikkeling omgeving parameterbestand die u hebt gemaakt in stap 3 vervangen van de waarden in de lijst rechts van de parameterwaarden in het bestand met de *waarden* in de lijst rechts van de **parameters** hieronder: 
 - **sitenaam**: *TestApp1DevApp*
 - **hostingPlanName**: *TestApp1DevPlan*
 - **siteLocation**: *US Central*
 - **servernaam**: *testapp1devsrv*
 - **serverLocation**: *US Central*
 - **administratorLogin**: *testapp1Admin*
 - **administratorLoginPassword**: *uw wachtwoord vervangen*
 - **databasenaam**: *testapp1devdb*

4. De Test-omgeving parameterbestand u hebt gemaakt in stap 3 vervangen met behulp van een tekst- of JSON-editor bewerken de de waarden in de lijst rechts van de parameterwaarden in het bestand met de *waarden* die worden vermeld aan de rechterkant van de onderstaande **parameters** :
 - **sitenaam**: *TestApp1TestApp*
 - **hostingPlanName**: *TestApp1TestPlan*
 - **siteLocation**: *US Central*
 - **servernaam**: *testapp1testsrv*
 - **serverLocation**: *US Central*
 - **administratorLogin**: *testapp1Admin*
 - **administratorLoginPassword**: *uw wachtwoord vervangen*
 - **databasenaam**: *testapp1testdb*

5. Met behulp van een tekst- of JSON editor bewerken de voorafgaand aan de productie-parameterbestand dat u in stap 3 hebt gemaakt.  Vervang de gehele inhoud van het bestand met wat lager dan is:

        {
          "$schema" : "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
          "contentVersion" : "1.0.0.0",
          "parameters" : {
        "administratorLogin" : {
          "value" : "testApp1Admin"
        },
        "administratorLoginPassword" : {
          "value" : "replace with your password"
        },
        "databaseName" : {
          "value" : "testapp1preproddb"
        },
        "hostingPlanName" : {
          "value" : "TestApp1PreProdPlan"
        },
        "serverLocation" : {
          "value" : "Central US"
        },
        "serverName" : {
          "value" : "testapp1preprodsrv"
        },
        "siteLocation" : {
          "value" : "Central US"
        },
        "siteName" : {
          "value" : "TestApp1PreProdApp"
        },
        "sku" : {
          "value" : "Standard"
        },
            "requestedServiceObjectiveName" : {
              "value" : "S1"
        }
          }
        }

In het bovenstaande parameterbestand voorafgaand aan de productie de **SKU's** en **requestedServiceObjectiveName** parameters zijn *toegevoegd*, dat ze in de ontwikkelings- en parameters zijn niet toegevoegd. Dit is omdat de standaardwaarden die zijn opgegeven voor deze parameters in de sjabloon, en in de ontwikkel- en testomgevingen, de standaardwaarden worden gebruikt, maar in de pre-productie omgeving niet-standaard waarden voor deze parameters worden gebruikt.

De reden niet-standaard waarden voor deze parameters in de voorbereiding van de productie-omgeving worden gebruikt is voor het testen van waarden voor deze parameters die u liever voor uw productieomgeving zodat ze kunnen ook worden getest.  Deze parameters alle betrekking op de Azure [Web App hosting plannen](https://azure.microsoft.com/pricing/details/app-service/), of **sku** en Azure [SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/), of **requestedServiceObjectiveName** die door de toepassing worden gebruikt.  Verschillende SKU's en servicenamen objectieve hebben verschillende kosten en functies en ondersteuning voor verschillende service level maatstaven.

In de volgende tabel worden de standaardwaarden voor deze parameters die zijn opgegeven in de sjabloon en de waarden die worden gebruikt in plaats van de standaardwaarden in het parameterbestand voorafgaand aan de productie.

| Parameter | Standaardwaarde | Parameterwaarde |
|---|---|---|
| **SKU** | Gratis | Standaard |
| **requestedServiceObjectiveName** | S0 | S1 |

## <a name="create-environments"></a>Omgevingen te maken
Alle Azure bronnen moeten worden gemaakt in een [Bronnengroep Azure](azure-resource-manager/resource-group-overview.md). Resourcegroepen kunnen u Azure resources groeperen zodat ze samen kunnen worden beheerd.  [Machtigingen](./active-directory/role-based-access-built-in-roles.md) kunnen worden toegewezen aan resourcegroepen, dat bepaalde personen binnen uw organisatie kunt maken, wijzigen, verwijderen of en de resources binnen deze weergeven.  Waarschuwingen en informatie over facturering als resources in de resourcegroep kunnen worden weergegeven in de [Portal Azure](https://portal.azure.com). Resourcegroepen worden gemaakt in een [regio](https://azure.microsoft.com/regions/)-Azure.  In dit artikel, worden alle bronnen in de centraal-Amerikaanse regio gemaakt. Als u werkelijke omgevingen hebt gestart, kiest u de regio die het beste aansluit bij uw behoeften. 

Resourcegroepen voor elke omgeving met behulp van de onderstaande methoden maken.  Alle methoden bereiken hetzelfde resultaat.

###<a name="azure-command-line-interface-cli"></a>Azure Command Line Interface (CLI)

Zorg ervoor dat u de CLI [geïnstalleerd](xplat-cli-install.md) op een Windows, OS X of Linux computer en dat u [aangesloten](xplat-cli-connect.md) uw [Azure AD-account hebt](./active-directory/active-directory-how-subscriptions-associated-directory.md) (ook wel een werk of school account) om uw abonnement op Azure. Typ de opdracht hieronder de resourcegroep voor de ontwikkelomgeving maken vanaf de opdrachtregel CLI.

    azure group create "TestApp1-Development" "Central US"

De opdracht retourneert de volgende als het is gelukt:

    info:    Executing command group create
    + Getting resource group TestApp1-Development
    + Creating resource group TestApp1-Development
    info:    Created resource group TestApp1-Development
    data:    Id:                  /subscriptions/uuuuuuuu-vvvv-wwww-xxxx-yyyy-zzzzzzzzzzzz/resourceGroups/TestApp1-Development
    data:    Name:                TestApp1-Development
    data:    Location:            centralus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

Typ de volgende opdracht te maken de resourcegroep voor de testomgeving:

    azure group create "TestApp1-Test" "Central US"

Typ de volgende opdracht te maken de resourcegroep voor de voorbereiding van de productie-omgeving:

    azure group create "TestApp1-Pre-Production" "Central US"

###<a name="powershell"></a>PowerShell

Zorg ervoor dat Azure PowerShell 1.01 of hoger is geïnstalleerd op een Windows-computer en uw [Azure AD-account](./active-directory/active-directory-how-subscriptions-associated-directory.md) (ook wel een account voor werk of school) hebt aangesloten op uw abonnement zoals wordt beschreven in het artikel voor [het installeren en configureren van Azure PowerShell](powershell-install-configure.md) . Een PowerShell-opdrachtprompt, typ de onderstaande opdracht voor het maken van de resourcegroep voor de ontwikkelomgeving.

    New-AzureRmResourceGroup -Name TestApp1-Development -Location "Central US"

De opdracht retourneert de volgende als het is gelukt:

    ResourceGroupName : TestApp1-Development
    Location          : centralus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/uuuuuuuu-vvvv-wwww-xxxx-yyyy-zzzzzzzzzzzz/resourceGroups/TestApp1-Development

Typ de volgende opdracht te maken de resourcegroep voor de testomgeving:

    New-AzureRmResourceGroup -Name TestApp1-Test -Location "Central US"

Typ de volgende opdracht te maken de resourcegroep voor de voorbereiding van de productie-omgeving:

    New-AzureRmResourceGroup -Name TestApp1-Pre-Production -Location "Central US"

###<a name="azure-portal"></a>Azure portal

1. Log in om de [Azure portal](https://portal.azure.com) met een account met [Azure AD](./active-directory/active-directory-how-subscriptions-associated-directory.md) (ook wel een werk of op school). Klik op Nieuw Management-->--> resourcegroep "TestApp1 ontwikkeling" invoeren in het naamvak van bron-groep, uw abonnement en selecteer 'Centrale ons' selecteren in het vak bron groep locatie zoals aangegeven in de volgende afbeelding.
   ![Portal](./media/solution-dev-test-environments/rgcreate.png)
2. Klik op de knop maken om de resourcegroep te maken.
3. Klik op Bladeren, blader door de lijst met resourcegroepen en klik op resourcegroepen, zoals hieronder wordt weergegeven.
   ![Portal](./media/solution-dev-test-environments/rgbrowse.png) 
4. Nadat de gebruiker op resourcegroepen ziet u de Resource groepen blade met uw nieuwe resourcegroep.
   ![Portal](./media/solution-dev-test-environments/rgview.png)
5. Maken van de TestApp1-Test en TestApp1-Pre-productie resource groepen dezelfde manier als die u de bovenstaande TestApp1 ontwikkeling resourcegroep hebt gemaakt.

##<a name="deploy-resources-to-environments"></a>Bronnen voor omgevingen implementeren

Azure bronnen om de resourcegroepen voor elke omgeving met behulp van het sjabloonbestand voor de oplossing en de parameterbestanden voor elke omgeving een van de onderstaande methoden te implementeren.  Beide methoden bereiken hetzelfde resultaat.

###<a name="azure-command-line-interface-cli"></a>Azure Command Line Interface (CLI)

Vanaf de opdrachtregel CLI, typt u de opdracht hieronder resources naar de resourcegroep implementeren dat u hebt gemaakt voor de ontwikkelomgeving [pad] vervangen door het pad naar de bestanden die u in de vorige stappen hebt opgeslagen.

    azure group deployment create -g TestApp1-Development -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Development.json 

Na een bericht 'Wachten voor distributie voltooien' ziet voor een paar minuten, retourneert de opdracht de volgende als dit lukt:

    info:    Executing command group deployment create
    + Initializing template configurations and parameters
    + Creating a deployment
    info:    Created template deployment "Deployment1"
    + Waiting for deployment to complete
    data:    DeploymentName     : Deployment1
    data:    ResourceGroupName  : TestApp1-Development
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          : XXXX-XX-XXT20:20:23.5202316Z
    data:    Mode               : Incremental
    data:    Name                           Type          Value
    data:    -----------------------------  ------------  ----------------------------
    data:    siteName                       String        TestApp1DevApp
    data:    hostingPlanName                String        TestApp1DevPlan
    data:    siteLocation                   String        Central US
    data:    sku                            String        Free
    data:    workerSize                     String        0
    data:    serverName                     String        testapp1devsrv
    data:    serverLocation                 String        Central US
    data:    administratorLogin             String        testapp1Admin
    data:    administratorLoginPassword     SecureString  undefined
    data:    databaseName                   String        testapp1devdb
    data:    collation                      String        SQL_Latin1_General_CP1_CI_AS
    data:    edition                        String        Standard
    data:    maxSizeBytes                   String        1073741824
    data:    requestedServiceObjectiveName  String        S0
    info:    group deployment create command OKx

Als de opdracht niet is gelukt, het oplossen van eventuele foutberichten en probeer het opnieuw.  Veelvoorkomende problemen zijn met behulp van parameterwaarden die niet aan de naamgeving Azure resourcebeperkingen houden. Andere tips voor probleemoplossing kunnen u vinden in het artikel [Troubleshooting resource groep implementaties in Azure](./resource-manager-troubleshoot-deployments-cli.md) .

Vanaf de opdrachtregel CLI, typt u de opdracht hieronder resources naar de resourcegroep implementeren dat u hebt gemaakt voor de testomgeving [pad] vervangen door het pad naar de bestanden die u in de vorige stappen hebt opgeslagen.

    azure group deployment create -g TestApp1-Test -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Test.json

Vanaf de opdrachtregel CLI, typt u de opdracht hieronder resources naar de resourcegroep implementeren dat u hebt gemaakt voor de voorbereiding van de productie-omgeving [pad] vervangen door het pad naar de bestanden die u in de vorige stappen hebt opgeslagen.

    azure group deployment create -g TestApp1-Pre-Production -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Pre-Production.json
  
###<a name="powershell"></a>PowerShell

Vanaf een opdrachtprompt Azure PowerShell (versie 1.01 of hoger), typt u de opdracht hieronder resources naar de resourcegroep implementeren dat u hebt gemaakt voor de ontwikkelomgeving [pad] vervangen door het pad naar de bestanden die u in de vorige stappen hebt opgeslagen.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Development -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Development.json -Name Deployment1 

Na een knipperende cursor ziet voor een paar minuten, retourneert de opdracht de volgende als dit lukt:

    DeploymentName    : Deployment1
    ResourceGroupName : TestApp1-Development
    ProvisioningState : Succeeded
    Timestamp         : XX/XX/XXXX 2:44:48 PM
    Mode              : Incremental
    TemplateLink      : 
    Parameters        : 
                        Name             Type                       Value     
                        ===============  =========================  ==========
                        siteName         String                     TestApp1DevApp
                        hostingPlanName  String                     TestApp1DevPlan
                        siteLocation     String                     Central US
                        sku              String                     Free      
                        workerSize       String                     0         
                        serverName       String                     testapp1devsrv
                        serverLocation   String                     Central US
                        administratorLogin  String                     testapp1Admin
                        administratorLoginPassword  SecureString                         
                        databaseName     String                     testapp1devdb
                        collation        String                     SQL_Latin1_General_CP1_CI_AS
                        edition          String                     Standard  
                        maxSizeBytes     String                     1073741824
                        requestedServiceObjectiveName  String                     S0        
                        
    Outputs           :

  Als de opdracht niet is gelukt, het oplossen van eventuele foutberichten en probeer het opnieuw.  Veelvoorkomende problemen zijn met behulp van parameterwaarden die niet aan de naamgeving Azure resourcebeperkingen houden. Andere tips voor probleemoplossing kunnen u vinden in het artikel [Troubleshooting resource groep implementaties in Azure](./resource-manager-troubleshoot-deployments-powershell.md) .

  Via een PowerShell-opdrachtprompt, typt u de opdracht hieronder resources naar de resourcegroep implementeren dat u hebt gemaakt voor de testomgeving [pad] vervangen door het pad naar de bestanden die u in de vorige stappen hebt opgeslagen.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Test -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Test.json -Name Deployment1

  Via een PowerShell-opdrachtprompt, typ de opdracht hieronder resources naar de resourcegroep implementeren dat u hebt gemaakt voor de voorbereiding van de productie-omgeving [pad] vervangen door het pad naar de bestanden die u in de vorige stappen hebt opgeslagen.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Pre-Production -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Pre-Production.json -Name Deployment1

De sjabloon en de parameter-bestanden kunnen worden versioned en onderhouden met uw toepassingscode in een bronbeheersysteem.  U kan ook de bovenstaande script bestanden en sla ze met uw code ook opdrachten opslaan.

> [AZURE.NOTE] Rechtstreeks door op de knop 'Implementeren op Azure' op het artikel [dient een Web App met een SQL-Database](https://azure.microsoft.com/documentation/templates/201-web-app-sql-database/) , kunt u de sjabloon distribueren naar Azure.  Wellicht vindt u dit handig voor meer informatie over sjablonen, maar dit dus niet kunt u versie, bewerken en opslaan uw sjabloon en parameter waarden met de toepassingscode van uw, zodat u meer details over deze methode is niet beschreven in dit artikel.

## <a name="maintain-environments"></a>Onderhouden van omgevingen
In de gehele ontwikkeling, configuratie van Azure resources in verschillende omgevingen mogelijk inconsistent gewijzigd per ongeluk of opzettelijk.  Hierdoor onnodig probleemvaststelling en -oplossing gedurende de ontwikkelingscyclus van toepassing.

1. De omgevingen wijzigen via de [portal Azure](https://portal.azure.com). 
2. Inloggen op het met de account die u gebruikt voor het voltooien van de bovenstaande stappen. 
3. Als in de afbeelding hieronder, klik op Bladeren resourcegroepen (misschien moet u schuiven om te zien de resourcegroepen)--> wordt weergegeven.
   ![Portal](./media/solution-dev-test-environments/rgbrowse.png)
4. Na op resourcegroepen in de bovenstaande afbeelding klikt, ziet u de Resource groepen blade en de drie resourcegroepen die u hebt gemaakt in een eerdere stap zoals in de volgende afbeelding wordt weergegeven. Klik op de resourcegroep TestApp1 ontwikkeling en ziet u het blad met de bronnen die zijn gemaakt met de sjabloon die in de TestApp1 ontwikkeling resource groep implementatie die u in een eerdere stap hebt voltooid.  De TestApp1DevApp Web App resource verwijderen door te klikken op TestApp1DevApp in de groep TestApp1 informatiebron blade en klik vervolgens op verwijderen te klikken in het blad TestApp1DevApp Web app.
   ![Portal](./media/solution-dev-test-environments/portal2.png)
5. Klik op 'Ja' wanneer u de portal u gevraagd of u zeker weet dat u wilt verwijderen van de resource. De groep TestApp1 informatiebron blade sluiten en opnieuw te openen wordt nu weergeven zonder de Web app dat u zojuist hebt verwijderd.  De inhoud van de resourcegroep zijn nu anders dan ze moeten zijn. Verder kunt u experimenteren met meerdere resources verwijderen uit verschillende resourcegroepen of zelfs wijzigen van configuratie-instellingen voor bepaalde resources. In plaats van de portal Azure een bron verwijderen uit een resourcegroep, kunt u de opdracht PowerShell [Verwijderen AzureResource](https://msdn.microsoft.com/library/azure/dn757676.aspx) of het of de opdracht 'azure bron verwijderen' van de CLI aan dezelfde taak uitgevoerd.
6. Als u alle bronnen en de configuratie die in de bronnengroepen weer over de moeten ze in staat zijn, de omgevingen aan de resourcegroepen die u gebruikt dezelfde opdrachten gebruiken in de sectie [bronnen implementeren in omgevingen](#deploy-resources-to-environments) opnieuw te implementeren, maar vervang 'Deployment1' met 'Deployment2'.
7.  Zoals in de samenvatting van de blade TestApp1 ontwikkeling in de afbeelding weergegeven in stap 4, zult u zien dat het Web app dat u in de portal in de vorige stap hebt verwijderd opnieuw bestaat, net als andere bronnen u hebt gekozen om te verwijderen. Als u de configuratie van de resources hebt gewijzigd, kunt u ook dat ze daarop ingesteld terug op de waarden in de parameterbestanden te controleren. Een van de voordelen van de implementatie van uw omgeving met Azure Resource Manager-sjablonen is dat u gemakkelijk opnieuw de omgevingen terug naar een bekende staat op elk gewenst moment implementeren kunt.
8. Als u op de tekst onder 'Laatste implementatie' in de volgende afbeelding klikt, ziet u een blade waarin de geschiedenis van de implementatie voor de resourcegroep.  Nadat u de naam "Deployment1" voor de eerste implementatie en "Deployment2" voor de tweede installatie gebruikt, hebt u twee vermeldingen.  Te klikken op een distributie weer een blade waarin de resultaten voor elke distributie.
  ![Portal](./media/solution-dev-test-environments/portal3.png)



## <a name="delete-environments"></a>Omgeving verwijderen
Zodra u klaar met een omgeving bent, zult u te verwijderen zodat u geen kosten voor Azure bronnen die u niet langer gebruikt oplopen.  Verwijderen van omgevingen is nog eenvoudiger dan het maken ervan.  Afzonderlijke groepen van Azure Resource zijn gemaakt voor elke omgeving en bronnen zijn geïmplementeerd in de resourcegroepen in de vorige stappen. 

Verwijder de omgevingen met behulp van de onderstaande methoden.  Alle methoden bereiken hetzelfde resultaat.

### <a name="azure-cli"></a>Azure CLI

Uit de CLI-prompt, typ het volgende:

    azure group delete "TestApp1-Development"

Wanneer daarom wordt gevraagd, voert u y en druk vervolgens op enter om de ontwikkelomgeving en alle bronnen te verwijderen. Na een paar minuten retourneert de opdracht de volgende:

    info:    group delete command OK

Typ het volgende als u wilt verwijderen van de resterende omgevingen van CLI-prompt:

    azure group delete "TestApp1-Test"
    azure group delete "TestApp1-Pre-Production"
  
### <a name="powershell"></a>PowerShell

Typ de onderstaande opdracht aan de resourcegroep en alle bijbehorende inhoud verwijderen vanaf een opdrachtprompt Azure PowerShell (versie 1.01 of hoger).    

    Remove-AzureRmResourceGroup -Name TestApp1-Development

Als u zeker weet dat u wilt verwijderen van de resourcegroep Voer desgevraagd y, gevolgd door de enter-toets.

Typ het volgende als u wilt verwijderen van de resterende omgevingen:

    Remove-AzureRmResourceGroup -Name TestApp1-Test
    Remove-AzureRmResourceGroup -Name TestApp1-Pre-Production

### <a name="azure-portal"></a>Azure portal

1. Blader in de Azure portal aan resourcegroepen zoals in de vorige stappen. 
2. Selecteer de resourcegroep TestApp1 ontwikkeling en klik vervolgens op verwijderen in de groep TestApp1 informatiebron blade. Een nieuwe blade wordt weergegeven. Voer de naam van de resource en klikt u op de knop verwijderen.
![Portal](./media/solution-dev-test-environments/rgdelete.png)
3. Verwijderen van de TestApp1-Test en TestApp1-Pre-productie resource groepen dezelfde manier als die u de resourcegroep TestApp1 ontwikkeling verwijderd.

Ongeacht welke methode die u gebruikt, de bronnengroepen en alle bronnen die zij zich niet meer zal bestaan en u zult niet langer facturering onkosten maken voor de resources.  

Resource-gebruik kosten tijdens de ontwikkeling van toepassingen met [Azure automatisering](automation/automation-intro.md) kunt u plannen om te minimaliseren de Azure taken die:

- Virtuele machines aan het einde van elke dag stoppen en opnieuw aan het begin van elke dag.
- Hele omgeving aan het einde van elke dag te verwijderen en opnieuw te maken aan het begin van elke dag.
 
Nu dat u hebt ondervonden hoe gemakkelijk het is om te maken, onderhouden, en verwijderen van ontwikkeling en testen omgevingen, kunt u meer informatie over wat u zojuist hebt gedaan door verder te experimenteren met de bovenstaande stappen en de verwijzingen in dit artikel vervatte lezen.

## <a name="next-steps"></a>Volgende stappen

- [Het administratieve beheer overdragen](./active-directory/role-based-access-control-configure.md) voor verschillende resources in elke omgeving met Microsoft Azure AD groepen of gebruikers toewijzen aan specifieke rollen die de mogelijkheid voor het uitvoeren van een subset van bewerkingen op Azure bronnen.
- [Codes toewijzen](resource-group-using-tags.md) aan de resourcegroepen voor elke omgeving en/of de afzonderlijke resources. U kunt een label "Milieu" toevoegen aan de bronnengroepen en stel de waarde overeen te komen met de namen van de omgeving. Tags is vooral handig als u nodig hebt voor het ordenen van bronnen voor facturering of management.
- Controleren op waarschuwingen en facturering voor resource groep resources in de [portal Azure](https://portal.azure.com).
