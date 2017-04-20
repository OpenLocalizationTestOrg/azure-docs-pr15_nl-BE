<properties
    pageTitle="Azure AD bevoorrechte Identity Management | Microsoft Azure"
    description="Een onderwerp waarin wordt uitgelegd wat Azure AD beschermde Identity Management is en het gebruik van PIM ter verbetering van de beveiliging van uw wolk."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="kgremban"/>

# <a name="azure-ad-privileged-identity-management"></a>Azure AD bevoorrechte Identity Management

U kunt met Azure Active Directory (AD) beschermde Identity Management, beheren en toegang controleren binnen uw organisatie. Deze service biedt toegang tot bronnen in Azure AD en andere Microsoft online services, zoals Office 365 of Microsoft Intune.  

> [AZURE.NOTE] Beschermde Identity Management is alleen beschikbaar bij de P2 Premium edition van Azure Active Directory. Zie [edities Azure Active Directory](active-directory-editions.md)voor meer informatie.

Organisaties wilt minimaliseren van het aantal personen die toegang tot beveiligde gegevens of middelen hebben, omdat die verkleint u de kans van een kwaadwillende gebruiker die toegang krijgen. Gebruikers moeten echter nog steeds om de begunstigde verrichtingen in Azure, Office 365 of SaaS-toepassingen uit te voeren. Organisaties geven gebruikers bevoorrechte toegang in Azure AD zonder toezicht wat gebruikers doen aan hun rechten admin. Azure rechten AD Identity Management helpt bij het oplossen van dit risico.  

Azure rechten AD Identity Management kunt u:  

- Zie AD Azure beheerders zijn
- On-demand 'just in time"administratieve toegang tot Microsoft Online Services, zoals Office 365 en Intune inschakelen
- Rapporten over de geschiedenis van beheerder toegang en wijzigingen in toewijzingen beheerder ophalen
- Ontvang berichten over de toegang tot een beschermde rol

Azure AD beschermde Identity Management kunt beheren de ingebouwde organisatie-Azure AD rollen, met inbegrip van:  

- Globale beheerder
- Factuuradres-beheerder
- De beheerder  
- Beheerder van gebruikers
- Wachtwoord beheerder

## <a name="just-in-time-administrator-access"></a>In tijd administrator-toegang

In het verleden, kan u een gebruiker toewijzen aan een rol admin via de Azure klassieke portal of Windows PowerShell. Als gevolg hiervan, wordt die gebruiker een **permanente admin**, altijd actief in de toegewezen rol. Azure rechten AD Identity Management introduceert het concept van een **in aanmerking komende admin**. In aanmerking komende beheerders moeten gebruikers die bevoorrechte toegang nodig hebt af en toe, maar niet elke dag. De rol is niet actief totdat de gebruiker toegang, moet zij een activering te voltooien en een actieve beheerder voor een vooraf bepaalde hoeveelheid tijd geworden.

## <a name="enable-privileged-identity-management-for-your-directory"></a>Beschermde Identity Management inschakelen voor uw map

U kunt starten met Azure AD beschermde identiteitsbeheer in de [portal Azure](https://portal.azure.com/).

>[AZURE.NOTE] U moet een globale beheerder met een organisatie account (bijvoorbeeld @yourdomain.com), niet in een Microsoft-account (bijvoorbeeld @outlook.com), Azure AD beschermde Identiteitsbeheer voor een map inschakelen.

1. Aanmelden bij de [Azure portal](https://portal.azure.com/) als globale beheerder van de map.
2. Als uw organisatie meer dan één map, selecteer uw gebruikersnaam in de rechterbovenhoek van de portal voor Azure. Selecteer de map waar u Azure AD beschermde Identity Management wordt gebruikt.
3. Selecteer **meer services** en de Filter textbox gebruiken om te zoeken naar **Azure AD beschermde Identity Management**.
4. **Pincode voor dashboard** controleren en klik vervolgens op **maken**. De beschermde Identity Management-toepassing wordt geopend.

Als u de eerste persoon Azure AD beschermde identiteitsbeheer in de map gebruikt, vervolgens helpt de [beveiligingswizard](active-directory-privileged-identity-management-security-wizard.md) u bij de eerste toewijzing ervaring. Daarna worden u automatisch de eerste **Beveiligingsbeheerder** en **bevoorrechte rol beheerder** van de map.

Alleen een bevoorrechte rol beheerder kan andere beheerders toegang beheren. U kunt [andere gebruikers de mogelijkheid om te beheren in PIM geven](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="privileged-identity-management-dashboard"></a>Beschermde Identity Management dashboard

Azure rechten AD id-beheer biedt een dashboard waarmee u belangrijke informatie, zoals:

- Waarschuwingen die wijzen op mogelijkheden om de beveiliging te verbeteren
- Het aantal gebruikers die zijn toegewezen aan elke rol beschermde  
- Het aantal in aanmerking komende en permanente beheerders
- Beoordelingen voor continue toegang

![PIM dashboard - screenshot][2]

## <a name="privileged-role-management"></a>Beheer van beschermde rollen

Met Azure AD beschermde Identity Management, kunt u de beheerders toevoegen of verwijderen van elke rol in aanmerking of de beheerders beheren.

![PIM-beheerders toevoegen of verwijderen - screenshot][3]

## <a name="configure-the-role-activation-settings"></a>De rol van activering-instellingen configureren

Met de [functie-instellingen](active-directory-privileged-identity-management-how-to-change-default-settings.md) kunt u de rol van in aanmerking komende activeringseigenschappen met inbegrip van:

- De duur van de activeringsperiode van rol
- De activering functie melding
- De gegevens die een gebruiker nodig heeft om te bieden tijdens het activeringsproces rol  

![Screenshot van PIM - beheerder activering - instellingen][4]

Houd er rekening mee dat in de afbeelding, worden de knoppen voor **Meerledige verificatie** uitgeschakeld. Voor bepaalde, zeer bevoorrecht rollen, MVR gesloten voor verhoogde bescherming vereist.

## <a name="role-activation"></a>Activering van de rol  

[Activeren van een rol](active-directory-privileged-identity-management-how-to-activate-role.md)vraagt een in aanmerking komende admin een tijd-gebonden "activering" voor de rol. De activering kan worden aangevraagd via de optie **activeren mijn rol** in Azure AD beschermde Identity Management.

Een beheerder die wil activeren een rol moet initialiseren Azure AD beschermde Identity Management in Azure portal.

Rol van productactivering kan worden aangepast. In de PIM-instellingen kunt u bepalen de lengte van de activering en wat informatie geven om de rol admin nodig is.

![PIM beheerder aanvraag rol activering - screenshot][5]

## <a name="review-role-activity"></a>Bekijk de activiteit van de rol

Er zijn twee manieren om te controleren hoe uw medewerkers en beheerders beschermde functies gebruikt. De eerste optie maakt gebruik van [Geschiedenis te controleren](active-directory-privileged-identity-management-how-to-use-audit-log.md). De controlegeschiedenis registreert wijzigingen bijhouden in beschermde roltoewijzingen en rol activering geschiedenis.

![PIM activering geschiedenis - screenshot][6]

De tweede is optie voor het instellen van regelmatige [toegang controleert](active-directory-privileged-identity-management-how-to-start-security-review.md). Deze beoordelingen toegang kunnen worden uitgevoerd door en toegewezen revisor (zoals een teammanager) of de werknemers zelf kunnen bekijken. Dit is de beste manier om te controleren die nog steeds toegang vereist is en wie niet meer doet.


## <a name="next-steps"></a>Volgende stappen
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_RoleActivationSettings.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png
