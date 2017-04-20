<properties
    pageTitle="Implementeren van een VM met C# en een sjabloon Resource Manager | Microsoft Azure"
    description="Informatie voor C# en een sjabloon Resource Manager gebruiken voor de implementatie van een Azure VM."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="davidmu"/>

# <a name="deploy-an-azure-virtual-machine-using-c-and-a-resource-manager-template"></a>Implementeren van een Azure Virtual Machine met behulp van C# en een sjabloon Resource Manager

Resourcegroepen en sjablonen gebruikt, kun je je voor het beheren van alle bronnen samen die ondersteuning bieden voor uw toepassing. In dit artikel wordt beschreven hoe u met Visual Studio en C# kunt u verificatie instellen, maakt u een sjabloon en vervolgens implementeren Azure resources met behulp van de sjabloon die u hebt gemaakt.

Moet u eerst controleren of dat u deze procedures hebt uitgevoerd:

- Installeer [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- Controleer of de installatie van [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) of [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855)
- Een [verificatietoken](../resource-group-authenticate-service-principal.md) ophalen
- Maak een groep met [Azure PowerShell](../resource-group-template-deploy.md) [Azure CLI](../resource-group-template-deploy-cli.md)of [Azure portal](../resource-group-template-deploy-portal.md).

Het duurt ongeveer 30 minuten deze stappen doen.
    
## <a name="step-1-create-the-visual-studio-project-the-template-file-and-the-parameters-file"></a>Stap 1: Maak de Visual Studio-project, de sjabloonbestand en het parameterbestand

### <a name="create-the-template-file"></a>Het sjabloonbestand maken

Een sjabloon Azure Resource Manager kunt u implementeren en beheren van bronnen van Azure samen. De sjabloon is een JSON-beschrijving van de bronnen en de implementatie van bijbehorende parameters.

Voer deze stappen uit in Visual Studio:

1. Klik op **bestand** > **nieuwe** > **Project**.

2. In **sjablonen** > **Visual C#**, **Consoletoepassing**selecteert, voert u de naam en locatie van het project en klik vervolgens op **OK**.

3. Klik met de rechtermuisknop op de naam van het project in de Solution Explorer, klikt u op **Add** > **Nieuw Item**.

4. Klik op Web, selecteer JSON-bestand, *VirtualMachineTemplate.json* opgeven voor de naam en klik op **toevoegen**.

5. Toevoegen in het openen en het vierkante haakje sluiten van het bestand VirtualMachineTemplate.json de vereiste schema-element en het element contentVersion vereist:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
        }

6. [Parameters](../resource-group-authoring-templates.md#parameters) zijn niet altijd verplicht, maar ze bieden een manier om de invoerwaarden worden wanneer de sjabloon wordt geïmplementeerd. Het element van de parameters en de onderliggende elementen toevoegen na het element contentVersion:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
        }

7. [Variabelen](../resource-group-authoring-templates.md#variables) kunnen worden gebruikt in een sjabloon opgeven voor waarden die kunnen vaak worden gewijzigd of die moeten worden gemaakt van een combinatie van waarden van de parameters. Het element variabelen toevoegen na de sectie parameters:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"  
          },
        }

8. [Bronnen](../resource-group-authoring-templates.md#resources) als de virtuele machine, het virtuele netwerk en de opslag account zijn vervolgens gedefinieerd in de sjabloon. De sectie bronnen toevoegen na de sectie variabelen:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "mystorage1",
              "apiVersion": "2015-06-15",
              "location": "[resourceGroup().location]",
              "properties": { "accountType": "Standard_LRS" }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/publicIPAddresses",
              "name": "myip1",
              "location": "[resourceGroup().location]",
              "properties": {
                "publicIPAllocationMethod": "Dynamic",
                "dnsSettings": { "domainNameLabel": "mydns1" }
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/virtualNetworks",
              "name": "myvnet1",
              "location": "[resourceGroup().location]",
              "properties": {
                "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
                "subnets": [ {
                  "name": "mysn1",
                  "properties": { "addressPrefix": "10.0.0.0/24" }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/networkInterfaces",
              "name": "mync1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/publicIPAddresses/myip1",
                "Microsoft.Network/virtualNetworks/myvn1"
              ],
              "properties": {
                "ipConfigurations": [ {
                  "name": "ipconfig1",
                  "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                      "id": "[resourceId('Microsoft.Network/publicIPAddresses', 'myip1')]"
                    },
                    "subnet": { "id": "[variables('subnetRef')]" }
                  }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Compute/virtualMachines",
              "name": "myvm1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/networkInterfaces/mync1",
                "Microsoft.Storage/storageAccounts/mystorage1"
              ],
              "properties": {
                "hardwareProfile": { "vmSize": "Standard_A1" },
                "osProfile": {
                  "computerName": "myvm1",
                  "adminUsername": "[parameters('adminUsername')]",
                  "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                  "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version" : "latest"
                  },
                  "osDisk": {
                    "name": "myosdisk1",
                    "vhd": {
                      "uri": "https://mystorage1.blob.core.windows.net/vhds/myosdisk1.vhd"
                    },
                    "caching": "ReadWrite",
                    "createOption": "FromImage"
                  }
                },
                "networkProfile": {
                  "networkInterfaces" : [ {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces','mync1')]"
                  } ]
                }
              }
            } ]
          }
      
9. Sla het bestand van de sjabloon die u hebt gemaakt.

### <a name="create-the-parameters-file"></a>Maak het parameterbestand

Waarden voor de resourceparameters die zijn gedefinieerd in de sjabloon wilt opgeven, maakt u een parameterbestand met de waarden die worden gebruikt wanneer de sjabloon wordt geïmplementeerd. Voer deze stappen uit in Visual Studio:

1. Klik met de rechtermuisknop op de naam van het project in de Solution Explorer, klikt u op **Add** > **Nieuw Item**.

2. Klik op Web, selecteer JSON-bestand, *Parameters.json* opgeven voor de naam en klik op **toevoegen**.

3. Open het bestand parameters.json en voeg deze inhoud JSON toe:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" }
          }
        }

    >[AZURE.NOTE] Dit artikel maakt u een virtuele machine met een versie van het besturingssysteem Windows Server. Zie voor meer informatie over het selecteren van andere afbeeldingen kunt [navigeren en afbeeldingen selecteren Azure virtuele machine met Windows PowerShell en de CLI Azure](virtual-machines-linux-cli-ps-findimage.md).

4. Sla het parameterbestand die u hebt gemaakt.

## <a name="step-2-install-the-libraries"></a>Stap 2: Installeer de bibliotheken

NuGet packages zijn de eenvoudigste manier om het installeren van de bibliotheken die u nodig hebt voor deze zelfstudie hebt voltooid. Moet u de bibliotheek Azure Resource Management en de bibliotheek Azure Active Directory-verificatie voor het maken van de bronnen. Als u deze bibliotheken in Visual Studio, voer deze stappen uit:

1. Klik met de rechtermuisknop op de naam van het project in de Solution Explorer **NuGet pakketten beheren**en klik vervolgens op Bladeren.

2. Op *Active Directory* in het zoekvak op **installeren** voor het Active Directory-verificatie Library-pakket en volg de instructies om het pakket te installeren.

4. Selecteer boven aan de pagina **Zijn voorlopige versie**. Type *Microsoft.Azure.Management.ResourceManager* in het zoekvak, klikt u op **installeren** voor de Microsoft Azure Resource Management bibliotheken en volgt u de instructies voor het installeren van het pakket.

Nu bent u klaar om uw toepassing te maken met behulp van de bibliotheken.

## <a name="step-3-create-the-credentials-that-are-used-to-authenticate-requests"></a>Stap 3: Maak de referenties die worden gebruikt voor verificatie-aanvragen

De Azure Active Directory-toepassing wordt gemaakt en de verificatie-bibliotheek is geïnstalleerd. Nu opmaken u de toepassingsgegevens in de referenties die worden gebruikt voor verificatie-aanvragen aan de bronnenbeheerder van Azure.

1. Open het bestand Program.cs voor het project dat u hebt gemaakt en voegt u deze instructies boven aan het bestand:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Rest;
        using System.IO;

2.  Deze methode toevoegen aan de klasse wordt de token die nodig is voor het maken van de referenties ophalen:

        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token.");
          }
          return token;
        }

    {Client-id} vervangen door de aanduiding van de Azure Active Directory-toepassing {-geheim client} met de toegangssleutel van de toepassing AD en {huurder-id} met de huurder voor uw abonnement-id. De huurder-id kunt u vinden door het uitvoeren van Get-AzureRmSubscription. Met behulp van de Azure portal vindt u de toegangstoets.

3. Om de referenties te maken, moet u deze code toevoegen aan de Main-methode in het bestand Program.cs:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

4. Sla het bestand Program.cs.

## <a name="step-4-deploy-the-template"></a>Stap 4: De sjabloon distribueren

In deze stap gebruikt u de resourcegroep die u eerder hebt gemaakt, maar u kunt ook een bronnengroep met de [ResourceGroup](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.models.resourcegroup.aspx) en de klassen [ResourceManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspx) maken.

1. Variabelen toevoegen aan de Main-methode van de klasse programma geeft u de namen van de resources die u eerder hebt gemaakt en de naam van de implementatie van uw abonnement-id:

        var groupName = "resource group name";
        var subscriptionId = "subsciption id";
        var deploymentName = "deployment name";

    De waarde van groupName vervangen door de naam van de resourcegroep. Vervang de waarde van deploymentName met de naam die u wilt gebruiken voor de implementatie. De abonnement-id kunt u vinden door het uitvoeren van Get-AzureRmSubscription.

2. Deze methode toevoegen aan de klasse programma om de resources naar de resourcegroep implementeren met behulp van de sjabloon die u hebt gedefinieerd:

        public static async Task<DeploymentExtended> CreateTemplateDeploymentAsync(
          TokenCredentials credential,
          string groupName,
          string deploymentName,
          string subscriptionId)
        {
          Console.WriteLine("Creating the template deployment...");
          var deployment = new Deployment();
          deployment.Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            Template = File.ReadAllText("..\\..\\VirtualMachineTemplate.json"),
            Parameters = File.ReadAllText("..\\..\\Parameters.json")
          };
          var resourceManagementClient = new ResourceManagementClient(credential) 
            { SubscriptionId = subscriptionId };
          return await resourceManagementClient.Deployments.CreateOrUpdateAsync(
            groupName,
            deploymentName,
            deployment);
        }

    Als u wilt dat de sjabloon van een account opslag implementeren, kunt u de sjablooneigenschap vervangen door de eigenschap TemplateLink.

3. Aanroepen van de methode die u zojuist hebt toegevoegd, moet u deze code toevoegen aan de Main-methode:

        var dpResult = CreateTemplateDeploymentAsync(
          credential,
          groupName,
          deploymentName,
          subscriptionId);
        Console.WriteLine(dpResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

## <a name="step-5-delete-the-resources"></a>Stap 5: Resources verwijderen

Omdat u in rekening worden gebracht voor de bronnen die worden gebruikt in Azure, is het altijd een goede gewoonte om de bronnen die u niet meer nodig hebt verwijderen. U hoeft niet elke resource afzonderlijk verwijderen uit een resourcegroep. Verwijder de resourcegroep en alle bronnen worden automatisch verwijderd.

1.  Toevoegen als u wilt verwijderen in de resourcegroep, deze methode aan de klasse van het programma:

        public static async void DeleteResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId)
        {
          Console.WriteLine("Deleting resource group...");
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await resourceManagementClient.ResourceGroups.DeleteAsync(groupName);
        }

2.  Aanroepen van de methode die u zojuist hebt toegevoegd, moet u deze code toevoegen aan de Main-methode:

        DeleteResourceGroupAsync(
          credential,
          groupName,
          subscriptionId);
        Console.ReadLine();

##<a name="step-6-run-the-console-application"></a>Stap 6: De consoletoepassing uitvoeren

1.  De als consoletoepassing wilt uitvoeren, klikt u op **starten** in Visual Studio en meldt u zich aan Azure AD met dezelfde referenties die u kunt bij uw abonnement gebruiken.

2.  Nadat de status goedgekeurd wordt weergegeven, drukt u op **Enter** .

    Het moet ongeveer vijf minuten voor deze consoletoepassing volledig van begin tot eind duren. Voordat u op Enter om te beginnen met het verwijderen van bronnen, kan u krijgen een paar minuten om te controleren of het maken van de bronnen in de portal Azure voordat u ze verwijdert.

3. Ga naar de controlelogboeken in Azure portal overzicht van de status van de bronnen:

    ![Controlelogboeken in Azure portal bladeren](./media/virtual-machines-windows-csharp-template/crpportal.png)

## <a name="next-steps"></a>Volgende stappen

- Als er problemen met de distributie, zou een volgende stap zijn te kijken naar de [Troubleshooting resource groep implementaties met Azure portal](../resource-manager-troubleshoot-deployments-portal.md).
- Informatie over het beheren van de virtuele machine die u hebt gemaakt aan de hand van [beheren virtuele machines met behulp van bronbeheer Azure en PowerShell](virtual-machines-windows-csharp-manage.md).
