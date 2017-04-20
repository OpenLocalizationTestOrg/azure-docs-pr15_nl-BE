<properties
    pageTitle="Gebruik de steekproeven in Machine Learning Studio | Microsoft Azure"
    description="Beschrijvingen van de gegevens die wordt gebruikt in modellen van monster in ML Studio opgenomen. U kunt deze steekproeven voor uw experimenten."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="garye"/>


# <a name="use-the-sample-data-sets-in-azure-machine-learning-studio"></a>Gebruik de steekproeven in Azure Machine Learning Studio

[top]: #machine-learning-sample-datasets

Wanneer u een nieuwe werkruimte in Azure Machine Learning maken, zijn standaard een aantal steekproeven en experimenten opgenomen. Veel van deze steekproeven worden gebruikt door de modellen van het monster in de [Galerie met Cortana Intelligence Azure](http://gallery.cortanaintelligence.com/)en anderen worden opgenomen als voorbeelden van verschillende typen gegevens die meestal wordt gebruikt in de machine learning.

Sommige gegevens zijn beschikbaar in Azure Blob-opslag. Voor deze sets met gegevens biedt in de onderstaande tabel een rechtstreekse koppeling. U kunt deze sets gegevens in uw experimenten met behulp van de [Gegevens importeren] [ import-data] module.

De rest van deze steekproeven worden vermeld onder **Datasets opgeslagen** in het palet van de module aan de linkerkant van het experiment canvas wanneer u open of maak een nieuw experiment in ML Studio.
U kunt een van deze sets gegevens in uw eigen experiment door deze te slepen om uw canvas experiment.


<!--
For a list of sample experiments available in ML Studio, see [Machine Learning Sample Experiments][sample-experiments].

[sample-experiments]: machine-learning-sample-experiments.md
-->

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


<table>

<tr>
  <th align=left>Naam gegevensset</th>
  <th align=left>Beschrijving van de DataSet</th>
</tr>

<tr>
  <td valign=top>Volwassen Census inkomsten binaire indeling dataset</td>
  <td valign=top>
Een subset van de 1994 Census-database met werkende volwassenen ouder dan 16 met een index van de gecorrigeerde winst > 100.<p> </p><b>Syntaxis:</b> classificeren mensen die demografische gegevens gebruiken om te voorspellen of een persoon meer dan 50K per jaar verdient.<p> </p><b>Onderzoek:</b> Kohavi, R., Becker, B., (1996). UCI Machine opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>leren. Irvine, CA: University of California, School van informatie en computerwetenschappen </td>
</tr>

<tr ID=airport-codes-dataset>
  <td valign=top>Luchthaven Codes Dataset</td>
  <td valign=top>
Amerikaanse luchthaven codes.<p> </p>Deze dataset bevat één rij voor elke luchthaven VS, bieden de luchthaven id-nummer en de naam en de locatie stad en staat.
  </td>
</tr>

<tr>
  <td valign=top>Auto prijsgegevens (Raw)</td>
  <td valign=top>
Informatie over auto door automerk en model, met inbegrip van de prijs, functies, zoals het aantal cilinders en MPG, alsmede een risicoscore verzekering.<p> </p>De risicoscore is in eerste instantie gekoppeld aan de prijs voor automatisch en vervolgens aangepast voor werkelijke risico in een proces dat bekend is dat actuarissen als symboling. Een waarde van + 3 geeft aan dat de auto riskant is en een waarde van -3 dat het waarschijnlijk heel veilig wordt.<p> </p><b>Syntaxis:</b> voorspellen de score van de risico's door functies, met behulp van regressie of multivariate-indeling. <p> </p><b>Onderzoek:</b> Schlimmer, J.C. (1987). UCI Machine opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>leren. Irvine, CA: University of California, School van informatie en computerwetenschappen </td>
</tr>

<tr ID=bike-rental-uci-dataset>
  <td valign=top>Fiets verhuur UCI dataset</td>
  <td valign=top>
UCI fiets verhuur dataset die is gebaseerd op werkelijke gegevens van kapitaal Bikeshare bedrijf dat een netwerk van fiets verhuur in Washington DC onderhoudt.<p> </p>De dataset bevat één rij voor elk uur van elke dag in 2011 en 2012, voor een totaal van 17,379 rijen. Het bereik van per uur fiets huren is van 1 977.

  </td>
</tr>

<tr ID=bill-gates-rgb-image>
  <td valign=top>Bill Gates RGB-afbeelding</td>
  <td valign=top>
Openbaar beschikbare bestand geconverteerd naar CSV-gegevens.<p> </p>De code voor het converteren van de afbeelding is opgenomen in de detailpagina model <strong>kleur quantization met clustering K weg</strong> .
  </td>
</tr>

<tr>
  <td valign=top>Bloed donatie gegevens</td>
  <td valign=top>
Een subset van gegevens uit de database van de donor bloed voor de transfusie van bloed Service Center van Hsin Chu City, Taiwan.<p> </p>Donateur gegevens omvatten de maanden sinds de laatste donatie), en frequentie, of het totale aantal donaties, tijd sinds de laatste schenking, en hoeveelheid bloed geschonken.<p> </p><b>Syntaxis:</b> het doel is om via de classificatie te voorspellen of de donor gedoneerd bloed in maart 2007, waarbij 1 staat voor een donor tijdens de doelperiode en 0 niet donor. <p> </p><b>Onderzoek:</b> Yeh, I.C., (2008). UCI Machine opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>leren. Irvine, CA: University of California, School van informatie en computerwetenschappen <p> </p>Yeh, I-Cheng, Yang, koning-Jang en Ting, Tao-Ming "Knowledge discovery op RFM model Bernoulli-reeks met ' Expert-systemen met toepassingen, 2008, <a href="http://dx.doi.org/10.1016/j.eswa.2008.07.018">http://dx.doi.org/10.1016/j.eswa.2008.07.018</a>
  </td>
</tr>

<tr ID=book-reviews-from-amazon>
  <td valign=top>Boek recensies van Amazon</td>
  <td valign=top>
Recensies van boeken in de Amazon, genomen uit de amazon.com-website door onderzoekers van de universiteit van Pennsylvania (<a href="http://www.cs.jhu.edu/~mdredze/datasets/sentiment/">sentiment</a>). Zie het onderzoeksrapport ' biografieën, Bollywood, giek dozen en Blenders: domein aanpassing voor Sentiment classificatie "door John Blitzer, Mark Dredze en Fernando Pereira; Vereniging van Computational Linguistics (ACL), 2007.<p> </p>De originele dataset bevat 975K beoordelingen met classificatie 1, 2, 3, 4 of 5. De beoordelingen zijn geschreven in het Engels en zijn uit de periode 1997-2007. Deze dataset is gedownsampled naar 10K beoordelingen.
  </td>
</tr>

<tr>
  <td valign=top>Borst kanker gegevens</td>
  <td valign=top>
Een van de drie kanker gerelateerde datasets verstrekt door het instituut voor Oncology die vaak in de machine learning literatuur verschijnt. Een combinatie van diagnostische gegevens met de functies van de laboratoriumanalyses van ongeveer 300 monsters van weefsel.<p> </p><b>Syntaxis:</b> classificeren van het type van kanker, op basis van de 9 kenmerken, waarvan sommige zijn lineaire en andere bestaan. <p> </p><b>Onderzoek:</b> O.L. Wohlberg, W.H., straat, W.N. en Mangasarian, (1995). UCI Machine opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>leren. Irvine, CA: University of California, School van informatie en computerwetenschappen </td>
</tr>

<tr ID=breast-cancer-features>
  <td valign=top>Borst kanker functies <td valign=top>
De dataset bevat informatie voor 102K verdachte gebieden (gegadigden) van röntgenstraling afbeeldingen, door 117 functies beschreven. De functies zijn eigen en hun betekenis, niet worden bekendgemaakt door de makers van de dataset (Siemens Healthcare). 
  </td>
</tr>

<tr ID=breast-cancer-info>
  <td valign=top>Borst kanker Info</td>
  <td valign=top>
De dataset bevat extra informatie voor elke regio verdachte van röntgenstraling afbeelding. Elk voorbeeld bevat informatie (b.v., label, patiënt-ID, de coördinaten van de patch ten opzichte van de hele afbeelding) over het nummer van de overeenkomstige rij in de gegevensset borst kanker functies. Elke patiënt heeft een aantal voorbeelden. Voor patiënten die een kanker hebben, enkele voorbeelden zijn positief en andere negatief. Alle voorbeelden zijn voor patiënten die niet beschikken over een kanker, negatief. De dataset is 102K voorbeelden. De dataset vertekende 0,6% van de punten positief zijn, de rest negatief zijn. De dataset is beschikbaar gesteld door Siemens Healthcare.
  </td>
</tr>

<tr ID=crm-appetency-labels-shared>
  <td valign=top>CRM Appetency Labels gedeeld</td>
  <td valign=top>
Labels van KDD Cup 2009 klant relatie voorspelling challenge (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_appetency.labels">orange_small_train_appetency.labels</a>).
  </td>
</tr>

<tr ID=crm-churn-labels-shared>
  <td valign=top>CRM lospeuteren Labels gedeeld</td>
  <td valign=top>
Labels van KDD Cup 2009 klant relatie voorspelling challenge (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_churn.labels">orange_small_train_churn.labels</a>).
  </td>
</tr>

<tr ID=crm-dataset-shared>
  <td valign=top>CRM Dataset gedeeld</td>
  <td valign=top>
Deze gegevens zijn afkomstig van KDD Cup 2009 klant relatie voorspelling challenge (<a href="http://www.sigkdd.org/kdd-cup-2009-customer-relationship-prediction - orange_small_train.data.zip">orange_small_train.data.zip</a>).<p> </p>De dataset bevat 50K klanten uit het Frans Telecom bedrijf oranje. Elke klant heeft 230 anoniem functies, 190 waarvan numeriek zijn en 40 zijn bestaan. De functies zijn zeer verspreid.
  </td>
</tr>

<tr ID=crm-upselling-labels-shared>
  <td valign=top>CRM Upselling Labels gedeeld</td>
  <td valign=top>
Labels van KDD Cup 2009 klant relatie voorspelling challenge (<a href="http://www.sigkdd.org/site/2009/files/orange_large_train_upselling.labels">orange_large_train_upselling.labels</a>).
  </td>
</tr>

<tr>
  <td valign=top>Energie-efficiëntie regressie gegevens</td>
  <td valign=top>
Een collectie van gesimuleerde energie profielen, op basis van 12 verschillende gebouwen vormen. De gebouwen verschillen met betrekking tot gedifferentieerd door 8 functies, zoals het gebied, de ruiten gebied distributie en afdrukstand voor ruiten.<p> </p><b>Syntaxis:</b> regressie of classificatie gebruiken om te voorspellen van de energie-efficiëntie als een van de twee reacties van reële waarden gebaseerde beoordeling. Voor meerdere klasse-indeling is ronde de variabele reactie op het dichtstbijzijnde gehele getal. <p> </p><b>Onderzoek:</b> Xifara, A. & Tsanas, A. (2012). UCI Machine opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>leren. Irvine, CA: University of California, School van informatie en computerwetenschappen </td>
</tr>

<tr ID=flight-delays-data>
  <td valign=top>Vlucht vertraagd gegevens</td>
  <td valign=top>
Passagier vlucht op tijd prestatiegegevens opgehaald uit de verzameling van het TranStats van het Amerikaanse Department of Transportation (<a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">Op tijd</a>).<p> </p>De dataset dekt de periode April-oktober 2013. Voordat u deze uploadt naar Azure ML Studio, is de dataset als volgt verwerkt:<ul><li>De dataset is gefilterd, zodat alleen de 70 drukste luchthavens in de continentale Verenigde Staten te dekken</li><li>Geannuleerde vluchten werden bestempeld als door meer dan 15 minuten vertraagd</li><li>Omgereden vluchten zijn gefilterd.</li><li>De volgende kolommen zijn geselecteerd: jaar, maand, DayofMonth, dag van de week, vervoerder, OriginAirportID, DestAirportID, CRSDepTime, DepDelay, DepDel15, CRSArrTime, ArrDelay, ArrDel15, geannuleerd</li></ul>
</td>
</tr>

<tr>
  <td valign=top>Vlucht op tijd prestaties (Raw)</td>
  <td valign=top>
Records van vliegtuig vlucht aankomsten en vertrekken in de Verenigde Staten van oktober 2011.<p> </p><b>Syntaxis:</b> flight vertragingen voorspellen. <p> </p><b>Onderzoek:</b> van de Amerikaanse afdeling van transport <a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time</a>.
  </td>
</tr>

<tr>
  <td valign=top>Forest gegevens wordt gestart</td>
  <td valign=top>
Bevat gegevens over weersomstandigheden, zoals temperatuur en vochtigheid indices en windsnelheid uit een gebied in het noordoosten Portugal, gecombineerd met records van bosbranden.<p> </p><b>Syntaxis:</b> dit is een regressie moeilijke taak, waarbij het doel is te voorspellen de gebrande gebied van bosbranden. <p> </p><b>Onderzoek:</b> Cortez, P., & Morais, A. (2008). UCI Machine opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>leren. Irvine, CA: University of California, School van informatie en computerwetenschappen <p> </p>[Cortez en Morais, 2007] P. Cortez en A. Morais. Een Data Mining aanpak voorspellen bosbranden met meteorologische gegevens. In J. Neves, M. F. Santos en J. Machado, Eds., nieuwe Trends in de kunstmatige intelligentie, Proceedings 13e EPIA 2007 - Portugese Conferentie over kunstmatige intelligentie, December, Guimarães, Portugal, pp. 512-523, 2007. APPIA, 13-ISBN 978-989-95618-0-9. Beschikbaar op: <a href="http://www.dsi.uminho.pt/~pcortez/fires.pdf">http://www.dsi.uminho.pt/~pcortez/fires.pdf</a>.
  </td>
</tr>

<tr ID=german-credit-card-uci-dataset>
  <td valign=top>Duits creditcard UCI dataset</td>
  <td valign=top>
De dataset ICB Statlog Duitse creditcard (<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">Statlog + Duits + Credit + gegevens</a>) met behulp van het bestand german.data.<p> </p>De dataset classificeert mensen, door een reeks kenmerken als laag of hoog kredietrisico's beschreven. Elk voorbeeld voorstelt een persoon. Er zijn 20 functies bestaan, zowel numerieke en een binaire label (krediet risico waarde). Hoog-risico creditposten label hebben = 2, risico's van lage creditposten label hebben = 1. De kosten van onjuiste voorbeeld van een laag risico zo hoog is 1, terwijl de kosten van een voorbeeld met een hoog risico zo klein onjuiste 5.
  </td>
</tr>

<tr ID=imdb-movie-titles>
  <td valign=top>Filmtitels IMDB</td>
  <td valign=top>
De dataset bevat informatie over de films die zijn geclassificeerd in Twitter tweets: IMDB movie-ID, de naam van de film en genre, jaar van productie. Er zijn 17K films in de dataset. De dataset is geïntroduceerd in de white paper 'S. Dooms, T. De Pessemier en L. Martens. MovieTweetings: een film score Dataset verzameld via Twitter. Workshop over Crowdsourcing en menselijke berekening voor Recommender systemen, CrowdRec op RecSys 2013."
  </td>
</tr>

<tr>
  <td valign=top>IRIS twee klassegegevens</td>
  <td valign=top>
Dit is misschien wel de meest bekende database worden gevonden in de literatuur patroon herkenning. De gegevensset is relatief klein, met 50 voorbeelden van metingen van drie rassen van iris Bloemblad.<p> </p><b>Syntaxis:</b> het type iris van de metingen te voorspellen.  <p> </p><b>Onderzoek:</b> Fisher, R.A. (1988). UCI Machine opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>leren. Irvine, CA: University of California, School van informatie en computerwetenschappen </td>
</tr>

<tr ID=movie-tweets>
  <td valign=top>Tweets van film</td>
  <td valign=top>
De dataset is een uitgebreide versie van de film Tweetings dataset. De dataset scoort 170K voor films, gewonnen uit gestructureerde tweets op Twitter. Elk exemplaar vertegenwoordigt een tweet en is een tupel: gebruikers-ID, IMDB movie-ID, beoordeling, tijdstempel, aantal favorieten voor deze tweet, en een aantal retweets van deze tweet. De dataset is beschikbaar gesteld door A. zei, S. Dooms, Loni B. en D. Tikk voor Recommender systemen Challenge 2014.
  </td>
</tr>

<tr>
  <td valign=top>MPG-gegevens voor verschillende auto</td>
  <td valign=top>
Deze dataset is een enigszins gewijzigde versie van de dataset die door de bibliotheek StatLib van Carnegie Mellon University. De dataset is gebruikt in het 1983 American statistische Association.<p> </p>De gegevens worden brandstofverbruik voor diverse Auto in mijl per gallon met gegevens die het aantal cilinders, motor verplaatsing paardenkracht, totale gewicht en versnelling.<p> </p><b>Syntaxis:</b> voorspellen op basis van de afzonderlijke kenmerken van 3 met meerdere waarden en 5 continue kenmerken brandstofverbruik. <p> </p><b>Onderzoek:</b> StatLib, Carnegie Mellon University (1993). UCI Machine opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>leren. Irvine, CA: University of California, School van informatie en computerwetenschappen </td>
</tr>

<tr>
  <td valign=top>Pima Indianen Diabetes binaire indeling dataset</td>
  <td valign=top>
Een subset van gegevens uit de nationale instituut van Diabetes en het spijsverteringskanaal en ziekten van de nieren. De dataset is gericht op de vrouwelijke patiënten van Indiase erfgoed Pima gefilterd. De gegevens omvatten medische gegevens zoals glucose en inulinestroop niveaus en leefgewoonten.<p> </p><b>Syntaxis:</b> voorspellen of het onderwerp diabetes (binaire indeling heeft). <p> </p><b>Onderzoek:</b> Sigillito, V. (1990). UCI-Machine Learning opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml '</a>. Irvine, CA: University of California, School van informatie en computerwetenschappen </td>
</tr>

<tr>
  <td valign=top>Restaurant klantgegevens</td>
  <td valign=top>
Een set metagegevens over klanten, met inbegrip van de demografische gegevens en voorkeuren.<p> </p><b>Syntaxis:</b> deze dataset, gebruiken in combinatie met de andere twee restaurant gegevenssets, trainen en testen van een recommender-systeem. <p> </p><b>Onderzoek:</b> Bache, K. en Lichman, M. (2013). UCI Machine opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>leren. Irvine, CA: University of California, School van informatie en computerwetenschappen.
  </td>
</tr>

<tr>
  <td valign=top>Restaurant functiegegevens</td>
  <td valign=top>
Een set metagegevens over restaurants en hun functies, zoals het type voedsel, dineren stijl en locatie.<p> </p><b>Syntaxis:</b> deze dataset, gebruiken in combinatie met de andere twee restaurant gegevenssets, trainen en testen van een recommender-systeem. <p> </p><b>Onderzoek:</b> Bache, K. en Lichman, M. (2013). UCI Machine opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>leren. Irvine, CA: University of California, School van informatie en computerwetenschappen.
  </td>
</tr>

<tr>
  <td valign=top>Restaurant beoordelingen</td>
  <td valign=top>
Beoordelingen gegeven door gebruikers restaurants op een schaal van 0 tot 2 bevat.<p> </p><b>Syntaxis:</b> deze dataset, gebruiken in combinatie met de andere twee restaurant gegevenssets, trainen en testen van een recommender-systeem. <p> </p><b>Onderzoek:</b> Bache, K. en Lichman, M. (2013). UCI Machine opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>leren. Irvine, CA: University of California, School van informatie en computerwetenschappen.
  </td>
</tr>

<tr>
  <td valign=top>Stalen Annealing met meerdere klasse dataset</td>
  <td valign=top>
Deze dataset bevat een reeks records uit staal aanhechten van proeven met de fysieke kenmerken (breedte, dikte, type (spiraal, blad, enz.) van de resulterende stalen typen.<p> </p><b>Syntaxis:</b> voorspellen twee numerieke klasse kenmerken; hardheid of sterkte. U kunt ook correlaties tussen kenmerken analyseren.<p> </p>Staalsoorten, gaat u als volgt een set standaard, gedefinieerd door SAE en andere organisaties. U bent op zoek naar een bepaalde 'kwaliteit' (de klassevariabele) en wilt weten over de waarden die nodig zijn. <p> </p><b>Onderzoek:</b> pond, D. & Buntine, W., (NA). UCI Machine opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>leren. Irvine, CA: University of California, School van informatie en computerwetenschappen <p> </p>Een handige handleiding voor staalsoorten vindt u hier: <a href="http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf">http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf</a>
  </td>
</tr>

<tr>
  <td valign=top>Gegevens van de telescoop</td>
  <td valign=top>
Records van hoge energie gamma partikel bursts en achtergrondruis, zowel in de simulatie via een proces van Monte Carlo.<p> </p>De bedoeling van de simulatie was het verbeteren van de nauwkeurigheid van de grond opererende atmosferische Cherenkov gamma telescopen, met behulp van statistische methoden om onderscheid te maken tussen het gewenste signaal (Cherenkov straling douches) en achtergrondruis (hadronic douches gestart door cosmic stralen in de bovenste atmosfeer).<p> </p>De gegevens zijn voorverwerkte een lang, staafvormig cluster maken met de lange as is gericht op het midden van de camera. De parameters die kunnen worden gebruikt voor discriminatie van afbeelding zijn de kenmerken van deze ellips, (vaak genoemd Hillas parameters).<p> </p><b>Syntaxis:</b> voorspellen of afbeelding van een douche signaal of achtergrond ruis vertegenwoordigt.<p> </p><b>Toelichting:</b> eenvoudige indeling nauwkeurigheid is niet relevant voor deze gegevens sinds de classificatie van een gebeurtenis achtergrond als signaal is dan een signaalgebeurtenis als achtergrond voor het classificeren. Voor een vergelijking van de verschillende classificaties moet de ROC grafiek worden gebruikt. De waarschijnlijkheid van een gebeurtenis achtergrond accepteren als signaal onder een van de volgende drempels moet: 0,01 0,02, 0,05, 0,1 of 0,2.<p> </p>Let ook op dat het aantal achtergrond-gebeurtenissen (h voor hadronic douches) wordt onderschat, terwijl in reële metingen, staat het merendeel van de gebeurtenissen voor de klasse h of ruis. <p> </p><b>Onderzoek:</b> Bock, R.K. (1995). UCI Machine opslagplaats <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>leren. Irvine, CA: Universiteit van California, School van informatie </td>
</tr>

<tr ID=weather-dataset>
  <td valign=top>Weerbericht Dataset</td>
  <td valign=top>
Per uur weer grond waarnemingen van NOAA (<a href="http://cdo.ncdc.noaa.gov/qclcd_ascii/, merged data from 201304 to 201310">samengevoegde gegevens uit de 201304 naar 201310</a>).<p> </p>Opmerkingen die door de luchthaven weer stations, die betrekking hebben op de periode April-oktober 2013 heeft betrekking op de weergegevens. Voordat u deze uploadt naar Azure ML Studio, is de dataset als volgt verwerkt:<ul><li>Weather station-id's zijn toegewezen aan de desbetreffende luchthaven id 's</li><li>Weather stations die niet zijn gekoppeld aan de 70 drukste luchthavens zijn gefilterd.</li><li>De kolom datum is gesplitst in afzonderlijke kolommen van jaar, maand en dag</li><li>De volgende kolommen zijn geselecteerd: AirportID, jaar, maand, dag, tijd, tijdzone, SkyCondition, zichtbaarheid, WeatherType, DryBulbFarenheit, DryBulbCelsius, WetBulbFarenheit, WetBulbCelsius, DewPointFarenheit, DewPointCelsius, RelativeHumidity, windsnelheid, WindDirection, ValueForWindCharacter, StationPressure, PressureTendency, PressureChange, SeaLevelPressure, RecordType, HourlyPrecip, Altimeter</li></ul>
  </td>
</tr>

<tr ID=wikipedia-sp-500-dataset>
  <td valign=top>Wikipedia SP 500 Dataset</td>
  <td valign=top>
Gegevens is afkomstig van Wikipedia (<a href="http://www.wikipedia.org/">http://www.wikipedia.org/</a>) op basis van de artikelen van elk S & P 500-bedrijf, opgeslagen als XML-gegevens.<p> </p>Voordat u deze uploadt naar Azure ML Studio, is de dataset als volgt verwerkt:<ul><li>Tekst voor elk specifiek bedrijf uitpakken</li><li>Wiki-opmaak verwijderen</li><li>Niet-alfanumerieke tekens verwijderen</li><li>Alle tekst omzetten in kleine letters</li><li>Bekende bedrijf categorieën zijn toegevoegd.</li></ul><p> </p>Opmerking: voor sommige een artikel bedrijven kan niet gevonden worden, het aantal records dat kleiner is dan 500.
  </td>
</tr>





<tr ID=direct-marketing>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/direct_marketing.csv">direct_marketing.csv</a></td>
  <td valign=top>
De dataset bevat gegevens van de klant en aanwijzingen geeft over hun reactie op een direct mailing campagne. Elke rij vertegenwoordigt een klant. De dataset 9 functies over demografische gebruikers en voorbij gedrag en 3 label kolommen bevat (bezoekt, conversie, en besteden).  Bezoek is een binaire kolom waarin wordt aangegeven dat een klant die na de marketingcampagne, conversie geeft aan een klant iets gekocht en besteden aan het bedrag dat is besteed.  De dataset is beschikbaar gesteld door Kevin Hillstrom voor MineThatData e-Analytics en Data Mining Challenge.
  </td>
</tr>

<tr ID=lyrl2004-tokens-test>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_test.csv">lyrl2004_tokens_test.csv</a></td>
  <td valign=top>
Functies van de voorbeelden test in de dataset RCV1 V2 Reuters nieuws. De dataset is 781K nieuwsartikelen en hun id (eerste kolom van de gegevensset). Elk artikel is ge? exeerd, stopworded, en voort. De dataset is beschikbaar gesteld door David. D. Lewis.
  </td>
</tr>

<tr ID=lyrl2004-tokens-train>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_train.csv">lyrl2004_tokens_train.csv</a></td>
  <td valign=top>
Functies van de voorbeelden in de dataset RCV1 V2 Reuters nieuws training. De dataset is 23K nieuwsartikelen en hun id (eerste kolom van de gegevensset). Elk artikel is ge? exeerd, stopworded, en voort. De dataset is beschikbaar gesteld door David. D. Lewis.
  </td>
</tr>

<tr ID=intrusion-detection>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a><br></td>
  <td valign=top>
De gegevensset van de KDD Cup 1999 Knowledge Discovery en datamining extra concurrentie (<a href="http://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html">kddcup99.html</a>).<p> </p>De dataset is gedownload en opgeslagen in Azure Blob-opslag (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a>) en omvat zowel opleiding als datasets te testen. De dataset training heeft ongeveer 126K rijen en 43 kolommen, met inbegrip van de etiketten; 3 kolommen maken deel uit van de labelgegevens en 40 kolommen, die bestaat uit een tekenreeks/bestaan en numerieke functies zijn beschikbaar voor de opleiding van het model. De testgegevens hebben ongeveer 22,5 K test voorbeelden met 43 dezelfde kolommen in de trainingsgegevens.

  </td>
</tr>

<tr ID=rcv1-v2-topics-qrels>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/rcv1-v2.topics.qrels.csv">rcv1 v2.topics.qrels.csv</a></td>
  <td valign=top>
Onderwerp toewijzingen voor nieuwsartikelen in de dataset RCV1 V2 Reuters nieuws. Een nieuwsartikel kan worden toegewezen aan verschillende onderwerpen. De indeling van elke rij is "<topic name>  <document id> 1". De dataset bevat 2.6M onderwerp toewijzingen. De dataset is beschikbaar gesteld door David. D. Lewis.
  </td>
</tr>

<tr ID=student-performance>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a></td>
  <td valign=top>
Deze gegevens zijn afkomstig van de challenge KDD Cup 2010 Student performance evaluatie (<a href="http://www.kdd.org/kdd-cup-2010-student-performance-evaluation">student prestatie-evaluatie</a>). De gebruikte gegevens is de Algebra_2008_2009 training (Mizil-Stamper, J., Niculescu, A., Ritter, S., Gordon, G.J. & Koedinger, K.R. (2010). Algebra I 2008-2009. Een gegevensset van KDD Cup 2010 educatieve Data Mining Challenge uitdaging. Deze vinden op <a href="http://pslcdatashop.web.cmu.edu/KDDCup/downloads.jsp">downloads.jsp</a> of <a href="http://www.kdd.org/sites/default/files/kddcup/site/2010/files/algebra_2008_2009.zip">algebra_2008_2009.zip</a>.<p> </p>De dataset is gedownload en opgeslagen in Azure Blob-opslag (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a>) en de logboekbestanden van een student Bijles systeem bevat. De opgegeven functies omvatten probleem-ID en een korte beschrijving, student-ID, timestamp en hoeveel pogingen de student voor het oplossen van het probleem op de juiste manier uitgevoerd. De originele dataset 8,9 M records bevat, is deze dataset gedownsampled naar de eerste 100K rijen. De dataset is 23 door tabs gescheiden kolommen van verschillende typen: numerieke bestaan, en tijdstempel.

  </td>
</tr>




</table>


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
