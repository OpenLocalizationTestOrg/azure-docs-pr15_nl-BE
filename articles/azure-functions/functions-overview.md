<properties
   pageTitle="Azure functies overzicht | Microsoft Azure"
   description="Inzicht in Azure functies gebruiken voor het optimaliseren van asynchrone werklasten in minuten."
   services="functions"
   documentationCenter="na"
   authors="mattchenderson"
   manager="erikre"
   editor=""
   tags=""
   keywords="Azure functies, functies, verwerking van de gebeurtenis, webhooks, dynamische compute, zonder server-architectuur"/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/29/2016"
   ms.author="cfowler;mahender;glenga"/>
   
   
# <a name="azure-functions-overview"></a>Overzicht van functies voor Azure

Azure functies is een oplossing voor het uitvoeren van kleine stukjes code, of "functies", eenvoudig in de cloud. Kunt u alleen de code die u nodig hebt voor het probleem bij de hand, zonder dat een volledige-toepassing of de infrastructuur uit te voeren. Deze ontwikkeling nog productiever kunt maken en kunt u de taal van keuze, zoals C#, F #, Node.js, Python of PHP. Betaal alleen voor de uitvoering van de code en vertrouwen Azure te schalen als nodig is.

Dit onderwerp bevat een overzicht van de functies van Azure. Als u wilt meedoen en aan de slag met Azure functies beginnen met [uw eerste Azure functie maken](functions-create-first-azure-function.md). Als u op zoek bent voor meer technische informatie over functies, Zie de [Naslaginformatie voor ontwikkelaars](functions-reference.md).

## <a name="features"></a>Functies

Hier volgen enkele belangrijke functies van Azure functies:
    
* **Keuze van taal** - schrijven-functies met C#, F #, Node.js, Python, PHP, batch, bash, Java of uitvoerbare bestanden.
* **Pay-per-use prijsmodel** - betalen alleen voor de tijd waarop de code wordt uitgevoerd. De optie dynamische App Service plannen in de [sectie prijzen](#pricing) hieronder weergegeven.  
* **Breng uw eigen afhankelijkheden** - functies ondersteunt NuGet en NPM, zodat u uw favoriete bibliotheken kunt.  
* **Geïntegreerde beveiliging** - functies met OAuth-providers zoals Azure Active Directory, Facebook, Google, Twitter en Microsoft-Account beveiligen HTTP geactiveerd.  
* **Vereenvoudigd integratie** - eenvoudig gebruikmaken van Azure services en oplossingen van de software-as-a-service (SaaS). Zie de [sectie integraties](#integrations) hieronder voor enkele voorbeelden.  
* **Flexibele ontwikkeling** - Code de functies rechts in de portal of continue integratie instellen en implementeren van uw code via GitHub, Visual Studio Team Services en andere [ontwikkelprogramma's ondersteund](../app-service-web/web-sites-deploy.md#deploy-using-an-ide).  
* **Open-source** - functies van de runtime is open source en [beschikbaar op GitHub](https://github.com/azure/azure-webjobs-sdk-script).  

## <a name="what-can-i-do-with-functions"></a>Wat kan ik doen met functies

Azure functies is een geweldige oplossing voor de verwerking van persoonsgegevens, integratie van systemen, werken met de internet-van-dingen (IoT) en simple API's en microservices op te bouwen. Houd rekening met functies voor taken als afbeelding of order verwerking, het bestandsonderhoud, langdurige taken die u wilt uitvoeren in een thread op de achtergrond of voor alle taken die u wilt uitvoeren op een schema. 

Functies biedt sjablonen om mee aan de slag met belangrijke scenario's, waaronder de volgende:

* **BlobTrigger** - opslag van proces Azure BLOB's wanneer ze worden toegevoegd aan containers. U kunt dit gebruiken voor de grootte van afbeeldingen aanpassen.
* **EventHubTrigger** - reageren op gebeurtenissen die aan een gebeurtenis Azure Hub geleverd. Met name handig voor toepassing instrumentation gebruiker ervaring of workflow verwerking en scenario's Internet dingen (IoT).
* **Algemene webhook** - proces webhook HTTP-aanvragen van elke service die webhooks ondersteunt.
* **Webhook GitHub** - reageren op gebeurtenissen die in de opslagplaatsen van de GitHub plaatsvinden. Zie voor een voorbeeld, [Maak een webhook of API-functie](functions-create-a-web-hook-or-api-function.md).
* **HTTPTrigger** - Trigger de uitvoering van de code met behulp van een HTTP-aanvraag.
* **QueueTrigger** - reageren op berichten die in een wachtrij voor de opslag van Azure aankomen. Zie voor een voorbeeld [maken een Azure-functie die wordt gebonden aan een Azure service](functions-create-an-azure-connected-function.md).
* **ServiceBusQueueTrigger** - code verbinden met andere Azure services of lokale diensten door te luisteren naar berichtwachtrijen. 
* **ServiceBusTopicTrigger** - code verbinden met andere Azure services of lokale diensten met een abonnement op onderwerpen. 
* **TimerTrigger** - opruimen of andere batchtaken uitvoeren op een vooraf bepaald schema. Zie voor een voorbeeld, [een gebeurtenis maken functie verwerkt](functions-create-an-event-processing-function.md).

Azure functies ondersteunt *triggers*, die zijn manieren om te beginnen de uitvoering van de code, en *Bindingen*zijn manieren om te coderen voor invoer en uitvoer van gegevens vereenvoudigen. Zie voor een gedetailleerde beschrijving van de triggers en de bindingen die Azure functies biedt, [Azure, functies, triggers en bindingen: referentie voor ontwikkelaars](functions-triggers-bindings.md).


## <a name="integrations"></a>Integraties

Azure functies geïntegreerd met tal van Azure en 3de partij diensten. U kunt deze de functie activeren en starten kan worden uitgevoerd of dienen als invoer en uitvoer voor de code. De volgende service-integratie worden door Azure-functies ondersteund. 

* Azure DocumentDB
* Azure gebeurtenis Hubs 
* Azure Mobile Apps (tabellen)
* Azure melding Hubs
* Azure Service Bus (wachtrijen en onderwerpen)
* Azure opslag (blob, wachtrijen en tabellen) 
* GitHub (webhooks)
* Op locatie (met Bus Service)

## <a name="pricing"></a>Wat kost functioneert kosten?

Azure functies heeft twee soorten prijzen plannen, kiezen die het beste past bij uw behoeften: 

* **Dynamische Hosting plan** - wanneer de functie wordt uitgevoerd, Azure biedt al het nodige rekenkracht. U hebt geen zorgen te maken over resourcemanagement en u betaalt alleen voor de tijd die de programmacode wordt uitgevoerd. Volledige prijsinformatie zijn beschikbaar op de [pagina met prijzen van functies](/pricing/details/functions). 

* **App serviceplan** - uw net als het web, mobiele en apps API-functies uitvoeren. Wanneer u de App Service al voor andere toepassingen gebruikt, kunt u uw functies uitvoeren op hetzelfde plan zonder extra kosten. Volledige details vindt u op de [pagina App serviceprijzen](/pricing/details/app-service/).

Zie voor meer informatie over het aanpassen van de functies, [functies Azure schalen](functions-scale.md).

##<a name="next-steps"></a>Volgende stappen

+ [Maak uw eerste Azure-functie](functions-create-first-azure-function.md)  
Meteen en maak uw eerste functie met behulp van de functies van Azure quickstart. 
+ [Azure functies: referentie voor ontwikkelaars](functions-reference.md)  
Meer technische informatie over de functies van Azure runtime en een verwijzing biedt voor codering, functies en triggers en bindingen te definiëren.
+ [Azure-functies testen](functions-test-a-function.md)  
Beschrijving van verschillende hulpprogramma's en technieken voor het testen van de functies.
+ [Schaalaanpassing van Azure functies](functions-scale.md)  
Serviceplannen beschikbaar met Azure-functies, met inbegrip van het dynamische serviceplan en het kiezen van het juiste plan beschreven. 
+ [Meer informatie over Azure App Service](../app-service/app-service-value-prop-what-is.md)  
Azure functies maakt gebruik van het platform Azure App Service voor basisfunctionaliteit zoals implementaties, omgevingsvariabelen en diagnostische gegevens. 