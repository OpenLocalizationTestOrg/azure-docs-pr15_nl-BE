<properties
    pageTitle="Toepassingen distribueren naar Azure App-Service"
    description="Meer informatie over toepassingen distribueren naar App Service werk"
    keywords="App-service, azure app service, implementatie, implementeren"
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="dariagrigoriu"/>

# <a name="azure-app-service-deployment-overview"></a>Azure App-Service implementatie-overzicht

Azure App-Service biedt een rijke en geïntegreerde functie voor het maken van krachtige en flexibele implementatie werkstromen instellen. App-implementatie kunt gebruikmaken van opties, zoals continue integratie of lokale bronbeheer publiceren, WebDeploy en FTP. De aanbevolen methode voor app-productie-implementatie is een implementatie sleuf swap. Implementatie van "slots" vertegenwoordigen staging en integratie-omgevingen die zijn gekoppeld aan productie apps. Implementatie van "slots" worden geconfigureerd en gericht met webverkeer voor validatie en verkeer kan op verzoek voor de implementatie van de productie, geen tijd worden verwisseld en geautomatiseerde opgewarmd. De stappen van de implementatieworkflow van een kunnen eenvoudig worden geautomatiseerd via release management producten zoals Visual Studio vrij. Dit is handig voor coördinatie met andere bronnen van de oplossing (bijv. gegevens opslaan), herhaling en replicatie tussen meerdere eenheden van de implementatie. 

[AZURE.INCLUDE [app-service-blueprint-deployment](../../includes/app-service-blueprint-deployment.md)]
