<properties
    pageTitle="Azure Service App: Schaling, toepassingen App Service"
    description="Informatie over alles van de schalen van toepassing in de App-Service."
    keywords="App-service, azure app service, schaal, schaalbare, app-serviceplan servicekosten app"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="byvinyal"/>

# <a name="azure-app-service-scaling-app-service-applications"></a>Azure Service App: Schaling, toepassingen App Service

Toepassingen die worden gehost in Azure App-Service kunnen [enorme omvang bereiken](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).
Schaal van een toepassing is echter een complexe probleem waarvoor een oplossing "één maat voor iedereen". Uw toepassing er goed op schaal zijn 3 belangrijke gebieden die zal bijdragen aan het succes van uw toepassingen:

1. Informatie over de toepassingsarchitectuur van uw en de zwakke punten.
    * Uw toepassing Stateful is? Stateless?
    * Wat zijn de onderdelen van uw toepassing?
        * Waar zijn de knelpunten in de toepassing?
    * Bij het laden wordt toegepast op uw app, breken wat eerst?
2. Wat zijn de verwachte belasting en prestaties eisen.
    * Moet de toepassing fungeren duizend gebruikers? of een miljoen?
    * Wordt verkeer afkomstig van één geografische locatie of wereldwijd?
    * Zijn er variaties per seizoen? pieken van verkeer?
    * Hoe snel moet de app reageren? 1 seconde? 1 milliseconde?
3. Begrijpen en het hosten van uw app platform goed te benutten.
    * Welke functies moet ik gebruikmaken van mijn schaal doelstellingen bereiken?

In deze sectie kunt u inzicht in de factoren en hulp bij het opstellen van een strategie die profiteert van de noodzakelijke onderdelen App Service om uw schaalbaarheid doelen te bereiken.

[AZURE.INCLUDE [app-service-blueprint-scaling-app-service-applications](../../includes/app-service-blueprint-scaling-app-service-applications.md)]
