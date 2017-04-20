<properties
    pageTitle="Maak een universele Windows Platform (UWP) die wordt gebruikt op mobiele Apps | Microsoft Azure"
    description="Volg deze zelfstudie aan de slag met Azure mobiele app backends voor Universal Windows Platform (UWP) app ontwikkeling in C#, Visual Basic of JavaScript gebruiken."
    services="app-service\mobile"
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-a-windows-app"></a>Maak een Windows-app

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Overzicht

In deze zelfstudie wordt beschreven hoe u een cloud-gebaseerde back-end-service toevoegen aan een app Universal Windows Platform (UWP). Zie [Wat zijn mobiele Apps](app-service-mobile-value-prop.md)voor meer informatie. Schermvastleggingen vanuit voltooide app zijn:

![Desktop app voltooid](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)   
Op het bureaublad uitgevoerd. 

![Voltooide telefoon app](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)  
Uitgevoerd op een telefoon

Voltooien van deze zelfstudie is een vereiste voor alle andere mobiele App-cursussen voor UWP apps. 

##<a name="prerequisites"></a>Vereisten

Deze zelfstudie hebt u het volgende nodig:

* Een actieve account Azure. Als u geen account hebt, kunt u zich aanmelden voor een evaluatieversie Azure en maximaal 10 gratis mobiele apps die u ook na het einde van de proefperiode kunt blijven gebruiken. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/).

* [Visual Studio Community 2015] of een latere versie.

>[AZURE.NOTE] Als u aan de slag met Azure App-Service wilt voordat u zich voor een account met Azure aanmeldt, gaat u naar de [App-Service probeert](https://tryappservice.azure.com/?appServiceName=mobile). Daar kunt u direct maken een tijdelijk starter mobiele app in App-Service — geen creditcard vereist en geen verplichtingen.

##<a name="create-a-new-azure-mobile-app-backend"></a>Maak een nieuwe Azure Mobile App backend.

Volg deze stappen uit om een nieuwe Mobile App backend.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

U hebt nu een backend Azure Mobile App die kan worden gebruikt door uw mobiele-clienttoepassingen ingericht. Vervolgens wordt u een serverproject voor een eenvoudige "todo lijst" downloaden backend en publiceren naar Azure.

## <a name="configure-the-server-project"></a>De serverproject configureren

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

##<a name="download-and-run-the-client-project"></a>Download en installeer de client-project

Wanneer u uw backend Mobile App hebt geconfigureerd, kunt u een nieuwe clienttoepassing maken of wijzigen van een bestaande app verbinding met Azure. In deze sectie kunt downloaden u een project UWP app sjabloon die is aangepast voor de verbinding met uw backend Mobile App.

1. Terug in de **Quick start** -blade voor uw mobiele App backend, klikt u op **nieuwe toepassing maken** > **downloaden**en vervolgens uittreksel gecomprimeerde projectbestanden naar de lokale computer.

    ![Project voor Windows-quickstart downloaden](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-app-windows-quickstart.png)

3. (Optioneel) De UWP-app-project toevoegen aan dezelfde oplossing als het serverproject. Dit gemakkelijker fouten opsporen en test de app en de back-end in dezelfde Visual Studio-oplossing als u ervoor kiest om dit te doen. Als een project UWP app aan de oplossing toevoegen, moet u Visual Studio 2015 of hoger gebruiken.

4. De UWP app als het project is gestart, druk op F5 om te implementeren en uitvoeren van de app.

5. In de app, zinvolle tekst, zoals *volledige zelfstudie*, typ in het tekstvak **invoegen van een TodoItem** en klik vervolgens op **Opslaan**.

    ![Quickstart-volledige Windows-bureaublad](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    Dit verzendt een POST-aanvraag naar de nieuwe mobiele app backend die wordt gehost in Azure.

6. (Optioneel) De toepassing stoppen en opnieuw op een ander apparaat of mobiele emulator.

    ![Windows quickstart volledige telefoonnummer](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)

    U ziet dat gegevens die worden opgeslagen in de vorige stap van Azure is geladen nadat de UWP-toepassing wordt gestart. 

##<a name="next-steps"></a>Volgende stappen

* [Verificatie toevoegen aan uw app](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Informatie over het verifiëren van gebruikers van uw toepassing met een id-provider.

* [Push-meldingen toevoegen aan uw app](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Informatie over het toevoegen van push-meldingen en support voor uw app en configureer uw mobiele App backend Azure melding Hubs gebruikt om push-meldingen te verzenden.

* [Off line synchronisatie van uw app inschakelen](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Informatie over het off line ondersteuning van uw app in de backend voor een mobiele App toevoegen. Off line synchronisatie kan eindgebruikers communiceren met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;zelfs als er geen netwerkverbinding is.

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[Mobile App SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2015]: https://go.microsoft.com/fwLink/p/?LinkID=534203
