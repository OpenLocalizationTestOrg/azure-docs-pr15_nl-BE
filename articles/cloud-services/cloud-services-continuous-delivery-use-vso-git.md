<properties
    pageTitle="Doorlopende levering met Git en Visual Studio Team Services in Azure | Microsoft Azure"
    description="Informatie over het configureren van Visual Studio Team Services teamprojecten voor het gebruik van Git automatisch bouwen en implementeren met de functie Web App in Azure App Service of cloud-services."
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

# <a name="continuous-delivery-to-azure-using-visual-studio-team-services-and-git"></a>Doorlopende levering aan met behulp van Visual Studio Team Services en Git Azure

U kunt teamprojecten Visual Studio Team Services een Git repository als host voor de broncode en automatisch bouwen en implementeren op Azure web apps of cloud services wanneer u een commit push naar de opslagplaats.

U moet Visual Studio 2013 en de Azure SDK is geïnstalleerd. Als u Visual Studio 2013 nog geen hebt, kiest u de koppeling **gratis aan de slag** bij [www.visualstudio.com](http://www.visualstudio.com)downloaden. De Azure SDK installeren vanaf [hier](http://go.microsoft.com/fwlink/?LinkId=239540).


> [AZURE.NOTE]U moet een account met Visual Studio Team Services voor het voltooien van deze zelfstudie: U kunt [gratis een Visual Studio Team Services-account openen](http://go.microsoft.com/fwlink/p/?LinkId=512979).

Ga als volgt te werk als u een cloud-service om automatisch te bouwen en implementeren op Azure met behulp van Visual Studio Team Services instellen.

## <a name="1-create-a-git-repository"></a>1: Maak een Git repository

1. Als u niet al een Visual Studio Team Services-account hebt, kunt u een [hier](http://go.microsoft.com/fwlink/?LinkId=397665). Wanneer u uw teamproject maakt, kiest u Git als uw bronbeheersysteem. Volg de instructies in Visual Studio verbinding met uw teamproject.

2. Kies de koppeling **deze opslagplaats klonen** in **Explorer Team**.

    ![][3]

3. Geef de locatie van de lokale kopie en kies vervolgens de knop **kopiëren** .

## <a name="2-create-a-project-and-commit-it-to-the-repository"></a>2: een project maken en deze aan de opslagplaats toewijst

1. Kies in **Explorer Team**in de sectie **oplossingen voor** de **nieuwe** koppeling voor het maken van een nieuw project in de lokale bibliotheek.

    ![][4]

2. De stappen in dit scenario kunt u een web app of een cloud-service (Application Azure) implementeren. Maak een nieuw project voor Azure Cloud Service of een nieuw project voor ASP.NET MVC. Zorg ervoor dat het project is bedoeld voor het .NET Framework 4 of hoger. Als u een cloud service project maakt, een functie van ASP.NET MVC web en de rol van een werknemer toevoegen.
Als u wilt voor het maken van een web app, kiest u de **ASP.NET Web Application** projectsjabloon en kies **MVC**. Zie [een ASP.NET-webtoepassing in Azure App-Service maken](../app-service-web/web-sites-dotnet-get-started.md) voor meer informatie.

3. Open het snelmenu voor de oplossing en kies **vastleggen**.

    ![][7]

4. Als dit de eerste keer dat u de Git in Visual Studio Team Services hebt gebruikt, moet u bepaalde informatie om uzelf te identificeren in Git. Voer uw gebruikersnaam en e-mailadres in het gebied **Wachtende wijzigingen** van **De Explorer Team**. Een opmerking invoeren voor het vastleggen en kies vervolgens de knop **vastleggen** .

    ![][8]

5. Opmerking de opties voor het opnemen of uitsluiten van specifieke wijzigingen wanneer u incheckt. Als u de gewenste wijzigingen zijn uitgesloten, **Inclusief alle**kiezen.

6. U hebt nu de wijzigingen doorgevoerd in het lokale exemplaar van de bibliotheek. Vervolgens deze wijzigingen met de server te synchroniseren kiest de **Sync** -link.

## <a name="3-connect-the-project-to-azure"></a>3: de projecten verbinden met Azure

1. Nu u een Git repository in Visual Studio Team Services met een bepaalde broncode in het hebt, bent u nu verbinding maken de git repository met Azure.  Selecteer in de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885)uw cloud of het web app of een nieuwe maken door te kiezen de + pictogram links onderaan en **Cloud Service** of **Web App** en vervolgens **Snel maken**te kiezen.

    ![][9]

3. Kies de koppeling **instellen met Visual Studio Team Services publiceren** voor cloud-services. Kies voor web apps, de koppeling met **de implementatie van het besturingselement instellen** .

    ![][10]

2. In de wizard typt u de naam van de Visual Studio Team Services-account in het tekstvak en kies de koppeling **Nu toestemming geven** . Mogelijk wordt u gevraagd aan te melden.

    ![][11]

3. Kies in het pop-upvenster **Verbinding aanvragen** **accepteren** Azure voor het configureren van uw teamproject in Visual Studio Team Services toe te staan.

    ![][12]

4. Als verificatie is gelukt, ziet u een vervolgkeuzelijst met de Visual Studio Team Services teamprojecten.  Selecteer de naam van de teamproject dat u in de vorige stappen hebt gemaakt en kies de wizardknop vinkje.

    ![][13]

    De volgende keer dat u een commit push naar uw bibliotheek, zal Visual Studio Team Services bouwen en implementeren van uw project met Azure.

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: de tabel opnieuw te starten en uw project implementeren

1. In Visual Studio een bestand te openen en wijzigen. Wijzig bijvoorbeeld het bestand `_Layout.cshtml` onder de weergaven\\gedeelde map in een web MVC rol.

    ![][17]

2. Bewerk de tekst van de voettekst voor de site en sla het bestand.

    ![][18]

3. Opent het snelmenu voor het knooppunt oplossing, projectknooppunt of het bestand dat u hebt gewijzigd in de **Solution Explorer**en kies vervolgens **doorvoeren**.

4. Typ in een opmerking en kies **vastleggen**.

    ![][20]

5. Kies de **Sync** -koppeling.

    ![][38]

6. Kies de **Push** -koppeling naar de commit push naar de opslagplaats in Visual Studio Team Services. (Kun je de knop **synchronisatie** doorvoeracties voor het kopiëren naar de opslagplaats. Het verschil is **synchronisatie** uit de bibliotheek ook de meest recente wijzigingen ophaalt.)

    ![][39]

7. Klik op **Home** om terug te keren naar de introductiepagina van de **Explorer Team** .

    ![][21]

8. Kies **bouwt** de opbouw in voortgang weergeven.

    ![][22]

    **Team Explorer** ziet u een build geactiveerd is voor het inchecken.

    ![][23]

9. Om u een gedetailleerd logboek zo de build vordert, dubbelklikt u op de naam van de build uitgevoerd.

10. Hoewel de build in uitvoering, bekijk de definitie van de build die is gemaakt toen u de wizard gebruikt om een koppeling naar Azure.  Open het snelmenu voor de definitie van de build en **Bouwen definitie bewerken**kiezen.

    ![][25]

11. In de **Trigger** -tabblad ziet u dat de definitie van de build is standaard ingesteld op voortbouwen op elke inchecken. (Voor een service cloud Visual Studio Team Services bouwt en implementeert u de master branch automatisch naar de staging-omgeving. U hebt nog een handmatige stap voor de implementatie van de live site doen. Voor een web app dat geen staging-omgeving, wordt de master branch gedistribueerd rechtstreeks naar de actieve site.

    ![][26]

1. In het tabblad **proces** kunt u zien dat de implementatieomgeving is ingesteld op de naam van de cloud of het web app.

    ![][27]

1. Waarden voor de eigenschappen opgeven als u wilt dat andere waarden dan de standaardwaarden. De eigenschappen voor Azure publiceren in de sectie **implementatie** zijn en moet u mogelijk ook MSBuild parameters instellen. Bijvoorbeeld in een wolk service project, geeft u een configuratie van de service dan "Wolk", de MSbuild parameters instellen op `/p:TargetProfile=[YourProfile]` *[YourProfile]* overeenkomt met een service-configuratiebestand met een naam als ServiceConfiguration. *YourProfile*.cscfg.

    In de volgende tabel ziet u de beschikbare eigenschappen in de sectie **implementatie** :

  	|Eigenschap|Standaardwaarde|
  	|---|---|
  	|Toestaan dat niet-vertrouwde certificaten|Als de waarde false, worden SSL-certificaten ondertekend door een basiscertificeringsinstantie.|
  	|Upgrade is toegestaan|Hiermee kunt de implementatie voor het bijwerken van een bestaande installatie in plaats van een nieuwe maken. Hiermee behoudt u het IP-adres.|
  	|Niet verwijderen|Als de waarde true is, niet een bestaande niet-verwante installatie overschreven (upgrade is toegestaan).|
  	|Pad naar de implementatie-instellingen|Het pad naar het bestand .pubxml voor een web app, ten opzichte van de hoofdmap van de repo. Voor cloud-services wordt genegeerd.|
  	|SharePoint-omgeving|Hetzelfde als de servicenaam van de.|
  	|Azure-omgeving|Het web app of cloud servicenaam.|

1. Op dat moment moet uw build worden voltooid.

    ![][28]

1. Als u dubbelklikt op de naam van de build, bevat Visual Studio een **Samenvatting maken**, met inbegrip van de testresultaten van bijbehorende unit testprojecten.

    ![][29]

1. In de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885)kunt u de bijbehorende implementatie weergeven op het tabblad **installaties** als de testomgeving is geselecteerd.

    ![][30]

1.  Blader naar de URL van uw site. Kies de knop **Bladeren** in de portal voor een web app. Kies voor een cloud-service, de URL in de sectie **Snel bekijken** van **de dashboardpagina waarin de Staging-omgeving** .

    Implementaties van continue integratie voor cloud services worden standaard gepubliceerd naar de Staging-omgeving. U kunt dit wijzigen door de eigenschap **Alternatieve Cloud-omgeving** voor de **productie**. Hier is waar de site-URL op van de service cloud dashboardpagina.

    ![][31]

    Een nieuw tabblad in de browser wordt geopend om de actieve site weer te geven.

    ![][32]

1.  Als u andere wijzigingen in uw project aanbrengt, trigger u meer maakt en u meerdere implementaties wordt verzameld. De meest recente versie is gemarkeerd als actief.

    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: Implementeer een eerdere build

Deze stap is optioneel. In de klassieke Azure portal, kies een eerdere implementatie en kies **opnieuw** moet worden teruggespoeld om een eerder inchecken. Opmerking Dit zal een nieuwe build in TFS activeren en maak een nieuwe vermelding in de geschiedenis van uw implementatie.

![][34]

## <a name="6-change-the-production-deployment"></a>6: wijzigen van de productie-implementatie

Wanneer u klaar bent, kunt u de Staging-omgeving naar de productieomgeving door te **wisselen** te kiezen in de klassieke Azure portal promoveren. De onlangs geïmplementeerde Staging omgeving wordt gepromoveerd tot de productie en de vorige productie-omgeving, wordt een Staging-omgeving. De actieve implementatie voor de productie en de Staging-omgevingen kan afwijken, maar de geschiedenis van de implementatie van recente builds is hetzelfde, ongeacht de omgeving.

![][35]

## <a name="7-deploy-from-a-working-branch"></a>7: implementeren van een bijkantoor werken.

Wanneer u met Git, meestal wijzigingen aanbrengt in een filiaal werken en integreren in de master branch wanneer uw ontwikkeling een afgewerkte staat bereikt. U zult tijdens de ontwikkelingsfase van een project, bouwen en implementeren van het filiaal werken op Azure.

1. Klik op de knop **Start** en kies vervolgens de knop **bijkantoren** in **Explorer Team**.

    ![][40]

2. Kies de koppeling **Nieuwe tak** .

    ![][41]

3. Voer de naam van het bijkantoor, zoals "werken", en kies **Structuur maken**. Hiermee maakt u een nieuwe lokale vestiging.

    ![][42]

4. De vertakking worden gepubliceerd. Kies de Filiaalnaam in **niet-gepubliceerde bedrijfstakken**en kies **publiceren**.

    ![][44]

6. Standaard trigger alleen wijzigingen in de master branch voor een continue build. Kies de pagina **maakt** in **Explorer Team**continu bouwen voor een filiaal werken stelt en kies **Bouwen definitie bewerken**.

7. Open het tabblad **Instellingen van de bron** . Kies onder het **gecontroleerde filialen voor continue integratie en bouwen**, **Klik hier om een nieuwe rij**.

    ![][47]

8. Geef de vertakking die u hebt gemaakt, zoals refs/heads/werken.

    ![][48]

9. Een wijziging aanbrengt in de code, opent u het snelmenu voor het gewijzigde bestand en kies vervolgens **doorvoeren**.

    ![][43]

10. Kies de koppeling **Niet-gesynchroniseerde doorvoeracties** en kies de knop **synchronisatie** of de **Push-** koppeling te kopiëren van de wijzigingen in de kopie van het bijkantoor werken in Visual Studio Team Services.

    ![][45]

11. Ga naar de weergave **wordt gemaakt** en de build die u zojuist hebt geactiveerd voor het filiaal werken vinden.

## <a name="next-steps"></a>Volgende stappen

Meer tips over het gebruik van Git met Visual Studio Team Services Zie [ontwikkelen en delen van uw code in met behulp van Visual Studio Git](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) en Zie voor meer informatie over het gebruik van een Git repository die niet wordt beheerd door Visual Studio Team Services te publiceren naar Azure [Continu distributie naar Azure App-Service](../app-service-web/app-service-continuous-deployment.md). Zie voor meer informatie over Visual Studio Team Services, [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateTeamProjectInGit.PNG
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png
[3]: ./media/cloud-services-continuous-delivery-use-vso-git/CloneThisRepository.PNG
[4]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateNewSolutionInClonedRepo.PNG
[7]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitMenuItem.PNG
[8]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[9]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateCloudService.PNG
[10]: ./media/cloud-services-continuous-delivery-use-vso-git/SetUpPublishingWithVSO.PNG
[11]: ./media/cloud-services-continuous-delivery-use-vso-git/AuthorizeConnection.PNG
[12]: ./media/cloud-services-continuous-delivery-use-vso-git/ConnectionRequest.PNG
[13]: ./media/cloud-services-continuous-delivery-use-vso-git/ChooseARepo3.PNG
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso-git/MakeACodeChange.PNG
[20]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[21]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerHome.png
[22]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerBuilds.PNG
[23]: ./media/cloud-services-continuous-delivery-use-vso-git/BuildInQueue.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso-git/ProcessTab.PNG
[28]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateANewAccount.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso-git/PushCurrentBranch.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso-git/BranchesInTeamExplorer.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso-git/NewBranch.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateBranch.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso-git/PublishBranch.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso-git/SourceSettingsPage.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso-git/IncludeWorkingBranch.PNG
