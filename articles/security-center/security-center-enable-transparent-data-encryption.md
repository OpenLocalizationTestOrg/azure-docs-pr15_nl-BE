<properties
   pageTitle="Transparante gegevenscodering in Azure Beveiligingscentrum inschakelen | Microsoft Azure"
   description="Dit document wordt beschreven hoe u voor de uitvoering van de aanbeveling Azure Beveiligingscentrum **Inschakelen transparante gegevenscodering**."
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

# <a name="enable-transparent-data-encryption-in-azure-security-center"></a>Transparante gegevenscodering in Azure Beveiligingscentrum inschakelen

Beveiligingscentrum Azure aanbevelen dat u transparante gegevens codering (TDE) in SQL-databases inschakelen als TDE is niet ingeschakeld. TDE beschermt uw gegevens en kunt u aan vereisten te voldoen aan uw database, back-ups van gekoppeld en transactielogboekbestanden in rust, zonder de wijzigingen in uw toepassing te coderen. Voor meer dat informatie Zie [Transparante gegevenscodering met Azure SQL-Database](https://msdn.microsoft.com/library/dn948096).

Deze aanbeveling is van toepassing op de service SQL Azure. bevat geen SQL uitvoeren op uw virtuele machines.

> [AZURE.NOTE] Dit document bevat de service met behulp van een voorbeeld van de implementatie.  Dit is een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>Implementatie van de aanbeveling

1. Selecteer in het blad **aanbevelingen** **Transparante codering inschakelen**.
![Transparante codering inschakelen][1]

2. Hiermee opent u het blad **Transparante gegevenscodering inschakelen op de SQL-databases** . Selecteer TDE inschakelen op een SQL-database.
![Selecteer SQL DB TDE inschakelen op][2]
3. Op het blad **transparante gegevenscodering** **op** Selecteer onder gegevensversleuteling en **selecteert u in het bovenste lint van het blad** .
![TDE inschakelen][3]

  Zodra TDE is ingeschakeld op de geselecteerde SQL-database, verandert de **status van de codering** in **gecodeerd**.    

  ![Status codering][4]

## <a name="see-also"></a>Zie ook

Dit artikel werd door het implementeren van de Security Center aanbevelingen "Transparante codering inschakelen." Voor meer informatie over SQL TDE, raadpleegt u het volgende:

- [Transparante gegevenscodering met Azure SQL-Database](https://msdn.microsoft.com/library/dn948096)
- [Aan de slag met transparante gegevens codering (TDE)](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

Voor meer informatie over de Security Center, raadpleegt u het volgende:

- Het [instellen van beveiligingsbeleid in Azure Security Center](security-center-policies.md) --informatie over het configureren van beveiligingsbeleid voor uw abonnementen Azure en resourcegroepen.
- [Aanbevelingen voor de beveiliging beheren in Azure Security Center](security-center-recommendations.md) --informatie over het aanbevelingen helpen beschermen uw Azure resources.
- [Statuscontrole voor beveiliging in Azure Security Center](security-center-monitoring.md) --informatie over het controleren van de gezondheid van uw resources Azure.
- [Beheren en reageren op de beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) --informatie over het beheren van en reageren op de beveiligingswaarschuwingen.
- [Monitoring van partneroplossingen met Azure Security Center](security-center-partner-solutions.md) --informatie over het controleren van de status van uw oplossingen van partners.
- [Veelgestelde vragen over het azure Security Center](security-center-faq.md) --zoeken vaak gestelde vragen over het gebruik van de service.
- [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) --laatste Azure beveiliging nieuws en informatie.

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
