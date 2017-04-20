<properties
    pageTitle="Bepaalde openbare gegevens in Azure gebeurtenis Hubs | Microsoft Azure"
    description="Overzicht van de gebeurtenis Hubs van monster web importeren"
    services="event-hubs"
    documentationCenter="na"
    authors="spyrossak"
    manager="timlt"
    editor=""/>

<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/25/2016"
    ms.author="spyros;sethm" />

# <a name="pulling-public-data-into-azure-event-hubs"></a>Bepaalde openbare gegevens in Azure gebeurtenis Hubs

In typische scenario's Internet dingen (IoT) hebt u de apparaten die u push-gegevens naar Azure, hetzij naar een gebeurtenis Azure Hub of een hub IoT programmeren kunt. Deze hubs zijn ingangspunten in Azure voor het opslaan, analyseren en visualiseren met een groot aantal hulpprogramma's beschikbaar worden gesteld op Microsoft Azure. Beide vereisen echter dat u push-gegevens naar deze opgemaakt als JSON en op bepaalde manieren beveiligd. Verschijnt de volgende vraag. Wat moet u doen als u brengen in de gegevens uit openbare of particuliere bronnen waar de gegevens als een webservice of diervoeder zonnestelsels wordt weergegeven wilt, maar u hebt niet de mogelijkheid om te wijzigen hoe de gegevens gepubliceerd? Houd rekening met het weer, of verkeer of aandelenkoersen - weet u niet NOAA, WSDOT en/of NASDAQ voor het configureren van een push naar uw Hub-gebeurtenis. U lost dit probleem, hebben we geschreven en open-die afkomstig zijn, een kleine wolk steekproef die u kunt aanpassen en implementeren die de gegevens ophalen uit een dergelijke bron en druk op uw Hub-gebeurtenis. Daar kunt u doen wat u wilt, onderworpen aan de licentievoorwaarden van de producent, natuurlijk. U kunt de toepassing vinden [hier](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/).

Houd er rekening mee dat de code in dit voorbeeld alleen ziet u hoe u gegevens ophalen uit gewone web feeds en het schrijven van een gebeurtenis Azure Hub. Dit is niet bedoeld als een productietoepassing, en geen is geprobeerd zodat het geschikt is voor gebruik in een dergelijke omgeving - is strictfly een DIY, alleen voorbeeld ontwikkelaars zijn gericht. Bovendien is het bestaan van dit voorbeeld niet de Commissie een aanbeveling moet u gegevens **pull** in Azure in plaats van **push** . U moet controleren beveiliging, prestaties, functionaliteit en de factoren voordat op een end-to-end-architectuur.

## <a name="application-structure"></a>Toepassingsstructuur

De toepassing is geschreven in C# en het [voorbeeld van een beschrijving](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/) bevat alle informatie die u wilt wijzigen, samenstellen en publiceren van de toepassing. De volgende secties vindt u een overzicht van de werking van de toepassing.

We beginnen met de veronderstelling dat u toegang tot een data hebt-kanaal. U wilt ophalen van de verkeersgegevens van de Washington State Department of Transportation of de weergegevens uit NOAA, om aangepaste rapporten weer te geven of te combineren met andere gegevens in uw toepassing gegevens. U moet ook een gebeurtenis Azure Hub hebt ingesteld en weet dat de verbinding nodig voor toegang tot deze tekenreeks.

Wanneer u de oplossing GenericWebToEH start, wordt het een configuratiebestand (App.config) om een aantal dingen:

1. De URL of een lijst met URL's voor het publiceren van de site. In het ideale geval, dit is een site die gegevens in JSON, zoals waarnaar wordt verwezen door WSDOT publiceert [hier](http://www.wsdot.wa.gov/Traffic/api/). 
2. Referenties voor de URL, indien nodig. Vele openbare bronnen hoeft geen referenties of kunt u de referenties in de URL-reeks plaatsen. Andere moet u afzonderlijk opgeven. (Let erop dat u slechts één reeks referenties in deze toepassing, opgeven kunt zodat het werkt alleen als u slechts één URL, niet een lijst met URL's opgeven.)
3. De verbindingsreeks en de naam van de gebeurtenis Hub in die naamruimte gebeurtenis Hubs, waarmee u de gegevens wordt push. U kunt deze informatie vinden in de portal Azure.
4. Een slaap-interval, in milliseconden, voor het interval tussen de site openbare gegevens polling. Dit is voorzichtig. Als u af en toe te controleren, mist u misschien gegevens; anderzijds, als u regelmatig te controleren, krijgt u een groot aantal herhaalde gegevens, of erger nog, u kan worden geblokkeerd als een slechte bot. Overwegen hoe vaak de gegevensbron wordt bijgewerkt - gegevens weer of verkeer kan worden vernieuwd elke 15 minuten, maar misschien om de paar seconden, afhankelijk van waar u krijgen van aandelenkoersen. 
5. Een vlag aan de toepassing of de gegevens afkomstig zijn JSON of XML. Aangezien u nodig hebt om de gegevens op een Hub gebeurtenis, heeft de toepassing een module XML omzetten in JSON alvorens te verzenden.

Na het lezen van het configuratiebestand, gaat de toepassing in een lus - toegang tot de openbare website, de gegevens indien nodig op uw Hub gebeurtenis schrijven en vervolgens de slaap-interval wachten voordat u deze helemaal opnieuw converteren. Met name:

  * Het lezen van de openbare website. Het exemplaar van de klasse RawXMLWithHeaderToJsonReader wordt om gegevens te ontvangen ready to send gebruikt van Azure/GenericWebToEH/ApiReaders/RawXMLWithHeaderToJsonReader.cs. Bronstroom in de methode GetData() wordt gelezen, en vervolgens deze kleinere stukken (dat wil zeggen records) met behulp van GetXmlFromOriginalText wordt gesplitst. 
  Met deze methode wordt de XML lezen en juist opgemaakte JSON of JSON array. Klik verwerking is gestart met de configuratie van App.config MergeToXML (standaard = leeg).
  * De ontvangende en verzendende gegevens is geïmplementeerd als een lus in de methode Process() in Program.cs. 
  De methode enqueues gescheiden na ontvangst van de resultaten van de uitvoer van GetData(), waarden op de Hub van de gebeurtenis.

## <a name="next-steps"></a>Volgende stappen

De oplossing wordt geïmplementeerd, klonen of download de toepassing [GenericWebToEH](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/) , bewerk het bestand App.config bouwen en ten slotte publiceren. Wanneer u de toepassing hebt gepubliceerd, kunt u deze in de Azure klassieke portal onder Cloud Services uitgevoerd zien en kunt u bepaalde configuratie-instellingen (zoals de gebeurtenis Hub doel en de slaap-interval) wijzigen in het tabblad **configureren** .

Zie meer voorbeelden van gebeurtenis Hubs in de [galerie met Azure monsters](https://azure.microsoft.com/documentation/samples/?service=event-hubs) en op [MSDN](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5).
