<properties
   pageTitle="Hybride Runbook werknemer: Een runbook taak wordt beëindigd met de status onderbroken | Microsoft Azure"
   description="Symptomen, oorzaken en oplossingen voor hybride Runbook werknemer taak beëindigen fout."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/17/2016"
   ms.author="magoedte" />

# <a name="hybrid-runbook-worker-a-runbook-job-terminates-with-a-status-of-suspended"></a>Hybride Runbook werknemer: Een runbook taak wordt beëindigd met de status onderbroken

## <a name="summary"></a>Samenvatting

De runbook is kort na het uitvoeren van deze drie keer onderbroken. Er zijn situaties waarin het runbook wordt voltooid kunnen worden onderbroken en het bijbehorende foutbericht bevat aanvullende informatie die aangeeft waarom niet. Dit artikel bevat stappen voor probleemoplossing voor problemen met betrekking tot fouten bij de uitvoering van de werknemer van hybride Runbook runbook.

Als uw Azure niet in dit artikel besproken wordt, gaat u naar de Azure forums op [MSDN en de Stack Overflow](https://azure.microsoft.com/support/forums/). U kunt het probleem op deze forums of boeken [ @AzureSupport op Twitter](https://twitter.com/AzureSupport). U kunt ook een Azure Ondersteuningsverzoek indienen door **ondersteuning** op de site [ondersteuning van Azure](https://azure.microsoft.com/support/options/) .

## <a name="symptom"></a>Symptoom

Uitvoering van Runbook mislukt en wordt de geretourneerde fout is, 'de taak actie 'Activeren' kan niet worden uitgevoerd omdat het proces is onverwacht gestopt. Actie voor de taak is 3 keer geprobeerd."


## <a name="cause"></a>Oorzaak

Er zijn verschillende mogelijke oorzaken voor de fout: 

  1. De hybride werknemer zich achter een proxy of firewall
  2. De computer waarop de werknemer hybride wordt uitgevoerd is minder dan de minimale hardware- [vereisten](automation-hybrid-runbook-worker.md#hybrid-runbook-worker-requirements) 
  3. De runbooks verifiëren niet met de lokale resources


## <a name="cause-1-hybrid-runbook-worker-is-behind-proxy-or-firewall"></a>Oorzaak 1: Hybride Runbook werknemer is achter een proxyserver of firewall

De computer actief is op de werknemer van hybride Runbook zich achter een firewall of proxy server en uitgaande toegang niet toegestaan of juist geconfigureerd.

### <a name="solution"></a>Oplossing

Controleer of de computer heeft een uitgaande toegang tot *. cloudapp.net op poort 443, 9354 en 30000 30199. 

## <a name="cause-2-computer-has-less-than-minimum-hardware-requirements"></a>Oorzaak 2: De Computer is minder dan de minimale hardwarevereisten

Computers met de hybride Runbook werknemer moeten voldoen aan de minimale hardwarevereisten voor de aanwijzing van deze als host voor deze functie. Gebruik anders afhankelijk van het brongebruik van andere achtergrondprocessen en het conflict veroorzaakt door het runbooks tijdens de uitvoering van de computer worden dan gebruikt en veroorzaken runbook taak vertragingen of time-outs. 

### <a name="solution"></a>Oplossing 

Controleer eerst de computer ingesteld voor het uitvoeren van de functie Hybride Runbook werknemer voldoet aan de minimale hardwarevereisten.  Als dit het geval is, controleren om na te gaan van een correlatie tussen de prestaties van hybride Runbook werkprocessen en Windows-CPU en geheugen gebruik.  Als er geheugen of CPU druk, kan dit duiden op de noodzaak om te upgraden of extra processors toevoegen of meer geheugen om het knelpunt resource te verhelpen van de fout. Ook een andere compute resource selecteren die de minimumeisen en de schaal ondersteunen kan wanneer de werkbelasting vraag geven dat een verhoging nodig is.         

## <a name="cause-3-runbooks-cannot-authenticate-with-local-resources"></a>Oorzaak 3: Runbooks niet verifiëren met de lokale resources

### <a name="solution"></a>Oplossing

Controleer het gebeurtenislogboek van **Microsoft SMA** voor een corresponderende gebeurtenis beschrijving *Win32-proces afgesloten met code [4294967295]*.  De oorzaak van deze fout is u nog niet hebt geconfigureerd verificatie in de runbooks of de referenties uitvoeren als voor de werknemer hybride groep opgegeven.  Lees [Runbook machtigingen](automation-hybrid-runbook-worker.md#runbook-permissions) om te bevestigen dat u verificatie correct hebt geconfigureerd voor uw runbooks.  


 

