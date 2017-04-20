<properties
   pageTitle="Het instellen van een Service Fabric '-cluster met behulp van Visual Studio | Microsoft Azure"
   description="Beschrijving van het instellen van een Service Fabric '-cluster met behulp van bronbeheer Azure sjabloon gemaakt met een resourcegroep Azure-project in Visual Studio"
   services="service-fabric"
   documentationCenter=".net"
   authors="karolz-ms"
   manager="adegeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/06/2016"
   ms.author="karolz@microsoft.com"/>

# <a name="set-up-a-service-fabric-cluster-by-using-visual-studio"></a>Instellen van een Service Fabric '-cluster met behulp van Visual Studio
In dit artikel wordt beschreven hoe een Azure Service Fabric '-cluster met behulp van Visual Studio en een bronnenbeheerder Azure sjabloon instellen. Een project van Visual Studio Azure resource groep gebruiken we om de sjabloon te maken. Nadat de sjabloon is gemaakt, kan deze rechtstreeks naar Azure geïmplementeerd vanuit Visual Studio. Het kan ook worden gebruikt in een script of als onderdeel van de faciliteit continuous integration (CI).

## <a name="create-a-service-fabric-cluster-template-by-using-an-azure-resource-group-project"></a>Een cluster Service Fabric sjabloon maken met behulp van een groep resources Azure project
Om te beginnen open Visual Studio en Azure resource group-projecten maken (het is beschikbaar in de map **wolk** ):

![De dialoogvenster New Project met de resourcegroep Azure project geselecteerd][1]

U kunt een nieuwe Visual Studio-oplossing voor dit project te maken of toe te voegen aan een bestaande oplossing.

>[AZURE.NOTE] Als u het project Azure resource groep onder het knooppunt wolk niet wordt weergegeven, hoeft u niet de Azure SDK is geïnstalleerd. Web Platform Installer ([Nu installeren](http://www.microsoft.com/web/downloads/platform.aspx) als u nog niet gedaan hebt), start en zoek vervolgens naar "Azure SDK voor .NET" en installeer de versie die compatibel is met uw versie van Visual Studio.

Nadat u de knop OK raakt, Visual Studio wordt u gevraagd om te selecteren, de Resource Manager-sjabloon die u wilt maken:

![Dialoogvenster sjabloon Azure selecteren met Fabric '-Cluster Service template geselecteerd][2]

Selecteer de sjabloon Fabric '-Cluster Service en druk opnieuw op de knop OK. Het project en de sjabloon Resource Manager zijn nu gemaakt.

## <a name="prepare-the-template-for-deployment"></a>De sjabloon voorbereiden voor distributie
Voordat de sjabloon wordt toegepast om het cluster te maken, moet u waarden opgeven voor de sjabloonparameters van de vereiste. De parameterwaarden voor deze worden gelezen uit de `ServiceFabricCluster.parameters.json` -bestand in de `Templates` map van het project resource groep. Open het bestand en geef de volgende waarden:

|Naam van de parameter           |Beschrijving|
|-----------------------  |--------------------------|
|adminUserName            |De naam van de administrator-account voor de Service Fabric machines (knooppunten).|
|certificateThumbprint    |De blauwdruk van het certificaat waarmee de cluster wordt beveiligd.|
|sourceVaultResourceId    |De *resource-ID* van de sleutel kluis waar het certificaat dat u het cluster beveiligt is opgeslagen.|
|certificateUrlValue      |De URL van het cluster beveiligingscertificaat.|

De bronnenbeheerder van Visual Studio Service Fabric-sjabloon maakt u een beveiligde cluster die is beveiligd met een certificaat. Dit certificaat wordt geïdentificeerd door de laatste drie sjabloonparameters (`certificateThumbprint`, `sourceVaultValue`, en `certificateUrlValue`), en deze in een **Kluis van Azure-sleutel**moet bestaan. Voor meer informatie over het maken van het cluster beveiligingscertificaat, Zie [cluster-Service Fabric scenario's voor netwerkbeveiliging](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) artikel.

## <a name="optional-change-the-cluster-name"></a>Optioneel: naam van het cluster wijzigen
Elke Service Fabric '-cluster heeft een naam. Wanneer een Fabric '-cluster wordt gemaakt in Azure, bepaalt clusternaam (samen met de regio Azure) het Domain Name System (DNS) naam voor het cluster. Als u het cluster naam bijvoorbeeld `myBigCluster`, en de locatie (regio Azure) van de resourcegroep die als host voor het nieuwe cluster fungeert Oost-VS, is de DNS-naam van het cluster `myBigCluster.eastus.cloudapp.azure.com`.

Standaard is de naam van het cluster automatisch gegenereerd en uniek gemaakt door een willekeurige achtervoegsel te koppelen aan een voorvoegsel 'cluster'. Hierdoor kunt heel gemakkelijk de sjabloon wilt gebruiken als onderdeel van een systeem van **continuous integration** (CI). Als u een specifieke naam voor het cluster, dat nuttig is, de waarde van de `clusterName` variabele in het sjabloonbestand Resource Manager (`ServiceFabricCluster.json`) op de naam van uw gekozen. Het is de eerste variabele in dat bestand zijn gedefinieerd.

## <a name="optional-add-public-application-ports"></a>Optioneel: openbare poorten toevoegen
U kunt ook de openbare poorten voor het cluster wijzigen voordat u het implementeert. Standaard wordt de sjabloon geopend slechts twee openbare TCP-poorten (80 en 8081). Als u meer van uw toepassingen, de definitie van Azure taakverdeling in de sjabloon wijzigen De definitie wordt opgeslagen in de belangrijkste sjabloonbestand (`ServiceFabricCluster.json`). Open dat bestand en zoek naar `loadBalancedAppPort`. Elke poort is gekoppeld aan drie onderdelen:

1. Een sjabloonvariabele dat de waarde van TCP-poort voor de poort definieert:

    ```json
    "loadBalancedAppPort1": "80"
    ```

2. Een *sonde* die hoe vaak en hoe lang de Azure load balancer probeert definieert te gebruiken van een bepaald knooppunt Fabric-Service voordat over naar een andere. De sondes zijn onderdeel van de Load Balancer-bron. Hier is de definitie van de sonde voor de eerste standaardpoort van toepassing:

    ```json
    {
        "name": "AppPortProbe1",
        "properties": {
            "intervalInSeconds": 5,
            "numberOfProbes": 2,
            "port": "[variables('loadBalancedAppPort1')]",
            "protocol": "Tcp"
        }
    }
    ```

3. Een *regel voor netwerktaakverdeling* die gekoppeld aan de poort en de sonde, waardoor de taakverdeling tussen een reeks knooppunten van het cluster Service Fabric:

    ```json
    {
        "name": "AppPortLBRule1",
        "properties": {
            "backendAddressPool": {
                "id": "[variables('lbPoolID0')]"
            },
            "backendPort": "[variables('loadBalancedAppPort1')]",
            "enableFloatingIP": false,
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPort": "[variables('loadBalancedAppPort1')]",
            "idleTimeoutInMinutes": 5,
            "probe": {
                "id": "[concat(variables('lbID0'),'/probes/AppPortProbe1')]"
            },
            "protocol": "Tcp"
        }
    }
    ```
Als de toepassingen die u wilt implementeren met de cluster meer poorten nodig hebt, kunt u deze toevoegen door te maken meer sonde en definities van netwerktaakverdeling. Zie [een interne taakverdeling met behulp van een sjabloon maken](../load-balancer/load-balancer-get-started-ilb-arm-template.md)voor meer informatie over het werken met Azure Load Balancer via Resource Manager-sjablonen.

## <a name="deploy-the-template-by-using-visual-studio"></a>De sjabloon distribueren met behulp van Visual Studio
Nadat u de vereiste parameterwaarden in hebt opgeslagen de`ServiceFabricCluster.param.dev.json` -bestand, bent u klaar om de sjabloon distribueren en uw Service Fabric '-cluster maken. De resource-groep-project in Visual Studio Solution Explorer met de rechtermuisknop en kies **implementeren | Nieuwe distributie...**. Indien nodig weergeven Visual Studio het dialoogvenster **distribueren aan resourcegroep** , waarin u voor de verificatie van Azure:

![Dialoogvenster resourcegroep implementeren][3]

In het dialoogvenster kunt u een bestaande resourcegroep Resource Manager voor het cluster kiezen en kunt u een nieuwe opmerking te maken. Normaal gesproken is het handig een afzonderlijke resourcegroep gebruiken voor een cluster Service Fabric.

Nadat u de knop Deploy raakt, wordt Visual Studio u gevraagd te bevestigen de parameterwaarden voor de sjabloon. Druk op de knop **Opslaan** . Één parameter heeft geen permanente waarde: het wachtwoord van de Administrator-account voor het cluster. U moet een wachtwoordwaarde als Visual Studio wordt u gevraagd om een.

>[AZURE.NOTE] Visual Studio ondersteunt vanaf Azure SDK 2,9 wachtwoorden voor het lezen van **Azure sleutel kluis** tijdens de implementatie. U ziet dat in het dialoogvenster sjabloon parameters de `adminPassword` parameter tekstvak heeft een beetje 'key'-pictogram aan de rechterkant. Dit pictogram kunt u een bestaande sleutel kluis geheim selecteren als het Administrator-wachtwoord voor de cluster. Zorg ervoor dat voor het inschakelen van toegang voor de distributie van de sjabloon in de geavanceerde-beleid van uw sleutel kluis Azure Resource Manager. 

U kunt de voortgang van het installatieproces in het venster Visual Studio uitvoer controleren. Zodra de sjabloonimplementatie is voltooid, wordt het nieuwe cluster is klaar voor gebruik!

>[AZURE.NOTE] Als PowerShell nooit gebruikt is voor Azure beheren vanaf de computer die u nu gebruikt, moet u een beetje huishoudelijke doen.
>1. Inschakelen van PowerShell scripts door de [`Set-ExecutionPolicy`](https://technet.microsoft.com/library/hh849812.aspx) opdracht. "Onbeperkt" beleid is voor de ontwikkeling van machines, doorgaans acceptabel.
>2. Beslissen of u wilt toestaan dat het verzamelen van diagnostische gegevens van Azure PowerShell-opdrachten en voer [`Enable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619303.aspx) of [`Disable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619236.aspx) als u nodig hebt. Dit vermijdt overbodige vragen tijdens de sjabloonimplementatie.

Als er fouten optreden, gaat u naar de [portal Azure](https://portal.azure.com/) en open de resourcegroep die u naar geïmplementeerd. **Alle instellingen**, klik op **implementaties** op het blad van de instellingen. Een mislukte resourcegroep implementatie worden er gedetailleerde diagnostische informatie.

>[AZURE.NOTE] Service Fabric clusters vereisen een bepaald aantal knooppunten te zijn van beschikbaarheid onderhouden en behouden van status - aangeduid als "het quorum onderhouden." Het is daarom niet veilig af te sluiten voor alle computers in het cluster tenzij u eerst een [volledige back-up van uw status](service-fabric-reliable-services-backup-restore.md)hebt uitgevoerd.

## <a name="next-steps"></a>Volgende stappen
- [Klik hier voor informatie over het instellen van Service Fabric '-cluster met behulp van de portal Azure](service-fabric-cluster-creation-via-portal.md)
- [Informatie over het beheren en implementeren met behulp van Visual Studio Service Fabric-toepassingen](service-fabric-manage-application-in-visual-studio.md)

<!--Image references-->
[1]: ./media/service-fabric-cluster-creation-via-visual-studio/azure-resource-group-project-creation.png
[2]: ./media/service-fabric-cluster-creation-via-visual-studio/selecting-azure-template.png
[3]: ./media/service-fabric-cluster-creation-via-visual-studio/deploy-to-azure.png
