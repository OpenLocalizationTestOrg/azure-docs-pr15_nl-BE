<properties services="virtual-machines" title="Setting up Azure CLI for service management" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## <a name="using-azure-cli"></a>Met behulp van Azure CLI

De volgende stappen kunt u gemakkelijk Azure CLI gebruiken met de meest recente versie en het juiste abonnement. Azure CLI installeren en verbinden met uw account eerst, Zie de [Azure opdrachtregelinterface (CLI Azure)](xplat-cli-install.md).

### <a name="step-1-update-azure-cli-version"></a>Stap 1: Update Azure CLI versie

Azure CLI dwingende opdrachten in de modus Beheer service gebruikt, moet u indien mogelijk een recente versie hebben. Als u wilt controleren of uw versie, typt u `azure --version`. Worden er ongeveer als volgt:

    $ azure --version
    0.8.17 (node: 0.10.25)

Als u wilt dat de nieuwste versie van Azure CLI, Zie [Azure CLI](https://github.com/Azure/azure-xplat-cli).

### <a name="step-2-set-the-azure-account-and-subscription"></a>Stap 2: Stel de Azure-account en abonnement

Zodra u uw Azure CLI hebt gekoppeld aan de account die u wilt gebruiken, moet u wellicht meer dan één abonnement. Wanneer u dit doet, raadpleegt u de abonnementen beschikbaar voor uw account door te typen `azure account list`, en selecteer vervolgens het abonnement dat u wilt gebruiken door te typen `azure account set <subscription id or name> true` waarbij _abonnement-id of naam_ is de abonnement-id of naam van het abonnement dat u graag wilt werken in de huidige sessie. Worden er ongeveer zo uit:

    $ azure account set "Visual Studio Ultimate with MSDN" true
    info:    Executing command account set
    info:    Setting subscription to "Visual Studio Ultimate with MSDN" with id "0e220bf6-5caa-4e9f-8383-51f16b6c109f".
    info:    Changes saved
    info:    account set command OK

> [AZURE.NOTE] Als u een abonnement op de MSDN-abonnement hebt, maar nog niet een Azure-account, krijgt u gratis Azure kredieten van het activeren van uw [MSDN subscriber voordelen hier](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -- of u kunt het gratis account. Hetzij werkt voor Azure access.
