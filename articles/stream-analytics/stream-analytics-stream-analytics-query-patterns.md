<properties
    pageTitle="Voorbeelden van veelvoorkomende gebruikspatronen in Analytics Stream query | Microsoft Azure"
    description="Algemene Azure Stream Analytics Query patronen "
    keywords="Voorbeelden van query"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="query-examples-for-common-stream-analytics-usage-patterns"></a>Voorbeelden van veelgebruikte Stream Analytics gebruikspatronen opvragen

## <a name="introduction"></a>Inleiding

Query's in Azure Stream Analytics worden uitgedrukt in een SQL-achtige querytaal die wordt beschreven in de [Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) guide.  In dit artikel worden oplossingen voor verschillende algemene query patronen die zijn gebaseerd op praktijksituaties.  Het is een werk in uitvoering en worden bijgewerkt met nieuwe patronen voortdurend blijft.

## <a name="query-example-data-type-conversions"></a>Voorbeeld van de query: gegevenstypeconversies
**Beschrijving**: de typen eigenschappen definiëren voor de invoerstroom.
Bijvoorbeeld, gewicht van de auto is afkomstig uit de invoerstroom als tekenreeksen en moet worden omgezet naar INT uitvoeren het totaliseren.

**Invoer**:

| Maken | Tijd | Gewicht |
| --- | --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z | "1000" |
| Honda | 2015-01-01T00:00:02.0000000Z | "2000" |

**Uitvoer**:

| Maken | Gewicht |
| --- | --- |
| Honda | 3000 |

**Oplossing**:

    SELECT
        Make,
        SUM(CAST(Weight AS BIGINT)) AS Weight
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Uitleg**: een CAST-instructie gebruiken in het veld gewicht geeft het type (Zie de lijst met ondersteunde gegevenstypen [hier](https://msdn.microsoft.com/library/azure/dn835065.aspx)).


## <a name="query-example-using-likenot-like-to-do-pattern-matching"></a>Voorbeeld van de query: met behulp van dergelijke/niet graag patroon overeenkomen
**Beschrijving**: Controleer de waarde van een veld op de gebeurtenis komt overeen met een bepaald patroon, bijvoorbeeld return nummerplaten die beginnen met A en eindigt met 9

**Invoer**:

| Maken | LicensePlate | Tijd |
| --- | --- | --- |
| Honda | ABC-123 | 2015-01-01T00:00:01.0000000Z |
| Toyota | AAA 999 | 2015-01-01T00:00:02.0000000Z |
| Nissan | ABC-369 | 2015-01-01T00:00:03.0000000Z |

**Uitvoer**:

| Maken | LicensePlate | Tijd |
| --- | --- | --- |
| Toyota | AAA 999 | 2015-01-01T00:00:02.0000000Z |
| Nissan | ABC-369 | 2015-01-01T00:00:03.0000000Z |

**Oplossing**:

    SELECT
        *
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LicensePlate LIKE 'A%9'

**Toelichting**: de LIKE-instructie gebruiken om te controleren dat de waarde van het LicensePlate veld begint met A en vervolgens heeft een willekeurige tekenreeks van nul of meer tekens en met 9 eindigt. 

## <a name="query-example-specify-logic-for-different-casesvalues-case-statements"></a>Voorbeeld van de query: Geef logica voor verschillende gevallen/waarden (CASE-instructies)
**Beschrijving**: bieden verschillende berekening voor een veld op basis van bepaalde criteria.
Zo bieden een beschrijving voor het aantal auto's van het hetzelfde merk met een speciaal geval voor 1 doorgegeven.

**Invoer**:

| Maken | Tijd |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Uitvoer**:

| CarsPassed | Tijd |
| --- | --- | --- |
| 1 Honda | 2015-01-01T00:00:10.0000000Z |
| 2 Toyotas | 2015-01-01T00:00:10.0000000Z |

**Oplossing**:

    SELECT
        CASE
            WHEN COUNT(*) = 1 THEN CONCAT('1 ', Make)
            ELSE CONCAT(CAST(COUNT(*) AS NVARCHAR(MAX)), ' ', Make, 's')
        END AS CarsPassed,
        System.TimeStamp AS Time
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Toelichting**: de CASE-component kan wij een andere berekening op basis van bepaalde criteria (in ons geval het aantal auto's in het venster statistische).

## <a name="query-example-send-data-to-multiple-outputs"></a>Voorbeeld van de query: gegevens verzenden naar meerdere uitgangen
**Beschrijving**: gegevens verzenden naar meerdere doelen voor uitvoer vanuit één taak.
Bijvoorbeeld gegevens van een waarschuwing op basis van een drempelwaarde analyseren en archiveren van alle gebeurtenissen naar een blobopslag

**Invoer**:

| Maken | Tijd |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Honda | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Output1**:

| Maken | Tijd |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Honda | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Output2**:

| Maken | Tijd | Aantal |
| --- | --- | --- |
| Toyota | 2015-01-01T00:00:10.0000000Z | 3 |

**Oplossing**:

    SELECT
        *
    INTO
        ArchiveOutput
    FROM
        Input TIMESTAMP BY Time

    SELECT
        Make,
        System.TimeStamp AS Time,
        COUNT(*) AS [Count]
    INTO
        AlertOutput
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
    HAVING
        [Count] >= 3

**Uitleg**: de in-component Stream Analytics geeft aan welke van de resultaten die de gegevens van deze instructie te schrijven.
De eerste query is een Pass Through-query van de gegevens die we met een vermogen ontvangen dat we de naam ArchiveOutput.
De tweede query komt enkele eenvoudige aggregatie en filteren, en stuurt de resultaten naar een downstream waarschuwingsmethoden systeem.
*Opmerking*: U kunt ook resultaten van CTE's (dat wil zeggen met overzichten) hergebruiken in meerdere instructies voor uitvoer – dit heeft het voordeel van het openen van de invoerbron minder lezers.
Bijvoorbeeld: 

    WITH AllRedCars AS (
        SELECT
            *
        FROM
            Input TIMESTAMP BY Time
        WHERE
            Color = 'red'
    )
    SELECT * INTO HondaOutput FROM AllRedCars WHERE Make = 'Honda'
    SELECT * INTO ToyotaOutput FROM AllRedCars WHERE Make = 'Toyota'

## <a name="query-example-counting-unique-values"></a>Voorbeeld van de query: unieke waarden tellen
**Beschrijving**: het aantal unieke waarden die worden weergegeven in de stroom in een venster.
Bijvoorbeeld hoeveel unieke merk van auto's doorgegeven via de gratis stand in een 2 tweede venster?

**Invoer**:

| Maken | Tijd |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Honda | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |
| Toyota | 2015-01-01T00:00:03.0000000Z |

**Uitvoer:**

| Aantal | Tijd |
| --- | --- |
| 2 | 2015-01-01T00:00:02.000Z |
| 1 | 2015-01-01T00:00:04.000Z |

**Oplossing:**

    WITH Makes AS (
        SELECT
            Make,
            COUNT(*) AS CountMake
        FROM
            Input TIMESTAMP BY Time
        GROUP BY
              Make,
              TumblingWindow(second, 2)
    )
    SELECT
        COUNT(*) AS Count,
        System.TimeStamp AS Time
    FROM
        Makes
    GROUP BY
        TumblingWindow(second, 1)


**Uitleg:** Doen we een eerste samenvoeging als u unieke maakt met hun aantal in het venster.
Dan doen we een samenvoeging van hoeveel maakt dat we alle unieke waarden in een venster het hetzelfde tijdstempel niet verkrijgen en vervolgens het tweede venster van de globalisatie moet minimaal 2 windows niet samenvoegen uit de eerste stap hebt – gegeven.

## <a name="query-example-determine-if-a-value-has-changed"></a>Voorbeeld van de query: bepalen of een waarde is gewijzigd#
**Beschrijving**: Bekijk een vorige waarde om te bepalen of andere dan de huidige waarde is bijvoorbeeld de vorige auto op de weg niet gratis is hetzelfde als de huidige auto maken?

**Invoer**:

| Maken | Tijd |
| --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z |
| Toyota | 2015-01-01T00:00:02.0000000Z |

**Uitvoer**:

| Maken | Tijd |
| --- | --- |
| Toyota | 2015-01-01T00:00:02.0000000Z |

**Oplossing**:

    SELECT
        Make,
        Time
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(minute, 1)) <> Make

**Uitleg**: vertraging om te kijken naar de invoer weer één gebeurtenis streamen en haal het merk. Vervolgens vergelijken met de zorg voor de huidige gebeurtenis en de gebeurtenis uitgevoerd als deze verschillen.

## <a name="query-example-find-first-event-in-a-window"></a>Voorbeeld van de query: eerste gebeurtenis in een venster zoeken
**Beschrijving**: eerste auto in elk interval van 10 minuten zoeken?

**Invoer**:

| LicensePlate | Maken | Tijd |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| RMV 8282 | Honda | 2015-07-27T00:05:01.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Uitvoer**:

| LicensePlate | Maken | Tijd |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |

**Oplossing**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) = 1

Nu gaan we wijzigen de eerste auto van bepaalde probleem te vinden in elk interval van 10 minuten.

| LicensePlate | Maken | Tijd |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Oplossing**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) OVER (PARTITION BY Make) = 1

## <a name="query-example-find-last-event-in-a-window"></a>Voorbeeld van de query: laatste gebeurtenis in een venster zoeken
**Beschrijving**: de laatste auto zoeken in elk interval van 10 minuten.

**Invoer**:

| LicensePlate | Maken | Tijd |
| --- | --- | --- |
| DXE 5291 | Honda | 2015-07-27T00:00:05.0000000Z |
| YZK 5704 | Ford | 2015-07-27T00:02:17.0000000Z |
| RMV 8282 | Honda | 2015-07-27T00:05:01.0000000Z |
| YHN 6970 | Toyota | 2015-07-27T00:06:00.0000000Z |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| QYF 9358 | Honda | 2015-07-27T00:12:02.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Uitvoer**:

| LicensePlate | Maken | Tijd |
| --- | --- | --- |
| VFE 1616 | Toyota | 2015-07-27T00:09:31.0000000Z |
| MDR 6128 | BMW | 2015-07-27T00:13:45.0000000Z |

**Oplossing**:

    WITH LastInWindow AS
    (
        SELECT 
            MAX(Time) AS LastEventTime
        FROM 
            Input TIMESTAMP BY Time
        GROUP BY 
            TumblingWindow(minute, 10)
    )
    SELECT 
        Input.LicensePlate,
        Input.Make,
        Input.Time
    FROM
        Input TIMESTAMP BY Time 
        INNER JOIN LastInWindow
        ON DATEDIFF(minute, Input, LastInWindow) BETWEEN 0 AND 10
        AND Input.Time = LastInWindow.LastEventTime

**Uitleg**: Er zijn twee stappen in de query: eerste vindt de meest recente tijdstempel in windows 10 minuten. De tweede stap wordt toegevoegd aan de resultaten van de eerste query in de oorspronkelijke stream laatste tijdstempels in elk venster overeenkomende gebeurtenissen vinden. 

## <a name="query-example-detect-the-absence-of-events"></a>Voorbeeld van de query: het ontbreken van gebeurtenissen detecteren
**Beschrijving**: controleren of een stream heeft geen waarde die overeenkomt met een bepaalde criteria.
Bijvoorbeeld 2 opeenvolgende auto's van hetzelfde merk hebt onderweg gratis binnen 90 seconden?

**Invoer**:

| Maken | LicensePlate | Tijd |
| --- | --- | --- |
| Honda | ABC-123 | 2015-01-01T00:00:01.0000000Z |
| Honda | AAA 999 | 2015-01-01T00:00:02.0000000Z |
| Toyota | DEF 987 | 2015-01-01T00:00:03.0000000Z |
| Honda | GHI-345 | 2015-01-01T00:00:04.0000000Z |

**Uitvoer**:

| Maken | Tijd | CurrentCarLicensePlate | FirstCarLicensePlate | FirstCarTime |
| --- | --- | --- | --- | --- |
| Honda | 2015-01-01T00:00:02.0000000Z | AAA 999 | ABC-123 | 2015-01-01T00:00:01.0000000Z |

**Oplossing**:

    SELECT
        Make,
        Time,
        LicensePlate AS CurrentCarLicensePlate,
        LAG(LicensePlate, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarLicensePlate,
        LAG(Time, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarTime
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(second, 90)) = Make

**Uitleg**: vertraging om te kijken naar de invoer weer één gebeurtenis streamen en haal het merk. Vervolgens vergelijken met de zorg voor de huidige gebeurtenis en de gebeurtenis uitgevoerd als ze hetzelfde zijn en VERTRAGINGSTIJD gebruikt om gegevens over de vorige auto.

## <a name="query-example-detect-duration-between-events"></a>Voorbeeld van de query: duur tussen gebeurtenissen detecteren
**Beschrijving**: de duur van een bepaalde gebeurtenis te vinden. Bijvoorbeeld, gegeven een web clickstream bepalen tijd besteed aan een functie.

**Invoer**:  
  
| Gebruiker | Functie | Gebeurtenis | Tijd |
| --- | --- | --- | --- |
| user@location.com | RightMenu | Start | 2015-01-01T00:00:01.0000000Z |
| user@location.com | RightMenu | Einde | 2015-01-01T00:00:08.0000000Z |
  
**Uitvoer**:  
  
| Gebruiker | Functie | Duur |
| --- | --- | --- |
| user@location.com | RightMenu | 7 |
  

**Oplossing**

````
    SELECT
        [user], feature, DATEDIFF(second, LAST(Time) OVER (PARTITION BY [user], feature LIMIT DURATION(hour, 1) WHEN Event = 'start'), Time) as duration
    FROM input TIMESTAMP BY Time
    WHERE
        Event = 'end'
````

**Uitleg**: laatste functie voor het ophalen van de laatste tijdwaarde wanneer het gebeurtenistype is de 'Start'. Opmerking de functie laatste partitie door [gebruiker] gebruikt om aan te geven dat per gebruiker uniek resultaat wordt berekend.  De query heeft een 1 uur maximale drempelwaarde voor het tijdsverschil tussen de 'Start' en 'Stop'-gebeurtenissen, maar kan worden geconfigureerd als nodig (maximale DURATION(hour, 1).

## <a name="query-example-detect-duration-of-a-condition"></a>Voorbeeld van de query: sporen van de duur van een voorwaarde
**Beschrijving**: zoek uit hoe lang een voorwaarde voor plaatsvond.
Stel dat een bug waardoor alle auto's met een onjuiste gewicht (meer dan 20.000 pond) – we willen voor het berekenen van de duur van de fout.

**Invoer**:

| Maken | Tijd | Gewicht |
| --- | --- | --- |
| Honda | 2015-01-01T00:00:01.0000000Z | 2000 |
| Toyota | 2015-01-01T00:00:02.0000000Z | 25000 |
| Honda | 2015-01-01T00:00:03.0000000Z | 26000 |
| Toyota | 2015-01-01T00:00:04.0000000Z | 25000 |
| Honda | 2015-01-01T00:00:05.0000000Z | 26000 |
| Toyota | 2015-01-01T00:00:06.0000000Z | 25000 |
| Honda | 2015-01-01T00:00:07.0000000Z | 26000 |
| Toyota | 2015-01-01T00:00:08.0000000Z | 2000 |

**Uitvoer**:

| StartFault | EndFault |
| --- | --- |
| 2015-01-01T00:00:02.000Z | 2015-01-01T00:00:07.000Z |

**Oplossing**:

````
    WITH SelectPreviousEvent AS
    (
    SELECT
    *,
        LAG([time]) OVER (LIMIT DURATION(hour, 24)) as previousTime,
        LAG([weight]) OVER (LIMIT DURATION(hour, 24)) as previousWeight
    FROM input TIMESTAMP BY [time]
    )

    SELECT 
        LAG(time) OVER (LIMIT DURATION(hour, 24) WHEN previousWeight < 20000 ) [StartFault],
        previousTime [EndFault]
    FROM SelectPreviousEvent
    WHERE
        [weight] < 20000
        AND previousWeight > 20000
````

**Uitleg**: gebruik VERTRAGINGSTIJD voor het weergeven van de invoerstroom gedurende 24 uur en zoek naar instanties waar StartFault en StopFault worden overbrugd door < 20000 gewicht.

## <a name="query-example-fill-missing-values"></a>Voorbeeld van de query: Vul ontbrekende waarden
**Omschrijving**: voor de stroom van gebeurtenissen met ontbrekende waarden produceren een reeks gebeurtenissen met regelmatige tussenpozen.
Genereren gebeurtenis bijvoorbeeld elke 5 seconden die het meest recent zichtbaar gegevenspunt wordt gemeld.

**Invoer**:

| t | waarde |
|--------------------------|-------|
| "2014-01-01T06:01:00" | 1 |
| "2014-01-01T06:01:05" | 2 |
| "2014-01-01T06:01:10" | 3 |
| "2014-01-01T06:01:15" | 4 |
| "2014-01-01T06:01:30" | 5 |
| "2014-01-01T06:01:35" | 6 |

De **output (eerste 10 rijen)**:

| windowend | lastevent.t | lastevent.Value |
|--------------------------|--------------------------|--------|
| 2014-01-01T14:01:00.000Z | 2014-01-01T14:01:00.000Z | 1 |
| 2014-01-01T14:01:05.000Z | 2014-01-01T14:01:05.000Z | 2 |
| 2014-01-01T14:01:10.000Z | 2014-01-01T14:01:10.000Z | 3 |
| 2014-01-01T14:01:15.000Z | 2014-01-01T14:01:15.000Z | 4 |
| 2014-01-01T14:01:20.000Z | 2014-01-01T14:01:15.000Z | 4 |
| 2014-01-01T14:01:25.000Z | 2014-01-01T14:01:15.000Z | 4 |
| 2014-01-01T14:01:30.000Z | 2014-01-01T14:01:30.000Z | 5 |
| 2014-01-01T14:01:35.000Z | 2014-01-01T14:01:35.000Z | 6 |
| 2014-01-01T14:01:40.000Z | 2014-01-01T14:01:35.000Z | 6 |
| 2014-01-01T14:01:45.000Z | 2014-01-01T14:01:35.000Z | 6 |

    
**Oplossing**:

    SELECT
        System.Timestamp AS windowEnd,
        TopOne() OVER (ORDER BY t DESC) AS lastEvent
    FROM
        input TIMESTAMP BY t
    GROUP BY HOPPINGWINDOW(second, 300, 5)


**Uitleg**: deze query genereert gebeurtenissen elke 5, tweede en de laatste gebeurtenis die is ontvangen voordat u gaat uitvoeren. [Venster Hopping] (https://msdn.microsoft.com/library/dn835041.aspx "Venster Hopping - Azure Stream Analytics") duur bepaalt hoe ver terug in de query eruit ziet als u wilt zoeken naar de meest recente gebeurtenis (300 seconden in dit voorbeeld).


## <a name="get-help"></a>Help-informatie opvragen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) voor verdere ondersteuning

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Analytics Azure Stream](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 
