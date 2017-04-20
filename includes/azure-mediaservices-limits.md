Resource|Standaardlimiet|Maximum aantal
---|---|---
Azure Media Services (AMS) rekeningen in een enkel abonnement||25
Activa per account AMS||1.000.000<sup>1</sup>
Gekoppelde taken per taak||30
Activa per taak||50
Activa per project||100
Projecten per account AMS ||50.000<sup>2</sup>
Unieke locators die is gekoppeld aan een activum in één keer||5<sup>4</sup>
Live kanalen per account AMS </p></td>|5</p></td>|N.V.T.<sup>1</sup>
Programma's in ongewijzigde staat per kanaal </p></td>|50</p></td>|N.V.T.<sup>1</sup>
Programma's in rijklare toestand per kanaal </p></td>|3</p></td>|3
Streaming eindpunten in rijklare toestand per account AMS</p></td>|2</p></td>|N.V.T.<sup>1</sup>
Eenheden per eindpunt streaming streaming </p></td>|10 </p></td>|N.V.T.<sup>1</sup>
Codering eenheden per account AMS </p></td>|25</p></td>|N.V.T.<sup>1</sup>
Opslag-accounts | |1000<sup>5</sup>
Beleid || 1.000.000<sup>6</sup>

<sup>1</sup> u kunt aanvragen om de limieten voor deze quota bijwerken door een support ticket openen. Maak niet meer AMS accounts te verruimen, in plaats daarvan een support ticket indienen.

Dit nummer bevat <sup>2</sup> in de wachtrij, voltooid, actieve en geannuleerde taken. Verwijderde taken niet opgenomen. U kunt de oude taken met behulp van **IJob.Delete** of de HTTP-aanvraag **verwijderen** verwijderen.

<sup>3</sup> als u een aanvraag in de lijst taak entiteiten, maximaal 1000 wordt geretourneerd per aanvraag. Als u nodig hebt voor het bijhouden van alle ingediende projecten, kunt u boven/overslaan gebruiken zoals beschreven in de [query-opties van OData-systeem](http://msdn.microsoft.com/library/gg309461.aspx).

<sup>4</sup> locators zijn niet ontworpen voor het beheren van toegangsbeheer op gebruikersniveau. Gebruiken om verschillende toegangsrechten voor afzonderlijke gebruikers, oplossingen voor Digital Rights Management (DRM).

<sup>5</sup> de accounts opslag moet zijn van het abonnement met dezelfde Azure.

<sup>6</sup> is er een limiet van 1.000.000 beleid voor verschillende AMS-beleid (bijvoorbeeld voor Locator beleid of ContentKeyAuthorizationPolicy). Gebruik hetzelfde beleid-ID als u altijd met behulp van dezelfde dagen / toegangsmachtigingen / enz.
