<properties 
    pageTitle="Maak een Hello World Web App voor Azure in Eclips" 
    description="In deze zelfstudie wordt beschreven hoe u met behulp van de Toolkit Azure voor Eclips een Hello World Web App voor Azure." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="create-a-hello-world-web-app-for-azure-in-eclipse"></a>Maak een Hello World Web App voor Azure in Eclips

Deze zelfstudie laat zien hoe maken en implementeren van een eenvoudige Hello World toepassing Azure als een Web App met behulp van de [Toolkit voor Eclips Azure]. Een eenvoudig voorbeeld van JSP weergegeven voor eenvoud, maar zeer vergelijkbare stappen voor een Java servlet, dienstig zou zijn wat betreft Azure-implementatie is.

Wanneer u deze zelfstudie hebt voltooid, zal uw toepassing uitzien in de volgende afbeelding wanneer u deze in een webbrowser bekijkt:

![][01]
 
## <a name="prerequisites"></a>Vereisten

* Een Java Developer Kit (JDK), v 1.7 of hoger.
* Voor ontwikkelaars van Java EE, IDE-Eclips Indigo of hoger. Dit kan worden gedownload van <http://www.eclipse.org/downloads/>.
* Een verdeling van een Java-webserver of een toepassing zoals Apache Tomcat of Jetty.
* Een Azure-abonnement, die kan worden verkregen uit <https://azure.microsoft.com/en-us/free/> of <http://azure.microsoft.com/pricing/purchase-options/>.
* De Azure Toolkit voor Eclips. Zie [de Azure Toolkit voor Eclips installeren]voor meer informatie.

## <a name="to-create-a-hello-world-application"></a>Voor het maken van een toepassing Hallo wereld

Eerst, we beginnen uit met het maken van een Java-project.

1. Eclips, start en klik op het menu **bestand**, klikt u op **Nieuw**en klik vervolgens op **Dynamische Web Project**. (Als er geen **Dynamische Web Project** weergegeven als een project beschikbaar na het klikken op **bestand** en **Nieuw**, vervolgens als volgt: klik op **bestand**, klikt u op **Nieuw**klikt u op **Project...**, vouw **Web**, klikt u op **Dynamic Web Project**en klik op **volgende**.)

1. Geef het project de **MyHelloWorld**voor de doeleinden van deze zelfstudie. Het scherm weergegeven met de volgende strekking:

    ![][02]

1. Klik op **Voltooien**.

1. Vouw in de weergave Project Explorer Eclips van **MyHelloWorld**. **Webinhoud**met de rechtermuisknop op **Nieuw**en klik vervolgens op **JSP-bestand**.

1. In het dialoogvenster **Nieuwe JSP-bestand** het bestand **index.jsp**een naam. De bovenliggende map bewaren als **MyHelloWorld/webinhoud**.

1. **JSP-sjabloon selecteren** in het dialoogvenster **Nieuwe JSP-bestand (html)**selecteren voor de doeleinden van deze zelfstudie en klik op **Voltooien**.

1. Als het bestand index.jsp wordt geopend in de Eclips, toevoegen in de tekst weer te geven dynamisch **Hello World!** binnen de bestaande `<body>` element. Uw bijgewerkte `<body>` inhoud moet vergelijkbaar zijn met het volgende voorbeeld:

    `<body><b><% out.println("Hello World!"); %></b></body>` 

1. Index.jsp opslaan.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Implementatie van de toepassing naar een Container Azure Web App

Er zijn verschillende manieren waarop u een Java webtoepassing Azure kunt implementeren. In deze zelfstudie wordt beschreven in een van de eenvoudigste: de toepassing wordt geïmplementeerd naar een Container Azure Web App - geen speciale projecttype en geen extra hulpmiddelen nodig zijn. De JDK en de container webserversoftware krijgt u door Azure, dus het is niet nodig voor het uploaden van uw eigen; u hoeft uw Java Web App is. Hierdoor duurt het publicatieproces voor uw toepassing geen minuten seconden.

1. Klik met de rechtermuisknop op **MyHelloWorld**in de Projectverkenner van de Eclips.

1. In het contextmenu **Azure**selecteren en klik op **publiceren als Azure Web App....**

    ![][03]
   
    Ook terwijl uw web application-project is geselecteerd in de Project Explorer, kunt u klikt u op de knop van de vervolgkeuzelijst **publiceren** op de werkbalk en selecteer **publiceren als Azure Web App** vanaf:
   
    ![][14]
   
1. Als u niet reeds in Azure uit Eclips gesloten, wordt u gevraagd uw Azure rekening ondertekenen:

    ![][04]
   
    Opmerking: Als er meerdere accounts voor Azure, enkele van de vragen tijdens het aanmelden proces kan worden weergegeven meer dan eens zelfs als ze lijken hetzelfde te zijn. Wanneer dit gebeurt, blijven na het aanmelden voor instructies.
1. Nadat u zich heeft aangemeld in uw Azure-account, wordt een lijst van abonnementen die gekoppeld aan uw referenties zijn weergegeven in het dialoogvenster **Abonnementen beheren** . Als er meerdere abonnementen wordt weergegeven en u wilt werken met een specifieke subset van deze, kunt u die u wilt gebruiken kan desgewenst uitschakelen. Als u uw abonnementen hebt geselecteerd, klikt u op **sluiten**.

    ![][05]
   
1. Wanneer het dialoogvenster **distribueren naar Azure Web App Container** wordt weergegeven, wordt deze weergegeven Web App de recipiënten die u eerder hebt gemaakt. Als u de recipiënten niet hebt gemaakt, wordt de lijst niet leeg zijn.

    ![][06]
   
1. Als u een Container Azure Web App voordat niet hebt gemaakt, of als u de toepassing uitgeeft aan een nieuwe container, gebruikt u de volgende stappen uit. Anders selecteert u een bestaande Web App Container en gaat u verder met stap 7 hieronder.

    1. Klik op **Nieuw...**

    1. Het dialoogvenster **Nieuwe Web App Container** wordt weergegeven:

        ![][07]

    1. Voer een **naam** voor uw Web App Container; Dit wordt de naam van de host-URL voor uw webtoepassing leaf in Azure vormen. Opmerking: De naam moet beschikbaar zijn en voldoen aan de vereisten voor naamgeving van Azure Web App.

    1. Selecteer in de vervolgkeuzelijst **Web Container** de geschikte software voor uw toepassing.

        U kunt op dit moment uit 8 Tomcat, Tomcat 7 of 9 Jetty. Een recente distributie van de geselecteerde software worden geleverd door Azure en wordt uitgevoerd op een recente distributie van JDK 8 door Oracle gemaakt en geleverd door Azure.

    1. Selecteer in de vervolgkeuzelijst **abonnement** het abonnement dat u wilt gebruiken voor deze installatie.

    1. Selecteer de resourcegroep die u wilt koppelen uw Web App in de **Resourcegroep** in de vervolgkeuzelijst.

        Opmerking: Azure resourcegroepen kunt u verwante bronnen samen zodat bijvoorbeeld ze samen kunnen worden verwijderd.

        U kunt een bestaande resourcegroep (als u een hebt) en overslaan naar stap g hieronder selecteren of gebruikt u de volgende deze stappen uit om een nieuwe resourcegroep maken:

        * Klik op **Nieuw...**

        * Het dialoogvenster **Nieuwe resourcegroep** wordt weergegeven:

            ![][08]

        * In de het tekstvak **naam** een naam voor uw nieuwe resourcegroep opgeven.

        * In de de **regio** vervolgkeuzelijst, selecteer de juiste Azure datacenters locatie voor de resourcegroep.

        * Klik op **OK**.

    1. In de vervolgkeuzelijst **App Service Plan** geeft een overzicht van de app serviceplannen die zijn gekoppeld aan de resourcegroep die u hebt geselecteerd.

        Opmerking: Een App Service Plan bevat informatie zoals de locatie van uw Web App, de prijzen laag en de grootte van de instantie berekenen. Een enkele App Service-indeling kan worden gebruikt voor meerdere webtoepassingen, dat is de reden waarom deze afzonderlijk van een specifieke implementatie van Web App onderhouden.

        U kunt een bestaande App Service Plan (als u een hebt) en overslaan naar stap h hieronder te selecteren of gebruikt u de volgende deze stappen uit om een nieuwe App Service plannen maken:

        * Klik op **Nieuw...**

        * Het dialoogvenster **Nieuwe App-Service Plan** wordt weergegeven:

            ![][09]

        * In de het tekstvak **naam** een naam voor uw nieuwe App Service plannen.

        * In de de **locatie** vervolgkeuzelijst, selecteer de juiste Azure datacenters locatie voor het plan.

        * In de **Prijzen laag** in de vervolgkeuzelijst selecteert u de juiste prijs voor het plan. Voor testdoeleinden kunt u **gratis**.

        * In de de **Grootte instantie** vervolgkeuzelijst, selecteer het formaat van de instantie voor het plan. Voor testdoeleinden kunt u **kleine**.

    1. Als u alle bovenstaande stappen hebt voltooid, ziet het dialoogvenster Nieuwe Web App Container in de volgende afbeelding:

        ![][10]

    1. Klik op **OK** om het maken van uw nieuwe Web App container.

        Wacht een paar seconden voor de lijst van de Web App containers worden vernieuwd en de gemaakte web app container moet nu worden geselecteerd in de lijst.

1. U bent nu gereed om de eerste implementatie van uw Web App met Azure te voltooien:

    ![][11]

    Klik op **OK** als u wilt uw Java-toepassing op de geselecteerde Web App container te implementeren.

    Opmerking: Standaard, de toepassing wordt gedistribueerd als een submap van de application server. Als u wilt dat deze worden geïmplementeerd als de basistoepassing, schakel het selectievakje **distribueren naar hoofdmap** in voordat u op **OK**.

1. Vervolgens ziet u de weergave **Logboek met faxactiviteit Azure** , die aan de implementatiestatus van uw Web App.

    ![][12]

    Implementeren van uw Web App met Azure moet slechts een paar seconden duren. Wanneer uw toepassing klaar is, ziet u een koppeling met de naam **gepubliceerd** in de kolom **Status** . Wanneer u op de koppeling klikt, gaat het u naar de introductiepagina van de geïmplementeerde Web App.

## <a name="updating-your-web-app"></a>Bijwerken van uw Web App

Bijwerken van een bestaande met Azure Web App is een snel en eenvoudig proces en hebt u twee opties voor het bijwerken van:

* U kunt de implementatie van een bestaande Java Web App bijwerken.
* U kunt een Java-toepassing aan dezelfde Container met App meer publiceren.

In beide gevallen wordt het proces is identiek en duurt slechts enkele seconden:

1. Klik met de rechtermuisknop op de Java-toepassing die u wilt bijwerken of toevoegen aan een bestaande Web App Container in de Projectverkenner Eclips.

2. Wanneer het snelmenu wordt weergegeven, selecteert u **Azure** en vervolgens **publiceren als Azure Web App....**

3. Omdat u bent al ingelogd eerder, ziet u een lijst van uw bestaande Web App-containers. Selecteer het model dat u wilt publiceren of de Java-toepassing opnieuw te publiceren en klik op **OK**.

Een paar seconden later, het **Logboek voor faxactiviteit Azure** weer de bijgewerkte implementatie **gepubliceerd** en is het mogelijk om te controleren of uw bijgewerkte toepassing in een webbrowser.

## <a name="stopping-an-existing-web-app"></a>Een bestaand Web App stoppen

Een bestaande Azure Web App container, (met inbegrip van alle gebruikte Java-toepassingen in deze), kunt u de verkennerweergave **Azure** stoppen.

Als de weergave van de **Verkenner Azure** nog niet is geopend, kunt u openen door te klikken op Klik menu **venster** in Eclips, en vervolgens klikt u op **Weergave weergeven**, vervolgens **andere...**en vervolgens **Azure**en klik op **Explorer Azure**. Als u eerder niet hebt aangemeld, wordt u daarom gevraagd.

Wanneer de **Azure Verkenner** -weergave wordt weergegeven, gebruikt als volgt uw Web App stoppen: 

1. Vouw het knooppunt **Azure** .

1. Vouw het knooppunt **Web Apps** . 

1. Met de rechtermuisknop op de gewenste Web App.

1. Wanneer het snelmenu wordt weergegeven, klikt u op **stoppen**.

    ![][13]

## <a name="next-steps"></a>Volgende stappen

Zie de volgende koppelingen voor meer informatie:

* [Java Developer Center](/develop/java/).
* [Web Apps-overzicht](app-service-web-overview.md)

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkID=699529
[Installatie van de Toolkit Azure voor Eclips]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[01]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/01-Web-Page.png
[02]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/02-Dynamic-Web-Project.png
[03]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/03-Context-Menu.png
[04]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/04-Log-In-Dialog.png
[05]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/05-Manage-Subscriptions-Dialog.png
[06]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/06-Deploy-To-Azure-Web-Container.png
[07]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/07-New-Web-App-Container-Dialog.png
[08]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/08-New-Resource-Group-Dialog.png
[09]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/09-New-Service-Plan-Dialog.png
[10]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/11-Completed-Deploy-Dialog.png
[12]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/12-Activity-Log-View.png
[13]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/13-Azure-Explorer-Web-App.png
[14]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/publishDropdownButton.png