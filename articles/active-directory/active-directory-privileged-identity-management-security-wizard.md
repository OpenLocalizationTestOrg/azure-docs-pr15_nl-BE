<properties
   pageTitle="De beveiligingswizard op Azure AD beschermde Identity Management"
   description="De eerste keer dat u de extensie Azure Active Directory beschermde Identity Management wordt weergegeven met een beveiligingswizard. Dit artikel beschrijft de stappen voor het gebruik van de wizard."
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

# <a name="the-azure-ad-privileged-identity-management-security-wizard"></a>De beveiligingswizard op Azure AD beschermde Identity Management

Als u de eerste persoon die Azure beschermde Identity Management (PIM) voor uw organisatie uitvoeren, worden weergegeven met een wizard. De wizard helpt u bij het beveiligingsrisico van beschermde identiteiten en hoe deze risico's te verminderen met PIM kent. U hoeft niet te wijzigingen aanbrengen in een bestaande toewijzing van functies in de wizard als u liever later doen.

## <a name="what-to-expect"></a>Wat u kunt verwachten

Voordat u uw organisatie begint met behulp van PIM, alle roltoewijzingen zijn permanent: de gebruikers zich altijd in deze rollen ook als ze hun bevoegdheden niet momenteel nodig.  De eerste stap van de wizard ziet u een lijst met veel rechten, rollen en hoeveel gebruikers zijn momenteel in deze functies. U kunt inzoomen aan een bepaalde functie voor meer informatie over gebruikers, als er een of meer van hen niet bekend zijn.

De tweede stap van de wizard kunt u een toewijzing van de functies van de beheerder wijzigen.  

> [AZURE.WARNING]Het is belangrijk dat er ten minste één globale beheerder en meer dan een bevoorrechte rol beheerder met een organisatie account (dus niet door een Microsoft-account). Als er slechts één bevoorrechte rol beheerder, kunnen de organisatie worden niet PIM beheren als deze account wordt verwijderd.
> Bovendien houden roltoewijzingen permanent als een gebruiker een Microsoft-account (een account die wordt gebruikt voor aanmelding bij Microsoft services, zoals Skype en Outlook.com). Als u wilt vereisen MVR gesloten voor activering voor die rol, wordt die gebruiker vergrendeld.


Nadat u wijzigingen hebt aangebracht, wordt de wizard niet meer weergegeven. De volgende keer dat u of een andere bevoorrechte rol beheerder gebruikt PIM, ziet u het dashboard PIM.  

- Als u wilt toevoegen of verwijderen uit functies of toewijzingen uit om in aanmerking voor de permanente lezen meer bij [het toevoegen of verwijderen van de rol van een gebruiker](active-directory-privileged-identity-management-how-to-add-role-to-user.md)wijzigen.
- Meer informatie als u meer gebruikers toegang geven wilt tot het beheren van PIM op [toegang geven tot het beheer van PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).



## <a name="next-steps"></a>Volgende stappen
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
