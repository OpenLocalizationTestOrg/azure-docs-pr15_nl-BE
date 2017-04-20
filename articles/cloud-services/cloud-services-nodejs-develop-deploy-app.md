<properties
    pageTitle="Node.js Getting Started Guide | Microsoft Azure"
    description="Informatie over het maken van een eenvoudige webtoepassing van Node.js en deze implementeren in een Azure cloud-service."
    services="cloud-services"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs"
    ms.topic="hero-article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="build-and-deploy-a-nodejs-application-to-an-azure-cloud-service"></a>Bouwen en implementeren van een toepassing Node.js een Azure Cloud-service

> [AZURE.SELECTOR]
- [Node.js](cloud-services-nodejs-develop-deploy-app.md)
- [.NET](cloud-services-dotnet-get-started.md)

Deze zelfstudie laat zien hoe een eenvoudige Node.js toepassing die wordt uitgevoerd in een Azure Cloud-Service maken. Cloud-Services zijn de bouwstenen van schaalbare cloud-toepassingen in Azure. Ze kunnen de scheiding en onafhankelijke management en schalen van front-end en back-end componenten van uw toepassing.  Cloud-Services bieden een robuuste specifieke virtuele machine voor het betrouwbaar hosten van elke rol.

Zie voor meer informatie over Cloud Services en vergelijk deze met Azure Websites en virtuele machines, [Websites Azure, Cloud Services en vergelijking van virtuele Machines].

>[AZURE.TIP] Zoek een eenvoudige website bouwen? Als uw scenario slechts een eenvoudige website front-end omvat, kunt u overwegen [een lichtgewicht web app]. U kunt eenvoudig upgraden naar een Cloud-Service als uw web app groeit en uw behoeften veranderen.

Door deze zelfstudie te volgen, bouwt u een eenvoudige webtoepassing opgenomen binnen een Webrol. U zal de emulator compute gebruiken om uw toepassing lokaal te testen en vervolgens implementeren met behulp van opdrachtregelprogramma's van PowerShell.

De toepassing is een eenvoudige "hello world":

![Een webbrowser weergeven van de webpagina van Hallo wereld][A web browser displaying the Hello World web page]

## <a name="prerequisites"></a>Vereisten

> [AZURE.NOTE] In deze zelfstudie wordt Azure PowerShell, Windows vereist.

- Installeer en configureer [Azure Powershell].
- Download en installeer de [SDK voor .NET 2.7 Azure]. Selecteer in de installatie-instellingen:
    - MicrosoftAzureAuthoringTools
    - MicrosoftAzureComputeEmulator


## <a name="create-an-azure-cloud-service-project"></a>Maak een project Azure Cloud Service

Maak een nieuw project Azure Cloud Service met elementaire Node.js steigers de volgende taken uitvoeren:

1. **Windows PowerShell** uitvoeren als Administrator. zoeken in het **Menu Start** of het **Startscherm van** **Windows PowerShell**.

2. [PowerShell verbinden] aan uw abonnement.

3. Voer de volgende PowerShell-cmdlet maken om het project te maken:

        New-AzureServiceProject helloworld

    ![Het resultaat van de opdracht Nieuw AzureService Hallo wereld][The result of the New-AzureService helloworld command]

    De cmdlet **New-AzureServiceProject** genereert een basisstructuur voor het uitgeven van een toepassing Node.js naar een Cloud-Service. Het bevat de configuratiebestanden die nodig zijn voor publicatie in Azure. De cmdlet verandert ook uw werkmap in de map voor de service.

    De cmdlet wordt gemaakt van de volgende bestanden:

    -   **ServiceConfiguration.Cloud.cscfg**, **ServiceConfiguration.Local.cscfg** en **ServiceDefinition.csdef**: Azure-specifieke bestanden nodig zijn voor uw toepassing te publiceren. Zie [overzicht van het maken van een gehoste Service voor Azure]voor meer informatie.

    -   **deploymentSettings.json**: lokale instellingen die worden gebruikt door de implementatie van Azure PowerShell cmdlets worden opgeslagen.

4.  Voer de volgende opdracht om een nieuwe rol van de webpagina toevoegen:

        Add-AzureNodeWebRole

    ![De uitvoer van de opdracht Add-AzureNodeWebRole][The output of the Add-AzureNodeWebRole command]

    De cmdlet **Add-AzureNodeWebRole** maakt een basistoepassing Node.js. De bestanden **.csfg** en **.csdef** voor de configuratie-items voor de nieuwe rol toevoegen ook gewijzigd.

    > [AZURE.NOTE] Als u geen naam van een functie opgeeft, wordt een standaardnaam gebruikt. U kunt een naam opgeven als de eerste cmdlet-parameter:`Add-AzureNodeWebRole MyRole`

De app Node.js is gedefinieerd in het bestand **server.js**, opgeslagen in de map voor de Webrol (standaard**WebRole1** ). Dit is de code:

    var http = require('http');
    var port = process.env.port || 1337;
    http.createServer(function (req, res) {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('Hello World\n');
    }).listen(port);

Deze code is in wezen hetzelfde als het monster "Hello World" op de website van [nodejs.org] , behalve het poortnummer dat is toegewezen door de cloud-omgeving wordt gebruikt.

## <a name="deploy-the-application-to-azure"></a>De Azure-toepassing implementeren

    [AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

### <a name="download-the-azure-publishing-settings"></a>Download de Azure publicatie-instellingen

Voor de implementatie van de toepassing naar Azure, moet u eerst de publicatie-instellingen voor uw abonnement op Azure downloaden.

1.  De volgende Azure PowerShell-cmdlet uitvoert:

        Get-AzurePublishSettingsFile

    Dit zal uw browser gebruiken om te navigeren naar de downloadpagina voor publicatie-instellingen. Mogelijk moet u zich aanmelden met een Microsoft-Account. Zo ja, gebruik de account die is gekoppeld aan uw abonnement op Azure.

    Sla het gedownloade profiel op een locatie die u eenvoudig kunt openen.

2.  Uitvoeren na de cmdlet importeert de publishing profiel dat u hebt gedownload:

        Import-AzurePublishSettingsFile [path to file]


    > [AZURE.NOTE] Na het importeren van de publicatie-instellingen, kunt u eventueel het bestand gedownload .publishSettings verwijderen omdat het informatie waardoor iemand toegang tot uw account bevat.

### <a name="publish-the-application"></a>Publiceer de toepassing

Als u wilt publiceren, voert u de volgende opdrachten:

    $ServiceName = "NodeHelloWorld" + $(Get-Date -Format ('ddhhmm'))   
    Publish-AzureServiceProject -ServiceName $ServiceName  -Location "East US" -Launch

- **-Servicenaam** geeft de naam voor de distributie. Dit moet een unieke naam zijn, anders mislukt de het proces publiceren. De opdracht **Get datum** kopspijkertjes op een datum/tijd-tekenreeks die de naam uniek te maken.

- **-Locatie** geeft het datacenter die in de toepassing wordt beheerd. Gebruik de cmdlet **Get-AzureLocation** voor een overzicht van beschikbare datacenters.

- **-Starten** wordt een browservenster geopend en gaat u naar de gehoste service nadat de implementatie is voltooid.

Na het publiceren is voltooid, ziet u een antwoord van de volgende strekking:

![De uitvoer van de opdracht Publiceren AzureService][The output of the Publish-AzureService command]

> [AZURE.NOTE]
> Het kan enkele minuten duren voordat de toepassing te implementeren en beschikbaar wanneer eerst gepubliceerd.

Nadat de installatie is voltooid, wordt een browservenster openen en navigeert u naar de cloud-service.

![Een browservenster de hello world-pagina. de URL geeft dat de pagina wordt gehost op Azure.][A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]

De toepassing wordt nu uitgevoerd op Azure.

De cmdlet **Publiceren AzureServiceProject** voert de volgende stappen uit:

1.  Hiermee maakt u een pakket te implementeren. Het pakket bevat alle bestanden in de toepassingsmap van uw.

2.  Maakt een nieuwe **opslag account** als dit nog niet bestaat. De opslag van Azure rekening wordt gebruikt voor het opslaan van het toepassingspakket tijdens de implementatie. U kunt de opslag account veilig verwijderen nadat de implementatie is voltooid.

3.  Als nog niet bestaat, maakt een nieuwe **wolk service** . Een **cloud-service** is de container waarin de toepassing wordt gehost als het naar Azure is geïmplementeerd. Zie [overzicht van het maken van een gehoste Service voor Azure]voor meer informatie.

4.  Hiermee geeft het pakket in Azure.


## <a name="stopping-and-deleting-your-application"></a>Stoppen en de toepassing verwijderen

Na de implementatie van uw toepassing, kunt u uitgeschakeld zodat u voorkomen de extra kosten dat kunt. Azure wissels web rol exemplaren per uur van de verbruikte servertijd. Servertijd wordt verbruikt nadat de toepassing is geïmplementeerd, zelfs als de exemplaren zijn niet actief en gestopt worden.

1.  Stop de service implementatie gemaakt in de vorige sectie, met de volgende cmdlet in de Windows PowerShell-venster:

        Stop-AzureService

    De service wordt gestopt, kan enkele minuten duren. Wanneer de service wordt gestopt, wordt een bericht weergegeven dat is gestopt.

    ![De status van de opdracht Stop AzureService][The status of the Stop-AzureService command]

2.  Als u wilt verwijderen van de service, roept de volgende cmdlet:

        Remove-AzureService

    Voer desgevraagd **Y** om de service te verwijderen.

    Als u de service verwijdert, kan enkele minuten duren. U ontvangt een bericht dat aangeeft dat de service is verwijderd nadat de service is verwijderd.

    ![De status van de opdracht verwijderen AzureService][The status of the Remove-AzureService command]

    > [AZURE.NOTE] Als de service verwijdert niet de opslag-account dat is gemaakt toen de service werd aanvankelijk gepubliceerd en blijft u wordt gefactureerd voor opslag gebruikt. Als niets anders de opslag wordt gebruikt, is het raadzaam om deze te verwijderen.

## <a name="next-steps"></a>Volgende stappen

Zie [Node.js Developer Center]voor meer informatie.

<!-- URL List -->

[Vergelijking van Azure Websites, Cloud Services en virtuele Machines]: ../app-service-web/choose-web-site-cloud-service-vm.md
[met behulp van een lichtgewicht web app.]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
[Azure Powershell]: ../powershell-install-configure.md
[Azure SDK voor .NET 2.7]: http://www.microsoft.com/en-us/download/details.aspx?id=48178
[Verbinding maken met PowerShell]: ../powershell-install-configure.md#how-to-connect-to-your-subscription
[nodejs.org]: http://nodejs.org/
[Overzicht van het maken van een gehoste Service voor Azure]: https://azure.microsoft.com/documentation/services/cloud-services/
[Node.js Developer Center]: https://azure.microsoft.com/develop/nodejs/

<!-- IMG List -->

[The result of the New-AzureService helloworld command]: ./media/cloud-services-nodejs-develop-deploy-app/node9.png
[The output of the Add-AzureNodeWebRole command]: ./media/cloud-services-nodejs-develop-deploy-app/node11.png
[A web browser displaying the Hello World web page]: ./media/cloud-services-nodejs-develop-deploy-app/node14.png
[The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node19.png
[A browser window displaying the hello world page; the URL indicates the page is hosted on Azure.]: ./media/cloud-services-nodejs-develop-deploy-app/node21.png
[The status of the Stop-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node48.png
[The status of the Remove-AzureService command]: ./media/cloud-services-nodejs-develop-deploy-app/node49.png
