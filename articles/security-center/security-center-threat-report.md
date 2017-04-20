<properties
   pageTitle="Beveiligingscentrum Azure Threat Intelligence Report | Microsoft Azure"
   description="Dit document kunt u intelligente rapporten van Azure Security Center bedreiging tijdens een onderzoek voor meer informatie over een beveiligingswaarschuwing."
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
   ms.date="10/17/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-threat-intelligence-report"></a>Beveiligingscentrum Azure Threat Intelligence Report
Dit document wordt uitgelegd hoe Azure Security Center bedreiging intelligente rapporten kunt u meer informatie over het risico dat een beveiligingswaarschuwing wordt gegenereerd.

## <a name="what-is-a-threat-intelligence-report"></a>Wat is een rapport threat intelligence?
Security Center bedreiging detectie werkt door te controleren, informatie over de beveiliging van uw resources Azure, de netwerk- en oplossingen van partners verbonden. Deze informatie is vaak gegevens uit meerdere bronnen, voor het identificeren van bedreigingen correleren wordt geanalyseerd. Dit proces is onderdeel van de Security Center- [functies](security-center-detection-capabilities.md). 

Wanneer Beveiligingscentrum een bedreiging vormt identificeert, wordt er een [Beveiligingswaarschuwing](security-center-managing-and-responding-alerts.md), waarin u gedetailleerde informatie over een bepaalde gebeurtenis, met inbegrip van suggesties voor herstel te starten. Om het incident response teams onderzoeken en verhelpen van bedreigingen, Security Center bevat een threat intelligence rapport met informatie over de bedreiging die is waargenomen, met inbegrip van informatie, zoals de: 

- De aanvaller van identiteit of verenigingen (als deze informatie beschikbaar is)
- Doelstellingen vereisen
- Actuele en historische aanval campagnes (als deze informatie beschikbaar is)
- Tactieken, hulpprogramma's en procedures vereisen
- Bijbehorende indicators van compromissen (IoC), zoals URL's en bestands-hashes
- Victimology die de industrie en de geografische prevalentie om u te helpen bij het bepalen of uw Azure middelen risico lopen
- Bestrijdings- en herstel van gegevens

>[AZURE.NOTE] De hoeveelheid gegevens in een bepaald rapport verschillen; het detailniveau is gebaseerd op de activiteit en de prevalentie van de malware.

Beveiligingscentrum heeft drie soorten rapporten bedreiging, die afhankelijk van de aanval variëren kunnen. De rapporten zijn beschikbaar:

- **Activiteitenoverzicht groep**: biedt verdiepen in aanvallers, hun doelstellingen en tactiek.
- **Rapport campagne**: richt zich op de details van een specifieke aanval campagnes. 
- **Rapport bedreiging**: heeft betrekking op alle items in de vorige twee verslagen.

Dit soort informatie is zeer nuttig tijdens het [beveiligingsplan](security-center-incident-response.md) , indien er een voortdurend onderzoek te begrijpen van de bron van de aanval, de beweegredenen van de aanvaller en wat u moet doen om dit probleem naar voren verplaatsen te beperken. 

## <a name="how-to-access-the-threat-intelligence-report"></a>Hoe kan ik toegang tot het rapport threat intelligence?

De huidige waarschuwingen kunt u controleren door te kijken naar de tegel **beveiligingswaarschuwingen** . De Portal Azure openen en volg de stappen hieronder voor meer details over elke waarschuwing:

1. Op het dashboard Security Center ziet u de tegel **beveiligingswaarschuwingen** .

2. Klik op de tegel om de **beveiligingswaarschuwingen** bladeserver met meer details over de waarschuwingen openen en klik op de beveiligingswaarschuwing dat u meer informatie krijgen wilt over.

    ![Beveiligingswaarschuwingen](./media/security-center-threat-report/security-center-threat-report-fig1.png)

3. In dit geval wordt de **verdachte proces uitgevoerd** bladeserver de details over de waarschuwing zoals weergegeven in de onderstaande figuur:

    ![Security alert detais](./media/security-center-threat-report/security-center-threat-report-fig2.png)

4.  De hoeveelheid informatie die beschikbaar is voor elke beveiligingswaarschuwing zijn afhankelijk van het type waarschuwing. In het veld **rapporten** hebt u een koppeling naar het rapport threat intelligence. Klik erop en een ander browservenster wordt weergegeven met een PDF-bestand.

    ![Selectie van de opslag](./media/security-center-threat-report/security-center-threat-report-fig3.png)

Hier kunt u downloaden van het PDF-bestand voor dit rapport en lees meer over het beveiligingsprobleem dat is vastgesteld en acties op basis van de verstrekte informatie.

## <a name="see-also"></a>Zie ook

In dit document, hebt u geleerd hoe u intelligente rapporten van Azure Security Center bedreiging kunt tijdens een onderzoek over beveiligingswaarschuwingen. Voor meer informatie over Azure Security Center, raadpleegt u het volgende:

- [Veelgestelde vragen over azure Security Center](security-center-faq.md). Veelgestelde vragen over het gebruik van de service vinden.
- [Gebruik van Beveiligingscentrum Azure beveiligingsplan](security-center-incident-response.md)
- [Detectiemogelijkheden Azure Security Center](security-center-detection-capabilities.md)
- [Azure Beveiligingscentrum planning en operations guide](security-center-planning-and-operations-guide.md). Informatie over het plannen en inzicht in de overwegingen Azure Beveiligingscentrum vast te stellen.
- [Beheren en reageren op de beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md). Informatie over het beheren van en reageren op de beveiligingswaarschuwingen.
- [Afhandelen van beveiligingsincidenten in Azure Security Center](security-center-incident.md)
- [Beveiligingsblog voor azure](http://blogs.msdn.com/b/azuresecurity/). Vinden van blogberichten over Azure beveiliging en naleving.
