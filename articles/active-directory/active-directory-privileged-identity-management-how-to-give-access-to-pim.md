<properties
   pageTitle="Hoe toegang te verlenen tot PIM | Microsoft Azure"
   description="Informatie over rollen toevoegen voor gebruikers met de extensie Azure Active Directory beschermde Identity Management zodat ze PIM kunnen beheren."
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
   ms.date="09/22/2016"
   ms.author="kgremban"/>

# <a name="how-to-give-access-to-manage-azure-ad-privileged-identity-management"></a>Geeft toegang tot het beheren van Azure AD beschermde Identity Management

De globale beheerder automatisch Azure AD beschermde Identity Management (PIM) voor een organisatie kan roltoewijzingen en toegang tot PIM opvragen. Niemand haalt schrijftoegang standaard, met inbegrip van andere globale beheerders. Andere globale beheerders, Beveiligingsbeheerders en beveiliging lezers hebben alleen-lezen toegang tot Azure AD PIM. Om toegang te verlenen aan PIM, kan de eerste gebruiker die anderen toekennen aan de rol **bevoorrechte rol beheerder** . Deze toewijzing moet worden uitgevoerd in PIM zelf en kan niet worden gewijzigd via PowerShell of andere portals.

> [AZURE.NOTE] Beheer van Azure AD PIM vereist Azure MVR gesloten. Aangezien Microsoft accounts niet voor Azure MVR gesloten registreren kunnen, kan een gebruiker die zich met een account van Microsoft aanmeldt Azure AD PIM niet openen.

Zorg ervoor dat er altijd ten minste twee gebruikers in een beheerdersrol bevoorrechte rol in het geval een gebruiker is vergrendeld of hun account wordt verwijderd.

## <a name="give-another-user-access-to-manage-pim"></a>Een andere gebruikerstoegang geven tot PIM beheren

1. Aanmelden bij de [Azure portal](https://portal.azure.com/) en selecteer de **Azure AD beschermde Identity Management** app op het dashboard.
2. Selecteer **beschermde rollen beheren** > **bevoorrechte rol beheerder** > **toevoegen**.

    ![Beheerders van bevoorrechte rol - schermafbeelding toevoegen][1]

4. Stap 1 is al voltooid op het blad van de beheerde gebruikers toevoegen. Selecteer in stap 2 gebruikers en **Selecteer** zoeken naar de gebruiker die u wilt toevoegen.

    ![Selecteer gebruikers - screenshot][2]

6. Selecteer de gebruiker in de lijst met zoekresultaten en klik op **Gereed**.
7. Klik op **OK** om de selectie opslaat. De gebruiker die u hebt geselecteerd wordt in de lijst met beheerders van bevoorrechte rol weergegeven.

    - Wanneer u een nieuwe rol aan iemand anders toewijzen, worden ze automatisch ingesteld als in aanmerking komende de rol. Als u permanent in de rol van maken, klikt u op de gebruiker in de lijst. **Macht maken** in het menu gebruiker gegevens selecteren.

8. Verzenden naar de gebruiker een koppeling om [aan de slag met Azure AD beschermde Identity Management](active-directory-privileged-identity-management-getting-started.md).


## <a name="remove-another-users-access-rights-for-managing-pim"></a>Een andere gebruiker toegangsrechten voor het beheren van PIM verwijderen

Voordat u uit de beheerdersrol bevoorrechte rol verwijderen, zorg ervoor dat altijd zal blijven er twee gebruikers zijn toegewezen.

1. Klik op de rol **beheerder bevoorrechte rol**in het dashboard PIM.  De lijst met gebruikers die momenteel in die rol wordt weergegeven.
2. Klik op de gebruiker in de lijst met gebruikers.
3. Klik op **verwijderen**.  Een bevestigingsbericht wordt weergegeven.
4. Klik op **Ja** om de gebruiker te verwijderen uit de rol.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_add_PRA.png
[2]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_select_users.png
