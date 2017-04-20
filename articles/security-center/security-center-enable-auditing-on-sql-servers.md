<properties
   pageTitle="Controle inschakelen in een SQL-servers in Beveiligingscentrum Azure | Microsoft Azure"
   description="Dit document wordt beschreven hoe u de aanbeveling van de Azure Beveiligingscentrum **inschakelen van controle voor SQL-servers**implementeren."
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

# <a name="enable-auditing-on-sql-servers-in-azure-security-center"></a>Controle inschakelen in een SQL-servers in Azure Security Center

Azure Security Center wordt aanbevolen dat u de controle voor alle databases op de Azure SQL servers als controle nog niet is ingeschakeld. Controle, kunt u de naleving van de regelgeving te onderhouden, te begrijpen activiteit in een database en inzicht in de verschillen en afwijkingen die kunnen duiden op zakelijke problemen of vermoedelijke schendingen van de beveiliging.

Nadat u controle hebt ingeschakeld, kunt u gevaar detectie-instellingen en e-mails ontvangen van waarschuwingen configureren. Detectie van bedreigingen detecteert afwijkende databaseactiviteiten die potentiële beveiligingsrisico's tot de database aangeeft. Hiermee kunt u op te sporen en te reageren op potentiële bedreigingen, zoals deze voorkomen.

Deze aanbeveling is van toepassing op de service SQL Azure. het bevat geen SQL Server wordt uitgevoerd op uw virtuele machines in Azure infrastructuurservices (IaaS Azure).

> [AZURE.NOTE] Dit document bevat de service met behulp van een voorbeeld van de implementatie.  Dit is een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>Implementatie van de aanbeveling

1. Selecteer in de blade **aanbevelingen** **Controle inschakelen op SQL-servers**.  Hiermee opent u de **Controle inschakelen op servers met SQL** -blade.
![Controle inschakelen in een SQL-servers][1]

2. Controle inschakelen in een SQL-server selecteren. Hiermee opent u het blad van de **Controle-instellingen** .
![Controle-instellingen][2]
3. Selecteer op het blad **Controle-instellingen** **op** onder **controle**.
![Controle-instellingen inschakelen][3]

4. Volg de stappen in [aan de slag met SQL-database controleren](../sql-database/sql-database-auditing-get-started.md) om opslag te configureren waar de controlelogboeken worden opgeslagen. Het abonnement van is opslag voor het verzamelen van gegevens de standaardaccount voor opslag.

5. Volg de stappen in de [aan de slag met SQL Database dreiging detectie](../sql-database/sql-database-threat-detection-get-started.md) inschakelen en configureren van de detectie van bedreigingen en de lijst met e-mails die u na detectie van afwijkende activiteiten waarschuwingen ontvangt configureren.

## <a name="see-also"></a>Zie ook

In dit artikel hebt u geleerd hoe de aanbeveling Security Center "Controle inschakelen in SQL-servers." Voor meer informatie over het beveiligen van uw SQL-database, raadpleegt u het volgende:

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
[1]: ./media/security-center-enable-auditing-on-sql-server/enable-auditing-on-sql-servers.png
[2]:./media/security-center-enable-auditing-on-sql-server/enable-auditing.png
[3]: ./media/security-center-enable-auditing-on-sql-server/auditing-settings-blade.png
