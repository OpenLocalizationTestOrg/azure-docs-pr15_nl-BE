<properties
    pageTitle="Ontwerpfunctie voor weergaven Analytics melden | Microsoft Azure"
    description="Ontwerpfunctie voor weergaven in Analytics logboek kunt u in de console OMS met verschillende visualisaties van gegevens in de bibliotheek OMS aangepaste weergaven maken. Dit artikel bevat een overzicht van de ontwerpfunctie voor weergaven en de procedures voor het maken en bewerken van aangepaste weergaven."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer"></a>Ontwerp van logboek Analytics weergeven
De ontwerpfunctie voor weergaven in Analytics logboek kunt u in de console OMS met verschillende visualisaties van gegevens in de bibliotheek OMS aangepaste weergaven maken. Dit artikel bevat een overzicht van de ontwerpfunctie voor weergaven en de procedures voor het maken en bewerken van aangepaste weergaven.

Andere artikelen voor de ontwerpfunctie voor weergaven beschikbaar zijn:

- [Tegel referentie](log-analytics-view-designer-tiles.md) - verwijzing van de instellingen voor elk van de tegels kunnen worden gebruikt in aangepaste weergaven. 
- [Visualisatie verwijzen](log-analytics-view-designer-parts.md) - verwijzing van de instellingen voor elk van de tegels kunnen worden gebruikt in aangepaste weergaven. 


## <a name="concepts"></a>Concepten
Weergaven maken met de ontwerpfunctie voor weergaven bevatten de elementen in de volgende tabel.

| Deel | Beschrijving |
|:--|:--|
| Naast elkaar | Op het belangrijkste overzicht Analytics dashboard weergegeven.  Omvat een visuele samenvatting van de gegevens in de aangepaste weergave.  Verschillende typen van de tegel bieden verschillende visualisaties van records in de opslagplaats OMS.  Klik op de tegel om de aangepaste weergave te openen. |
| Aangepaste weergave | Weergegeven wanneer de gebruiker op de tegel klikt.  Bevat een of meer onderdelen van de visualisatie. |
| Visualisatie-onderdelen | Visualisatie van gegevens in de OMS-bibliotheek op basis van een of meer [logboekbestanden zoekopdrachten](log-analytics-log-searches.md).  De meeste onderdelen omvatten een Header die een hoog niveau visualisatie bevat en een lijst van de belangrijkste resultaten.  Verschillende typen bieden verschillende visualisaties van records in de opslagplaats OMS.  Klik op de elementen in het gedeelte om een logboek met gedetailleerde records. |

![Overzicht van de ontwerper](media/log-analytics-view-designer/overview.png)

## <a name="add-view-designer-to-your-workspace"></a>Ontwerpfunctie voor weergaven toevoegen aan de werkruimte
Terwijl ontwerp weergeven in voorvertoning, moet u het toevoegen aan uw werkruimte door **Voorvertoning functies** selecteren in de sectie **Instellingen** van de portal OMS.

![Afdrukvoorbeeld inschakelen](media/log-analytics-view-designer/preview.png)

## <a name="creating-and-editing-views"></a>Maken en bewerken van weergaven

### <a name="create-a-new-view"></a>Een nieuwe weergave maken
Open een nieuwe weergave in **Ontwerp weergeven** door te klikken op de tegel ontwerp weergeven in de belangrijkste OMS-dashboard.

![Designer naast elkaar weergeven](media/log-analytics-view-designer/view-designer-tile.png)

### <a name="edit-an-existing-view"></a>Een bestaande weergave bewerken
Als u wilt bewerken in de ontwerpfunctie voor weergaven, de weergave door te klikken op de tegel in de belangrijkste OMS dashboard te openen.  Klik vervolgens op de knop **bewerken** om de weergave in de ontwerpfunctie voor weergaven.

![Een weergave bewerken](media/log-analytics-view-designer/menu-edit.png)

### <a name="clone-an-existing-view"></a>Klonen van een bestaande weergave
Als u een kloon een weergave, wordt een nieuwe weergave gemaakt en geopend in de ontwerpfunctie voor weergaven.  De nieuwe weergave heeft dezelfde naam als de oorspronkelijke met "kopiëren" toegevoegd aan het einde van het.  Klonen van een weergave, de bestaande weergave te openen door te klikken op de tegel in de belangrijkste OMS-dashboard.  Klik vervolgens op de knop **klonen** om de weergave in de ontwerpfunctie voor weergaven.

![Klonen van een weergave](media/log-analytics-view-designer/edit-menu-clone.png)

### <a name="delete-an-existing-view"></a>Een bestaande weergave verwijderen
U verwijdert een bestaande weergave, opent u de weergave door te klikken op de tegel in de belangrijkste OMS-dashboard.  Vervolgens klikt u op de knop **bewerken** om de weergave in de ontwerpfunctie voor weergaven en klik op **Weergave verwijderen**.

![Een weergave verwijderen](media/log-analytics-view-designer/edit-menu-delete.png)

### <a name="export-an-existing-view"></a>Een bestaande weergave exporteren
U kunt een weergave exporteren naar een JSON-bestand dat u kunt importeren in een andere werkruimte of in een [sjabloon Azure Resource Manager](../resource-group-authoring-templates.md)gebruiken.  Als u wilt exporteren een bestaande weergave, opent u de weergave door te klikken op de tegel in de belangrijkste OMS-dashboard.  Klik vervolgens op de knop **exporteren** om een in de downloadmap van de browser.  De naam van het bestand moet de naam van de weergave met de extensie *omsview*.

![Een weergave exporteren](media/log-analytics-view-designer/edit-menu-export.png)

### <a name="import-an-existing-view"></a>Importeren van een bestaande weergave
U kunt een bestand *omsview* dat u hebt geëxporteerd uit een andere management groep importeren.  Als u wilt importeren in een bestaande weergave, maakt u eerst een nieuwe weergave.  Klik op de knop **importeren** en selecteer het bestand *omsview* .  De configuratie in het bestand wordt gekopieerd naar de bestaande weergave.

![Een weergave exporteren](media/log-analytics-view-designer/edit-menu-import.png)

## <a name="working-with-view-designer"></a>Werken met de ontwerpfunctie voor weergaven
De ontwerper van het beeld bestaat uit drie deelvensters.  Het deelvenster **ontwerp** geeft de aangepaste weergave.  Wanneer u tegels en onderdelen vanuit het deelvenster **besturingselement** naar **het ontwerpvenster toevoegen** worden ze toegevoegd aan de weergave.  Het deelvenster **Eigenschappen** worden de eigenschappen van de tegel of het geselecteerde onderdeel weergegeven.

![Ontwerp weergeven](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Configureren weergave naast elkaar
Een aangepaste weergave kan slechts één tegel hebben.  Selecteer het tabblad **naast elkaar** in het deelvenster **beheer** om het huidige venster weergeven of Selecteer een alternatief.  Het deelvenster **Eigenschappen** worden de eigenschappen voor het huidige venster weergegeven.  De eigenschappen naast elkaar op basis van de gedetailleerde informatie in de [Tegel verwijzing](log-analytics-view-designer-tiles.md) configureren en klik op **toepassen** om de wijzigingen opslaan.

### <a name="configure-visualization-parts"></a>Visualisatie-onderdelen configureren
Een weergave kan een willekeurig aantal visualisatie onderdelen bevatten.  Selecteer het tabblad **weergave** en visualisatie-onderdeel toe te voegen aan de weergave.  Het deelvenster **Eigenschappen** worden de eigenschappen voor het geselecteerde onderdeel weergegeven.  De weergave eigenschappen op basis van de gedetailleerde informatie in de [visualisatie verwijzen](log-analytics-view-designer-parts.md) configureren en klik op **toepassen** om de wijzigingen opslaan.

### <a name="delete-a-visualization-part"></a>Visualisatie-onderdeel verwijderen
Door te klikken op de knop **X** in de rechterbovenhoek van het gedeelte kunt u een deel van de visualisatie uit de weergave verwijderen.

### <a name="rearrange-visualization-parts"></a>Visualisatie onderdelen anders rangschikken
Weergaven hebben slechts één rij van visualisatie delen.  Bestaande onderdelen in een weergave wijzigen door te klikken en deze naar een nieuwe locatie te slepen.


## <a name="next-steps"></a>Volgende stappen

- [Tegels](log-analytics-view-designer-tiles.md) toevoegen aan de aangepaste weergave.
- [Visualisatie onderdelen](log-analytics-view-designer-parts.md) toevoegen aan de aangepaste weergave.
