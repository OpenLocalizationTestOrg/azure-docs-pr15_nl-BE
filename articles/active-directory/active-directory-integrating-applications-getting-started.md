<properties
   pageTitle="Azure Active Directory integreren in toepassingen die aan de slag |  Microsoft Azure"
   description="Dit artikel is een handleiding voor het ophalen voor het integreren van Azure Active Directory (AD) met toepassingen in gebouwen en cloud-toepassingen."
   services="active-directory"
   documentationCenter=""
   authors="ihenkel"
   manager="femila"
   editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="02/09/2016"
      ms.author="inhenk"/>

# <a name="integrating-azure-active-directory-with-applications-getting-started-guide"></a>Azure Active Directory integreren in toepassingen die aan de slag
## <a name="overview"></a>Overzicht
Dit onderwerp is bedoeld om u te bieden een routekaart voor de integratie van toepassingen met Azure Active Directory (AD). Elk van de onderstaande secties bevatten een korte samenvatting van een meer gedetailleerde onderwerp, zodat u kunt bepalen welke delen van deze handleiding voor verkrijgen die relevant zijn voor u.  Volg de koppelingen voor een diepere kennismaking op elk onderwerp.

## <a name="before-you-begin-take-inventory"></a>Voordat u begint, inventariseren
Voordat u naar de integratie van toepassingen met Azure AD in gaan, is het belangrijk om te weten waar u bent en waar u naartoe wilt gaan.  De volgende vragen zijn bedoeld om te denken over uw project Azure AD application integration.

### <a name="application-inventory"></a>Inventarisatie van toepassingen
- Waar zijn alle toepassingen? Wie de eigenaar ervan?
- Welk type verificatie hebben uw toepassingen nodig?
- Wie toegang tot welke toepassingen nodig?
- Wilt u een nieuwe toepassing implementeren?
  - Zal u zelf bouwen en implementeren op een instantie Azure compute?
  - U gebruikt een die beschikbaar is in de galerie met Azure toepassing?

### <a name="user-and-group-inventory"></a>Gebruikers- en voorraad
- Waar de gebruikersaccounts zich bevinden?
 - On-premises Active Directory
 - Azure AD
 - In een afzonderlijke toepassing-database waarvan u eigenaar
 - Verzoek tot niet toegestande toepassingen
 - Alle bovenstaande
- Welke machtigingen en roltoewijzingen individuele gebruikers hebt? Moet u controleren hun toegang of weet u zeker dat uw gebruikers toegang en de rol van toewijzingen nu geschikt zijn?
- Groepen al gevestigd zijn in Active Directory in de lokalen?
 - Hoe zijn de groepen ingedeeld?
 - Wie zijn de leden van de groep?
 - Welke machtigingen/roltoewijzingen de groepen op dat moment hebt?
- Moet u voor het opschonen van de databases van de gebruiker of groep voor integratie?  (Dit is een heel belangrijke vraag. Garbage in, garbage out.)

### <a name="access-management-inventory"></a>Toegang beheer van voorraad
- Hoe kan u momenteel toegang tot toepassingen beheren? Die moet worden gewijzigd?  Hebt u rekening gehouden met andere manieren voor het beheren van toegang, zoals met [RBAC](role-based-access-control-configure.md) bijvoorbeeld?
- Wie toegang krijgt tot welke nodig?

Misschien hoeft u niet de antwoorden op al deze vragen vooraf maar dat is geen probleem.  Deze handleiding kunt u sommige van deze vragen te beantwoorden en sommige gefundeerde beslissingen te nemen.

## <a name="prerequisites"></a>Vereisten
- Een Azure-abonnement en een lijst met Azure Active Directory.  Als u niet al een Azure-abonnement hebt, kunt u Azure voor 30 dagen gratis uitproberen. [Probeer het zelf!](https://azure.microsoft.com/trial/get-started-active-directory/)

## <a name="application-integration-with-azure-ad"></a>Integratie met AD Azure
### <a name="finding-unsanctioned-cloud-applications-with-cloud-app-discovery"></a>Verzoek tot niet toegestande zoeken cloud-toepassingen met Cloud App Discovery
Zoals hierboven vermeld, worden toepassingen die door uw organisatie tot op heden nog niet zijn beheerd.  Als onderdeel van de voorraad is het mogelijk om verzoek tot niet toegestande cloud-toepassingen te vinden. Zie [toepassingen met Cloud App Discovery verzoek tot niet toegestande wolk zoeken](active-directory-cloudappdiscovery-whatis.md).

### <a name="authentication-types"></a>Verificatietypen
Elke toepassing mogelijk verschillende verificatievereisten. Met Azure Active Directory, kan certificaten ondertekenen worden gebruikt met toepassingen die gebruikmaken van SAML 2.0, WS-Federation of OpenID verbinding protocollen, alsmede wachtwoord eenmalige aanmelding. Voor meer informatie over de toepassing Zie verificatietypen voor gebruik met AD Azure [Certificaten beheren voor federatieve eenmalige aanmelding in Azure Active Directory](active-directory-sso-certs.md) en het [wachtwoord op eenmalige basis](active-directory-appssoaccess-whatis.md).

### <a name="enabling-sso-with-azure-ad-app-proxy"></a>Eenmalige aanmelding met Azure AD App-Proxy inschakelen
Met Microsoft Azure AD toepassingsproxy, kunt u bieden toegang tot toepassingen die zich in uw particuliere netwerk veilig, overal en op elk apparaat. Nadat u een toepassing proxy-connector hebt geïnstalleerd in uw omgeving, kan deze eenvoudig worden geconfigureerd met Azure AD.

### <a name="integrating-applications-with-azure-ad"></a>Integratie van toepassingen met Azure AD
De volgende artikelen Bespreek de verschillende manieren waarop toepassingen integreren met AD Azure en sommige richtsnoeren te verschaffen.

- [Om te bepalen welke Active Directory gebruiken](active-directory-administer.md)
- [Toepassingen gebruiken in de galerie met Azure toepassing](active-directory-appssoaccess-whatis.md)
- [Zelfstudies lijst van SaaS-toepassingen integreren](active-directory-saas-tutorial-list.md)

## <a name="managing-access-to-applications"></a>Toegang tot toepassingen beheren
De volgende artikelen beschreven kunt u toegang tot toepassingen beheren nadat ze zijn geïntegreerd met Azure AD met Azure AD Connectors en Azure AD manieren.

- [Beheren van toegang tot toepassingen met behulp van Azure AD](active-directory-managing-access-to-apps.md)
- [Automatiseren met Azure AD-Connectors](active-directory-saas-app-provisioning.md)
- [Gebruikers toewijzen aan een toepassing](active-directory-applications-guiding-developers-assigning-users.md)
- [Groepen toewijzen aan een toepassing](active-directory-applications-guiding-developers-assigning-groups.md)
- [Delen van accounts](active-directory-sharing-accounts.md)

## <a name="integrating-custom-applications"></a>Integratie van aangepaste toepassingen
Als u een nieuwe toepassing schrijft en ontwikkelaars helpen bij het gebruik van de kracht van Azure AD, Zie [Guiding ontwikkelaars](active-directory-applications-guiding-developers-for-lob-applications.md).

Als u wilt dat uw aangepaste toepassing toevoegen aan de galerie Azure toepassing, Zie ["uw eigen app brengen' met Azure AD zelf SAML-configuratie](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

## <a name="see-also"></a>Zie ook

- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
