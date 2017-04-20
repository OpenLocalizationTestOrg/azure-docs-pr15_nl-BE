<properties
    pageTitle="Azure Active Directory B2C: Kenmerken van aangepast veld | Microsoft Azure"
    description="Het gebruik van aangepaste kenmerken in Azure Active Directory B2C informatie verzamelen over uw consumenten"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

#  <a name="azure-active-directory-b2c-use-custom-attributes-to-collect-information-about-your-consumers"></a>Azure Active Directory B2C: Aangepaste kenmerken gebruiken voor het verzamelen van informatie over de consumenten

De map B2C Azure Active Directory (AD Azure) wordt geleverd met een ingebouwde reeks informatie (kenmerken): voornaam, achternaam, plaats, postcode en andere kenmerken. Elke consument gerichte toepassing heeft echter op welke kenmerken unieke vereisten voor het verzamelen van de consument. Met Azure AD B2C, kunt u de set kenmerken die zijn opgeslagen op elke rekening van de consument uitbreiden. U kunt aangepaste kenmerken die op de [Azure portal](https://portal.azure.com/) maken en gebruiken in uw aanmelding beleid, zoals hieronder wordt weergegeven. U kunt ook lezen en schrijven van deze kenmerken met de [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).

> [AZURE.NOTE]
Aangepaste kenmerken gebruiken [Azure AD Graph API Directory Schema-uitbreidingen](https://msdn.microsoft.com/library/azure/dn720459.aspx).

## <a name="create-a-custom-attribute"></a>Maak een aangepast attribuut

1. [Volg deze stappen om te navigeren naar de B2C functies blade van de Azure portal](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klik op de **kenmerken van de gebruiker**.
3. Klik op **+ toevoegen** aan de bovenkant van het blad.
4. Geef een **naam** voor het aangepaste kenmerk (bijvoorbeeld "ShoeSize") en eventueel een **Beschrijving**. Klik op **maken**.

    > [AZURE.NOTE]
    Alleen de 'String'- **Gegevenstype** is momenteel beschikbaar.

Het aangepaste kenmerk is nu beschikbaar in de lijst van **kenmerken van de gebruiker**en voor gebruik in uw aanmelding beleid.

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a>Een aangepast kenmerk gebruiken in uw aanmelding beleid

1. [Volg deze stappen om te navigeren naar de B2C functies blade van de Azure portal](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klik op **aanmelden bij beleid**.
3. Klik op uw aanmelding beleid (bijvoorbeeld ' B2C_1_SiUp') om deze te openen. Klik op **bewerken** op de bovenkant van het blad.
4. Klik op **aanmelden bij kenmerken** en selecteert u het aangepaste kenmerk (bijvoorbeeld "ShoeSize"). Klik op **OK**.
5. **Vorderingen van toepassing** op en selecteert u het aangepaste kenmerk. Klik op **OK**.
6. Klik op **Opslaan** op de bovenkant van het blad.

U kunt de functie 'Direct uitvoeren' in het beleid om te controleren of de ervaring van de consument. U moet nu Zie "ShoeSize" in de lijst met kenmerken die zijn verzameld tijdens het aanmelden bij consument en het in het token terug naar de toepassing verzonden.

## <a name="notes"></a>Notities

- Aanmelding beleid, samen met de worden aangepaste kenmerken ook gebruikt in aanmelden of aanmelden beleid en profiel bewerken van beleid.
- Er is een bekende beperking van aangepaste kenmerken. Het is alleen de eerste keer dat het wordt gebruikt in elk beleid en niet wanneer u deze toevoegt aan de lijst met **kenmerken van de gebruiker**gemaakt.
