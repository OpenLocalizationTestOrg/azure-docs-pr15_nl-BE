<properties
    pageTitle="Kan niet aanmelden bij Azure abonnement | Microsoft Azure"
    description="Wordt beschreven hoe u enkele algemene problemen Azure abonnement login."
    services=""
    documentationCenter=""
    authors="genlin"
    manager="mbaldwin"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="genli"/>

# <a name="i-cant-sign-in-to-manage-my-azure-subscription"></a>Ik kan niet aanmelden bij mijn Azure abonnement beheren

Dit artikel begeleidt u door enkele van de meest voorkomende methoden login problemen op te lossen.

## <a name="page-hangs-in-the-loading-status"></a>Pagina reageert niet meer in de laadstatus

Als uw internet-browserpagina vastloopt, kunt u elk van de volgende stappen uit totdat u kunt de [Azure portal](https://portal.azure.com).

-   Vernieuw de pagina.
-   Gebruik een andere internetbrowser.
-   Als u Microsoft Internet Explorer gebruikt, Ga naar de portal Azure via de InPrivate-navigatie surfen modus. 

    A.  Klik op **Extra** ![knop Extra](./media/billing-cannot-login-subscription/Toolsbutton.png) > **veiligheid** > **Gewenst**.

    B.  Ga naar de [portal Azure](https://portal.azure.com)en meldt u zich aan de portal.

## <a name="error-message-no-subscriptions-found"></a>Foutbericht "Geen abonnementen gevonden"

Als uw account geen machtigingen heeft, wordt er een foutbericht **geen abonnement gevonden** . Alleen de accountbeheerder van een kunt krijgen naar het [Midden van de Account](https://account.windowsazure.com/)niet in de servicebeheerders (SA) of CO-beheerders (CA).

**Scenario 1: Foutbericht wordt ontvangen in de [Azure portal](https://portal.azure.com)**

Dit probleem op te lossen, [de rol van CO-beheerder of eigenaar toevoegen](billing-add-change-azure-subscription-administrator.md) voor de account.

**Scenario 2: Foutbericht wordt in het [Midden van Azure-Account](https://account.windowsazure.com/Subscriptions) ontvangen.**

Controleer of de account die u gebruikt de accountbeheerder. Als u wilt controleren of de account administrator is, de volgende stappen uit:

1.  Log in om de [Azure portal](https://portal.azure.com).
2.  Selecteer in het menu Hub **abonnement**.
3.  Selecteer het abonnement dat u wilt controleren en selecteer **Instellingen**.
4.  Selecteer **Eigenschappen**. De beheerder van de rekening van het abonnement wordt weergegeven in het vak **Account Admin** .

## <a name="you-are-automatically-signed-in-as-a-different-user"></a>U bent automatisch aangemeld als een andere gebruiker

Dit probleem kan optreden als u meer dan één gebruikersaccount in een Internet-browser gebruikt.

Probeer een van de volgende methoden het probleem op te lossen:

-   Schakel de cache en internetcookies verwijderen. Klik in Internet Explorer op **Extra** ![knop Extra](./media/billing-cannot-login-subscription/Toolsbutton.png) > **Internet-opties** > **verwijderen**. Zorg ervoor dat de selectievakjes uit voor de tijdelijke bestanden, cookies, wachtwoord en browsegeschiedenis zijn geselecteerd en klik vervolgens op verwijderen.

-   De standaardinstellingen van de Internet Explorer als u persoonlijke instellingen die u hebt aangebracht. Klik op **Extra** ![knop Extra](./media/billing-cannot-login-subscription/Toolsbutton.png)> **Internet-opties** > **Geavanceerd** > het **verwijderen van persoonlijke instellingen** > **opnieuw**.

-   Ga naar de Azure portal in InPrivate-navigatie surfen modus. Klik op **Extra** ![knop Extra](./media/billing-cannot-login-subscription/Toolsbutton.png) > **veiligheid** > **Gewenst**.

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met support. 

Als u nog steeds hulp nodig hebt, [Neem contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) voor uw probleem snel opgelost. 