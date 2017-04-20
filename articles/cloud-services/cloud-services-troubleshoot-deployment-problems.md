<properties
 pageTitle="Problemen oplossen met cloud-service implementatie | Microsoft Azure"
 description="Er zijn enkele veelvoorkomende problemen die u uitvoeren kunt in een cloud-service implementeert op Azure. In dit artikel vindt oplossingen voor enkele van deze."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="troubleshoot-cloud-service-deployment-problems"></a>Problemen oplossen met cloud service implementeren

Wanneer u een toepassingspakket cloud service op Azure implementeren, kunt u informatie over het installeren van het deelvenster **Eigenschappen** in de portal Azure. Kunt u de gegevens in dit deelvenster om te helpen bij het oplossen van problemen met de service cloud en kunt u deze informatie ter ondersteuning van Azure geven bij het openen van een nieuw ondersteuningsverzoek in.

U kunt het deelvenster **Eigenschappen** als volgt vinden:

* In het portal voor Azure klikt u op de implementatie van uw cloud-service, klikt u op **alle instellingen**en klik vervolgens op **Eigenschappen**.
* In de klassieke Azure portal, klik op de implementatie van uw cloud-service, **DASHBOARD**, te vinden op de rechterbenedenhoek van de pagina ( **Overzicht**). Let wel, er is geen label 'Eigenschappen' in dit venster.

> [AZURE.NOTE] U kunt de inhoud van het deelvenster **Eigenschappen** naar het Klembord kopiëren door te klikken op het pictogram in de rechterbovenhoek van het deelvenster.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="problem-i-cannot-access-my-website-but-my-deployment-is-started-and-all-role-instances-are-ready"></a>Probleem: ik kan geen toegang tot mijn website, maar mijn installatie wordt gestart en alle exemplaren van de rol gereed zijn

De poort niet wordt opgenomen in de website URL-koppeling weergegeven in de portal. De standaardpoort voor websites is 80. Als uw toepassing is geconfigureerd voor uitvoering op een andere poort, moet u het juiste poortnummer toevoegen aan de URL wanneer toegang tot de website.

1. Klik op de implementatie van uw cloud-service in het portal voor Azure.
2. Controleer de poorten voor de functie exemplaren (onder **Input eindpunten**) in het deelvenster **Eigenschappen** van de portal voor Azure.
3. Als de poort niet 80, moet u de juiste poortwaarde toevoegen aan de URL toegang te krijgen tot de toepassing. Als u een niet-standaard poort, typt u de URL, gevolgd door een dubbele punt (:), gevolgd door het poortnummer, zonder spaties.

## <a name="problem-my-role-instances-recycled-without-me-doing-anything"></a>Probleem: Mijn rol exemplaren gerecycled zonder mij iets te doen

Retoucheren service vindt automatisch plaats als Azure knooppunten probleem detecteert en daarom rol exemplaren naar nieuwe knooppunten verplaatst. Wanneer dit gebeurt, ziet u de rol exemplaren automatisch recyclen. Om erachter te komen als service retoucheren is opgetreden:

1. Klik op de implementatie van uw cloud-service in het portal voor Azure.
2. Bekijk de informatie in het deelvenster **Eigenschappen** van de portal voor Azure en bepalen of service retoucheren opgetreden op het moment dat u de rollen recycling waargenomen.

Rollen worden ook ongeveer recyclen eenmaal per maand tijdens de host-OS en Gast OS updates.  
Zie de blog boeken [Rol exemplaar opnieuw is opgestart vanwege Upgrades voor het besturingssysteem](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx) voor meer informatie.

## <a name="problem-i-cannot-do-a-vip-swap-and-receive-an-error"></a>Probleem: die ik niet kan doen een VIP-swap en een foutbericht weergegeven

Een VIP-swap is niet toegestaan als een update van de implementatie uitgevoerd wordt. Implementatie-updates automatisch kunnen optreden wanneer:

* Een gastbesturingssysteem beschikbaar is en u voor automatische updates worden geconfigureerd.
* Service retoucheren plaatsvindt.

Om erachter te komen als een automatische update wordt voorkomen door een VIP ruil doen:

1. Klik op de implementatie van uw cloud-service in het portal voor Azure.
2. Bekijk de waarde van de **Status**in het deelvenster **Eigenschappen** van de portal voor Azure. Als het **klaar**is, Controleer **laatste bewerking** te zien als een onlangs is gebeurd die mogelijk niet de swap VIP.
3. Herhaal stap 1 en 2 voor de productie-implementatie.
4. Als een automatische update uitgevoerd wordt, wacht tot deze is voltooid voordat u probeert de swap VIP doen.

## <a name="problem-a-role-instance-is-looping-between-started-initializing-busy-and-stopped"></a>Probleem: Een exemplaar van de rol is lus tussen gestart, Bezig met initialiseren, bezet en gestopt

Deze voorwaarde kan duiden op een probleem met uw code, pakket of de configuratie-bestand van de toepassing. In dat geval moet u de status wijzigen om de paar minuten kunnen en de Azure portal kan iets zegt zoals **Recycling**, **bezet**of **Bezig met initialiseren**. Dit geeft aan dat er iets mis met de toepassing waardoor het rol-exemplaar wordt uitgevoerd.

Voor meer informatie over het oplossen van dit probleem, Zie de blog boeken [Azure PaaS berekenen diagnostische gegevens](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx) en [algemene problemen die ervoor zorgen de rollen dat te recyclen](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

## <a name="problem-my-application-stopped-working"></a>Probleem: Mijn toepassing werkt niet

1. Klik op de instantie van de rol in het portal voor Azure.
2. Bekijk in het deelvenster **Eigenschappen** van de portal voor Azure oorzaken uw probleem op te lossen:
   * Als het exemplaar van de rol is onlangs gestopt (u kunt de waarde van **aantal afbrekingen**controleren), de implementatie kan worden bijgewerkt. Wacht om te zien als het exemplaar van de rol hervat op het eigen functioneren.
   * Als de instantie rol **bezet is**, Controleer uw toepassingscode om te zien als de gebeurtenis [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck) wordt verwerkt. U wilt toevoegen of corrigeren van bepaalde code die deze gebeurtenis verwerkt.
   * Doorloop de scenario's voor het oplossen van problemen in het weblogbericht [Azure PaaS berekenen diagnostische gegevens](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)en diagnostische gegevens.

>[AZURE.WARNING] Als u uw service cloud recyclen, opnieuw u de eigenschappen voor de implementatie, effectief wissen van de gegevens van het oorspronkelijke probleem.

## <a name="next-steps"></a>Volgende stappen

Bekijk meer [artikelen probleemoplossing](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) voor cloud services.

Als u wilt weten hoe u problemen met cloud service rol met Azure PaaS computer diagnostische gegevens, Zie [blog-serie van Kevin Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
