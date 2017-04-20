<properties
   pageTitle="Beveiligen van uw netwerk in Beveiligingscentrum Azure | Microsoft Azure"
   description="Dit document adressen in Azure Security Center aanbevelingen die u helpen bij het beveiligen van uw netwerk Azure en blijven met inachtneming van de regels voor het beveiligingsbeleid."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/04/2016"
   ms.author="terrylan"/>

# <a name="protecting-your-network-in-azure-security-center"></a>Beveiligen van uw netwerk in Azure Security Center

Azure Beveiligingscentrum analyseert de beveiligingsstatus van uw resources Azure. Beveiligingscentrum geeft potentiële beveiligingsproblemen, wordt de aanbevelingen die u bij het helpt configureren van de benodigde besturingselementen gemaakt.  Aanbevelingen hebben betrekking op Azure brontypen: virtuele machines (VMs), netwerken, SQL en toepassingen.

In dit artikel komen de aanbevelingen die betrekking hebben op uw netwerk.  Netwerkcentrum aanbevelingen om de volgende generatie firewalls, netwerk, beveiligingsgroepen en configuratie regels voor binnenkomend verkeer.  De onderstaande tabel als referentie gebruiken om te begrijpen van de aanbevelingen voor de beschikbare netwerken en elkaar doen als u het toepast.

## <a name="available-network-recommendations"></a>Aanbevelingen voor de beschikbare netwerken

|Aanbeveling|Beschrijving|
|-----|-----|
|[Een volgende generatie Firewall toevoegen](security-center-add-next-generation-firewall.md)|Beveelt de volgende generatie Firewall (NGFW) van een Microsoft-partner toe te voegen om de beveiligingsinstellingen te verhogen.|
|[Route-verkeer via NGFW alleen](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only)|Wordt aanbevolen dat u configureert network group (NSG) regels voor binnenkomend verkeer voor uw VM via uw NGFW te dwingen.|
|[Inschakelen netwerk beveiligingsgroepen op subnetten of virtuele machines](security-center-enable-network-security-groups.md)|Raadt aan om NSGs in te schakelen op subnetten of VMs.|
|[Beperken van toegang via Internet facing eindpunt](security-center-restrict-access-through-internet-facing-endpoints.md)|Adviseert het configureren van regels voor binnenkomend verkeer voor NSGs.|

## <a name="see-also"></a>Zie ook

Voor meer informatie over aanbevelingen voor andere brontypen Azure, raadpleegt u het volgende:

- [Bescherming van uw virtuele machines in Azure Security Center](security-center-virtual-machine-recommendations.md)
- [Bescherming van uw toepassingen in Azure Security Center](security-center-application-recommendations.md)
- [Bescherming van uw service Azure SQL in Azure Security Center](security-center-sql-service-recommendations.md)

Voor meer informatie over de Security Center, raadpleegt u het volgende:

- Het [instellen van beveiligingsbeleid in Azure Security Center](security-center-policies.md) --informatie over het configureren van beveiligingsbeleid voor uw abonnementen Azure en resourcegroepen.
- [Beheren en reageren op de beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) --informatie over het beheren van en reageren op de beveiligingswaarschuwingen.
- [Veelgestelde vragen over het azure Security Center](security-center-faq.md) --zoeken vaak gestelde vragen over het gebruik van de service.
