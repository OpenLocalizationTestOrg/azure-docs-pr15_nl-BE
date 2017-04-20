<properties
   pageTitle="Wat is Microsoft Power BI ingesloten?"
   description="Power BI ingesloten kunt u rapporten Power BI integreren in uw web of mobiele toepassingen, zodat u niet hoeft te maken van aangepaste oplossingen om gegevens voor uw gebruikers"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="what-is-microsoft-power-bi-embedded"></a>Wat is Microsoft Power BI ingesloten?

In **Power BI ingesloten**, kunt u rapporten Power BI integreren in uw web of mobiele toepassingen.

![](media\powerbi-embedded-whats-is\what-is.png)

Power BI ingesloten is een **Azure service** waarmee onafhankelijke softwareleveranciers en app-ontwikkelaars naar de oppervlakte ervaringen van Power BI gegevens binnen hun toepassingen. Als ontwikkelaar u toepassingen hebt gemaakt en deze toepassingen hebben hun eigen gebruikers en verschillende set functies. Die apps kunnen ook gebeuren te hebben sommige gegevenselementen ingebouwde zoals grafieken en rapporten die nu kunnen worden aangestuurd door Power BI ingesloten. Gebruikers hoeven niet een Power BI-account om uw app te gebruiken. Ze kunnen doorgaan met het aanmelden bij uw toepassing net als voorheen, en bekijken en interactief werken met de Power BI ervaring melden zonder aanvullende licentie.

## <a name="licensing-for-microsoft-power-bi-embedded"></a>Licentiëring voor Microsoft Power BI ingesloten

In het objectmodel van **Microsoft Power BI ingesloten** gebruik is licentieverlening voor Power BI niet de verantwoordelijkheid van de eindgebruiker.  In plaats daarvan **wordt gerenderd** worden gekocht door de ontwikkelaar van de toepassing die de beelden verbruikt en aan het abonnement dat eigenaar is van de resources in rekening worden gebracht.

## <a name="microsoft-power-bi-embedded-conceptual-model"></a>Microsoft Power BI ingesloten conceptueel Model

![](media\powerbi-embedded-whats-is\model.png)

Net als elke andere service in Azure, worden bronnen voor Power BI ingesloten ingericht via de [API's van Azure ARM](https://msdn.microsoft.com/library/mt712306.aspx). In dit geval is de resource die u wilt inrichten een **Power BI werkruimte-collectie**.

## <a name="workspace-collection"></a>Werkruimte-collectie

Een **Collectie van de werkruimte** is de container op hoofdniveau Azure voor bronnen met 0 of meer **werkruimten**.  Een **werkruimte** - **collectie** bevat alle eigenschappen van de standaard Azure, alsmede de volgende:

-   **Toegangstoetsen** – sleutels die worden gebruikt bij het aanroepen van de Power BI-API's (Zie verderop) veilig.
-   **Gebruikers** : Azure Active Directory (AAD) gebruikers met beheerdersrechten voor het beheren van de Power BI werkruimte of de collectie via de portal Azure ARM API.
-   **Regio** : als onderdeel van het inrichten van een **Collectie van de werkruimte**, kunt u een regio te richten. Zie voor meer informatie [Azure regio's](https://azure.microsoft.com/regions/).

## <a name="workspace"></a>Werkruimte

Een **werkruimte** is een container van de Power BI-inhoud, waaronder datasets, rapporten en dashboards. Een **werkruimte** is leeg wanneer gemaakt. Tijdens de voorvertoning, zult u alle inhoud met behulp van Power BI bureaublad auteur en zult u uploadt naar een van uw werkruimten met behulp van de [Power BI REST API's](http://docs.powerbi.apiary.io/reference).

## <a name="using-workspace-collections-and-workspaces"></a>Met behulp van de werkruimte collecties en werkruimten
**Werkruimte collecties** en **werkruimten** zijn containers die worden gebruikt en op welke manier best past bij het ontwerp van de toepassing die u samenstelt ingedeeld. Er zijn veel verschillende manieren dat u de inhoud ervan kan rangschikken. U kunt alle inhoud in een werkruimte en app tokens later verder onderverdelen van de inhoud onder uw klanten te gebruiken. U kunt ook uw klanten plaatsen in afzonderlijke werkruimten, zodat er een scheiding tussen deze. Of u kunt gebruikers per regio in plaats van door de klant te organiseren. Dit flexibele ontwerp kunt u kiest de beste manier om inhoud te ordenen.

## <a name="cached-datasets"></a>In de cache Datasets

Datasets in de cache kan worden gebruikt in het voorbeeld.  U kunt echter gegevens in de cache niet vernieuwen zodra deze is geladen in **Microsoft Power BI ingesloten**.

## <a name="authentication-and-authorization-with-app-tokens"></a>Verificatie en autorisatie met tokens app

**Power BI ingesloten** past omleiding naar de toepassing voor het uitvoeren van alle nodige gebruikersverificatie en machtiging. Er is geen expliciete vereiste dat uw eindgebruikers klanten van Azure Active Directory (AD Azure zijn).  In plaats daarvan uw toepassing wordt uitgedrukt als **Power BI ingesloten** vergunning een Power BI-rapport genereren met behulp van **Toepassing-verificatietokens (Tokens App)**.  Deze **App Tokens** worden gemaakt indien nodig wanneer uw app wil om een rapport weer te geven.  Zie [App Tokens](power-bi-embedded-get-started-sample.md#key-flow).

![](media\powerbi-embedded-whats-is\app-tokens.png)

Toepassing-worden **Verificatietokens (Tokens App)** gebruikt voor verificatie op **Power BI ingesloten**.  Er zijn drie soorten **App Tokens**:

1.  Inrichten van Tokens - die worden gebruikt tijdens het inrichten van een nieuwe **werkruimte** in een **Werkruimte-collectie**
2.  Ontwikkeling van Tokens - gebruikt bij het rechtstreeks aanroepen van de **Power BI REST API's** maken
3.  Tokens - gebruikt bij aanroepen voor de weergave van een rapport in de iframe ingesloten insluiten

Deze tokens worden gebruikt voor de verschillende fasen van uw interacties met **Power BI ingesloten**.  De tokens zijn zo ontworpen dat u machtigingen in uw app Power BI delegeren kunt. Voor meer informatie Zie [Token Flow App](power-bi-embedded-app-token-flow.md).

## <a name="see-also"></a>Zie ook
- [Algemene scenario's voor Power BI ingesloten](power-bi-embedded-scenarios.md)
- [Aan de slag met Microsoft Power BI ingesloten](power-bi-embedded-get-started.md)
