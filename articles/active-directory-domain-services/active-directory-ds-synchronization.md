<properties
    pageTitle="Azure Active Directory Domain Services: Synchronisatie in beheerde domeinen | Microsoft Azure"
    description="Synchronisatie in een beheerde Azure Active Directory Domain Services-domein begrijpen"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="synchronization-in-an-azure-ad-domain-services-managed-domain"></a>Synchronisatie in een beheerde Azure AD Domain Services-domein
In het volgende diagram ziet u hoe synchronisatie werkt in Azure AD Domain Services beheerde domeinen.

![Synchronisatie-topologie in Azure AD Domain Services](./media/active-directory-domain-services-design-guide/sync-topology.png)

## <a name="synchronization-from-your-on-premises-directory-to-your-azure-ad-tenant"></a>Synchronisatie tussen uw directory op de gebouwen en de huurder Azure AD
Azure AD verbinden synchronisatie wordt gebruikt voor het synchroniseren van gebruikersaccounts, groepslidmaatschappen en referentie-hashes aan de huurder Azure AD. Kenmerken van de gebruiker bijvoorbeeld de UPN-accounts en SID (security identifier) worden gesynchroniseerd op gebouwen. Als u een AD-domein Azure Services gebruikt, worden ook oudere referentie-hashes voor NTLM en Kerberos-verificatie vereist aan de huurder Azure AD gesynchroniseerd.

Als u terugschrijven configureert, worden veranderingen die zich in de map Azure AD gesynchroniseerd terug naar Active Directory op gebouwen. Bijvoorbeeld, als u uw wachtwoord met functies voor Azure AD zelf wachtwoord wijzigen wijzigen, het gewijzigde wachtwoord wordt bijgewerkt in uw locatie op AD-domein.

> [AZURE.NOTE] Gebruik altijd de nieuwste versie van Azure AD verbinden zodat u correcties voor alle bekende fouten.


## <a name="synchronization-from-your-azure-ad-tenant-to-your-managed-domain"></a>Synchronisatie tussen uw huurder Azure AD en uw beheerde domein
Gebruikersaccounts, groepslidmaatschappen en referentie-hashes worden gesynchroniseerd vanuit de huurder Azure AD aan uw beheerde Azure AD Domain Services-domein. Het synchronisatieproces is automatisch. U hoeft niet te configureren, controleren of het synchronisatieproces beheren. Tijdens de synchronisatie ook één-way/unidirectioneel is in de natuur. Uw beheerde domein grotendeels alleen-lezen is, met uitzondering van een aangepaste organisatie-eenheden u wilt maken. Daarom kan niet u wijzigingen aanbrengen in de gebruikerskenmerken, wachtwoorden van gebruikers of groepslidmaatschap binnen het domein beheerd. Hierdoor is er geen reverse synchronisatie van veranderingen van uw beheerde domein terug naar uw huurder Azure AD.


## <a name="synchronization-from-a-multi-forest-on-premises-environment"></a>Synchronisatie van een omgeving met meerdere forests op ruimten
Veel hebben organisaties een vrij complexe op ruimten identiteit infrastructuur die bestaat uit meerdere forests van het account. Azure AD Connect ondersteunt het synchroniseren van gebruikers, groepen en referentie-hashes aan de huurder Azure AD in omgevingen met meerdere forests.

De huurder Azure AD is daarentegen een veel eenvoudiger en flat-naamruimte. Als u wilt dat gebruikers op betrouwbare wijze toegang krijgen tot toepassingen die worden beveiligd door AD Azure, UPN-conflicten worden opgelost tussen gebruikersaccounts in verschillende forests. Het AD-Domain Azure Services beheerde domein bears sluit waardoor de gelijkenis met de huurder Azure AD. Daarom ziet u een platte eenheidstructuur van organisatie-in uw domein beheerd. Alle gebruikers en groepen worden in de container 'AADDC Users' op locatie, domein of forest waaruit ze zijn gesynchroniseerd in opgeslagen. U hebt een hiërarchische organisatie-eenheid geconfigureerd structuur op gebouwen. Uw beheerde domein heeft echter nog steeds een eenvoudige platte eenheidstructuur van organisatie.


## <a name="exclusions---what-isnt-synchronized-to-your-managed-domain"></a>Uitsluitingen - wat niet is gesynchroniseerd met uw beheerde domein
De volgende objecten of -kenmerken worden niet gesynchroniseerd naar uw huurder Azure AD of uw beheerde domein:

- **Uitgesloten kenmerken:** U kunt kiezen uit te sluiten van bepaalde kenmerken van synchroniseren met uw huurder Azure AD uit uw domein op ruimten is Azure AD verbinden met. Deze uitgesloten kenmerken zijn niet beschikbaar in uw domein beheerd.

- **Groep beleid:** Groepsbeleid is geconfigureerd in uw domein op ruimten worden niet gesynchroniseerd naar uw domein beheerd.

- **SYSVOL-share:** Ook worden de inhoud van de SYSVOL-share op uw domein op ruimten niet gesynchroniseerd naar uw domein beheerd.

- **Computerobjecten:** Computerobjecten voor computers die deel uitmaken van uw domein op ruimten worden niet gesynchroniseerd naar uw domein beheerd. Deze computers niet hebben een vertrouwensrelatie met het domein beheerd en deel uitmaken van uw domein op gebouwen alleen. Zoek in uw beheerde domein computerobjecten alleen bestemd voor computers die u expliciet domein tot het domein beheerd behoren hebt.

- **Kenmerken van de SID-geschiedenis voor de gebruikers en groepen:** De primaire gebruiker en de SID's van uw domein op de locatie van de primaire groep worden gesynchroniseerd met uw beheerde domein. Bestaande kenmerken van de SID-geschiedenis voor de gebruikers en groepen worden echter niet gesynchroniseerd van uw domein op ruimten aan uw domein beheerd.

- **Structuren van organisatie-eenheden (OU):** Organisatie-eenheden die zijn gedefinieerd in uw domein op ruimten worden niet gesynchroniseerd naar uw domein beheerd. Er zijn twee ingebouwde organisatie-eenheden in uw domein beheerd. Uw beheerde domein heeft een platte eenheidstructuur van organisatie. U kunt echter [een aangepaste organisatie-eenheid in uw beheerde domein maken](./active-directory-ds-admin-guide-create-ou.md).


## <a name="how-specific-attributes-are-synchronized-to-your-managed-domain"></a>Hoe specifieke kenmerken worden gesynchroniseerd met uw beheerde domein
De volgende tabel geeft een overzicht van enkele algemene kenmerken en wordt beschreven hoe deze worden gesynchroniseerd naar uw domein beheerd.

| In uw domein beheerd kenmerk | Bron | Notities |
|:---|:---|:---|
|UPN|UPN-kenmerk van de gebruiker in uw huurder Azure AD|Het UPN-kenmerk van de huurder Azure AD gesynchroniseerd naar uw beheerde domein is. Daarom is de meest betrouwbare manier aanmelden bij uw domein beheerd met behulp van de UPN.|
|SAMAccountName|Van gebruiker mailNickname kenmerk in uw huurder Azure AD of automatisch gegenereerd|Het kenmerk SAMAccountName is die afkomstig is van het kenmerk mailNickname in uw huurder Azure AD. Als u meerdere gebruikersaccounts hebt hetzelfde kenmerk mailNickname, is de SAMAccountName automatisch gegenereerd. Als de mailNickname van de gebruiker of het UPN-voorvoegsel meer dan 20 tekens is, is de SAMAccountName automatisch gegenereerd om te voldoen aan de limiet van 20 tekens op SAMAccountName kenmerken.|
|Wachtwoorden|Het wachtwoord van de gebruiker uit uw huurder Azure AD|Referentie-hashes die zijn vereist voor NTLM of Kerberos-verificatie (ook wel aanvullende referenties genoemd) worden gesynchroniseerd vanuit de huurder Azure AD. Als uw huurder Azure AD een gesynchroniseerde huurder is, deze referenties zijn die afkomstig is van uw domein op gebouwen.|
|De primaire gebruiker/groep SID|Automatisch gegenereerde|De primaire SID voor accounts van de gebruiker of groep wordt automatisch gegenereerd in uw domein beheerd. Dit kenmerk komt niet overeen met de primaire gebruiker of groep beveiligings-id van het object in uw bedrijf op AD-domein. Dit heeft namelijk het beheerde domein een andere beveiligings-id-naamruimte heeft dan uw domein op gebouwen.|
|SID-geschiedenis voor de gebruikers en groepen|De primaire gebruiker van lokalen en groep|Het kenmerk SidHistory voor gebruikers en groepen in uw beheerde domein is ingesteld op overeenkomen met de bijbehorende primaire gebruiker of groep in uw domein op gebouwen. Met deze functie kunt gemakkelijker lift-en-verschuiving van toepassingen voor ruimten met het domein beheerd, aangezien u niet te re-ACL-bronnen hoeft.|

> [AZURE.NOTE] **Aanmelden bij de beheerde domein met de UPN-notatie:** Het kenmerk SAMAccountName mogelijk automatisch gegenereerd voor sommige gebruikersaccounts in uw domein beheerd. Als meerdere gebruikers hetzelfde kenmerk mailNickname of gebruikers overmatig lange UPN-voorvoegsels hebben, kan de SAMAccountName voor deze gebruikers worden automatisch gegenereerd. Daarom is de SAMAccountName notatie (bijvoorbeeld ' CONTOSO100\joeuser') niet altijd een betrouwbare manier aanmelden bij het domein. Gebruikers automatisch gegenereerde SAMAccountName kan verschillen van de UPN-voorvoegsel. De UPN-notatie (bijvoorbeeld, 'joeuser@contoso100.com') aan te melden bij het domein beheerd op betrouwbare wijze.


## <a name="objects-that-are-not-synchronized-to-your-azure-ad-tenant-from-your-managed-domain"></a>Objecten die niet worden gesynchroniseerd met uw huurder Azure AD uit uw domein beheerd
Zoals in een vorige sectie van dit artikel wordt beschreven, is er geen synchronisatie van uw beheerde domein terug naar uw huurder Azure AD. U kunt [een aangepaste organisatie-eenheid (OU) te maken](./active-directory-ds-admin-guide-create-ou.md) in uw domein beheerd. Bovendien kunt u andere organisatie-eenheden, gebruikers, groepen of serviceaccounts binnen deze aangepaste organisatie-eenheden. Geen van de objecten die zijn gemaakt in een aangepaste organisatie-eenheden worden gesynchroniseerd naar uw huurder Azure AD. Deze objecten zijn beschikbaar voor gebruik binnen uw domein beheerd. Deze objecten zijn daarom niet zichtbaar met Azure AD PowerShell-cmdlets, Azure AD Graph API of de Azure AD management UI.


## <a name="related-content"></a>Verwante inhoud
- [Functies - Azure AD Domain Services](active-directory-ds-features.md)

- [Implementatiescenario's - Azure AD Domain Services](active-directory-ds-scenarios.md)

- [Overwegingen voor het AD-Domain Azure Services toegang](active-directory-ds-networking.md)

- [Aan de slag met Azure AD Domain Services](active-directory-ds-getting-started.md)
