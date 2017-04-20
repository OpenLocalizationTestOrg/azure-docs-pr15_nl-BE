<properties
    pageTitle="Azure Active Directory identiteitsbeveiliging meldingen | Microsoft Azure"
    description="Informatie over hoe meldingen ondersteunen uw onderzoeksactiviteiten."
    services="active-directory"
    keywords="Azure active directory identiteitsbeveiliging, cloud app discovery, toepassingen, beveiliging, risico's, risiconiveau, beveiligingslek, beveiligingsbeleid beheren"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory identiteitsbeveiliging meldingen 


Azure AD identiteitsbeveiliging verzendt twee soorten automatische e-mailberichten voor het risico van de gebruiker en de risico's beheren:

- Gebruiker gemanipuleerde e-mailwaarschuwingen

- Wekelijks overzicht via e-mail

## <a name="user-compromised-alert-email"></a>Gebruiker gemanipuleerde e-mailwaarschuwingen

Een waarschuwing voor een gebruiker schadelijke e-mailbericht wordt gegenereerd wanneer identiteitsbeveiliging Azure AD een account wordt geïdentificeerd als gevaar lopen. Het e-mailbericht bevat een koppeling naar de gebruikers die zijn gemarkeerd voor risico-rapport in het dashboard voor bescherming van identiteiten. Het beste direct kennisgevingen te onderzoeken in gevaar gebracht.


## <a name="weekly-digest-email"></a>Wekelijks overzicht via e-mail

Wekelijks overzicht via e-mail bevat een overzicht van nieuwe risico's.<br>
Het bevat:

- Gebruikers die een risico
- Verdachte activiteiten
- Beveiligingslekken aangetroffen
- Koppelingen naar gerelateerde lijsten in de bescherming van de identiteit


<br>
![Herstel](./media/active-directory-identityprotection-notifications/400.png "Remediation")
<br> 

U kunt schakelen een wekelijks overzicht via e-mail verzenden uit.
<br><br>
![Risico's van gebruiker](./media/active-directory-identityprotection-notifications/62.png "User risks")
<br>
 

**Het bijbehorende dialoogvenster openen**:

1. Klik op **Instellingen**op het blad **identiteitsbeveiliging Azure AD** .
<br><br>
![Gebruikersbeleid voor risico](./media/active-directory-identityprotection-notifications/401.png "User risk policy")
<br>

2. Klik in de sectie **Algemeen** **meldingen**.
<br><br>
![Gebruikersbeleid voor risico](./media/active-directory-identityprotection-notifications/405.png "User risk policy")
<br>




## <a name="see-also"></a>Zie ook

- [Identiteitsbeveiliging Azure Active Directory](active-directory-identityprotection.md) 

