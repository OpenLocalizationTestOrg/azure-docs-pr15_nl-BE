De volgende tabel worden beschreven van de grote quota, maxima, standaardwaarden en throttles in Azure planner.

|Resource|Beperking beschrijving|
|---|---|
|**Grootte**|De maximale grootte is 16 kB. Als een PUT of een PATCH in een taak groter is dan deze limiet resulteert, een 400 onjuiste aanvraag statuscode.|
|**Grootte URL aanvragen**|Maximale grootte van de aanvraag-URL is 2048 tekens.|
|**Grootte van koptekst statistische**|Grootte van de maximale totale header is 4096 tekens.|
|**Aantal kop**|Koptekst maximum aantal is 50-headers.|
|**Hoofdtekst**|Maximale grootte is 8192 tekens.|
|**Bereik van terugkeerpatroon**|Bereik van terugkeerpatroon maximum is 18 maanden.|
|**Tijd voor de begintijd**|"Tijd in de begintijd" maximaal is 18 maanden.|
|**Overzicht van functies**|Maximale reactie instantie opgeslagen in de geschiedenis is 2048 bytes.|
|**Frequentie**|Het standaardquotum van de maximale frequentie is 1 uur in een collectie gratis job en 1 minuut in een standaard taak-collectie. De maximale frequentie kan worden geconfigureerd voor een taak collectie lager is dan het maximum. Alle taken in de collectie van het project zijn beperkt de waarde is ingesteld op de collectie van de taak. Als u probeert een taak wilt maken met een hogere frequentie dan de maximale frequentie voor de taak collectie mislukt verzoek met een 409 statuscode van het Conflict.|
|**Taken**|Het standaardquotum max taken is 5 projecten in een verzameling gratis job en 50 taken in een project standard-collectie. Het maximum aantal taken kan worden geconfigureerd voor de collectie van een taak. Alle taken in de collectie van het project zijn beperkt de waarde is ingesteld op de collectie van de taak. Als u meer functies dan de maximale taken quota maken probeert, mislukt de aanvraag met een 409 statuscode van het Conflict.|
|**Geschiedenis vasthouden**|Geschiedenis van maximaal 2 maanden of de laatste 1000 uitvoeringen behouden.|
|**Vasthouden van voltooid en mislukt.**|Voltooide en mislukte taken worden bewaard gedurende 60 dagen.|
|**Time-out**|Er is een statische (niet geconfigureerd) verzoek time-out van 60 seconden voor HTTP-bewerkingen. Langer lopende operaties, gaat u als volgt de asynchrone protocollen HTTP; bijvoorbeeld een 202 onmiddellijk terug, maar blijven op de achtergrond werken.|
