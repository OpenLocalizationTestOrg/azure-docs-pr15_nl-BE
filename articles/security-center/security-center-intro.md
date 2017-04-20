<properties
   pageTitle="Inleiding tot Azure Security Center | Microsoft Azure"
   description="Informatie over Azure Security Center, de belangrijkste mogelijkheden en hoe het werkt."
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
   ms.date="07/21/2016"
   ms.author="terrylan"/>

# <a name="introduction-to-azure-security-center"></a>Inleiding tot Azure Security Center

Informatie over Azure Security Center, de belangrijkste mogelijkheden en hoe het werkt.

> [AZURE.NOTE] Dit document bevat de service met behulp van een voorbeeld van de implementatie.

## <a name="what-is-azure-security-center"></a>Wat is Azure Security Center?
 Beveiligingscentrum helpt u bij het voorkomen, opsporen en reageren op bedreigingen met een beter inzicht in en controle over de beveiliging van uw resources Azure. Het biedt geïntegreerde beveiliging beleid en het toezicht op verschillende de Azure abonnementen, helpt detecteren bedreigingen die anders opgemerkt en werkt met een brede selectie van beveiligingsoplossingen.

##  <a name="key-capabilities"></a>Belangrijkste mogelijkheden
 Security Center biedt eenvoudig te gebruiken en daadwerkelijke bedreiging preventie, detectie en reactie mogelijkheden die zijn ingebouwd in Azure. Belangrijkste mogelijkheden zijn:

| | |
|----- |-----|
| Voorkomen | Controleert de beveiligingsstatus van uw resources Azure |
| | Beleid voor uw abonnementen Azure en resourcegroepen op basis van de vereisten van de beveiliging van uw bedrijf, definieert de typen toepassingen die u gebruikt en de gevoeligheid van de gegevens |
| | Aanbevelingen voor de beveiliging beleidsgerichte gebruikt als richtlijn voor eigenaren van service aan de hand van de besturingselementen nodig |
| | Snel implementeren van beveiligingsservices en -toestellen van Microsoft en partners |
| Detecteren |Automatisch verzamelt en analyseert beveiligingsgegevens van uw resources Azure, het netwerk en oplossingen van partners zoals antimalware programma's en firewalls |
| | Globale hefboomwerkingen dreiging bedrijfsinformatie van Microsoft-producten en -services, de Microsoft Digital misdrijven eenheid (DCU), het Microsoft Security Response Center (MSRC) en externe feeds |
| | Geavanceerde analytics, met inbegrip van machine learning en doorgevoerd analyse van toepassing is |
| Reageren | Prioriteit security waarschuwingen incidenten / |
| | Biedt inzicht in de bron van de aanval en risico middelen |
| | Suggesties voor het stoppen van de aanval van de huidige en toekomstige aanvallen te voorkomen |

## <a name="introductory-walkthrough"></a>Inleidende scenario
 U openen Beveiligingscentrum vanaf de [portal Azure](https://azure.microsoft.com/features/azure-portal/). [Aanmelden bij de portal](https://portal.azure.com), selecteer **Bladeren**, en blader naar de optie **Security Center** of de **Security Center** -tegel die u eerder hebt vastgemaakt aan het dashboard van de portal.

![Beveiliging tegel in Azure portal][1]

In Beveiligingscentrum kunt u beveiligingsbeleid instellen, beveiligingsconfiguraties controleren en waarschuwingen weergeven.

### <a name="security-policies"></a>Beveiligingsbeleid

U kunt beleid voor uw abonnementen Azure en resourcegroepen, afhankelijk van de vereisten voor de beveiliging van uw bedrijf bepalen. U kunt ook op maat maken ze de typen toepassingen die u gebruikt of de gevoeligheid van de gegevens in elk abonnement. Bronnen die worden gebruikt voor het ontwikkelen of testen kunnen bijvoorbeeld verschillende beveiligingseisen dan die gebruikt voor productietoepassingen. Toepassingen met gereglementeerde gegevens, zoals persoonlijke gegevens kunnen ook een hoger niveau van beveiliging vereisen.

> [AZURE.NOTE] Als u wilt wijzigen van een beveiligingsbeleid op het niveau van het abonnement of de resource groep, moet u de eigenaar van het abonnement of een rol.

Selecteer de tegel **beleid** voor een lijst van uw abonnementen en resourcegroepen op het blad **Security Center** .   

![Security Center-blade][2]

Selecteer een abonnement op de details van het beleid op het **beveiligingsbeleid** blade.

![Security policy blade-abonnement][3]

**Het verzamelen van gegevens** (Zie boven) kunt gegevens verzamelen voor een beveiligingsbeleid. Het inschakelen van biedt:

- Virtuele machines dagelijks scannen van alle voor de controle van de beveiliging en aanbevelingen worden ondersteund.
- Verzameling van beveiligingsgebeurtenissen voor de detectie van analyse en bedreiging.

**Kies een account voor opslag per regio** (Zie boven) kunt u kiezen voor elke regio waarin u virtuele machines die worden uitgevoerd, hebben de opslag account waarin gegevens verzameld van deze virtuele machines worden opgeslagen. Als u niet voor elke regio een account opslag kiest, wordt deze voor u gemaakt. De gegevens die worden verzameld logisch gescheiden is van de gegevens van andere klanten om veiligheidsredenen.

> [AZURE.NOTE] Het verzamelen van gegevens en een account voor opslag per regio te kiezen is geconfigureerd op het niveau van het abonnement.

Selecteer **preventiebeleid** (Zie hierboven) voor het openen van het **preventiebeleid voor** blade. **Aanbevelingen voor het weergeven** , kunt u de besturingselementen die u wilt controleren en het beste voor de beveiliging op basis van de beveiligingsbehoeften van de bronnen in het abonnement kiezen.

Selecteer een groep om de details weer te geven.

![Security policy blade resourcegroep][4]

**Overname** (Zie boven) kunt u de resourcegroep als definiëren:

- (Standaard) overgenomen wat betekent dat alle beveiligingsbeleid voor deze resourcegroep worden overgenomen van het abonnement.
- Unieke wat betekent dat de resourcegroep heeft een aangepast beveiligingsbeleid. U moet wijzigingen onder **aanbevelingen voor weergeven**.

> [AZURE.NOTE] Als er een conflict tussen een abonnement op beleid en resource group level, de resource groep niveau beleid gaat.

### <a name="security-recommendations"></a>Aanbevelingen voor beveiliging

 Beveiligingscentrum analyseert de beveiligingsstatus Azure resources voor het identificeren van potentiële beveiligingsproblemen. Een lijst met aanbevelingen begeleidt u door het proces van het benodigde besturingselementen configureren. Voorbeelden zijn:

- Inrichten antimalware te kunnen identificeren en verwijderen van schadelijke software
- Netwerk-beveiligingsgroepen en regels voor het verkeer voor netwerkbesturing aan virtuele machines configureren
- Web application firewalls te beschermen tegen aanvallen die doel inrichten van uw webtoepassingen
- Ontbrekende systeemupdates implementeren
- OS-configuraties die niet overeenkomen met de aanbevolen basislijnen adressering

Klik op de tegel **aanbevelingen** voor een lijst met aanbevelingen. Klik op elke aanbeveling om aanvullende informatie weer te geven of te doen om het probleem te verhelpen.

![Azure Security Center aanbevelingen voor beveiliging][5]

### <a name="resource-health"></a>Resource-gezondheid

De **Resource security health** tegel toont de houding van de algehele beveiliging van het milieu door het resourcetype, met inbegrip van virtuele machines, toepassingen en andere bronnen.   

Selecteer een resourcetype op de tegel **Resource beveiliging gezondheid** voor meer informatie, inclusief een lijst van alle potentiële beveiligingsproblemen zijn geïdentificeerd. (**Virtuele machines** is geselecteerd in het voorbeeld hieronder).

![Resources gezondheid naast elkaar][6]

### <a name="security-alerts"></a>Beveiligingswaarschuwingen

 Security Center automatisch worden verzameld, geanalyseerd en logboekgegevens van uw resources Azure, het netwerk en oplossingen van partners zoals antimalware programma's en firewalls integreert. Wanneer er bedreigingen zijn gedetecteerd, wordt een beveiligingswaarschuwing wordt gemaakt. Detectie van enkele voorbeelden:

- Gemanipuleerde virtuele machines communiceren met bekende schadelijke IP-adressen
- Geavanceerde malware gedetecteerd door Windows Foutrapportage gebruiken
- Brute kracht aanvallen op virtuele machines
- Beveiligingswaarschuwingen van geïntegreerde antimalware programma's en firewalls

Een lijst met naar prioriteit gerangschikte waarschuwingen te klikken op de tegel **beveiligingswaarschuwingen** worden weergegeven.

![Beveiligingswaarschuwingen][7]

Ziet u meer informatie over de aanval en suggesties voor het verhelpen van het selecteren van een waarschuwing.

![Security alert details][8]

### <a name="partner-solutions"></a>Oplossingen van partners

De **oplossingen van partners** naast elkaar kunt controleren in een oogopslag de status van uw partneroplossingen is geïntegreerd met uw abonnement Azure. Berichten die afkomstig zijn van de oplossingen wordt in Beveiligingscentrum weergegeven.

Selecteer de tegel **oplossingen van partners** . Een blade wordt geopend met een lijst van alle aangesloten partneroplossingen.

![Oplossingen van partners][9]

## <a name="get-started"></a>Aan de slag
Om te beginnen met Security Center, moet u een abonnement op Microsoft Azure. Beveiligingscentrum is ingeschakeld bij uw abonnement Azure. Als u niet een abonnement hebt, kunt u zich aanmelden voor een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).

 U openen Beveiligingscentrum vanaf de [portal Azure](https://azure.microsoft.com/features/azure-portal/). Raadpleeg de [documentatie van de portal](https://azure.microsoft.com/documentation/services/azure-portal/) voor meer informatie.

[Aan de slag met Azure Security Center](security-center-get-started.md) leidt snel u door middel van de security-controle en -beheer van onderdelen van Security Center.

## <a name="see-also"></a>Zie ook
In dit document, kunt u kennisgemaakt met Security Center, de belangrijkste mogelijkheden en hoe aan de slag. Voor meer informatie, Zie de volgende:

- [Het instellen van beveiligingsbeleid in Beveiligingscentrum Azure](security-center-policies.md) , informatie over het configureren van beveiligingsbeleid voor uw abonnementen Azure en resourcegroepen.
- [Azure Security Center aanbevelingen voor beveiliging te beheren](security-center-recommendations.md) , informatie over het aanbevelingen helpen beschermen uw Azure resources.
- [Security health monitoring in Beveiligingscentrum Azure](security-center-monitoring.md) , informatie over het controleren van de gezondheid van uw resources Azure.
- [Beheren en reageren op de beveiligingswaarschuwingen in Beveiligingscentrum Azure](security-center-managing-and-responding-alerts.md) , informatie over het beheren van en reageren op de beveiligingswaarschuwingen.
- [Monitoring oplossingen van partners met Azure Security Center](security-center-partner-solutions.md) , informatie over het controleren van de status van uw oplossingen van partners.
- [Veelgestelde vragen over het Azure Security Center](security-center-faq.md) , zoeken, veelgestelde vragen over het gebruik van de service.
- [Beveiliging Azure blog](http://blogs.msdn.com/b/azuresecurity/) , de meest recente Azure beveiliging nieuws en informatie.

<!--Image references-->
[1]: ./media/security-center-intro/security-tile.png
[2]: ./media/security-center-intro/security-center.png
[3]: ./media/security-center-intro/security-policy.png
[4]: ./media/security-center-intro/security-policy-blade.png
[5]: ./media/security-center-intro/recommendations.png
[6]: ./media/security-center-intro/resources-health.png
[7]: ./media/security-center-intro/security-alert.png
[8]: ./media/security-center-intro/security-alert-detail.png
[9]: ./media/security-center-intro/partner-solutions.png
