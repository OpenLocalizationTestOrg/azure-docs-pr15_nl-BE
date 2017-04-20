<properties
    pageTitle="Azure Service Bus met Azure automatisering beheren | Microsoft Azure"
    description="Informatie over de service Azure automatisering gebruiken voor het beheren van Azure Service Bus."
    services="service-bus, automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="magoedte;csand"/>

# <a name="managing-azure-service-bus-using-azure-automation"></a>Azure Service Bus met Azure automatisering beheren

Deze gids laat u kennismaken met de automatisering van Azure-service en hoe deze kan worden gebruikt om het beheer van Azure Service Bus te vereenvoudigen.

## <a name="what-is-azure-automation"></a>Wat is Azure automatisering?

[Azure automatisering](../automation/automation-intro.md) is een Azure voor cloud beheer via automatisering van bedrijfsprocessen en de gewenste status configuratie te vereenvoudigen. Azure automatisering, handmatige, kunnen herhaald, langdurige en lastige taak worden geautomatiseerd om betrouwbaarheid, efficiëntie en tijd waarde voor uw organisatie te vergroten.

Azure automatisering biedt een uiterst betrouwbare, hoge beschikbaarheid uitvoering workflowengine die wordt aangepast aan uw behoeften. In Azure automatisering, processen volledig uit handmatig, door 3de partij systemen of regelmatig zodat taken gebeurt precies wanneer dit nodig is.

Operationele overhead te verminderen en vrij van IT en medewerkers zich richten op werk dat zakelijke waarde wordt toegevoegd door het verplaatsen van uw taken automatisch worden uitgevoerd door de automatisering van Azure cloud DevOps.

## <a name="how-can-azure-automation-help-manage-azure-service-bus"></a>Hoe kan automatisering Azure helpen beheren Azure Service Bus

Service Bus met Azure automatisering kunt u beheren met behulp van de [Service Bus REST API](https://msdn.microsoft.com/library/azure/mt639375.aspx). U kunt in Azure automatisering PowerShell-scripts voor het uitvoeren van veel van de taken van uw Service Bus met de REST API uitvoeren. U kunt ook deze REST API-aanroepen in Azure automatisering met de cmdlets voor andere Azure diensten, complexe taken automatiseren via Azure services en 3de partij systemen combineren.

Hier volgen enkele voorbeelden van het gebruik van PowerShell Azure Service Bus beheren:

* [Aangepaste PowerShell-cmdlets voor het beheren van wachtrijen Azure Service Bus](https://blogs.technet.microsoft.com/uktechnet/2014/12/04/sample-of-custom-powershell-cmdlets-to-manage-azure-servicebus-queues)
* [Het maken van wachtrijen, onderwerpen en abonnementen met een PowerShell script Service Bus](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Azure Service Bus naamruimten met PowerShell maken](http://buildazure.com/2015/09/24/create-azure-service-bus-namespaces-using-powershell-and-x-plat-cli/)

De PowerShell-module voor het werken met Azure service bus in automatisering runbooks kan worden gedownload van de [Galerie met PowerShell](https://www.powershellgallery.com/packages/AzureServiceBusCreation/1.0).


## <a name="next-steps"></a>Volgende stappen

U hebt geleerd dat de basisbeginselen van Azure automatisering en hoe deze kan worden gebruikt voor het beheren van Azure Service Bus, klik op deze koppelingen voor meer informatie over het automatiseren van Azure.

* Zie de Azure automatisering [aan de slag zelfstudie](../automation/automation-first-runbook-graphical.md)
* Zie [Service Bus met PowerShell](service-bus-powershell-how-to-provision.md) beheren
