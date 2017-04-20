<Properties
    pageTitle="Azure Active Directory voorwaardelijke toegang | Microsoft Azure"  
    description="Voorwaardelijke toegangsbeheer in Azure Active Directory gebruiken om te controleren op bepaalde voorwaarden bij de verificatie voor toegang tot toepassingen."  
    services="active-directory"
    keywords="voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsbronnen, voorwaardelijke-beleid"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/21/2016"
    ms.author="markvi"/>


# <a name="conditional-access-in-azure-active-directory"></a>Voorwaardelijke toegang in Azure Active Directory   

De mogelijkheden van het besturingselement in Azure Active Directory (AD Azure) voorwaardelijke toegang bieden eenvoudige manieren om te helpen de beveiligde bronnen in de cloud en op gebouwen. Voorwaardelijke-beleid zoals meerledige verificatie kunt beschermen tegen het risico van diefstal en phished referenties. Ander beleid van voorwaardelijke toegang kunnen beschermen van uw organisatie. Bijvoorbeeld, naast de referenties moeten wellicht een beleid dat alleen apparaten die zijn ingeschreven in een mobiel apparaat management system, zoals Microsoft Intune toegang tot gevoelige services van uw organisatie.


## <a name="prerequisites"></a>Vereisten

Azure AD voorwaardelijke toegang is een functie van [Azure Active Directory](http://www.microsoft.com/identity). Elke gebruiker die toegang heeft tot een toepassing met voorwaardelijke-beleid is toegepast, moet een Azure AD Premium-licentie hebben. U kunt meer informatie over licentievereisten in het [rapport met niet-gelicentieerde gebruiker](https://aka.ms/utc5ix).


## <a name="how-is-conditional-access-control-enforced"></a>Hoe wordt de voorwaardelijke toegang tot afgedwongen?  

Met voorwaardelijke toegangsbeheer op plaats Azure AD gecontroleerd op de specifieke criteria instellen voor een gebruiker toegang tot een toepassing. Nadat de toegangsvereisten wordt voldaan, wordt de gebruiker geverifieerd en toegang tot de toepassing.  

![Voorwaardelijke toegang-overzicht](./media/active-directory-conditional-access/conditionalaccess-overview.png)

## <a name="conditions"></a>Voorwaarden

Dit zijn de voorwaarden die u in een beleid van voorwaardelijke toegang opnemen kunt:

- **Lidmaatschap van de groep**. De toegang van de gebruiker op basis van lidmaatschap van een groep.

- **Locatie**. De locatie van de gebruiker om de trigger meerledige verificatie te gebruiken en blok besturingselementen gebruiken als een gebruiker zich niet in een vertrouwd netwerk.

- **Apparaat-platform**. Gebruik het apparaatplatform, zoals iOS, Android, Windows Mobile of Windows, als voorwaarde voor het toepassen van beleid.

- Het **apparaat is ingeschakeld**. Status van het apparaat, wordt ingeschakeld of uitgeschakeld, gevalideerd tijdens de evaluatie van beleid apparaat. Als u een verloren of gestolen apparaat in de directory uitschakelt, het niet langer voldoen aan de vereisten.

- **Gebruiker- en risico's**. U kunt [Azure AD identiteit bescherming](active-directory-identityprotection.md) voor voorwaardelijke toegang risico beleid. Voorwaardelijke-risico beleid helpen beschermen uw organisatie vooraf op basis van risico's en ongebruikelijke activiteiten aanmelden.


## <a name="controls"></a>Besturingselementen

Dit zijn besturingselementen waarmee u kunt een voorwaardelijke-beleid afdwingen:

- **Meerledige verificatie**. U kunt geavanceerde verificatie via meerledige verificatie vereisen. U kunt een meerledige verificatie gebruiken met Azure meerledige verificatie of via een provider op ruimten meerledige verificatie, in combinatie met Active Directory Federation Services (AD FS). Meerledige verificatie voorkomt bronnen wordt benaderd door een onbevoegde gebruiker die toegang tot de referenties van een gebruiker geldig kan zijn.

- **Blok**. U kunt voorwaarden als locatie voor het blokkeren van toegang voor gebruikers toepassen. U kunt bijvoorbeeld toegang blokkeren als een gebruiker zich niet in een vertrouwd netwerk.

- **Compatibele apparaten**. U kunt voorwaardelijke-beleid instellen op het apparaatniveau van het. U kunt een beleid instellen zodat alleen computers die deel uitmaakt van een domein of mobiele apparaten die zijn ingeschreven in een toepassing voor mobiel apparaat, toegang krijgen de bronnen van uw organisatie tot. U kunt bijvoorbeeld Intune nagegaan apparaat gebruiken en wanneer de gebruiker probeert toegang te krijgen tot een toepassing rapporteren naar Azure AD voor controledoeleinden. Zie voor gedetailleerde richtlijnen over het gebruik van Intune apps en gegevens beveiligen [Beveilig apps en gegevens met Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/protect-apps-and-data-with-microsoft-intune). Ook kunt u Intune gebruikt om gegevensbescherming voor verloren of gestolen apparaten. Zie voor meer informatie [beschermen uw gegevens met volledig of selectief wissen met behulp van Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune).

## <a name="applications"></a>Toepassingen

U kunt een voorwaardelijke-beleid op het niveau van afdwingen. Toegangsniveaus instellen voor toepassingen en services in de cloud of op locatie. Het beleid wordt toegepast op direct naar de website of service. Het beleid wordt afgedwongen voor toegang tot de browser, en toepassingen die toegang hebben tot de service.


## <a name="device-based-conditional-access"></a>Voorwaardelijke toegang op basis van het apparaat

U kunt de toegang beperken tot toepassingen van apparaten die met Azure Active Directory zijn geregistreerd en die aan bepaalde voorwaarden voldoen. Voorwaardelijke toegang op basis van het apparaat voorkomt gebruikers die proberen toegang te krijgen tot de bronnen van de resources van een onderneming:

- Onbekende of niet-beheerde apparaten.
- Apparaten die niet voldoen aan het beveiligingsbeleid uw organisatie is ingesteld.

Beleid op basis van deze vereisten, kunt u instellen:

- **Apparaten die deel uitmaakt van een domein**. Stel een beleid voor toegang tot apparaten die lid zijn van een Active Directory-domein op gebouwen en die ook zijn geregistreerd met Azure Active Directory te beperken. Dit beleid is van toepassing op Windows-desktops, laptops en tablets van de onderneming.
Voor meer informatie over het instellen van automatische inschrijving van apparaten met Azure Active Directory domein behoren, Zie [Automatische registratie van Windows-apparaten met Azure Active Directory domein behoren](active-directory-conditional-access-automatic-device-registration-setup.md).

- **Compatibele apparaten**. Een beleid om toegang te beperken tot apparaten die zijn gemarkeerd als **compatibel** in de directory management-systeem instellen. Dit beleid zorgt u ervoor dat alleen de apparaten die voldoen aan beleid zoals het afdwingen van bestanden coderen op een apparaat toegang hebben. U kunt dit beleid gebruiken om toegang te beperken van de volgende apparaten:

    - **Windows-apparaten een domein behoren**. Beheerd door System Center Configuration Manager (in de huidige structuur) in een hybride configuratie wordt geïmplementeerd.
    - **Windows 10 Mobile werk of persoonlijke apparaten**. Beheerd door Intune of door het beheersysteem ondersteunde mobiele apparaten van andere leveranciers.
    - **iOS- en Android-apparaten**. Beheerd door Intune.


Gebruikers die toegang hebben tot toepassingen die worden beveiligd door een apparaat op basis van beleid moet toegang tot de toepassing van een apparaat dat voldoet aan de vereisten van dit beleid. Toegang is geweigerd als geprobeerd op een apparaat dat niet voldoet aan de vereisten.

Zie voor informatie over het configureren van een beleid op basis van apparaat in Azure AD, [voorwaardelijke toegang op basis van het apparaat het beleid instellen voor Azure Active Directory - toepassingen](active-directory-conditional-access-policy-connected-applications.md).

## <a name="resources"></a>Bronnen

Zie de volgende categorieën van resources en artikelen voor meer informatie over het instellen van voorwaardelijke toegang voor uw organisatie.


### <a name="multi-factor-authentication-and-location-policies"></a>Meerledige verificatie- en locatie-beleid

- [Aan de slag met de voorwaardelijke toegang tot Azure AD verbonden apps op basis van de groep, locatie en beleid meerledige verificatie](active-directory-conditional-access-azuread-connected-apps.md)
- [Toepassingen die worden ondersteund](active-directory-conditional-access-supported-apps.md)


### <a name="device-based-conditional-access"></a>Voorwaardelijke toegang op basis van het apparaat

- [Beleid voor voorwaardelijke toegang op basis van het apparaat voor toegangsbeheer op Azure Active Directory-toepassingen instellen](active-directory-conditional-access-policy-connected-applications.md)
- [Automatische registratie van Windows instellen met apparaten met Azure Active Directory domein behoren](active-directory-conditional-access-automatic-device-registration-setup.md)
- [Het oplossen van problemen met Azure Active Directory toegang](active-directory-conditional-access-device-remediation.md)
- [Beveiligen van gegevens op zoekgeraakte of gestolen apparaten met behulp van Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune)


### <a name="protect-resources-based-on-sign-in-risk"></a>Resources op basis van-in gevaar te beschermen

-   [Azure AD identiteit bescherming](active-directory-identityprotection.md)

### <a name="next-steps"></a>Volgende stappen

- [Veelgestelde vragen voor voorwaardelijke toegang](active-directory-conditional-faqs.md)
- [Technische naslaginformatie](active-directory-conditional-access-technical-reference.md)
