<properties
    pageTitle="Beveiliging van bevoorrechte toegang in Azure AD | Microsoft Azure"
    description="Een onderwerp met informatie over de methoden voor de beveiliging van bevoorrechte toegang via Azure, Azure Active Directory en Microsoft Online Services."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="mwahl"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="kgremban"/>


# <a name="securing-privileged-access-in-azure-ad"></a>Beveiliging van bevoorrechte toegang in Azure AD

Bevoorrechte toegang beveiligen is een kritieke eerste stap ter bescherming van de bedrijfsmiddelen in een moderne organisatie. Bevoegde accounts worden beheren en IT-systemen te beheren. Cyber-aanvallen als doel deze accounts toegang krijgen tot de gegevens en systemen van een organisatie. Wilt bevoorrechte toegang beveiligen, moet u de rekeningen en de systemen van het risico worden blootgesteld aan een kwaadwillende gebruiker te isoleren.

Meer gebruikers zijn bevoorrechte toegang krijgen via cloud-services gestart. Dit kunt globale beheerders van Office365, Azure abonnement beheerders en gebruikers met beheerderstoegang in VMs of SaaS apps opnemen.

Microsoft adviseert dat u deze routekaart volgen op [Bevoorrechte toegang beveiligen](https://technet.microsoft.com/library/mt631194.aspx).

Deze principes gelden voor klanten die Azure Active Directory gebruikt voor het beheren van toegang tot Azure, Office 365, of andere Microsoft-services en toepassingen, of gebruikersaccounts worden beheerd en geverifieerd door Active Directory of in Azure Active Directory. De volgende secties vindt meer informatie over functies voor de ondersteuning van bevoorrechte toegang beveiligen Azure AD.

## <a name="multi-factor-authentication"></a>Meerledige verificatie

U verbetert de beveiliging van de beheerder verificatie, moet u een meerledige verificatie (MVR gesloten) voordat het verlenen van bevoegdheden vereisen. MVR gesloten is een methode om te controleren wie u bent die het gebruik van meer dan alleen een gebruikersnaam en wachtwoord vereist. Het biedt een tweede laag van beveiliging voor aanmeldingen van gebruikers en transacties.

Azure meerledige verificatie helpt beschermen toegang tot gegevens en toepassingen voldoen aan eisen van de gebruiker voor een eenvoudig aanmelden proces. Levert een sterke verificatie via een reeks opties voor eenvoudige verificatie met inbegrip van telefoongesprekken, tekstberichten, mobiele app-meldingen of verificatiecode en 3de partij EDE tokens.

Zie de volgende video voor een overzicht van de werking van Azure meerledige verificatie.

>[AZURE.VIDEO windows-azure-multi-factor-authentication]

Zie voor meer details, [MVR gesloten voor Office 365 en MVR gesloten voor Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).

## <a name="time-bound-privileges"></a>Tijd-afhankelijke machtigingen

In sommige organisaties kunnen het gebeuren dat ze te veel gebruikers met bijzondere rechten rollen hebben. Een gebruiker kan zijn toegevoegd aan de rol voor een bepaalde activiteit, zoals zich aanmelden voor een service, maar deze rechten vaak achteraf niet gebruiken.

Verlaagt de belichtingstijd van bevoegdheden en verhogen uw inzicht in het gebruik ervan door gebruikers beperken tot alleen nemen op hun bevoegdheden Just in Time (JIT) bij het uitvoeren van een taak. U kunt voor Azure Active Directory en Microsoft Online Services, [Azure AD beschermde Identity Management (PIM)](http://aka.ms/AzurePIM).


![PIM-dashboard][2]


## <a name="attack-detection"></a>Aanval detectie

[Azure Active Directory identiteit bescherming](../active-directory-identityprotection.md) biedt een geconsolideerde weergave van risico's en potentiële beveiligingsproblemen op het gebied van de identiteit van uw organisatie. Op basis van risico's, berekend identiteit bescherming het risiconiveau van een gebruiker voor elke gebruiker, zodat u kunt configureren op basis van risico beleid ter bescherming van de identiteit van uw organisatie automatisch. Dit beleid, samen met andere besturingselementen voorwaardelijke toegang is geleverd door Azure Active Directory en EMS, kunnen automatisch blokkeren van de gebruiker of suggesties zoals wachtwoorden en meerledige verificatie afdwingen.

![Azure identiteitsbeveiliging AD][3]

## <a name="conditional-access"></a>Voorwaardelijke toegang

Azure Active Directory gecontroleerd met voorwaardelijke toegang beheren, de specifieke voorwaarden die u kiest bij het verifiëren van een gebruiker, voor het toestaan van toegang tot een toepassing. Zodra deze voorwaarden wordt voldaan, wordt de gebruiker geverifieerd en toegang hebben tot de toepassing.


![Het instellen van regels voor voorwaardelijke toegang met de MVR gesloten][4]


## <a name="related-articles"></a>Verwante artikelen

- [Azure meerledige verificatie](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md) inschakelen
- [Azure AD bevoorrechte Identity Management](../active-directory-privileged-identity-management-configure.md) inschakelen
- [Azure AD identiteit beveiliging](../active-directory-identityprotection.md) inschakelen
- [Besturingselementen voor voorwaardelijke toegang](../active-directory-conditional-access.md) inschakelen


Zie de sectie 'verantwoordelijkheden voor de klant en roadmap' van het document [Microsoft Cloud Security for Enterprise Architects](http://aka.ms/securecustomer) voor meer informatie over het bouwen van een routekaart voor volledige beveiliging. Neem contact op met uw Microsoft-vertegenwoordiger voor meer informatie over Microsoft-services om u te helpen bij deze onderwerpen die, of Ga naar onze [pagina voor Cybersecurity-oplossingen](https://www.microsoft.com/microsoftservices/campaigns/cybersecurity-protection.aspx).

<!--Image references-->
[1]: ../media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ../media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ../media/active-directory-identityprotection/29.png
[4]: ../media/active-directory-conditional-access/conditionalaccess-saas-apps.png
