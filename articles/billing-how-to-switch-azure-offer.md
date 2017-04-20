<properties
    pageTitle="Uw abonnement op Azure overschakelen naar een ander voorstel | Microsoft Azure"
    description="Meer informatie over het wijzigen van uw abonnement Azure en overschakelen naar een ander voorstel met het abonnement management portal"
    services=""
    documentationCenter=""
    authors="genlin"
    manager="mbaldwin"
    editor=""
    tags="billing,top-support-issue"/>

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="genli"/>

# <a name="switch-your-azure-subscription-to-another-offer"></a>Uw abonnement op Azure overschakelen naar een ander voorstel

Als een klant [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) u mogelijk uw abonnement Azure overschakelen naar een ander voorstel in het [Midden van de Account](https://account.windowsazure.com/Subscriptions). Bijvoorbeeld, kunt u deze functie om te profiteren van de [maandelijkse credits voor abonnees van Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Als u op de [Gratis proefperiode](https://azure.microsoft.com/free/), informatie over het [upgraden naar Pay-As-You-Go](billing-upgrade-azure-subscription.md).

#### <a name="whats-supported"></a>Wat wordt ondersteund:

| Van                                                              | Aan                                                                                      |
|-------------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Pay-As-You-Go Dev/Test](https://azure.microsoft.com/offers/ms-azr-0023p/)              |
| [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)          |
| [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p/)     |
| [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) | [MSDN-Platforms](https://azure.microsoft.com/offers/ms-azr-0062p/)                      |
| [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/)            |
| [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/) | [Visual Studio Enterprise (Bizspark)](https://azure.microsoft.com/offers/ms-azr-0064p/) |

> [AZURE.NOTE] Voor andere wijzigingen, [Neem contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)te bieden.
    
## <a name="switch-subscription-offer"></a>Switch-abonnement aanbieding

> [AZURE.VIDEO switch-to-a-different-azure-offer]

1.  Midden van [Azure Account](https://account.windowsazure.com/Subscriptions)aanmelden.

2.  Selecteer uw Pay-As-You-Go abonnement.

3.  Klik op **een ander voorstel**. De knop is alleen beschikbaar als u van Pay-As-You-Go gebruikmaakt gedaan met de eerste betalingsperiode.

    ![Let op de knop schakelen aanbieding aan de rechterkant van de pagina](./media/billing-how-to-switch-azure-offer/switchbutton.png)
    
4.  **Selecteer de gewenste aanbieding** uit de lijst met aanbiedingen die uw abonnement te kan worden ontstoken. Deze lijst is afhankelijk van het lidmaatschap dat uw account is gekoppeld. Als niets beschikbaar is, controleert u de [lijst met beschikbare aanbiedingen die u naar overschakelen kunt](#whats-supported) en zorg ervoor dat u beschikt over de juiste lidmaatschappen. 

    ![Selecteer een aanbieding die u overschakelen wilt naar de](./media/billing-how-to-switch-azure-offer/selectoffer.png)

5.  Afhankelijk van de aanbieding die u naar omschakelt ziet u een opmerking over de gevolgen van het overschakelen naar een andere. Deze lijst zorgvuldig doorlopen en volg de instructies voordat u verdergaat.

    ![Notities bekijken](./media/billing-how-to-switch-azure-offer/thingstonote.png)

6.  U kunt de naam van uw abonnement. Standaard stellen we het op de naam van de nieuwe aanbieding. Klik op de **Switch bieden** om het proces te voltooien.

    ![Klik op de groene knop](./media/billing-how-to-switch-azure-offer/confirmpage.png)

7.  Succes! Uw abonnement is nu overgeschakeld naar de nieuwe aanbieding.

## <a name="why-cant-i-switch-offers"></a>Waarom kan niet ik overschakelen van aanbiedingen?

**Overschakelen naar een ander voorstel** kan niet worden weergegeven als:

- Bent u niet op [Pay-As-You-Go](https://azure.microsoft.com/offers/ms-azr-0003p/). Op dit moment alleen Pay-As-You-Go abonnementen kunnen worden verplaatst naar een ander voorstel.

    - Als u op de [Gratis proefperiode](https://azure.microsoft.com/free/), informatie over het [upgraden naar Pay-As-You-Go](billing-upgrade-azure-subscription.md).

    - Om over te schakelen op voorstel van een ander abonnement, [Neem contact op met support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

- U bent nog steeds op de eerste betalingsperiode; u moet wachten tot de eerste betalingsperiode beëindigen voordat u de aanbiedingen kunt schakelen.

Mogelijk ziet u **er geen aanbiedingen beschikbaar in uw regio of land op dit moment** als:

- Bent u niet in aanmerking voor alle switches bieden. Controleer de [lijst met beschikbare aanbiedingen die u naar overschakelen kunt](#whats-supported).

## <a name="what-does-switching-azure-offers-do-to-my-service-and-billing"></a>Wat doet de Azure voorstellen doen overschakelen naar Mijn service en facturering?

Hier vindt u de details van wat er gebeurt wanneer u overschakelt van Azure plannen in het midden van de Account.

### <a name="access-to-services"></a>Toegang tot services

Er is geen service downtime voor gebruikers die zijn gekoppeld aan het abonnement. De aanbieding die u mogelijk echter beperkingen. Bijvoorbeeld, verbieden sommige aanbiedingen gebruik in productie, zodat moet u productieresources verplaatsen naar een ander abonnement.

### <a name="billing"></a>Facturering

Op de dag dat u overschakelt, wordt een factuur gegenereerd voor alle openstaande kosten. Vervolgens wordt uw abonnement gefactureerd per betalingsvoorwaarden van het nieuwe aanbod. De betalingsdatum van het abonnement wordt gewijzigd op de datum waarop u aanbiedingen hebt gewijzigd. Gebruik en facturering gegevens voordat de wijziging van het voorstel wordt niet bewaard, dus het is raadzaam dat u een exemplaar voordat u overschakelt downloaden.

> [AZURE.NOTE] Als gevolg van beperkingen met betrekking tot facturering zijn aanbieding schakelopties niet mogelijk binnen de eerste factureringscyclus na het maken van een abonnement.

## <a name="can-i-migrate-from-pay-as-you-go-to-cloud-solution-providerhttpspartnermicrosoftcomsolutionscloud-reseller-overview-csp-or-enterprise-agreementhttpsazuremicrosoftcompricingenterprise-agreement-ea"></a>Kan ik migreren van Pay-As-You-Go [Cloud Solution Provider](https://partner.microsoft.com/Solutions/cloud-reseller-overview) (CSP) of [Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/) (EA)?

We ondersteunen op dit moment geen voorstel overschakelen naar de CSP of EA in het midden van de rekeningen. Als u wilt uw bestaande abonnement verplaatst naar EA, hebben uw inschrijving admin uw account toevoegen in de EA. Ontvangt u een uitnodiging via e-mail. Wanneer u de instructies om de uitnodiging te accepteren, worden uw abonnementen automatisch verplaatst onder de Enterprise Agreement. Als u wilt migreren naar de CSP, Zie [Azure abonnement migratie naar CSP](https://blogs.technet.microsoft.com/hybridcloudbp/2016/08/26/azure-subscription-migration-to-csp/).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [beheerdersrollen beheren](billing-add-change-azure-subscription-administrator.md) voor uw abonnement

- Het gebruik bijhouden door het [downloaden van gegevens over het gebruik en de factuur](billing-download-azure-invoice-daily-usage-date.md)

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met support.

Als u nog verdere vragen hebt, neem [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) voor uw probleem snel opgelost.
