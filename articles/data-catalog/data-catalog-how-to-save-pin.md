<properties
   pageTitle="Sla zoekopdrachten en vastmaken gegevens activa | Microsoft Azure"
   description="Hoe kan ik artikel mogelijkheden in de catalogus van Azure gegevens markeren voor gegevensbronnen en gegevens activa voor later gebruik opslaan."
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
   ms.date="10/10/2016"
   ms.author="maroche"/>

# <a name="how-to-save-searches-and-pin-data-assets"></a>Zoekacties opslaan en de gegevens activa vastmaken

## <a name="introduction"></a>Inleiding

Catalogus gegevens van Microsoft Azure biedt mogelijkheden voor gegevensbron te zoeken. Gebruikers kunnen snel zoeken en filteren van de catalogus om gegevensbronnen te begrijpen van het beoogde gebruik, waardoor het gemakkelijker wordt om te zoeken naar de juiste gegevens voor de taak bij de hand.

Maar hoe zit wanneer gebruikers moeten regelmatig werken met dezelfde gegevens? Wat gebeurt er wanneer gebruikers regelmatig hun kennis bijdragen aan de dezelfde gegevensbronnen in de catalogus? In situaties als deze hebben er herhaaldelijk de dezelfde zoekopdrachten niet efficiënt kan worden – dit is waar zoekactie opgeslagen en vastgehouden gegevens activa kunnen helpen.

## <a name="saved-searches"></a>Opgeslagen zoekopdrachten

Een opgeslagen zoekactie in de catalogus gegevens Azure is een herbruikbare, per gebruiker zoeken-definitie. Nadat u een zoekopdracht – zoals zoektermen, labels en andere filters – gedefinieerd kan hij het opslaan voor toekomstig gebruik. De definitie opgeslagen zoekopdracht kan vervolgens opnieuw worden uitgevoerd op een later tijdstip te retourneren gegevens activa die aan de zoekcriteria voldoen.

### <a name="creating-a-saved-search"></a>Een opgeslagen zoekactie maken

Een opgeslagen zoekactie maken, voert eerst de zoekcriteria opnieuw worden gebruikt. Klik vervolgens op de link 'Opslaan' in het vak 'Huidige zoeken' in de catalogus van Azure gegevens portal.

 ![Selecteer 'opslaan' om de huidige instellingen van de zoekopdracht opslaan](./media/data-catalog-how-to-save-pin/01-save-option.png)

Voer desgevraagd een naam voor de opgeslagen zoekopdracht. Kies een naam die relevant en beschrijvend is van de gegevens activa die worden geretourneerd door de zoekopdracht.

 ![Geef een naam voor de opgeslagen zoekopdracht](./media/data-catalog-how-to-save-pin/02-name.png)

### <a name="managing-saved-searches"></a>Beheren van opgeslagen zoekopdrachten

Zodra een gebruiker een of meer zoekopdrachten opgeslagen heeft, verschijnt een optie "Opgeslagen zoekopdrachten" in de catalogus van Azure gegevens portal onder het vak 'Huidige zoeken'. Als het is uitgevouwen, wordt de volledige lijst van opgeslagen zoekopdrachten worden weergegeven.

 ![Lijst met opgeslagen zoekopdrachten](./media/data-catalog-how-to-save-pin/03-list.png)

Een opgeslagen zoekactie selecteren in de lijst, wordt de zoekactie moet worden uitgevoerd.

Selecteren van het menu biedt een aantal opties:

 ![Opties voor het beheren van opgeslagen zoekopdrachten](./media/data-catalog-how-to-save-pin/04-managing.png)

"Naam" selecteren wordt de gebruiker gevraagd een nieuwe naam voor de opgeslagen zoekopdracht invoeren. De definitie van de zoekactie wordt niet gewijzigd.

De gebruiker om bevestiging wordt gevraagd 'Verwijderen' selecteren en vervolgens de opgeslagen zoekopdracht wordt verwijderd uit de lijst.

Selecteert 'Opslaan als standaard' markeert de zoekopdracht opgeslagen als de standaard zoekactie naar de gebruiker gekozen. Als een "lege" zoekopdracht vanaf de introductiepagina van de catalogus met Azure gegevens uitvoert, zal de zoekopdracht van de gebruiker worden uitgevoerd. Bovendien zoeken als standaard wordt weergegeven aan de bovenkant van de lijst van opgeslagen zoekopdracht gemarkeerd.

### <a name="organizational-saved-searches"></a>Organisatie-opgeslagen zoekopdrachten

Elke gebruiker kan zoekacties voor eigen gebruik opslaan. Gegevensbeheerders catalogus kunnen ook zoekopdrachten voor alle gebruikers binnen de organisatie opslaan. Wanneer u een zoekopdracht opslaat, worden de beheerders gepresenteerd met een optie voor het delen van de opgeslagen zoekopdracht binnen het bedrijf. Als deze optie is geselecteerd, wordt de opgeslagen zoekopdracht worden opgenomen in de lijst met beschikbare zoekopdrachten voor alle gebruikers.

 ![Organisatie-opgeslagen zoekopdrachten](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)


## <a name="pinned-data-assets"></a>Gegevens met vaste activa

Opgeslagen zoekopdrachten kunnen gebruikers opslaan en hergebruiken zoeken definities; de gegevens activa die worden geretourneerd door de zoekopdrachten kunnen wijzigen als de inhoud van de catalogus wijzigen. Vastzetten gegevens activa kan gebruikers expliciet identificeren activa specifieke gegevens zodat ze gemakkelijker toegang zonder gebruik van een zoekopdracht.

Vastzetten van een activum gegevens is eenvoudig: gebruikers kunnen gewoon klikken op het pictogram 'pin' voor de gegevens activa toe te voegen aan hun lijst met vastgehouden. Dit pictogram wordt weergegeven in de linkerbenedenhoek van het activum naast elkaar plaatsen in de weergave naast elkaar en in de meest linkse kolom in de lijstweergave van de catalogus met Azure gegevens portal.

![Een activum gegevens vastzetten](./media/data-catalog-how-to-save-pin/05-pinning.png)

Een actief unpinning is even eenvoudig – gebruikers klikt u op het pictogram 'pin' opnieuw uit om te schakelen tussen de instellingen voor de geselecteerde activa.

![Een activum gegevens unpinning](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="my-assets"></a>"Mijn activa"
De introductiepagina van de catalogus gegevens Azure portal bevat een sectie "Mijn activa" waarin elementen van belang voor de huidige gebruiker. Deze sectie bevat zowel vaste activa en opgeslagen zoekopdrachten.

!["Mijn activa" op de startpagina](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a>Samenvatting
Catalogus Azure gegevens biedt mogelijkheden waardoor het gemakkelijker voor gebruikers om te ontdekken de gegevensbronnen die ze nodig hebben, zodat u kunt besteden minder tijd op zoek naar gegevens en meer tijd ermee te werken. Opgeslagen zoekopdrachten en vastgehouden gegevens activa op deze kernfuncties bouwen zodat gebruikers gemakkelijk gegevensbronnen waarmee ze herhaaldelijk werken kunnen herkennen.
