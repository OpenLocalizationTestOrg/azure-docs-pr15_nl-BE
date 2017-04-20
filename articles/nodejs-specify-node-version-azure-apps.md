<properties
    pageTitle="Een versie van Node.js opgeven"
    description="Informatie over het opgeven van de versie van Node.js gebruikt door websites Azure en Cloud Services"
    services=""
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="specifying-a-nodejs-version-in-an-azure-application"></a>Een Node.js versie op te geven in een toepassing Azure

Wanneer een Node.js toepassing die als host fungeert, is het raadzaam om ervoor te zorgen dat uw toepassing gebruikmaakt van een specifieke versie van Node.js. Er zijn verschillende manieren om dit te doen voor toepassingen die worden gehost op Azure.

##<a name="default-versions"></a>Standaardversies

De Node.js versies geleverd door Azure worden voortdurend bijgewerkt. Tenzij anders vermeld, de standaardversie die is opgegeven in de `WEBSITE_NODE_DEFAULT_VERSION` variabele wordt gebruikt. U kunt deze waarde overschrijven, volg de stappen in de volgende secties van dit artikel

> [AZURE.NOTE] Als u de toepassing in een Azure Cloud-Service (web of werknemer rol) host en is de eerste keer dat u de toepassing hebt geïmplementeerd, probeert Azure te gebruiken dezelfde versie van Node.js zoals u hebt geïnstalleerd op uw ontwikkelomgeving als deze overeenkomt met een van de standaardversies beschikbaar op Azure.

##<a name="versioning-with-packagejson"></a>Versiebeheer met package.json

U kunt opgeven dat de versie van Node.js moet worden gebruikt door het volgende toe te voegen aan het bestand **package.json** :

    "engines":{"node":version}

*Versie* is waar het specifieke versienummer. U kunt kunt meer complexe voorwaarden opgeven voor versie, zoals:

    "engines":{"node": "0.6.22 || 0.8.x"}

Aangezien 0.6.22 niet een van de versies die beschikbaar zijn in de hosting-omgeving, de hoogste versie van de reeks die beschikbaar zal zijn 0,8 gebruikt - 0.8.4.

##<a name="versioning-websites-with-app-settings"></a>Versioning Websites met App instellingen
Als u de toepassing in een Website beheert, kunt u de omgevingsvariabele **WEBSITE_NODE_DEFAULT_VERSION** instellen op de gewenste versie. 

##<a name="versioning-cloud-services-with-powershell"></a>Versioning Cloud Services met PowerShell

Als u de toepassing in een Cloud-Service beheert en distribueert de toepassing met Azure PowerShell, kunt u de standaardversie Node.js overschrijven met behulp van de PowerShell-cmdlet **Set-AzureServiceProjectRole** . Bijvoorbeeld:

    Set-AzureServiceProjectRole WebRole1 Node 0.8.4

Opmerking dat de parameters in het bovenstaande overzicht zijn hoofdlettergevoelig.  U kunt controleren of dat de juiste versie van Node.js is geselecteerd door de eigenschap **motoren** in uw rol van **package.json**te controleren.

Ook kunt u de **Get-AzureServiceProjectRoleRuntime** voor een lijst van Node.js versies beschikbaar voor toepassingen die worden gehost als een Cloud-Service.  Controleer altijd of de versie van uw project afhankelijk van is Node.js is in deze lijst.

##<a name="using-a-custom-version-with-azure-websites"></a>Met behulp van een aangepaste versie met Azure Websites

Azure biedt verschillende standaardversies van Node.js, kunt u een versie die niet standaard wordt geleverd. Als uw toepassing wordt gehost als een Azure-Website, kunt u dit doen met behulp van het bestand **iisnode.yml** . De volgende stappen doorlopen van het proces van het gebruik van een aangepaste versie van Node.Js met een Azure-Website:

1. Maak een nieuwe map en vervolgens een **server.js** bestand in de map maken. Het bestand **server.js** moet het volgende bevatten:

        var http = require('http');
        http.createServer(function(req,res) {
          res.writeHead(200, {'Content-Type': 'text/html'});
          res.end('Hello from Azure running node version: ' + process.version + '</br>');
        }).listen(process.env.PORT || 3000);

    De Node.js versie wordt gebruikt wanneer u de website wordt weergegeven.

2. Maak een nieuwe Website en noteer de naam van de site. Bijvoorbeeld de volgende maakt gebruik van de [opdrachtregelprogramma's Azure] een nieuwe Azure-Website met de naam **MijnWebsite**maken en vervolgens een Git repository voor de website inschakelen.

        azure site create mywebsite --git

3. Maak een nieuwe map met de naam **opslaglocatie** als een onderliggend element van de map die het bestand **server.js** .

4. Download de specifieke versie van **node.exe** (de Windows-versie) die u wilt gebruiken met uw toepassing. Bijvoorbeeld het volgende gebruikt **curl-** versie 0.8.1 downloaden:

        curl -O http://nodejs.org/dist/v0.8.1/node.exe

    Sla het bestand **node.exe** naar de map **bin** eerder hebt gemaakt.

5. Maak een **iisnode.yml** bestand in dezelfde map als het bestand **server.js** en voeg de volgende inhoud toe aan het bestand **iisnode.yml** :

        nodeProcessCommandLine: "D:\home\site\wwwroot\bin\node.exe"

    Dit pad is waar het bestand **node.exe** in uw project zich bevindt wanneer u uw toepassing naar de Azure-Website hebt gepubliceerd.

6. Publiceer de toepassing. Bijvoorbeeld sinds ik een nieuwe website met de parameter--git eerder gemaakt, wordt de volgende opdrachten de toepassingsbestanden toevoegen aan mijn lokale Git repository en duwen ze vervolgens naar de bibliotheek van de website:

        git add .
        git commit -m "testing node v0.8.1"
        git push azure master

    Nadat de toepassing is gepubliceerd, kunt u de website openen in een browser. U ziet een bericht met de mededeling "Hello from Azure knooppunt versie: v0.8.1".

##<a name="next-steps"></a>Volgende stappen

Nu dat u hoe geeft u de versie van Node.js gebruikt door de toepassing begrijpt, meer informatie over het [werken met modules], [bouwen en implementeren van een Node.js website]en [het gebruik van de opdrachtregelprogramma's van Azure voor Mac en Linux].

Zie [Node.js Developer Center](/develop/nodejs/)voor meer informatie.

[Het gebruik van de opdrachtregelprogramma's voor Azure voor Mac en Linux]: xplat-cli-install.md
[Azure opdrachtregelprogramma 's]: xplat-cli-install.md
[werken met modules]: nodejs-use-node-modules-azure-apps.md
[bouwen en implementeren van een website Node.js]: web-sites-nodejs-develop-deploy-mac.md
