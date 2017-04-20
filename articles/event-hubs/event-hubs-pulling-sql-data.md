<properties
   pageTitle="SQL-gegevens kunnen ophalen in Azure gebeurtenis Hubs | Microsoft Azure"
   description="Overzicht van de gebeurtenis Hubs importeren vanuit de SQL-voorbeeld"
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

# <a name="pulling-data-from-sql-into-an-azure-event-hub"></a>Bepaalde gegevens van SQL in een Hub Azure-gebeurtenis

Een typische architectuur voor een toepassing voor het verwerken van real-time gegevens omvat het eerst op een gebeurtenis Azure Hub duwen. Een scenario IoT, zoals het toezicht op het verkeer op verschillende delen van baanvakken van een weg, of een scenario voor games, toezicht op de acties van een Houd van contestants, frenzied, of een onderneming scenario, zoals monitoring gebouw bezetting kan zijn. In deze gevallen de producenten gegevens zijn over het algemeen externe objecten produceren tijdreeks gegevens die u moet verzamelen, analyseren, opslaan en verwerken en u kan hebben geïnvesteerd veel inspanning in het gebouw van de infrastructuur voor deze processen. Wat u doen als u brengen in de gegevens van iets als een database in plaats van een bron wilt van gegevensstromen, en in combinatie met de streaming gegevens gebruiken? Houd rekening met het geval waar u Azure Stream Analytics, externe Data Explorer (RDX) of een ander hulpmiddel te analyseren en te reageren op de gegevens vanuit Microsoft Dynamics AX langzaam wijzigen of een aangepaste Faciliteitenbeheer systeem gebruiken? U lost dit probleem, hebben we geschreven en open-die afkomstig zijn, een kleine wolk steekproef die u kunt aanpassen en implementeren die de gegevens ophalen uit een SQL-tabel en druk op een Hub Azure gebeurtenis gebruiken als invoer in de analytische downstream-toepassingen. Realiseert zich dat dit een zeldzaam scenario en het tegenovergestelde van wat u normaal doet met een Hub voor de gebeurtenis. Als u zelf in de situatie waarin dit is wat u moet doen vindt, kunt u de code vinden in Azure monsters galerie [hier](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-import-from-sql/).  

Houd er rekening mee dat de code in dit voorbeeld alleen dat een monster is. Het is **niet** bedoeld als een productietoepassing, en geen is geprobeerd zodat het geschikt is voor gebruik in een dergelijke omgeving - is een DIY, stricly ontwikkelaars zijn gericht, voorbeeld. U moet controleren van alle soorten beveiliging, prestaties, functionaliteit en kosten factoren voordat op een end-to-end-architectuur.

## <a name="application-structure"></a>Toepassingsstructuur

De toepassing is geschreven in C# en het bestand readme.md in de steekproef bevat alle informatie die u wilt wijzigen, samenstellen en publiceren van de toepassing. De volgende secties vindt een overzicht van de werking van de toepassing van hoog niveau.

We beginnen met de veronderstelling dat u toegang tot een Azure SQL-tabel hebt. U moet ook een gebeurtenis Azure Hub hebt ingesteld en weet dat de verbinding nodig voor toegang tot deze tekenreeks.

Wanneer u de oplossing SqlToEventHub start, wordt een configuratiebestand (App.config) om een aantal dingen, zoals wordt beschreven in het bestand readme.md gelezen. Dit zijn behoorlijk geen uitleg, zoals de naam van de tabel, enz en is niet nodig om de uitleg hier rehash. 

Zodra de toepassing het config-bestand lezen heeft, het gaat in een lus, de SQL-tabel te lezen en records op de hub gebeurtenis duwen, en vervolgens wachten op een door de gebruiker gedefinieerde slaap-interval voordat u deze helemaal opnieuw. Er zijn een paar dingen opgemerkt:

1. De toepassing is gebaseerd op de veronderstelling dat de SQL-tabel door een extern proces wordt bijgewerkt en u wilt verzenden alle en alleen de updates op de Hub van een gebeurtenis.
2. De SQL-tabel moet een veld met een unieke en toenemende getal, bijvoorbeeld een record numer. Dit is net zo eenvoudig als een veld met de naam 'Id' of iets anders dat wordt verhoogd als wat die database bijgewerkt records zoals "Creation_time" of "Sequence_number" toegevoegd. De toepassing notities en wordt de waarde van dat veld in elke iteratie. In een latere stap de lus is doorlopen, de toepassing in feite query's in de tabel voor alle records waarin de waarde van dat veld groter is dan de waarde werd het de laatste keer dat de lus is doorlopen. Wij bellen het deze laatste waarde de "offset".
3. De toepassing wordt een tabel gemaakt 'TableOffsets' bij het starten, voor het opslaan van de verschuiving. De tabel wordt gemaakt met de query 'CreateOffsetTableQuery' in het config-bestand is gedefinieerd. 
4. Er zijn meerdere query's gebruikt voor het werken met de offset-tabel in het config-bestand gedefinieerd als 'OffsetQuery', 'UpdateOffsetQuery' en 'InsertOffsetQuery'. U moet deze niet wijzigen.
5. Ten slotte is de query "DataQuery" wordt gedefinieerd in het config-bestand de query die wordt uitgevoerd om op te halen records uit de SQL-tabel. Het is momenteel alleen beschikbaar op de top 1000 records in elke stap de lus ter optimalisatie - als bijvoorbeeld 25.000 records sinds de laatste query zijn toegevoegd aan de database dit kan even duren de query uit te voeren. 1000 records van de query telkens beperkt, worden de query's veel sneller. De top 1000 eenvoudig selecteren duwt opeenvolgende batches van 1000 records op de hub van de gebeurtenis.    

## <a name="next-steps"></a>Volgende stappen

De oplossing wordt geïmplementeerd, klonen of download de toepassing SqlToEventHub, bewerk het bestand App.config bouwen en ten slotte publiceren. Wanneer u de toepassing hebt gepubliceerd, kunt u zien in Azure klassieke portal onder Cloud Services wordt uitgevoerd en de gebeurtenissen die aan de gebeurtenis hub controleren. Houd er rekening mee dat de frequentie wordt bepaald door twee dingen: de frequentie van de updates voor de SQL-tabel en de slaap-interval dat u hebt opgegeven in het configuratiebestand voor de toepassing.