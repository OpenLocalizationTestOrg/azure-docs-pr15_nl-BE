<properties
   pageTitle="Controle inschakelen in SQL-databases in Beveiligingscentrum Azure | Microsoft Azure"
   description="Dit document wordt beschreven hoe u voor de uitvoering van de aanbeveling van de Azure Beveiligingscentrum **inschakelen van controle voor SQL-databases**."
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
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="enable-auditing-on-sql-databases-in-azure-security-center"></a>Controle inschakelen in SQL-databases in Azure Security Center

Azure Security Center wordt aanbevolen dat u de controle voor alle SQL-databases als controle nog niet is ingeschakeld. Controle, kunt u de naleving van de regelgeving te onderhouden, te begrijpen activiteit in een database en inzicht in de verschillen en afwijkingen die kunnen duiden op zakelijke problemen of vermoedelijke schendingen van de beveiliging.

Nadat u controle hebt ingeschakeld, kunt u gevaar detectie-instellingen en e-mails ontvangen van waarschuwingen configureren. Detectie van bedreigingen detecteert afwijkende databaseactiviteiten die potentiële beveiligingsrisico's tot de database aangeeft. Hiermee kunt u op te sporen en te reageren op potentiële bedreigingen, zoals deze voorkomen.

Deze aanbeveling is van toepassing op de service SQL Azure. bevat geen SQL uitvoeren op uw virtuele machines.

> [AZURE.NOTE] Dit document bevat de service met behulp van een voorbeeld van de implementatie.  Dit is een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>Implementatie van de aanbeveling

1. Selecteer in de blade **aanbevelingen** **Controle inschakelen op de SQL-databases**.  Hiermee opent u de **Controle inschakelen voor SQL-databases** blade.
![Controle inschakelen in SQL-databases][1]

2. Controle inschakelen in een SQL-database selecteren. Hiermee opent u de bladeserver **controleren & bedreiging detectie** .
![Controle- en bedreiging detectie][2]
3. Selecteer op de bladeserver **controleren & bedreiging detectie** **op** onder **controle**.
![Controle- en bedreiging detectie inschakelen][3]


5. Volg de stappen in de [aan de slag met SQL Database dreiging detectie](../sql-database/sql-database-threat-detection-get-started.md) inschakelen en configureren van de detectie van bedreigingen en de lijst met e-mails die u na detectie van afwijkende activiteiten waarschuwingen ontvangt configureren.

## <a name="see-also"></a>Zie ook

Dit artikel werd door het implementeren van de Security Center aanbevelingen "Controle inschakelen in SQL-databases." Voor meer informatie over het beveiligen van uw SQL-database, raadpleegt u het volgende:

- [De SQL-Database beveiligen](../sql-database/sql-database-security.md)

Voor meer informatie over de Security Center, raadpleegt u het volgende:

- Het [instellen van beveiligingsbeleid in Azure Security Center](security-center-policies.md) --informatie over het configureren van beveiligingsbeleid voor uw abonnementen Azure en resourcegroepen.
- [Aanbevelingen voor de beveiliging beheren in Azure Security Center](security-center-recommendations.md) --informatie over het aanbevelingen helpen beschermen uw Azure resources.
- [Statuscontrole voor beveiliging in Azure Security Center](security-center-monitoring.md) --informatie over het controleren van de gezondheid van uw resources Azure.
- [Beheren en reageren op de beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) --informatie over het beheren van en reageren op de beveiligingswaarschuwingen.
- [Monitoring van partneroplossingen met Azure Security Center](security-center-partner-solutions.md) --informatie over het controleren van de status van uw oplossingen van partners.
- [Veelgestelde vragen over het azure Security Center](security-center-faq.md) --zoeken vaak gestelde vragen over het gebruik van de service.
- [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) --laatste Azure beveiliging nieuws en informatie.

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-databases/enable-auditing-on-sql-databases.png
[2]:./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection.png
[3]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection-blade.png
