<properties
    pageTitle="Quick start guide: Machine Learning aanbevelingen API | Microsoft Azure"
    description="Azure Machine Learning aanbevelingen - Quick Start Guide"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="luisca"/>

# <a name="quick-start-guide-for-the-cognitive-services-recommendations-api"></a>De slag voor de cognitieve Services API aanbevelingen

Dit document wordt beschreven hoe met on-board uw service of toepassing voor het gebruik van de [API voor aanbevelingen](http://go.microsoft.com/fwlink/?LinkId=759710).
U vindt meer informatie over de API voor aanbevelingen en andere cognitieve diensten [hier](http://go.microsoft.com/fwlink/?LinkId=759709). In deze handleiding ook mogelijk de [API-naslaggids aanbevelingen](http://go.microsoft.com/fwlink/?LinkId=759348) bij de hand hebt.


<a name="Overview"></a>
## <a name="general-overview"></a>Algemeen overzicht

Dit document is een stapsgewijze handleiding. Ons doel is u stapsgewijs door de stappen die nodig zijn een model van de trein en wijs hulpbronnen, waarmee u het model van uw productieomgeving verbruiken.

In deze oefening duurt ongeveer 30 minuten.

[Aanbevelingen API](http://go.microsoft.com/fwlink/?LinkId=759710)gebruikt, moet u de volgende stappen uit:

1. Maak een model - een model is een container van de gebruiksgegevens, gegevens in de catalogus en het model van de aanbeveling.
1. Gegevens importeren - een catalogus bevat metagegevens gegevens over de artikelen.
1. Gebruiksgegevens importeren - gebruiksgegevens kan worden geüpload in een van de volgende 2 manieren (of beide):
  -  Door het uploaden van een bestand bevat dat de gegevens.
  -  Verzenden van data acquisition gebeurtenissen.
  Meestal uploaden u van een bestand gebruik te kunnen maken een eerste aanbeveling model (bootstrap) en daarmee tot het systeem voldoende gegevens worden verzameld met behulp van de gegevensindeling voor de aanschaf.
1. Bouwen van een model aanbeveling - dit is een asynchrone bewerking waarin de aanbeveling wordt de gebruiksgegevens en maakt een aanbeveling model. Deze bewerking kan enkele minuten tot enkele uren, afhankelijk van de grootte van de gegevens en de configuratieparameters build nemen. Wanneer de build activeren, krijgt u een build-ID. Gebruiken om te controleren wanneer het buildproces is beëindigd voordat u begint met de aanbevelingen in beslag nemen.
1. Aanbevelingen - aanbevelingen voor een bepaald artikel of een lijst met items ophalen in beslag nemen.

<a name="GetStarted"></a>
### <a name="lets-get-started"></a>We gaan aan de slag!

U wilt beginnen met het bouwen van een model voor aanbevelingen. Vervolgens zullen wij helpen u over het gebruik van de resultaten die door het model power aanbevelingen op een e-commerce site.

<a name="Ex1Task1"></a>
#### <a name="task-1---signing-up-for-the-recommendations-api"></a>Taak 1 - handtekeningcertificaat voor de aanbevelingen-API ####

In deze taak, u aanmelden bij de service API aanbevelingen, en maak een model aanbevelingen.

1. Ga naar de **Azure Portal** op [http://portal.azure.com/](http://portal.azure.com/) en meld u aan met uw account Azure.

1.  Klik op **+ nieuwe**.

1. Selecteer de optie **Intelligence** .

1. Selecteer het product **Cognitieve Services API's** .
Dit product kunt u een abonnement voor een van de cognitieve services API's (gezicht, tekst Analytics, Computer Vision, enz.). Vandaag gaan we in op de aanbevelingen-API.

1. Voer de **accountnaam** voor uw abonnement aanbevelingen op de openingspagina cognitieve Services API. (Voor instace: 'MyRecommendations'). Deze naam mag spaties hebben.

1. Selecteer op **type API**, **aanbevelingen**.

1. In de **prijzen laag**, kunt u een plan. Selecteer de laag **vrij** voor 10.000 transacties per maand. Dit is een gratis plan is een goede manier om te proberen het systeem start. Als u naar de productie gaat, wordt aangeraden u rekening houden met het volume van uw aanvraag en het plan dienovereenkomstig wijzigen. (Opmerking: Er kan slechts één laag gratis abonnement per keer)

1. Een **Resourcegroep**selecteren of een nieuwe maken als u geen sjabloon hebt.

1. U kunt andere elementen in het dialoogvenster maken. We er even op wijzen dat de resource provider vandaag wordt alleen ondersteund in de Verenigde Staten datacenters.

1. Klik op **maken**als u klaar bent met de selecties.

1. Wacht een paar minuten voor de resource moet worden geïmplementeerd.
Zodra deze is geïmplementeerd, kunt u gaan naar de sectie **sleutels** in de blade **Instellingen** waar krijgt u een primaire en secundaire sleutel de API te gebruiken.  De primaire sleutel, moet u deze bij het maken van uw eerste model kopiëren.

<a name="Ex1Task2"></a>
#### <a name="task-2---did-you-bring-your-data"></a>Taak 2 - heeft u uw gegevens overbrengen? ####

Goede aanbevelingen om leren de aanbevelingen API uit de catalogus en uw transacties. Dit betekent dat u moet het kanaal met goede gegevens over uw producten (We noemen dit **een catalogusbestand** ) en een reeks transacties die groot genoeg is om te zoeken naar interessante consumptiepatronen (We noemen dit **Gebruik**).

1. Gewoonlijk zou u uw database transacties voor deze stukjes informatie opvragen.
Als u niet deze bij de hand hebt, hebben we voorbeeldgegevens verstrekt voor u op basis van de gegevens van Microsoft Store.

 U kunt de gegevens downloaden vanaf [hier](http://aka.ms/RecoSampleData). Kopiëren en de MsStoreData.Zip uitpakken naar een map op uw lokale computer.

 > **Opmerking:** De voorbeeldcode die u wilt downloaden en uitvoeren in taak 3 bevat voorbeeldgegevens die al deel uitmaken van deze--zodat deze taak optioneel is.  Dat gezegd, deze taak kunt u een meer realistische gegevenssets downloaden en kunt u de invoer in de API aanbevelingen beter te begrijpen zijn.

1.  Nu gaan we een kijkje nemen op het catalogusbestand. Ga naar de locatie waar u de gegevens hebt gekopieerd.
 De catalogusbestand openen in **Kladblok**.

 U merkt dat het catalogusbestand heel eenvoudig is. Heeft de volgende indeling:`<itemid>,<item name>,<product category>`

 >  AAA-04294, OfficeLangPack 2013 32/64 E34 Online DwnLd, Office <br>
 > AAA-04303, OfficeLangPack 2013 32/64 ET on line DwnLd, Office  <br>
 > C9F 00168, Kiruna KRUSELL Flip Cover voor Nokia Lumia 635 - kamelen, Bureau-accessoires

 We er even op wijzen dat een bestand, kunt u veel rijkere bijvoorbeeld kunt u metagegevens over de producten (noemen We deze *functies item*) toevoegen. Ziet u de sectie [catalogusopmaak](http://go.microsoft.com/fwlink/?LinkID=760716) in de API Reference voor meer informatie over de indeling van de catalogus.

1. We gaan doen met de gegevens over het gebruik. U ziet dat de datum van het gebruik van de indeling `<User Id>,<Item Id>,<Time Stamp>,<Event>`.

  > 00037FFEA61FCA16, 288186200, 2015-08/04T11:02:52, 0003BFFDD4C2148C, aankoop, 297833400, 2015-08/04T11:02:50, 0003BFFDD4C2118D, aankoop, 297833300, 2015/08/04T11:02:40, 00030000D16C4237, aankoop, 297833300, 2015-08/04T11:02:37, 0003BFFDD4C20B63, inkoop, 297833400, 2015-08/04T11:02:12, 00037FFEC8567FB8, aankoop, 297833400, 2015-08/04T11:02:04, aankoop

U ziet dat de eerste drie elementen verplicht zijn. Het gebeurtenistype is optioneel. Het [gebruik van opmaak](http://go.microsoft.com/fwlink/?LinkID=760712) voor meer informatie over dit onderwerp kan worden uitgecheckt.

 > **Hoeveel gegevens hebt u nodig?**
 <p>
Welnu, deze echt afhankelijk is van de gebruiksgegevens zelf. Leert het systeem wanneer gebruikers verschillende artikelen kopen. Voor sommige versies zoals FBT is het belangrijk om te weten welke artikelen worden ingekocht in dezelfde transacties. (We noemen dit *CO exemplaren*). Een goede vuistregel is dat de meeste items in de transacties van 20 of meer, dus als u 10.000 items in uw catalogus had, zou raden u ten minste 20-maal het aantal transacties of transacties van ongeveer 200.000.
Nogmaals, is dit een vuistregel. U wilt experimenteren met uw gegevens.
</p>

<a name="Ex1Task3"></a>
####Taak 3 - maken van een model van aanbevelingen####

Nu u een account hebt en dat u gegevens hebt, laten we het eerste model te maken.

In deze taak gebruikt u de voorbeeldtoepassing te maken van uw eerste model.

1. Ten eerste moet u rekening houden met de [Aanbevelingen API Reference](http://go.microsoft.com/fwlink/?LinkId=759348).

1. De [Voorbeeldtoepassing](http://go.microsoft.com/fwlink/?LinkID=759344) downloaden naar een lokale map.

1. De zich in de map **C#** **RecommendationsSample.sln** -oplossing in Visual Studio openen.

1. Open het bestand **SampleApp.cs** . Opmerking dat de volgende stappen uit in het bestand:
 + Een Model maken
 + Een bestand toevoegen
 + Een bestand gebruik toevoegen
 + Trigger een gecompileerde versie van het Model
 + Een aanbeveling op basis van een paar items ophalen
<p></p>

1. De waarde voor het veld **AccountKey** vervangen door de sleutel van taak 1.

1. Stap voor stap de oplossing, en ziet u hoe een model wordt gemaakt.

1. Probeer de catalogus en het gebruik van bestanden die u zojuist hebt gedownload om een nieuw model voor de Microsoft Store of boek aanbevelingen voor vervangen. U moet ook de naam van het model en de items waarvoor u aanbevelingen aanvragen wijzigen.

1. Wanneer het model wordt gemaakt, dient u de **model-ID** als u deze hebt nodig bij het aanvragen van de aanbevelingen in uw productieomgeving.

>  Meer informatie over typen en het evalueren van de kwaliteit van de builds bouwen [hier](cognitive-services-recommendations-buildtypes.md).

<a name="Ex1Task4"></a>
### <a name="putting-your-model-in-production"></a>Plaatsen van het model in productie! ###

U begrijpt dat het maken van een model en aanbevelingen in beslag nemen, is de volgende stap het in productie op uw website, mobiele toepassing of integreren in uw CRM- of ERP-systeem.
Uiteraard zou deze allemaal verschillend zijn. Aangezien de API aanbevelingen worden gevraagd als een webservice, is u moet kunnen aanroepen vanuit een van deze verschillende omgevingen eenvoudig.

**Belangrijk:**  Als u aanbevelingen weergeven in een openbare-client (bijvoorbeeld uw e-commerce site), moet u een proxy-server om de aanbevelingen te maken. Dit is van belang dat uw API-sleutel wordt niet blootgesteld aan externe (mogelijk niet-vertrouwde) entiteiten.

Hier zijn een paar ideeën van locaties waar u aanbevelingen kunt gebruiken:

### <a name="product-page"></a>Productpagina

**Item aanbevelingen**
<p>Als het model is getraind op aankoop gegevens, is het toegestaan de klant voor *producten die zijn waarschijnlijk van belang zijn voor mensen die de bron-object hebt gekocht*.</p>
<p>Klik op gegevens, als het model is getraind voor uw klant te *ontdekken die dreigen te worden bezocht door mensen die hebben bezocht het bronitem producten*is het toegestaan. Dit type model mogelijk vergelijkbare objecten als resultaat.</p>

**Vaak gekocht samen aanbevelingen**
<p>Vaak gekocht samen build A kan worden getraind, zodat u sets van artikelen dreigen te worden met dit object aangeschaft.</p>

### <a name="check-out-page"></a>Pagina uitchecken

**Item aanbevelingen**
<p>Een aanbevelingen kan krijgen over het model als een lijst met items als invoer. Zo kunt u alle items in het winkelmandje doorgeven als invoer voor aanbevelingen krijgen.
In dat geval krijgt het model aanbevelingen die van belang zijn de artikelen in het mandje gegeven.
</p>

### <a name="landing-page"></a>Portaalpagina

**Aanbevelingen van de gebruiker**
<p>
Een model kunt treffen als aanbevelingen worden de gebruikers-id ingevoerd.  Dit wordt de geschiedenis van de transacties die de gebruiker gebruiken om persoonlijke aanbevelingen voor de opgegeven gebruiker.
</p>

Bekijk de [Documentatie voor Item aanbevelingen krijgen](http://go.microsoft.com/fwlink/?LinkID=760719).

<a name="Ex1Task6"></a>
### <a name="whats-next"></a>Hoe nu verder?
Gefeliciteerd, als u dat dit tot nu toe! Als u wilt weten meer Ga naar de volledige [API-naslaggids aanbevelingen](http://go.microsoft.com/fwlink/?LinkId=759348) als u vragen hebt, gerust contact met ons opmlapi@microsoft.com
