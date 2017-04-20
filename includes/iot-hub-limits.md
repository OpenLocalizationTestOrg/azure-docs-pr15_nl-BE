De volgende tabel worden de grenzen die zijn gekoppeld aan de verschillende niveaus (S1, S2, S3, F1). Zie voor informatie over de kosten per *eenheid* in elke laag [IoT Hub prijzen](https://azure.microsoft.com/pricing/details/iot-hub/).

| Resource | S1-standaard | S2 norm | S3-standaard | Gratis F1 |
| -------- | ----------- | ----------- | ----------- | ------- |
| Berichten per dag | 400.000 | 6,000,000   | 300,000,000 | 8.000   |
| Het maximum aantal eenheden | 200    | 200         | 200         | 1       |

> [AZURE.NOTE] Als u met meer dan 200 eenheden met een laag S1 of S2 of S3 hub verwacht, neem contact op met Microsoft support.

De volgende tabel worden de limieten die voor IoT Hub bronnen gelden:

| Resource | Limiet |
| -------- | ----- |
| Maximale IoT hubs per Azure abonnement betaald | 10 |
| Maximale vrije IoT hubs per abonnement Azure | 1 |
| Maximum aantal apparaat-id 's<br/>  in één aanroep is geretourneerd | 1000 |
| IoT Hub bericht Maximale inhouding voor apparaat-wolk-berichten | 7 dagen |
| Maximumgrootte van bericht apparaat naar cloud | 256 KB |
| Maximale grootte van het apparaat naar cloud-batch | 256 KB |
| Maximaal aantal berichten in batch apparaat naar cloud | 500 |
| Maximale grootte van het bericht van de wolk naar apparaat | 64 KB |
| Maximale TTL voor cloud-to-device-berichten | 2 dagen |
| Levering van maximum aantal voor wolk naar apparaat <br/> berichten | 100 |
| Levering van maximum aantal voor Feedbackberichten <br/> in antwoord op een bericht van de wolk naar apparaat | 100 |
| Maximale TTL voor Feedbackberichten in <br/> antwoord op een bericht van de wolk naar apparaat | 2 dagen |

> [AZURE.NOTE] Als u meer dan 10 betaalde IoT hubs in een Azure-abonnement nodig hebt, neem contact op met Microsoft support.

De service IoT Hub bandbreedte verzoeken wanneer de volgende quota worden overschreden:

| Gashendel | Waarde per hub |
| -------- | ------------- |
| Identiteit registerbewerkingen <br/> (maken, ophalen, lijst, bijwerken en verwijderen), <br/> individuele of bulk importeren/exporteren | 5000/min/eenheid (S3) <br/> 100/min/eenheid (S1 en S2). |
| Apparaatverbindingen | sec-6000/eenheid (voor S3), 120/seconde/eenheid (S2), 12/seconde/eenheid (S1). <br/>Minimaal 100 per seconde. |
| Hiermee verzendt u apparaat naar cloud | sec-6000/eenheid (voor S3), 120/seconde/eenheid (S2), 12/seconde/eenheid (S1). <br/>Minimaal 100 per seconde. |
| Wolk naar apparaat verzenden | min-5000/eenheid (voor S3), 100/min/eenheid (S1 en S2). |
| Wolk naar apparaat ontvangt | min-50000/eenheid (voor S3), 1000/min/eenheid (S1 en S2). |
| Uploaden van bestanden | 5000 bestand uploaden min-meldingen/eenheid (voor S3), 100 bestand uploaden meldingen/min/eenheid (voor S1 en S2). <br/> 10000 SAS-URI's kunnen zijn van een account Azure opslag in één keer.<br/> 10 SAS URI's / apparaat kan worden uitgevoerd in één keer. |
