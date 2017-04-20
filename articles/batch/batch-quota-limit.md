<properties
    pageTitle="Batch-servicequota en maxima | Microsoft Azure"
    description="Meer informatie over de standaard Azure Batch quota, grenzen en beperkingen en verhoogt het quotum aanvragen"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/10/2016"
    ms.author="marsma"/>

# <a name="quotas-and-limits-for-the-azure-batch-service"></a>Quota's en beperkingen voor de Batch Azure service

Als met andere Azure services, er zijn grenzen op bepaalde resources die aan de Batch-service is gekoppeld. Veel van deze limieten zijn standaardquota door Azure toegepast op de account niveau of een abonnement. Deze standaardinstellingen wordt beschreven en hoe u kunt aangeven dat quotum verhoogt.

Als u wilt de werkbelasting productie in Batch uitvoeren, wellicht moet u een of meer van de boven de standaard-quota verhogen. Als u een quota verhogen wilt, kunt u een on line [klantondersteuning verzoek](#increase-a-quota) kosteloos kunt openen.

>[AZURE.NOTE] Een quotum is een kredietlimiet, geen garantie capaciteit. Als u grootschalige die u nodig hebt, neem contact op met ondersteuning van Azure.

## <a name="subscription-quotas"></a>Abonnement quota
**Resource**|**Standaardlimiet**|**Maximum aantal**
---|---|---
Batch rekeningen per regio, per abonnement | 1 | 50

## <a name="batch-account-quotas"></a>Batch account quota
[AZURE.INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="other-limits"></a>Andere limieten
**Resource**|**Maximum aantal**
---|---
[Gelijktijdige taken](batch-parallel-node-tasks.md) per-computerknooppunt | 4 x aantal kernen knooppunt
[Toepassingen](batch-application-packages.md) per Batch-account        | 20
Toepassingspakketten per toepassing  | 40
Grootte van het pakket toepassing (elk)       | Ongeveer 195GB<sup>1</sup>

<sup>1</sup> azure opslaglimiet voor maximale blob blokgrootte

## <a name="view-batch-quotas"></a>Batch quota weergeven

Uw account Batch quota weergeven in de [Azure portal][portal].

1. **Batch-accounts** in de portal selecteert en vervolgens de batchverwerking account waarin je geïnteresseerd bent.

2. Selecteer de **Eigenschappen** op van de Batch-account menu blade

3. De blade eigenschappen geeft de **quota's** die momenteel worden toegepast op de rekening van de partij

    ![Batch account quota][account_quotas]

## <a name="increase-a-quota"></a>Een quota verhogen

Volg de onderstaande stappen om te vragen een quota verhogen met de [Azure portal][portal].

1. Selecteer de tegel **Help + ondersteuning** op uw portal dashboard of het vraagteken (**?**) in de rechterbovenhoek van de portal.

2. Selecteer **nieuw ondersteuningsverzoek** > **Grondbeginselen**.

3. Op het blad **Grondbeginselen** :

    een. **Type probleem** > **Quota**

    b. Selecteer uw abonnement.

    c. **TARGET type** > **Batch**

    d. **Ondersteuning van plan** > **Quota support - inbegrepen**

    Klik op **volgende**.

4. Op het blad **probleem** :

    een. Selecteer een **prioriteitsniveau** volgens uw [bedrijfsresultaten][support_sev].

    b. Geef elk quotum dat u wilt wijzigen, de naam van de account en de nieuwe limiet in **Details**.

    Klik op **volgende**.

5. Op het blad **gegevens van contactpersoon** :

    een. Selecteer een **voorkeur contactmethode**.

    b. Controleer en voer de vereiste gegevens voor de contactpersoon.

    Klik op **maken** om de aanvraag voor ondersteuning indienen.

Zodra u uw verzoek om ondersteuning hebt verzonden, Azure ondersteuning zal contact met u opnemen. Duren voordat de aanvraag voltooid kan maximaal 2 werkdagen.

## <a name="related-topics"></a>Verwante onderwerpen

* [Een Batch Azure account maken met de Azure portal](batch-account-create-portal.md)

* [Azure Batch Functieoverzicht](batch-api-basics.md)

* [Azure abonnement en grenzen van de service, quota's en beperkingen](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG
