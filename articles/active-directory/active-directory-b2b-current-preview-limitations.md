<properties
   pageTitle="Huidige voorvertoning beperkingen voor samenwerking Azure Active Directory B2B | Microsoft Azure"
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
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-current-preview-limitations"></a>Azure AD B2B samenwerking voorbeeld: huidige beperkingen bekijken

- Meerledige verificatie (MVR gesloten) niet ondersteund voor externe gebruikers. Bijvoorbeeld als Contoso heeft de MVR gesloten, maar de Partner organigram niet, kunnen niet Partner Org gebruikers worden verleend MVR gesloten door B2B samenwerking.
- Nodigt zijn mogelijk alleen via CSV; afzonderlijke nodigt en API-toegang worden niet ondersteund.
- Azure AD globale beheerders kunnen alleen CSV-bestanden uploaden.
- Uitnodigingen voor de consument e-mailadressen (zoals hotmail.com, Gmail.com of comcast.net) worden momenteel niet ondersteund.
- Toegang tot externe gebruiker op-premises toepassingen niet getest.
- Externe gebruikers worden niet automatisch opgeschoond wanneer de werkelijke gebruiker wordt verwijderd uit de map.
- Uitnodigingen voor distributielijsten worden niet ondersteund.
- Maximaal 2.000 records u kunt uploaden via CSV.

## <a name="related-articles"></a>Verwante artikelen
Bezoek onze andere artikelen over Azure AD B2B samenwerking:

- [Wat is Azure AD B2B samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Hoe het werkt](active-directory-b2b-how-it-works.md)
- [Gedetailleerde scenario](active-directory-b2b-detailed-walkthrough.md)
- [Verwijzing naar een CSV-bestand opmaken](active-directory-b2b-references-csv-file-format.md)
- [Token-indeling van externe gebruiker](active-directory-b2b-references-external-user-token-format.md)
- [Wijzigingen van kenmerken object externe gebruiker](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
