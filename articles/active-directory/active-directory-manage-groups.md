<properties
    pageTitle="Toegang tot bronnen te beheren met Azure Active Directory-groepen | Microsoft Azure"
    description="Het gebruik van groepen in Azure Active Directory voor het beheren van toegang tot lokale en cloud-toepassingen en bronnen."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""
/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>


# <a name="managing-access-to-resources-with-azure-active-directory-groups"></a>Toegang tot bronnen te beheren met Azure Active Directory-groepen

Azure Active Directory (AD Azure) is een uitgebreide identiteits- en toegangsbeheer management oplossing een krachtige set van mogelijkheden biedt voor het beheren van toegang tot lokale en cloud-toepassingen en bronnen, met inbegrip van Microsoft online services, zoals Office 365 en een wereld van Microsoft SaaS-toepassingen. In dit artikel wordt een overzicht gegeven, maar als u wilt aan de slag met Azure AD groepen nu, volg de instructies in [de beveiligingsgroepen beheren in Azure AD](active-directory-accessmanagement-manage-groups.md). Als u zien hoe u PowerShell kunt gebruiken wilt voor het beheren van groepen in Azure Active directory kunt u lezen meer in [Azure Active Directory voorbeeld cmdlets voor het groepsbeheer](active-directory-accessmanagement-groups-settings-v2-cmdlets.md).


> [AZURE.NOTE] Azure Active Directory gebruikt, moet u een account met Azure. Als u geen account hebt, kunt u [zich aanmelden voor een gratis account Azure](https://azure.microsoft.com/pricing/free-trial/).


In Azure AD is een van de belangrijkste functies de mogelijkheid om toegang tot bronnen te beheren. Deze bronnen kunnen deel uitmaken van de map, zoals in het geval van machtigingen voor het beheren van objecten door middel van rollen in de map of naar de map, zoals SaaS-toepassingen, Azure services en SharePoint-sites of lokale bronnen van externe bronnen. Er zijn vier manieren waarop die een gebruiker toegangsrechten aan een resource kan worden toegewezen:


1. Directe toewijzing

    Gebruikers kunnen worden toegewezen aan een resource rechtstreeks door de eigenaar van die bron.

2. Groepslidmaatschap

    Een groep kan worden toegewezen aan een resource door de eigenaar van de resource en door dit te doen, de leden van die groepstoegang tot de bron te verlenen. Lidmaatschap van de groep kan vervolgens worden beheerd door de eigenaar van de groep. Effectief, delegeert de eigenaar van de resource de machtiging gebruikers toe te wijzen aan de resource aan de eigenaar van de groep.

3. Op basis van een regel

    De eigenaar van de bron kunt u een regel gebruiken om welke gebruikers moeten toegang worden toegewezen aan een resource. Het resultaat van de regel is afhankelijk van de kenmerken die in die regel en de bijbehorende waarden worden gebruikt voor bepaalde gebruikers en doet, delegeert de eigenaar van de resource daadwerkelijk het recht op toegang tot de bron aan de bindende bron voor de kenmerken die worden gebruikt in de regel te beheren. De eigenaar van de bron nog steeds de regel zelf beheert en bepaalt welke kenmerken en waarden toegang tot de bron bieden.

4. Externe dienst

    De toegang tot een bron afkomstig is van een externe bron; bijvoorbeeld, een groep die uit een gezaghebbende bron, zoals een map op de bedrijfsruimten of een SaaS-app zoals werkdag wordt gesynchroniseerd. De eigenaar van de resource wordt toegewezen voor de groep voor toegang tot de bron en de externe bron beheert de leden van de groep.

  ![Overzicht van access management diagram](./media/active-directory-access-management-groups/access-management-overview.png)


## <a name="watch-a-video-that-explains-access-management"></a>Bekijk de video met uitleg over toegangsbeheer

U kunt een korte video die verklaart meer informatie over dit kijken:

**Azure AD: Inleiding tot dynamische lidmaatschap voor groepen**

> [AZURE.VIDEO azure-ad--introduction-to-dynamic-memberships-for-groups]

## <a name="how-does-access-management-in-azure-active-directory-work"></a>Hoe heeft toegang tot management in Azure Active Directory werk?
In het midden van de advertentie Azure is oplossing voor toegang tot de beveiligingsgroep. Met behulp van een beveiligingsgroep toegang tot bronnen te beheren is een bekende paradigma waarmee een flexibele en gemakkelijk te begrijpen manier om toegang te bieden tot een bron voor de voorgenomen groep gebruikers. Een groep naar een bepaalde toegang krijgen tot de bronnen die ze eigenaar rechts kunt toewijzen door de eigenaar van de resource (of de beheerder van de map). De leden van de groep de toegang worden verstrekt en de eigenaar van de bron voor het beheren van de lijst van de leden van een groep aan iemand anders, bijvoorbeeld een afdelingsmanager of een beheerder van een helpdesk rechts kunt overdragen.

![Diagram van Azure Active Directory toegang beheer](./media/active-directory-access-management-groups/active-directory-access-management-works.png)

De eigenaar van een groep kan ook die groep beschikbaar maken voor zelf aanvragen. Daarbij kunt een eindgebruiker zoeken en vinden van de groep en kunnen een aanvraag voor deelname, effectief om machtiging voor toegang tot de bronnen die worden beheerd door de groep. De eigenaar van de groep kan de groep instellen zodat deze automatisch worden goedgekeurd of worden goedgekeurd door de eigenaar van de groep moet. Wanneer een gebruiker een aanvraag voor het deelnemen aan een groep maakt, wordt de join-verzoek doorgestuurd naar de eigenaars van de groep. Een van de eigenaren van de aanvraag heeft goedgekeurd, de gebruiker is aangemeld als de gebruiker lid is van de groep. Als een van de eigenaren van de aanvraag wordt geweigerd, wordt de gebruiker gewaarschuwd maar geen deel uitmaakt van de groep.


## <a name="getting-started-with-access-management"></a>Aan de slag met access management
Klaar om aan de slag? Sommige van de fundamentele taken die u met Azure AD groepen doen kunt te proberen. Met deze mogelijkheden kunt u speciale toegang bieden tot verschillende groepen mensen voor verschillende resources in uw organisatie. Hieronder vindt u een overzicht van de eerste basisstappen.

* [Een eenvoudige regel configureren van dynamische lidmaatschappen voor een groep maken](active-directory-accessmanagement-manage-groups.md#how-can-i-manage-the-membership-of-a-group-dynamically)

* [Een groep gebruiken voor het beheren van SaaS-toepassingen](active-directory-accessmanagement-group-saasapps.md)

* [Een groep maken beschikbaar voor de eindgebruiker Self-service](active-directory-accessmanagement-self-service-group-management.md)

* [Synchroniseren van Azure Azure AD verbinding maken met een groep van gebouwen](active-directory-aadconnect.md)

* [Eigenaren van een groep beheren](active-directory-accessmanagement-managing-group-owners.md)


## <a name="next-steps-for-access-management"></a>Vervolgstappen voor toegangsbeheer
Nu u de basisbeginselen van het toegangsbeheer hebben begrepen, hier bent sommige meer geavanceerde mogelijkheden beschikbaar in Azure Active Directory voor het beheren van uw toepassingen en bronnen.

* [Geavanceerde regels maken met behulp van kenmerken](active-directory-accessmanagement-groups-with-advanced-rules.md)

* [Beveiligingsgroepen beheren in Azure AD](active-directory-accessmanagement-manage-groups.md)

* [Speciale groepen in AD Azure instellen](active-directory-accessmanagement-dedicated-groups.md)

* [Graph API reference voor groepen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#GroupFunctions)

* [Azure Active Directory cmdlets voor het configureren van de instellingen](active-directory-accessmanagement-groups-settings-cmdlets.md)
