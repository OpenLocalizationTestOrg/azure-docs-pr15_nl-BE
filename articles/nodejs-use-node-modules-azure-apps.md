<properties
    pageTitle="Werken met Modules Node.js"
    description="Informatie over het werken met modules Node.js als met Azure App Service of Cloud-Services."
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


# <a name="using-nodejs-modules-with-azure-applications"></a>Met behulp van Node.js Modules met Azure toepassingen

Dit document bevat richtlijnen voor het gebruik van Node.js modules met toepassingen die worden gehost op Azure. Het verschaft leidraden voor ervoor te zorgen dat uw toepassing gebruikmaakt van een specifieke versie van de module als u systeemeigen modules met Azure.

Als u al bekend met bent is gebruik van Node.js modules, **package.json** en **npm-shrinkwrap.json** -bestanden, de volgende een kort overzicht van wat in dit artikel wordt beschreven:

* Azure App Service begrijpt de bestanden **package.json** en **npm-shrinkwrap.json** en modules op basis van gegevens in deze bestanden kunt installeren.
* Azure Cloud Services verwacht dat alle modules te worden geïnstalleerd op de ontwikkelomgeving en het **knooppunt\_modules** map moeten worden opgenomen als onderdeel van het implementatiepakket. Het is mogelijk ondersteuning voor het installeren van modules met **package.json** of **npm-shrinkwrap.json** bestanden op Cloud diensten, maar dit aanpassing van de standaardscripts die worden gebruikt door de Service Cloud projecten vereist in te schakelen. Zie voor een voorbeeld van hoe dit [Azure opstarten taak npm installeren om te voorkomen dat knooppunt modules distribueren](https://github.com/woloski/nodeonazure-blog/blob/master/articles/startup-task-to-run-npm-in-azure.markdown)

> [AZURE.NOTE] Azure virtuele Machines worden niet in dit artikel wordt besproken, zoals de implementatie-ervaring in een VM afhankelijk van het besturingssysteem die worden gehost door de virtuele Machine zijn zal.

##<a name="nodejs-modules"></a>Node.js Modules

Modules zijn geladen JavaScript-pakketten die specifieke functionaliteit voor uw toepassing bieden. Een module wordt meestal geïnstalleerd met het opdrachtregelprogramma **npm** , maar sommige (zoals de HTTP-module) worden geleverd als onderdeel van het pakket core Node.js.

Wanneer modules zijn geïnstalleerd, worden opgeslagen in de **knooppunt\_modules** map in de hoofdmap van de toepassing directory-structuur. Elke module binnen de **knooppunt\_modules** directory onderhoudt een eigen **knooppunt\_modules** map waarin alle modules die afhankelijk is van, en dit opnieuw wordt herhaald voor elke module helemaal naar beneden op de afhankelijkheidsketen. Hierdoor kan elke module die is geïnstalleerd als u een eigen versievereisten voor de modules afhankelijk, maar dit leiden heel grote mapstructuur tot kan.

Bij de implementatie van de **knooppunt\_modules** directory als onderdeel van uw toepassing, het vergroten van de distributie in vergelijking met het gebruik van een bestand **package.json** of **npm-shrinkwrap.json** ; Dit betekent echter dat de versie van de modules productie worden gebruikt dezelfde als die in ontwikkeling zijn.

###<a name="native-modules"></a>Systeemeigen Modules

De meeste modules zijn alleen maar tekst zonder opmaak JavaScript-bestanden, zijn sommige modules platformspecifieke binaire afbeeldingen. Deze modules zijn gecompileerd tijdens de installatie, meestal met behulp van Python en knooppunt gyp. Aangezien Azure Cloud Services zijn afhankelijk van de **knooppunt\_modules** wordt geïmplementeerd als onderdeel van de toepassing, een ingebouwde module die deel uitmaken van de geïnstalleerde modules map moet werken in een cloud-service als deze is geïnstalleerd en gecompileerd op een Windows-systeem voor ontwikkeling.

Azure App-Service biedt geen ondersteuning voor alle native modules en tijdens het compileren met zeer specifieke vereisten kan mislukken. Terwijl sommige populaire modules zoals MongoDB optionele native afhankelijkheden en werk gewoon fijn zonder hen, bleek twee oplossingen met bijna alle native modules beschikbaar vandaag geslaagd:

* **Npm installeren** op een Windows-computer die alle native van de module vereisten geïnstalleerd heeft uitgevoerd. Implementeert u de gemaakte **knooppunt\_modules** map als onderdeel van de toepassing naar Azure App-Service.
* Azure App-Service kan worden geconfigureerd voor het uitvoeren van aangepaste bash of shell-scripts tijdens de implementatie, waardoor u de verkoopkans aangepaste opdrachten uit te voeren en nauwkeurig configureren van de manier waarop **npm installeren** wordt uitgevoerd. Zie voor een video waarin wordt beschreven hoe u dit moet doen, [Aangepaste Website-implementatie van Scripts met Kudu].

###<a name="using-a-packagejson-file"></a>Met behulp van een package.json-bestand

Het bestand **package.json** is een manier om op te geven van het hoogste niveau afhankelijkheden uw toepassing vereist zodat de hosting platform installeren kunt de afhankelijkheden in plaats van dat u hoeft te nemen de **knooppunt\_pakketten** map als onderdeel van de implementatie. Nadat de toepassing is geïmplementeerd, wordt de opdracht **npm installeren** gebruikt voor het parseren van het bestand **package.json** en installeren de afhankelijkheden die zijn vermeld.

Tijdens de ontwikkeling, kunt u de **--Opslaan**, **--Opslaan dev**, of **--Opslaan optioneel** parameters bij het installeren van modules om een vermelding voor de module automatisch toevoegen aan het bestand **package.json** . Zie [npm installeren](https://docs.npmjs.com/cli/install)voor meer informatie.

Een potentieel probleem met het bestand **package.json** is het alleen de versie voor het hoogste niveau afhankelijkheden bevat. Elke module geïnstalleerd kan of kan de versie van de modules die afhankelijk van is op te geven en is het mogelijk dat u uiteindelijk over een afhankelijkheidsketen van andere dan de één in ontwikkeling.

> [AZURE.NOTE]
> Bij het implementeren van Azure App-Service, als een systeemeigen module wordt verwezen naar het bestand <b>package.json</b> ziet u een vergelijkbaar met de volgende fout bij het publiceren van de toepassing met behulp van Git:

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'

>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1


###<a name="using-a-npm-shrinkwrapjson-file"></a>Met behulp van een bestand npm-shrinkwrap.json

Het bestand **npm-shrinkwrap.json** is een poging om de beperkingen van de module versies van het bestand **package.json** . Terwijl het bestand **package.json** bevat alleen de versies voor de modules van het hoogste niveau, bevat het bestand **npm-shrinkwrap.json** de versievereisten voor de volledige module afhankelijkheidsketen.

U kunt vergrendelen wanneer de toepassing gereed is voor productie,-versievereisten en een **npm-shrinkwrap.json** -bestand maken met behulp van de opdracht **npm verpakking** . Dit wordt gebruikt de versies die momenteel zijn geïnstalleerd de **knooppunt\_modules** map, en deze naar het bestand **npm shrinkwrap.json** vastleggen. Nadat de toepassing is geïmplementeerd in de hosting-omgeving, wordt de opdracht **npm installeren** gebruikt voor het parseren van het bestand **npm-shrinkwrap.json** en installeren de afhankelijkheden die zijn vermeld. Zie [npm verpakking](https://docs.npmjs.com/cli/shrinkwrap)voor meer informatie.

> [AZURE.NOTE]
>Bij het implementeren van Azure App-Service als de <b>npm-shrinkwrap.json</b> -bestand verwijst naar een systeemeigen module ziet u een vergelijkbaar met de volgende fout bij het publiceren van de toepassing met behulp van Git:

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'

>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1


##<a name="next-steps"></a>Volgende stappen

Nu u het gebruik van Node.js modules met Azure begrijpen, leren hoe [geeft u de versie van Node.js], [bouwen en implementeren van een Node.js web app]en [het gebruik van de opdrachtregelinterface van Azure voor Mac en Linux].

Zie [Node.js Developer Center](/develop/nodejs/)voor meer informatie.

[Geef de versie Node.js]: nodejs-specify-node-version-azure-apps.md
[Het gebruik van de opdrachtregelinterface van Azure voor Mac en Linux]: xplat-cli-install.md
[bouwen en implementeren van een web app Node.js.]: web-sites-nodejs-develop-deploy-mac.md
[Node.js Web Application with Storage on MongoDB (MongoLab)]: store-mongolab-web-sites-nodejs-store-data-mongodb.md
[Build and deploy a Node.js application to an Azure Cloud Service]: cloud-services-nodejs-develop-deploy-app.md
[Scripts voor aangepaste Website-implementatie met Kudu]: /documentation/videos/custom-web-site-deployment-scripts-with-kudu/
