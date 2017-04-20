<properties
    pageTitle="Abonnement en rekeningen richtlijnen | Microsoft Azure"
    description="Informatie over de belangrijkste ontwerp en de implementatie richtlijnen voor abonnementen en rekeningen op Azure."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="subscription-and-accounts-guidelines"></a>Richtlijnen voor het abonnement en accounts

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

In dit artikel is gericht op het inzicht van de benadering van abonnement en account management als uw omgeving en gebruikers toeneemt.


## <a name="implementation-guidelines-for-subscriptions-and-accounts"></a>Implementatie van richtlijnen voor abonnementen en accounts

Besluiten:

- Welke set u nodig hebt voor het hosten van uw werkbelasting IT of infrastructuur abonnementen en rekeningen doen?
- Hoe te splitsen in de hiërarchie van uw organisatie past?

Taken:

- De hiërarchie van de logische organisatie definiëren als u nodig hebt voor het beheren van een abonnement.
- Aan deze logische hiërarchie, definieert u de rekeningen vereist en abonnementen op elke rekening.
- De set van abonnementen en rekeningen met behulp van de naamgevingsconventie maken.


## <a name="subscriptions-and-accounts"></a>Abonnementen en accounts

Met Azure wilt werken, moet u een of meer Azure abonnementen. Bronnen, zoals virtuele machines (VMs) of virtuele netwerken in deze abonnementen bestaan.

- Bedrijven hebben meestal een inschrijving onderneming, die is gekoppeld aan een of meer accounts en wordt de bovenste bron in de hiërarchie.
- De bovenste bron is voor consumenten en klanten zonder dat de inschrijving van een onderneming, de rekening.
- Abonnementen zijn gekoppeld aan accounts en kunnen er een of meer abonnementen per account. Azure records factureringsgegevens op het niveau van het abonnement.

Als gevolg van de limiet van twee niveaus op de relatie rekening/abonnement is het belangrijk de naamgevingsconventie van accounts en abonnementen op de factuur moet worden uitgelijnd. Bijvoorbeeld, als een wereldwijd bedrijf Azure worden gebruikt, kunnen ze ervoor kiezen om één rekening per regio en hebt abonnementen beheerd op het regioniveau van de:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub01.png)

Gebruik bijvoorbeeld de volgende structuur:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub02.png)

Als een regio wil hebben van meer dan één abonnement is gekoppeld aan een bepaalde groep, moet een manier voor het coderen van de extra gegevens over de account of de naam van het abonnement opnemen in de naamgevingsconventie. Deze organisatie kan massaging facturatie gegevens om de nieuwe niveaus van de hiërarchie tijdens facturering rapporten genereren:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub03.png)

De organisatie kan er als volgt uit:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

Wij bieden gedetailleerde facturering via een downloadbaar bestand voor één account of voor alle accounts in een enterprise agreement.


## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 