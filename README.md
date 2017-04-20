# <a name="azure-technical-documentation-contributor-guide"></a>Technische documentatie Azure Inzender gids

U kunt de GitHub-bibliotheek die de bron van de technische documentatie die is gepubliceerd in het midden van Azure documentatie op [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation)hebt gevonden.

Deze opslagplaats bevat ook richtlijnen u bijdragen tot onze technische documentatie.  Zie voor een overzicht van de artikelen in de handleiding de medewerkers, [de index](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).

## <a name="contribute-to-azure-documentation"></a>Bijdragen tot de Azure documentatie

Bedankt voor uw interesse in Azure documentatie.

* [Manieren om bij te dragen](#ways-to-contribute)
* [Gedragscode](#code-of-conduct)
* [Over uw bijdragen tot Azure inhoud](#about-your-contributions-to-azure-content)
* [Organisatie van de opslagplaats](#repository-organization)
* [Gebruik van GitHub, Git en deze opslagplaats](#use-github-git-and-this-repository)
* [Prijsverlaging gebruiken voor het opmaken van uw onderwerp](#how-to-use-markdown-to-format-your-topic)
* [Feedback, opmerkingen en ondersteuning](./contributor-guide/feedback-and-comments.md)
* [Meer bronnen](#more-resources)
* [Index van alle coauteurs handleiding artikelen](./contributor-guide/contributor-guide-index.md) (nieuwe pagina wordt geopend)

## <a name="ways-to-contribute"></a>Manieren om bij te dragen 

U kunt bijdragen tot [Azure documentatie](http://azure.microsoft.com/documentation/) in een aantal verschillende manieren:

* Bijdragen aan een [discussie forum](http://social.msdn.microsoft.com/Forums/windowsazure/home).
* Disqus opmerkingen onderaan in de artikelen verzenden.
* U kunt gemakkelijk bijdragen tot technische artikelen in de gebruikersinterface van GitHub. Het artikel van deze opslagplaats, zoeken of Ga naar het artikel op [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation) en klik op de link in het artikel naar de GitHub bron voor het artikel.
* Als u ingrijpende wijzigingen aan een bestaand artikel aanbrengen wilt, moet toevoegen of wijzigen van afbeeldingen of een nieuw artikel bijdragen u deze opslagplaats zich splitsen, installeren van Git Bash, prijsverlaging Pad, en informatie over bepaalde opdrachten git.

##<a name="code-of-conduct"></a>Gedragscode

Dit project heeft vastgesteld, de [Microsoft Open Source van de gedragscode](https://opensource.microsoft.com/codeofconduct/). Zie voor meer informatie [De Code van voeren Veelgestelde vragen](https://opensource.microsoft.com/codeofconduct/faq/) of contact [opencode@microsoft.com](mailto:opencode@microsoft.com) met extra vragen of opmerkingen.

##<a name="about-your-contributions-to-azure-content"></a>Over uw bijdragen tot Azure inhoud

###<a name="minor-corrections"></a>Kleine correcties

U voor documentatie en code-voorbeelden in deze repo indienen verduidelijkingen of kleine correcties worden gedekt door de [Azure Website gebruiksvoorwaarden (gebruiksvoorwaarden)](http://azure.microsoft.com/support/legal/website-terms-of-use/).


###<a name="larger-submissions"></a>Grotere inzendingen

Als u met nieuwe of belangrijke wijzigingen in de documentatie en voorbeelden van code een pull-aanvraag indient, sturen we een opmerking in de GitHub waarin u een on line bijdrage licentie overeenkomst (CLA) verzenden als u in een van deze groepen zijn:

* Leden van de groep openen Microsoft-technologieën.
* Medewerkers die niet voor Microsoft werken.

Moeten we u het on line formulier voltooien voordat we de pull-aanvraag kunt accepteren.

Alle details zijn beschikbaar op [http://azure.github.io/guidelines/#cla](http://azure.github.io/guidelines/#cla).

## <a name="repository-organization"></a>Organisatie van de opslagplaats

De inhoud van de opslagplaats azure inhoud volgt de organisatie van de documentatie op [Azure.Microsoft.com](http://azure.microsoft.com). Deze opslagplaats bevat twee hoofdmappen:

### <a name="articles"></a>\articles

De map *\articles* bevat de documentatie artikelen opgemaakt als bestanden met de extensie *.md* prijsverlaging.

Artikelen in de hoofdmap worden gepubliceerd naar de Azure.Microsoft.com in het pad *http://azure.microsoft.com/documentation/articles/ {artikel-naam-zonder-md} /*.

* **Artikel bestandsnamen:** Raadpleeg [onze richtlijnen voor naamgeving](./contributor-guide/file-names-and-locations.md).

Artikelen in hun eigen servicemap worden gepubliceerd naar de Azure.Microsoft.com in het pad *http://azure.microsoft.com/documentation/articles/service-folder/ {artikel-naam-zonder-md} /*

* **Submappen Media:** De map *\articles* bevat de map *\media* voor root directory artikel media-bestanden in submappen met de afbeeldingen voor elk artikel zijn.  De service mappen bevatten een afzonderlijke media-map voor de artikelen in elke servicemap. De afbeelding artikel mappen dezelfde naam hebben als het bestand artikel, verminderd met de bestandsextensie *.md* .

### <a name="includes"></a>\Includes

U kunt herbruikbare inhoud secties moeten worden opgenomen in een of meer artikelen maken. Zie [aangepaste extensies in onze technische inhoud gebruikt](./contributor-guide/custom-markdown-extensions.md).

### <a name="markdown-templates"></a>sjablonen voor \markdown

Deze map bevat onze standaard prijsverlaging sjabloon met de elementaire prijsverlaging opmaak die u nodig hebt voor een artikel.

### <a name="contributor-guide"></a>\contributor-Guide

Deze map bevat artikelen die deel van onze medewerkers handleiding uitmaken.  

## <a name="use-github-git-and-this-repository"></a>Gebruik van GitHub, Git en deze opslagplaats

Zie voor meer informatie over hoe te dragen, het gebruik van de UI GitHub te dragen kleine veranderingen en zich splitsen en klonen van de opslagplaats voor meer belangrijke bijdragen [installeren en instellen van hulpprogramma's voor het schrijven in de GitHub](./contributor-guide/tools-and-setup.md).

Als u GitBash installeert en u om lokaal te werken, de stappen voor het maken van een nieuw lokaal werkende bijkantoor, wijzigingen aanbrengen en indienen van dat de wijzigingen weer met de belangrijkste tak staan in [Git opdrachten voor een nieuw artikel maken of bijwerken van een bestaand artikel](./contributor-guide/git-commands-for-master.md)

### <a name="branches"></a>Bijkantoren

Het is raadzaam om lokaal werkende bijkantoren die gericht zijn op een bepaald bereik van de wijziging te maken. Elke tak moet worden beperkt tot één begrip/artikel zowel aan de workflow te stroomlijnen en verminderen de kans op conflicten bij het samenvoegen.  De volgende inspanningen zijn van het juiste bereik voor een nieuwe tak:

* Een nieuw artikel (en de bijbehorende afbeeldingen)
* Spelling en grammatica van de afbeelding op een artikel.
* Toepassen van een opmaakwijziging enkel over een groot aantal artikelen (bijvoorbeeld nieuwe copyright voettekst).

## <a name="how-to-use-markdown-to-format-your-topic"></a>Prijsverlaging gebruiken voor het opmaken van uw onderwerp

Alle artikelen in deze bibliotheek gebruiken GitHub flavored prijsverlaging.  Hier volgt een lijst met resources.

- [Grondbeginselen van prijsverlaging](https://help.github.com/articles/markdown-basics/)

- [Prijsverlaging afdrukbare Cheatsheet voor](./contributor-guide/media/documents/markdown-cheatsheet.pdf?raw=true)

- Zie de [hulpprogramma's en instellingen onderwerp](./contributor-guide/tools-and-setup.md#install-a-markdown-editor)voor onze lijst van prijsverlaging editors.

## <a name="article-metadata"></a>Artikel metagegevens

Artikel metagegevens kan bepaalde functies op de website van azure.microsoft.com, zoals auteur attribution attribution Inzender, broodkruimels, artikel omschrijvingen en Zoekmachineoptimalisatie optimalisaties zoals Microsoft rapportage wordt gebruikt voor de evaluatie van de prestaties van de inhoud. De metagegevens is dus belangrijk! [Hier volgt de richtlijnen voor de zorg dat de metadata recht wordt gedaan](./contributor-guide/article-metadata.md).

### <a name="labels"></a>Labels

Automatische labels zijn toegewezen aan de pull-verzoeken ons de pull-aanvraag werkstroom beheren en te helpen zodat u weet wat er gebeurt met de pull-aanvraag:

* Bijdrage overeenkomst betrekking
    * CLA niet nodig: de wijziging is relatief klein en geen vereist dat u zich een CLA aanmeldt.
    * CLA vereist: het bereik van de wijziging is relatief groot en is vereist dat u zich een CLA aanmeldt.
    * CLA is ondertekend: de medewerker de CaO, ondertekend zodat de pull-aanvraag nu voor revisie vooruit kan.
* Etiketten voor pijler: Labels, zoals PnP, moderne Apps en TDC te categoriseren met de pull-aanvragen door de interne organisatie die nodig zijn om te controleren de pull-aanvraag.
* Wijziging verzonden naar de auteur: de auteur is gesteld van de aanvraag in behandeling pull.

## <a name="more-resources"></a>Meer bronnen

Zie de [index van onze medewerker gids](./contributor-guide/contributor-guide-index.md) voor alle onderwerpen van onze richtlijnen.
