<properties
   pageTitle="Toevoegen of verwijderen van een gebruikersfunctie | Microsoft Azure"
   description="Informatie over rollen toevoegen aan beschermde identiteiten aan de toepassing Azure Active Directory beschermde Identity Management."
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
   ms.date="10/24/2016"
   ms.author="kgremban"/>

# <a name="azure-ad-privileged-identity-management-how-to-add-or-remove-a-user-role"></a>Azure AD beschermde Identity Management: Het toevoegen of verwijderen van een gebruikersfunctie

Met Azure Active Directory (AD), kunt een globale beheerder (of de systeembeheerder van bedrijf) bijwerken welke gebruikers zijn **permanent** toegewezen aan rollen in Azure AD. Dit wordt gedaan met de PowerShell cmdlets zoals `Add-MsolRoleMember` en `Remove-MsolRoleMember`. Of ze kunnen de klassieke Azure portal gebruiken zoals beschreven in het [toewijzen van beheerdersrollen in Azure Active Directory](active-directory-assign-admin-roles.md).

De toepassing Azure AD beschermde identiteitsbeheer kan beheerders bevoorrechte rol ook permanent roltoewijzingen maken. Beheerders van bevoorrechte rol kunnen u bovendien gebruikers **die in aanmerking komen** voor de rollen beheerder. Een in aanmerking komende admin kunt de functie activeren wanneer ze deze nodig hebben, en vervolgens hun machtigingen verlopen zodra ze klaar.

## <a name="manage-roles-with-pim-in-the-azure-portal"></a>Rollen met PIM in Azure portal beheren

In uw organisatie, kunt u gebruikers toewijzen aan verschillende administratieve rollen in Azure AD, Office 365 en andere Microsoft-services en toepassingen.  Meer informatie over de beschikbare functies kunnen u vinden op de [rollen in Azure AD PIM](active-directory-privileged-identity-management-roles.md).

Als u wilt toevoegen of verwijderen van een gebruiker in een rol met beschermde Identity Management, het dashboard PIM weergeven. Vervolgens klikt u op **gebruikers in rollen Admin** of Selecteer een specifieke rol (zoals globale beheerder) uit de tabel functies.

> [AZURE.NOTE] Als u nog PIM in Azure portal nog niet hebt ingeschakeld, gaat u aan de [slag met Azure AD PIM](active-directory-privileged-identity-management-getting-started.md) voor meer informatie.

Als u wilt dat een andere gebruikerstoegang te verlenen tot PIM zelf, zijn de rollen die PIM heeft de gebruiker nodig wordt verder beschreven in [hoe u toegang geven tot PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="add-a-user-to-a-role"></a>Een gebruiker aan een rol toevoegen

1. Selecteer de tegel **Azure AD beschermde Identity Management** op het dashboard in [Azure portal](https://portal.azure.com/).
2. Selecteer **beschermde rollen beheren**.
3. Selecteer in de tabel **Samenvatting rol** , de rol die u wilt beheren.
4. Selecteer in het blad rol **toevoegen**.
5. Klik op **gebruikers selecteren** en zoeken naar de gebruiker op de bladeserver **gebruikers selecteren** .  
6. Selecteer de gebruiker in de lijst met zoekresultaten en klik op **Gereed**.
4. Klik op **OK** om de selectie opslaat. De gebruiker die u hebt geselecteerd wordt in de lijst als in aanmerking komend voor de rol weergegeven.

> [AZURE.NOTE]
>Nieuwe gebruikers een rol alleen in aanmerking komen voor de rol van standaard. Als u wilt dat het permanent maken van de rol, klikt u op de gebruiker in de lijst. Informatie van de gebruiker wordt weergegeven in een nieuwe blade. **Macht maken** in het menu gebruiker gegevens selecteren.  
>Als een gebruiker niet MVR van voor Azure meerledige verificatie (gesloten registreren kan) of met behulp van een Microsoft-account (meestal @outlook.com), moet u permanent in alle hun rollen maken. In aanmerking komende beheerders wordt te registreren voor de MVR gesloten tijdens de activering gevraagd.

Nu dat de gebruiker in aanmerking voor een rol komt, laten weten dat zij deze volgens de instructies in [het activeren of deactiveren van een rol activeren kunnen](active-directory-privileged-identity-management-how-to-activate-role.md).

## <a name="remove-a-user-from-a-role"></a>Een gebruiker uit een rol verwijderen

U kunt gebruikers verwijderen uit de toewijzing van functies die in aanmerking komen, maar zorg ervoor dat er altijd ten minste één gebruiker die een permanente algemene beheerder is.

Ga als volgt te werk om een specifieke gebruiker uit een rol verwijderen:

1. Ga naar de rol in de lijst functie door een rol in het dashboard Azure AD PIM te selecteren of door te klikken op de knop **gebruikers in rollen Admin** .
2. Klik op de gebruiker in de lijst met gebruikers.
3. Klik op **verwijderen**. Een bericht wordt u gevraagd om te bevestigen.
4. Klik op **Ja** om de rol van de gebruiker verwijderd.

Als u niet zeker weet welke gebruikers moeten nog steeds hun roltoewijzingen, vervolgens kunt u [start een revisie van de toegang voor de rol](active-directory-privileged-identity-management-how-to-start-security-review.md).


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
