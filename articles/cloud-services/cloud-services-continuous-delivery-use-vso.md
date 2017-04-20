<properties
    pageTitle="Doorlopende levering met Visual Studio Team Services in Azure | Microsoft Azure"
    description="Informatie over het configureren van Visual Studio Team Services teamprojecten om automatisch te bouwen en implementeren met de functie Web App in Azure App Service of cloud-services."
    services="cloud-services"
    documentationCenter=".net"
    authors="mlearned"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="mlearned"/>

# <a name="continuous-delivery-to-azure-using-visual-studio-team-services"></a>Doorlopende levering aan met behulp van Visual Studio Team Services Azure

U kunt uw teamprojecten Visual Studio Team Services automatisch bouwen en implementeren op Azure web apps of cloud services configureren.  (Zie voor meer informatie over het instellen van een continue bouwen en implementeren met behulp van een *op bedrijf* , Team Foundation Server systeem [Continue levering voor Cloud Services in Azure](cloud-services-dotnet-continuous-delivery.md).)

In deze zelfstudie wordt ervan uitgegaan dat u beschikt over Visual Studio 2013 en de Azure SDK is geïnstalleerd. Als u Visual Studio 2013 nog geen hebt, kiest u de koppeling **gratis aan de slag** bij [www.visualstudio.com](http://www.visualstudio.com)downloaden. De Azure SDK installeren vanaf [hier](http://go.microsoft.com/fwlink/?LinkId=239540).

> [AZURE.NOTE]U moet een account met Visual Studio Team Services voor het voltooien van deze zelfstudie: U kunt [gratis een Visual Studio Team Services-account openen](http://go.microsoft.com/fwlink/p/?LinkId=512979).

Ga als volgt te werk als u een cloud-service om automatisch te bouwen en implementeren op Azure met behulp van Visual Studio Team Services instellen.

## <a name="1-create-a-team-project"></a>1: Maak een teamproject

Volg de instructies [hier](http://go.microsoft.com/fwlink/?LinkId=512980) uw teamproject maken en koppelen aan een Visual Studio. In dit scenario wordt ervan uitgegaan dat u Team Foundation versie Control (TFVC) gebruikt als de bron oplossing. Als u wilt dat Git gebruikt voor versiebeheer, Zie [de Git versie van dit overzicht](http://go.microsoft.com/fwlink/p/?LinkId=397358).

## <a name="2-check-in-a-project-to-source-control"></a>2: in een project om het besturingselement te controleren

1. Open de oplossing die u wilt implementeren in Visual Studio of maak een nieuwe.
De stappen in dit scenario kunt u een web app of een cloud-service (Application Azure) implementeren.
Als u wilt een nieuwe oplossing te maken, maakt u een nieuw project voor Azure Cloud Service of een nieuw project voor ASP.NET MVC. Zorg ervoor dat het project is bedoeld voor .NET Framework 4 of 4.5, en als u een cloud service project maakt, een functie van ASP.NET MVC web en de rol van een werknemer toevoegen en kies Internet-toepassing voor de Webrol. Wanneer dat wordt gevraagd, kies **Internet-toepassing**.
Als u wilt voor het maken van een web app, kiest u de ASP.NET Web Application projectsjabloon en kies MVC. Zie [een ASP.NET-webtoepassing in Azure App-Service maken](../app-service-web/web-sites-dotnet-get-started.md).

    > [AZURE.NOTE] Visual Studio Team Services alleen CI implementaties van Visual Studio-toepassingen op dit moment wordt ondersteund. Website-projecten zijn buiten bereik.

1. Open het contextmenu voor de oplossing en kies de **Oplossing toevoegen aan het besturingselement**.

    ![][5]

1. Accepteren of wijzigen van de standaardinstellingen en klik op **OK** . Nadat het proces is voltooid, source control pictogrammen worden weergegeven in de **Solution Explorer**.

    ![][6]

1. Open het snelmenu voor de oplossing en kies **Inchecken**.

    ![][7]

1. In het gebied van de **Wachtende wijzigingen** van **Team Explorer**, typt u een opmerking voor het inchecken en klik op de knop **Inchecken** .

    ![][8]

    Opmerking de opties voor het opnemen of uitsluiten van specifieke wijzigingen wanneer u incheckt. Als de gewenste wijzigingen zijn uitgesloten, kies de koppeling **Alles opnemen** .

    ![][9]

## <a name="3-connect-the-project-to-azure"></a>3: de projecten verbinden met Azure

1. Nu u een project VS Team teamservices met sommige broncode erin hebt, bent u nu verbinding maken met uw teamproject Azure.  Selecteer in de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885)uw cloud of het web app of een nieuwe maken door te kiezen de **+** pictogram links onderaan en **Cloud Service** of **Web App** en vervolgens **Snel maken**te kiezen. Kies de koppeling **instellen met Visual Studio Team Services publiceren** .

    ![][10]

1. In het tekstvak typt u de naam van de Visual Studio Team Services-account en klik op de koppeling **Nu toestaan** in de wizard. Mogelijk wordt u gevraagd aan te melden.

    ![][11]

1. Kies de knop **accepteren** te machtigen Azure uw teamproject in VS Team Services configureren in het pop-upvenster **Verbinding aanvragen** .

    ![][12]

1. Als verificatie is gelukt, ziet u een vervolgkeuzelijst met een lijst van de projecten van Visual Studio Team teamservices. Kies de naam van de teamproject dat u in de vorige stappen hebt gemaakt en klik vervolgens op van de wizard is ingeschakeld.

    ![][13]

1. Nadat het project is gekoppeld, ziet u het aantal instructies voor het controleren van wijzigingen aan uw project van Visual Studio Team teamservices.  Visual Studio Team Services wordt op de volgende inchecken, bouwen en implementeren van uw project met Azure.  Probeer dit nu door te klikken op de koppeling **Inchecken vanuit Visual Studio** en vervolgens de koppeling **Visual Studio starten** (of gelijkwaardige **Visual Studio** knop onderaan het scherm portal).

    ![][14]

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: de tabel opnieuw te starten en uw project implementeren

1. Kies de link **Source Control Explorer** in Visual Studio **Team Explorer**.

    ![][15]

1. Ga naar het solution-bestand en open het.

    ![][16]

1. Een bestand te openen en wijzigen in de **Solution Explorer**. Wijzig bijvoorbeeld het bestand `_Layout.cshtml` onder de weergaven\\gedeelde map in een web MVC rol.

    ![][17]

1. Het logo voor de site bewerken en kies **Ctrl + S** om het bestand opslaan.

    ![][18]

1. Kies in de **Explorer Team**de koppeling **Wijzigingen in behandeling** .

    ![][19]

1. Voer een opmerking en kies vervolgens de knop **Inchecken** .

    ![][20]

1. Klik op **Home** om terug te keren naar de introductiepagina van de **Explorer Team** .

    ![][21]

1. Kies de koppeling **wordt gemaakt** om de opbouw in voortgang weer te geven.

    ![][22]

    **Team Explorer** ziet u een build geactiveerd is voor het inchecken.

    ![][23]

1. Dubbelklik op de naam van de build Bezig een gedetailleerd logboek weergeven als de build vordert.

    ![][24]

1. Hoewel de build in uitvoering, bekijk de definitie van de build die is gemaakt toen u met de wizard TFS gekoppeld aan Azure.  Open het snelmenu voor de definitie van de build en **Bouwen definitie bewerken**kiezen.

    ![][25]

    In de **Trigger** -tabblad ziet u dat de definitie van de build is standaard ingesteld op voortbouwen op elk selectievakje in.

    ![][26]

    In het tabblad **proces** kunt u zien dat de implementatieomgeving is ingesteld op de naam van de cloud of het web app. Als u met een web-apps werkt, worden de eigenschappen u ziet afwijken van de hier weergegeven.

    ![][27]

1. Waarden voor de eigenschappen opgeven als u wilt dat andere waarden dan de standaardwaarden. De eigenschappen voor Azure publiceren zijn in het gedeelte **implementeren** .

    In de volgende tabel ziet u de beschikbare eigenschappen in de sectie **implementatie** :

  	|Eigenschap|Standaardwaarde|
  	|---|---|
  	|Toestaan dat niet-vertrouwde certificaten|Als de waarde false, worden SSL-certificaten ondertekend door een basiscertificeringsinstantie.|
  	|Upgrade is toegestaan|Hiermee kunt de implementatie voor het bijwerken van een bestaande installatie in plaats van een nieuwe maken. Hiermee behoudt u het IP-adres.|
  	|Niet verwijderen|Als de waarde true is, niet een bestaande niet-verwante installatie overschreven (upgrade is toegestaan).|
  	|Pad naar de implementatie-instellingen|Het pad naar het bestand .pubxml voor een web app, ten opzichte van de hoofdmap van de repo. Voor cloud-services wordt genegeerd.|
  	|SharePoint-omgeving|Hetzelfde als de servicenaam van de.|
  	|Azure-omgeving|Het web app of cloud servicenaam.|

1. Als u meerdere configuraties (.cscfg-bestanden) gebruikt, kunt u de gewenste serviceconfiguratie in de instelling **bouwen, Geavanceerd, MSBuild argumenten** . Bijvoorbeeld voor het gebruik van ServiceConfiguration.Test.cscfg, MSBuild argumenten regeloptie instellen `/p:TargetProfile=Test`.

    ![][38]

    Op dat moment moet uw build worden voltooid.

    ![][28]

1. Als u dubbelklikt op de naam van de build, bevat Visual Studio een **Samenvatting maken**, met inbegrip van de testresultaten van bijbehorende unit testprojecten.

    ![][29]

1. In de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885)kunt u de bijbehorende implementatie weergeven op het tabblad **installaties** als de testomgeving is geselecteerd.

    ![][30]

1.  Blader naar de URL van uw site. Klik op de knop **Bladeren** in de werkbalk voor een web app. Kies voor een cloud-service, de URL in de sectie **Snel bekijken** van **de dashboardpagina waarin de Staging-omgeving voor een cloud-service** . Implementaties van continue integratie voor cloud services worden standaard gepubliceerd naar de Staging-omgeving. U kunt dit wijzigen door de eigenschap **Alternatieve Cloud-omgeving** voor de **productie**. Deze schermafbeelding ziet u waar de site-URL op dashboardpagina cloud-service is.

    ![][31]

    Een nieuw tabblad in de browser wordt geopend om de actieve site weer te geven.

    ![][32]

    Voor cloud-services als u andere wijzigingen in uw project aanbrengt en u meerdere implementaties wordt verzamelt u trigger meer maakt. De meest recente versie is gemarkeerd als actief.

    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: Implementeer een eerdere build

Deze stap geldt voor cloud services en is optioneel. In de klassieke Azure portal, kiest u een eerdere installatie en kies vervolgens de knop **opnieuw** moet worden teruggespoeld om een eerder inchecken.  Opmerking Dit zal een nieuwe build in TFS activeren en maak een nieuwe vermelding in de geschiedenis van uw implementatie.

![][34]

## <a name="6-change-the-production-deployment"></a>6: wijzigen van de productie-implementatie

Deze stap geldt alleen voor cloud-services, geen web apps. Wanneer u klaar bent, kunt u de Staging-omgeving naar de productieomgeving promoveren met de knop **wisselen** in de klassieke Azure portal. De onlangs geïmplementeerde Staging omgeving wordt gepromoveerd tot de productie en de vorige productie-omgeving, wordt een Staging-omgeving. De actieve implementatie voor de productie en de Staging-omgevingen kan afwijken, maar de geschiedenis van de implementatie van recente builds is hetzelfde, ongeacht de omgeving.

![][35]

## <a name="7-run-unit-tests"></a>7: UnitTests uitvoeren

Deze stap geldt alleen voor web apps, geen cloud services. Als u een kwaliteit gate op de implementatie, kunt u controles uitvoeren en als zij niet, kunt u de implementatie kunt stoppen.

1.  Voeg een testproject eenheid in Visual Studio.

    ![][39]

1.  Projectverwijzingen toevoegen aan het project dat u wilt testen.

    ![][40]

1.  Sommige controles toevoegen. Probeer een dummytest die altijd wordt doorgegeven aan de slag.

        ```
        using System;
        using Microsoft.VisualStudio.TestTools.UnitTesting;

        namespace UnitTestProject1
        {
            [TestClass]
            public class UnitTest1
            {
                [TestMethod]
                [ExpectedException(typeof(NotImplementedException))]
                public void TestMethod1()
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

1.  Vouw het knooppunt **Test** de build-definitie bewerken en kies het tabblad **proces** .

1.  De **build test mislukt mislukken** ingesteld op True. Dit betekent dat de implementatie zich niet voor tenzij de tests zijn gelukt.

    ![][41]

1.  Een nieuwe build wachtrij.

    ![][42]

    ![][43]

1. Terwijl de build nadert de voortgang controleren.

    ![][44]

    ![][45]

1. Wanneer de build is voltooid, controleert u de testresultaten.

    ![][46]

    ![][47]

1.  Maak een test die niet worden uitgevoerd. Een nieuwe test toevoegen door het kopiëren van de eerste, wijzigt u de naam en de regel van de code dat NotImplementedException is een verwachte uitzondering.

        ```
        [TestMethod]
        //[ExpectedException(typeof(NotImplementedException))]
        public void TestMethod2()
        {
            throw new NotImplementedException();
        }
        ```

1. Controleer in het wijzigen van een nieuwe build wachtrij.

    ![][48]

1. De testresultaten voor details over de fout weergeven.

    ![][49]

    ![][50]

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over testen in de Visual Studio Team Services eenheid [testen in uw versie van de eenheid](http://go.microsoft.com/fwlink/p/?LinkId=510474). Als u Git, Zie [delen van uw code in Git](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) en [continue implementatie naar Azure App-Service](../app-service-web/app-service-continuous-deployment.md).  Zie [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861)voor meer informatie over Visual Studio Team Services.

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso/tfs1.png
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png

[5]: ./media/cloud-services-continuous-delivery-use-vso/tfs5.png
[6]: ./media/cloud-services-continuous-delivery-use-vso/tfs6.png
[7]: ./media/cloud-services-continuous-delivery-use-vso/tfs7.png
[8]: ./media/cloud-services-continuous-delivery-use-vso/tfs8.png
[9]: ./media/cloud-services-continuous-delivery-use-vso/tfs9.png
[10]: ./media/cloud-services-continuous-delivery-use-vso/tfs10.png
[11]: ./media/cloud-services-continuous-delivery-use-vso/tfs11.png
[12]: ./media/cloud-services-continuous-delivery-use-vso/tfs12.png
[13]: ./media/cloud-services-continuous-delivery-use-vso/tfs13.png
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso/tfs18.png
[19]: ./media/cloud-services-continuous-delivery-use-vso/tfs19.png
[20]: ./media/cloud-services-continuous-delivery-use-vso/tfs20.png
[21]: ./media/cloud-services-continuous-delivery-use-vso/tfs21.png
[22]: ./media/cloud-services-continuous-delivery-use-vso/tfs22.png
[23]: ./media/cloud-services-continuous-delivery-use-vso/tfs23.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso/tfs27.png
[28]: ./media/cloud-services-continuous-delivery-use-vso/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso/tfs37.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso/AdvancedMSBuildSettings.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso/AddUnitTestProject.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso/AddProjectReferences.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso/EditBuildDefinitionForTest.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso/QueueNewBuild.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso/QueueBuildDialog.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso/BuildInTeamExplorer.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso/BuildRequestInfo.PNG
[46]: ./media/cloud-services-continuous-delivery-use-vso/BuildSucceeded.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso/TestResultsPassed.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso/CheckInChangeToMakeTestsFail.PNG
[49]: ./media/cloud-services-continuous-delivery-use-vso/TestsFailed.PNG
[50]: ./media/cloud-services-continuous-delivery-use-vso/TestsResultsFailed.PNG
