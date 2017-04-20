<properties
   pageTitle="Het meerledige verificatie vereisen | Microsoft Azure"
   description="Informatie over het meerledige verificatie (MVR gesloten) vereist is voor geprivilegieerde identiteiten met de extensie Azure Active Directory beschermde Identity Management."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="how-to-require-mfa-in-azure-ad-privileged-identity-management"></a>Het vereisen van de MVR gesloten in Azure AD beschermde Identity Management

We raden u aan meerledige verificatie (MVR gesloten) voor al uw beheerders. Dit verkleint het risico van een aanval door een gekraakte wachtwoord.

U kunt vereisen dat gebruikers een uitdaging MVR gesloten voltooien wanneer ze zich aanmelden. Het weblogbericht [MVR gesloten voor Office 365 en MVR gesloten voor Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/) vergelijkt wat is opgenomen in Office en Azure-abonnementen, met de onderdelen van Microsoft Azure meerledige verificatie aangeboden.

U kunt ook vereisen dat gebruikers een uitdaging MVR gesloten voltooien wanneer ze een rol in Azure AD PIM activeert. Op deze manier, als de gebruiker niet een uitdaging MVR gesloten wanneer deze zijn ondertekend, wordt ze gevraagd om dit te doen door PIM.

## <a name="requiring-mfa-in-azure-ad-privileged-identity-management"></a>MVR gesloten identiteitsbeheer Azure AD privileges vereisen

Bij het beheren van identiteiten in PIM als geprivilegieerde rol beheerder er waarschuwingen waarin wordt aanbevolen de MVR gesloten voor beschermde accounts. Op de beveiligingswaarschuwing in het dashboard PIM en een nieuwe blade wordt geopend met een lijst van de administrator-accounts waarvoor MVR gesloten vereist.  U kunt verplicht stellen MVR gesloten door meerdere rollen te selecteren en vervolgens te klikken op de knop **herstellen** of u kunt op de weglatingstekens afzonderlijke rollen en klik vervolgens op de knop **herstellen** .

> [AZURE.IMPORTANT] En nu werkt alleen voor Azure MVR gesloten met werk of school rekeningen, geen Microsoft-accounts (meestal een persoonlijke account die wordt gebruikt voor aanmelding bij Microsoft-diensten zoals Skype, Xbox, Outlook.com, enz.) naar rechts. Hierdoor kan iedereen met behulp van een Microsoft-account een in aanmerking komende admin niet omdat ze MVR gesloten niet gebruiken voor het activeren van hun rollen. Als deze gebruikers blijven werkbelasting met behulp van een Microsoft-account te beheren moeten, waardoor u ze permanent beheerders nu.

Bovendien kunt u de vereiste MVR gesloten voor een specifieke rol wijzigen door erop te klikken in de sectie rollen van het dashboard PIM. Klik vervolgens op **Instellingen** in de rol van blade en vervolgens onder meerledige verificatie **inschakelen** te selecteren.

## <a name="how-azure-ad-pim-validates-mfa"></a>Hoe Azure AD PIM valideert MVR gesloten

Er zijn twee opties voor het valideren van de MVR gesloten wanneer een gebruiker een rol te activeren.

De eenvoudigste optie is vertrouwen op Azure MVR gesloten voor gebruikers die een geprivilegieerde rol activeert. Hiertoe controleren die gebruikers worden in licentie gegeven, indien nodig, en voor Azure MVR gesloten zijn geregistreerd. Meer informatie over hoe u dit doet, wordt in [aan de slag met Azure meerledige verificatie in de cloud](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md). Het wordt aanbevolen, maar niet vereist dat u configureert Azure AD om af te dwingen MVR gesloten voor deze gebruikers wanneer ze zich aanmelden. Dit komt doordat de MVR gesloten controles door Azure AD PIM zelf komen.

Als gebruikers worden geverifieerd op ruimten kunt u ook laten uw identiteitsprovider verantwoordelijk voor de MVR gesloten. Als u hebt AD omvat Federation Services voordat u AD Azure, [cloud-bronnen beveiligen met Azure meerledige verificatie en AD FS](../multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud.md) -verificatie op basis van een smartcard vereist instructies voor het configureren van AD FS voor het verzenden van aanvragen naar Azure AD. Wanneer een gebruiker probeert een functie te activeren, accepteert Azure AD PIM dat MVR gesloten is al gevalideerd voor de gebruiker het juiste claims krijgt.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
