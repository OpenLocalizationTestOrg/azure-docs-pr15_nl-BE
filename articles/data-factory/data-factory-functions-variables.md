<properties 
    pageTitle="Data Factory-functies en variabelen | Microsoft Azure" 
    description="Een lijst met Azure Data Factory-functies en variabelen" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"
    services="data-factory"
/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---functions-and-system-variables"></a>Azure Data Factory - functies en variabelen
Dit artikel bevat informatie over functies en variabelen die worden ondersteund door Azure Data Factory.
  
## <a name="data-factory-system-variables"></a>Data Factory systeemvariabelen

Naam van variabele | Beschrijving | Bereik van een object | JSON Scope en Use-Cases
------------- | ----------- | ------------ | ------------------------
WindowStart | Begin van het tijdsinterval voor de huidige activiteit uitvoeren venster | activiteit | <ol><li>Gegevensquery's selectie opgeven. Zie connector artikelen waarnaar wordt verwezen in het artikel [Gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) .</li><li>Parameters doorgeven aan het script component (voorbeeld boven weergegeven).</li>
WindowEnd | Einde van een tijdsinterval voor de huidige activiteit uitvoeren venster | activiteit | zoals boven
SliceStart | Begin van het tijdsinterval voor het segment wordt geproduceerd | activiteit<br/>DataSet | <ol><li>Dynamische paden en bestandsnamen opgeven tijdens het werken met [Azure Blob](data-factory-azure-blob-connector.md) en [bestandssysteem datasets](data-factory-onprem-file-system-connector.md).</li><li>Data factory functies in activiteit ingangen collectie invoer afhankelijkheden opgeven.</li></ol>
SliceEnd | Einde van een tijdsinterval voor het huidige segment wordt geproduceerd | activiteit<br/>DataSet | hetzelfde als hierboven. 

> [AZURE.NOTE] Data factory is momenteel vereist dat het schema dat is opgegeven in de activiteit precies overeenkomen met het schema dat in de beschikbaarheid van de dataset uitvoer. Dit betekent dat WindowStart, WindowEnd en SliceStart en SliceEnd worden altijd toegewezen aan dezelfde periode en een segment met één output.
 
## <a name="data-factory-functions"></a>Data Factory-functies 

U kunt functies gebruiken in data factory samen met hierboven vermelde variabelen voor de volgende doeleinden:

1.  Gegevensquery's selectie opgeven (Zie connector artikelen waarnaar wordt verwezen door het artikel [Gegevensverplaatsing activiteiten](data-factory-data-movement-activities.md) .

    De syntaxis voor het aanroepen van een functie van de fabriek is: ** $$ ** voor selectie gegevensquery's en andere eigenschappen in activiteit en datasets.  
2. Collectie invoer afhankelijkheden opgeven met data factory-functies van de activiteit-ingangen (Zie voorbeeld hierboven).

    $$ is niet nodig voor het opgeven van de afhankelijkheid van ingevoerde expressies.   

In het volgende voorbeeld wordt is de eigenschap **sqlReaderQuery** in een JSON-bestand toegewezen aan een waarde die wordt geretourneerd door de functie **Text.Format** . In dit voorbeeld gebruikt ook een systeemvariabele met de naam **WindowStart**, die de begintijd van de activiteit uitvoeren venster vertegenwoordigt.
    
    {
        "Type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
    }

### <a name="functions"></a>Functies

De volgende tabellen staan de functies in Azure Data Factory:

Categorie | Functie | Parameters | Beschrijving
-------- | -------- | ---------- | ----------- 
Tijd | AddHours(X,Y) | X: DateTime <br/><br/>Y: int | Y uren toevoegt aan de gegeven tijd X. <br/><br/>Voorbeeld: 5/9/2013 12:00:00 uur + 2 uur = 9/5/2013 2:00:00 uur
Tijd | AddMinutes(X,Y) | X: DateTime <br/><br/>Y: int | X van Y minuten toevoegen<br/><br/>Voorbeeld: 15/9/2013 12:00:00 uur + 15 minuten = 9/15/2013 12:15:00 uur
Tijd | StartOfHour(X) | X: Datetime | Deze eigenschap haalt de begintijd voor het uur wordt vertegenwoordigd door het uur onderdeel van X. <br/><br/>Voorbeeld: StartOfHour 15-9/2013 05:10:23 PM is 15-9/2013 05:00:00 uur
Datum | AddDays(X,Y) | X: DateTime<br/><br/>Y: int | Y dagen aan X toegevoegd.<br/><br/>Voorbeeld: 15/9/2013 12:00:00 uur + 2 dagen = 9/17/2013 12:00:00 uur
Datum | AddMonths(X,Y) | X: DateTime<br/><br/>Y: int | X van Y maanden toevoegen<br/><br/>Voorbeeld: 15/9/2013 12:00:00 uur + 1 maand = 10/15/2013 12:00:00 uur 
Datum | AddQuarters(X,Y) | X: DateTime <br/><br/>Y: int | Y toegevoegd * 3 maanden tot en met X.<br/><br/>Voorbeeld: 15/9/2013 12:00:00 uur + 1 kwartaal = 12/15/2013 12:00:00 uur
Datum | AddWeeks(X,Y) | X: DateTime<br/><br/>Y: int | Y toegevoegd * X 7 dagen<br/><br/>Voorbeeld: 15/9/2013 12:00:00 uur + 1 week = 9/22/2013 12:00:00 uur
Datum | AddYears(X,Y) | X: DateTime<br/><br/>Y: int | Jaar Y aan X toegevoegd.<br/><br/>Voorbeeld: 15/9/2013 12:00:00 uur + 1 jaar = 9/15/2014 12:00:00 uur
Datum | Day(X) | X: DateTime | Deze eigenschap haalt de dag onderdeel van X.<br/><br/>Voorbeeld: Dag 15-9/2013 12:00:00 uur is 9. 
Datum | DayOfWeek(X) | X: DateTime | Met deze eigenschap wordt de dag van de week onderdeel van X.<br/><br/>Voorbeeld: Dag van de week van 15-9/2013 12:00:00 uur zondag is.
Datum | DayOfYear(X) | X: DateTime | Met deze eigenschap wordt de dag van het jaar, uitgedrukt in het jaar onderdeel van X.<br/><br/>Voorbeelden:<br/>12/1/2015: dag 335 van 2015<br/>31/12/2015: 365 dagen van 2015<br/>31-12/2016: 366 dagen van 2016 (schrikkeljaar)
Datum | DaysInMonth(X) | X: DateTime | Deze eigenschap haalt de dagen van de maand wordt vertegenwoordigd door het onderdeel van de maand van de parameter X.<br/><br/>Voorbeeld: DagenInMaand van 15-9/2013 zijn 30, aangezien er 30 dagen in de maand September.
Datum | EndOfDay(X) | X: DateTime | Deze eigenschap haalt de datum-tijd die het einde van de dag (dagcomponent) van X.<br/><br/>Voorbeeld: EndOfDay 15-9/2013 05:10:23 PM is 15-9/2013 11:59:59 PM.
Datum | EndOfMonth(X) | X: DateTime | Deze eigenschap haalt het einde van de maand wordt vertegenwoordigd door een onderdeel van de maand van de parameter X. <br/><br/>Voorbeeld: EndOfMonth 15-9/2013 05:10:23 PM is 30-9/2013 11:59:59 PM (datum tijd die het einde van de maand September)
Datum | StartOfDay(X) | X: DateTime | Met deze eigenschap wordt het begin van de dag die wordt vertegenwoordigd door het onderdeel van de dag van de parameter X.<br/><br/>Voorbeeld: StartOfDay 15-9/2013 05:10:23 PM is 15-9/2013 00:00:00 uur.
Datum/tijd | FROM(X) | X: string | Parseren X tekenreeks naar een datum-tijd.
Datum/tijd | Ticks(X) | X: DateTime | Deze eigenschap haalt de maten eigenschap van de parameter X. Eén tik is gelijk aan 100 nanoseconden. De waarde van deze eigenschap geeft het aantal tikken weergegeven dat is verstreken sinds middernacht 12:00:00, 1 januari 0001. 
Tekst | Format(X) | X: variabele | De tekst wordt opgemaakt.

#### <a name="textformat-example"></a>Voorbeeld van de Text.Format

    "defines": { 
        "Year" : "$$Text.Format('{0:yyyy}',WindowStart)",
        "Month" : "$$Text.Format('{0:MM}',WindowStart)",
        "Day" : "$$Text.Format('{0:dd}',WindowStart)",
        "Hour" : "$$Text.Format('{0:hh}',WindowStart)"
    }

Zie [aangepaste datum en tijd notatiereeksen](https://msdn.microsoft.com/library/8kb3ddd4.aspx) onderwerp dat verschillende opmaakopties beschrijft die u kunt gebruiken (bijvoorbeeld: jj versus jjjj). 

> [AZURE.NOTE] Wanneer u een functie binnen een andere functie, hoeft u niet te gebruiken **$$** voorvoegsel voor de interne functie. Bijvoorbeeld: $$Text.Format ("PartitionKey eq \\' my_pkey_filter_value\\" en ge RowKey \\' {0:yyyy-MM-dd: mm: SS}\\'', Time.AddHours (SliceStart -6)). In dit voorbeeld ziet u dat **$$** voorvoegsel wordt niet gebruikt voor de functie **Time.AddHours** . 
  

