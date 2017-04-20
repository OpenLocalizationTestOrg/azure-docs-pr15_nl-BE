<properties
    pageTitle="Lokale Git distributie naar Azure App-Service"
    description="Informatie over het inschakelen van lokale Git distributie naar Azure App-Service."
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
    ms.date="06/13/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="local-git-deployment-to-azure-app-service"></a>Lokale Git distributie naar Azure App-Service

In deze zelfstudie wordt beschreven hoe u uw app [Azure App] service implementeren vanaf een Git repository op uw lokale computer. App-Service ondersteunt deze aanpak met de optie **Lokale Git** distributie in de [Portal Azure].  
Veel van de in dit artikel beschreven Git-opdrachten worden automatisch uitgevoerd bij het maken van een App Service app via de [opdrachtregelinterface Azure] als beschreven [hier](app-service-web-get-started.md).

## <a name="prerequisites"></a>Vereisten

Deze zelfstudie hebt u het volgende nodig:

- GIT. U kunt de installatie van binaire [hier](http://www.git-scm.com/downloads)downloaden.  
- Basiskennis van Git.
- Een account van Microsoft Azure. Als u geen account hebt, kunt u [uw voordelen van Visual Studio abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details)of [registreren voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial) .

>[AZURE.NOTE] Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een app tijdelijk starter in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.  

## <a name="Step1"></a>Stap 1: Maak een lokale opslagplaats

De volgende taken uitvoeren om te maken een nieuw Git repository.

1. Start een opdrachtregelprogramma, zoals **GitBash** (Windows) of **Bash** (Unix-Shell). Op OS X-systemen is toegankelijk vanaf de opdrachtregel door de **Terminal** -toepassing.

2. Ga naar de map waar de inhoud te implementeren geplaatst worden.

3. Gebruik de volgende opdracht om een nieuw Git repository initialiseren:

        git init

## <a name="Step2"></a>Stap 2: Uw inhoud vastleggen

App-Service ondersteunt toepassingen die zijn gemaakt in verschillende programmeertalen. 

1. Als uw bibliotheek bevat inhoud overslaan dit punt en verplaatst naar punt 2 hieronder. Als uw bibliotheek niet al inhoud eenvoudig te vullen met een statische HTML-bestand als volgt: 

    - Met behulp van een teksteditor, maak een nieuw bestand met de naam **index.html** in de hoofdmap van de Git repository
    - De volgende tekst toevoegen als u de inhoud voor het index.html bestand en sla het op: *Hallo Git!*
        
2. Controleer of u bent onder de hoofdmap van je Git repository vanaf de opdrachtregel. Bestanden toevoegen aan uw bibliotheek gebruikt u de volgende opdracht:

        git add -A 

4. Doorvoeren van de wijzigingen vervolgens naar de bibliotheek met de volgende opdracht:

        git commit -m "Hello Azure App Service"

## <a name="Step3"></a>Stap 3: De App Service app bibliotheek inschakelen

Voer de volgende stappen uit om een Git repository voor uw app App-Service inschakelen.

1. Log in op de [Azure Portal].

2. In uw App Service app van blade, klikt u op **Instellingen > distributiebron**. Klik op **bron kiezen**en vervolgens klikt u op **Lokale Git Repository**en klik op **OK**.  

    ![Lokale Git Repository](./media/app-service-deploy-local-git/local_git_selection.png)

3. Als dit de eerste keer instellen van een opslagplaats in Azure, moet u voor het maken van inloggegevens. U kunt ze zich aanmelden bij de Azure repository en push wijzigingen van uw lokale Git repository. Blade van uw app, klik op **Instellingen > referenties implementatie**, configureert u uw implementatie van gebruikersnaam en wachtwoord. Wanneer u bent klaar, klikt u op **Opslaan**.

    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a name="Step4"></a>Stap 4: Uw project implementeren

Gebruik de volgende stappen uit voor het publiceren van uw app App-service met behulp van lokale Git.

1. In de blade van uw app in de Portal Azure, klikt u op **Instellingen > Eigenschappen van** voor de **Git URL**.

    ![](./media/app-service-deploy-local-git/git_url.png)

    **GIT-URL** is de externe verwijzing naar implementeren van uw lokale bibliotheek. U gebruikt deze URL in de volgende stappen uit.

2. Met behulp van de opdrachtregel, Controleer of u in de hoofdmap van uw lokale Git repository.

3. Gebruik `git remote` de externe verwijzing toevoegen die worden vermeld in **Git URL** uit stap 1. De opdracht ziet er ongeveer als volgt:

        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
    > [AZURE.NOTE] De **externe** opdracht wordt toegevoegd met de naam verwijst naar een externe dataopslag. In dit voorbeeld wordt een verwijzing met de naam 'azure' voor uw webtoepassing opslagplaats gemaakt.

4. Uw inhoud push naar App-Service met behulp van de nieuwe **azure** externe die u zojuist hebt gemaakt.

        git push azure master

    U wordt gevraagd het wachtwoord dat u eerder hebt gemaakt als u uw referenties voor implementatie in de Portal Azure opnieuw instelt. Voer het wachtwoord (Let op dat Gitbash geen sterretjes aan de console heeft echo terwijl u uw wachtwoord typt). 
       
5. Ga terug naar uw app in de Portal Azure. Een vermelding van uw meest recente push moet worden weergegeven in de blade **implementaties** . 

    ![](./media/app-service-deploy-local-git/deployment_history.png)

6. Klik op de knop **Bladeren** boven blade van de toepassing om te controleren of dat de inhoud is geïmplementeerd. 
    
## <a name="Step5"></a>Het oplossen van problemen

Zijn fouten of problemen vaak bij het gebruik van Git publiceren naar een App Service app in Azure:

****

**Probleem**: kan geen toegang [siteURL]: kan geen verbinding maken met [scmAddress]

**Oorzaak**: deze fout kan optreden als de toepassing niet actief is.

**Oplossing**: Start de app in de Portal Azure. Implementatie van GIT werken alleen als de toepassing wordt uitgevoerd. 


****

**Probleem**: kan geen host 'naam' oplossen

**Oorzaak**: deze fout kan optreden als de gegevens ingevoerd bij het maken van de 'azure' RAS onjuist is.

**Oplossing**: Gebruik de `git remote -v` opdracht om een lijst van alle afstandsbedieningen, samen met de bijbehorende URL. Controleer of de URL voor de externe 'azure' klopt. Indien nodig kunt verwijderen en opnieuw maken deze afstandsbediening met behulp van de juiste URL.

****

**Symptoom**: geen referenties in gemeenschappelijke en niet opgegeven; niets te doen. Misschien moet u een tak zoals 'master'.

**Oorzaak**: deze fout kan optreden als u niet een bijkantoor opgeeft bij het uitvoeren van een git push-bewerking en de push.default-waarde die wordt gebruikt door Git niet hebt ingesteld.

**Resolutie**: de push-bewerking opnieuw opgeven van de master branch uitvoeren. Bijvoorbeeld:

    git push azure master

****

**Symptoom**: src refspec [branchname] komt niet overeen.

**Oorzaak**: deze fout kan optreden als u probeert te duwen op een tak dan master op 'azure' RAS.

**Resolutie**: de push-bewerking opnieuw opgeven van de master branch uitvoeren. Bijvoorbeeld:

    git push azure master

****

**Symptoom**: fout: uw web app niet bijgewerkt, maar wijzigingen die zijn doorgevoerd op de externe dataopslag.

**Oorzaak**: deze fout kan optreden als u een Node.js app met een package.json-bestand waarmee u aanvullende vereiste modules implementeert.

**Resolutie**: extra berichten met 'npm ERR!' moeten worden geregistreerd voordat deze fout en kan extra context bieden bij de fout. Hieronder worden bekende oorzaken van deze fout en de bijbehorende 'npm ERR!' Bericht:

* **Onjuiste package.json bestand**: npm ERR! Afhankelijkheden kan niet lezen.

* **Systeemeigen module die niet over een binaire distributie voor Windows**:

    * npm fout! \`cmd '/ c""knooppunt gyp opnieuw maken'\` is mislukt: 1

        OR

    * npm fout! [modulename@version]vooraf: \`maken || gmake\`


## <a name="additional-resources"></a>Aanvullende bronnen

* [GIT-documentatie](http://git-scm.com/documentation)
* [Project Kudu documentatie](https://github.com/projectkudu/kudu/wiki)
* [Continu-implementatie naar Azure App-Service](app-service-continuous-deployment.md)
* [Hoe gebruikt u PowerShell voor Azure](../powershell-install-configure.md)
* [Het gebruik van de opdrachtregelinterface van Azure](../xplat-cli-install.md)

[Azure App-Service]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/
[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[Azure Portal]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure Interface met opdrachtregel]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
