<properties
pageTitle="Azure Active Directory-toepassing en Service Principal objecten | Microsoft Azure"
description="Een bespreking van de relatie tussen toepassing en service SPN-objecten in Active Directory Azure"
documentationCenter="dev-center-name"
authors="bryanla"
manager="mbaldwin"
services="active-directory"
editor=""/>

<tags
ms.service="active-directory"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="identity"
ms.date="08/10/2016"
ms.author="bryanla;mbaldwin"/>

# <a name="application-and-service-principal-objects-in-azure-active-directory"></a>Toepassing en service SPN-objecten in Active Directory Azure
Als u over een Azure Active Directory (AD) 'application' leest, is het niet altijd duidelijk precies wat wordt bedoeld door de auteur. Het doel van dit artikel is duidelijker, door het definiëren van de conceptuele en concrete aspecten van Azure AD toepassingsintegratie, met een voorbeeld van registratie en toestemming voor een [toepassing met meerdere huurder](active-directory-dev-glossary.md#multi-tenant-application).

## <a name="overview"></a>Overzicht
Een toepassing AD Azure is breder dan alleen een stuk van software. Het is een algemene term, verwijst niet alleen naar toepassingen, maar ook de registratie (aka: identiteit configuratie) met Azure AD, die kan deelnemen aan verificatie en machtiging 'gesprekken' bij uitvoering. Per definitie werken een toepassing in de rol van een [client](active-directory-dev-glossary.md#client-application) (een resource verbruikt), een [resource](active-directory-dev-glossary.md#resource-server) serverfunctie bloot API's (clients) of zelfs beide. Het gesprek-protocol is gedefinieerd door een [stroom OAuth 2.0 vergunning verlenen](active-directory-dev-glossary.md#authorization-grant), met het doel van de client of de resource waardoor toegang/van een resource om gegevens te beschermen respectievelijk. Nu eens een niveau lager en Zie hoe de toepassing AD Azure model staat voor een toepassing intern. 

## <a name="application-registration"></a>Registratie van toepassingen
Wanneer u een toepassing registreren in [Azure klassieke portal][AZURE-Classic-Portal], twee objecten worden gemaakt in uw huurder Azure AD: een application-object en een service principal-object.

#### <a name="application-object"></a>Application-object
Een toepassing AD Azure is *gedefinieerd* door de één en alleen application-object dat zich in de Azure AD huurder waarvoor de aanvraag is geregistreerd bevindt, hierna aangeduid als "home" huurder van de toepassing. Het object application identity-gerelateerde informatie voor een toepassing en is de sjabloon van waaruit de bijbehorende service principal objecten worden *afgeleid* voor gebruik tijdens runtime. 

U kunt zien van de toepassing als de *algemene* weergave van de toepassing (voor gebruik in alle huurders) en de service principal als de *plaatselijke* vertegenwoordiging (voor gebruik in een bepaalde huurder). De grafiek van Azure AD [Toepassingsentiteit] [ AAD-Graph-App-Entity] definieert het schema voor een application-object. Een application-object heeft dus een 1:1 relatie met de softwaretoepassing en een 1:*n* -relatie met de bijbehorende *n* service principal of meer objecten.

#### <a name="service-principal-object"></a>Service principal-object
De belangrijkste service-object bepaalt het beleid en de machtigingen voor een toepassing, die de basis vormen voor een beveiligings-principal voor de toepassing toegang tot bronnen in runtime. De grafiek van Azure AD [entiteit ServicePrincipal] [ AAD-Graph-Sp-Entity] het schema voor een service principal-object definieert. 

Een service principal object is in elke huurder waarvan een exemplaar van het gebruik van de toepassing moet worden weergegeven, waardoor veilige toegang tot bronnen die eigendom zijn van de gebruikersaccounts van deze huurder vereist. Een toepassing één huurder heeft slechts één service principal (in de home huurder). Een multi-huurder [webtoepassing](active-directory-dev-glossary.md#web-client) hebben ook een service principal in elke huurder waar een beheerder of een gebruiker (s) van de huurder die toestemming hebben gegeven, zodat het gebruik van de bronnen. Na instemming, zal het hoofdvoorwerp van de service worden geraadpleegd voor toekomstige vergunning aanvragen. 

> [AZURE.NOTE] Eventuele wijzigingen in het object application worden ook doorgevoerd in de service SPN-object in de toepassing thuis huurder alleen (de huurder waar het is geregistreerd). Voor toepassingen met meerdere huurder, naar het application-object niet wijzigingen in de consument huurders service SPN-objecten, totdat de huurder consument toegang verwijderd en opnieuw toegang verleent.

## <a name="example"></a>Voorbeeld
In het volgende diagram ziet u de relatie tussen een toepassing op het application-object en de bijbehorende service van SPN-objecten in de context van een voorbeeldtoepassing met meerdere huurder **HR app**genoemd. Er zijn drie Azure AD huurders in dit scenario: 

- **Adatum** - de huurder gebruikt door het bedrijf dat de **HR-app** ontwikkeld
- **Contoso** - de huurder gebruikt door de organisatie Contoso is een consument van de **HR-app**
- **Fabrikam** - de huurder gebruikt door het bedrijf Fabrikam die ook de **HR-app verbruikt**

![Relatie tussen een application-object en een service principal-object](./media/active-directory-application-objects/application-objects-relationship.png)

In het vorige diagram is stap 1 het proces van het maken van de toepassing en service SPN-objecten in huis pachter van de toepassing.

In stap 2 wanneer beheerders van Contoso en Fabrikam toestemming, is een service principal-object gemaakt in Azure AD-huurder van hun bedrijf en machtigingen die de beheerder wordt toegekend. Vergeet niet dat de HR-app worden geconfigureerd/ontworpen kan voor toestemming door gebruikers voor individueel gebruik.

In stap 3 hebt de consument huurders van de HR-toepassing (Contoso en Fabrikam) elk hun eigen service principal-object. Elke geeft het gebruik van een exemplaar van de toepassing in runtime wordt bepaald door de machtigingen ingestemd door de beheerder respectieve.

## <a name="next-steps"></a>Volgende stappen
Application-object van de toepassing toegankelijk zijn via de API Azure AD grafiek vertegenwoordigd door de OData- [Toepassingsentiteit][AAD-Graph-App-Entity]

Van de toepassing service principal object toegankelijk via de API Azure AD grafiek vertegenwoordigd door de OData- [ServicePrincipal-entiteit][AAD-Graph-Sp-Entity]



<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AZURE-Classic-Portal]: https://manage.windowsazure.com