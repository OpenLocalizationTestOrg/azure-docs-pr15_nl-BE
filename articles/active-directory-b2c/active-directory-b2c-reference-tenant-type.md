<properties
    pageTitle="Azure Active Directory B2C: Productie-schaal versus voorbeeld B2C huurders | Microsoft Azure"
    description="Een onderwerp over de verschillende typen Azure Active Directory B2C huurders"
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
    ms.date="08/30/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-production-scale-vs-preview-b2c-tenants"></a>Azure Active Directory B2C: Productie-schaal versus voorbeeld B2C huurders

Als u van plan bent een app productie van B2C Azure Active Directory (AD Azure) te schrijven, moet u er zeker van zijn dat u beschikt over de juiste huurder "type" gaat live op. Om te zien wat u hebt, als volgt te werk om te [Navigeren naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) op de Azure portal en kijk onder **huurder type**.

## <a name="summary"></a>Samenvatting

Azure AD B2C ondersteunt productie apps alleen op **productie-schaal** B2C huurders in Noord-Amerika.

| Type van de huurder | Landen/regio 's | In het algemeen-beschikbaar? |
| ----------- | -------------- | --------------------- |
| **Productie-schaal huurder** | Noord-Amerikaanse landen | Ja |
| **Productie-schaal huurder** | Alle landen/regio's, met uitzondering van Noord-Amerika | Nee |
| **Voorbeeld van huurder** | Alle landen/regio 's | Nee |

> [AZURE.NOTE]
Azure AD B2C huurders (voor consumenten) zijn momenteel niet beschikbaar in enkele landen of regio's waar huurders Azure AD (voor werknemers) beschikbaar zijn. Lees de volgende secties voor meer informatie.

## <a name="production-scale-b2c-tenant-in-north-america"></a>Productie-schaal B2C huurder in Noord-Amerika

Als [uw huurder B2C gemaakt](active-directory-b2c-get-started.md) in Noord-Amerika, dat wil zeggen, in een van de volgende landen of regio's: de Verenigde Staten, Canada, Costa Rica, Dominicaanse Republiek, El Salvador, Guatemala, Mexico, Panama, Puerto Rico en Trinidad en Tobago, en de **huurder type** op uw B2C Admin UI zegt **productie-schaal**, de huurder kan worden gebruikt voor de productie-apps.

> [AZURE.NOTE]
Productie-schaal huurders zijn kunnen schalen naar 100s van miljoenen consumenten identiteiten per huurder.

![Schermafdruk van een huurder productie-schaal](./media/active-directory-b2c-reference-tenant-type/production-scale-b2c-tenant.png)

## <a name="preview-b2c-tenant-in-any-countryregion"></a>Voorbeeld B2C huurder in elk land/regio

Als u een huurder B2C had gemaakt tijdens de testperiode Azure AD B2C, is het waarschijnlijk dat de **huurder Typ** **huurder bekijken zegt**. Als dit het geval is, moet u de huurder alleen voor ontwikkel- en testdoeleinden en niet voor productie toepassingen gebruiken.

> [AZURE.IMPORTANT]
Er is geen migratiepad van een voorbeeld B2C huurder naar een productie-schaal B2C huurder. Opmerking: Er zijn bekende problemen wanneer u een voorvertoning B2C huurder verwijderen en opnieuw maken van een productie-schaal B2C huurder met dezelfde domeinnaam. U moet een huurder productie schaal B2C maken met een andere domeinnaam.

![Schermafdruk van een huurder preview](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)

## <a name="production-scale-b2c-tenant-outside-of-north-america"></a>Productie-schaal B2C huurder buiten Noord-Amerika

Azure AD B2C is momenteel niet in het algemeen-beschikbaar buiten Noord-Amerika. Maar u kunt maken en productie-schaal huurders, gebruiken voor het ontwikkelen en testen van doeleinden in een van de volgende landen of regio's: Algerije, Oostenrijk, Azerbeidzjan, Bahrein, Belarus, België, Bulgarije, Kroatië, Cyprus, Tsjechië, Denemarken, Egypte, Estland, Finland, Frankrijk, Duitsland, Griekenland, Hongarije, IJsland, Ierland, Israël, Italië, Jordanië, Kazachstan, Kenia, Koeweit, Letland, Libanon, Liechtenstein, NNNNNN, Luxemburg, Voormalige Joegoslavische Republiek Macedonië, Malta, Montenegro, Marokko, Nederland, Nigeria, Noorwegen , Oman, Pakistan, Polen, Portugal, Qatar, Roemenië, Rusland, Saudi-Arabië, Servië, Slowakije, Slovenië, Zuid-Afrika, Spanje, Zweden, Zwitserland, Tunesië, Turkije, Oekraïne, Verenigde Arabische Emiraten en het Verenigd Koninkrijk.

Zodra de Azure AD B2C kondigt de algemene beschikbaarheid in bovenstaande landen of regio's, kunt u blijven gebruiken deze productie schaal huurders en go-live met uw productie-apps zonder dat er gegevens verloren.

## <a name="availability-of-b2c-tenants"></a>Beschikbaarheid van B2C huurders

B2C huurders zijn momenteel niet beschikbaar in de volgende landen: Afghanistan, Argentinië, Australië, Brazilië, Chili, Colombia, Ecuador, Hong Kong SAR, India, Indonesië, Irak, Japan, Korea, Maleisië, Nieuw-Zeeland, Paraguay, Peru, Filippijnen, Singapore, Sri Lanka, Taiwan, Thailand, Uruguay en Venezuela. We willen opnemen in de toekomst.
