<properties
    pageTitle="Aan de slag met Azure zoeken in NodeJS | Microsoft Azure | De zoekservice hosted cloud"
    description="Wandel door het bouwen van een zoektoepassing op een zoekservice hosted cloud op Azure NodeJS als uw programmeertaal gebruiken."
    services="search"
    documentationCenter=""
    authors="EvanBoyle"
    manager="pablocas"
    editor="v-lincan"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.date="07/14/2016"
    ms.author="evboyle"/>

# <a name="get-started-with-azure-search-in-nodejs"></a>Aan de slag met Azure zoeken in NodeJS
> [AZURE.SELECTOR]
- [Portal](search-get-started-portal.md)
- [.NET](search-howto-dotnet-sdk.md)

Informatie over het maken van een aangepaste NodeJS search toepassing die gebruikmaakt van Azure zoeken voor de zoekfunctie. In deze zelfstudie wordt de [REST API-Azure Search Service](https://msdn.microsoft.com/library/dn798935.aspx) te maken van de objecten en bewerkingen in deze oefening gebruikt.

Wij ontwikkelen en testen van deze code [NodeJS](https://nodejs.org) en NPM, [Sublime tekst 3](http://www.sublimetext.com/3)en Windows PowerShell in Windows 8.1 gebruikt.

Als u dit voorbeeld uitvoert, moet u een Azure Search-service u voor de [Azure Portal aanmelden kunt](https://portal.azure.com)hebben. Zie [een Azure Search-service in de portal maken](search-create-service-portal.md) voor stapsgewijze instructies.

## <a name="about-the-data"></a>Over de gegevens

Deze voorbeeldtoepassing gebruikt de gegevens uit de [Verenigde Staten geologische Services USG's ()](http://geonames.usgs.gov/domestic/download_data.htm), gefilterd op de status van Rhode Island dataset te verkleinen. Deze gegevens gebruiken we bouwen een zoektoepassing oriëntatiepunten gebouwen zoals ziekenhuizen, scholen en geologische functies zoals stromen, meren en topontmoetingen geeft als resultaat.

Voor deze **DataIndexer** wordt gemaakt en de index met een [indexeerfunctie](https://msdn.microsoft.com/library/azure/dn798918.aspx) constructie voor de gefilterde USG dataset ophalen uit een openbare Azure SQL-Database wordt geladen. Referenties en verbinding gegevens bij de gegevensbron voor on line beschikbaar is in de programmacode. Er is geen verdere configuratie nodig.

> [AZURE.NOTE] We een filter toegepast op deze dataset onder de 10.000 Documentlimiet van de gratis prijzen laag blijven. Als u de standaard laag, is deze beperking niet van toepassing. Zie voor meer informatie over de capaciteit voor elke laag van de prijzen [Search service grenzen](search-limits-quotas-capacity.md).


<a id="sub-2"></a>
## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>De servicenaam en de api-sleutel van de zoekservice Azure zoeken

Nadat u de service hebt gemaakt, terug naar de portal voor het ophalen van de URL of `api-key`. Verbindingen met uw zoekservice moeten de zowel de URL en een `api-key` voor het verifiëren van de oproep.

1. Aanmelden bij de [Azure Portal](https://portal.azure.com).
2. Klik in de balk Snel **Search-service** -overzicht van de zoekservices van Azure ingericht voor uw abonnement.
3. Selecteer de service die u wilt gebruiken.
4. Op het servicedashboard ziet u tegels voor essentiële informatie, alsmede het sleutelpictogram voor toegang tot de admin-sleutels.

    ![][3]

5. Kopieer de URL, een admin-toets en de toets van een query. U moet alle drie later, wanneer u ze aan het bestand config.js toevoegt.

## <a name="download-the-sample-files"></a>De voorbeeldbestanden downloaden

Als u een van de volgende methoden gebruiken voor het downloaden van het monster.

1. Ga naar [AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodeJSIndexerDemo).
2. Klik op **Downloaden ZIP**, sla het ZIP-bestand en pak de bestanden erin.

Alle instructies uitvoeren en bestand latere wijzigingen worden ten opzichte van bestanden in deze map.


## <a name="update-the-configjs-with-your-search-service-url-and-api-key"></a>De config.js bijwerken. met de URL van de zoekopdracht en api-sleutel

Met behulp van de api-sleutel die u eerder hebt gekopieerd en URL de URL, admin-, en query-sleutel opgeven in configuratiebestand.

Admin sleutels verlenen volledige controle over de service-activiteiten, inclusief het maken of verwijderen van een index en laden van documenten. Query-sleutels zijn daarentegen voor alleen-lezen bewerkingen, meestal gebruikt door toepassingen die verbinding met Azure zoeken maken.

In dit voorbeeld zijn we de sleutel query ter versterking van het beste gebruik van de query-sleutel in client-toepassingen.

Het volgende screenshot toont **config.js** geopend in een tekst-editor met de relevante vermeldingen gescheiden, zodat u kunt zien waar u het bestand wilt bijwerken met de waarden die geldig voor de zoekservice zijn.

![][5]


## <a name="host-a-runtime-environment-for-the-sample"></a>Host een runtimeomgeving voor het monster

Het monster is een HTTP-server, die u kunt installeren met globaal npm vereist.

Een venster PowerShell gebruiken voor de volgende opdrachten.

1. Ga naar de map met het bestand **package.json** .
2. Type `npm install`.
2. Type `npm install -g http-server`.

## <a name="build-the-index-and-run-the-application"></a>De index bouwen en uitvoeren van de toepassing

1. Type `npm run indexDocuments`.
2. Type `npm run build`.
3. Type `npm run start_server`.
4. Uw browser directe`http://localhost:8080/index.html`

## <a name="search-on-usgs-data"></a>Zoeken op USG gegevens

De gegevensset USG bevat records die relevant zijn voor de staat Rhode Island. Als u klikt op **Zoeken** op een leeg zoekvak, krijgt u de top 50 posten, de standaardinstelling.

Invoeren van een zoekterm krijgt de zoekmachine iets om op te gaan. Geef de naam van een regionale. "Roger Williams" was de eerste gouverneur van Rhode Island. Talloze parken, gebouwen en scholen zijn naam hem.

![][9]

Ook kunt u een van deze voorwaarden:

- Pawtucket
- Pembroke
- goose + Kaapverdië


## <a name="next-steps"></a>Volgende stappen

Dit is de eerste Azure Search zelfstudie op basis van NodeJS en USG dataset. Na verloop van tijd verlengen we deze zelfstudie om extra zoekfuncties die u kunt gebruiken in aangepaste oplossingen aan te tonen.

Als er al een achtergrond in Azure zoeken, kunt u dit voorbeeld als een springboard voor het uitproberen van suggesters (type-ahead of AutoAanvullen query's), filters en navigatie beperkt. U kunt ook op de pagina met zoekresultaten verbeteren door tellingen toe te voegen en batchen van documenten, zodat gebruikers de resultaten kunnen doorlopen.

Nieuwe Azure zoeken? Het is raadzaam probeert andere cursussen voor het ontwikkelen van een goed begrip van kunt maken. Ga naar onze [documentatiepagina](https://azure.microsoft.com/documentation/services/search/) als u wilt zoeken naar andere bronnen. U kunt de koppelingen ook bekijken in onze [Video en zelfstudie lijst](search-video-demo-tutorial-list.md) naar access voor meer informatie.

<!--Image references-->
[1]: ./media/search-get-started-nodejs/create-search-portal-1.PNG
[2]: ./media/search-get-started-nodejs/create-search-portal-2.PNG
[3]: ./media/search-get-started-nodejs/create-search-portal-3.PNG
[5]: ./media/search-get-started-nodejs/AzSearch-NodeJS-configjs.png
[9]: ./media/search-get-started-nodejs/rogerwilliamsschool.png
