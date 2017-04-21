# <a name="steps-to-follow-when-you-retire-or-change-the-name-of-an-acom-technical-article"></a>Volgende stappen uitvoeren als u wilt intrekken of wijzigen van de naam van een technisch artikel ACOM

Deze richtlijnen zijn voor KMO's die worden weergegeven als de auteur van een artikel dat moet worden ingetrokken van de technische documentatie sectie van azure.microsoft.com. De stappen zijn ook van toepassing als de naam van een bestand is gewijzigd.

Als u lid van onze community Azure bent en u denkt dat een artikel moet worden ingetrokken voor welke reden dan ook, laat u een opmerking in de stroom Disqus commentaar voor het artikel waarmee de auteur weten is iets mis met het artikel.

Auteurs van het MKB moeten als volgt verschillende inhoud zonder problemen verwijderen zodat gebruikers van de website niet een slechte ervaring wanneer we de inhoud van de site intrekken. Het artikel verwijderen of wijzigen van de naam moet het laatste wat er gebeurt!

## <a name="step-1-set-the-article-to-no-indexno-follow-and-republish-it-recommended"></a>Stap 1: Het artikel ingesteld op Nee index/niet volgen en opnieuw publiceren (aanbevolen)

Het eerste wat dat u moet doen is het artikel publiceren als geen index/Nee-volgen een paar weken voordat werkelijk verwijderen. Dit wordt beschouwd als de beste praktijken "voorafgaand werk" voor het intrekken van inhoud. Hierdoor verwijdert het artikel van zoekindexen engine zodat mensen het artikel niet in de zoekresultaten gevonden. [Zie de interne wiki voor meer informatie.](https://microsoft.sharepoint.com/teams/azurecontentguidance/wiki/Pages/Remove%20published%20pages%20and%20request%20redirects.aspx)

## <a name="step-2-manage-inbound-links-required"></a>Stap 2: Beheren van binnenkomende koppelingen (vereist)

Bepalen of er een niet-Microsoft links naar uw inhoud zijn. Vaak, blogs, forums en andere inhoud op het web verwijst naar artikelen. Vaak kunt u werken met blog eigenaren van deze koppelingen wijzigen en u kunt verwijderen of bijwerken van koppelingen van het forum posten. Web analytics tools kunnen u vertellen of er binnenkomende koppelingen die u wilt beheren op deze manier veel verkeer zijn.

## <a name="step-3-remove-all-crosslinks-to-the-article-from-the-technical-content-repository-required"></a>Stap 3: Alle kruiskoppelingen om het artikel te verwijderen uit de technische content repository (vereist)

Vertrouw niet op omleidingen naar kruiskoppelingen van andere artikelen. Bijwerken of verwijderen de cross-verwijzingen naar het artikel dat u wilt verwijderen of hernoemen, met inbegrip van de in de artikelen die eigendom zijn van andere mensen.

1. Ervoor te zorgen u werkt in een recente lokale vestiging – uitgevoerd `git pull upstream master` (of de juiste variatie op deze opdracht.

2.  Scan de azure-inhoud-pr/artikelen-map en de map azure-inhoud-pr/bevat voor alle artikelen en bevat de koppeling naar het artikel dat u wilt intrekken en de kruiskoppelingen van beide verwijderen of vervangen door een passende nieuwe kruiskoppelingen. U kunt een zoekopdracht gebruiken en vervangen hulpprogramma om de kruiskoppelingen vinden als u geïnstalleerd hebt. Als u dat niet doet, kunt u Windows PowerShell gratis! Hier is hoe de kruiskoppelingen vinden met PowerShell:

 een. Start Windows PowerShell.

 b. Bij de PowerShell-prompt naar de map azure-inhoud-pr\articles te wijzigen:

 `cd azure-content-pr\articles`

 c. Typ deze opdracht wordt een lijst met alle bestanden met een verwijzing naar het artikel dat u wilt verwijderen:

 `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name`

  U kunt desgewenst de lijst met namen te verzenden naar een tekstbestand (in dit geval, met de naam psoutput.txt):

  `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name | Out-File C:\Users\<your account>\psoutput.txt`

3. Toevoegen en alle wijzigingen doorvoeren en deze push naar uw vork een pull-aanvraag de wijzigingen van uw vork verplaatsen naar de master-vertakking van de belangrijkste opslagplaats maken.

## <a name="step-4-update-the-fwlink-tool-required"></a>Stap 4: De fwlink-verhaal over tool (vereist) bijwerken

Controleren of het hulpprogramma fwlink-verhaal over eventuele FWLinks die naar het artikel verwijzen mogelijk. Een FWLinks wijst op vervangende inhoud; Als u niet de eigenaar van de koppeling alias, lid van worden. Als de eigenaar kan de koppeling niet bijgewerkt, het bestand een ticket met MSCOM de koppeling gewijzigd. Meer info - [interne wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Manage%20inbound%20links%20to%20retired%20topics.aspx).

## <a name="step-5-remove-all-crosslinks-to-the-article-from-other-pages-on-azuremicrosoftcom-and-create-a-redirect-for-the-retired-page-if-appropriate-required"></a>Stap 5: Alle kruiskoppelingen het artikel van andere pagina's op azure.microsoft.com en maak een omleiding voor de teruggetrokken pagina indien nodig (vereist)

U hebt om te werken met de persoon die wordt bijgehouden en werkt de openingspagina van de documentatie voor uw service voor dit deel. Neem contact op met uw partner inhoud team als u niet weet wie die persoon is. De persoon die wordt bijgehouden en werkt de openingspagina doc moet twee dingen doen:

1. Scan de **hele** ACOM web-oplossing voor verwijzingen naar het bestand op te trekken in Visual Studio. De kruisverwijzingen verwijderen of vervangen door een bijgewerkte kruisverwijzing. U moet de HTML-koppelingen, alsmede de bijbehorende resource tekenreeksen voor de HTML-koppelingen te verwijderen. Meer info - Zie de [interne wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Create%20or%20edit%20a%20service%20landing%20page%20or%20left%20nav.aspx)

2. Als een vervangend artikel bestaat, maakt u een omleiding. Meer info - Zie de [interne wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Remove%20published%20pages%20and%20request%20redirects.aspx).

3. Controleer de wijzigingen in de opslagplaats.

## <a name="step-6-retire-the-article"></a>Stap 6: Het artikel intrekken

Nadat u de voorgaande stappen hebt uitgevoerd en deze wijzigingen live zijn, kunt u het artikel uit de bibliotheek verwijderen. 

**Belangrijk:** Wanneer u bestanden verwijdert, moet u de `git add --all` opdracht.

## <a name="step-7-remove-links-from-msdn-required"></a>Stap 7: Koppelingen verwijderen van MSDN (vereist)

Bekijk de inhoud QA-tool voor verbroken koppelingen naar het onderwerp ingetrokken of gewijzigd en fix/verwijderen de koppelingen in alle MSDN-onderwerpen waarin dit probleem optreedt.

## <a name="step-8-remove-cached-pages-from-search-engines-only-if-absolutely-necessary"></a>Stap 8: Verwijder cache pagina's van zoekmachines (alleen als dit absoluut noodzakelijk)

Doe dit alleen als de inhoud moet snel worden verwijderd vanwege juridische of ernstige problemen. Per beste praktijken van Google moeten gewoon normale prioriteit pagina verwijderingen worden verwerkt door natuurlijke search engine processen. Ga naar deze webpagina's in de cache opgeslagen webpagina's verwijderen uit zoekmachines:

[Bing](https://www.bing.com/webmaster/tools/content-removal?rflid=1)
[Google](https://www.google.com/webmasters/tools/removals?pli=1)


### <a name="contributors-guide-links"></a>Medewerkers handleiding voor koppelingen

- [Overzichtsartikel](./../README.md)
- [Index van artikelen](./contributor-guide-index.md)
