<properties
    pageTitle="Een enkel Azure AD huurder over Office 365 en Azure abonnementen | Microsoft Azure"
    description="Informatie over het delen van uw Office 365 Azure AD huurder en de gebruikers van uw abonnement Azure, of vice versa"
    services=""
    documentationCenter=""
    authors="JiangChen79"
    manager="mbaldwin"
    editor=""
    tags="billing,top-support-issue"/>

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="cjiang"/>

# <a name="use-an-existing-office-365-account-with-your-azure-subscription-or-vice-versa"></a>Een bestaand Office 365-account bij uw abonnement Azure of vice versa gebruiken
Scenario: U al een abonnement op Office 365 en klaar zijn voor een Azure-abonnement, maar u wilt de bestaande Office 365-gebruikersaccounts gebruiken voor uw abonnement op Azure. Ook een Azure-abonnee bent en wilt u een abonnement op Office 365 voor de gebruikers in uw bestaande Azure Active Directory. In dit artikel ziet u hoe eenvoudig het is om beide te bereiken.

> [AZURE.NOTE] In dit artikel geldt niet voor klanten met Enterprise-overeenkomst (EA). Als u meer hulp op elk punt in dit artikel, [Neem contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) nodig om het probleem snel opgelost.


## <a name="quick-guidance"></a>Snelle hulp

- Als u al een Office 365-abonnement hebt en u wilt aanmelden voor Azure, moet u de optie **aanmelden met uw organisatie-account** gebruiken. Vervolgens doorgaan met de Azure aanmelden met uw account voor Office 365. Zie de [gedetailleerde procedure verderop in dit artikel](#s1).

- Als u al een abonnement op Azure en op te halen van een Office 365-abonnement, aanmelden bij Office 365 met uw account Azure. Ga vervolgens verder met de aanmelding stappen. Nadat u de aanmelding hebt voltooid, wordt het Office 365-abonnement wordt toegevoegd aan hetzelfde exemplaar Azure Active Directory waartoe uw abonnement Azure. Zie de sectie [gedetailleerde stappen verderop in dit artikel](#s2)voor meer informatie.

>[AZURE.NOTE] Als u een Office 365-abonnement, de account u gebruikt voor aanmelding moet lid zijn van de rol van globale beheer of facturering Admin map in uw huurder Azure Active Directory. [Meer informatie over het bepalen van de rol in Azure Active Directory](#how-to-know-your-role-in-your-azure-active-directory).

Om te begrijpen wat er gebeurt wanneer u een abonnement op een account toevoegen, de [achtergrondinformatie verderop in het artikel](#background-information)te zien.

## <a name="detailed-steps"></a>Gedetailleerde stappen
<a id="s1"></a>
### <a name="scenario-1-office-365-users-who-plan-to-buy-azure"></a>Scenario 1: De Office 365-gebruikers die van plan te bent kopen Azure
In dit scenario, nemen we aan dat Kelley Wall is een gebruiker die een Office 365-abonnement heeft, en is van plan om u te abonneren op Azure. Er zijn twee extra actieve gebruikers, Jeroen en Tricia. Kelley van rekening is admin@contoso.onmicrosoft.com.

![Office 365 gebruiker admin center](./media/billing-use-existing-office-365-account-azure-subscription/1-office365-users-admin-center.png)

Als u wilt aanmelden voor Azure, volg deze stappen:

1. Aanmelden voor Azure op [Azure.com](https://azure.microsoft.com/). Klik op **gratis proberen**. Klik op **nu starten**op de volgende pagina.

    ![Probeer gratis Azure.](./media/billing-use-existing-office-365-account-azure-subscription/2-azure-signup-try-free.png)

2. Klik op **aanmelden met de account van uw organisatie**.

    ![Aanmelden bij Azure.](./media/billing-use-existing-office-365-account-azure-subscription/3-sign-in-to-azure.png)

3. Log in met uw account voor Office 365. In dit geval is het Kelley van Office 365-account.

    ![Log in met uw account voor Office 365.](./media/billing-use-existing-office-365-account-azure-subscription/4-sign-in-with-org-account.png)

4. De gegevens in te vullen en het aanmeldingsproces te voltooien.

    ![Gegevens in te vullen en de registratie hebt voltooid.](./media/billing-use-existing-office-365-account-azure-subscription/5-azure-sign-up-fill-information.png)

    ![Klik op Start mijn service beheren.](./media/billing-use-existing-office-365-account-azure-subscription/6-azure-start-managing-my-service.png)

Nu bent u klaar. U ziet in het portal voor Azure dezelfde gebruikers wordt weergegeven. U kunt dit controleren door de volgende stappen uit:

1. Klik op **Start mijn service beheren** in het scherm eerder weergegeven.
2. Klik op **Bladeren**en klik vervolgens op **Active Directory**.

    ![Klik op Bladeren en klik vervolgens op Active Directory.](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)

3. Klik op **gebruikers**.

    ![Het tabblad gebruikers](./media/billing-use-existing-office-365-account-azure-subscription/8-azure-portal-ad-users-tab.png)

4. Alle gebruikers, met inbegrip van Kelley, worden weergegeven zoals verwacht.

    ![Lijst met gebruikers](./media/billing-use-existing-office-365-account-azure-subscription/9-azure-portal-ad-users.png)

<a id="s2"></a>
### <a name="scenario-2-azure-users-who-plan-to-buy-office-365"></a>Scenario 2: Azure gebruikers die van plan zijn te kopen van Office 365

In dit scenario Kelley Wall is een gebruiker die een abonnement Azure onder de account heeft admin@contoso.onmicrosoft.com. Kelley wil abonneren op Office 365 en ze al met Azure heeft dezelfde map te gebruiken.

>[AZURE.NOTE] Als u een Office 365-abonnement, moet de account die u gebruikt voor aanmelding bij een lid van de rol van globale beheer of facturering Admin map in uw huurder Azure Active Directory. [Meer informatie over het op de hoogte van de rol in Azure Active Directory](#how-to-know-your-role-in-your-azure-active-directory).

![Azure portal abonnementsinstellingen](./media/billing-use-existing-office-365-account-azure-subscription/10-azure-portal-settings-subscription.png)

![Portal Azure Active Directory-gebruikers](./media/billing-use-existing-office-365-account-azure-subscription/11-azure-portal-ads-users.png)

Abonneren op Office 365, volg deze stappen:

1. Ga naar de [pagina Office 365-product](https://products.office.com/business), en selecteer vervolgens een indeling die geschikt is voor u.
2. Nadat u het plan hebt geselecteerd, wordt de volgende pagina wordt weergegeven. Vul het formulier. Klik op **aanmelden** in de rechterbovenhoek van de pagina.

    ![Testversie Office 365-pagina](./media/billing-use-existing-office-365-account-azure-subscription/12-office-365-trial-page.png)

3. Log in met uw referenties. In dit voorbeeld is de Kelley account.

    ![Aanmelden bij Office 365](./media/billing-use-existing-office-365-account-azure-subscription/13-office-365-sign-in.png)

4. Klik op **nu proberen**.

    ![Bevestig uw order voor Office 365.](./media/billing-use-existing-office-365-account-azure-subscription/14-office-365-confirm-your-order.png)

5. Klik op **Doorgaan**op de pagina bestelling ontvangen.

    ![Office 365 ontvangst](./media/billing-use-existing-office-365-account-azure-subscription/15-office-365-order-receipt.png)

Nu bent u klaar. In het Office 365 admin center ziet u de gebruikers van Contoso weergegeven als gebruikers van active directory. U kunt dit controleren door de volgende stappen uit:

1. Open het Office 365 admin center.
2. Vouw **gebruikers**en klik op **Actieve gebruikers**.

    ![Gebruikers van Office 365 admin center](./media/billing-use-existing-office-365-account-azure-subscription/16-office-365-admin-center-users.png)

### <a name="how-to-know-your-role-in-your-azure-active-directory"></a>Hoe weet u uw rol in Azure Active Directory

1. Log in om de [Azure portal](https://portal.azure.com/).
2. Klik op **Bladeren**en klik vervolgens op **Active Directory**.

    ![Active Directory in de portal voor Azure](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)

3. Klik op **gebruikers**.

    ![Azure portal standaard Active Directory-gebruikers](./media/billing-use-existing-office-365-account-azure-subscription/17-azure-portal-default-ad-users.png)

4. Klik op de gebruiker. In dit voorbeeld wordt de gebruiker Kelley Wall.

    Let op het gebied van de **Rol binnen de organisatie**.

    ![Azure portal-gebruikers-id](./media/billing-use-existing-office-365-account-azure-subscription/18-azure-portal-user-identity.png)

## <a name="background-information-about-azure-and-office-365-subscriptions"></a>Achtergrondinformatie over Azure en Office 365-abonnementen
Office 365 en Azure gebruiken de Azure Active Directory-service voor het beheren van gebruikers en abonnementen. Een directory Azure beschouwen als een container waarin u gebruikers en abonnementen kunt groeperen. U moet dezelfde gebruikersaccount gebruikt voor uw Azure en Office 365-abonnementen, om ervoor te zorgen dat de abonnementen worden gemaakt in dezelfde map. Houd de volgende punten:

- Een abonnement wordt gemaakt in een map, niet andersom.
- Gebruikers behoren tot de mappen en niet andersom.
- Een abonnement belandt in de map van de gebruiker die het abonnement maakt. Uw abonnement op Office 365 is daarom gekoppeld aan hetzelfde account als uw abonnement Azure wanneer u het Office 365-abonnement maken met die account.

![Achtergrondinformatie](./media/billing-use-existing-office-365-account-azure-subscription/19-background-information.png)

Zie [hoe Azure abonnementen zijn gekoppeld aan Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md)voor meer informatie.

>[AZURE.NOTE] Azure abonnementen zijn eigendom van afzonderlijke gebruikers in de directory.

>[AZURE.NOTE] Office 365-abonnementen zijn eigendom van de map zelf. Als gebruikers in de directory met de vereiste machtigingen, kunnen deze worden toegepast op deze abonnementen.

## <a name="next-steps"></a>Volgende stappen
Als u uw Azure en Office 365-abonnementen apart hebt aangeschaft in het verleden en u wilt toegang tot de Office 365-huurder van het abonnement Azure, Zie [een huurder Office 365 met een Azure abonnement koppelen](billing-add-office-365-tenant-to-azure-subscription.md).

> [AZURE.NOTE] Als u nog vragen hebt, [Neem contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) voor uw probleem snel opgelost.
