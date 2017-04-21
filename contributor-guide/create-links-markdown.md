<properties
   pageTitle="Koppelingen maken in artikelen prijsverlaging" description="Wordt uitgelegd hoe u code kruiskoppelingen in prijsverlaging." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="02/03/2015" ms.author="tysonn" />

# <a name="linking-guidance-for-azure-technical-content"></a>Richtlijnen voor Azure technische inhoud koppelen

### <a name="links-from-one-acom-article-to-another"></a>Koppelingen van één ACOM artikel naar het andere

Een in line als koppeling wilt maken vanuit een technisch artikel ACOM op een ander ACOM technische artikel, gebruikt u de volgende syntaxis voor de koppeling:  

- Een artikel in een directory service een koppeling naar een ander artikel in dezelfde directory service:

  `[link text](article-name.md)`

- Een artikel koppelingen van een submap service naar een artikel in de hoofdmap:

  `[link text](../article-name.md)`

- Een artikel in de root directory koppelingen aan een artikel in een submap van de service: 

  `[link text](./service-directory/article-name.md)`

- Een artikel in een submap service een koppeling naar een artikel in een andere submap van de service:

  `[link text](../service-directory/article-name.md)`
 

## <a name="links-to-anchors"></a>Koppelingen met ankers

U hoeft niet te worden ankers maken - ze automatisch worden gegenereerd bij het publiceren van tijd voor alle H2 koppen. Het enige wat dat u hoeft te doen is het maken van koppelingen naar de secties H2.

- Koppelen aan een post van dat artikel:

  `[link](#the-text-of-the-H2-section-separated-by-hyphens)`  
  `[Create cache](#create-cache)`

- Koppelen aan een anker in een ander artikel in de submap dezelfde:

  `[link text](article-name.md#anchor-name)`
  `[Configure your profile](media-services-create-account.md#configure-your-profile)`

- Koppelen naar een anker in een submap van een andere service:

  `[link text](../service-directory/article-name.md#anchor-name)`
  `[Configure your profile](../service-directory/media-services-create-account.md#configure-your-profile)`

Een manier voor het automatiseren van het maken van koppelingen in uw artikelen aan de koppelingen automatisch gegenereerde anker is [MarkdownAnchorLinkGenerator - een hulpmiddel voor het genereren van koppelingen naar anker voor ACOM in de juiste indeling](https://github.com/Azure/Azure-CSI-Content-Tools/tree/master/Tools/ACOMMarkdownAnchorLinkGenerator).

## <a name="links-from-includes"></a>Bevat koppelingen van

Sindsdien zijn bestanden bevinden zich in een andere map, moet u meer relatieve paden gebruiken zoals hieronder wordt weergegeven. Als u wilt koppelen aan een artikel van een include-bestand, gebruikt u deze indeling:

    [link text](../articles/service-folder/article-name.md)
    
Meer informatie over het gebruik van een include-bestand in de [richtlijnen voor aangepaste prijsverlaging extensies](custom-markdown-extensions.md#includes).

## <a name="links-in-selectors"></a>Koppelingen in selectors

Als u de kiezers die zijn ingesloten in een opnemen, gebruikt u dit soort koppelen: 

    > [AZURE. LIJST EENHEDENSELECTIE (Dropdown1 | Dropdown2)]     -  [(Tekst1 | Voorbeeld1)](../articles/service-folder/article-name1.md)
    - [(Tekst1 | Voorbeeld2)](../articles/service-folder/article-name2.md)
    - [(Tekst2 | Voorbeeld3)](../articles/service-folder/article-name3.md)
    - [(Tekst2 | Voorbeeld4)](../articles/service-folder/article-name4.md)


## <a name="reference-style-links"></a>Verwijzingstype koppelingen

Verwijzing stijl koppelingen kunt u uw broninhoud gemakkelijker te lezen. De syntaxis van de link in line vervangen stijl verwijzing koppelingen door vereenvoudigde syntaxis waarmee u de lange URL's verplaatsen naar het einde van het artikel. Hier is een voorbeeld van durf Vuurbal:

Inlinetekst:

    I get 10 times more traffic from [Google][1] than from [Yahoo][2] or [MSN][3].

De referenties van de koppeling aan het einde van het artikel:

    <!--Reference links in article-->
    [1]: http://google.com/
    [2]: http://search.yahoo.com/  
    [3]: http://search.msn.com/

Zorg ervoor dat u de spatie na de dubbele punt, voor de koppeling. Wanneer u een koppeling naar andere technische artikelen, als u vergeet de spatie, wordt de koppeling opgesplitst in een gepubliceerd artikel. 

## <a name="link-to-acom-pages-that-are-not-part-of-the-technical-documentation-set"></a>Koppeling met ACOM's die geen deel uitmaken van de technische documentatieset

Als u wilt koppelen aan een pagina op ACOM (zoals een pagina prijzen SLA pagina of iets anders dat is niet een documentatie-artikel), een absolute URL gebruiken, maar de landinstelling weglaten. Het doel hier is dat werk koppelingen in GitHub en op de site weergegeven:

    [link text](http://azure.microsoft.com/pricing/details/virtual-machines/)


## <a name="link-to-msdn-or-technet"></a>Koppeling naar MSDN of TechNet

Wanneer u een koppeling naar MSDN of TechNet, gebruikt u de volledige koppeling naar het onderwerp en verwijder de nl-ons de taalinstelling van de koppeling. 

### <a name="use-friendly-link-text-for-all-links"></a>Met beschrijvende koppelingstekst voor alle koppelingen gebruiken

De woorden die u in een koppeling opneemt moeten worden beschrijvende - met andere woorden, ze moeten normale Engelse woorden of de titel van de pagina die u wilt koppelen. Gebruik geen "Klik hier". Het is slecht voor Zoekmachineoptimalisatie en niet afdoende Omschrijf het doel.

**Corrigeren:**

- `For more information, see the [contributor guide index](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).`

- `For more details, see the [SET TRANSACTION ISOLATION LEVEL](https://msdn.microsoft.com/library/ms173763.aspx) reference.`

**Onjuist:**

- `For more details, see [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).`

- `For more information, click [here](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).`


## <a name="fwlinks"></a>FWLinks

FWLinks (onze omleiding systeem) voorkomen in de inhoud van de azure.microsoft.com. Ze moeten alleen als laatste redmiddel gebruikt wanneer u een koppeling voor een pagina waarvan de URL nog niet weet te maken. Ze zijn bijna nooit echt nodig. Voor ACOM definieert u de naam van het bestand, zodat u weet wat deze tijdig worden. Voor een bibliotheek-onderwerp dat nog niet is gepubliceerd, kunt u een koppeling met de GUID van het onderwerp, zodat u niet hoeft te gebruiken een fwlink-verhaal over.

Als u een fwlink-verhaal over op een webpagina, moet zijn dan de parameter P om het te maken een permanente omleiding:

    http://go.microsoft.com/fwlink/p/?LinkId=389595

Wanneer u de doel-URL in het hulpprogramma fwlink-verhaal over plakt, moet u de landinstelling verwijderen als de koppeling target ACOM, of de MSDN of TechNet library.

## <a name="remember-the-azure-library-chrome"></a>Vergeet niet het chrome Azure bibliotheek!
Als u koppelen aan een Azure library-onderwerp dat zich onder [dit knooppunt wilt](https://msdn.microsoft.com/library/azure), moet u opgeven de Azure chroom in de koppeling (/ azure /). De Azure chroom deelt de navigatieopties ACOM en wordt alleen de Azure inhoud van de MSDN-bibliotheek. Een goed bereik koppeling ziet er zo uit:

    http://msdn.microsoft.com/library/azure/dd163896.aspx

Anders wordt worden de pagina weergegeven in de standaardweergave van MSDN met de gehele MSDN-structuur weergegeven.

### <a name="contributors-guide-links"></a>Medewerkers handleiding voor koppelingen

- [Overzichtsartikel](./../README.md)
- [Index van artikelen](./contributor-guide-index.md)

<!--image references-->
[1]: ./media/create-tables-markdown/table-markdown.png
[2]: ./media/create-tables-markdown/break-tables.png
