<properties
    pageTitle="Veelgestelde vragen over Azure Active Directory | Microsoft Azure"
    description="Azure Active Directory Veelgestelde vragen die antwoorden op vragen in combinatie met de toegang tot Azure en Azure Active Directory, biedt wachtwoord beheer- en access."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/16/2016"
    ms.author="markusvi"/>

# <a name="azure-active-directory-faq"></a>Veelgestelde vragen over Azure Active Directory

Azure Active Directory is een uitgebreide identiteit als (IDaaS) serviceoplossing die alle aspecten van de identiteit,, toegangsbeheer en beveiliging omvat.


Zie voor meer informatie [Wat is Azure Active Directory?](active-directory-whatis.md).



## <a name="accessing-azure-and-azure-active-directory"></a>Toegang tot Azure en Azure Active Directory


**V: Waarom krijg ik 'geen abonnementen gevonden' als ik probeer toegang te krijgen tot Azure AD in de klassieke Azure portal (https://manage.windowsazure.com)?**

**A:** Toegang tot de portal voor Azure klassieke moet elke gebruiker machtigingen op een Azure-abonnement. Als u een betaalde Office 365 of Azure AD, Ga naar [http://aka.ms/accessAAD](http://aka.ms/accessAAD) voor een eenmalige activeringsstap hebt, anders moet u een volledige [evaluatie Azure](https://azure.microsoft.com/pricing/free-trial/) of een betaalde licentie te activeren. 

Zie voor meer informatie:

- [Hoe Azure abonnementen gekoppeld aan Azure Active Directory zijn](active-directory-how-subscriptions-associated-directory.md)

- [De map voor uw abonnement op Office 365 in Azure beheren](active-directory-manage-o365-subscription.md)

---

**V: Wat is de relatie tussen AD Azure, Office 365 en Azure?**

**A:** Azure Active Directory biedt u mogelijkheden voor gemeenschappelijke identiteits- en toegangsbeheer voor alle Microsoft online services. Of u met Office 365, Microsoft Azure, Intune of anderen gebruikt u al een advertentie Azure aanmelding inschakelen en gebruiken voor al deze diensten. 

Alle gebruikers die u hebt ingeschakeld voor Microsoft Online services worden in feite gedefinieerd als gebruikersaccounts in een of meer exemplaren van AD Azure. U kunt deze rekeningen gratis Azure AD mogelijkheden zoals wolk toepassing toegang inschakelen.
 
Bovendien AD Azure services betaald (bv.: Azure AD basic, Premium, EMS, enz.) een aanvulling op andere on line services, zoals Office 365 en Microsoft Azure met uitgebreide schaal beheer- en bedrijfsoplossingen.


---



## <a name="getting-started-with-hybrid-azure-ad"></a>Aan de slag met Azure AD Hybrid


**V: hoe kan ik mijn directory op ruimten naar Azure AD aansluiten?**

**A:** Kunt u de map op locatie Azure AD met **Azure AD verbinden**. 

Zie [integratie van identiteiten in ruimten met Azure Active Directory](active-directory-aadconnect.md)voor meer informatie.


---

**V: hoe kan ik SSO instellen tussen mijn map op gebouwen en Mijn cloud-toepassingen?**

**A:** U hoeft alleen de eenmalige aanmelding instellen tussen uw directory op gebouwen en Azure AD. Als u toegang uw cloud-toepassingen via Azure AD tot, stations de service automatisch gebruikers correct worden geverifieerd met hun referenties op gebouwen.

SSO implementeren op ruimten kan worden gemakkelijk met federation-oplossingen zoals AD FS of wachtwoord hash-synchronisatie configureren. U kunt beide opties met de configuratiewizard Azure AD verbinden gemakkelijk implementeren.
  

Zie [integratie van identiteiten in ruimten met Azure Active Directory](active-directory-aadconnect.md)voor meer informatie.
  

---

**V: Azure Active Directory biedt een self-service portal voor gebruikers in mijn organisatie?**

**A:** Ja, Azure Active Directory biedt het [deelvenster Azure AD toegang](http://myapps.microsoft.com) voor de gebruiker zelf en toegang tot toepassingen. Als u een Office 365-klant bent, vindt u veel van dezelfde functionaliteit in de Office 365 portal. 

Voor meer informatie, Zie de [Inleiding tot het Access-venster](active-directory-saas-access-panel-introduction.md). 



---

**V: is Azure AD Help mij bij mijn op gebouwen-infrastructuur beheren?**

**A:** Ja, dit het geval is. De Azure AD Premium edition biedt u **Verbinding maken met de gezondheid**. Gezondheid Azure AD verbinding helpt u bij het bewaken en inzicht in uw identiteit op gebouwen infrastructuur en de diensten van de synchronisatie.  

Voor meer informatie Zie [Monitor uw locatie op identiteit synchronisatie van infrastructuur en services in de cloud](active-directory-aadconnect-health.md).  

---

## <a name="password-management"></a>Wachtwoordbeheer

**V: kan ik de Azure AD wachtwoord terugneming gebruiken zonder wachtwoord synchronisatie? (AKA, ik wil graag Azure AD SSPR gebruiken met wachtwoord terugneming maar ik wil niet dat mijn wachtwoorden opgeslagen in de cloud?)**

**A:** U hoeft niet uw wachtwoorden AD naar Azure AD synchroniseren zodat terugschrijven. In een federatieve omgeving is Azure AD SSO is afhankelijk van de map op locatie om de gebruiker te verifiëren. In dit scenario is niet vereist voor het wachtwoord op gebouwen in Azure AD worden bijgehouden.

---

**V: hoe lang duurt het om een wachtwoord terug naar AD op ruimten worden geschreven?**

**A:** Terugneming wachtwoord werkt in real-time. 

Zie voor meer details [aan de slag met wachtwoordbeheer](active-directory-passwords-getting-started.md) 


---

**V: kan ik wachtwoord terugschrijven gebruiken met wachtwoorden die door een beheerder worden beheerd?**

**A:** Ja, als u terugschrijven ingeschakeld wachtwoord, het wachtwoord bewerkingen die worden uitgevoerd door een beheerder worden opgeslagen in uw omgeving op gebouwen.  

Vragen over, Zie voor meer antwoorden op wachtwoord [Wachtwoord beheer Veelgestelde vragen](active-directory-passwords-faq.md).

---

## <a name="application-access"></a>Toegang tot toepassingen


**V: waar vind ik een lijst met toepassingen die vooraf worden geïntegreerd met AD Azure en hun mogelijkheden?**

**A:** Azure AD heeft meer dan 2600 vooraf geïntegreerde toepassingen van Microsoft, application serviceproviders of partners. Alle toepassingen van vooraf geïntegreerde ondersteuning voor eenmalige aanmelding. Eenmalige aanmelding kunt u uw organisatie-referenties gebruikt om uw toepassingen. Sommige toepassingen ondersteunen ook geautomatiseerde provisioning en verwijderen van gegevens

Zie de [Active Directory-marktplaats](https://azure.microsoft.com/marketplace/active-directory/)voor een volledige lijst van de vooraf geïntegreerde toepassingen.


---

**V: Wat gebeurt er als de toepassing die ik nodig heb is niet op de markt Azure AD?**

**A:** U kunt met Azure AD Premium, toevoegen en configureren van elke gewenste toepassing. Afhankelijk van uw voorkeuren en mogelijkheden van uw toepassing, kunt u eenmalige aanmelding en geautomatiseerde provisioning.  

Zie voor meer informatie:

- [Eenmalige aanmelding voor toepassingen die niet in de galerie met Azure Active Directory-toepassing configureren](active-directory-saas-custom-apps.md)
- [Met behulp van SCIM automatisch inrichten van gebruikers en groepen van Azure Active Directory voor toepassingen inschakelen](active-directory-scim-provisioning.md) 


---

**V: hoe gebruikers zich aanmelden in Azure Active Directory-toepassingen?**
 
**A:** Azure Active directory biedt verschillende manieren om gebruikers te bekijken en toegang tot hun toepassingen, zoals:

- Het deelvenster Azure AD toegang

- De Office 365-toepassing starten

- Rechtstreekse aanmelding voor toegang tot federatieve toepassingen

- Diep koppelingen naar federatieve, op basis van wachtwoorden of bestaande apps

Zie [Deploying AD Azure geïntegreerde toepassingen voor gebruikers](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)voor meer informatie.


---

**V: wat zijn de verschillende manieren Azure Active Directory kunnen u verificatie en eenmalige aanmelding voor toepassingen?**
 
**A:** Azure Active Directory ondersteunt een groot aantal gestandaardiseerde protocollen voor verificatie en autorisatie zoals SAML 2.0 verbinding OpenID, OAuth 2.0 en WS-Federation. Azure AD ondersteunt ook wachtwoord vaulting en geautomatiseerde-in mogelijkheden voor apps die alleen ondersteuning voor forms-verificatie.  

Zie voor meer informatie:

- [Verificatie-scenario's voor Azure AD](active-directory-authentication-scenarios.md)

- [Active Directory-verificatieprotocollen](https://msdn.microsoft.com/library/azure/dn151124.aspx)

- [Hoe de eenmalige aanmelding met Azure Active Directory werk?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)


---

**V: kan ik toepassingen die ik heb op ruimten toevoegen?**

**A:** AD-toepassingsproxy Azure biedt eenvoudige en veilige toegang tot webtoepassingen voor bedrijven die u kiest. U kunt deze toepassingen gebruiken op dezelfde manier die krijgt u toegang uw toepassingen SaaS in Azure Active Directory tot. Er is geen noodzaak voor een VPN- of het wijzigen van uw netwerkinfrastructuur.  

Voor meer informatie, Zie [veilige externe toegang tot toepassingen in gebouwen](active-directory-application-proxy-get-started.md).


--- 

**V: hoe nodig ik MVR gesloten voor gebruikers met toegang tot een bepaalde toepassing hebben?**

**A:** U kunt met Azure AD voorwaardelijke toegang, een unieke-beleid voor elke toepassing. U kunt in uw beleid, MVR gesloten vereisen te allen tijde, of wanneer de gebruikers niet zijn verbonden met het lokale netwerk.  

Zie [toegang tot Office 365 en andere apps verbonden met Azure Active Directory beveiligen](active-directory-conditional-access.md)voor meer informatie.


---

**V: Wat is geautomatiseerd gebruikersaanvragen voor SaaS-toepassingen?**

**A:** Azure Active Directory kunt u het maken, onderhouden en verwijderen van gebruikersidentiteiten in veel populaire cloud (SaaS)-toepassingen te automatiseren. 

Zie [Gebruikersaanvragen automatiseren en Deprovisioning voor SaaS-toepassingen met Azure Active Directory](active-directory-saas-app-provisioning.md) voor meer informatie

---



