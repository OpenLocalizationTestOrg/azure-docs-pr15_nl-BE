<properties
    pageTitle="Aan de slag met Azure Search in Java | Microsoft Azure | De zoekservice hosted cloud"
    description="Het bouwen van een gehoste cloud-zoektoepassing op Azure met de programmeertaal Java."
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

# <a name="get-started-with-azure-search-in-java"></a>Aan de slag met Azure Search in Java
> [AZURE.SELECTOR]
- [Portal](search-get-started-portal.md)
- [.NET](search-howto-dotnet-sdk.md)

Informatie over het maken van een aangepaste Java search toepassing die gebruikmaakt van Azure zoeken voor de zoekfunctie. In deze zelfstudie wordt de [REST API-Azure Search Service](https://msdn.microsoft.com/library/dn798935.aspx) te maken van de objecten en bewerkingen in deze oefening gebruikt.

Als u dit voorbeeld uitvoert, moet u een Azure Search-service u voor de [Azure Portal aanmelden kunt](https://portal.azure.com)hebben. Zie [een Azure Search-service in de portal maken](search-create-service-portal.md) voor stapsgewijze instructies.

We de volgende software te testen, in dit voorbeeld gebruikt:

- [Eclips IDE voor ontwikkelaars van Java EE](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar). Zorg ervoor dat de EE-versie te downloaden. Een van de stappen voor verificatie vereist een functie die alleen in deze editie wordt gevonden.

- [JDK 8u40](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

- [Apache Tomcat 8.0](http://tomcat.apache.org/download-80.cgi)

## <a name="about-the-data"></a>Over de gegevens

Deze voorbeeldtoepassing gebruikt de gegevens uit de [Verenigde Staten geologische Services USG's ()](http://geonames.usgs.gov/domestic/download_data.htm), gefilterd op de status van Rhode Island dataset te verkleinen. Deze gegevens gebruiken we bouwen een zoektoepassing oriëntatiepunten gebouwen zoals ziekenhuizen, scholen en geologische functies zoals stromen, meren en topontmoetingen geeft als resultaat.

Voor deze **SearchServlet.java** wordt gemaakt en de index met een [indexeerfunctie](https://msdn.microsoft.com/library/azure/dn798918.aspx) constructie voor de gefilterde USG dataset ophalen uit een openbare Azure SQL-Database wordt geladen. Vooraf gedefinieerde referenties en de verbindingsgegevens voor de gegevensbron online beschikbaar in de programmacode. Wat betreft de toegang tot gegevens is geen verdere configuratie nodig.

> [AZURE.NOTE] We een filter toegepast op deze dataset onder de 10.000 Documentlimiet van de gratis prijzen laag blijven. Deze beperking geldt niet als u de standaard laag, en kunt u deze code voor het gebruik van een grotere dataset. Zie voor meer informatie over de capaciteit voor elke laag van de prijzen de [grenzen en beperkingen](search-limits-quotas-capacity.md).

## <a name="about-the-program-files"></a>Over de programma 's

De volgende lijst bevat de bestanden die relevant voor dit voorbeeld zijn.

- Search.JSP: De gebruikersinterface biedt
- SearchServlet.java: Biedt methoden (vergelijkbaar met een domeincontroller in het MVC)
- SearchServiceClient.java: Grepen HTTP-aanvragen
- SearchServiceHelper.java: Een helperklasse met statische methoden
- Document.Java: Biedt het gegevensmodel
- Config.Properties: Hiermee stelt u de URL van de zoekopdracht en de api-sleutel
- Pom.XML: Een Maven-afhankelijkheid

<a id="sub-2"></a>
## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>De servicenaam en de api-sleutel van de zoekservice Azure zoeken

Alle overige API-aanroepen in Azure Search vereist dat u de URL en een api-sleutel opgeven. 

1. Aanmelden bij de [Azure Portal](https://portal.azure.com).
2. Klik in de balk Snel **Search-service** -overzicht van de zoekservices van Azure ingericht voor uw abonnement.
3. Selecteer de service die u wilt gebruiken.
4. Op het servicedashboard ziet u tegels voor essentiële informatie en het sleutelpictogram voor toegang tot de admin-sleutels.

    ![][3]

5. Kopieer de URL en een admin-toets. U moet deze later, wanneer u ze aan het bestand **config.properties toevoegt** .

## <a name="download-the-sample-files"></a>De voorbeeldbestanden downloaden

1. Ga naar [AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) op Github.

2. Klik op **ZIP-Download**het ZIP-bestand op schijf opslaan en pak de bestanden erin. Rekening houden met het uitpakken van de bestanden in uw werkruimte Java gemakkelijker terugvinden van het project.

3. De voorbeeldbestanden zijn alleen-lezen. Met de rechtermuisknop op de mapeigenschappen en schakelt u het kenmerk alleen-lezen.

Alle instructies uitvoeren en bestand latere wijzigingen worden ten opzichte van bestanden in deze map.  

## <a name="import-project"></a>Project importeren

1. In de Eclips, kiest u **bestand** > **importeren** > **Algemeen** > **Bestaande projecten in de werkruimte**.

    ![][4]

2. In de **hoofdmap te selecteren**, Ga naar de map met de voorbeeldbestanden. Selecteer de map met de map .project. Het project moet worden weergegeven in de lijst met **projecten** als een geselecteerd item.

    ![][12]

3. Klik op **Voltooien**.

4. Met de **Project Explorer** kunt weergeven en bewerken van de bestanden. Als deze nog niet is geopend, klikt u op **venster** > **Weergave tonen** > **Project Explorer** of de snelkoppeling te openen.

## <a name="configure-the-service-url-and-api-key"></a>De URL van de service en de api-sleutel configureren

1. Dubbelklik in de **Projectverkenner**op **config.properties** om de naam van de server en api-sleutel met configuratie-instellingen bewerken.

2. Raadpleeg de stappen die eerder in dit artikel, waar u de URL van de service en de api-sleutel in de [Azure Portal gevonden](https://portal.azure.com)naar de waarden die u zal nu in de **config.properties**.

3. In **config.properties**, "Api Key" te vervangen met de api-sleutel voor uw service. Servicenaam (het eerste onderdeel van de URL-http://servicename.search.windows.net) wordt vervolgens vervangen door 'servicenaam' in hetzelfde bestand.

    ![][5]

## <a name="configure-the-project-build-and-runtime-environments"></a>Configureren van de project, bouwen en runtime-omgevingen

1. Eclips in Project Explorer met de rechtermuisknop op het project > **Eigenschappen** > **Project facetten**.

2. Selecteer **dynamische webmodule**, **Java**en **JavaScript**.

    ![][6]

3. Klik op **toepassen**.

4. Selecteer **venster** > **Voorkeuren** > **Server** > **-runtimeomgevingen** > **Rapp.**.

5. Vouw Apache en selecteert u de versie van de Apache Tomcat-server die u eerder hebt geïnstalleerd. Op ons systeem, we geïnstalleerd versie 8.

    ![][7]

6. Geef op de volgende pagina de Tomcat-installatiemap. Op een computer met Windows is dit waarschijnlijk C:\Program Files\Apache Software Foundation\Tomcat *versie*.

6. Klik op **Voltooien**.

7. Selecteer **venster** > **Voorkeuren** > **Java** > **geïnstalleerd JREs** > **toevoegen**.

8. Selecteer in de **JRE toevoegen**, **Standaard VM**.

10. Klik op **volgende**.

11. In Java Runtime Environment-definitie JRE thuis, klikt u op **map**.

12. Ga naar **Program Files** > **Java** en selecteer de JDK u eerder hebt geïnstalleerd. Het is belangrijk de JDK als de Java Runtime Environment.

13. Kies de **JDK**in JREs geïnstalleerd. Uw instellingen er ongeveer het volgende screenshot.

    ![][9]

14. Selecteer **venster** > **Web Browser** > **Internet Explorer** voor het openen van de toepassing in een externe browser-venster. Met een externe browser hebt u een toepassing eenvoudiger.

    ![][8]

U hebt nu de configuratietaken voltooid. U zult vervolgens bouwen en uitvoeren van het project.

## <a name="build-the-project"></a>Compileer het project

1. In de Project Explorer, klik met de rechtermuisknop op de naam van het project en kiest u **Uitvoeren als** > **Maven... maken** voor het configureren van het project.

    ![][10]

8. Typ "schone installatie" in configuratie bewerken in de doelstellingen, en klik vervolgens op **uitvoeren**.

Statusberichten zijn uitvoer naar het consolevenster. U ziet bouwen SUCCES aangeeft van het project zonder fouten wordt gebouwd.

## <a name="run-the-app"></a>De toepassing uitvoeren

In deze laatste stap door de toepassing wordt uitgevoerd in een lokale server runtime-omgeving.

Als u nog een server runtime-omgeving in Eclips opgegeven hebt, moet u dat eerst doen.

1. Vouw in de Projectverkenner **webinhoud**.

5. Klik met de rechtermuisknop op **Search.jsp** > **als** > **op de Server worden uitgevoerd**. Selecteer de Apache Tomcat-server en klik vervolgens op **uitvoeren**.

> [AZURE.TIP] Als u een werkruimte niet standaard gebruikt voor het opslaan van uw project, moet u de **Configuratie uitvoert** om te verwijzen naar de projectlocatie om te voorkomen dat een fout opstarten wijzigen. Project Explorer met de rechtermuisknop op **Search.jsp** > **Uitvoeren als** > **Configuraties uitvoeren**. Selecteer de server Apache Tomcat. Klik op de **argumenten**. Klik op **werkruimte** of het **Bestandssysteem** in te stellen van de map met het project.

Wanneer u de toepassing uitvoert, ziet u een browservenster biedt een zoekvak voor het invoeren van voorwaarden.

Wacht ongeveer een minuut voordat u op **Zoeken** om de servicetijd te maken en laden van de index. Als u een HTTP 404-fout krijgt, moet u alleen een beetje langer wachten voordat u het opnieuw probeert.

## <a name="search-on-usgs-data"></a>Zoeken op USG gegevens

De gegevensset USG bevat records die relevant zijn voor de staat Rhode Island. Als u klikt op **Zoeken** op een leeg zoekvak, krijgt u de top 50 posten, de standaardinstelling.

Invoeren van een zoekterm krijgt de zoekmachine iets om op te gaan. Geef de naam van een regionale. "Roger Williams" was de eerste gouverneur van Rhode Island. Talloze parken, gebouwen en scholen zijn naam hem.

![][11]

Ook kunt u een van deze voorwaarden:

- Pawtucket
- Pembroke
- goose + Kaapverdië

## <a name="next-steps"></a>Volgende stappen

Dit is de eerste Azure Search zelfstudie op basis van Java en de dataset USG's. Na verloop van tijd verlengen we deze zelfstudie om extra zoekfuncties die u kunt gebruiken in aangepaste oplossingen aan te tonen.

Als er al een achtergrond in Azure zoeken, kunt u dit voorbeeld als een springboard voor verdere experimenten misschien verbetert zo de [zoekpagina](search-pagination-page-layout.md)of implementatie van [Navigatie beperkt](search-faceted-navigation.md). U kunt ook op de pagina met zoekresultaten verbeteren door tellingen toe te voegen en batchen van documenten, zodat gebruikers de resultaten kunnen doorlopen.

Nieuwe Azure zoeken? Het is raadzaam probeert andere cursussen voor het ontwikkelen van een goed begrip van kunt maken. Ga naar onze [documentatiepagina](https://azure.microsoft.com/documentation/services/search/) als u wilt zoeken naar andere bronnen. U kunt de koppelingen ook bekijken in onze [Video en zelfstudie lijst](search-video-demo-tutorial-list.md) naar access voor meer informatie.

<!--Image references-->
[1]: ./media/search-get-started-java/create-search-portal-1.PNG
[2]: ./media/search-get-started-java/create-search-portal-21.PNG
[3]: ./media/search-get-started-java/create-search-portal-31.PNG
[4]: ./media/search-get-started-java/AzSearch-Java-Import1.PNG
[5]: ./media/search-get-started-java/AzSearch-Java-config1.PNG
[6]: ./media/search-get-started-java/AzSearch-Java-ProjectFacets1.PNG
[7]: ./media/search-get-started-java/AzSearch-Java-runtime1.PNG
[8]: ./media/search-get-started-java/AzSearch-Java-Browser1.PNG
[9]: ./media/search-get-started-java/AzSearch-Java-JREPref1.PNG
[10]: ./media/search-get-started-java/AzSearch-Java-BuildProject1.PNG
[11]: ./media/search-get-started-java/rogerwilliamsschool1.PNG
[12]: ./media/search-get-started-java/AzSearch-Java-SelectProject.png
