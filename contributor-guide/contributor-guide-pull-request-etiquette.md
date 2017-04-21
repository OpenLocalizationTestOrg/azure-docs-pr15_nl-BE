# <a name="pull-request-etiquette-and-best-practices-for-microsoft-contributors-to-azure-documentation"></a>Pull-aanvraag etiquette en best practices voor medewerkers van Microsoft Azure documentatie

Om wijzigingen te publiceren naar documentatie, indienen pull aanvragen u van uw vork. Elke pull-aanvraag heeft worden gecontroleerd voor het wordt samengevoegd. Lees dit artikel als u wilt weten hoe u moet werken met een pull-aanvraag revisoren en hoe kunt u pull-aanvragen die zijn eenvoudiger en sneller om te controleren, zodat de aanvraagwachtrij pull beter voor iedereen werkt.

## <a name="working-with-pull-request-reviewers"></a>Werken met een pull-aanvraag revisoren

Hier zijn de basisbeginselen u weten moet over het werken met een pull-aanvraag revisoren. 

- <b>Inzicht in de rol van de revisor pull-aanvraag. De revisor:</b>
  - Zorgt voor de algemene kwaliteit van de inhoud
  - Hiermee voorkomt u dat behalen in de bibliotheek
  - Feedback biedt voor het samenvoegen van

  Pull-aanvraag revisoren zijn in een rol content governance. Het primaire doel is niet om samen te voegen simpelweg wat zo snel mogelijk wordt ingediend. Verwachten feedback die moet worden bijgewerkt, met name voor nieuwe en sterk gewijzigde artikelen.

- <b>Vooruit te plannen met de revisor pull-aanvraag:</b>
  - Voor hoge prioriteit pull aanvragen
  - Pull-aanvragen voor getimede/gedateerd releases
  - Pull-aanvragen dat veel bestanden toevoegen of wijzigen

- <b>SLA voor pull-aanvraag controleren</b>

  In de persoonlijke bibliotheek wil telkens wanneer die de pull-aanvraag de aanvraagwachtrij pull met het label klaar wilt samenvoegen, voert het team de pull-aanvraag binnen 12 uur van business (M-F, 8 AM tot 5 PM) bekijken en feedback of samenvoegen als u geen feedback is vereist. Deze SLA is van toepassing op de handeling van het controleren van de PR, niet samen te voegen. Wanneer zij voldoen aan [de criteria voor het samenvoegen van](contributor-guide-pr-criteria.md)worden PRs samengevoegd. 

## <a name="make-the-pull-request-queue-work-better-for-everyone"></a>Controleer de pull aanvraagwachtrij werken beter voor iedereen

Er zijn twee fundamentele realiteit in de wachtrij PR:

- Pull-aanvragen die in het bereik zijn klein en bevatten erg veel wijzigingen nemen minder tijd om te bekijken. 
- Pull-aanvragen die zijn groot in omvang of met andere, verschillende soorten wijzigingen duren langer om te controleren.

U kunt helpen de pull aanvraagwachtrij beter samen te werken door deze aanbevolen procedures te volgen:

- Afzonderlijke kleine updates van bestaande artikelen van nieuwe artikelen of grote herschreven. Deze wijzigingen in afzonderlijke werken bedrijfstakken werken. 

- Wanneer u artikelen of afbeeldingen verwijderen, de verwijdering niet mengen met nieuwe inhoud toevoegen of updates. Verwerken van de wijzigingen en de nieuwe inhoud in een aparte tak van de werken.

- Voor versies of refactoring van inhoud, vooruit te plannen met uw PR revisor. Mogelijk moet u zijn of haar hulp voor het maken van een bijkantoor release of keer samenvoegen met de tijden publiceren coördineren, zodat de inhoud wordt gepubliceerd op het juiste moment.

- Als u probeert wijzigingen in de ACOM repo (ie, wijzigingen in de navigatiebalk aan de linkerkant, landingspagina's, omleidingen of leren kaarten) met de wijzigingen die u in de opslagplaats azure-inhoud-pr aanbrengt coördineren, moet u dat werk tevoren samenwerken met uw PR revisor. Anders loopt u het risico dat een groot aantal verbroken koppelingen.

## <a name="criteria-for-expedited-pull-requests"></a>Criteria voor de versnelde pull aanvragen

- Neem contact op met azdocprs PRs alleen als dat absoluut noodzakelijk te versnellen. U kunt aanvragen afgehandeld PR verwerking voor rode Zone, privacy, legal en beveiligingsproblemen; voor echt gebroken klantervaringen; en executive doorverwijzingen. 
- Inhoud voor versies van de functie niet in aanmerking voor het verwerken van versnelde - functie release inhoud vereist de voorafgaande planning of deze moet worden verwerkt door de prioriteit standaard wachtrij.


## <a name="in-a-hurry-submit-prs-that-can-be-accepted-automatically"></a>Hebt u haast? Indienen van PRs die automatisch worden geaccepteerd

PRMerger regels voor automatisering gebruiken om meer van uw dagelijkse PRs automatisch samengevoegd.

PRMerger accepteert uw PR automatisch, als:
* Heeft dit gevolgen voor 10 bestanden of minder.
* Het bevat doorvoeracties voor 15 of minder.
* Minder dan 20% van de tekst wijzigen.
* Selectoren/switchers niet worden bijgewerkt.
* Er zijn geen bestanden zijn verwijderd of toegevoegd.
* Er zijn geen afbeeldingen nieuw, gewijzigd of verwijderd.

Als de pull-aanvraag niet voldoet aan deze criteria, wordt het label "PRmerger kan niet samenvoegen" automatisch toegewezen zodat u weet dat het moet worden gecontroleerd door een menselijke PR revisor.

### <a name="need-to-make-a-lot-of-little-changes"></a>Moet een groot aantal kleine wijzigingen aanbrengen?

Nemen de hint van de bovenstaande regels voor PRMerger automatisering en het volgende doen:
* Indienen van artikelen met lichte wijzigingen samen in een PR met 10 of minder bestanden.
* Maak een afzonderlijke PR voor artikelen in welke afbeeldingen of selectors wijzigen. Hiervoor menselijke controle.
* Maak een afzonderlijke PR voor nieuwe of verwijderde artikelen. Hiervoor menselijke controle.

## <a name="related"></a>Gerelateerd

- [Kwaliteitscriteria voor pull-aanvraag controleren](contributor-guide-pr-criteria.md)

- [Pull-aanvraag Opmerking automatisering](contributor-guide-pull-request-comments.md)
