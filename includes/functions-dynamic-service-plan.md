## <a name="dynamic-service-plan"></a>Dynamische serviceplan

In de dynamische Service plannen, wordt uw functie apps toegewezen aan een functie app-exemplaar. Indien nodig meer exemplaren zal dynamisch worden toegevoegd.
Deze instanties kunnen meerdere IT-bronnen, waardoor optimaal gebruik van de beschikbare infrastructuur Azure overspannen. Bovendien wordt uw functies uitgevoerd parallel te minimaliseren van de totale tijd die nodig is om aanvragen te verwerken. Uitvoeringstijd voor elke functie is toegevoegd, in seconden, en door de bovenliggende functie app samengevoegd. Met kosten aangedreven door het aantal instanties, de grootte van het systeemgeheugen en de totale uitvoeringstijd gemeten in Gigabyte seconden. Dit is een uitstekende optie als uw behoeften compute periodieke of uw project tijden zijn meestal erg kort omdat u betaalt alleen voor compute bronnen wanneer zij daadwerkelijk gebruikt worden.   

### <a name="memory-tier"></a>Geheugen-laag

Afhankelijk van uw functie nodig heeft kunt u de hoeveelheid geheugen die nodig is om ze uit te voeren in de functie App (container van functies).
De grootte van geheugenopties variëren van **128 MB tot 1536 MB**. Grootte van het geselecteerde geheugen correspondeert met de werkset die nodig zijn voor de functies die deel van uw functie app uitmaken. Als uw code meer geheugen dan de geselecteerde grootte vereist, wordt de functie app-exemplaar afgesloten vanwege onvoldoende geheugen beschikbaar is.

### <a name="scaling"></a>Schalen

Het verkeer moet, op basis van de geconfigureerde triggers bepalen wanneer u omhoog of omlaag te schalen, de functies van Azure platform worden geëvalueerd. De granulatie van de schaal is de functie app. Verticaal schalen betekent in dit geval meer exemplaren van een app functie toe te voegen. Omgekeerd als het verkeer afneemt, zijn functie app-exemplaren uitgeschakeld-uiteindelijk het verkleinen van nul wanneer zijn uitgeschakeld.  

### <a name="resource-consumption-and-billing"></a>Verbruik en facturering

In de dynamische modus brontoewijzing is anders dan de standaard App serviceplan gedaan, dus het verbruik model verschilt ook, waardoor een 'pay-per-use'-model. Verbruik wordt gerapporteerd per functie app, alleen voor het moment waarop code wordt uitgevoerd.  
Deze wordt berekend door vermenigvuldiging van de grootte van het geheugen (in GB) door de totale hoeveelheid tijd (in seconden) voor alle functies die worden uitgevoerd binnen de toepassing van deze functie. De eenheid van het verbruik is **GB-s (Gigabyte seconden)**.