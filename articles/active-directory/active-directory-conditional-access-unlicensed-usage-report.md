<properties
    pageTitle="Gebruik zonder licentie rapport | Microsoft Azure"
    description="Het gebruik zonder licentie rapport helpt die u identificeren illegale gebruikers die betaald Azure AD-functies."
    services="active-directory"
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

# <a name="unlicensed-usage-report"></a>Rapport gebruik zonder licentie

Het gebruik zonder licentie rapport helpt die u identificeren illegale gebruikers die betaald Azure AD-functies. Hierdoor kunt u beter het gebruik van licenties die u hebt gekocht en om aan te duiden weet wanneer u extra licenties moet. 

Het rapport bevat een actief gebruik van de betaalde functies in de afgelopen 30 dagen. 

## <a name="report-structure"></a>Het rapport
 
| Naam van de kolom          |    Beschrijving |
| :--                  | :--         |
| Niet-gelicentieerde gebruiker      |    Naam van de gebruiker |
| Functie              | De naam van de functie. Bijvoorbeeld: voorwaardelijke toegang |
| Toepassing geopend | De naam van de toepassing die wordt gebruikt met de functie. Bijvoorbeeld: Office 365 SharePoint Online |

 
> [AZURE.NOTE] Als een gebruikersaccount is verwijderd wordt de 'Niet-gelicentieerde gebruiker' kolom gevuld met een ID, zoals 1003000090D8B285


## <a name="conditional-access-feature"></a>Functie voor voorwaardelijke toegang

Gebruikers zonder licentie gemarkeerd wanneer zij toegang krijgen een service die voorwaardelijke toegangsbeleid toegepast tot als er geen een Azure AD Premium licentie heeft. 

Dit geldt voor de MVR gesloten / locatie beleid als apparaat beleid met Intune.
 

## <a name="see-also"></a>Zie ook

- [Met behulp van voorwaardelijke toegang en Office 365 en andere Azure Active Directory verbonden apps](active-directory-conditional-access.md)
- [Aan de slag met de voorwaardelijke toegang tot Azure AD](active-directory-conditional-access-azuread-connected-apps.md) 


