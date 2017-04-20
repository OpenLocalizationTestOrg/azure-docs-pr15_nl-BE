<properties
    pageTitle="Azure Active Directory de registratie overzicht | Microsoft Azure"
    description="is de basis voor scenario's voor voorwaardelijke toegang op basis van het apparaat. Wanneer een apparaat wordt geregistreerd, bepalingen Azure Active Directory de registratie in het apparaat met een identiteit die wordt gebruikt voor het verifiëren van het apparaat wanneer de gebruiker zich aanmeldt."
    services="active-directory"
    keywords="de registratie, de registratie inschakelen, de registratie en MDM"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/27/2016"
    ms.author="Markvi"/>

# <a name="get-started-with-azure-active-directory-device-registration"></a>Aan de slag met Azure Active Directory de registratie

Azure Active Directory de registratie is de basis voor scenario's voor voorwaardelijke toegang op basis van het apparaat. Wanneer een apparaat wordt geregistreerd, vindt Azure Active Directory de registratie u het apparaat met een identiteit die wordt gebruikt voor het verifiëren van het apparaat wanneer de gebruiker zich aanmeldt. De geverifieerde apparaat en de kenmerken van het apparaat, kunnen vervolgens gebruikt om af te dwingen van voorwaardelijke-beleidsregels voor toepassingen die worden gehost in de wolk en in ruimten.

In combinatie met een mobiel apparaat management(MDM) oplossing zoals de Microsoft Intune, worden de kenmerken van het apparaat in Azure Active Directory bijgewerkt met aanvullende informatie over het apparaat. Hiermee kunt u voor het maken van regels voor voorwaardelijke toegang die toegang van apparaten om te voldoen aan de normen voor beveiliging en naleving afdwingen. Zie voor meer informatie over het inschrijven van apparaten in Microsoft Intune [apparaten inschrijven voor beheer in Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune).

## <a name="scenarios-enabled-by-azure-active-directory-device-registration"></a>Scenario's die zijn ingeschakeld door Azure Active Directory de registratie

Azure Active Directory de registratie bevat ondersteuning voor iOS, Android en Windows apparaten. De afzonderlijke scenario's die gebruikmaken van registratie Azure AD apparaat wellicht meer specifieke eisen en ondersteund platform. Deze scenario's zijn als volgt:

- **Voorwaardelijke toegang tot toepassingen die worden gehost op ruimten**: kunt u geregistreerde apparaten met-beleidsregels voor toepassingen die zijn geconfigureerd voor het gebruik van AD FS met Windows Server 2012 R2. Zie voor meer informatie over het instellen van voorwaardelijke toegang op ruimten [instellen van On-premises voorwaardelijke toegang met behulp van Azure Active Directory de registratie](active-directory-conditional-access-on-premises-setup.md).

- **Voorwaardelijke toegang voor Office 365-toepassingen met Microsoft Intune** : IT-beheerders apparaatbeleid voorwaardelijke toegang wilt beveiligen, bedrijfsnetwerken, terwijl tegelijkertijd waardoor de informatiewerkers op compatibele apparaten toegang krijgen tot de services kunt inrichten. Zie [Voorwaardelijke-apparaatbeleid voor Office 365-services](active-directory-conditional-access-device-policies.md)voor meer informatie.

##<a name="setting-up-azure-active-directory-device-registration"></a>Azure Active Directory de registratie instellen

U moet registratie in de Portal Azure Azure AD apparaat inschakelen, zodat mobiele apparaten de service vinden kunnen door te zoeken naar bekende DNS-records. U moet uw bedrijf DNS zodanig configureren dat Windows 10, Windows 8.1, Windows 7, Android en iOS-apparaten kunnen ontdekken en gebruiken van de service.
U kunt bekijken en uitschakelen geregistreerde apparaten met behulp van de Portal-beheerder in Azure Active Directory.

>[AZURE.NOTE]
 Nieuwste Zie voor instructies voor het instellen van automatische registratie, [instellen van automatische inschrijving van een Windows-domein verbonden apparaten met Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).

### <a name="enable-azure-active-directory-device-registration-service"></a>Azure Active Directory apparaat registratie-Service inschakelen

1. Aanmelden bij de Microsoft Azure portal als beheerder.
2. Selecteer **Active Directory**in het linkerdeelvenster.
3. Selecteer op het tabblad **map** de map.
4. Selecteer in het tabblad **configureren** .
5. Ga naar de sectie **apparaten**.
6. Selecteer **alle** voor **gebruikers WERKPLEK JOIN apparaten mogelijk**.
7. Selecteer het maximum aantal apparaten die u wilt autoriseren per gebruiker.

>[AZURE.NOTE]
>Inschrijving met Microsoft Intune of Mobile Device Management voor Office 365 moet lid worden van werkplek. Als u een van deze services hebt geconfigureerd, worden alle wordt geselecteerd en de knop is uitgeschakeld.

Verificatie met twee factoren is standaard niet ingeschakeld voor de service. Verificatie met twee factoren wordt echter aangeraden bij het registreren van een apparaat.

- Verificatie met twee factoren worden voor deze service, moet u een provider verificatie met twee factoren in Azure Active Directory configureren en de gebruikersaccounts configureren voor meerledige verificatie, Zie [Meerledige verificatie naar Azure Active Directory toe te voegen](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)

- Als u AD FS met Windows Server 2012 R2, moet u een module voor verificatie met twee factoren in AD FS configureren, Zie [Meerledige verificatie met behulp van Active Directory Federation Services](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="configure-azure-active-directory-device-registration-discovery"></a>Azure Active Directory de registratie-detectie configureren
Windows 7 en Windows 8.1-apparaten ontdekt de registratieservice via het apparaat door de combinatie van de naam van de gebruikersaccount met de naam van een bekende registratie server.

U moet een CNAME DNS-record die naar de A-record die is gekoppeld aan uw registratieservice Azure Active Directory apparaat verwijst maken. De CNAME-record moet de bekende voorvoegsel enterpriseregistration gevolgd door het UPN-achtervoegsel dat is gebruikt door de gebruikersaccounts in uw organisatie gebruiken. Als uw organisatie gebruikmaakt van meerdere UPN-achtervoegsels, moeten meerdere CNAME-records in DNS worden gemaakt.

Bijvoorbeeld twee UPN-achtervoegsels in uw organisatie met de naam @contoso.com en @region.contoso.com, maakt u de volgende DNS-records.

| Post                                     | Type  | Adres                            |
|-------------------------------------------|-------|------------------------------------|
| enterpriseregistration.contoso.com        | CNAME | enterpriseregistration.Windows.NET |
| enterpriseregistration.Region.contoso.com | CNAME | enterpriseregistration.Windows.NET |

## <a name="view-and-manage-device-objects-in-azure-active-directory"></a>Weergeven en beheren van apparaatobjecten in Active Directory Azure
1. Vanaf de portal-beheerder Azure kunt u weergeven, blokkeren en deblokkeren van apparaten. Een apparaat dat is geblokkeerd hebben niet langer toegang tot toepassingen die zijn geconfigureerd als u wilt dat alleen geregistreerde apparaten.
2. Aanmelden als beheerder van de Portal voor Microsoft Azure.
3. Selecteer **Active Directory**in het linkerdeelvenster.
4. Selecteer de map.
5. Selecteer het tabblad **gebruikers** . Selecteer een gebruiker hun apparaten weergeven
6. Selecteer het tabblad **apparaten** .
7. **Geregistreerde apparaten** selecteert in de vervolgkeuzelijst.
8. Hier kunt u weergeven, blokkeren of deblokkeren geregistreerde gebruikers apparaten.

## <a name="additional-topics"></a>Extra onderwerpen

Met de registratie van Azure AD-apparaat kunt u uw Windows 7 en Windows 8.1 domein verbonden apparaten registreren. De volgende onderwerpen vindt u meer informatie over de vereisten en de stappen die zijn vereist voor het configureren van de registratie op Windows 7 en Windows 8.1-apparaten.

- [Automatische registratie met Azure Active Directory voor Windows-apparaten een domein behoren](active-directory-conditional-access-automatic-device-registration.md)
- [Automatische registratie voor Windows 7-domein gekoppeld apparaten configureren](active-directory-conditional-access-automatic-device-registration-windows7.md)
- [Automatische registratie voor Windows 8.1 domein gekoppeld apparaten configureren](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)
- [Automatische registratie met een domein behoren Azure Active Directory voor Windows 10-apparaten](active-directory-azureadjoin-devices-group-policy.md)
