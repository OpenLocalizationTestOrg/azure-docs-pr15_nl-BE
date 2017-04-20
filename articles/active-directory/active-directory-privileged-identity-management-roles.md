<properties
   pageTitle="Rollen in PIM | Microsoft Azure"
   description="Informatie over welke functies worden gebruikt voor beschermde identiteiten met de extensie Azure beschermde Identity Management."
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

# <a name="roles-in-azure-ad-privileged-identity-management"></a>Rollen in Azure AD bevoorrecht Identity Management

<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

U kunt gebruikers in uw organisatie verschillende administratieve rollen in Azure AD. Deze roltoewijzingen bepalen welke taken, zoals het toevoegen of verwijderen van gebruikers of wijzigen van instellingen, de gebruikers kunnen uitvoeren op Azure AD, Office 365 en andere Microsoft Online Services en toepassingen verbonden.  

Een globale beheerder kunt bijwerken, welke gebruikers zijn **permanent** toegewezen aan rollen in Azure AD, met PowerShell-cmdlets, zoals `Add-MsolRoleMember` en `Remove-MsolRoleMember`, of via de klassieke portal als beschreven in [de beheerdersrollen in Azure Active Directory toe te wijzen](active-directory-assign-admin-roles.md).

Azure rechten AD Identity Management (PIM) beheert beleidsregels voor bevoorrechte toegang voor gebruikers in Azure AD. PIM wijst gebruikers toe aan een of meer rollen in Azure AD en kunt u iemand zich permanent in de rol, of die in aanmerking komen voor de rol. Wanneer een gebruiker permanent is toegewezen aan een functie of een in aanmerking komende roltoewijzing wordt geactiveerd en vervolgens kunnen ze Active Directory Azure, Office 365 en andere toepassingen met de machtigingen die zijn toegewezen aan hun rollen beheren.

Er is geen verschil in de toegang gegeven aan iemand met een permanente versus een roltoewijzing die in aanmerking komen. Het enige verschil is dat sommige mensen niet nodig die toegang tot alle tijd hebt. Zij worden gedaan die in aanmerking komen voor de rol en kunnen inschakelen en uitschakelen wanneer ze moeten.

## <a name="roles-managed-in-pim"></a>Rollen die worden beheerd in PIM

Beschermde Identity Management kunt u gebruikers toewijzen aan de administrator rollen, met inbegrip van:


- **Globale beheerder** (ook bekend als de systeembeheerder van bedrijf) heeft toegang tot alle administratieve functies. U kunt meer dan één globale beheerder in uw organisatie hebben. De persoon die zich aanmeldt automatisch Office 365 aanschaffen wordt een globale beheerder.
- **Bevoorrechte rol beheerder** beheert Azure AD PIM en rol-toewijzingen voor andere gebruikers.  
- **Facturering beheerder** maakt inkopen, abonnementen beheren support tickets beheert en controleert de gezondheid van de service.
- **Administrator wachtwoord** wachtwoorden opnieuw instellen, serviceaanvragen beheert en controleert de gezondheid van de service. Beheerders wachtwoord zijn beperkt tot het opnieuw instellen van wachtwoorden voor gebruikers.
- **De beheerder** beheert aanvragen en health Monitor-service.

  > [AZURE.NOTE] Als u van Office 365 gebruikmaakt, vervolgens voordat u de service admin rol toewijst aan een gebruiker, eerst de gebruiker administratieve machtigingen toewijzen aan een service, zoals Exchange Online.

- **Beheerder gebruiker** wachtwoorden opnieuw instellen, health service controleert en beheert u gebruikersaccounts, gebruikersgroepen en serviceaanvragen. Admin gebruiker management kan niet verwijderen van een globale beheerder van andere rollen admin maken of opnieuw instellen van wachtwoorden voor de verrekening, globale, Serviceadministrators.
- **Exchange-beheerder** heeft administratieve toegang tot Exchange Online via het beheercentrum van Exchange (SBV) en kan bijna elke taak uitvoeren in Exchange Online.
- **SharePoint-beheerder** met beheerderstoegang tot SharePoint Online via SharePoint Online admin center en bijna elke taak kunt uitvoeren in SharePoint Online.
- **Skype voor Business administrator** beheerderstoegang tot Skype voor bedrijven via de Skype voor Business admin center en bijna elke taak kunt uitvoeren in Skype for Business Online.

Lees deze artikelen voor meer informatie over [toegewezen beheerdersrollen in Azure AD](active-directory-assign-admin-roles.md) en het [toewijzen van rollen admin in Office 365](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504).

<!--**PLACEHOLDER: The above article may not be the one we want since PIM gets roles from places other that Office 365**-->


Van PIM kunt u [deze rollen aan een gebruiker toewijzen](active-directory-privileged-identity-management-how-to-add-role-to-user.md) zodat de gebruiker kan [de rol als dat nodig is](active-directory-privileged-identity-management-how-to-activate-role.md).

Als u wilt dat een andere gebruikerstoegang geven tot in PIM zelf beheren, zijn de rollen die PIM heeft de gebruiker nodig wordt verder beschreven in [hoe u toegang geven tot PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).


<!-- ## The PIM Security Administrator Role **PLACEHOLDER: Need description of the Security Administrator role.**-->

## <a name="roles-not-managed-in-pim"></a>Functies die niet worden beheerd in PIM

Rollen binnen Exchange Online of SharePoint Online, met uitzondering van die hierboven vermeld, worden niet weergegeven in Azure AD en dus niet zichtbaar zijn in PIM. Zie voor meer informatie over het wijzigen van fijnmazig roltoewijzingen in deze Office 365 services [machtigingen in Office 365](https://support.office.com/article/Permissions-in-Office-365-da585eea-f576-4f55-a1e0-87090b6aaa9d).

Azure abonnementen en resourcegroepen worden ook niet weergegeven in Azure AD. Azure abonnementen beheren, [toevoegen of wijzigen van de beheerdersrollen Azure](../billing-add-change-azure-subscription-administrator.md) Zie en Zie [Toegangsbeheer Azure Role-Based](role-based-access-control-configure.md)voor meer informatie over Azure RBAC.

<!--**The above links might be replaced by ones that are from within this documentation repository **-->


## <a name="user-roles-and-signing-in"></a>Gebruikersrollen en aanmelden
Voor bepaalde Microsoft-services en toepassingen, een gebruiker toewijzen aan een rol mogelijk niet voldoende zijn voor een beheerder zijn voor die gebruiker.

Toegang tot de klassieke Azure portal moet dat de gebruiker een beheerder of een CO-beheerder op een Azure-abonnement worden zelfs als de gebruiker niet hoeft te de Azure abonnementen beheren.  Bijvoorbeeld voor het beheren van configuratie-instellingen voor Azure advertentie in de klassieke portal, een gebruiker moet zowel een globale beheerder in Azure AD en een abonnement CO-beheerder op een Azure-abonnement.  Zie meer informatie over het toevoegen van gebruikers aan abonnementen Azure, [het toevoegen of wijzigen van de beheerdersrollen Azure](../billing-add-change-azure-subscription-administrator.md).

Toegang tot Microsoft Online Services kan eisen dat de gebruiker ook beschikken over een licentie kunnen openen van de service portal of beheertaken uit te voeren.

## <a name="assign-a-license-to-a-user-in-azure-ad"></a>Een licentie toewijzen aan een gebruiker in AD Azure

1. Aanmelden bij de [Azure klassieke portal] (http://manage.windowsazure.com) met een globale administrator-account of een collega administrator-account.
2. **Alle Items** selecteren in het hoofdmenu.
3. Selecteer de map waarin die u wilt werken en die is gekoppeld aan licenties.
4. Selecteer **licenties**. De lijst met beschikbare licenties wordt weergegeven.
5. Selecteer het licentie-plan waarin de licenties die u wilt verdelen.
6. Selecteer **gebruikers toewijzen**.
7. Selecteer de gebruiker die u wilt een licentie toewijzen.
8. Klik op de knop **toewijzen** .  De gebruiker kan nu inloggen op Azure.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
