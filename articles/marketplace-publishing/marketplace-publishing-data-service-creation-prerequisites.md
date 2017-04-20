<properties
   pageTitle="Technische minimumvereisten voor het maken van een Service voor de markt | Microsoft Azure"
   description="Inzicht in de vereisten voor het maken van een Service om te implementeren en te verkopen op de Marketplace Azure"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="hascipio; avikova" />

# <a name="technical-pre-requisites-for-creating-a-data-service-offer-for-the-azure-marketplace"></a>Technische minimumvereisten voor het maken van een Service aanbieden voor Azure Marketplace

>[AZURE.IMPORTANT] **Op dit moment we zijn niet langer onboarding alle nieuwe Data Service uitgevers. Nieuwe dataservices zal niet krijgen goedgekeurd voor de aanbieding.** Als u beschikt over een SaaS zakelijke toepassing die u wilt publiceren op AppSource vindt u meer informatie [hier](https://appsource.microsoft.com/partners). Als u een IaaS toepassingen of developer service die u wilt publiceren op Azure Marketplace meer informatie vindt [hier](https://azure.microsoft.com/marketplace/programs/certified/).

Het proces grondig voordat u begint te lezen en begrijpen waar en waarom elke stap wordt uitgevoerd. Zo veel mogelijk, u moet informatie over uw bedrijf en andere gegevens voorbereiden, benodigde hulpprogramma's downloaden en/of technische componenten maken voordat u begint met het proces voor het maken van het voorstel.

U moet de volgende items klaar voordat u begint met het proces:

## <a name="make-a-decision-on-what-technology-will-be-used-to-publish-your-data-service-offer"></a>Een beslissing nemen over welke technologie wordt gebruikt voor het publiceren van uw Data Service-aanbod

Een uitgever kunt kiezen tussen verschillende technologieën als Service gegevens publiceren in Azure Marketplace. De belangrijkste technologieën die worden ondersteund die hieronder worden beschreven. Ongeacht welke technologie wordt gebruikt voor het publiceren van de Service van gegevens, de gegevens via de **OData-feed** die door de Service Marketplace Azure door de eindgebruiker worden verbruikt. Volledige informatie over de OData-service kunt u vinden op [http://www.odata.org/](http://www.odata.org/)

## <a name="sql-azure-database"></a>SQL Database Azure

Klaar in SQL Azure dataset die is de verantwoordelijkheid van de uitgever. U moet zich abonneren op Azure, inrichten van de juiste grootte van de Database en uw gegevens uploaden naar SQL Azure DB. Publisher is ook verantwoordelijk voor zijn of haar gegevens altijd up-to-date te houden. Meer informatie over het abonneren op Azure Services kunt u vinden op [https://azure.microsoft.com/services/sql-database/](https://azure.microsoft.com/services/sql-database/)


Bij het verplaatsen van de gegevens in SQL Azure, kan de markt Azure blootgesteld tabellen en weergaven. De uitgever kunt opgeven welke tabellen/weergaven en kolommen worden blootgesteld aan de eindgebruiker. De inhoudsprovider kan verder ook kolommen die door de eindgebruiker kunnen worden doorzocht en welke alleen worden geretourneerd in de nettolading opgeven. Dit biedt een hoge mate van flexibiliteit over welke gegevens in de database worden blootgesteld. Kolommen die kunnen worden doorzocht moeten worden ondersteund door een of meer database-indexen.

## <a name="rest-based-web-service"></a>Op basis van REST-webservice

Protocol ondersteund: **alleen HTTPS**

Bestaande REST gebaseerde services kunnen via de markt Azure worden blootgesteld. Omdat de dataset is altijd beschikbaar voor de eindgebruiker als een OData-feed, op de Marketplace Azure service behoeften kunnen de service toewijzen aan een OData basis service. Doet de REST op basis van eindpunten moet alle parameters als HTTP-parameters.

De payload moet in een formulier dat kan worden toegewezen aan een ATOM-antwoord. Dus het antwoord van de diensten moet worden in XML-indeling en kan uitsluitend één herhalende element met de nettolading waarden (zoals recordreeks) bevatten. De service Marketplace Azure zal toe te wijzen het herhalende knooppunt naar het knooppunt item in ATOM en de waarden van de nettolading eigenschap knooppunten binnen het post-knooppunt.

Verificatie-informatie (zoals API key, verificatie token, enz.) moet worden opgegeven als een parameter van het HTTP- of in de HTTP-header (sleutel-waardepaar) – basisverificatie wordt ook ondersteund. Een geldige productcode moet worden opgegeven en alle aanvragen via Azure Marketplace worden gesteld door middel van die sleutel. De gebruiker controle en facturering gebeurt op de laag Azure Marketplace.

Fouten die worden geretourneerd door de service moeten worden toegewezen in de HTTP-statuscodes. In het geval retourneert een XML-bestand dat de fout bevat de service gaat deze worden toegewezen door de service Marketplace Azure HTTP-statuscodes.

## <a name="soap-based-web-services"></a>Op basis van SOAP-webservices

Protocol: **alleen HTTPS**

De eisen zijn dat hetzelfde als in de REST sectie service gebaseerd. Het enige verschil is dat parameters kunnen ook worden geleverd in een XML-instantie die met elke aanvraag die via Azure Marketplace wordt geboekt naar de service van de uitgever. Dit betekent dat HTTP-parameters die de gebruiker op de front-end verstrekt-zijn wordt vertaald in de XML-elementen in een XML-document dat wordt geboekt met het verzoek aan de aanbieder van de inhoud van webservice.

## <a name="odata-based-web-services"></a>Op basis van OData-webservices

Protocol: **alleen HTTPS**

Gegevens kunnen worden weergegeven als een service OData Azure Marketplace. Het systeem zal worden doorgegeven van de service via en vervangen door de hoofdmap Marketplace Azure service – alle daaropvolgende aanroepen via Azure Marketplace gaan om de hoofdmap van de service.

OData-services hoeft niet alleen te gaan tegen een database in de backend. OData ondersteunt alle soorten opslag of de bedrijfslogica voor de service.


## <a name="next-steps"></a>Volgende stappen
Nu u de noodzakelijke voorwaarden te herzien en dat de vereiste taken uitgevoerd, kunt u verdergaan met het maken van uw Data Service aanbod zoals beschreven in de [Data Service Publishing Guide](marketplace-publishing-data-service-creation.md).

Of, als u bekijken van het algehele proces en de respectieve artikelen voor elk van de fasen publiceren wilt, gaat u naar het artikel [aan de slag: het publiceren van een aanbod op de markt Azure](marketplace-publishing-getting-started.md).

[link-acct]:marketplace-publishing-accounts-creation-registration.md
