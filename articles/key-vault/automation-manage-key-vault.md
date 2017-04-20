<properties
    pageTitle="Azure sleutel kluis met Azure automatisering beheren | Microsoft Azure"
    description="Meer informatie over hoe de automatisering Azure-service kan worden gebruikt voor het beheren van Azure sleutel kluis."
    services="Key-Vault, automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="magoedte;csand"/>

#<a name="managing-azure-key-vault-using-azure-automation"></a>Azure sleutel kluis met Azure automatisering beheren

Deze gids laat u kennismaken met de automatisering van Azure-service en hoe deze kan worden gebruikt om het beheer van uw sleutels en geheimen in Azure sleutel kluis te vereenvoudigen.

## <a name="what-is-azure-automation"></a>Wat is Azure automatisering?

[Azure automatisering](../automation/automation-intro.md) is een Azure voor cloud beheer via automatisering van bedrijfsprocessen en de gewenste status configuratie te vereenvoudigen. Azure automatisering, handmatige, kunnen herhaald, langdurige en lastige taak worden geautomatiseerd om betrouwbaarheid, efficiëntie en tijd waarde voor uw organisatie te vergroten.

Azure automatisering biedt een uiterst betrouwbare, hoge beschikbaarheid uitvoering workflowengine die wordt aangepast aan uw behoeften. In Azure automatisering, processen volledig uit handmatig, door 3de partij systemen of regelmatig zodat taken gebeurt precies wanneer dit nodig is.

Operationele overhead te verminderen en vrij van IT en medewerkers zich richten op werk dat zakelijke waarde wordt toegevoegd door het verplaatsen van uw taken automatisch worden uitgevoerd door de automatisering van Azure cloud DevOps.


## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a>Hoe kan automatisering Azure helpen beheren Azure sleutel kluis

Sleutel kluis in Azure automatisering kunnen worden beheerd met behulp van de [AzureRM sleutel kluis cmdlets] (https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) en [de klassieke sleutel kluis Azure cmdlets](https://msdn.microsoft.com/library/azure/dn868052.aspx). De Azure-module voor het beheren van klassieke sleutel kluis is beschikbaar in Azure automatisering automatisch en kunt u de [module AzureRM KeyVault](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) importeren in Azure automatisering, zodat u veel van uw sleutel kluis taken binnen de service kan uitvoeren. U kunt ook deze cmdlets in Azure automatisering met de cmdlets voor andere Azure diensten, complexe taken automatiseren via Azure services en 3de partij systemen combineren.

Met de cmdlets Azure sleutel kluis kunt u deze taken onder andere uitvoeren: 

- Maken en configureren van een sleutel kluis
- Maak of importeer een sleutel
- Maken of bijwerken van een geheim
- Kenmerken van een sleutel bijwerken
- Een sleutel of een geheim ophalen
- Verwijderen van een sleutel of een geheim

Hier volgen enkele voorbeelden van de sleutel kluis beheren met PowerShell:  

* [Azure sleutel kluis - Step by Step](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [Instellen en configureren van een Azure sleutel kluis](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)


## <a name="next-steps"></a>Volgende stappen

U hebt geleerd dat de basisbeginselen van Azure automatisering en hoe deze kan worden gebruikt voor het beheren van Azure sleutel kluis, klik op deze koppelingen voor meer informatie over het automatiseren van Azure.

* Zie de Azure automatisering [aan de slag zelfstudie](../automation/automation-first-runbook-graphical.md).
* Zie de [Azure sleutel kluis PowerShell-scripts](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).
