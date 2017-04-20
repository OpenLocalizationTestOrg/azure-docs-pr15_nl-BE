<properties
    pageTitle="v2.0 app registratie | Microsoft Azure"
    description="Het registreren van een app met Microsoft voor het inschakelen van aanmelden en toegang tot Microsoft-services met behulp van het eindpunt v2.0"
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="how-to-register-an-app-with-the-v20-endpoint"></a>Het registreren van een app aan het eindpunt v2.0

Voor het opbouwen van een app die zowel MSA & Azure AD accepteert aanmelden, u moet eerst een app te registreren bij Microsoft.  Op dit moment niet mogelijk alle bestaande apps met Azure AD gebruiken of MSA - moet u een nieuwe opmerking te maken.

> [AZURE.NOTE]
    Niet alle scenario's Azure Active Directory en functies worden ondersteund door het eindpunt v2.0.  Om te bepalen als u het eindpunt v2.0 moet gebruiken, Lees over [v2.0 beperkingen](active-directory-v2-limitations.md).

## <a name="visit-the-microsoft-app-registration-portal"></a>Ga naar de Microsoft-portal Registratie van app
Eerste dingen eerst - gaat u naar [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Dit is een nieuwe app-portal Registratie van waar u uw Microsoft-toepassingen kunt beheren.

Aanmelden met een een persoonlijke of werk of school Microsoft-account.  Als u deze niet hebt, moet u zich aanmelden voor een nieuwe persoonlijke account. Ga verder, won't duurt het lang - we hier zult wachten.

Gedaan? U moet nu kijken op uw lijst met Microsoft-apps die waarschijnlijk leeg is.  We gaan wijzigen.

Klik op **een app toevoegen**en een naam geven.  De portal toewijzen uw app een globaal unieke toepassings-Id waarmee u verderop in uw code.  Als uw app een component op de server bevat die moet de toegangstokens voor aanroepen API's (denkt: Office, Azure of uw eigen web API), zult u ook een **Geheim toepassing** hier maken.
<!-- TODO: Link for app secrets -->

Voeg vervolgens de Platforms die door uw toepassing wordt gebruikt.

- Voor toepassingen op het web, bieden een **Redirect URI** waar-in berichten kunnen worden verzonden.
- Kopiëren naar de standaard redirect uri automatisch voor u gemaakt voor mobiele toepassingen.

Desgewenst kunt u het uiterlijk van uw pagina in de sectie van het profiel aanpassen.  Zorg ervoor dat u **opslaat** voordat u doorgaat.

> [AZURE.NOTE] Wanneer u een toepassing met [https://apps.dev.microsoft.com/?deeplink=/appList maken](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), kan de toepassing zal worden geregistreerd in de home pachter van de account waarmee u kunt inloggen op de portal.  Dit betekent dat u een toepassing niet kunt registreren in uw Azure AD huurder met behulp van een persoonlijke Microsoft-account.  Als u expliciet registreren van een toepassing in een bepaalde huurder wilt, aanmelden met een account die de huurder oorspronkelijk gemaakt.

## <a name="build-a-quick-start-app"></a>Een snelle start app bouwen
Nu dat er een Microsoft-toepassing, kunt u een van onze zelfstudies v2.0 snel starten te voltooien.  Hier volgen enkele aanbevelingen:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]
