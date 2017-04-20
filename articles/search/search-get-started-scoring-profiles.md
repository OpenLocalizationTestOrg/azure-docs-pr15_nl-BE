<properties 
    pageTitle="Het gebruik van scoring profielen in Azure zoeken | Microsoft Azure | De zoekservice hosted cloud" 
    description="Positie door middel van profielen scoren in Azure zoeken een zoekservice cloud gehost op Microsoft Azure search afstemmen." 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="mblythe" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

# <a name="how-to-use-scoring-profiles-in-azure-search"></a>Het gebruik van profielen score in Azure zoeken

Score profielen zijn een functie van Microsoft Azure Search die door aanpassen van de berekening van de puntenwaardering zoeken, beïnvloeden de manier waarop de items zijn gerangschikt in een lijst met zoekresultaten. U kunt zien van profielen als een manier om de relevantie van model, door de verhoging van de items die voldoen aan vooraf gedefinieerde criteria scoren. Stel bijvoorbeeld dat uw toepassing is een site online hotel reservering. Door versterking van de `location` veld zoekopdracht met een term zoals Seattle leidt tot hogere scores voor artikelen met Seattle in de `location` veld. Houd er rekening mee dat u of geen profiel met meer dan één score op alle hebt kunt als het standaard scoren voldoende is voor uw toepassing.

Om u te helpen experimenteren met score profielen, kunt u downloaden van een voorbeeldtoepassing die score profielen gebruikt om de volgorde rangschikken van zoekresultaten. Het monster is een consoletoepassing – misschien niet erg realistisch voor de ontwikkeling van praktische toepassingen – maar handig toch als een leerzaam boek. 

De voorbeeldtoepassing toont score gedrag met fictieve gegevens, de `musicstoreindex`. De eenvoud van het monster app kunt u gemakkelijk score profielen en query's wijzigen en vervolgens de onmiddellijke gevolgen voor rang order zien wanneer het programma wordt uitgevoerd.

<a id="sub-1"></a>
## <a name="prerequisites"></a>Vereisten

De voorbeeldtoepassing is geschreven in C# met behulp van Visual Studio 2013. Probeer de gratis [editie van Visual Studio 2013 Express](http://www.visualstudio.com/products/visual-studio-express-vs.aspx) als u al een exemplaar van Visual Studio niet hebt.

U moet een Azure-abonnement en een Azure Search-service voor het voltooien van de zelfstudie. Zie [maken van een zoekservice in de portal](search-create-service-portal.md) voor hulp bij het instellen van de service.

[AZURE. ZIJN [u een Azure-account nodig voor deze zelfstudie:](../../includes/free-trial-note.md)]

<a id="sub-2"></a>
## <a name="download-the-sample-application"></a>De voorbeeldtoepassing downloaden

Ga naar [Azure Search scoren profielen Demo](https://azuresearchscoringprofiles.codeplex.com/) op codeplex voor het downloaden van de voorbeeldtoepassing in deze zelfstudie wordt beschreven.

Klik op het tabblad broncode **downloaden** als u een zip-bestand van de oplossing. 

 ![][12]

<a id="sub-3"></a>
## <a name="edit-appconfig"></a>App.config bewerken

1. Nadat u de bestanden wilt uitpakken, opent u de oplossing in Visual Studio het configuratiebestand te bewerken.
1. Dubbelklik in de Solution Explorer op **app.config**. Dit bestand bevat het service-eindpunt en een `api-key` gebruikt voor de verificatie van de aanvraag. U kunt deze waarden verkrijgen via de klassieke Portal.
1. Aanmelden bij de [Azure Portal](https://portal.azure.com).
1. Ga naar het servicedashboard voor Azure zoeken.
1. Klik op de tegel **Eigenschappen** om de service-URL kopiëren
1. Klik op het vakje **sleutels** kopiëren de `api-key`.

Als u hebt de URL toe te voegen en `api-key` aan app.config, toepassingsinstellingen ziet er zo uit:

   ![][11]


<a id="sub-4"></a>
## <a name="explore-the-application"></a>Verken de toepassing

U bent bijna klaar om te bouwen en uitvoeren van de app, maar voordat u dit doet, kijk eens naar de JSON-bestanden gebruikt voor het maken en vullen van de index.

**Schema.JSON** definieert de index, met inbegrip van de score profielen die worden geplaatst in deze demo. Aankondiging van het schema worden alle velden die worden gebruikt in de index, met inbegrip van niet-doorzoekbare velden, zoals `margin`, die u kunt gebruiken in een score-profiel. Scoren profiel syntaxis wordt beschreven in [een zoekindex Azure score profiel toevoegen](http://msdn.microsoft.com/library/azure/dn798928.aspx).

**Data1 3.json** levert de gegevens, 246 albums via een handvol genres. De gegevens is een combinatie van werkelijke album en artiest informatie met fictieve velden zoals `price` en `margin` gebruikt ter illustratie van zoekopdrachten. De bestanden voldoen aan de index en worden geüpload naar uw Azure Search-service. Nadat de gegevens zijn geüpload en geïndexeerd, kunt u query's voor het uitgeven.

**Program.cs** worden de volgende bewerkingen uitgevoerd:

- Hiermee opent u het venster van een console.

- Maakt verbinding met Azure zoeken op basis van de URL en `api-key`.

- Hiermee verwijdert u de `musicstoreindex` als deze bestaat.

- Deze methode maakt een nieuw `musicstoreindex` met behulp van het bestand schema.json.

- De index met behulp van de gegevensbestanden gevuld.

- De index vier query's met een query. U ziet dat de score profielen worden opgegeven als een queryparameter. Alle query's zoeken 'beste' voor dezelfde periode. De eerste query wordt standaard scoren. De resterende drie query's gebruiken een score profiel.

<a id="sub-5"></a>
## <a name="build-and-run-the-application"></a>Bouwen en uitvoeren van de toepassing

Connectiviteit of assembly-verwijzing problemen uitsluit, bouwen en uitvoeren van de toepassing om ervoor te zorgen er zijn geen problemen eerst werken. Hier ziet u een consoletoepassing openen op de achtergrond. Alle vier query's worden uitgevoerd zonder onderbreking in de reeks. Op veel systemen is het volledige programma wordt uitgevoerd binnen 15 seconden. Als de consoletoepassing bevat een bericht met de melding 'Complete. Druk op enter om door te gaan", het programma is voltooid. 

Als u wilt vergelijken in query wordt uitgevoerd, kunt markeren, kopiëren, plakken de queryresultaten uit de console en plak deze in een Excel-bestand. 

De volgende afbeelding ziet u de resultaten van de eerste drie query's side-by-side. Alle query's gebruiken dezelfde zoekterm beste, die in talrijke albumtitels weergegeven.

   ![][10]

De eerste query gebruikt standaard scoren. Aangezien de zoekterm alleen in de albumtitels weergegeven en geen andere criteria is opgegeven, worden artikelen met 'beste' in de titel van het album in de volgorde waarin de search-service-id's geretourneerd. 

De tweede query gebruikt een score profiel, maar merk op dat het profiel geen invloed heeft. De resultaten zijn identiek aan die van de eerste query. Dit komt doordat het scorepatroon profiel vergroot een veld dat niet aan de query belangrijkste (genre). De zoekterm "beste" bestaat niet in een veld 'genre' van een document. Wanneer een score profiel geen invloed heeft, zijn de resultaten hetzelfde als standaard scoren.  

De derde query is het eerste bewijs van het profiel impact score. De zoekterm is nog steeds best zodat wij werken met dezelfde set van albums, maar omdat het scorepatroon profiel biedt aanvullende criteria waarmee 'score' en 'laatst bijgewerkt', sommige items worden aangedreven hoger in de lijst.

De volgende afbeelding ziet u de vierde en laatste query, versterkt door 'marge'. Het veld 'marge' is niet kan worden gezocht en kan niet worden geretourneerd in de zoekresultaten. De marge is handmatig toegevoegd aan het werkblad om te illustreren het punt dat items met hogere marges hoger in de lijst met zoekresultaten. 

   ![][9]

Nu dat u hebt geëxperimenteerd met score profielen te wijzigen van andere query-syntaxis gebruiken scoren profielen of uitgebreidere gegevens. Koppelingen in de volgende sectie vindt u meer informatie.

<a id="next-steps"></a>
## <a name="next-steps"></a>Volgende stappen

Meer informatie over profielen te scoren. Zie [een zoekindex Azure score profiel toevoegen](http://msdn.microsoft.com/library/azure/dn798928.aspx) voor meer informatie.

Meer informatie over het zoeken en de syntaxis van de query-parameters. Zie [Documenten zoeken (Azure Search REST API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) voor meer informatie.

Wilt u een stap terug en meer informatie over het maken van een index? [Bekijk deze video](http://channel9.msdn.com/Shows/Cloud+Cover/Cloud-Cover-152-Azure-Search-with-Liam-Cavanagh) de basisbeginselen.

<!--Anchors-->
[Prerequisites]: #sub-1
[Download the sample application]: #sub-2
[Edit app.config]: #sub-3
[Explore the application]: #sub-4
[Build and run the application]: #sub-5
[Next steps]: #next-steps

<!--Image references-->
[12]: ./media/search-get-started-scoring-profiles/AzureSearch_CodeplexDownload.PNG
[11]: ./media/search-get-started-scoring-profiles/AzureSearch_Scoring_AppConfig.PNG
[10]: ./media/search-get-started-scoring-profiles/AzureSearch_XLSX1.PNG
[9]: ./media/search-get-started-scoring-profiles/AzureSearch_XLSX2.PNG 