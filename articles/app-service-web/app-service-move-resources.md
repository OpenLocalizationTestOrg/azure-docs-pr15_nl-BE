<properties
    pageTitle="Web App Resources verplaatsen naar een andere groep"
    description="Beschrijving van de scenario's waarin u Web Apps en App Services uit een resourcegroep naar de andere verplaatsen kunt."
    services="app-service"
    documentationCenter=""
    authors="ZainRizvi"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/04/2016"
    ms.author="zarizvi"/>
    
# <a name="supported-move-configurations"></a>Verplaatsen van ondersteunde configuraties

U kunt verplaatsen Azure Web App resources met behulp van de [Api van ARM Resources verplaatsen](../resource-group-move-resources.md).

Azure Web Apps ondersteunt momenteel de volgende scenario's voor verplaatsen:

* De gehele inhoud van een resourcegroep (web apps, app serviceplannen en certificaten) verplaatsen naar een andere groep 
    * Opmerking: De resourcegroep bestemming mag geen Microsoft.Web bronnen in dit scenario
* Afzonderlijke web-apps verplaatsen naar een groep van verschillende bronnen, terwijl nog steeds gehost in hun huidige app serviceplan (het abonnement app blijft in de oude resourcegroep)
