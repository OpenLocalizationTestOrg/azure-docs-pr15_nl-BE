<properties
    pageTitle="Het instellen van de Business-verklarende woordenlijst voor tagging geregeld | Microsoft Azure"
    description="De business verklarende woordenlijst in de catalogus van Azure gegevens markeren voor het definiëren en gebruiken van een algemene zakelijke vocabulaire label artikel activa gegevens geregistreerd."
    services="data-catalog"
    documentationCenter=""
    authors="steelanddata"
    manager="NA"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/21/2016"
    ms.author="maroche"/>

# <a name="how-to-set-up-the-business-glossary-for-governed-tagging"></a>Het instellen van de Business-verklarende woordenlijst voor Tagging geregeld

## <a name="introduction"></a>Inleiding

Catalogus Azure gegevens biedt mogelijkheden voor gegevensbron te zoeken, zodat gebruikers gemakkelijk ontdekken en begrijpen van de gegevensbronnen die ze nodig hebben om analyse en beslissingen te nemen. Deze detectie-mogelijkheden maken de grootste impact wanneer gebruikers kunnen vinden en een breed scala aan beschikbare gegevensbronnen te begrijpen.

Het coderen van een catalogus gegevens functie die zorgt voor een beter inzicht in de gegevens van de activa. Tagging kan gebruikers trefwoorden koppelen aan een actief of een kolom die op hun beurt het ontdekken van de activa vereenvoudigt via zoeken of bladeren, en kan gebruikers gemakkelijker begrijpt de context en doel van het activum.

Echter, tagging kan problemen veroorzaken zelf. Enkele voorbeelden van problemen die kunnen worden ingevoerd door labels zijn:

1.  Gebruikers met afkortingen op sommige activa en de uitgebreide tekst op andere tijdens het coderen. Deze inconsistentie belemmert de ontdekking van activa, zelfs als de bedoeling is om de activa te labelen met dezelfde tag.
2.  Labels die verschillende in verschillende contexten betekenissen. Bijvoorbeeld een label genaamd "Opbrengst" op een gegevensset klant omzet per klant kan betekenen, maar hetzelfde label op een driemaandelijkse verkoop dataset omzet per kwartaal voor het bedrijf kan betekenen.  

Catalogus gegevens bevat om te helpen deze en andere vergelijkbare uitdagingen, een Business-verklarende woordenlijst.

De verklarende woordenlijst van gegevens catalogus Business kunnen organisaties document belangrijke zakelijke termen en hun definities voor het maken van een woordenlijst met veelgebruikte zakelijke. Deze governance kunt consistentie in het gebruik van de gegevens in de hele organisatie. Wanneer termen zijn gedefinieerd in de woordenlijst van het bedrijf, ze kunnen worden toegewezen aan activa van de gegevens in de catalogus, met behulp van dezelfde aanpak als labels, waardoor de _codering geregeld_.

> [AZURE.NOTE] De functionaliteit die in dit artikel worden beschreven zijn alleen beschikbaar in de Standard Edition van Azure catalogus gegevens. De gratis versie biedt geen mogelijkheden voor geregeld tagging of de woordenlijst van een bedrijf.

## <a name="glossary-availability-and-privileges"></a>Beschikbaarheid van de woordenlijst en de bevoegdheden

*De verklarende woordenlijst van business is beschikbaar in de Standard Edition van Azure catalogus gegevens. De gratis editie van de catalogus gegevens bevat geen een verklarende woordenlijst.*

Woordenlijst van het bedrijf toegankelijk is via de optie 'Woordenlijst' in het navigatiemenu van de portal gegevenscatalogus.  

![Toegang tot de business-verklarende woordenlijst](./media/data-catalog-how-to-business-glossary/01-portal-menu.png)


Gegevens catalogus beheerders en leden van de groep Administrators van de verklarende woordenlijst kunnen maken, bewerken en verwijderen van termen in de woordenlijst van het bedrijf. Alle gebruikers van de catalogus gegevens kunnen weergeven van de definities van de term en activa met termen kunnen labelen.

![Een nieuwe term in de woordenlijst toevoegen](./media/data-catalog-how-to-business-glossary/02-new-term.png)


## <a name="creating-glossary-terms"></a>Termen uit de woordenlijst maken

Catalogus Gegevensbeheerders en woordenlijst beheerders nieuwe termen uit de woordenlijst maken door te klikken op de nieuwe Term "knop termen uit de woordenlijst maken met de volgende velden:

* De definitie van een bedrijf voor de term
* Een beschrijving die een beeld geeft van het beoogde gebruik of voor zakelijke regels voor de activa/kolom
* Een lijst van belanghebbenden die het meest weten dat de term
* De bovenliggende term waarin de hiërarchie waarin de term is georganiseerd.


## <a name="glossary-term-hierarchies"></a>Verklarende woordenlijst term hiërarchieën

De business Data Catalog woordenlijst biedt de mogelijkheid om uw business-woordenlijst wordt beschreven als een hiërarchie van voorwaarden. Hierdoor kan organisaties een classificatie van de voorwaarden die de taxonomie van hun bedrijf beter te maken.

De naam van een term moet uniek zijn op een bepaald niveau van de hiërarchie - dubbele namen zijn niet toegestaan. Er is geen limiet aan het aantal niveaus in een hiërarchie, maar een hiërarchie is vaak gemakkelijker te begrijpen wanneer er drie niveaus of minder zijn.

Het gebruik van hiërarchieën in de verklarende woordenlijst van business is optioneel. De bovenliggende term veld leeg laten voor termen in de maakt u een vaste (niet-hiërarchische) lijst met termen in de woordenlijst.  

## <a name="tagging-assets-with-glossary-terms"></a>Codering van activa met termen uit de woordenlijst

Zodra de termen in de catalogus zijn gedefinieerd, is de ervaring van labels voor activa als u wilt zoeken de verklarende woordenlijst van de gebruiker typt het label geoptimaliseerd. De portal gegevenscatalogus geeft een lijst met overeenkomende termen uit de woordenlijst voor de gebruiker om uit te kiezen. Als de gebruiker een term in woordenlijst in de lijst selecteert wordt toegevoegd aan het activum als een tag (ook verklarende woordenlijst-tag). De gebruiker kan ook kiezen voor het maken van een nieuwe code door te typen, een term die niet in de woordenlijst (ook gebruiker-tag).

![Gegevens activa gelabeld met één gebruiker tag en twee codes van de verklarende woordenlijst](./media/data-catalog-how-to-business-glossary/03-tagged-asset.png)

> [AZURE.NOTE] Gebruiker-Tags zijn de enige type label wordt ondersteund in de gratis editie van de catalogus met gegevens.

### <a name="hover-behavior-on-tags"></a>Gedrag van labels bij aanwijzen
De twee typen labels zijn in de portal catalogus gegevens visueel onderscheiden met gedragingen van verschillende aanwijzen. Wanneer de gebruiker op een label gebruiker rust ziet zij de labeltekst en de gebruiker of gebruikers die de code hebben toegevoegd. Wanneer de gebruiker op een label woordenlijst rust, zien zij ook de definitie van de term en een koppeling naar de business-verklarende woordenlijst om de volledige definitie van de term weer te openen.

### <a name="search-filters-for-tags"></a>Zoekfilters voor tags
Doorzoekbaar zijn zowel woordenlijst tags en labels van de gebruiker en kunnen worden toegepast als filters in een zoekopdracht.

## <a name="summary"></a>Samenvatting
De verklarende woordenlijst van business in Azure catalogus met gegevens en het geregeld labelen, kunnen activa kunnen worden geïdentificeerd, beheerd en ontdekt op een consistente manier gegevens toestaan. De verklarende woordenlijst van business kunt leren van de woordenschat business onder gebruikers van een organisatie bevorderen en ondersteunt zinvolle meta-gegevens worden vastgelegd, aanbrengen van asset discovery en heel gemakkelijk te begrijpen.

## <a name="see-also"></a>Zie ook

- [REST API-documentatie voor een verklarende woordenlijst van bedrijfsactiviteiten](https://msdn.microsoft.com/library/mt708855.aspx)
