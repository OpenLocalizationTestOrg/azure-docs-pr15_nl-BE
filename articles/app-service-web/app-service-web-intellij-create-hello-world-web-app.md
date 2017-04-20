<properties 
    pageTitle="Maak een Hello World Web App voor Azure in IntelliJ | Microsoft Azure" 
    description="In deze zelfstudie wordt beschreven hoe u met behulp van de Toolkit Azure voor IntelliJ een Hello World Web App voor Azure." 
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
    ms.date="08/11/2016" 
    ms.author="asirveda;robmcm"/>

# <a name="create-a-hello-world-web-app-for-azure-in-intellij"></a>Maak een Hello World Web App voor Azure in IntelliJ

Deze zelfstudie laat zien hoe maken en implementeren van een eenvoudige Hello World toepassing Azure als een Web App met behulp van de [Toolkit voor IntelliJ Azure]. Een eenvoudig voorbeeld van JSP weergegeven voor eenvoud, maar zeer vergelijkbare stappen voor een Java servlet, dienstig zou zijn wat betreft Azure-implementatie is.

Wanneer u deze zelfstudie hebt voltooid, zal uw toepassing uitzien in de volgende afbeelding wanneer u deze in een webbrowser bekijkt:

![][01]
 
## <a name="prerequisites"></a>Vereisten

* Een Java Developer Kit (JDK), v-1,8 of hoger.
* IntelliJ IDEE Ultimate Edition. Dit kan worden gedownload van <https://www.jetbrains.com/idea/download/index.html>.
* Een verdeling van een Java-webserver of een toepassing zoals Apache Tomcat of Jetty.
* Een Azure-abonnement, die kan worden verkregen uit <https://azure.microsoft.com/free/> of <http://azure.microsoft.com/pricing/purchase-options/>.
* De Azure Toolkit voor IntelliJ. Zie [de Azure Toolkit voor IntelliJ installeren]voor meer informatie.

## <a name="to-create-a-hello-world-application"></a>Voor het maken van een toepassing Hallo wereld

Eerst, we beginnen uit met het maken van een Java-project.

1. IntelliJ, start en klik op het menu **bestand**en klik op **Nieuw**en klik vervolgens op **Project**.

   ![][02]

1. Klik in het dialoogvenster Nieuw Project **Java**, vervolgens **Webtoepassing**selecteren en klik op **volgende**.

   ![][03a]

   Als u wordt gevraagd om door te gaan met geen SDK toegewezen, klikt u op **Ja**.

   ![][03b]

1. Geef het project de **Java-Web-App-op-Azure**ten behoeve van deze zelfstudie, en klik vervolgens op **Voltooien**.

   ![][04]

1. In Project Explorer-weergave van IntelliJ, **Java-Web-App-op-Azure**, vouw vervolgens **web**vouwen en dubbelklik vervolgens op **index.jsp**.

   ![][05]

1. Wanneer het bestand index.jsp in IntelliJ wordt geopend, in tekst weer te geven dynamisch toevoegen **Hello World!** binnen de bestaande `<body>` element. Uw bijgewerkte `<body>` inhoud moet vergelijkbaar zijn met het volgende voorbeeld:

   `<body><b><% out.println("Hello World!"); %></b></body>` 

1. Index.jsp opslaan.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Implementatie van de toepassing naar een Container Azure Web App

Er zijn verschillende manieren waarop u een Java webtoepassing Azure kunt implementeren. In deze zelfstudie wordt beschreven in een van de eenvoudigste: de toepassing wordt geïmplementeerd naar een Container Azure Web App - geen speciale projecttype en geen extra hulpmiddelen nodig zijn. De JDK en de container webserversoftware krijgt u door Azure, dus het is niet nodig voor het uploaden van uw eigen; u hoeft uw Java Web App is. Hierdoor duurt het publicatieproces voor uw toepassing geen minuten seconden.

1. Klik met de rechtermuisknop op de **Java-Web-App-op-Azure** project in de Projectverkenner van IntelliJ. Wanneer het snelmenu wordt weergegeven, selecteert u **Azure**en klik op **publiceren als Azure Web App....**

   ![][06]

1. Als u niet reeds in Azure uit IntelliJ gesloten, wordt u gevraagd uw Azure rekening ondertekenen:

   ![][07]

   Opmerking: Als er meerdere accounts voor Azure, enkele van de vragen tijdens het aanmelden proces kan worden weergegeven meer dan eens zelfs als ze lijken hetzelfde te zijn. Wanneer dit gebeurt, blijven na het aanmelden voor instructies.

1. Nadat u zich heeft aangemeld in uw Azure-account, wordt een lijst van abonnementen die gekoppeld aan uw referenties zijn weergegeven in het dialoogvenster **Abonnementen beheren** . Als er meerdere abonnementen wordt weergegeven en u wilt werken met een specifieke subset van deze, kunt u die u wilt gebruiken kan desgewenst uitschakelen. Als u uw abonnementen hebt geselecteerd, klikt u op **sluiten**.

   ![][08]

1. Wanneer het dialoogvenster **distribueren naar Azure Web App Container** wordt weergegeven, wordt deze weergegeven Web App de recipiënten die u eerder hebt gemaakt. Als u de recipiënten niet hebt gemaakt, wordt de lijst niet leeg zijn.   

   ![][09]

1. Als u een Container Azure Web App voordat niet hebt gemaakt, of als u de toepassing uitgeeft aan een nieuwe container, gebruikt u de volgende stappen uit. Anders selecteert u een bestaande Web App Container en gaat u verder met stap 6 hieronder.

  1. Klik op**+**

        ![][10]

  1. Het dialoogvenster **Nieuwe Web App Container** verschijnt, die wordt gebruikt voor de volgende verschillende stappen.

        ![][11]

  1. Voer een **naam** voor uw Web App Container; Dit wordt de naam van de host-URL voor uw webtoepassing leaf in Azure vormen. Opmerking: De naam moet beschikbaar zijn en voldoen aan de vereisten voor naamgeving van Azure Web App.

  1. Selecteer in de vervolgkeuzelijst **Web Container** de geschikte software voor uw toepassing.

        U kunt op dit moment uit 8 Tomcat, Tomcat 7 of 9 Jetty. Een recente distributie van de geselecteerde software worden geleverd door Azure en wordt uitgevoerd op een recente distributie van JDK 8 door Oracle gemaakt en geleverd door Azure.

  1. Selecteer in de vervolgkeuzelijst **abonnement** het abonnement dat u wilt gebruiken voor deze installatie.

  1. Selecteer de resourcegroep die u wilt koppelen uw Web App in de **Resourcegroep** in de vervolgkeuzelijst.

        Opmerking: Azure resourcegroepen kunt u verwante bronnen samen zodat bijvoorbeeld ze samen kunnen worden verwijderd.

        U kunt een bestaande resourcegroep (als u een hebt) en overslaan naar stap g hieronder selecteren of gebruikt u de volgende deze stappen uit om een nieuwe resourcegroep maken:

      * Klik op **Nieuw...**

      * Het dialoogvenster **Nieuwe resourcegroep** wordt weergegeven:

            ![][12]

      * In de het tekstvak **naam** een naam voor uw nieuwe resourcegroep opgeven.

      * In de de **regio** vervolgkeuzelijst, selecteer de juiste Azure datacenters locatie voor de resourcegroep.

      * Klik op **OK**.

  1. In de vervolgkeuzelijst **App Service Plan** geeft een overzicht van de app serviceplannen die zijn gekoppeld aan de resourcegroep die u hebt geselecteerd.

        Opmerking: Een App Service Plan bevat informatie zoals de locatie van uw Web App, de prijzen laag en de grootte van de instantie berekenen. Een enkele App Service-indeling kan worden gebruikt voor meerdere webtoepassingen, dat is de reden waarom deze afzonderlijk van een specifieke implementatie van Web App onderhouden.

        U kunt een bestaande App Service Plan (als u een hebt) en overslaan naar stap h hieronder te selecteren of gebruikt u de volgende deze stappen uit om een nieuwe App Service plannen maken:

      * Klik op **Nieuw...**

      * Het dialoogvenster **Nieuwe App-Service Plan** wordt weergegeven:

            ![][13]

      * In de het tekstvak **naam** een naam voor uw nieuwe App Service plannen.

      * In de de **locatie** vervolgkeuzelijst, selecteer de juiste Azure datacenters locatie voor het plan.

      * In de **Prijzen laag** in de vervolgkeuzelijst selecteert u de juiste prijs voor het plan. Voor testdoeleinden kunt u **gratis**.

      * In de de **Grootte instantie** vervolgkeuzelijst, selecteer het formaat van de instantie voor het plan. Voor testdoeleinden kunt u **kleine**.

  1. Als u alle bovenstaande stappen hebt voltooid, ziet het dialoogvenster Nieuwe Web App Container in de volgende afbeelding:

        ![][14]

  1. Klik op **OK** om het maken van uw nieuwe Web App container.

        Wacht een paar seconden voor de lijst van de Web App containers worden vernieuwd en de gemaakte web app container moet nu worden geselecteerd in de lijst.

1. U bent nu gereed voor het voltooien van de eerste installatie van uw Web App met Azure; Klik op **OK** als u wilt uw Java-toepassing op de geselecteerde Web App container te implementeren.

    ![][15]

    Opmerking: Standaard, de toepassing wordt gedistribueerd als een submap van de application server. Als u wilt dat deze worden geïmplementeerd als de basistoepassing, schakel het selectievakje **distribueren naar hoofdmap** in voordat u op **OK**.

1. Vervolgens ziet u de weergave **Logboek met faxactiviteit Azure** , die aan de implementatiestatus van uw Web App.

    ![][16]

    Implementeren van uw Web App met Azure moet slechts een paar seconden duren. Wanneer uw toepassing klaar is, ziet u een koppeling met de naam **gepubliceerd** in de kolom **Status** . Wanneer u op de koppeling klikt, het gaat u naar de introductiepagina van de geïmplementeerde Web App of kunt u de stappen in de volgende sectie om te bladeren naar uw web app.

## <a name="browsing-to-your-web-app-on-azure"></a>Surfen op het Web App op Azure

Om de browser naar uw Web App op Azure, kunt u de verkennerweergave **Azure** .

Als de **Azure Verkenner** -weergave nog niet is geopend, kunt u openen door te klikken op Klik menu **Beeld** van IntelliJ, en vervolgens klikt u op **Hulpprogramma voor Windows**en klik op **Explorer Service**. Als u eerder niet hebt aangemeld, wordt u daarom gevraagd.

Wanneer de **Azure Verkenner** -weergave wordt weergegeven, gebruikt als volgt uw Web App stoppen: 

1. Vouw het knooppunt **Azure** .

1. Vouw het knooppunt **Web Apps** . 

1. Met de rechtermuisknop op de gewenste Web App.

1. Wanneer het snelmenu wordt weergegeven, klikt u op **openen in Browser**.

    ![][17]

## <a name="updating-your-web-app"></a>Bijwerken van uw Web App

Bijwerken van een bestaande met Azure Web App is een snel en eenvoudig proces en hebt u twee opties voor het bijwerken van:

* U kunt de implementatie van een bestaande Java Web App bijwerken.
* U kunt een Java-toepassing aan dezelfde Container met App meer publiceren.

In beide gevallen wordt het proces is identiek en duurt slechts enkele seconden:

1. Klik met de rechtermuisknop op de Java-toepassing die u wilt bijwerken of toevoegen aan een bestaande Web App Container in de Projectverkenner IntelliJ.

1. Wanneer het snelmenu wordt weergegeven, selecteert u **Azure** en vervolgens **publiceren als Azure Web App....**

1. Omdat u bent al ingelogd eerder, ziet u een lijst van uw bestaande Web App-containers. Selecteer het model dat u wilt publiceren of de Java-toepassing opnieuw te publiceren en klik op **OK**.

Een paar seconden later, het **Logboek voor faxactiviteit Azure** weer de bijgewerkte implementatie **gepubliceerd** en is het mogelijk om te controleren of uw bijgewerkte toepassing in een webbrowser.

## <a name="starting-or-stopping-an-existing-web-app"></a>Starten of stoppen van een bestaand Web App

Wilt starten of stoppen van een bestaande container Azure Web App (met inbegrip van alle gebruikte Java-toepassingen in deze), kunt u de verkennerweergave **Azure** .

Als de **Azure Verkenner** -weergave nog niet is geopend, kunt u openen door te klikken op Klik menu **Beeld** van IntelliJ, en vervolgens klikt u op **Hulpprogramma voor Windows**en klik op **Explorer Service**. Als u eerder niet hebt aangemeld, wordt u daarom gevraagd.

Wanneer de **Azure Verkenner** -weergave wordt weergegeven, gebruikt als volgt starten of stoppen van uw Web App: 

1. Vouw het knooppunt **Azure** .

1. Vouw het knooppunt **Web Apps** . 

1. Met de rechtermuisknop op de gewenste Web App.

1. Wanneer het snelmenu wordt weergegeven, klikt u op **starten** of **stoppen**. Let op: de menu-opties zijn context-aware zodat u kunt alleen een actieve web app stoppen of starten van een web app die momenteel niet wordt uitgevoerd.

    ![][18]

## <a name="next-steps"></a>Volgende stappen

Zie de volgende koppelingen voor meer informatie over de Azure Toolkits voor Java IDEs:

- [Azure Toolkit voor Eclips]
  - [Installatie van de Toolkit Azure voor Eclips]
  - [Maak een Hello World Web App voor Azure in Eclips]
  - [Wat is nieuw in de Azure Toolkit voor Eclips]
- [Azure Toolkit voor IntelliJ]
  - [Installeren van de Azure Toolkit voor IntelliJ]
  - *Maak een Hello World Web App voor Azure in IntelliJ (dit artikel)*
  - [Wat is nieuw in de Azure Toolkit voor IntelliJ]

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center].

Zie voor meer informatie over het maken van Azure Web Apps, [Web Apps-overzicht].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure Toolkit voor Eclips]: ../azure-toolkit-for-eclipse.md
[Azure Toolkit voor IntelliJ]: ../azure-toolkit-for-intellij.md
[Maak een Hello World Web App voor Azure in Eclips]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Installatie van de Toolkit Azure voor Eclips]: ../azure-toolkit-for-eclipse-installation.md
[Installeren van de Azure Toolkit voor IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Wat is nieuw in de Azure Toolkit voor Eclips]: ../azure-toolkit-for-eclipse-whats-new.md
[Wat is nieuw in de Azure Toolkit voor IntelliJ]: ../azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Web Apps-overzicht]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-intellij-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-intellij-create-hello-world-web-app/02-File-New-Project.png
[03a]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-Dialog.png
[03b]: ./media/app-service-web-intellij-create-hello-world-web-app/03-No-SDK-Specified.png
[04]: ./media/app-service-web-intellij-create-hello-world-web-app/04-New-Project-Dialog.png
[05]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Index-Page.png
[06]: ./media/app-service-web-intellij-create-hello-world-web-app/06-Azure-Publish-Context-Menu.png
[07]: ./media/app-service-web-intellij-create-hello-world-web-app/07-Azure-Log-In-Dialog.png
[08]: ./media/app-service-web-intellij-create-hello-world-web-app/08-Manage-Subscriptions.png
[09]: ./media/app-service-web-intellij-create-hello-world-web-app/09-App-Containers.png
[10]: ./media/app-service-web-intellij-create-hello-world-web-app/10-Add-App-Container.png
[11]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container.png
[12]: ./media/app-service-web-intellij-create-hello-world-web-app/12-New-Resource-Group.png
[13]: ./media/app-service-web-intellij-create-hello-world-web-app/13-New-App-Service-Plan.png
[14]: ./media/app-service-web-intellij-create-hello-world-web-app/14-New-App-Container.png
[15]: ./media/app-service-web-intellij-create-hello-world-web-app/15-Deploy-To-Azure.png
[16]: ./media/app-service-web-intellij-create-hello-world-web-app/16-Progress-Indicator.png
[17]: ./media/app-service-web-intellij-create-hello-world-web-app/17-Browse-Web-App.png
[18]: ./media/app-service-web-intellij-create-hello-world-web-app/18-Stop-Web-App.png
