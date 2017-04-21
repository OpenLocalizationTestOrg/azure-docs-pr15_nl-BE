<properties
pageTitle="Installeren en instellen van hulpprogramma's voor het schrijven in de GitHub"
description="Hulpprogramma's en stappen ingesteld voor authoring Azure inhoud op GitHub ophalen."
services="contributor-guide"
documentationCenter=""
authors="tysonn"  
manager="carolz" />

<tags
ms.service="contributor-guide"
 ms.devlang=""
 ms.topic="article"
  ms.tgt_pltfrm=""
  ms.workload=""
  ms.date="01/19/2015"
  ms.author="tysonn" />

#<a name="install-and-set-up-tools-for-authoring-in-github"></a>Installeren en instellen van hulpprogramma's voor het schrijven in de GitHub

Volg de stappen in dit artikel voor het instellen van extra bij te dragen tot de Azure technische documentatie. Toevallige en onregelmatige medewerkers kunnen waarschijnlijk de GitHub gebruikersinterface beschreven in stap 2 gebruiken.

Als u niet bekend met Git bent, wilt u mogelijk Git terminologie bekijken: [https://help.github.com/articles/github-glossary](https://help.github.com/articles/github-glossary). Ook deze thread StackOverflow bevat een lijst van Git termen die u in deze reeks stappen tegenkomt: [http://stackoverflow.com/questions/7076164/terminology-used-by-git](http://stackoverflow.com/questions/7076164/terminology-used-by-git)

## <a name="contents"></a>Inhoud

- [Maak een GitHub-account en uw profiel instellen](#create-a-github-account-and-set-up-your-profile)
- [Aanmelden voor Disqus](#sign-up-for-disqus)
- [Bepalen of u echt nodig hebt om de rest van deze stappen volgen](#determine-whether-you-really-need-to-follow-the-rest-of-these-steps)
- [Machtigingen op GitHub](#permissions-in-github)
- [Git installeren voor Windows](#install-git-for-windows)
- [Verificatie met twee factoren inschakelen](#enable-two-factor-authentication)
- [Een prijsverlaging editor installeren](#install-a-markdown-editor)
- [Atom configureren](#configure-atom)
- [De opslagplaats zich splitsen en naar uw computer kopiëren](#fork-the-repository-and-copy-it-to-your-computer)
- [Configureer uw gebruikersnaam en e-lokaal](#configure-your-user-name-and-email-locally)
- [Volgende stappen](#next-steps)

## <a name="create-a-github-account-and-set-up-your-profile"></a>Maak een GitHub-account en uw profiel instellen

Als u wilt bijdragen aan de Azure technische inhoud, moet u een [GitHub](http://www.github.com) -account.

Als u een Microsoft-medewerker bent, moet u uw GitHub-account instellen zodat u duidelijk zijn geïdentificeerd als een medewerker van Microsoft. Uw profiel als volgt instellen:

- **Profielfoto**: een foto van u (vereist)
- **Naam**: uw eerste en laatste naam (vereist)
- **E-mailadres**: uw e-mailadres voor Microsoft (optioneel)
- **Bedrijf**: Microsoft Corporation (vereist)
- **Locatie**: uw woonplaats (optioneel)

Dit profiel moet vergelijkbaar zijn met uw profiel:

<p align="center">
 ![Voorbeeld van GitHub profiel](./media/tools-and-setup/githubprofile.png)

## <a name="sign-up-for-disqus"></a>Aanmelden voor Disqus

Elke gepubliceerde technische artikel Azure heeft een opmerking stroom die door de service Disqus.

 ![Dialoog-logo](./media/tools-and-setup/discus.png)

Als u een Microsoft-medewerker en als u de auteur van of naar een artikel Inzender, moet u zich aanmelden voor Disqus zodat u aan de opmerking stroom voor het artikel deelnemen kunt.

1. Aanmelden voor een account op [http://www.disqus.com/](http://www.disqus.com/)
2. Vul uw profiel als volgt in:

 - **Volledige naam**: uw volledige naam, zoals wordt weergegeven in uw aanbieding Microsoft adresboek, plus de tussen haakjes info, is uw alias plus @MSFT. Indeling: *eerst laatst [alias@MSFT] *
 - **Locatie**: uw locatie
 - **Korte Bio**: de titel van uw

## <a name="determine-whether-you-really-need-to-follow-the-rest-of-these-steps"></a>Bepalen of u echt nodig hebt om de rest van deze stappen volgen

Mogelijk moet u niet de stappen in dit artikel te volgen. Dat hangt af van de sortering van webinhoud die u wilt of moet maken.

###<a name="submit-a-text-only-change-to-an-existing-article"></a>Een alleen-tekst wijzigen op een bestaand artikel indienen

Als u alleen nodig hebt of wilt tekstuele wijzigingen aan een bestaand artikel, moet waarschijnlijk niet u de rest van de stappen te volgen. Van GitHub prijsverlaging web gebaseerde editor kunt u uw wijzigingen wilt indienen. Klik op de link GitHub in het artikel dat u wilt wijzigen:

 ![Voorbeeld van GitHub profiel](./media/tools-and-setup/contributetogit.png)

 Klik vervolgens op het bewerkingspictogram in de GitHub-versie van het artikel

 ![Voorbeeld van GitHub profiel](./media/tools-and-setup/editicon.PNG)

 Die wordt geopend met de eenvoudig te gebruiken webeditor waarmee u gemakkelijk wijzigingen wilt indienen. U hoeft niet volgt u de overige stappen in dit artikel.

###<a name="all-other-changes"></a>Alle andere wijzigingen
De UI GitHub ondersteunt het maken van nieuwe bestanden slepen en neerzetten van afbeeldingen. Echter, wanneer u in de gebruikersinterface werkt, bijkantoren beheren kan verwarrend zijn dus meestal aangeraden u installeert de hulpprogramma's en informatie over de opdrachten voor het maken en beheren van artikelen. Als u wilt dat de gebruikersinterface, Zie:

- [Bestanden maken op Github](https://github.com/blog/1327-creating-files-on-github)
- [Bestanden uploaden naar de opslagplaatsen](https://github.com/blog/2105-upload-files-to-your-repositories)

Voor de volgende soorten werk raden wij u installeren en te leren werken met de hulpprogramma's:

 - Grote wijzigingen aanbrengt op een artikel
 - Maken en publiceren van een nieuw artikel
 - Nieuwe afbeeldingen toevoegen of bijwerken van afbeeldingen
 - Bijwerken van een artikel gedurende een periode van dagen zonder publiceren wijzigingen van die dagen
 - Maken van inhoud voor een release die moet op een bepaalde dag op een bepaald moment

##<a name="permissions-in-github"></a>Machtigingen op GitHub

Iedereen met een GitHub account kan leiden tot Azure technische inhoud via onze openbare bibliotheek op [https://github.com/Azure/azure-content](https://github.com/Azure/azure-content). Er zijn geen speciale machtigingen vereist.

Als u een Microsoft-PM of schrijver die op Azure inhoud werkt, moet u werken in onze eigen opslagplaats voor inhoud, azure-inhoud-pr. Ga naar [https://repos.opensource.microsoft.com](https://repos.opensource.microsoft.com ) om te vragen de leesmachtigingen waarmee u bijdragen tot en met de particuliere repo - GitHub via de knop aanmelden > Klik op Azure > op **Join een team** of **deelnemen aan een ander team**, en vervolgens zoeken naar en deelnemen aan de groep **azure inhoud lezen** .

## <a name="install-git-for-windows"></a>Git installeren voor Windows

Git voor Windows installeren vanaf [http://git-scm.com/download/win](http://git-scm.com/download/win). Deze download installeert het Git version control system, en Git Bash, het opdrachtregelprogramma app die u gebruiken wilt voor de interactie met uw lokale Git repository worden geïnstalleerd.

U kunt de standaard-instellingen accepteren Als u wilt dat de opdrachten die beschikbaar zijn in de Windows-opdrachtregel de optie die het mogelijk maakt.

<p align="center">
 ![Voorbeeld van GitHub profiel](./media/tools-and-setup/gitbashinstall.png)

(Opmerking: dit is niet hetzelfde als 'Github for Windows'. "Github voor Windows" is een andere GUI-hulpprogramma is dat ook werkt als u wilt Lees informatie over uzelf. [https://windows.github.com/](https://windows.github.com/))

## <a name="enable-two-factor-authentication"></a>Verificatie met twee factoren inschakelen

U moet verificatie met twee factoren (2FA) inschakelen op je GitHub account als u in de persoonlijke content repository werkt. In de opslagplaats van persoonlijke verplicht.

Volg de instructies in zowel de GitHub help-onderwerpen om dit mogelijk:

- [Informatie over verificatie met twee factoren](https://help.github.com/articles/about-two-factor-authentication/)
- [Maken van een toegangstoken voor de opdrachtregel gebruiken](https://help.github.com/articles/creating-an-access-token-for-command-line-use/)

Als u het token maakt, selecteert u alle scopes die beschikbaar zijn in de gebruikersinterface maken ([details op elk bereik](https://developer.github.com/v3/oauth/#scopes))

Nadat u 2FA hebt ingeschakeld, hebt u het toegangstoken in plaats van uw wachtwoord GitHub op de opdrachtregel opgeven wanneer u probeert toegang te krijgen tot een opslagplaats GitHub vanaf de opdrachtregel. Het toegangstoken is niet de verificatiecode die u in een bericht ontvangt bij het instellen van 2FA. Het is een lange tekenreeks die er ongeveer als volgt uitziet: fdd3b7d3d4f0d2bb2cd3d58dba54bd6bafcd8dee. Enkele opmerkingen over dit:

- Opslaan in een tekstbestand, zodat deze gemakkelijk toegankelijk is wanneer u deze nodig hebt bij het maken van het toegangstoken.

- Later, wanneer u plakken van het token wilt, weet er zijn twee manieren om te plakken op de opdrachtregel:

 - Klik op het pictogram in de linkerbovenhoek van het venster opdrachtregel > Bewerken > Plakken.
 - Klik met de rechtermuisknop op het pictogram in de linkerbovenhoek van het venster en klik op Eigenschappen > Opties > modus Snel bewerken. Hiermee configureert u de opdrachtregel, zodat u met de rechtermuisknop op in het venster opdrachtregel kunt plakken.

## <a name="install-a-markdown-editor"></a>Een prijsverlaging editor installeren

We schrijven inhoud met behulp van eenvoudige "prijsverlaging" notatie in de bestanden, maar dan complex "aantekeningen" (HTML, XML, enz.). U moet dus een prijsverlaging editor installeren.

- **Atom**: de meeste van ons gebruik van GitHub Atom prijsverlaging editor: [http://atom.io](http://atom.io). Het hoeft niet een licentie voor zakelijk gebruik. Spellingcontrole is.

- **Kladblok**: U kunt Kladblok voor een zeer lichte optie.

- **Prose**: dit is een lichte, elegante online en open source prijsverlaging editor biedt een voorbeeld. Ga naar [http://prose.io](http://prose.io) en Prose in uw bibliotheek staan.

- **[Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx)** - item van Microsoft in deze ruimte.

## <a name="configure-atom"></a>Atom configureren

Als u een atoom gebruikt, moet u een aantal zaken instellen.

- Atom 2 ruimten voor tabs gebruikt standaard, maar de prijsverlaging 4 ruimten verwacht. Als u deze op de standaardinstelling van twee laat uw artikel zullen er fantastisch uitzien in lokale voorvertoning, maar niet wanneer deze wordt geïmporteerd in Azure. Dus configureren Atom 4 ruimten gebruik - u kunt deze instelling vinden onder bestand > Instellingen > Editor instellingen > tabblad lengte.
- U zal waarschijnlijk ook wilt inschakelen zachte teruglopen in deze sectie, welke is gelijk aan "word wrap" in Kladblok.
- Klik in te schakelen op de voorvertoning prijsverlaging pakketten > prijsverlaging Preview > Voorvertoning in-of uitschakelen. Ctrl-Shift-M kunt u de HTML-weergave van de voorvertoning in-of uitschakelen.

## <a name="fork-the-repository-and-copy-it-to-your-computer"></a>De opslagplaats zich splitsen en naar uw computer kopiëren

1. Een vork van de opslagplaats maken in GitHub - Ga naar de rechterbovenhoek van de pagina en klik op de knop Fork. Selecteer desgevraagd uw rekening als de locatie waar de fork moet worden gemaakt. Hiermee maakt u een kopie van de bibliotheek in uw account Git Hub. In het algemeen, technisch schrijvers en programma managers moeten fork azure-inhoud-pr, het particuliere repo. Medewerkers van de Gemeenschap moeten fork azure-inhoud, de openbare repo. U hoeft slechts één keer; zich splitsen na de eerste setup als u wilt uw vork kopiëren naar een andere computer hebt alleen u voor het uitvoeren van de opdrachten die in deze sectie de repo naar uw computer kopiëren.  Als u de vorken van beide opslaglocaties maken, moet u een vork voor elke opslagplaats te maken.

2. Kopieer de persoonlijke Access Token die u hebt gekregen van [https://github.com/settings/tokens](https://github.com/settings/tokens). U kunt de standaardmachtigingen voor de token accepteren.  Persoonlijke Access Token opslaan in een tekstbestand voor later gebruik.

3. Kopieer de bibliotheek naar uw computer met de referenties die zijn ingesloten in de opdrachtreeks.  U doet dit door Git Bash openen en uitvoeren als beheerder. Voer de volgende opdracht bij de opdrachtprompt.  Met deze opdracht maakt een map azure-content(-pr) op uw computer.  Als u de standaardlocatie gebruikt, dient op c:\users<your Windows user name>\azure-content(-pr).

Openbare repo:

        git clone https://[your GitHub user name]:[token]@github.com/<your GitHub user name>/azure-content.git

Persoonlijke repo:

        git clone https://[your GitHub user name]:[token]@github.com/<your GitHub user name>/azure-content-pr.git

Bijvoorbeeld deze opdracht kloon kan als volgt uitzien:

        git clone https://smithj:b428654321d613773d423ef2f173ddf4a312345@github.com/smithj/azure-content-pr.git  

## <a name="set-remote-repository-connection-and-configure-credentials"></a>Externe dataopslag verbinding instellen en configureren van referenties

Een verwijzing naar de hoofdmap opslagplaats maken door te voeren deze opdrachten. Deze verbindingen met de opslagplaats in GitHub ingesteld zodat u kunt ophalen van de meest recente wijzigingen op uw lokale computer en de wijzigingen terug te op GitHub duwen. Met deze opdracht ook de token lokaal zodanig geconfigureerd dat u hoeft niet uw naam en wachtwoord invoeren elke keer dat u probeert toegang te krijgen tot de upstream repo en uw vork op GitHub.

Openbare repo:

        cd azure-content
        git remote add upstream https://[your GitHub user name]:[token]@github.com/Azure/azure-content.git
        git fetch upstream

Persoonlijke repo:

        cd azure-content-pr
        git remote add upstream https://[your GitHub user name]:[token]@github.com/Azure/azure-content-pr.git
        git fetch upstream

Dit duurt meestal een tijdje. Nadat u dit doet, hoeft u niet te zich opnieuw splitsen of uw referenties opnieuw invoeren. U moet alleen de vorken naar een lokale computer kopiëren opnieuw als u de hulpprogramma's op een andere computer.


## <a name="configure-your-user-name-and-email-locally"></a>Configureer uw gebruikersnaam en e-lokaal

Zodat u goed als medewerker worden vermeld, moet u uw gebruikersnaam en e-mailadres lokaal in Git configureren.

1. Start Git Bash en overschakelen naar azure inhoud of azure-inhoud-pr:

   ````
   cd azure-content
   ````

 of

   ````
   cd azure-content-pr
   ````

2. Uw gebruikersnaam configureren zodat deze overeenkomt met uw naam zoals u deze in uw profiel GitHub instellen:

    ````
    git config --global user.name "John Doe"
    ````
3. Uw e-mailadres configureren zodat deze overeenkomt met de primaire e-mail die in uw profiel GitHub; Als u een werknemer MSFT, moet uw e-mailadres van MSFT:

    ````
    git config --global user.email "alias@example.com"
    ````
4. Type `git config -l` en bekijken van uw lokale instellingen om ervoor te zorgen de gebruikersnaam en e-mailadres in de configuratie correct zijn.

##<a name="next-steps"></a>Volgende stappen

- Het type inhoud waartoe de technische inhoud repo begrijpen en weten wat niet hoort. Zie de [richtlijnen voor inhoud kanaal](./content-channel-guidance.md)!
- Volg [deze stappen om te maken of wijzigen van een artikel en indienen voor publicatie](./git-commands-for-master.md).
- Kopieer [de sjabloon prijsverlaging](../markdown templates/markdown-template-for-new-articles.md) als basis voor een nieuw artikel.
- Gebruik [deze controlelijst om te controleren of uw aanvraag pull zal voldoen aan de kwaliteitscriteria](./contributor-guide-pr-criteria.md) voor het samenvoegen.


###<a name="contributors-guide-navigation"></a>Medewerkers handleiding voor navigatie

- [Overzichtsartikel](./../README.md)
- [Index van artikelen](./contributor-guide-index.md)



<!--Anchors-->
[Use a customer-friendly voice]: #use-a-customer-friendly-voice
[Consider localization and machine translation]: #consider-localization-and-machine-translation
[other style and voice issues to watch for]: #other-style-and-voice-issues-to-watch-for


[Create a GitHub account and set up your profile]: #create-a-github-account-and-set-up-your-profile
[Determine whether you really need to follow the rest of these steps]: #determine-whether-you-really-need-to-follow-the-rest-of-these-steps
[Permissions in GitHub]: #permissions-in-github
[Install Git for Windows]: #install-git-for-windows
[Enable two-factor authentication]: #enable-two-factor-authentication
[Install a markdown editor]: #install-a-markdown-editor
[Fork the repository and copy it to your computer]: #fork-the-repository-and-copy-it-to-your-computer
[Install git-credential-winstore]: #install-git-credential-winstore
[Sign up for Disqus]: #sign-up-for-disqus
[Configure your user name and email locally]: #configure-your-user-name-and-email-locally
[Next steps]: #next-steps
