<properties
    pageTitle="Azure MVR gesloten cloud vs server | Microsoft Azure"
    description="Kies de secutiry oplossing meerledige verificatie is recht voor u door te vragen wat ik bij het beveiligen van am en waar zijn mijn gebruikers zich bevindt.  Kies vervolgens de wolk, MVR gesloten Server of AD FS."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>

#<a name="choose-the-azure-multi-factor-authentication-solution-for-you"></a>Kies de Azure meerledige verificatie-oplossing voor u

Omdat er verschillende flavors MVR van van Azure meerledige verificatie (gesloten), moeten we beantwoord enkele vragen om erachter te komen welke versie het hulpmiddel is te gebruiken.  Deze vragen zijn:

-   [Wat ben ik probeert te beveiligen](#what-am-i-trying-to-secure)
-   [Waar de gebruikers zich bevinden](#where-are-the-users-located)
- [Welke opties heb ik nodig?](#what-featured-do-i-need)

De volgende secties bevatten richtlijnen voor het bepalen van elk van deze antwoorden.

## <a name="what-am-i-trying-to-secure"></a>Wat ben ik probeert te beveiligen?

Om te bepalen van de juiste twee stappen verificatie oplossing, eerst we de vraag moet beantwoorden van welke u wilt beveiligen met een tweede methode voor verificatie.  Is het een toepassing die in Azure is?  Of een RAS-systeem?  Door te bepalen wat we wilt beveiligen, kunnen we de vraag van waar meerledige verificatie moet worden ingeschakeld.  


Wat wilt u beveiligen| Meerledige verificatie in de cloud|Meerledige verificatieserver
------------- | :-------------: | :-------------: |
Directe Microsoft apps|● |● |
SaaS-apps in de app-galerie|● |● |
IIS-toepassingen wordt gepubliceerd via Azure AD App Proxy|● |● |
IIS-toepassingen die niet zijn gepubliceerd via Azure AD App Proxy | |● |
Externe toegang, zoals VPN, RDG| |● |



## <a name="where-are-the-users-located"></a>Waar de gebruikers zich bevinden

Vervolgens kijken naar onze gebruikers helpt bij het bepalen van de juiste oplossing te gebruiken in de cloud of op locatie met behulp van de Server van de MVR gesloten.



Locatie| Meerledige verificatie in de cloud|Meerledige verificatieserver
------------- | :-------------: | :-------------: |
Azure Active Directory|● | |
Azure AD en Federatie met AD FS met AD ruimten|● |● |
Azure AD en DirSync, Azure AD Sync, Azure AD verbinden - geen wachtwoord synchroniseren met AD ruimten|● |● |
Azure AD en AD DirSync, Azure AD Sync, Azure AD Connect - met wachtwoord synchronisatie met ruimten|● | |
On-premises Active Directory| |● |

## <a name="what-features-do-i-need"></a>Welke opties heb ik nodig?

De volgende tabel wordt een vergelijking van de functies die beschikbaar met een meerledige verificatie in de cloud en met de Server meerledige verificatie zijn.

 | Meerledige verificatie in de cloud | Meerledige verificatieserver
------------- | :-------------: | :-------------: |
Mobiele app-meldingen als tweede factor | ● | ● |
Mobiele app verificatiecode als tweede factor | ● | ●
Telefoongesprek als tweede factor | ● | ●
Enkelvoudige SMS als tweede factor | ● | ●
Twee richtingen SMS als tweede factor |  | ●
Hardware-Tokens als tweede factor |  | ●
Wachtwoorden voor clients die geen ondersteuning voor MVR gesloten bieden App | ● |  
Admin controle over verificatiemethoden | ● | ●
PIN-modus |  | ●
Fraudewaarschuwing | ● | ●
Rapporten van de MVR gesloten | ● | ●
Eenmalige overslaan |  | ●
Aangepaste begroetingen voor telefoongesprekken | ● | ●
Aanpasbare beller-ID voor telefoongesprekken | ● | ●
Vertrouwde IP-adressen | ● | ●
MVR gesloten voor vertrouwde apparaten onthouden  | ● |  
Voorwaardelijke toegang | ● | ●
Cache |  | ●

Nu dat we hebben vastgesteld of cloud meerledige verificatie of de Server van de MVR gesloten op ruimten, we kunnen aan de slag instellen en gebruiken van Azure meerledige verificatie. **Selecteer het pictogram voor uw scenario!**

<center>




[![Cloud](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Proofup](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</center>
