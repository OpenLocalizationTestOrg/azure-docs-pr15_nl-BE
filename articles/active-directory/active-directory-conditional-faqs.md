<properties
    pageTitle="Voorwaardelijke toegang tot Active Directory Azure Veelgestelde vragen | Microsoft Azure"
    description="Veelgestelde vragen over voorwaardelijke toegang "
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-conditional-access-faq"></a>Voorwaardelijke toegang tot Active Directory Azure Veelgestelde vragen

## <a name="which-applications-work-with-conditional-access-policies"></a>Welke toepassingen werken met beleid voor voorwaardelijke toegang?

**A:** Zie het onderwerp, [voorwaardelijke toegang, welke toepassingen worden ondersteund](active-directory-conditional-access-supported-apps.md).

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>Voorwaardelijke-beleid afgedwongen voor B2B-samenwerking en gastgebruikers?

**A:** Beleid worden voor gebruikers van B2B-samenwerking afgedwongen. Echter in sommige gevallen kan een gebruiker niet mogelijk om te voldoen aan de voorwaarde als bijvoorbeeld een organisatie meerledige verificatie niet ondersteunen. 

Het beleid is niet geforceerd voor gebruikers van SharePoint gasten. De Gast-relatie behouden in SharePoint. Gastgebruikers accounts zijn niet onderworpen aan access beleid op de verificatieserver. Toegang voor gasten kan worden beheerd in SharePoint.

## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a>Is een beleid voor SharePoint Online ook toepassing OneDrive voor bedrijven?

**A:** Ja.
 
## <a name="why-cant-i-set-a-policy-on-client-apps-like-word-or-outlook"></a>Waarom kan ik een beleid niet instellen op clienttoepassingen zoals Word of Outlook?

**A:** Een beleid van voorwaardelijke toegang vereisten voor toegang tot een service wordt ingesteld en wordt toegepast wanneer de verificatie gebeurt er met die service. Het beleid is niet ingesteld op een clienttoepassing; in plaats daarvan wordt toegepast wanneer het in een service aanroept. Bijvoorbeeld een beleid instellen voor SharePoint is van toepassing op SharePoint aanroepen van clients en een beleid instellen voor Exchange is van toepassing op Outlook.


## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a>Geldt een beleid van voorwaardelijke toegang voor serviceaccounts?

**A:** Voorwaardelijke toegangsbeleid geldt voor alle gebruikersaccounts. Dit geldt ook voor gebruikersaccounts die worden gebruikt als serviceaccounts. In veel gevallen kan een service-account die wordt uitgevoerd zonder toezicht niet voldoen aan een beleid. Dit is bijvoorbeeld het geval wanneer de MVR gesloten is vereist. In deze gevallen kunnen een beleid, met instellingen voor voorwaardelijke toegang beheer serviceaccounts worden uitgesloten. Meer informatie over het toepassen van een beleid voor gebruikers hier.
