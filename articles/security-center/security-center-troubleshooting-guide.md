<properties
   pageTitle="Beveiligingscentrum Azure Troubleshooting Guide | Microsoft Azure"
   description="Dit document helpt bij het oplossen van problemen in Azure Security Center."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-troubleshooting-guide"></a>Beveiligingscentrum Azure Troubleshooting Guide
Deze handleiding is bedoeld voor IT-specialisten (IT), informatie beveiliging analisten en cloud beheerders waarvan organisaties Azure Beveiligingscentrum gebruikt en problemen met dat vraagstukken in verband met Security Center.

## <a name="troubleshooting-guide"></a>Problemen oplossen met
Deze handleiding wordt beschreven voor het oplossen van problemen met de Security Center. De meeste van de methode voor troubleshooting in Security Center wordt gedaan zal plaatsvinden door eerst te zoeken naar records voor het defecte onderdeel [Controlelogboek](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) . Door middel van controlelogboeken, kunt u het volgende bepalen:

- Welke bewerkingen zijn plaatsgevonden
- Wie heeft de bewerking gestart
- Wanneer de bewerking is opgetreden
- De status van de bewerking.
- De werking van de waarden van andere eigenschappen die u kunnen helpen bij onderzoek

Het controlelogboek bevat alle schrijfbewerkingen (PUT, POST, verwijderen) op uw bronnen, maar geen leesbewerkingen (GET bevat).

## <a name="troubleshooting-monitoring-agent-installation-in-windows"></a>Monitoring agent-installatie in Windows oplossen

De Security Center monitoring agent wordt gebruikt voor het verzamelen van gegevens uitvoeren. Na het verzamelen van gegevens is ingeschakeld en de agent correct is geïnstalleerd op de doelcomputer, moet deze processen worden uitgevoerd:

- ASMAgentLauncher.exe - Azure Monitoring Agent 
- ASMMonitoringAgent.exe - extensie Azure beveiliging controleren
- ASMSoftwareScanner.exe – Azure Scan Manager

De extensie Azure Security Monitoring zoekt naar verschillende relevante Beveiligingsconfiguratie en verzamelt beveiligingslogboeken van de virtuele machine. De manager van de scan wordt gebruikt als een patch-scanner.

Als de installatie met succes is uitgevoerd worden er een post die vergelijkbaar zijn met die in de controlelogboeken voor het doel VM hieronder:

![Controlelogboeken](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig1.png)

Verder kunt u meer informatie wilt over het installatieproces door te lezen van de logboekbestanden van de agent, te vinden op *%systemdrive%\windowsazure\logs* (voorbeeld: C:\WindowsAzure\Logs).

> [AZURE.NOTE] Als de Azure Security Center-Agent problemen oplevert, moet u opnieuw het doel VM omdat er geen opdracht is te stoppen en starten van de agent.

## <a name="troubleshooting-monitoring-agent-installation-in-linux"></a>Problemen oplossen bij installatie van monitoring agent in Linux
Bij het oplossen van VM Agent-installatie in een Linux-systeem moet u ervoor zorgen dat de uitbreiding naar/var/lib/waagent/werd gedownload. De opdracht hieronder om te controleren of deze is geïnstalleerd, kunt u uitvoeren:

`cat /var/log/waagent.log` 

De andere logboekbestanden die u kunt bekijken voor het oplossen van doel zijn: 

- /var/log/mdsd.Err
- / var/log/azure /

In een werkend systeem ziet u een verbinding met het mdsd-proces op TCP-29130. Dit is de communicatie met het proces mdsd syslog. U kunt dit probleem valideren door de opdracht hieronder uit te voeren:

`netstat -plantu | grep 29130`

## <a name="contacting-microsoft-support"></a>Contact opnemen met Microsoft Support

Sommige problemen kunnen worden geïdentificeerd met behulp van de richtlijnen vermeld in dit artikel wordt beschreven anderen die kunt u vinden op het publieke [Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter)over Security Center. Maar als u meer wilt oplossen, kunt u een nieuw ondersteuningsverzoek Azure Portal gebruiken als volgt openen: 

![Ondersteuning van Microsoft](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)


## <a name="see-also"></a>Zie ook

In dit document, hebt u geleerd hoe beveiligingsbeleid configureren in Azure Security Center. Voor meer informatie over Azure Security Center, raadpleegt u het volgende:

- [Azure Security Center Planning en Operations Guide](security-center-planning-and-operations-guide.md) , informatie over het plannen en inzicht in de overwegingen Azure Beveiligingscentrum vast te stellen.
- [Security health monitoring in Beveiligingscentrum Azure](security-center-monitoring.md) , informatie over het controleren van de gezondheid van uw resources Azure
- [Beheren en reageren op de beveiligingswaarschuwingen in Beveiligingscentrum Azure](security-center-managing-and-responding-alerts.md) , informatie over het beheren van en reageren op de beveiligingswaarschuwingen
- [Monitoring oplossingen van partners met Azure Security Center](security-center-partner-solutions.md) , informatie over het controleren van de status van uw oplossingen van partners.
- [Azure Security Center FAQ](security-center-faq.md) , veelgestelde vragen over het gebruik van de service zoeken
- [Beveiligingsblog voor Azure](http://blogs.msdn.com/b/azuresecurity/) — vinden over Azure beveiliging en naleving van blogberichten
