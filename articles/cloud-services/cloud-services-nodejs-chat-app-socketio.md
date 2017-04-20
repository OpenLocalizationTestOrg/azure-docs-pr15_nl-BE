<properties 
    pageTitle="Node.js-toepassing met behulp van Socket.io | Microsoft Azure" 
    description="Informatie over het gebruik van socket.io in een node.js-toepassing die wordt gehost op Azure." 
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
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="build-a-nodejs-chat-application-with-socketio-on-an-azure-cloud-service"></a>Bouwen van een toepassing Node.js Chat met Socket.IO op een Azure Cloud-Service

Socket.IO biedt realtime communicatie tussen tussen de node.js server en clients. Deze handleiding helpt u bij het hosten van een socket. I/o op basis van chatprogramma op Azure. Zie <http://socket.io/>voor meer informatie op Socket.IO.

Een schermafbeelding van de voltooide toepassing lager is dan:

![Een venster voor het weergeven van de service in Azure][completed-app]  

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat de volgende producten en versies zijn geïnstalleerd om te kunnen voltooien in het voorbeeld in dit artikel:

* Installeer [Visual Studio 2013](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx)
* [Node.js](https://nodejs.org/download/) installeren
* Installeer [Python versie 2.7.10](https://www.python.org/)

## <a name="create-a-cloud-service-project"></a>Maak een Cloud Service Project

De volgende stappen uit maken het project van cloud-service die als host voor de toepassing van de Socket.IO fungeert.

1. Zoeken in het **Menu Start** of het **Startscherm van** **Windows PowerShell**. Ten slotte met de rechtermuisknop op **Windows PowerShell** en selecteer **Als Administrator uitvoeren**.

    ![Azure PowerShell-pictogram][powershell-menu]

2. Maak een map met de naam **c:\\knooppunt**. 
 
        PS C:\> md node

3. Eerst naar de **c:\\knooppunt** directory
 
        PS C:\> cd node

4. Voer de volgende opdrachten voor het maken van een nieuwe oplossing met de naam **chatapp** en de rol van een werknemer met de naam **WorkerRole1**:

        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole

    U ziet het volgende antwoord:

    ![De uitvoer van de nieuwe azureservice en het toevoegen van de azurenodeworkerrolecmdlets](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## <a name="download-the-chat-example"></a>Voorbeeld van de Chat downloaden

Voor dit project gebruiken we het voorbeeld chat uit de [opslagplaats Socket.IO GitHub]. Voer de volgende stappen uit om het voorbeeld te downloaden en toe te voegen aan het project dat u eerder hebt gemaakt.

1.  Maak een lokale kopie van de bibliotheek met behulp van de knop **kopiëren** . U kunt ook de knop **ZIP** te downloaden van het project.

    ![Een browser-venster weergeven met het pictogram voor ZIP download gemarkeerd, https://github.com/LearnBoost/socket.io/tree/master/examples/chat,][chat-example-view]

3.  De mapstructuur van de lokale bibliotheek te navigeren totdat u aankomt bij de **voorbeelden\\chat** directory. Kopieer de inhoud van deze map naar de **C:\\knooppunt\\chatapp\\WorkerRole1** directory eerder hebt gemaakt.

    ![Verkenner-weergave van de inhoud van de voorbeelden\\geëxtraheerd uit het archief van chat-directory][chat-contents]

    De gemarkeerde items in het screenshot hierboven worden de bestanden gekopieerd van de **voorbeelden\\chat** directory

4.  In de **C:\\knooppunt\\chatapp\\WorkerRole1** map, verwijder het bestand **server.js** en wijzig de naam van het bestand **app.js** naar **server.js**. Dit het standaardbestand **server.js** eerder hebt gemaakt door de cmdlet **Add-AzureNodeWorkerRole** wordt verwijderd en vervangen door het bestand van de toepassing van het chat-voorbeeld.

### <a name="modify-serverjs-and-install-modules"></a>Server.js wijzigen en Modules installeren

Voordat u de toepassing te testen in de emulator Azure, maken we enkele kleine wijzigingen. Voer de volgende stappen uit om het bestand server.js:

1.  Open het bestand **server.js** in Visual Studio of een teksteditor.

2.  Zoek de sectie **Module afhankelijkheden** aan het begin van de server.js en wijzig de regel met **sio = require('.. //.. lib//socket.IO ")** tot **sio require('socket.io') =** als volgt:

        var express = require('express')
        , stylus = require('stylus')
        , nib = require('nib')
        //, sio = require('..//..//lib//socket.io'); //Original
        , sio = require('socket.io');                //Updated

3.  Om dat de toepassing op de juiste poort luistert, open server.js in Kladblok of uw favoriete editor en wijzig de volgende regel te **3000** vervangen door **process.env.port** , zoals hieronder wordt weergegeven:

        //app.listen(3000, function () {            //Original
        app.listen(process.env.port, function () {  //Updated
          var addr = app.address();
          console.log('   app listening on http://' + addr.address + ':' + addr.port);
        });

Gebruik de volgende stappen vereist modules te installeren na het opslaan van de wijzigingen in **server.js**, en vervolgens de toepassing te testen in de emulator Azure:

1.  Mappen met **Azure PowerShell**, wijzig de **C:\\knooppunt\\chatapp\\WorkerRole1** map en gebruik de volgende opdracht op de modules die nodig zijn door deze toepassing te installeren:

        PS C:\node\chatapp\WorkerRole1> npm install

    Hiermee installeert u de modules die zijn opgenomen in het bestand package.json. Nadat de opdracht is voltooid, ziet u uitvoer van de volgende strekking:

    ![De uitvoer van de npm opdracht install][The-output-of-the-npm-install-command]

4.  Omdat in dit voorbeeld oorspronkelijk een deel van de bibliotheek Socket.IO GitHub was en rechtstreeks verwijst naar de bibliotheek Socket.IO door het relatieve pad, Socket.IO is niet naar wordt verwezen in het bestand package.json, zodat we moeten worden geïnstalleerd door de volgende opdracht:

        PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### <a name="test-and-deploy"></a>Testen en implementeren

1.  De emulator starten door de volgende opdracht:

        PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch

2.  Open een browser en Ga naar **http://127.0.0.1**.

3.  Wanneer het browservenster wordt geopend, voert u een bijnaam en klikt u vervolgens op ENTER drukken.
    Dit zal u om berichten te posten als een specifieke bijnaam. Test de functionaliteit voor meerdere gebruikers, extra vensters met dezelfde URL openen en voer andere bijnamen.

    ![Twee browservensters chatberichten van Gebruiker1 en Gebruiker2 weergeven](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)

3.  Na het testen van de toepassing, de emulator te stoppen door de volgende opdracht:

        PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator

4.  Gebruik de cmdlet **Publiceren AzureServiceProject** voor de implementatie van de toepassing op Azure. Bijvoorbeeld:

        PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch

    > [AZURE.IMPORTANT] Een unieke naam te gebruiken, anders is het proces publiceren mislukken. Nadat de installatie is voltooid, wordt de browser openen en navigeert u naar de service geïmplementeerd.
    > 
    > Als er een met de melding foutbericht dat de naam van het opgegeven abonnement niet in het profiel geïmporteerd publiceren bestaat, moet u downloaden en importeren van het profiel publiceren voor uw abonnement Azure voordat. Zie de sectie **de Azure-toepassing implementeren** van [bouwen en implementeren van een toepassing Node.js een Azure Cloud-service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)

    ![Een venster voor het weergeven van de service in Azure][completed-app]

    > [AZURE.NOTE] Als er een met de melding foutbericht dat de naam van het opgegeven abonnement niet in het profiel geïmporteerd publiceren bestaat, moet u downloaden en importeren van het profiel publiceren voor uw abonnement Azure voordat. Zie de sectie **de Azure-toepassing implementeren** van [bouwen en implementeren van een toepassing Node.js een Azure Cloud-service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)

Uw toepassing wordt nu uitgevoerd op Azure en chat-berichten tussen de verschillende clients met behulp van Socket.IO kan doorsturen.

> [AZURE.NOTE] In dit voorbeeld is voor eenvoud, beperkt tot het chatten tussen gebruikers verbinding met hetzelfde exemplaar. Dit betekent dat als de service cloud maakt twee exemplaren van de werknemer rol, gebruikers is alleen mogelijk om te chatten met andere gebruikers zijn verbonden met hetzelfde exemplaar van de rol van werknemer. Als u wilt schalen in de toepassing werkt met meerdere exemplaren van de rol, u een technologie zoals Service Bus instanties delen de Socket.IO winkel staat. Zie voor voorbeelden van de Service Bus wachtrijen en onderwerpen gebruik monsters in de [SDK voor Node.js GitHub opslagplaats Azure](https://github.com/WindowsAzure/azure-sdk-for-node).

##<a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe een eenvoudig chatprogramma gehost in een Azure Cloud-Service maken. Zie [een toepassing Node.js Chat met Socket.IO op een Azure-website maken]als u wilt weten hoe u als host voor deze toepassing in een Website Azure,[chatwebsite].

Zie ook de [Node.js Developer Center](/develop/nodejs/)voor meer informatie.

  [chatwebsite]: /develop/nodejs/tutorials/website-using-socketio/

  [Azure SLA]: http://www.windowsazure.com/support/sla/
  [Azure SDK for Node.js GitHub repository]: https://github.com/WindowsAzure/azure-sdk-for-node
  [completed-app]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-10.png
  [Azure SDK for Node.js]: https://www.windowsazure.com/develop/nodejs/
  [Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
  [Socket.IO GitHub opslagplaats]: https://github.com/LearnBoost/socket.io/tree/0.9.14
  [Azure Considerations]: #windowsazureconsiderations
  [Hosting the Chat Example in a Worker Role]: #hostingthechatexampleinawebrole
  [Summary and Next Steps]: #summary
  [powershell-menu]: ./media/cloud-services-nodejs-chat-app-socketio/azure-powershell-start.png

  [chat example]: https://github.com/LearnBoost/socket.io/tree/master/examples/chat
  [chat-example-view]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png
  
  
  [chat-contents]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-5.png
  [The-output-of-the-npm-install-command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-7.png
  [The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-9.png
  
 
