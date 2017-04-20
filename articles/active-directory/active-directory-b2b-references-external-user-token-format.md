<properties
   pageTitle="Externe gebruiker token indeling voor Azure Active Directory B2B samenwerking preview | Microsoft Azure"
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

# <a name="azure-ad-b2b-collaboration-preview-external-user-token-format"></a>Azure AD B2B samenwerking voorbeeld: token-indeling van externe gebruiker

De claims voor een standaard Azure AD token worden beschreven in het artikel [ondersteund Token en claimtypen](active-directory-token-and-claims.md) op azure.microsoft.com.

De claims die verschillend voor een geverifieerde externe gebruiker voor B2B samenwerking zijn zijn als volgt:<br/>
- **OID:** de object-ID van de resource-huurder<br/>
- **TID**: ID van de resource huurder pachters<br/>
- **Uitgever**: dit is de huurder resource<br/>
- **IDP**: dit is de huurder basismap van de gebruiker<br/>
- **AltSecId**: dit is de alternatieve beveiligings-ID, dat u dekkend<br/>

## <a name="related-articles"></a>Verwante artikelen
Bezoek onze andere artikelen over Azure AD B2B samenwerking:

- [Wat is Azure AD B2B samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Hoe het werkt](active-directory-b2b-how-it-works.md)
- [Gedetailleerde scenario](active-directory-b2b-detailed-walkthrough.md)
- [Verwijzing naar een CSV-bestand opmaken](active-directory-b2b-references-csv-file-format.md)
- [Wijzigingen van kenmerken object externe gebruiker](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Huidige voorvertoning beperkingen](active-directory-b2b-current-preview-limitations.md)
- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
