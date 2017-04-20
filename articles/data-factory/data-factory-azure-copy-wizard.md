<properties
    pageTitle="Wizard Data Factory-Azure kopiëren | Microsoft Azure"
    description="Meer informatie over de Wizard Data Factory Azure kopiëren gebruiken om gegevens te kopiëren van de ondersteunde gegevensbronnen te putten."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="spelluru"/>

# <a name="azure-data-factory---copy-wizard"></a>Azure Data Factory - Wizard kopiëren
De Azure fabriek kopie Wizard is het vereenvoudigen van het proces van ingesting gegevens, die meestal is dit een eerste stap in een scenario van gegevens voor end-to-end integratie. Bij gebruik van de Wizard Azure Data Factory kopiëren, hoeft u niet te begrijpen alle JSON definities voor gekoppelde services, datasets en pijpleidingen. Echter, nadat u de stappen in de wizard hebt voltooid, de wizard maakt automatisch een pijplijn gegevens uit de geselecteerde gegevensbron kopiëren naar de geselecteerde bestemming. Bovendien kopiëren, de Wizard begeleidt u bij het valideren van gegevens op het moment van schrijven, wordt ingenomen wordt veel van uw tijd, vooral wanneer u zijn ingesting gegevens voor de eerste keer uit de gegevensbron. U start de Wizard kopiëren, klikt u op de tegel **kopiëren van gegevens** op de introductiepagina van uw data factory genoemd.

![Wizard kopiëren](./media/data-factory-copy-wizard/copy-data-wizard.png)


## <a name="an-intuitive-wizard-for-copying-data"></a>Een intuïtieve wizard voor het kopiëren van gegevens
Met deze wizard kunt u eenvoudig verplaatsen gegevens uit verschillende bronnen naar bestemmingen in minuten. Na het doorlopen van de wizard, wordt een pijpleiding aan een activiteit kopie automatisch gemaakt voor u afhankelijke Data Factory entiteiten (gekoppelde services en datasets). Geen extra stappen vereist voor het maken van de pijpleiding.   

![Gegevensbron selecteren](./media/data-factory-copy-wizard/select-data-source-page.png)

> [AZURE.NOTE] Zie [Wizard kopiëren zelfstudie](data-factory-copy-data-wizard-tutorial.md) artikel voor stapsgewijze instructies voor het maken van een pijplijn monster te kopiëren naar een Azure SQL-databasetabel blob-gegevens vanuit een Azure. 

De wizard is ontworpen met grote gegevens in gedachten vanaf het begin. Het is eenvoudig en efficiënt te ontwerpen Data Factory pijpleidingen die honderden mappen, bestanden of tabellen met de wizard gegevens kopiëren, verplaatsen. De wizard ondersteunt de volgende drie functies: voorbeeld van automatische gegevens, schema vastleggen en toewijzing en filteren van gegevens. 

## <a name="automatic-data-preview"></a>Voorbeeld van automatische gegevens 
De wizard kopiëren kunt u bekijken deel van de gegevens uit de geselecteerde gegevensbron die u wilt controleren of de gegevens is de juiste gegevens die u wilt kopiëren. Als de gegevens in een tekstbestand, parseert de wizard copy bovendien het tekstbestand automatisch meer informatie over rij- en kolom scheidingstekens en schema. 

![Instellingen voor notatie van bestand](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Schema vastleggen en toewijzen 
Het schema van de ingevoerde gegevens mogelijk niet overeen met het schema van de van uitvoergegevens in sommige gevallen. In dit scenario moet u kolommen uit het doelschema kolommen uit het schema van de gegevensbron koppelen. 

Kolommen in het schema van de gegevensbron kopiëren, de wizard automatisch toegewezen aan de kolommen in het doelschema. U kunt de toewijzingen overschrijven met behulp van de vervolgkeuzelijsten (of) opgeven of een kolom worden overgeslagen moet tijdens het kopiëren van de gegevens.   

![Schematoewijzing](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Filteren van gegevens  
De wizard kunt u voor het filteren van gegevens om alleen de gegevens selecteren die moet worden gekopieerd naar de bestemming/sink-gegevensopslag. Filteren, vermindert het volume van de gegevens moeten worden gekopieerd naar de gegevensopslag sink en daarom verhoogt de doorvoer van de kopieerbewerking. Biedt een flexibele manier om gegevens te filteren in een relationele database via SQL query language (of) bestanden in een map Azure blob met behulp van [Data Factory-functies en variabelen](data-factory-functions-variables.md).   

### <a name="filtering-of-data-in-a-database"></a>Het filteren van gegevens in een database  
In het voorbeeld wordt de SQL-query maakt gebruik van de `Text.Format` functie en `WindowStart` variabele. 

![Valideren van expressies](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Het filteren van gegevens in een map Azure blob
U kunt variabelen in het pad naar de map gegevens kopiëren vanuit een map die wordt bepaald in runtime, op basis van de [Systeemvariabelen](data-factory-functions-variables.md#data-factory-system-variables). De ondersteunde variabelen zijn: **{jaar}**, **{maand}** **{dag}**, **{uur}**, **{minuut}**en **{aangepaste}**. Voorbeeld: inputfolder / {jaar} / {maand} / {dag}.

Stel dat u mappen in de volgende notatie hebt ingevoerd:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

Klik op de knop **Bladeren** voor een **bestand of map**, bladert u naar een van deze mappen (bijvoorbeeld in 2016 03 -> -> 01-02 >), en klikt u op **kiezen**. U ziet nu `2016/03/01/02` in het tekstvak. Nu vervangen door **2016** **{jaar}**, **03** met **{maand}**, **01** met **{dag}**en **02** met **{uur}**en druk op Tab. Vervolgkeuzelijsten te selecteren van de indeling van deze vier variabelen worden weergegeven:

![Met behulp van variabelen](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

Zoals in de volgende schermafdruk, ook kunt u een **aangepaste** variabele en alle [ondersteunde tekenreeksen opmaken](https://msdn.microsoft.com/library/8kb3ddd4.aspx). Selecteer een map die structuur, eerst de knop **Bladeren** gebruiken. Vervolgens vervangen door een waarde **{aangepaste}**en druk op Tab om te zien van het tekstvak u de notatie-expressie typt.     

![Met behulp van aangepaste variabele](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)


## <a name="support-for-diverse-data-and-object-types"></a>Ondersteuning voor diverse gegevens en de objecttypen
U kunt honderden mappen, bestanden of tabellen efficiënt verplaatsen met behulp van de Wizard kopiëren.

![Selecteer de tabellen waaruit u gegevens kopiëren](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

## <a name="scheduling-options"></a>Planningsopties
U kunt de kopieerbewerking uitvoeren eenmaal of volgens een schema (per uur, dagelijks, enzovoort). Met beide opties kunnen worden gebruikt voor de breedte van de verbindingslijnen voor gebouwen, cloud-en lokale bureaublad kopiëren.

Een eenmalige kopieerbewerking kan slechts één keer verplaatsen van gegevens van een bron naar een bestemming. Dit geldt voor gegevens van elk formaat en elke ondersteunde indeling. De geplande kopie kunt u kopiëren van gegevens op een herhaling van de voorgeschreven. Uitgebreide instellingen (zoals het opnieuw proberen, time-out en waarschuwingen) kunt u de geplande exemplaar configureren.

![Eigenschappen van de planning](./media/data-factory-copy-wizard/scheduling-properties.png)


## <a name="next-steps"></a>Volgende stappen
Zie voor een snel overzicht van het gebruik van de Wizard fabriek kopie maken van een pijplijn met kopie activiteit [Zelfstudie: maken van een pijplijn met behulp van de Wizard Copy](data-factory-copy-data-wizard-tutorial.md).
