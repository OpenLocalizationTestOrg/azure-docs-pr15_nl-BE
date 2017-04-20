<properties
   pageTitle="Bescherming van uw toepassingen in Beveiligingscentrum Azure | Microsoft Azure"
   description="Dit document adressen in Azure Security Center aanbevelingen waarmee u uw Azure toepassingen beveiligen en blijven met inachtneming van de regels voor het beveiligingsbeleid."
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

# <a name="protecting-your-applications-in-azure-security-center"></a>Bescherming van uw toepassingen in Azure Security Center

Azure Beveiligingscentrum analyseert de beveiligingsstatus van uw resources Azure. Beveiligingscentrum geeft potentiële beveiligingsproblemen, wordt de aanbevelingen die u bij het helpt configureren van de benodigde besturingselementen gemaakt.  Aanbevelingen hebben betrekking op Azure brontypen: virtuele machines (VMs), netwerken, SQL en toepassingen.

In dit artikel komen de aanbevelingen die betrekking hebben op toepassingen.  Application center van de aanbevelingen rond de implementatie van een web application firewall.  De onderstaande tabel gebruiken als referentie bij het begrijpen van de aanbevelingen van de toepassing beschikbaar en wat elke doen als u het toepassen.

## <a name="available-application-recommendations"></a>Beschikbare toepassing aanbevelingen

|Aanbeveling|Beschrijving|
|-----|-----|
|[Een web application firewall toevoegen](security-center-add-web-application-firewall.md)|Raadt een web application firewall (WAF) voor web-eindpunten. U kunt meerdere webtoepassingen in Beveiligingscentrum beveiligen door deze toepassingen toevoegen aan uw bestaande WAF-implementaties. WAF-toestellen (gemaakt met het implementatiemodel Resource Manager) moeten worden geïmplementeerd op een afzonderlijke virtuele netwerk. WAF-toestellen (gemaakt met het klassieke implementatiemodel) zijn beperkt tot het met behulp van een beveiligingsgroep voor het netwerk. Deze ondersteuning zal in de toekomst worden uitgebreid tot een volledig aangepaste implementatie van een WAF toestel (klassiek).|
|[Toepassingsbeveiliging voltooien](security-center-add-web-application-firewall.md#finalize-application-protection)|Als u de configuratie van een WAF wilt moet verkeer worden omgeleid naar het toestel WAF. Na deze aanbeveling wordt de nodige wijzigingen worden voltooid.|

## <a name="see-also"></a>Zie ook

Voor meer informatie over aanbevelingen voor andere brontypen Azure, raadpleegt u het volgende:

- [Bescherming van uw virtuele machines in Azure Security Center](security-center-virtual-machine-recommendations.md)
- [Beveiligen van uw netwerk in Azure Security Center](security-center-network-recommendations.md)
- [Bescherming van uw service Azure SQL in Azure Security Center](security-center-sql-service-recommendations.md)

Voor meer informatie over de Security Center, raadpleegt u het volgende:

- Het [instellen van beveiligingsbeleid in Azure Security Center](security-center-policies.md) --informatie over het configureren van beveiligingsbeleid voor uw abonnementen Azure en resourcegroepen.
- [Beheren en reageren op de beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) --informatie over het beheren van en reageren op de beveiligingswaarschuwingen.
- [Veelgestelde vragen over het azure Security Center](security-center-faq.md) --zoeken vaak gestelde vragen over het gebruik van de service.
