<properties 
    pageTitle="Basisbegrippen voor beheer-API" 
    description="Meer informatie over API's, producten, functies, groepen en andere belangrijke concepten voor de beheer-API." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

#<a name="what-is-api-management"></a>Wat is de beheer-API?

Beheer-API helpt organisaties API's om externe partner en interne ontwikkelaars om te ontgrendelen van het potentieel van hun gegevens en -services te publiceren. Bedrijven zijn overal die willen uitbreiden van hun activiteiten als een platform voor digitale, maken nieuwe kanalen, zoeken naar nieuwe klanten en gereden diepere betrokkenheid bij bestaande. Beheer-API biedt de kerncompetenties om een succesvolle API-programma via de ontwikkelaar betrokkenheid, meer inzicht, analytics, beveiliging en bescherming te garanderen.

Bekijk de volgende video voor een overzicht van Azure API beheer en informatie over het gebruik van beheer-API veel functies toevoegen aan uw API, met inbegrip van toegangsbeheer, snelheidsbeperking, controle, logboekregistratie en antwoord in cache plaatsen, met minimale inspanning van uw kant.

> [AZURE.VIDEO azure-api-management-overview]

Beheerders maken gebruik van de beheer-API API's. De API bestaat uit een of meer bewerkingen en elke API kan worden toegevoegd aan een of meer producten. Als u een API, ontwikkelaars zich abonneren op een product met deze API, en vervolgens ze kunnen aanroepen van de API-bewerking, onder van gebruiksbeleid dat van kracht.

Dit onderwerp bevat een overzicht van de basisbegrippen van beheer-API.

>[AZURE.NOTE] Zie voor meer informatie de [Cloud-gebaseerde API beheer: gebruik te maken van de kracht van de API's van](http://j.mp/ms-apim-whitepaper) PDF-witboek. Dit inleidende witboek over beheer-API door CITO-onderzoek heeft betrekking op: 
>
> - Gemeenschappelijke API eisen en uitdagingen
>     - Ontkoppeling van de API's en presenteren van de facades
>     - Slag van ontwikkelaars snel gebruiksklaar
>     - Toegang beveiligen
>     - Analytics en statistieken
> - Krijgen controle over en inzicht met een API Management platform
> - Met behulp van cloud versus lokale oplossingen
> - Azure API beheer

## <a name="apis"> </a>-API's en bewerkingen

API's vormen de basis van een exemplaar van de service Management API. Elke API vertegenwoordigt een reeks bewerkingen die beschikbaar zijn voor ontwikkelaars. Elke API bevat een verwijzing naar de back-end-service dat de API implementeert en de bewerkingen toewijzen aan de acties van de back-end-service. Bewerkingen in de beheer-API zijn uiterst configureerbaar controle over URL-toewijzing, query en pad parameters aanvraag en antwoord inhoud en bewerking antwoord in cache opslaan. Limiet, quota en IP-restrictiebeleid kunnen ook op het niveau van de afzonderlijke bewerking of API worden geïmplementeerd.

Zie [API's maken][] en [het toevoegen van bewerkingen voor een API][]voor meer informatie.


## <a name="products"></a> Producten

Producten zijn hoe de API's voor ontwikkelaars zijn opgehaald. Producten in de beheer-API API's voor één of meer hebben en zijn geconfigureerd met een titel, beschrijving en gebruiksvoorwaarden. Producten kunnen worden **geopend** of **beveiligd**. Beschermde producten moeten geabonneerd op voordat ze kunnen worden gebruikt, terwijl deze was geopend producten kunnen worden gebruikt zonder een abonnement. Wanneer een product klaar voor gebruik door ontwikkelaars is kan worden gepubliceerd. Nadat deze is gepubliceerd, kan worden bekeken (en geabonneerd op beschermde producten) door ontwikkelaars. Abonnement goedkeuring kan is geconfigureerd op het productniveau van het en de goedkeuring van de beheerder vereisen, of worden automatisch goedgekeurd.

Groepen worden gebruikt voor het beheren van de zichtbaarheid van producten voor ontwikkelaars. Producten zichtbaarheid toekennen aan groepen en ontwikkelaars kunnen bekijken en zich abonneren op de producten die zichtbaar zijn voor de groepen waarin ze behoren. 

Zie voor meer informatie, [het maken en publiceren van een product][] en de volgende video.

> [AZURE.VIDEO using-products]

## <a name="groups"></a> Groepen

Groepen worden gebruikt voor het beheren van de zichtbaarheid van producten voor ontwikkelaars. API-Management heeft de volgende systeemgroepen van onveranderbare.

-   **Beheerders** - abonnement Azure beheerders zijn leden van deze groep. Beheerders beheren Management API-exemplaren, maken van de API's, activiteiten en producten die worden gebruikt door ontwikkelaars.
-   **Ontwikkelaars** - geverifieerde developer-portal gebruikers in deze groep vallen. Ontwikkelaars zijn de klanten die het bouwen van toepassingen met behulp van de API's. Ontwikkelaars krijgen toegang tot het portal voor ontwikkelaars en toepassingen maken die de bewerkingen van een API aanroept.
-   **Gasten** - ontwikkelaar niet-geverifieerde gebruikers van het webportaal, bijvoorbeeld potentiële klanten naar de portal voor ontwikkelaars van een exemplaar van de beheer-API vallen in deze groep. Deze kunnen toegang worden verleend bepaalde alleen-lezen, zoals de mogelijkheid API's bekijken, maar deze niet aanroepen.

Naast deze systeemgroepen kunnen beheerders aangepaste groepen of [Maak gebruik van externe groepen in de bijbehorende Azure Active Directory huurders](api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group)maken. Aangepaste en externe groepen kunnen worden gebruikt naast systeemgroepen waardoor ontwikkelaars zichtbaarheid en toegang tot API-producten. U kan bijvoorbeeld een aangepaste groep maken voor ontwikkelaars die zijn gekoppeld aan een organisatie bepaalde partner en zij uitsluitend toegang hebben tot de API's van een product met de relevante API's. Een gebruiker kan lid zijn van meer dan één groep.

Zie voor meer informatie, [het maken en gebruiken van groepen][].

## <a name="developers"></a> Ontwikkelaars

Ontwikkelaars vertegenwoordigen de gebruikersaccounts in een exemplaar van de service Management API. Ontwikkelaars kunnen worden gemaakt of uitgenodigd deel te nemen aan door beheerders of ze vanuit de [Developer-portal][]kunnen aanmelden. Elke ontwikkelaar kan is een lid van een of meer groepen, en zich abonneren op de producten die zichtbaarheid aan deze groepen verlenen.

Wanneer ontwikkelaars zich op een product abonneren werd zij de primaire en secundaire sleutel voor het product. Deze sleutel wordt gebruikt bij gesprekken in de API's van het product.

Zie voor meer informatie [het maken of ontwikkelaars uitnodigen][] en [het koppelen van groepen met ontwikkelaars][].

## <a name="policies"></a> Beleid

Beleid is een krachtige mogelijkheid van beheer-API waarmee de uitgever van het gedrag van de API via configuratie wijzigen. Beleid is een verzameling instructies die worden uitgevoerd achter elkaar op de aanvraag of antwoord van een API. Populaire overzichten bevatten Indelingsconversie van XML naar JSON en snelheidsbeperking oproep voor het beperken van het bedrag van de inkomende oproepen van een ontwikkelaar en veel ander beleid beschikbaar zijn.

Beleid-expressies kunnen worden gebruikt als het kenmerkwaarden of tekst in een van de API informatiebeheerbeleid tenzij u iets anders heeft het beleid. Sommige beleid zoals het beleid voor [Besturing](https://msdn.microsoft.com/library/azure/dn894085.aspx#choose) en [variabele](https://msdn.microsoft.com/library/azure/dn894085.aspx#set-variable) zijn gebaseerd op expressies van beleid. Zie [Geavanceerde beleid](https://msdn.microsoft.com/library/azure/dn894085.aspx#AdvancedPolicies), [beleid voor expressies](https://msdn.microsoft.com/library/azure/dn910913.aspx), en bekijk de volgende video voor meer informatie.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

Zie voor een volledige lijst van het beleid van de beheer-API [reference beleid][]. Zie [beleid voor API][]voor meer informatie over het gebruiken en configureren van beleid. Zie voor een zelfstudie over het maken van een product met tarief limiet en quota, [hoe maken en configureren van instellingen voor geavanceerde product][]. Zie de volgende video voor een demonstratie.

> [AZURE.VIDEO rate-limits-and-quotas]

## <a name="developer-portal"></a> Developer-portal

De developer-portal is waar ontwikkelaars kunnen de API's, weergave en call-bewerkingen en abonneren op producten. Potentiële klanten kunnen de developer-portal op de website aanmelden en API's en bewerkingen weergeven. De URL voor de developer-portal bevindt zich op het dashboard in de klassieke Azure-Portal voor uw exemplaar van de service Management API.

U kunt het uiterlijk van de developer-portal aanpassen door aangepaste inhoud toevoegen, stijlen aanpassen en uw huisstijl toe te voegen.

## <a name="api-management-and-the-api-economy"></a>Beheer-API en de economie API

Meer informatie over beheer-API, bekijk de volgende presentatie van de Conferentie over het Microsoft Ignite 2015.

> [AZURE.VIDEO microsoft-ignite-2015-azure-api-management-and-the-api-economy]

[APIs and operations]: #apis
[Products]: #products
[Groups]: #groups
[Developers]: #developers
[Policies]: #policies
[Developer-portal]: #developer-portal

[Het maken van API 's]: api-management-howto-create-apis.md
[Bewerkingen toevoegen aan een API]: api-management-howto-add-operations.md
[Het maken en publiceren van een product]: api-management-howto-add-products.md
[Het maken en gebruiken van groepen]: api-management-howto-create-groups.md
[Groepen koppelen aan ontwikkelaars]: api-management-howto-create-groups.md#associate-group-developer
[Hoe maken en configureren van instellingen voor geavanceerde product]: api-management-howto-product-with-rules.md
[Het maken of ontwikkelaars uitnodigen]: api-management-howto-create-or-invite-developers.md
[Beleidsverwijzing]: api-management-policy-reference.md
[Beleidsregels voor informatiebeheer API]: api-management-howto-policies.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance



 
