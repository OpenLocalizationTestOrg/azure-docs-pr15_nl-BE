<properties 
    pageTitle="Fouten opsporen in een Java Web App op Azure in IntelliJ | Microsoft Azure" 
    description="In deze zelfstudie wordt beschreven hoe u de Azure Toolkit voor IntelliJ gebruiken voor foutopsporing van een Java Web App op Azure." 
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

# <a name="debug-a-java-web-app-on-azure-in-intellij"></a>Fouten opsporen in een Java Web App op Azure in IntelliJ

Deze zelfstudie laat zien hoe een Java Web App op Azure uitgevoerd met behulp van de [Toolkit voor IntelliJ Azure]debug. U een eenvoudig voorbeeld van Java Server-pagina (JSP) voor deze zelfstudie wilt gebruiken omwille van de eenvoud, maar de stappen voor een Java servlet vergelijkbaar zou zijn wanneer u foutopsporing op Azure.

Wanneer u deze zelfstudie hebt voltooid, zal uw toepassing uitzien in de volgende afbeelding wanneer u deze in IntelliJ foutopsporing:

![][01]
 
## <a name="prerequisites"></a>Vereisten

* Een Java Developer Kit (JDK), v-1,8 of hoger.
* IntelliJ IDEE Ultimate Edition. Dit kan worden gedownload van <https://www.jetbrains.com/idea/download/index.html>.
* Een verdeling van een Java-webserver of een toepassing zoals Apache Tomcat of Jetty.
* Een Azure-abonnement, die kan worden verkregen uit <https://azure.microsoft.com/en-us/free/> of <http://azure.microsoft.com/pricing/purchase-options/>.
* De Azure Toolkit voor IntelliJ. Zie [de Azure Toolkit voor IntelliJ installeren]voor meer informatie.
* Een dynamische Web Project gemaakt en geïmplementeerd op Azure App-Service. Zie bijvoorbeeld [maken een Hello World Web App voor Azure in IntelliJ].

## <a name="to-debug-a-java-web-app-on-azure"></a>Voor foutopsporing van een Java Web App op Azure

Om deze stappen in deze sectie kunt u een dynamische webproject die u al hebt geïmplementeerd als een Java Web App op Azure, een [Dynamische webpagina voorbeeldproject] downloaden en volg de stappen in het [maken van een Hello World Web App voor Azure in IntelliJ] te implementeren op Azure. 

1. Open het project Java Web App die u naar Azure in IntelliJ geïmplementeerd.

1. Klik op **uitvoeren** in het menu en klik vervolgens op **Configuratie bewerken**.

    ![][02]

1. Wanneer het dialoogvenster **Uitvoeren/Debug configuraties** wordt geopend: 

    1. Selecteer **Azure Web App**.
    1. Klik op **+** voor het toevoegen van een nieuwe configuratie.
    1. Geef een **naam** voor de configuratie.
    1. De overige standaardwaarden die worden voorgesteld door de Toolkit Azure accepteren en klik vervolgens op **OK**.

        ![][03]

1. Selecteer de Azure Web App debug-configuratie die u zojuist hebt gemaakt in het menu rechtsboven en klik op **Foutopsporing**

    ![][04]

1. Wanneer u wordt gevraagd **Foutopsporing op afstand inschakelen in het externe Web App nu?**, klikt u op **OK**.

1. Die **uw web app is gereed voor foutopsporing op afstand**, klik op **OK**.

    ![][05]

1. Selecteer de Azure Web App debug-configuratie die u zojuist hebt gemaakt in het menu rechtsboven en klik op op te **sporen**.

1. Een opdrachtprompt van Windows of Unix shell openen en voorbereiden van de benodigde verbindingsinstellingen voor foutopsporing; u moet wachten totdat de verbinding met uw externe Java Web app voltooid is voordat u verdergaat. Als u Windows gebruikt, lijkt het alsof de volgende afbeelding:

    ![][06]

1. Een breekpunt in de JSP-pagina invoegen en vervolgens de URL voor uw Java Web App in een browser openen:

    1. Open up **Explorer Azure** in IntelliJ.
    1. Navigeer naar het **Web Apps** en de Java Web App u fouten wilt opsporen.
    1. Klik met de rechtermuisknop op het Web App en klik op **openen in Browser**.
    1. IntelliJ treedt nu in de foutopsporingsmodus.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center].

Zie voor meer informatie over het maken van Azure Web Apps, [Web Apps-overzicht].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure Toolkit voor IntelliJ]: ../azure-toolkit-for-intellij.md
[Installeren van de Azure Toolkit voor IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Maak een Hello World Web App voor Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Voorbeeld van dynamische webproject]: http://go.microsoft.com/fwlink/?LinkId=817337

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Web Apps-overzicht]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-intellij/01-debug-java-web-app-in-intellij.png
[02]: ./media/app-service-web-debug-java-web-app-in-intellij/02-configure-intellij-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-intellij/03-debug-configuration.png
[04]: ./media/app-service-web-debug-java-web-app-in-intellij/04-select-debug.png
[05]: ./media/app-service-web-debug-java-web-app-in-intellij/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-intellij/06-windows-command-prompt-connection-successful-to-remote.png
