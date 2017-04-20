<properties
    pageTitle="Doorlopende implementatie met Azure App Service | Microsoft Azure"
    description="Leer doorlopend implementatie met Azure App Service mogelijk te maken."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="continuous-deployment-to-azure-app-service"></a>Doorlopende implementatie naar Azure App-Service

In deze zelfstudie wordt beschreven hoe u een doorlopende implementatieworkflow configureren voor uw app [Azure App-Service] . App-integratie met BitBucket, GitHub en Visual Studio Team Services (VSTS) kan een continue implementatieworkflow waar Azure in de meest recente updates van uw project op een van deze services zijn gepubliceerd haalt. Continuous deployment is een geweldige optie voor projecten waarin meerdere en frequent bijdragen worden geïntegreerd.

## <a name="overview"></a>Doorlopende implementatie mogelijk te maken

Doorlopende implementatie, inschakelen 

1. De inhoud van uw app publiceren naar de bibliotheek die wordt gebruikt voor de implementatie van continue.  
    Zie voor meer informatie over het publiceren van uw project op deze services [maken een repo (GitHub)], [maken een repo (BitBucket)]en [aan de slag met VSTS].

2. Klik in het menu-blade van uw app in de [portal Azure], **APP implementatie > implementatie-opties**. Klik op **Bron kiezen**en selecteer vervolgens de bron van de implementatie.  

    ![](./media/app-service-continuous-deployment/cd_options.png)
    
    > [AZURE.NOTE] Als u wilt configureren een VSTS Zie account voor de implementatie van App-Service deze [zelfstudie](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).
    
3. De vergunning workflow voltooid. 

4. Kies het project en de structuur voor de implementatie van in de blade **distributiebron** . Wanneer u bent klaar, klikt u op **OK**.
  
    ![](./media/app-service-continuous-deployment/github_option.png)

    > [AZURE.NOTE] Als u doorlopend implementatie met GitHub of BitBucket inschakelt, wordt openbare en particuliere projecten weergegeven.

    App Service maakt een koppeling met de geselecteerde bibliotheek, worden de bestanden in de opgegeven structuur en onderhoudt een kloon van de opslagplaats voor uw app App-Service. Wanneer u continu implementatie van het portal voor Azure VSTS configureert, de integratie wordt gebruikt de App Service [Kudu installatie-engine](https://github.com/projectkudu/kudu/wiki)die al geautomatiseerd samenstellen en uitvoeren van taken met elke `git push`. U hoeft niet afzonderlijk instellen van de continue implementatie in VSTS. Nadat dit proces is voltooid, ziet de app **implementatie-opties** bladeserver een actieve implementatie die geeft aan dat de installatie is voltooid.

5. Om te controleren of dat de toepassing is geïnstalleerd, klikt u op de **URL** aan de bovenkant van de bladeserver in Azure portal van de app. 

6. Om te controleren dat continue implementatie in de opslagplaats van uw keuze plaatsvindt, push-een wijziging in de opslaglocatie. Uw app moet worden bijgewerkt met de wijzigingen kort nadat de drukknop in de opslagplaats is voltooid. U kunt controleren of de update in de **implementatie-opties** bladeserver van uw app heeft getrokken.

## <a name="VSsolution"></a>Continuous deployment van Visual Studio-oplossing 

Een Visual Studio-oplossing te pushen naar Azure App Service is net zo gemakkelijk als een eenvoudige index.html bestand duwen. Het implementatieproces App Service stroomlijnt alle details, inclusief afhankelijkheden NuGet herstellen en bouwen van de binaire bestanden van toepassing. U kunt afdoende is de bron controle code alleen in de Git repository te handhaven en laat App-Service implementatie voor de rest zorgen.

De stappen voor het duwen van de Visual Studio-oplossing met App-Service zijn hetzelfde als in de [vorige sectie](#overview), op voorwaarde dat u de oplossing en de opslagplaats als volgt configureren:

-   Gebruik de optie besturingselement voor Visual Studio voor het genereren van een `.gitignore` , zoals de afbeelding hieronder of handmatig toevoegen bestand een `.gitignore` bestand in de hoofdmap van uw bibliotheek met inhoud, zoals in dit [voorbeeld .gitignore](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore). 

    ![](./media/app-service-continuous-deployment/VS_source_control.png)
 
-   De mapstructuur van de gehele oplossing toevoegen aan uw bibliotheek, met het bestand .sln in de hoofdmap van de opslagplaats.

Nadat u hebt ingesteld in uw bibliotheek zoals beschreven en uw app in Azure voor continue publiceren vanaf een van de online opslagplaatsen voor Git geconfigureerd, kunt u uw ASP.NET-toepassing in Visual Studio lokaal ontwikkelen en voortdurend uw code implementeren door uw wijzigingen aan uw online Git repository pushen.

## <a name="disableCD"></a>Continuous deployment uitschakelen

Doorlopende implementatie, uitschakelen 

1. Klik in het menu-blade van uw app in de [portal Azure], **APP implementatie > implementatie-opties**. Klik op **verbinding verbreken** in de **implementatie-opties** bladeserver.

    ![](./media/app-service-continuous-deployment/cd_disconnect.png)    

2. Na het beantwoorden van **Ja** om bevestiging wordt gevraagd, kunt u terug naar de blade van uw app en klik **implementatie APP > Opties voor distributie** als u wilt publiceren uit een andere bron instellen.

## <a name="additional-resources"></a>Aanvullende bronnen

* [Hoe u veelvoorkomende problemen met de implementatie van continue onderzoeken](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Hoe gebruikt u PowerShell voor Azure]
* [Het gebruik van de opdrachtregelprogramma's voor Azure voor Mac en Linux]
* [GIT-documentatie]
* [Project Kudu](https://github.com/projectkudu/kudu/wiki)

>[AZURE.NOTE] Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een starter tijdelijk web app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.

[Azure App-Service]: https://azure.microsoft.com/en-us/documentation/articles/app-service-changes-existing-services/ 
[Azure portal]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Hoe gebruikt u PowerShell voor Azure]: ../articles/powershell-install-configure.md
[Het gebruik van de opdrachtregelprogramma's voor Azure voor Mac en Linux]: ../articles/xplat-cli-install.md
[GIT-documentatie]: http://git-scm.com/documentation

[Maak een repo (GitHub)]: https://help.github.com/articles/create-a-repo
[Maak een repo (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[Aan de slag met VSTS]: https://www.visualstudio.com/get-started/overview-of-get-started-tasks-vs
[Continuous delivery to Azure using Visual Studio Team Services]: ../articles/cloud-services/cloud-services-continuous-delivery-use-vso.md
