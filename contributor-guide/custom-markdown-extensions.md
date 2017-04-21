<properties
    title="required"
    pageTitle="Prijsverlaging aangepaste extensies die worden gebruikt in onze technische artikelen"
    description="Hier worden de aangepaste prijsverlaging-extensies inschakelen voor ingesloten video's, notities en tips herbruikbare inhoud en andere items in de technische artikelen voor azure.microsoft.com."
    services=""
    solutions=""
    documentationCenter=""
    authors="tysonn"
    manager="carolz"
    editor=""/>

<tags
    ms.service="contributor-guide"
    ms.devlang=""
    ms.topic="article"
    ms.tgt_pltfrm=""
    ms.workload=""
    ms.date="01/22/2015"
    ms.author="tysonn"/>

## <a name="markdown-for-azuremicrosoftcom"></a>Prijsverlaging voor Azure.microsoft.com

Zie voor algemene prijsverlaging tips [Prijsverlaging basisbeginselen](https://help.github.com/articles/markdown-basics/) en onze [Cheatsheet voor prijsverlaging](./media/documents/markdown-cheatsheet.pdf?raw=true). Artikel kruiskoppelingen maken in prijsverlaging, Zie [koppelen richtlijnen] (. / create-links-markdown.md#markdown-syntax-for-acom-relative-links.md/).

Azure.Microsoft.com ondersteunt [codeblokken omheind](https://help.github.com/articles/github-flavored-markdown/#fenced-code-blocks) en [syntaxismarkering](https://help.github.com/articles/github-flavored-markdown/#syntax-highlighting). ACOM ondersteunt echter slechts één syntaxis kleurenschema, ongeacht de taal die u in een codeblok markeren.

## <a name="custom-markdown-extensions-used-in-our-technical-articles"></a>Prijsverlaging aangepaste extensies die worden gebruikt in onze technische artikelen

Onze artikelen gebruiken GitHub flavored prijsverlaging punten voor de meeste artikel opmaak -, koppelingen, lijsten, koppen, enz. Maar we prijsverlaging aangepaste extensies waar we moeten rijkere opmaak in de gerenderde pagina's op azure.microsoft.com. Hier ziet u de extensies die we gebruiken:

+ [Opmerkingen en tips]
+ [Bevat]
+ [Ingesloten video 's]
+ [Technologie en platform selectors]

## <a name="notes-and-tips"></a>Opmerkingen en tips

U kunt kiezen uit 4 typen opmerkingen en tips:

- AZURE. OPMERKING
- AZURE. WAARSCHUWING
- AZURE. TIPss
- AZURE. BELANGRIJK

###<a name="usage"></a>Gebruik
Gebruik in het algemeen, opmerkingen en tips spaarzaam in uw artikelen. Wanneer u ze gebruikt, kiest u het juiste type opmerking of tip:

- AZURE gebruiken. Opmerking voor de neutrale of positieve gegevens markeren die benadrukt of aanvullingen van de belangrijkste punten van de tekst. Een notitie bevat informatie die van toepassing slechts in bijzondere gevallen is.

  ![](./media/custom-markdown-extensions/Notes-note.PNG)

- AZURE gebruiken. Waarschuwing: de gebruiker aan een voorwaarde die in de toekomst een probleem kan veroorzaken. Bijvoorbeeld een bepaalde optie te selecteren of het maken van een bepaalde keuze mogelijk definitief vergrendeld u in een bepaald scenario.

  ![](./media/custom-markdown-extensions/Notes-warning.PNG)

- AZURE gebruiken. TIP waarmee gebruikers de technieken en procedures die worden beschreven in de tekst op hun specifieke behoeften passen. Een tip misschien ook alternatieve methoden die niet voor de hand liggende voorstellen. Tips, echter zijn niet essentieel voor de elementaire kennis van de tekst.

  ![](./media/custom-markdown-extensions/Notes-tip.PNG)

- AZURE gebruiken. BELANGRIJKE informatie die essentieel is voor de voltooiing van een taak.

  ![](./media/custom-markdown-extensions/Notes-important.PNG)

Terwijl deze opmerkingen en tips ondersteuning codeblokken, afbeeldingen, lijsten en koppelingen, probeer te houden van uw opmerkingen en tips eenvoudig en ongecompliceerd. Als u merkt dat complexe notities maken met veel opmaak, kan dat een teken dat u hoeft alleen maar een andere sectie in de hoofdtekst van het artikel zijn. En te veel notities in een artikel kunnen worden afleidende en moeilijk te scannen of te lezen.

###<a name="sample-markdown"></a>Monster prijsverlaging

De monsters van alle tonen een AZURE. HOUD ER REKENING MEE. Voor het gebruik van een TIP, waarschuwing of belangrijke 'Opmerking' in de prijsverlaging te vervangen:

    > [AZURE.TIP]

    > [AZURE.WARNING]

    > [AZURE.IMPORTANT]

Enkele alinea:

    > [AZURE.NOTE] Deze zelfstudie hebt u een actieve account met Microsoft Azure. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten.

Multiparagraph:

    > [AZURE.NOTE] Deze zelfstudie hebt u een actieve account met Microsoft Azure.
    >
    > Als u geen account hebt, kunt u [een gratis proefperiode account maken](http://www.windowsazure.com/pricing/free-trial/) binnen een paar minuten.

## <a name="includes"></a>Bevat

Herbruikbare tekst in onze opslagplaats GitHub bevindt zich in de bestanden dat we noemen "bevat". Wanneer u de tekst die moet worden gebruikt in meerdere artikelen hebt, kunt u een verwijzing naar dat bestand van herbruikbare informatie opnemen. Het opnemen zelf is een eenvoudige prijsverlaging (.md)-bestand. Dit kan elke geldige prijsverlaging, inclusief tekst, koppelingen en afbeeldingen bevatten. Alle bestanden moeten in prijsverlaging bevatten [de / directory omvat](https://github.com/Azure/azure-content/tree/master/includes) in de hoofdmap van de opslagplaats. Wanneer het artikel is gepubliceerd, wordt de tekst opnemen naadloos geïntegreerd in het onderwerp gepubliceerd.

- We gebruiken een specifieke syntaxis om te verwijzen naar een opnemen.

- Media-bestanden in een opnemen plaatst moeten worden gemaakt in een map media specifiek voor het opnemen. Mappen voor Media bevat in [de map azure inhoud/bevat/media](https://github.com/Azure/azure-content/tree/master/includes/media)behoren. De map media mogen geen afbeeldingen in de hoofdmap. Een bijbehorende map in de media is niet vereist als de opnemen geen afbeeldingen.

###<a name="usage"></a>Gebruik

- Gebruik omvat waar nodig dezelfde tekst in meerdere artikelen wilt weergeven.

- Bevat zijn bedoeld om te worden gebruikt voor grote hoeveelheden inhoud - een alinea of twee, een gedeelde procedure of een gedeelde sectie. Gebruik ze niet voor kleiner is dan een zin; **ze zijn niet voor productnamen**.

- Controleer alle tekst in een opnemen is geschreven in volledige zinnen of zinsdelen die niet afhankelijk van voorafgaande of volgende tekst in het artikel dat verwijst naar de opnemen. Negeren van deze instructies maakt een untranslatable tekenreeks in het artikel die de gelokaliseerde ervaring. 

- Niet insluiten in andere bevat bevat. Ze worden niet ondersteund door DPS publishing systeem.

- Geen media tussen bestanden delen. Gebruik een afzonderlijk bestand met een unieke naam voor elke opnemen en het artikel. Het bestand opslaan in de map media die zijn gekoppeld aan het opnemen.

- Gebruik niet een opnemen als de enige inhoud van een artikel.  Bevat zijn bedoeld als aanvulling op de inhoud van de rest van het artikel.

- Omdat alle moet in de / directory, inclusief het pad naar een opnemen van een artikel is altijd

    .. / bevat

- De verwijzing naar een koppeling of afbeelding bestandsnaam in zowel het artikel en het opnemen niet herhaalt. Toevoegen "-opnemen" aan de bestandsnaam koppeling referentie- of herhaling van de verwijzing:

 **Verwijzing naar de koppeling**

 Wijzigen: odata.org te: odata.org opnemen

 **Verwijzing naar afbeelding**

 Wijzigen: table.png te: tabel include.png

###<a name="sample-markdown"></a>Monster prijsverlaging
De syntaxis voor het toevoegen van een opnemen naar een artikel in de documentatie is:

    [AZURE.INCLUDE [include-short-name](../includes/include-file-name.md)]

Voorbeeld

    [AZURE.INCLUDE [howto-blob-storage](../includes/howto-blob-storage.md)]

Het eerste deel van het opnemen is de naam opnemen zonder het pad en de extensie .md. Het tweede deel is het relatieve pad naar het opnemen in het / de map met de extensie .md bevat.

###<a name="rendering"></a>Opbouw

In de pagina GitHub, zal het opnemen als volgt opgebouwd:

 [AZURE. Procedure-blob-opslag zijn]

In de gegenereerde HTML op azure.microsoft.com, de HTML-code van de bevat in de rest van de HTML-code van het document is samengevoegd. De HTML-code bevat, een HTML-code aan de oorspronkelijke commentaar opnemen prijsverlaging bestandsnaam en de GitHub commit-hash. Deze opmerking is opgenomen voor het oplossen van problemen zodat de broninhoud gemakkelijk kan worden geïdentificeerd en in GitHub gevonden:

  ![](./media/custom-markdown-extensions/include.png)


## <a name="embedded-videos"></a>Ingesloten video 's

Als de video's op de site van Microsoft's [Channel 9 worden](http://channel9.msdn.com/) , onze technische artikelen embeddeded video's ondersteunen in de technische artikelen. De video's van kanaal 9 moeten worden geïntegreerd met [de azure.microsoft.com Video Center](http://azure.microsoft.com/documentation/videos/home/). Wij bieden momenteel geen ondersteuning voor ingesloten YouTube-video's; Als u een medewerker van de Gemeenschap, zijn Welkom bij YouTube koppelen als de gewenste functie video er is geboekt. Medewerkers van Microsoft gebruik Channel 9 en het midden van de Video.

### <a name="usage"></a>Gebruik

- Zorg ervoor dat de video in het midden van de Video.

- Kopieer de video-ID van de beschrijvende URL van de video op Channel 9 of vanuit het midden van de Video Azure. De video-ID voor de video op [http://azure.microsoft.com/documentation/videos/azure-scheduler-unusual-schedules/](http://azure.microsoft.com/documentation/videos/azure-scheduler-unusual-schedules/) is bijvoorbeeld **azure-planner-ongebruikelijke-schema's**.

### <a name="syntax"></a>Syntaxis

    > [AZURE.VIDEO video-id-string]

### <a name="rendering"></a>Opbouw

Op GitHub: [https://github.com/Azure/azure-content-pr/blob/master/articles/web-sites-backup.md](https://github.com/Azure/azure-content-pr/blob/master/articles/web-sites-backup.md)

Gepubliceerde artikel: [http://azure.microsoft.com/documentation/articles/web-sites-backup/](http://azure.microsoft.com/documentation/articles/web-sites-backup/)


## <a name="technology-and-platform-selectors"></a>Technologie en platform selectors

Technologie en platform switchers in technische artikelen gebruiken wanneer u meerdere versies van hetzelfde artikel verschillen in implementatie adres via technologieën of platforms ontwerpt. Dit is meestal meest van toepassing is op de inhoud van onze mobiele platform voor ontwikkelaars. Er zijn twee verschillende soorten selectors, [eenvoudige selectors](#simple-selectors) en [bilaterale selectors](#two-way-selectors).

Omdat het dezelfde selector prijsverlaging in elk onderwerp in de selectie gaat, wordt aangeraden het plaatsen van de selector voor uw onderwerp in een opnemen en vervolgens verwijzen naar deze opnemen in alle van de onderwerpen die de kiezer dezelfde gebruiken.

###<a id="simple-selectors"></a>Eenvoudige selectors

Eenvoudige (één richting) selectoren worden weergegeven als een reeks keuzerondjes direct onder de titel. Gebruik deze knoppen wanneer klanten nodig hebben om te kiezen uit de onderwerpen in een enkel platform of technologie, zoals .NET Node.js en Java.  Gebruik de extensie aangepaste prijsverlaging voor alle kiezers - geen HTML gebruiken voor de kiezers.  

Zie [aan de slag met Hubs melding](http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started/) te zien hoe de auteur 8 versies van het artikel, maar gebruikte kiezers om te navigeren tussen alle gemaakt.

![Voorbeeld van een eenvoudige selector](./media/custom-markdown-extensions/selectors.PNG)

####<a name="syntax"></a>Syntaxis

    > [AZURE.SELECTOR]
    - [Koppeling Label #1](link #1 url)
    - [Link #2-Label](link #2 url)

Voorbeeld:

    > [AZURE.SELECTOR]
    - [Universele Windows](../articles/notification-hubs-windows-store-dotnet-get-started/)
    - [Windows Phone](../articles/notification-hubs-windows-phone-get-started/)
    - [i/o](../articles/notification-hubs-ios-get-started/)
    - [Android](../articles/notification-hubs-android-get-started/)
    - [Kindle](../articles/notification-hubs-kindle-get-started/)
    - [Baidu](../articles/notification-hubs-baidu-get-started/)
    - [Xamarin.iOS](../articles/partner-xamarin-notification-hubs-ios-get-started/)
    - [Xamarin.Android](../articles/partner-xamarin-notification-hubs-android-get-started/)

#### <a name="rendering"></a>Opbouw

De afbeelding hierboven ziet u de weergave op azure.microsoft.com. Op de weergegeven pagina's GitHub de kiezers worden weergegeven als een lijst met opsommingstekens van koppelingen.

###<a id="two-way-selectors"></a>Twee richtingen selectors

Twee richtingen kiezers kunnen gebruikers een onderwerpen kiezen uit een matrix met twee richtingen. Dit is noodzakelijk wanneer een Azure-technologie, zoals mobiele Services, meerdere backend platforms en meerdere clients ondersteunt. Houd rekening met het volgende:

- Terwijl het was bedoeld als `(Platform | Backend)`, de dropwdown tekst kan nu worden aangepast.
- U hoeft niet een item in de lijst voor elk punt in de matrix, maar hoeft alleen een item waar een onderwerp URL bestaat en niet een duplicaat.
- De koppeling is een URL zijn, hoewel het in het algemeen een ander GitHub onderwerp is.

Zie [aan de slag met mobiele Services](http://azure.microsoft.com/en-us/documentation/articles/mobile-services-ios-get-started/) om te zien hoe de auteur 15 versies van het artikel (9 mobiele clientplatforms en 2 backend platforms), maar gebruikte kiezers om te navigeren tussen alle gemaakt. Houd er rekening mee dat 3 artikelen niet beide versies van de back-end.

![Voorbeeld van de twee richtingen selectors](./media/custom-markdown-extensions/selector-list.png)

####<a name="syntax"></a>Syntaxis

    > [AZURE. LIJST EENHEDENSELECTIE (Dropdown1 | Dropdown2)]     -  [(Dropdown1Text1 | Dropdown2Text1)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text1 | Dropdown2Text2)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text2 | Dropdown2Text3)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text3 | Dropdown2Text4)](../articles/dropdown1-text1-dropdown2-text1.md)

Voorbeeld:

    > [AZURE. LIJST EENHEDENSELECTIE (Platform | Back-end)]     -  [(iOS | .NET)](./mobile-services-dotnet-backend-ios-get-started-push.md)
    - [(iOS | JavaScript)](./mobile-services-javascript-backend-ios-get-started-push.md)
    - [(Windows universal C# | .NET)](./mobile-services-dotnet-backend-windows-universal-dotnet-get-started-push.md)
    - [(Windows universal C# | JavaScript)](./mobile-services-javascript-backend-windows-universal-dotnet-get-started-push.md)
    - [(Windows Phone | .NET)](./mobile-services-dotnet-backend-windows-phone-get-started-push.md)
    - [(Windows Phone | JavaScript)](./mobile-services-javascript-backend-windows-phone-get-started-push.md)
    - [(Android | .NET)](./mobile-services-dotnet-backend-android-get-started-push.md)
    - [(Android | JavaScript)](./mobile-services-javascript-backend-android-get-started-push.md)
    - [(Xamarin iOS | JavaScript)](./partner-xamarin-mobile-services-ios-get-started-push.md)
    - [(Xamarin Android | JavaScript)](./partner-xamarin-mobile-services-android-get-started-push.md)

#### <a name="rendering"></a>Opbouw

De afbeelding hierboven ziet u de weergave op azure.microsoft.com. Op de weergegeven pagina's GitHub de kiezers worden weergegeven als een lijst met opsommingstekens van koppelingen.

<!--Anchors-->
[Opmerkingen en tips]: #notes-and-tips
[Bevat]: #includes
[Ingesloten video 's]: #embedded-videos
[Technologie en platform selectors]: #technology-and-platform-selectors

###<a name="contributors-guide-links"></a>Medewerkers handleiding voor koppelingen

- [Overzichtsartikel](./../README.md)
- [Index van artikelen](./contributor-guide-index.md)
