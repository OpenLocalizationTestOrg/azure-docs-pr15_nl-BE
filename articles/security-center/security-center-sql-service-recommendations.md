<properties
   pageTitle="Bescherming van Azure SQL service in Beveiligingscentrum Azure | Microsoft Azure"
   description="Dit document adressen in Azure Security Center aanbevelingen waarmee u Azure SQL service beschermen en blijven met inachtneming van de regels voor het beveiligingsbeleid."
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

# <a name="protecting-azure-sql-service-in-azure-security-center"></a>Bescherming van de service SQL Azure in Azure Security Center

Azure Beveiligingscentrum analyseert de beveiligingsstatus van uw resources Azure. Beveiligingscentrum geeft potentiële beveiligingsproblemen, wordt de aanbevelingen die u bij het helpt configureren van de benodigde besturingselementen gemaakt.  Aanbevelingen hebben betrekking op Azure brontypen: virtuele machines (VMs), netwerken, SQL en toepassingen.

In dit artikel komen de aanbevelingen die betrekking hebben op Azure SQL service.  Azure SQL aanbevelingen servicecentrum om het inschakelen van controle voor Azure SQL servers en databases en het inschakelen van codering voor SQL-databases.  De onderstaande tabel gebruiken als referentie bij het begrijpen van de beschikbare SQL service-aanbevelingen en elkaar doen als u het toepast.

## <a name="available-sql-service-recommendations"></a>Beschikbare SQL service-aanbevelingen

|Aanbeveling|Beschrijving|
|-----|-----|
|[Controle van SQL-server inschakelen](security-center-enable-auditing-on-sql-servers.md)|Wordt aanbevolen dat u de controle voor Azure SQL servers (Azure SQL service; niet opgenomen op de virtuele machines met SQL).|
|[Database SQL-controle inschakelen](security-center-enable-auditing-on-sql-databases.md)|Wordt aanbevolen dat u de controle voor Azure SQL-databases (Azure SQL service; niet opgenomen op de virtuele machines met SQL).|
|[Schakel transparante gegevenscodering op SQL-databases](security-center-enable-transparent-data-encryption.md)|Raadt aan om de codering voor SQL-databases (alleen SQL Azure service) in te schakelen.|

## <a name="see-also"></a>Zie ook

Voor meer informatie over aanbevelingen voor andere brontypen Azure, raadpleegt u het volgende:

- [Bescherming van uw virtuele machines in Azure Security Center](security-center-virtual-machine-recommendations.md)
- [Bescherming van uw toepassingen in Azure Security Center](security-center-application-recommendations.md)
- [Beveiligen van uw netwerk in Azure Security Center](security-center-network-recommendations.md)

Voor meer informatie over de Security Center, raadpleegt u het volgende:

- Het [instellen van beveiligingsbeleid in Azure Security Center](security-center-policies.md) --informatie over het configureren van beveiligingsbeleid voor uw abonnementen Azure en resourcegroepen.
- [Beheren en reageren op de beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) --informatie over het beheren van en reageren op de beveiligingswaarschuwingen.
- [Veelgestelde vragen over het azure Security Center](security-center-faq.md) --zoeken vaak gestelde vragen over het gebruik van de service.
