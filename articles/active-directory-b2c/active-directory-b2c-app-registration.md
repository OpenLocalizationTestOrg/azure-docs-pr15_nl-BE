<properties
    pageTitle="Azure Active Directory B2C: Registratie toepassing | Microsoft Azure"
    description="Het registreren van uw toepassing met Azure Active Directory B2C"
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
    ms.topic="get-started-article"
    ms.date="08/30/2016"
    ms.author="swkrish"/>


# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: Uw toepassing registreren

## <a name="prerequisite"></a>Vereiste

Als u een toepassing die de consument-aanmelding en -in accepteert, moet u eerst de toepassing met een huurder Azure Active Directory B2C registreren. Uw eigen huurder ophalen met behulp van de stappen in het [maken van een huurder Azure AD B2C](active-directory-b2c-get-started.md). Nadat u de stappen in dit artikel volgt, hebt u de B2C functies blade vastgemaakt aan de Startboard.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="navigate-to-the-b2c-features-blade"></a>Ga naar de blade B2C-functies

Als u de B2C functies blade vastgemaakt aan de Startboard hebt, ziet u het blad zodra u bij de [Azure portal](https://portal.azure.com/) als globale beheerder van de pachter B2C aanmelden.

U kunt het blad ook openen door te klikken op **Bladeren** en **Azure AD B2C** in het linkernavigatievenster op de [Azure portal](https://portal.azure.com/).

> [AZURE.IMPORTANT] U moet een globale beheerder van de pachter B2C kunnen toegang krijgen tot de functies B2C blade. Een globale beheerder van een andere huurder of een gebruiker van een huurder niet kan openen.  U kunt overschakelen naar de huurder B2C met behulp van de huurder schakelbaar in de rechterbovenhoek van de Portal Azure.

## <a name="register-an-application"></a>Een toepassing registreren

1. Klik op het blad van de functies B2C in Azure portal **toepassingen**.
2. Klik op **+ toevoegen** aan de bovenkant van het blad.
3. Typ een **naam** voor de toepassing waarin u uw toepassing aan de consument. U kunt bijvoorbeeld "Contoso B2C app" invoeren.
4. Als u een webgebaseerde toepassing schrijft, schakelen de schakeloptie **web app of web-API** op **Ja**. De **URL's antwoord** zijn eindpunten waarbij Azure AD B2C retourneert de tokens die uw toepassing aanvraagt. Voer bijvoorbeeld `https://localhost:44321/`. Als uw webtoepassing ook sommige web beveiligd door Azure AD B2C API aanroept wordt, moet u een **Geheim toepassing** ook maken door te klikken op de knop **Sleutel genereren** .

    > [AZURE.NOTE] Een **Toepassing geheim** is een belangrijke en op de juiste wijze moet worden beveiligd.

5. Als u een mobiele toepassing schrijft, schakelen de schakeloptie voor **native client opnemen** op **Ja**. De standaardwaarde wordt automatisch voor u gemaakt **Redirect URI** kopiëren.
6. Klik op **maken** om de toepassing te registreren.
7. Klik op de toepassing die u zojuist hebt gemaakt en u globaal unieke **Client-toepassings-ID** waarmee u later in de code te kopiëren.

> [AZURE.IMPORTANT] Toepassingen die zijn gemaakt in het blad B2C functies moeten op dezelfde locatie wordt beheerd. B2C als u toepassingen met PowerShell of een andere portal, ze worden niet ondersteund en werkt waarschijnlijk niet met Azure AD B2C.

## <a name="build-a-quick-start-application"></a>Een Quick Start-toepassing maken

Nu dat u een toepassing met Azure AD B2C geregistreerd hebt, kunt u een van onze quick start zelfstudie aan de slag te kunnen voltooien. Hier volgen enkele aanbevelingen:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]
