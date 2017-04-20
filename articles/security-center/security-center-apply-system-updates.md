<properties
   pageTitle="Systeemupdates in Beveiligingscentrum Azure toepassen | Microsoft Azure"
   description="Dit document wordt beschreven hoe u de Azure Security Center aanbevelingen **systeemupdates toe te passen** en **opnieuw opstarten na systeemupdates**implementeren."
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

# <a name="apply-system-updates-in-azure-security-center"></a>Systeemupdates in Beveiligingscentrum Azure toepassen

Azure Beveiligingscentrum controleert dagelijks Windows en Linux virtuele machines (VMs) voor het besturingssysteem-updates ontbreken. Security Center haalt een lijst van de beschikbare beveiligingsupdates en essentiële updates van Windows Update of Windows Server Update Services (WSUS), afhankelijk van welke service is geconfigureerd op een Windows VM.  Beveiligingscentrum controleert ook of de meest recente updates in Linux-systemen. Als u een systeemupdate van uw VM ontbreekt, wordt Security Center aangeraden systeemupdates

> [AZURE.NOTE] Dit document bevat de service met behulp van een voorbeeld van de implementatie.  Dit is een stapsgewijze handleiding.

## <a name="implement-the-recommendation"></a>Implementatie van de aanbeveling

1. Selecteer in het blad **aanbevelingen** **systeemupdates toepassen**.
![Systeemupdates toepassen][1]

2. De blade **systeemupdates toepassen** wordt geopend met een lijst van VMs-systeemupdates ontbreken. Selecteer een VM.
![Selecteer een VM][2]

3. Een blade wordt geopend met een lijst van ontbrekende updates voor deze VM. Selecteer een systeemupdate. In dit voorbeeld gaan we KB3156016 selecteren.
![Ontbrekende beveiligingsupdates][3]

4. Volg de stappen in de blade **-Beveiligingsupdate** toepassen van de ontbrekende update.
![Beveiligingsupdate][4]

## <a name="reboot-after-system-updates"></a>Opnieuw opstarten na systeemupdates

5. Terug naar de blade **aanbevelingen** . Een nieuwe post is gegenereerd nadat u systeemupdates, genaamd **opnieuw opstarten na systeemupdates**toegepast. Deze vermelding kunt u weet dat u moet opnieuw worden opgestart om het proces van het systeemupdates toepassen VM.
![Opnieuw opstarten na systeemupdates][5]

6. Selecteer **opnieuw opstarten na systeemupdates**. Hiermee opent u **dat een opnieuw zal worden opgestart voor het voltooien van systeemupdates** blade weergegeven met een lijst van VMs die u nodig hebt op te starten om het proces toepassen systeem updates te voltooien.
![Opnieuw gestart][6]

Start de VM in Azure om het proces te voltooien.

## <a name="see-also"></a>Zie ook

Voor meer informatie over de Security Center, raadpleegt u het volgende:

- Het [instellen van beveiligingsbeleid in Azure Security Center](security-center-policies.md) --informatie over het configureren van beveiligingsbeleid voor uw abonnementen Azure en resourcegroepen.
- [Aanbevelingen voor de beveiliging beheren in Azure Security Center](security-center-recommendations.md) --informatie over het aanbevelingen helpen beschermen uw Azure resources.
- [Statuscontrole voor beveiliging in Azure Security Center](security-center-monitoring.md) --informatie over het controleren van de gezondheid van uw resources Azure.
- [Beheren en reageren op de beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) --informatie over het beheren van en reageren op de beveiligingswaarschuwingen.
- [Monitoring van partneroplossingen met Azure Security Center](security-center-partner-solutions.md) --informatie over het controleren van de status van uw oplossingen van partners.
- [Veelgestelde vragen over het azure Security Center](security-center-faq.md) --zoeken vaak gestelde vragen over het gebruik van de service.
- [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) --blog zoeken naar berichten over Azure beveiliging en naleving.

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/recommendation.png
[2]:./media/security-center-apply-system-updates/select-vm.png
[3]: ./media/security-center-apply-system-updates/missing-security-updates.png
[4]: ./media/security-center-apply-system-updates/security-update.png
[5]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[6]: ./media/security-center-apply-system-updates/restart-pending.png
