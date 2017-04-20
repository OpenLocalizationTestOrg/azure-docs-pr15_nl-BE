<properties 
   pageTitle="Mobiele Services toe te voegen met behulp van Connected Services in Visual Studio | Microsoft Azure"
   description="Mobile-Services toevoegen via het dialoogvenster Visual Studio verbonden Services toevoegen"
   services="visual-studio-online"
   documentationCenter="na"
   authors="mlhoop"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="mobile"
   ms.date="12/16/2015"
   ms.author="mlearned" />

# <a name="adding-mobile-services-by-using-visual-studio-connected-services"></a>Mobiele Services toe te voegen met behulp van Visual Studio Connected Services

Met Visual Studio 2015, kunt u verbinding met Azure Mobile Services met behulp van het dialoogvenster **Verbonden Service toevoegen** . Kunt u in een C#-clienttoepassing JavaScript app of cross-platform Cordova app. Als u verbinding hebt gemaakt, kunt u maken en toegang tot gegevens, aangepaste API's en geplande taken maken of toevoegen van ondersteuning voor push-meldingen.  De gekoppelde bewerking alle passende verwijzingen en verbinding code toegevoegd. U kunt ook gebruikmaken van de ingebouwde ondersteuning voor verificatie met een groot aantal populaire identiteit regelingen, zoals AD Azure, Facebook, Twitter en Microsoft-Accounts.

## <a name="supported-project-types"></a>Ondersteunde projecttypen

>[AZURE.NOTE] In Visual Studio 2015, wordt Azure Mobile Services toe te voegen aan een universele Windows (Windows 10) projecten met behulp van het dialoogvenster verbonden Services toevoegen niet ondersteund. Azure Mobile Services kunt u door het installeren van de juiste pakketten met de NuGet Package Manager voor uw project toevoegen.

Het dialoogvenster verbonden Services kunt u verbinding maken met Azure Mobile Services in de volgende projecttypen.

- .NET Windows 8.1 Store, telefoon en Universal App projecten

- JavaScript Windows 8.1 Store, telefoon en Universal App projecten

- Projecten die zijn gemaakt met Visual Studio Tools voor Apache Cordova


## <a name="connect-to-azure-mobile-services-using-the-add-connected-services-dialog"></a>Verbinding maken met Azure Mobile Services met behulp van het dialoogvenster verbonden Services toevoegen

1. Zorg ervoor dat u een Azure-account hebt. Als u niet een Azure-account hebt, kunt u zich aanmelden voor een [gratis proefperiode](http://go.microsoft.com/fwlink/?LinkId=518146).

1. Open het dialoogvenster **Verbonden Services toevoegen** .
 - Open het project in Visual Studio voor .NET-toepassingen, opent u het contextmenu voor het knooppunt met **verwijzingen** in de Solution Explorer en kies vervolgens **Verbonden Service toevoegen**
 
        ![Connecting to Azure Mobile Service](./media/vs-azure-tools-connected-services-add-mobile-services/IC797635.png)

 - Apache Cordova app projecten, het project in Visual Studio openen, opent u het contextmenu voor het projectknooppunt in de Solution Explorer en kies vervolgens **Verbonden Service toevoegen**.

1. **Azure Mobile Services**kiezen in het dialoogvenster **Verbonden Service toevoegen** en kies vervolgens de knop **configureren** . U wordt mogelijk gevraagd om in te loggen Azure als u nog niet hebt gedaan.

    ![Toevoegen van een Azure mobiele Service](./media/vs-azure-tools-connected-services-add-mobile-services/IC797636.png)

1. Kies in het dialoogvenster **Azure Mobile Services** een bestaande mobiele service als er een. Als u een nieuwe Azure mobiele service maken, volg hiervoor de volgende procedure. Ga anders verder met de volgende stap.

    Voor het maken van een nieuwe mobile service-account:
    1. Kies de **Service maken **koppeling onder aan het dialoogvenster.
        ![Nieuwe mobiele verbonden service toevoegen](./media/vs-azure-tools-connected-services-add-mobile-services/IC797637.png)




    2. **Mobile-Service maakt** in het dialoogvenster kunt u een mobiele JavaScript back-end-service of een mobiele .NET back-end-service uit de vervolgkeuzelijst **Runtime** . 
  
        ![Maken van een mobiele service](./media/vs-azure-tools-connected-services-add-mobile-services/IC797638.png)

        Een JavaScript back-end-service is eenvoudig en krachtig. Als u een mobiele JavaScript back-end-service maakt, wordt de server-side JavaScript-code wordt opgeslagen in de cloud, maar u kunt serverscripts bewerken met behulp van Server Explorer of de Azure management portal. 

        Een mobiele .NET back-end-service biedt u de volledige kracht en flexibiliteit van de Web-API en entiteit Framework. Als u een mobiele .NET back-end-service maakt, wordt een project voor u gemaakt en toegevoegd aan uw oplossing. 

    1. Kies de **regio** waar u de mobiele service en voer vervolgens een gebruikersnaam en wachtwoord voor de server.
 
    1. Nadat u de vereiste gegevens hebt ingevoerd, klik op **maken** om de mobiele service te maken.
    2. De nieuwe mobiele service moet worden weergegeven in de lijst in het dialoogvenster **Azure mobiele Services** . De nieuwe mobiele service kiezen in de lijst en kies vervolgens de knop **toevoegen** aan de service toevoegen aan uw project.
    

1. Controleer de ophalen begonnen pagina die wordt weergegeven en ontdek hoe het project is gewijzigd. Een pagina aan de slag wordt weergegeven in uw browser wanneer u een service verbonden toevoegt. U kunt de codevoorbeelden en mogelijke volgende stappen controleren of Ga naar de pagina Wat is er gebeurd om te zien welke verwijzingen zijn toegevoegd aan het project en hoe de code en configuratie-bestanden zijn gewijzigd.

1. Start met behulp van de codevoorbeelden als leidraad voor het schrijven van code om toegang te krijgen tot uw mobiele service!

## <a name="how-your-project-is-modified"></a>Hoe het project is gewijzigd

Hoe het project in Visual Studio wordt gewijzigd, is afhankelijk van het projecttype. Voor C# client-toepassingen, Zie [Wat is er gebeurd – C# projecten](http://go.microsoft.com/fwlink/p/?LinkId=513119). Zie voor JavaScript-clienttoepassingen, [Wat is er gebeurd – JavaScript-projecten](http://go.microsoft.com/fwlink/p/?LinkId=513120). Zie [Wat is er gebeurd – Cordova projecten](http://go.microsoft.com/fwlink/p/?LinkId=513116)voor Cordova-apps.


##<a name="next-steps"></a>Volgende stappen

Vragen stellen en Help-informatie opvragen: 

 - [MSDN-Forum: Azure mobiele Services](https://social.msdn.microsoft.com/forums/azure/home?forum=azuremobile)

 - [Azure mobiele Services op Microsoft Azure-teamblog](https://azure.microsoft.com/blog/topics/mobile/)

 - [Azure mobiele Services op azure.microsoft.com](https://azure.microsoft.com/services/mobile-services/)

 - [Azure mobiele Services documentatie op azure.microsoft.com](https://azure.microsoft.com/documentation/services/mobile-services/)



