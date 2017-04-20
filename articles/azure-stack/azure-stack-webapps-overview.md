<properties
    pageTitle="Azure Stack Web Apps-overzicht | Microsoft Azure"
    description="Overzicht van Web Apps in Azure stapel"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="azure-stack-web-apps-overview"></a>Azure Stack Web Apps-overzicht
    
> [AZURE.NOTE] De volgende informatie geldt alleen voor Azure Stack TP1-implementaties.

Azure Stack Web Apps is het eerste element van Azure App Service aangeboden in Azure stapel gebracht. De installateur Azure Stack Web Apps maakt een exemplaar van elk van de vijf typen van de vereiste rol en maakt ook een bestandsserver. Hoewel u meer exemplaren voor elk van de typen rol toevoegen kunt, onthouden is niet veel ruimte voor VMs in technische Preview 1. De huidige mogelijkheden voor Azure Stack Web Apps zijn voornamelijk foundation mogelijkheden die nodig zijn voor het beheren van het systeem en de host web apps.

![Azure Stack App Service Web Apps in de Azure stapelen Portal][1]

## <a name="limitations-of-the-technical-preview"></a>Beperkingen van technische Preview

Er is geen ondersteuning voor de preview-versies van Azure Stack App-Service. Plaats geen productie werkbelasting voor deze preview-versie. Er is geen upgrade tussen preview-versies van Azure Stack App-Service. Het voornaamste doel van deze preview-versies zijn om weer te geven wat wij bieden en feedback worden opgehaald. 

## <a name="what-is-an-app-service-plan"></a>Wat is een App Service plannen?

De provider van de resource Azure Stack Web Apps gebruikt dezelfde code die gebruikmaakt van de functie Azure Web Apps in de App Azure Service. Hierdoor zijn sommige gemeenschappelijke begrippen waard beschrijven. In Web Apps, de prijzen container voor web apps de App serviceplan genoemd. Hiermee geeft u de set met specifieke virtuele machines die worden gebruikt voor het opslaan van uw toepassingen. U kunt meerdere App serviceplannen hebben binnen een bepaald abonnement. Dit geldt ook in Azure Stack Web Apps. 

In Azure zijn er gedeelde en gespecialiseerde werknemers. Een gedeelde werknemer ondersteunt met hoge dichtheid multitenant app webhosting en er is slechts één set gedeelde werknemers. Specifieke servers alleen worden gebruikt door een huurder en worden geleverd in drie formaten: klein, middelgroot en groot. De behoeften van klanten voor het bedrijf kunnen niet altijd worden beschreven met behulp van deze voorwaarden. In Azure Stack Web Apps kunnen resource provider beheerders de werkprocessen lagen die ze beschikbaar maken wilt, dat ze hebben meerdere sets met gedeelde werknemers verschillende sets van gespecialiseerde werknemers op basis van de behoeften van hun unieke webhosting definiëren. Met deze definities werknemer laag, kunnen ze opgeven hun eigen prijzen SKU's.

## <a name="portal-features"></a>Portal-functies

Zoals geldt ook voor de back-end, Azure Stack Web Apps maakt gebruik van dezelfde gebruikersinterface met Azure Web Apps. Sommige functies zijn uitgeschakeld en niet nog functioneel in Azure stapel. Dit komt doordat de Azure-specifieke verwachtingen of services waarvoor die functies zijn nog niet beschikbaar in de Azure. 

## <a name="next-steps"></a>Volgende stappen

- [Voordat u aan de slag met Azure Stack Web Apps](azure-stack-webapps-before-you-get-started.md)
- [De voorziening Web Apps-bron installeren](azure-stack-webapps-deploy.md)

U kunt ook andere [platform als een service (PaaS)-services](azure-stack-tools-paas-services.md), zoals de [resource-provider voor SQL Server](azure-stack-sql-rp-deploy-short.md) en [MySQL resource provider](azure-stack-mysql-rp-deploy-short.md)uitproberen.

<!--Image references-->
[1]: ./media/azure-stack-webapps-overview/AppService_Portal.png
