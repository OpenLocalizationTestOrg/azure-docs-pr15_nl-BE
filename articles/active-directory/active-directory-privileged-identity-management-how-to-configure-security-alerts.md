<properties
   pageTitle="Waarschuwingen configureren | Microsoft Azure"
   description="Informatie over beveiligingswaarschuwingen voor Azure beschermde Identity Management-extensie configureren."
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
   ms.date="09/02/2016"
   ms.author="kgremban"/>

# <a name="how-to-configure-security-alerts-in-azure-ad-privileged-identity-management"></a>Waarschuwingen configureren in Azure AD beschermde Identity Management

## <a name="security-alerts"></a>Beveiligingswaarschuwingen
Azure beschermde Identity Management (PIM) waarschuwingen genereert wanneer er verdachte of onveilige activiteiten in uw omgeving. Wanneer een waarschuwing wordt gegeven, wordt deze weergegeven op het dashboard PIM. Selecteer de waarschuwing voor een overzicht van een rapport waarin de gebruikers of rollen die de waarschuwing heeft geactiveerd.

![PIM dashboard beveiligingswaarschuwingen - screenshot][1]



| Een waarschuwing | Trigger | Aanbeveling |
| ----- | ------- | -------------- |
| **Rollen worden toegewezen buiten PIM** | Een beheerder is permanent toegewezen aan een rol, buiten de PIM-interface. | Bekijk de nieuwe functie toewijzen. Omdat andere services alleen permanente beheerders toewijzen kunnen, een andere voor een toewijzing die in aanmerking komen. |
| **Rollen worden te vaak geactiveerd** | Er zijn te veel heractiveringen van dezelfde rol binnen de gestelde termijn in de instellingen. | Neem contact op met de gebruiker om te zien waarom ze hebt geactiveerd de rol zo vaak. Misschien is de termijn te kort voor hen om hun taken te voltooien, of misschien bent met behulp van scripts automatisch activeren een rol. |
| **Rollen vereist meerledige verificatie niet voor activering** | Er zijn rollen zonder MVR gesloten ingeschakeld in de instellingen. | We vereisen MVR gesloten voor de meest bijzondere rechten rollen, maar raden MVR gesloten voor de activering van alle functies in te schakelen. |
| **Hun bevoorrechte rol wordt niet gebruikt voor beheerders** | Er zijn in aanmerking komende beheerders die hun rollen onlangs niet hebt geactiveerd. | Start een revisie toegang om te bepalen welke gebruikers toegang niet meer nodig. |
| **Er zijn te veel algemene beheerders** | Er zijn meer globale beheerders dan aanbevolen. | Als u een groot aantal globale beheerders, is het waarschijnlijk dat gebruikers meer machtigingen dan ze nodig hebben krijgen. Gebruikers aan rollen minder bevoegde verplaatsen of maken ze in aanmerking komen voor de rol in plaats van permanent toegewezen. |

## <a name="configure-security-alert-settings"></a>Instellingen voor waarschuwingen configureren

U kunt sommige van de beveiligingswaarschuwingen in PIM werken met uw omgeving en beveiligingseisen. Volg deze stappen voor het bereiken van de bladeserver instellingen:

1. Aanmelden bij de [Azure portal](https://portal.azure.com/) en selecteer de tegel **Azure AD beschermde Identity Management** in het dashboard.
2. Selecteer **beheerde beschermde rollen** > **Instellingen** > **Instellingen voor meldingen**.

    ![Ga naar instellingen voor waarschuwingen][2]

### <a name="roles-are-being-activated-too-frequently-alert"></a>Waarschuwing 'Rollen worden geactiveerd te vaak'

Deze waarschuwing wordt geactiveerd als een gebruiker meerdere keren binnen een bepaalde termijn dezelfde bevoorrechte rol te activeren. U kunt zowel de periode en het aantal activeringen.

- **Activering vernieuwing tijdsbestek**: opgeven in dagen, uren, minuten en de periode die u gebruiken wilt voor het bijhouden van verdachte vernieuwingen seconde.

- **Aantal vernieuwingen activering**: Geef het aantal activeringen van 2 tot en met 100, die u tot slot melding binnen de periode die u hebt gekozen. U kunt deze instelling wijzigen door de schuifregelaar te verplaatsen of een getal in het tekstvak te typen.


### <a name="there-are-too-many-global-administrators-alert"></a>"Er zijn te veel beheerders van globale" waarschuwing

PIM geeft deze waarschuwing als twee verschillende criteria is voldaan en u ze kunt. Eerst moet u een bepaalde drempel van globale beheerders te bereiken. Ten tweede moet een bepaald percentage van uw totale roltoewijzingen globale beheerders. Als u alleen aan een van deze metingen wordt de waarschuwing niet weergegeven.  

- **Minimumaantal globale beheerders**: Geef het aantal globale beheerders, van 2 tot en met 100, dat u rekening houden met een bedrag dat onveilig.

- **Percentage van de globale beheerders**: het percentage van de beheerders die globale beheerders opgeven, van 0% tot 100%, dat is niet veilig in uw omgeving.

### <a name="administrators-arent-using-their-privileged-roles-alert"></a>Waarschuwing "Beheerders worden niet met behulp van hun bevoorrechte rol"

Deze waarschuwing wordt geactiveerd als een gebruiker een bepaalde hoeveelheid tijd gaat zonder te activeren, een rol.

- **Aantal dagen**: Geef het aantal dagen tussen 0 en 100, die een gebruiker kan gaan zonder het activeren van een rol.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_dash.png
[2]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_settings.png
