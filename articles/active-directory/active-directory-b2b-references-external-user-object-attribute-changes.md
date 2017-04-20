<properties
   pageTitle="Externe gebruiker object kenmerkwijzigingen Azure Active Directory B2B samenwerking bekijken | Microsoft Azure"
   description="Azure Active Directory B2B ondersteunt uw relaties tussen bedrijven met zakenpartners selectief toegang krijgen tot uw zakelijke toepassingen inschakelen"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-external-user-object-attribute-changes"></a>Azure AD B2B samenwerking voorbeeld: wijzigingen van kenmerken object externe gebruiker

Elke gebruiker in een directory Azure AD wordt vertegenwoordigd door een user-object. Het gebruikersobject in Azure AD ondergaat wijzigingen van kenmerken in verschillende stadia van de samenwerking met B2B invite-inwisselen stroom. De gebruiker dat de gebruiker in Active directory heeft kenmerken die veranderen op tijd, wanneer de gebruiker klikt op de koppeling in de e-mail uitnodigen inwisselen. Met name:

- De kenmerken **SignInName** en **AltSecId** worden gevuld.
- Het kenmerk **DisplayName** wordt gewijzigd van de UPN-naam (user_fabrikam.com#EXT#@contoso.com) met de naam(user@fabrikam.com)

Bijhouden van deze kenmerken in Azure AD kunt u problemen oplossen met het al dan niet een gebruiker partner hun samenwerking B2B uitnodiging heeft ingeleverd.

## <a name="related-articles"></a>Verwante artikelen
Bezoek onze andere artikelen over Azure AD B2B samenwerking:

- [Wat is Azure AD B2B samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Hoe het werkt](active-directory-b2b-how-it-works.md)
- [Gedetailleerde scenario](active-directory-b2b-detailed-walkthrough.md)
- [Verwijzing naar een CSV-bestand opmaken](active-directory-b2b-references-csv-file-format.md)
- [Token-indeling van externe gebruiker](active-directory-b2b-references-external-user-token-format.md)
- [Huidige voorvertoning beperkingen](active-directory-b2b-current-preview-limitations.md)
- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
