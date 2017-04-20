<properties
   pageTitle="Azure functies schaalaanpassing | Microsoft Azure"
   description="Begrijpen hoe Azure functies om te voldoen aan de behoeften van uw werkbelasting gebeurtenisgestuurde schalen."
   services="functions"
   documentationCenter="na"
   authors="dariagrigoriu"
   manager="erikre"
   editor=""
   tags=""
   keywords="Azure functies, functies, verwerking van de gebeurtenis, webhooks, dynamische compute, zonder server-architectuur"/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="reference"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="dariagrigoriu"/>

# <a name="how-to-scale-azure-functions"></a>Schaalaanpassing van Azure functies

## <a name="introduction"></a>Inleiding

Een voordeel van Azure functies is dat compute resources alleen worden verbruikt, wanneer dit nodig is. Dit betekent dat u niet voor niet-actieve VMs betaalt of reserveren capaciteit voor wanneer u het mogelijk moet zijn. In plaats daarvan wijst het platform rekenkracht wanneer uw code wordt uitgevoerd, als nodig is schaalt om de belasting te verwerken en vervolgens naar beneden wordt geschaald wanneer de code wordt niet uitgevoerd.

Het mechanisme voor deze nieuwe functionaliteit is de dynamische serviceplan.  

Dit artikel bevat een overzicht van de werking van de dynamische serviceplan en hoe het platform worden op vraag uw code uit te voeren.

Als u nog niet bekend bent met de functies in Azure, zorg ervoor dat het artikel [overzicht van Azure functies](functions-overview.md) voor een beter begrip van de mogelijkheden ervan te controleren.

## <a name="configure-azure-functions"></a>Azure functies configureren

Twee belangrijke instellingen hebben betrekking op de schaal:

* [Azure App serviceplan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) of dynamische Service plan
* Grootte van het systeemgeheugen voor de omgeving

De kosten van een functie is afhankelijk van het abonnement dat u selecteert. Met een dynamische serviceplan is kosten een functie van de tijd en grootte van het systeemgeheugen aantal uitvoeringen. In hun kosten alleen wanneer uw code daadwerkelijk uitgevoerd.

Een App serviceplan fungeert als host voor de functies op bestaande VMs, die ook kunnen worden gebruikt voor het uitvoeren van andere code. Nadat u voor deze VMs elke maand betaalt, is er geen extra kosten voor de uitvoering van functies op deze.

## <a name="choose-a-service-plan"></a>Kies een serviceplan

Wanneer u functies wilt maken, kunt u ze op een dynamische Service plan of een [App serviceplan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)uit te voeren.
In het plan App-Service worden op een specifieke VM, net als het web apps werken vandaag voor Basic, Standard of Premium SKU's uw functies uitgevoerd.
Deze speciale VM is toegewezen aan uw toepassingen en functies en is altijd beschikbaar of code wordt actief uitgevoerd of niet. Dit is een goede optie als u bestaande, onder gebruikt VMs waarop andere code hebt of als u verwacht dat continu of vrijwel onophoudelijk functies uitvoeren. Een VM decouples-kosten van runtime en de geheugen. Hierdoor kunt u de kosten van veel langdurige functies aan de kosten van de een of meer VMs dat ze worden uitgevoerd op beperken.

[AZURE.INCLUDE [Dynamic Service plan](../../includes/functions-dynamic-service-plan.md)]
