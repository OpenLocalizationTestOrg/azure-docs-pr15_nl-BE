<properties
    pageTitle="Flexibele softwareontwikkeling met Azure App-Service"
    description="Informatie over het maken van complexe toepassingen voor hoge schaalbaarheid met Azure App-Service op een manier die de ontwikkeling van flexibele software ondersteunt."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/01/2016"
    ms.author="cephalin"/>


# <a name="agile-software-development-with-azure-app-service"></a>Flexibele softwareontwikkeling met Azure App-Service #

In deze zelfstudie leert u het maken van complexe toepassingen voor hoge schaalbaarheid met [Azure App-Service](/services/app-service/) op een manier die de [ontwikkeling van flexibele software](https://en.wikipedia.org/wiki/Agile_software_development)ondersteunt. Hierbij wordt ervan uitgegaan dat u al weet dat het [implementeren van complexe toepassingen voorspelbaar in Azure](app-service-deploy-complex-application-predictably.md).

Beperkingen in de technische processen kunnen de succesvolle implementatie van flexibele methoden vaak verhinderen. Azure App-Service met de functies van de [continue publiceren](app-service-continuous-deployment.md), [staging-omgevingen](web-sites-staged-publishing.md) (sleuven) en [controle](web-sites-monitor.md), als verstandige in combinatie met de orchestration en het beheer van de implementatie in [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)kan deel uitmaken van een geweldige oplossing voor ontwikkelaars die Profiteer van flexibele softwareontwikkeling.

De volgende tabel is een overzicht van de vereisten die zijn gekoppeld aan een flexibele ontwikkeling en hoe Azure maakt van deze services.

| Vereiste | Hoe kan Azure |
|---------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -Bouwen met elke commit<br>-Automatisch bouwen en snel | Wanneer geconfigureerd met continue implementatie, werken Azure App Service als builds van live uitvoeren op basis van een dev-vertakking. Telkens wanneer de code op de vertakking wordt geduwd, is automatisch gebouwd en uitgevoerd in Azure wonen.|
| -Controleer bouwt zelf testen | Laden van tests, web, tests, enz., kunnen worden geïmplementeerd met de sjabloon Azure Resource Manager.|
| -Tests uitvoeren in een kloon van de productie-omgeving | Azure Resource Manager sjablonen kunnen worden gebruikt voor het klonen van de Azure productieomgeving (met inbegrip van de instellingen van de app, connection string sjablonen, schalen, enz.) voor het testen van snel en voorspelbaar maken.|
| -Resultaat weergeven van de laatste build gemakkelijk | Doorlopende implementatie van Azure uit een bibliotheek betekent dat u nieuwe code in een live-toepassing testen kunt onmiddellijk nadat u de wijzigingen doorvoert. |
| -Doorvoeren in de belangrijkste tak elke dag<br>-Automatisering | Elke commit/samenvoegen continue integratie van een productietoepassing met de belangrijkste tak van de bibliotheek automatisch geïmplementeerd op de belangrijkste vertakking naar productie. |

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-do"></a>Wat u doet ##

U helpt bij een standaardworkflow dev test-fase productie nieuwe wijzigingen in de voorbeeldtoepassing [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) dat bestaat uit twee [web apps](/services/app-service/web/), namelijk een frontend (FE) en anderzijds een Web API backend (BE) en een [SQL-database](/services/sql-database/)te publiceren. U werkt met de onderstaande implementatie-architectuur:

![](./media/app-service-agile-software-development/what-1-architecture.png)

Zet de afbeelding in woorden:

-   De implementatie-architectuur is onderverdeeld in drie verschillende omgevingen (of [resourcegroepen](../azure-resource-manager/resource-group-overview.md) in Azure), elk met een eigen [App serviceplan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), instellingen voor [schalen](web-sites-scale.md) en SQL-database. 
-   Elke omgeving kan afzonderlijk worden beheerd. Zij kunnen ook bestaan in verschillende abonnementen.
-   Ontwikkel- en productiecomputers worden geïmplementeerd als twee sleuven van de dezelfde App Service app. De master branch is ingesteld voor de continue integratie met de staging-sleuf.
-   Wanneer een commit op de master branch op de staging-sleuf (met productiegegevens) wordt gecontroleerd, wordt de geverifieerde staging-app in de productie-sleuf [met geen uitvaltijd](web-sites-staged-publishing.md)verwisseld.

De productie en staging omgeving wordt gedefinieerd door de sjabloon op [ * &lt;repository_root >*/ARMTemplates/ProdandStage.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/ProdAndStage.json).

De dev- en testomgevingen worden gedefinieerd door de sjabloon op [ * &lt;repository_root >*/ARMTemplates/Dev.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/Dev.json).

U gebruikt ook de typische vertakkende strategie met code verplaatsen van het bijkantoor dev tot de tak test vervolgens de master branch (omhoog verplaatst in kwaliteit, dus tot speak).

![](./media/app-service-agile-software-development/what-2-branches.png) 

## <a name="what-you-will-need"></a>Wat moet u ##

-   Een account Azure
-   Een [GitHub](https://github.com/) -account
-   GIT-Shell (geïnstalleerd met [GitHub voor Windows](https://windows.github.com/)) - Hiermee kunt u de Git en de PowerShell-opdrachten uitvoeren in dezelfde sessie 
-   Nieuwste [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/0.9.4-June2015/azure-powershell.0.9.4.msi) bits
-   Basiskennis van de volgende handelingen uit:
    -   Implementatie van de sjabloon [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) (Zie ook [een complexe toepassing in Azure voorspelbare implementatie](app-service-deploy-complex-application-predictably.md))
    -   [GIT](http://git-scm.com/documentation)
    -   [PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [AZURE.NOTE] Een Azure-account voor het voltooien van deze zelfstudie hebt u nodig:
> + U kunt [gratis een Azure account openen](/pricing/free-trial/) - credits dat u kunt u uitproberen betaalde Azure services en zelfs nadat u ze gebruikt kunt u de account en gebruik gratis Azure services, zoals Web Apps.
> + U kunt [Visual Studio abonnee voordelen activeren](/pricing/member-offers/msdn-benefits-details/) - Visual Studio uw abonnement geeft u credits elke maand die u voor betaalde Azure services gebruiken kunt.
>
> Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een starter tijdelijk web app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.

## <a name="set-up-your-production-environment"></a>De productie-omgeving instellen ##

>[AZURE.NOTE] Het script dat wordt gebruikt in deze zelfstudie configureert automatisch continue publicatie uit uw bibliotheek GitHub. Hiervoor is vereist dat uw referenties GitHub al zijn opgeslagen in Azure, anders mislukt de implementatie met de scripts tijdens het configureren van instellingen voor bronbeheer voor het web apps. 
>
>Voor het opslaan van uw referenties GitHub in Azure, een web app te maken in de [Portal Azure](https://portal.azure.com/) en [implementatie van GitHub configureren](app-service-continuous-deployment.md). Alleen moet u dit één keer doen. 

In een typisch voorbeeld van DevOps, hebt u een toepassing die wordt uitgevoerd in Azure wonen en u wilt wijzigen door middel van continue publiceren. In dit scenario hebt u een sjabloon die ontwikkeld, getest en de productie-omgeving te implementeren. U wordt geïnstalleerd in deze sectie.

1.  Maak uw eigen fork van de bibliotheek [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) . Zie voor meer informatie over het maken van uw vork [vork een Repo](https://help.github.com/articles/fork-a-repo/). Als uw vork is gemaakt, kunt u deze bekijken in uw browser.
 
    ![](./media/app-service-agile-software-development/production-1-private-repo.png)

2.  Open een Git-Shell-sessie. Als u nog geen Git-Shell, nu [GitHub voor Windows](https://windows.github.com/) installeren.

3.  Maak een lokale kopie van uw vork door het uitvoeren van de volgende opdracht:

        git clone https://github.com/<your_fork>/ToDoApp.git 

4.  Als u uw lokale kopie hebt, Ga naar * &lt;repository_root >*\ARMTemplates en de deploy.ps1 als volgt een script uitvoeren:

        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git

4.  Wanneer dat wordt gevraagd, typt u de gewenste gebruikersnaam en wachtwoord voor toegang tot de database.

    U ziet de provisioning voortgang van verschillende bronnen van Azure. Wanneer de implementatie is voltooid, wordt het script start de toepassing in de browser en geeft u een beschrijvende pieptoon.

    ![](./media/app-service-agile-software-development/production-2-app-in-browser.png)
 
    >[AZURE.TIP] Kijk eens naar * &lt;repository_root >*\ARMTemplates\Deploy.ps1 om te zien hoe deze bronnen met unieke id's genereert. U kunt dezelfde aanpak maken klonen van de dezelfde implementatie zonder zorgen te maken over tegenstrijdige resourcenamen.
 
6.  Terug in de Git-Shell-sessie uitvoeren:

        .\swap –Name ToDoApp<unique_string>master

    ![](./media/app-service-agile-software-development/production-4-swap.png)

7.  Wanneer het script is voltooid, gaat u terug naar het adres van de frontend (http://ToDoApp*&lt;unique_string >*master.azurewebsites.net/) voor een overzicht van de toepassing wordt uitgevoerd in de productie.
 
5.  Log in op de [Portal Azure](https://portal.azure.com/) en kijk eens wat wordt gemaakt.

    U moet kunnen zien twee web apps in dezelfde bronnengroep, een met de `Api` achtervoegsel in de naam. Als u de weergave van de groep resources bekijken, ook ziet u de SQL-Database en server, het abonnement App en de staging-sleuven voor het web apps. Blader door de verschillende bronnen en vergelijk deze met * &lt;repository_root >*\ARMTemplates\ProdAndStage.json om te zien hoe deze zijn geconfigureerd in de sjabloon.

    ![](./media/app-service-agile-software-development/production-3-resource-group-view.png)

U hebt nu de productie-omgeving instellen. U wordt vervolgens ere van een nieuwe update aan de toepassing.

## <a name="create-dev-and-test-branches"></a>Dev maken en testen van bijkantoren ##

Nu dat er een complexe toepassing die wordt uitgevoerd in de productie in Azure, brengt u een update voor uw toepassing volgens flexibele methodologie. In deze sectie kunt u de dev maakt en bijkantoren die u nodig hebt om de vereiste updates te testen.

1.  De testomgeving eerst maken. Voer de volgende opdrachten voor het maken van de omgeving voor een nieuwe tak **NewUpdate**genoemd in Git-Shell-sessie. 

        git checkout -b NewUpdate
        git push origin NewUpdate 
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch NewUpdate

1.  Wanneer dat wordt gevraagd, typt u de gewenste gebruikersnaam en wachtwoord voor toegang tot de database. 

    Wanneer de implementatie is voltooid, wordt het script start de toepassing in de browser en geeft u een beschrijvende pieptoon. En net zoals dat u hebt nu een nieuw bijkantoor met een eigen testomgeving. Even een paar dingen over deze testomgeving te bekijken:

    -   U kunt het maken van een abonnement op Azure. Dit betekent dat de productie-omgeving van uw testomgeving afzonderlijk kan worden beheerd.
    -   Uw testomgeving wordt live in Azure uitgevoerd.
    -   Uw testomgeving is identiek aan de productie-omgeving, met uitzondering van de staging-sleuven en de instellingen voor schalen. U kunt dit weten, omdat dit de enige verschillen tussen ProdandStage.json en Dev.json zijn.
    -   U kunt uw testomgeving beheren in een eigen App Service plan, met een andere prijs laag (zoals **gratis**).
    -   Verwijderen van deze testomgeving is net zo eenvoudig als het verwijderen van de resourcegroep. U zult zien hoe u dit [later](#delete)doen.

2.  Ga op een dev-subsleutel maken door de volgende opdrachten:

        git checkout -b Dev
        git push origin Dev
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch Dev

3.  Wanneer dat wordt gevraagd, typt u de gewenste gebruikersnaam en wachtwoord voor toegang tot de database. 

    Even een paar dingen over deze dev omgeving bekijken: 

    -   Uw omgeving dev heeft een configuratie die identiek zijn aan de testomgeving omdat deze is geïmplementeerd met behulp van dezelfde sjabloon.
    -   Elke dev omgeving kan worden gemaakt in de developer Azure abonnement, waardoor de testomgeving wordt afzonderlijk beheerd.
    -   Uw omgeving dev wordt live in Azure uitgevoerd.
    -   Het is net zo eenvoudig als het verwijderen van de resourcegroep de dev omgeving te verwijderen. U zult zien hoe u dit [later](#delete)doen.

>[AZURE.NOTE] Wanneer er meerdere ontwikkelaars die werken op de nieuwe update, kunt elk van hen een vertakking en speciale dev omgeving gemakkelijk maken door het volgende te doen:
>
>1. Maken van hun eigen fork van de opslagplaats in GitHub (Zie [een Repo Fork](https://help.github.com/articles/fork-a-repo/)).
>2. De vork op hun lokale computer kopiëren
>3. Dezelfde opdrachten maken en hun eigen filiaal dev omgeving worden uitgevoerd.

Wanneer u bent klaar, moet je vork GitHub drie vertakkingen hebben:

![](./media/app-service-agile-software-development/test-1-github-view.png)

En hebt u zes web apps (drie sets van twee) in drie verschillende resourcegroepen:

![](./media/app-service-agile-software-development/test-2-all-webapps.png)
 
>[AZURE.NOTE] Houd er rekening mee dat de ProdandStage.json geeft aan de productie-omgeving voor het gebruik van de **standaard** prijzen laag, die geschikt voor de schaalbaarheid van de productietoepassing is.

## <a name="build-and-test-every-commit"></a>Bouwen en testen van elke commit ##

De sjabloonbestanden, ProdAndStage.json en Dev.json geven al de parameters voor de bron, die standaard wordt doorlopend publishing voor het web app. Daarom gebeurtenis elke commit op de vertakking van GitHub wordt een automatische implementatie naar Azure van dat bijkantoor. We gaan kijken hoe de installatie werkt nu.

1.  Zorg ervoor dat u in de Dev-structuur van de lokale bibliotheek. Voer hiertoe de volgende opdracht in Git-Shell:

        git checkout Dev

2.  Eenvoudige wijzigingen aanbrengt in een van de app UI laag door het wijzigen van de code voor het [Bootstrap](http://getbootstrap.com/components/) -lijsten gebruiken. Open * &lt;repository_root >*\src\MultiChannelToDo.Web\index.cshtml en om de gemarkeerde wijziging hieronder:

    ![](./media/app-service-agile-software-development/commit-1-changes.png)

    >[AZURE.NOTE] Als u de bovenstaande afbeelding lezen kan: 
    >
    >- Wijzig in regel 18 `check-list` naar `list-group`.
    >- Wijzig in regel 19 `class="check-list-item"` naar `class="list-group-item"`.

3.  De wijziging opslaan. Terug in Git-Shell, voert u de volgende opdrachten:

        cd <repository_root>
        git add .
        git commit -m "changed to bootstrap style"
        git push origin Dev
 
    Deze opdrachten git zijn vergelijkbaar met 'controle op in uw code' in een ander bronbeheersysteem zoals TFS. Bij het uitvoeren van `git push`, de nieuwe commit activeert een drukknop automatische code in Azure, waarvan de toepassing aan de wijziging in de dev-omgeving opnieuw ingesteld.

4.  Om te controleren dat deze push code op uw omgeving dev is opgetreden, gaat u naar de dev omgeving van web app blade en bekijk de **implementatie** deel. U moet de laatste commit bericht te zien.

    ![](./media/app-service-agile-software-development/commit-2-deployed.png)

5.  Daar klikt u op **Bladeren** om de nieuwe wijziging in de actieve toepassing in Azure.

    ![](./media/app-service-agile-software-development/commit-3-webapp-in-browser.png)

    Dit is een heel kleine wijziging aan de toepassing. Veel tijden nieuwe wijzigingen in een complexe webtoepassing heeft echter onbedoelde en ongewenste neveneffecten. Kan heel gemakkelijk testen elke commit in live-versies kunt u deze problemen te onderscheppen voordat ze uw klanten zien.

Nu moet u vertrouwd zijn met de uitvoering, dat als een ontwikkelaar van het project **NewUpdate** kun je eenvoudig een dev omgeving maken voor uzelf, en vervolgens elke commit bouwen en testen van elke build.

## <a name="merge-code-into-test-environment"></a>Code in een testomgeving samenvoegen ##

Wanneer u klaar bent om uw code van tak tot tak NewUpdate Dev, is het proces standaard git:

1.  Alle nieuwe samenvoegen worden doorgevoerd in de NewUpdate in het bijkantoor Dev in GitHub, zoals doorvoeracties die door andere ontwikkelaars zijn gemaakt. Een nieuwe commit op GitHub wordt een push code activeren en in de omgeving van dev bouwen. U kunt vervolgens controleren of dat uw code in de tak van Dev werkt nog steeds met de meest recente bits NewUpdate zijn ondergebracht.

2.  Uw nieuwe doorvoeracties Dev vertakken samenvoegen met NewUpdate branch op GitHub. Deze actie veroorzaakt een code push en bouwen in de testomgeving. 

Notitie opnieuw omdat continuous deployment al ingesteld met deze bijkantoren git is, hoeft u niet elke andere actie zoals integratie met bouwt. U hoeft alleen voor het uitvoeren van standaard bron controle procedures met git en Azure zal alle build processen voor u uitvoeren.

Nu, laten we de code push naar **NewUpdate** tak. Voer de volgende opdrachten in de Git-Shell:

    git checkout NewUpdate
    git pull origin NewUpdate
    git merge Dev
    git push origin NewUpdate

Dat is alles. 

Ga naar de blade web app voor uw testomgeving voor een overzicht van de nieuwe commit (samengevoegd met de tak NewUpdate) nu naar de testomgeving wordt geduwd. Klik vervolgens op **Bladeren** om te zien of de stijl wijzigen wordt nu uitgevoerd live in Azure.

## <a name="deploy-update-to-production"></a>Update implementeren op productie ##

Code voor het milieu van de ontwikkel- en productiecomputers duwen erop vertrouwen dat het niet anders dan wat u al hebt verricht wanneer u code naar de testomgeving geduwd. Het is heel eenvoudig. 

Voer de volgende opdrachten in de Git-Shell:

    git checkout master
    git pull origin master
    git merge NewUpdate
    git push origin master

Houd er rekening mee dat op basis van de manier waarop die de ontwikkel- en productiecomputers omgeving ingesteld op ProdandStage.json wordt, de nieuwe code naar de **Staging-** sleuf wordt geduwd en er wordt uitgevoerd. Dus als u naar de URL van de staging-sleuf navigeert, ziet u de nieuwe code er uitgevoerd. Voer hiervoor de `Show-AzureWebsite` cmdlet in Git-Shell.

    Show-AzureWebsite -Name ToDoApp<unique_string>master -Slot Staging
 
En nu, nadat u de update in de staging-sleuf hebt gecontroleerd, het enige wat te doen is vervangt in productie. In de Git-Shell, maar Voer de volgende opdrachten:

    cd <repository_root>\ARMTemplates
    .\swap.ps1 -Name ToDoApp<unique_string>master

Gefeliciteerd! U hebt een nieuwe update naar uw webtoepassing productie gepubliceerd. Sterker nog is dat u hebt zojuist gedaan door dev gemakkelijk maken en testen omgevingen, en bij het maken en testen van elke commit. Dit zijn de essentiële bouwstenen voor de softwareontwikkeling van flexibele.

<a name="delete"></a>
## <a name="delete-dev-and-test-enviroments"></a>Dev verwijderen en enviroments testen ##

Omdat u uw dev- en testomgevingen voor zelfstandige resourcegroepen worden doelbewust hebben ontworpen, is het heel gemakkelijk om ze te verwijderen. Als u wilt verwijderen die u hebt gemaakt in deze zelfstudie, de GitHub takken en de Azure artefacten, alleen de volgende opdrachten uitvoeren in de Git-Shell:

    git branch -d Dev
    git push origin :Dev
    git branch -d NewUpdate
    git push origin :NewUpdate
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>dev-group -Force -Verbose
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>newupdate-group -Force -Verbose

## <a name="summary"></a>Samenvatting ##

Flexibele softwareontwikkeling is een must zijn voor veel bedrijven willen Azure als platform voor hun vast te stellen. In deze zelfstudie hebt u geleerd hoe maken en exacte replica of replica's van de productie-omgeving in de buurt van tear met gemak, zelfs voor complexe toepassingen. U hebt ook geleerd hoe om te profiteren van deze mogelijkheid voor het maken van een ontwikkelingsproces dat kunt bouwen en testen van elke één commit in Azure. Deze zelfstudie Hopelijk leert u hoe u best kunt Azure App Service en Azure Resource Manager samen een DevOps oplossing die de caters voor flexibele methodologie te maken. Vervolgens kunt u in dit scenario maken door het uitvoeren van geavanceerde technieken voor DevOps zoals het [testen van de productie](app-service-web-test-in-production-get-start.md). Zie voor een algemeen scenario voor het testen van in productie [Flighting-implementatie (béta-testfase) in Azure App-Service](app-service-web-test-in-production-controlled-test-flight.md).

## <a name="more-resources"></a>Meer bronnen ##

-   [Een complexe toepassing in Azure voorspelbare implementeren](app-service-deploy-complex-application-predictably.md)
-   [Flexibele ontwikkeling in de praktijk: Tips en trucs voor gemoderniseerde ontwikkelingscyclus](http://channel9.msdn.com/Events/Ignite/2015/BRK3707)
-   [Geavanceerde implementatiestrategieën voor Azure Web Apps met behulp van bronbeheer sjablonen](http://channel9.msdn.com/Events/Build/2015/2-620)
-   [Azure Resource Manager sjablonen ontwerpen](../resource-group-authoring-templates.md)
-   [JSONLint - de JSON Validator](http://jsonlint.com/)
-   [ARMClient – GitHub publiceren naar een site instellen](https://github.com/projectKudu/ARMClient/wiki/Setup-GitHub-publishing-to-Site)
-   [GIT vertakkingen: Basic vertakkingen en samenvoegen](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
-   [Blog van David Ebbo](http://blog.davidebbo.com/)
-   [Azure PowerShell](../powershell-install-configure.md)
-   [Opdrachtregelprogramma's voor Azure Cross-Platform](../xplat-cli-install.md)
-   [Maken of gebruikers in Azure advertentie bewerken](https://msdn.microsoft.com/library/azure/hh967632.aspx#BKMK_1)
-   [Project Kudu Wiki](https://github.com/projectkudu/kudu/wiki)
