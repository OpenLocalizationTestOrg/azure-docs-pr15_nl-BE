<properties 
    pageTitle="Fouten opsporen in een Java Web App op Azure in Eclips | Microsoft Azure" 
    description="In deze zelfstudie wordt beschreven hoe u de Azure Toolkit voor Eclips gebruiken voor foutopsporing van een Java Web App op Azure." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="selvasingh" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="asirveda;robmcm"/>

# <a name="debug-a-java-web-app-on-azure-in-eclipse"></a>Fouten opsporen in een Java Web App op Azure in Eclips

Deze zelfstudie laat zien hoe een Java Web App op Azure uitgevoerd met behulp van de [Toolkit voor Eclips Azure]debug. U een eenvoudig voorbeeld van Java Server-pagina (JSP) voor deze zelfstudie wilt gebruiken omwille van de eenvoud, maar de stappen voor een Java servlet vergelijkbaar zou zijn wanneer u foutopsporing op Azure.

Wanneer u deze zelfstudie hebt voltooid, zal uw toepassing uitzien in de volgende afbeelding wanneer u deze in de Eclips foutopsporing:

![][01]
 
## <a name="prerequisites"></a>Vereisten

* Een Java Developer Kit (JDK), v-1,8 of hoger.
* Voor ontwikkelaars van Java EE, IDE-Eclips Indigo of hoger. Dit kan worden gedownload van <http://www.eclipse.org/downloads/>.
* Een verdeling van een Java-webserver of een toepassing zoals Apache Tomcat of Jetty.
* Een Azure-abonnement, die kan worden verkregen uit <https://azure.microsoft.com/en-us/free/> of <http://azure.microsoft.com/pricing/purchase-options/>.
* De Azure Toolkit voor Eclips. Zie [de Azure Toolkit voor Eclips installeren]voor meer informatie.
* Een dynamische Web Project gemaakt en geïmplementeerd op Azure App-Service. Zie bijvoorbeeld [maken een Hello World Web App voor Azure in Eclips].

## <a name="to-debug-a-java-web-app-on-azure"></a>Voor foutopsporing van een Java Web App op Azure

Om deze stappen in deze sectie kunt u een dynamische webproject die u al hebt geïmplementeerd als een Java Web App op Azure, een [Dynamische webpagina voorbeeldproject] downloaden en volg de stappen in het [maken van een Hello World Web App voor Azure in Eclips] te implementeren op Azure. 

1. Eclips openen.

1. Configureer time-outs voor foutopsporing op afstand:

    1. Klik op het **Windows** -menu in de Eclips en klik op **Voorkeuren**.
    1. Vouw het knooppunt **Java** en selecteert u **Foutopsporing**.
    1. De **Debugger-out (ms)** en de **time-out (ms) starten** -instellingen te configureren `120000`.

        ![][02]

    1. Klik op **OK** om het dialoogvenster **Voorkeuren** te sluiten.

1. In de weergave van de Eclips Project Explorer met de rechtermuisknop op het dynamische Web Project die u hebt geïmplementeerd op Azure. Wanneer het snelmenu wordt weergegeven, selecteert u **Foutopsporing als**en klik vervolgens op **Azure Web App**.

    ![][03]

1. Als dit de eerste keer dat u fouten van uw dynamische webproject opsporen wilt, wordt het dialoogvenster **Foutopsporing configuraties** wordt geopend. u kunt de standaardwaarden die zijn opgegeven met de Toolkit op het tabblad **verbinding** accepteren. **Op het tabblad **bron** klikt u op **toevoegen**en vervolgens de **Java-project**en selecteer **Dynamische Web-Project**.** Als u deze stappen hebt voltooid, klikt u op de **Debug**.

    ![][04]

1. Wanneer u wordt gevraagd **Foutopsporing op afstand inschakelen in het externe Web App nu?**, klikt u op **OK**.

1. Die **uw web app is gereed voor foutopsporing op afstand**, klik op **OK**.

    ![][05]

1. Wanneer het dialoogvenster **Foutopsporing configuraties** opnieuw wordt weergegeven, klikt u op **fouten opsporen**.

1. Een opdrachtprompt van Windows of Unix shell openen en voorbereiden van de benodigde verbindingsinstellingen voor foutopsporing; u moet wachten totdat de verbinding met uw externe Java Web app voltooid is voordat u verdergaat. Als u Windows gebruikt, lijkt het alsof de volgende afbeelding.

    ![][06]

1. Een breekpunt in de JSP-pagina invoegen en vervolgens de URL voor uw Java Web App in een browser openen:

    1. Open up **Explorer Azure** in Eclips.
    1. Navigeer naar het **Web Apps** en de Java Web App u fouten wilt opsporen.
    1. Klik met de rechtermuisknop op het Web App en klik op **openen in Browser**.
    1. Eclips treedt nu in de foutopsporingsmodus.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center].

Zie voor meer informatie over het maken van Azure Web Apps, [Web Apps-overzicht].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure Toolkit voor Eclips]: ../azure-toolkit-for-eclipse.md
[Installatie van de Toolkit Azure voor Eclips]: ../azure-toolkit-for-eclipse-installation.md
[Maak een Hello World Web App voor Azure in Eclips]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Voorbeeld van dynamische webproject]: http://go.microsoft.com/fwlink/?LinkId=817337

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Web Apps-overzicht]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-eclipse/01-debug-java-web-app-in-eclipse.png
[02]: ./media/app-service-web-debug-java-web-app-in-eclipse/02-configure-eclipse-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-eclipse/03-debug-as.png
[04]: ./media/app-service-web-debug-java-web-app-in-eclipse/04-debug-configurations.png
[05]: ./media/app-service-web-debug-java-web-app-in-eclipse/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-eclipse/06-windows-command-prompt-connection-successful-to-remote.png
