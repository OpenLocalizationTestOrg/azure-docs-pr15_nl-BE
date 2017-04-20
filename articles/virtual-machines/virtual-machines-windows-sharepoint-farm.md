<properties
    pageTitle="SharePoint server-farms maken | Microsoft Azure"
    description="Maak snel een nieuwe SharePoint 2013 of SharePoint 2016 farm in Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="JoeDavies-MSFT"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="josephd"/>

# <a name="create-sharepoint-server-farms"></a>Maken-SharePoint server-farms

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassiek model.

## <a name="sharepoint-2013-farms"></a>SharePoint 2013 farms

Met de Microsoft Azure portal marketplace, kunt u snel vooraf geconfigureerde SharePoint Server 2013 bedrijven maken. Dit bespaart u veel tijd wanneer u een SharePoint-farm standaard of hoge beschikbaarheid voor een dev/testomgeving nodig hebt of als u SharePoint Server 2013 als samenwerkingsoplossing voor uw organisatie evalueert.

> [AZURE.NOTE] De **SharePoint-serverfarm** item in de markt Azure van Azure portal is verwijderd. Het is vervangen door de artikelen **SharePoint 2013 niet-HA** en **SharePoint 2013 HA Farm** .

De standaard SharePoint-farm bestaat uit drie virtuele machines in deze configuratie.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/Non-HAFarm.png)

U kunt deze serverfarm-configuratie voor een vereenvoudigde setup voor de ontwikkeling van apps in SharePoint of de eerste evaluatie van SharePoint 2013.

Voor het maken van de standaard SharePoint-farm (drie-server):

1. Klik [hier](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).
2. Klik op **installeren**.
3. Klik in het deelvenster **SharePoint 2013 niet HA bedrijf** **maken**.
4. Geef instellingen op de stappen voor het **Maken van SharePoint 2013 niet-HA Farm** deelvenster en klik vervolgens op **maken**.

De hoge beschikbaarheid SharePoint-farm bestaat uit negen virtuele machines in deze configuratie.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/HAFarm.png)

U kunt deze serverfarm-configuratie testen hogere belasting van de client, de beschikbaarheid van de externe SharePoint-site en SQL Server AlwaysOn beschikbaarheid-groepen voor een SharePoint-farm. U kunt deze configuratie ook gebruiken voor de ontwikkeling van apps in SharePoint in een omgeving met hoge beschikbaarheid.

Voor het maken van de SharePoint-farm van hoge beschikbaarheid (9-server):

1. Klik [hier](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).
2. Klik op **installeren**.
3. Klik in het deelvenster **SharePoint 2013 HA-Farm** **maken**.
4. Geef instellingen op de zeven stappen van het deelvenster **Maakt SharePoint 2013 HA-Farm** en klik vervolgens op **maken**.

> [AZURE.NOTE] U kunt het **SharePoint 2013 niet HA landbouwbedrijf** of **SharePoint 2013 HA-Farm** maken met een gratis proefversie van Azure.

De Azure portal maakt van deze bedrijven in een wolk alleen virtueel netwerk met een internetverbinding op het web presenteren. Er is geen site naar site VPN- of ExpressRoute verbinding naar het netwerk van uw organisatie.

> [AZURE.NOTE] Wanneer u de basic maken of de Azure portal met hoge beschikbaarheid SharePoint-farms, u kunt een bestaande resourcegroep niet opgeven. U kunt deze beperking omzeilen, maakt u deze bedrijven met Azure PowerShell. Zie voor meer informatie, [maken van SharePoint 2013 dev/test bedrijven met Azure PowerShell](https://technet.microsoft.com/library/mt743093.aspx#powershell).

## <a name="sharepoint-2016-farms"></a>SharePoint 2016 farms

Raadpleeg [dit artikel](https://technet.microsoft.com/library/mt723354.aspx) voor de instructies voor het maken van de volgende SharePoint Server 2016 farm voor één server.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/SP2016Farm.png)

## <a name="managing-the-sharepoint-farms"></a>De SharePoint-farms beheren

U kunt de servers van deze bedrijven via Extern bureaublad-verbindingen beheren. Zie voor meer informatie, [aanmelden bij de virtuele machine](virtual-machines-windows-hero-tutorial.md#log-on-to-the-virtual-machine).

Op de site voor Centraal beheer van SharePoint, kunt u Mijn sites, SharePoint-toepassingen en andere functies. Zie [SharePoint configureren](http://technet.microsoft.com/library/ee836142.aspx)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Ontdek meer [SharePoint-configuraties](https://technet.microsoft.com/library/dn635309.aspx) in Azure infrastructuurservices.
