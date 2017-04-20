# <a name="quality-criteria-for-pull-request-review"></a>Kwaliteitscriteria voor pull-aanvraag controleren

Deze criteria zijn bedoeld voor auteurs die maken en beheren van technische artikelen en pull-aanvraag revisoren die inhoud pull aanvragen bekijken. Als u de pull-aanvraag komt niet in aanmerking voor het [automatisch samenvoegen](contributor-guide-pull-request-etiquette.md#in-a-hurry-submit-prs-that-can-be-accepted-automatically), zal deze worden beoordeeld door een revisor menselijke pull-aanvraag. Pull-aanvraag revisoren bekijken in het algemeen alleen wat nieuw of gewijzigd is. Pull-aanvraag revisoren evalueren de wijzigingen in een pull-aanvraag volgens de blokkerende en niet-blokkerende kwaliteit objecten bekijken die in dit artikel worden vermeld.

## <a name="blocking-content-quality-items"></a>Inhoud artikelen blokkeren

De updates van de pull-aanvraag moeten voldoen aan de volgende criteria moeten worden samengevoegd. Pull-aanvraag revisoren feedback geven in een pull-aanvraag opmerkingen voor deze artikelen en het type `#hold-off` in de pull-aanvraag met feedback terugbrengen naar u (de auteur).

| Categorie | Kwaliteit controleren item |
|----------|---------------------|
|Vereisten| De 'ready-naar-samenvoegen' en de labels 'gevalideerd' zijn toegewezen aan de PR.|
|Vereisten| De pull-aanvraag kan niet worden geblokkeerd door een conflict samenvoegen.|
|Repo-integriteit|    Pull-aanvraag bevat geen duidelijke inhoud behalen.|
|Repo-integriteit|    Pull-aanvraag bevat geen een ingesloten repo of ongebruikelijke, overbodige bestanden.|
|Repo-integriteit |Pull-aanvraag bevat minder dan 100 gewijzigde bestanden, tenzij de PR opzettelijk een vertakking van de release van model wordt bijgewerkt. (Echt, een PR moet bevatten veel minder dan dat, maar na 100 gewijzigde bestanden, GitHub de verschilbestanden wordt niet weergegeven).|
|Naamgeving |Bestandsnamen voor nieuwe bestanden voldoen aan de [richtlijnen voor naamgeving van bestanden](file-names-and-locations.md).|
|Naamgeving |Nieuwe mappen binnengebracht de repo-Volg de [richtlijnen voor naamgeving van map](file-names-and-locations.md#folder-names-in-the-repo).|
|Inhoud    |Het artikel is een technisch document, en daarom in het juiste kanaal inhoud. Zie de [Wat verder mag guidance](content-channel-guidance.md).|
|Inhoud    |Het onderwerp in het technische document is geschikt voor een technisch artikel. Zie de [Wat verder mag guidance](content-channel-guidance.md).|
|Inhoud    |Het artikel bevat een inleidende alinea en een procedurele of conceptuele lichaam van inhoud. Het artikel moet voldoende en volledige inhoud staan op de eigen als een artikel bevatten. Een klein stukje informatie moet niet worden. (Uitzondering: een onderwerp "Beperkingen" als in de context van een groot artikel met alle van de grenzen van de service.)|
|Inhoud| Elementen die genummerde lijsten worden moeten worden genummerd, elementen die moeten worden niet-geordend lijsten met opsommingstekens. Dit is de algemene bruikbaarheid.|
|Inhoud| Moeten worden ingevoerd met de potentiële klant PR revisor ongebruikelijke of nieuwe afbeeldingen, informatie-architectuur of structuren of uiteraard niet-standaard ontwerpen. Teams die met nieuwe dingen experimenteren moeten beschikken over een probleem/oplossing canvas of een plan voor het evalueren van experimenten.|
|Siteontwerp functionaliteit| Switchers worden alleen gebruikt voor switching op meerdere versies van hetzelfde artikel.|
|Siteontwerp functionaliteit| De titels van artikelen switchered bevatten informatie die zich onderscheidt van elk artikel van de andere artikelen in de switchered.|
|Siteontwerp functionaliteit| Handmatig geschreven zijn inhoudsopgaven niet toegestaan. Het artikel moet vertrouwen op H2s voor de inhoudsopgave op de pagina.|
|Siteontwerp functionaliteit| Als koppen H2 aanwezig zijn, wordt het artikel ten minste twee H2 koppen bevat. Met behulp van een H2-post wordt een enkel item artikel inhoudsopgave gemaakt. H2 koppen moeten worden gebruikt voordat H3 koppen te waarborgen dat wordt een inhoudsopgave gemaakt.|
|Prijsverlaging| HTML: Broninhoud geen HTML-code op het niveau van het blok: kleine inline HTML toegestaan – als superscript, subscript, speciale tekens en andere kleine dingen die u niet kunt met de prijsverlaging doen. HTML-tabellen zijn alleen toegestaan als de tabel bevat een lijst met opsommingstekens of genummerde lijsten, maar is meestal een indicatie dat de inhoud moet worden vereenvoudigd, zodat de bron kan worden gecodeerd in een prijsverlaging.|
|Prijsverlaging   |Prijsverlaging aangepaste elementen worden gebruikt waar nodig. Ex: Notities zijn gecodeerd met de AZURE. Opmerking uitbreiding niet als tekst zonder opmaak.|
|ZOEKMACHINEOPTIMALISATIE    |De "& #124; Microsoft Azure' site-id is vereist|
|ZOEKMACHINEOPTIMALISATIE    |De titel H1 bevat voldoende informatie om te beschrijven de inhoud van het artikel om het te onderscheiden van andere artikelen Azure en toewijzen aan klant waarschijnlijk trefwoorden. Bijvoorbeeld is "Overzicht" als titel H1 een fail.|
|Terminologie| Het gebruik van het acroniem ARM, V1 of V2 als verwijzingen naar de klassieke en implementatiemodellen Resource Manager is een blokkerende terminologie item.|
|Afbeeldingen |GIF-animaties worden niet geaccepteerd in de repo.|
|Afbeeldingen | Afbeeldingen hebben een duidelijke resolutie, vrij zijn van verkeerd gespelde woorden en bevatten geen persoonlijke gegevens | 
|Fasering|Voorbeeld van het artikel moet schoon op fasering. Het kan niet voor de hand liggende opmaak problemen bevatten: <br><br>-Een lijst met nummers of opsommingstekens die wordt weergegeven als een alinea <br> -Code in een codeblok die gedeeltelijk in het codeblok en gedeeltelijk buiten <br>-Genummerde stappen onjuist genummerd als gevolg van defecte inspringing|

## <a name="non-blocking-content-quality-items"></a>Niet-blokkerende artikelen van inhoud

Voor deze artikelen bieden pull-aanvraag revisoren feedback en instructies voor de auteur met de correcties in een pull-aanvraag later opvolgen. Deze feedback blokkeert echter niet de beschikking samen te voegen. Auteurs moeten binnen 3 werkdagen in correcties opvolgen.

| Categorie | Kwaliteit controleren item |
|----------|---------------------|
|Inhoud|Artikelen moeten hebben 'volgende stappen' met 1-3 relevante en aantrekkelijke volgende stappen. Korte tekst moet worden opgenomen die helpt bij de klant begrijpt waarom de volgende stappen zijn relevant. (Nieuwe artikelen alleen). Voorbeeld: <https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-install/><br>![](media/contributor-guide-pr-criteria/nextstepsexample.PNG)|
|Inhoud|Spelling, grammatica en andere problemen schrijven - pull-aanvraag revisoren kunnen feedback geven over een paar kleine problemen als niet-blokkerende feedback. Als er meer dan een paar problemen met redactionele, meld u revisoren voor het artikel voor het bewerken van een publicatie na een bewerkingsaanvraag.|
|Afbeeldingen|Afbeeldingen met de juiste toelichting stijl en kleur en screenshots gebruikt u de juiste notatie voor de rand en de tijdelijke aanduiding. [Zie de instructies van de afbeelding](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md).|
|Afbeeldingen|Afbeeldingen zijn alt-tekst. [Zie de instructies van de afbeelding](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md).|
|Siteontwerp functionaliteit|De koppen H2, als weergegeven in de inhoudsopgave op de pagina moeten in het ideale geval teruglopen aan niet meer dan 2 regels. Meer koppen moeilijker de inhoudsopgave van het artikel om te scannen.|
|Stijl verdragen|Alle titels en koppen zijn zin per Azure stijl.|
|Proces|Als de pull-aanvraag kan gemakkelijk opnieuw geconfigureerd om te profiteren van de automatisering van de PRmerger, feedback pull-aanvraag revisoren geven aan de auteur over het gebruik van takken, zodat de wijzigingen automatisch kunnen worden samengevoegd. Zie [het artikel PR etiquette](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-pull-request-etiquette.md#in-a-hurry-submit-prs-that-can-be-accepted-automatically).|
|Proces|Zorg bij het verwijderen of wijzig de naam van een artikel, volgt u de procedure. Pull-aanvraag revisoren moeten de volgende opmerking en een koppeling toevoegen in een Opmerking:<br><br>*Controleer of u het proces in de handleiding de inzenders gevolgd voor het verwijderen van artikelen: <https://github.com/Azure/azure-content/blob/master/contributor-guide/retire-or-rename-an-article.md> .*|

## <a name="related"></a>Gerelateerd

- [Pull-aanvraag etiquette en aanbevolen procedures voor Microsoft-medewerkers](contributor-guide-pull-request-etiquette.md)

- [Pull-aanvraag Opmerking automatisering](contributor-guide-pull-request-comments.md)
