<properties
   pageTitle="Overdracht van eigendom van een abonnement op Azure | Microsoft Azure"
   description="Een abonnement op Azure vaak overbrengen naar een andere gebruiker, en sommige Veelgestelde vragen (FAQ) over het proces"
   services=""
   documentationCenter=""
   authors="genlin"
   manager="stevenpo"
   editor=""
   tags="billing,top-support-issue"/>

<tags
   ms.service="billing"
   ms.workload="na"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/10/2016"
   ms.author="genli"/>

# <a name="transferring-ownership-of-an-azure-subscription"></a>Overdracht van eigendom van een Azure-abonnement

U doet:

- Om eigendom van uw abonnement Azure aan iemand anders facturering nodig?
- Wilt u de account die wordt gebruikt om aan te melden voor Azure wijzigen? Misschien gebruikt uw Microsoft-Account maar bedoeld voor het gebruik van uw werk of school account in plaats daarvan?
- Wilt u uw abonnement Azure verplaatsen vanuit een map?
- Azure en Office 365 in verschillende huurders en wilt samenvoegen?

U kunt nu doen dit gemakkelijk in het Microsoft Azure Account Center - voor Pay-As-You-Go, MSDN, Pack actie of BizSpark abonnementen.  We hebben hebt de mogelijkheid om uw abonnement overbrengen naar een andere gebruiker toegevoegd. Met andere woorden, kunt u de account admin op een Pay-As-You-Go, MSDN, Pack actie of BizSpark abonnement dat u eigenaar bent, maakt niet uit welk land u werkt in nu wijzigen. Wij ondersteunen de overdracht van Azure Marketplace aankopen voor deze typen abonnement ook.

> [AZURE.NOTE] Als u uw abonnement wilt verschillende aanbiedingen, Zie [schakeloptie Azure een abonnement op een ander voorstel](billing-how-to-switch-azure-offer.md) voor meer informatie. Als u meer Help-informatie op elk punt in dit artikel, neem [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) voor uw probleem snel opgelost.

## <a name="how-to-transfer-ownership-of-an-azure-subscription"></a>Het overdragen van de eigendom van een Azure-abonnement

> [AZURE.VIDEO transfer-an-azure-subscription]

1.  Log in op <https://account.windowsazure.com/Subscriptions>. U moet de beheerder van de rekening voor het uitvoeren van een overdracht van eigendom. Zie de [Veelgestelde vragen](#faq)voor meer informatie over hoe u kunt nagaan wie er met de accountbeheerder van het abonnement.

2.  Selecteer het abonnement te brengen.

3.  Klik op de optie **Transfer abonnement** .

    ![Tabblad Azure abonnementen](./media/billing-subscription-transfer/image1.png)

4.  Volg de aanwijzingen voor de ontvanger opgeven.

    ![Overdracht van het dialoogvenster abonnement](./media/billing-subscription-transfer/image2.PNG)

5.  De ontvanger krijgt automatisch een e-mail met een link aanvaarding.

    ![E-mail naar geadresseerde overdracht](./media/billing-subscription-transfer/image3.png)

6.  De ontvanger klikt op de koppeling en volgt de instructies, met inbegrip van hun betalingsgegevens invoeren.

    ![Abonnement overdracht webpagina](./media/billing-subscription-transfer/image4.png)

    ![Tweede abonnement overdracht webpagina](./media/billing-subscription-transfer/image5.png)

7. Succes! Het abonnement wordt nu overgebracht.

<a id="faq"></a>
## <a name="frequently-asked-questions-faq"></a>Veelgestelde vragen (FAQ)

-   **Hoe weet ik wie de accountbeheerder van het abonnement is?**

    U kunt controleren wie de accountbeheerder van het abonnement is als volgt:

    1. Log in om de [Azure portal](https://portal.azure.com).
    2. Selecteer in het menu Hub **abonnement**.
    3. Selecteer het abonnement dat u wilt controleren en selecteer **Instellingen**.
    4. Selecteer **Eigenschappen**. De beheerder van de rekening van het abonnement wordt weergegeven in het vak **Account Admin** .  

-   **De overdracht van een abonnement tot gevolg heeft als de onderhoudstijd?**

    Er is geen gevolgen heeft voor de service. Dit effectief Hiermee annuleert u het abonnement onder de huidige Account beheerder en maakt u een nieuwe account van de geadresseerde, maar de onderliggende Azure services worden gekoppeld aan het nieuwe abonnement. De abonnement-ID blijft dezelfde.

-   **Hoe gebruik ik dit mechanisme aan de map voor abonnement wijzigen**-   
    Een Azure-abonnement wordt gemaakt in de map die de Admin-Account hoort. Dus om de map te wijzigen, net transfer het abonnement aan een gebruikersaccount in de doelmap. Wanneer die gebruiker de stappen voor het accepteren van de overdracht is voltooid wordt het abonnement automatisch verplaatst naar de doelmap.

-   **Als ik via facturatie eigendom van een abonnement van een andere organisatie, blijft ze toegang hebben tot mijn bronnen?**

    Het abonnement is overgebracht naar een andere huurder, kwijt de gebruikers die zijn gekoppeld aan de vorige huurder als toegang tot het abonnement. Zelfs als een gebruiker niet een Co-beheerder of een Admin-Service niet meer, kunnen ze nog steeds toegang hebben tot het abonnement via andere beveiligingsmechanismen. Deze omvatten:
    - Beheer van certificaten die de gebruiker admin rechten voor abonnement bronnen toekennen. Zie voor meer informatie, [maken en uploaden van een certificaat voor Azure](https://msdn.microsoft.com/library/azure/gg551722.aspx)
    -   Toegangstoetsen voor services zoals opslag. Zie voor meer informatie, [weergeven, kopiëren en toegangstoetsen opslag opnieuw genereren](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)
    -   RAS-referenties voor services zoals Azure virtuele Machines

    Dit is geen volledige lijst. De ontvanger moet rekening houden met alle geheimen die zijn gekoppeld aan de service als ze nodig hebben om toegang te beperken tot de bronnen worden bijgewerkt. De meeste bronnen kunnen als volgt worden bijgewerkt:

    1.   Ga naar de portal Azure: [ *https://portal.azure.com*](https://portal.azure.com)

    2.    Klik op Bladeren, All -&gt; alle Resources

    3.    Selecteer de resource. Hiermee opent u de resource-blade.

    4.    Klik op **Instellingen**in de resource-blade. Hier kunt u bekijken en bijwerken van bestaande geheimen.


-   **Als ik het abonnement in het midden van de factureringscyclus overbrengt, de ontvanger betaalt de hele facturering-cyclus?**

    De afzender is verantwoordelijk voor de betaling voor een gebruik dat is gemeld, tot het punt dat de overdracht is voltooid. De ontvanger is verantwoordelijk voor gebruik vanaf het moment van de overdracht wordt gerapporteerd. Het is mogelijk sommige gebruik die plaatsvonden vóór de overdracht, maar daarna werd gerapporteerd. Deze oplossing wordt opgenomen in de rekening van de ontvanger.

-   **Heeft de geadresseerde toegang tot het gebruik en facturering geschiedenis**

    Op dit moment is is de enige weergegeven aan de geadresseerde het bedrag van de laatste factuur (of het huidige saldo als het abonnement is overgebracht voordat de eerste factuur is gegenereerd). De rest van het gebruik en de facturering geschiedenis wordt niet overdragen aan het abonnement.

-   **Kan het voorstel worden gewijzigd tijdens de overdracht?**

    De aanbieding moet hetzelfde blijven. Als u wilt uw aanbieding wijzigen, moet u [contact opnemen met ondersteuning](http://go.microsoft.com/fwlink/?LinkID=619338).

-   **Kan ik een licentie overbrengen naar een gebruikersaccount in een ander land?**

    Nee, op dit moment die wordt niet ondersteund. De gebruikersaccount van de ontvanger moet zich in hetzelfde land.

-   **Kan de geadresseerde een andere mechanisme gebruiken?**

    Ja. Er zijn hier beperkingen: nu het abonnement facturering geschiedenis wordt gesplitst in twee rekeningen. Maar het voordeel is dat u dit doen kunt zonder [contact op met Support](http://go.microsoft.com/fwlink/?LinkID=619338).

-   **De betalingsmethode worden beïnvloed nadat ik een abonnement Azure overgebracht?**

    Om een abonnement overdracht accepteren, worden een creditcard of een vergelijkbare betalingsmethode verstrekt om te betalen voor het abonnement. Bijvoorbeeld als Bob een abonnement naar Jan brengt en Jantine de overdracht accepteert, moet Jane ook bieden een betalingsmethode die ze gebruiken om te betalen voor het abonnement. Nadat de overdracht voltooid is, Bob niet langer brengt voor het abonnement is dat hij naar Jane overgebracht.

## <a name="next-steps-after-accepting-ownership-of-a-subscription"></a>Volgende stappen na aanvaarding van de eigendom van een abonnement

1. U bent nu de Account beheerder. Bekijken en bijwerken van de beheerder en CO-beheerders. Beheerders in [Azure klassieke portal](https://manage.windowsazure.com) beheren door naar instellingen te gaan. [Meer informatie](http://go.microsoft.com/fwlink/?LinkID=533293).
2. U kunt ook op rollen gebaseerde toegangscontrole (RBAC) gebruiken voor uw abonnement en services. Ga naar de [portal Azure](https://portal.azure.com) - [meer informatie over RBAC](http://go.microsoft.com/fwlink/?LinkID=544802)
3. Referenties die zijn gekoppeld aan services van dit abonnement bijwerken. Deze omvatten:
    - Beheer van certificaten die de gebruiker admin rechten voor abonnement bronnen toekennen. Zie voor meer informatie, [maken en uploaden van een certificaat voor Azure](https://msdn.microsoft.com/library/azure/gg551722.aspx)
    -   Toegangstoetsen voor services zoals opslag. Zie voor meer informatie, [weergeven, kopiëren en toegangstoetsen opslag opnieuw genereren](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)
    -   RAS-referenties voor services zoals Azure virtuele Machines
4. Factuuradres waarschuwingen voor dit abonnement, op de [Azure Account Center](https://account.windowsazure.com/Subscriptions)[meer informatie](http://go.microsoft.com/fwlink/?LinkID=533292) bijwerken  
5.  Als u met een partner werkt, kunt u het bijwerken van de partner-ID voor dit abonnement. U kunt dit doen in het [Midden van Azure-Account](https://account.windowsazure.com/Subscriptions).

> [AZURE.NOTE] Als u nog verdere vragen hebt, neem [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) voor uw probleem snel opgelost.
