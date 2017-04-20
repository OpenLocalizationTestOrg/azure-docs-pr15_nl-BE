<properties
  pageTitle="Beheren van toegang tot toepassingen met behulp van Azure AD |  Microsoft Azure"
  description="Hierin wordt beschreven hoe Azure Active Directory kunnen organisaties geven de apps waartoe elke gebruiker toegang heeft."
  services="active-directory"
  documentationCenter=""
  authors="femila"
  manager="femila"
  editor=""/>

 <tags
  ms.service="active-directory"
  ms.workload="identity"
  ms.tgt_pltfrm="na"
  ms.devlang="na"
  ms.topic="article"
  ms.date="10/13/2016"
  ms.author="femila"/>


# <a name="managing-access-to-apps"></a>Toegang tot toepassingen beheren

Toegangsbeheer voor continue, gebruik, evaluatie en rapportage blijven een uitdaging nadat een app is geïntegreerd in uw organisatie de identiteit system. In veel gevallen hebben IT-beheerders of helpdesk te nemen een voortdurende actieve rol bij het beheren van uw toepassingen. Toewijzing wordt soms uitgevoerd door een algemene of divisie IT-team. Vaak, het besluit van de toewijzing is bedoeld om te worden overgedragen aan de besluitvormer vereisen van hun goedkeuring voordat IT heeft de toewijzing.  Andere organisaties investeren in integratie met een bestaande geautomatiseerde identiteits- en toegangsbeheer management system, zoals Role-Based Access Control RBAC () of op basis van het kenmerk toegangsbeheer (ABAC). De integratie en de ontwikkeling van de regel zijn meestal gespecialiseerde en dure. Controleren of het melden van een managementbenadering is een eigen aparte, complexe en dure investering.

## <a name="how-does-azure-active-directory-help"></a>Hoe beschermt Azure Active Directory

 Azure AD ondersteunt uitgebreide toegangsbeheer voor geconfigureerde toepassingen, waardoor organisaties eenvoudig de juiste toegangsbeleid variërend van automatische, op basis van het kenmerk toewijzing (ABAC of RBAC scenario's) via de delegatie en met het beheer door de systeembeheerder. Azure AD u kunt gemakkelijk complexe beleid bereiken combineren van meerdere modellen voor één toepassing en kan zelfs hergebruik regels voor alle toepassingen met de dezelfde doelgroepen.

 - [Toevoegen van nieuwe of bestaande toepassingen](active-directory-sso-integrate-saas-apps.md)


 Toewijzing van Azure AD toepassing ligt de nadruk op twee modi van de primaire toewijzing:

- **Afzonderlijke toewijzing** Een IT-beheerder met een globale directory-beheerdersrechten kunt u afzonderlijke gebruikersaccounts te selecteren en ze toegang verlenen tot de toepassing.
- **Toewijzing op basis van een groep (Azure AD alleen betaald)** Een IT-beheerder met een globale directory-beheerdersrechten kunt u een groep toewijzen aan de toepassing. Bepaalde gebruikers toegang wordt bepaald door de vraag of ze lid van de groep op het moment dat ze proberen zijn te krijgen tot de toepassing. Een beheerder kan met andere woorden, een toewijzingsregel met vermelding van "alle leden van de toegewezen groep toegang heeft tot de toepassing" effectief maken. Met deze optie toewijzing kunnen beheerders profiteren van Azure AD groep Beheer opties, met inbegrip van [dynamische groepen op basis van het kenmerk](active-directory-accessmanagement-manage-groups.md), extern systeemgroepen (bijvoorbeeld Active Directory op de bedrijfsruimten of werkdag) of beheerder wordt beheerd of zelfstandige-verkeer worden beheerd. Een groep kan worden gemakkelijk toegewezen aan meerdere apps, ervoor te zorgen dat toepassingen met affiniteit toewijzing toewijzingsregels, kunnen delen minder complex algemeen beheer. Houd er rekening mee dat geneste groep lidmaatschappen worden niet ondersteund voor de toewijzing op basis van een groep met toepassingen op dit moment.

Met deze toewijzing van twee modi, kunnen beheerders een managementbenadering wenselijk toewijzing bereiken.

Met Azure AD, is gebruik en de rapportage volledig geïntegreerd, waardoor beheerders gemakkelijk rapporteren over de status toewijzing toewijzingsfouten en zelfs gebruik.

## <a name="complex-application-assignment-with-azure-ad"></a>Toewijzing van complexe toepassingen met Azure Active Directory

U kunt een toepassing als televergaderingen. In veel organisaties wordt televergaderingen voornamelijk gebruikt door de marketing- en organisaties. Vaak voordelige leden van het marketingteam zeer toegang tot televergaderingen, terwijl de leden van het verkoopteam beperkte toegang hebben. In veel gevallen een brede populatie van IT-medewerkers toegang hebt beperkt tot de toepassing. Uitzonderingen op deze regels aangelegenheden bemoeilijken. Het is vaak het prerogatief van de teams van de leiderschap marketing- of een gebruikerstoegang te verlenen of hun functie onafhankelijk van deze algemene regels wijzigen.

Azure AD, toepassingen, zoals televergaderingen worden vooraf geconfigureerd voor eenmalige aanmelding (SSO) en geautomatiseerde provisioning. Zodra de toepassing is geconfigureerd, kan een beheerder de eenmalige actie maken en toewijzen van de juiste groepen te nemen. In dit voorbeeld, kan een beheerder de volgende toewijzingen uitvoeren:

- [Dynamische groepen](active-directory-accessmanagement-manage-groups.md) kunnen automatisch voor alle leden van de marketing- en teams met kenmerken zoals afdeling of rol worden gedefinieerd:

    - Alle leden van groepen marketing wordt toegewezen aan de rol 'marketing' in televergaderingen

    - Alle leden van het verkoopteam groepen wordt toegewezen aan de rol 'verkoop' in de televergaderingen. Een verdere verfijning kan meerdere groepen die overeenkomen met regionale verkoopteams die zijn toegewezen aan de verschillende rollen van de televergaderingen gebruiken.

- Als u wilt dat het mechanisme van de uitzondering, kan een groep zelf worden gemaakt voor elke rol. De groep 'Televergaderingen marketing uitzondering' kan bijvoorbeeld worden gemaakt als een groep zelf. De groep kan worden toegewezen aan de rol van televergaderingen marketing en het marketingteam leiderschap eigenaars kan worden gemaakt. Leden van het marketingteam leiderschap kunnen toevoegen of verwijderen gebruikers, een join-beleid instellen of zelfs goedkeuren of weigeren van aanvragen voor deelname aan afzonderlijke gebruikers. Dit wordt ondersteund door een informatie werknemer passende ervaring die geen speciale training voor eigenaren of leden vereist.

In dit geval zou alle toegewezen gebruikers worden automatisch ingericht op televergaderingen, als ze worden toegevoegd aan andere groepen die hun roltoewijzing zou worden bijgewerkt in de televergaderingen. Gebruikers zou kunnen ontdekken en toegang tot televergaderingen via de toepassing toegang tot Configuratiescherm van Microsoft Office web-clients, of zelfs door te gaan naar de aanmeldingspagina van hun organisatie televergaderingen. Beheerders zou kunnen eenvoudig bekijken en toewijzing de status met Azure AD rapportage zijn.

Beheerders kunnen gebruikmaken van [voorwaardelijke toegang Azure AD](active-directory-conditional-access.md) -beleid voor specifieke rollen instellen. Deze beleidsregels zijn of de toegang tot buiten de bedrijfsomgeving en zelfs meerledige verificatie of apparaat eisen te verwezenlijken in diverse gevallen toegang hebben.

## <a name="how-can-i-get-started"></a>Hoe kan ik beginnen?

Ten eerste, als u al Azure AD niet gebruikt en u een IT-beheerder bent:

 - [Probeer het zelf!](https://azure.microsoft.com/trial/get-started-active-directory/) -u kunt zich aanmelden voor een gratis proefperiode van 30 dagen vandaag en uw eerste cloud-oplossing implementeren onder 5 minuten met behulp van deze koppeling

Azure zijn AD waarmee account delen:

- [Toewijzingen groeperen](active-directory-accessmanagement-self-service-group-management.md)
- Toepassingen toevoegen aan AD Azure
- Aan de slag met toewijzing
- Toewijzing van toepassingen Veelgestelde vragen
- [App dashboard/rapporten](active-directory-passwords-get-insights.md)

## <a name="where-can-i-learn-more"></a>Waar vind ik meer informatie?

- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Bescherming van de apps met voorwaardelijke toegang](active-directory-conditional-access.md)
- [Self-service groep management/SSAA](active-directory-accessmanagement-self-service-group-management.md)
