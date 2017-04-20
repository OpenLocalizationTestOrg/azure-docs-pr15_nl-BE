<properties
   pageTitle="Security Center prijzen | Microsoft Azure"
   description="Dit artikel bevat informatie over de prijzen voor Azure Security Center."
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
   ms.date="10/12/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-pricing"></a>Prijzen Azure Security Center

Azure Beveiligingscentrum helpt bij het voorkomen, opsporen en reageren op bedreigingen met een beter inzicht in en controle over de beveiliging van uw resources Azure. Het biedt geïntegreerde beveiliging beleid en het toezicht op verschillende de Azure abonnementen, helpt detecteren bedreigingen die anders opgemerkt en werkt met een brede selectie van beveiligingsoplossingen.

## <a name="pricing-tiers"></a>Prijzen lagen

Beveiligingscentrum is verkrijgbaar in twee lagen:

- De **gratis tier** wordt automatisch ingeschakeld op alle Azure abonnementen. De vrij laag biedt inzicht in de beveiligingsstatus van uw Azure bronnen, basic beveiligingsbeleid, aanbevelingen voor beveiliging en integratie met, beveiligingsproducten en diensten van partners.
- De **standaard laag** toegevoegd geavanceerde bedreiging detectiemogelijkheden zoals threat intelligence, doorgevoerd analyse afwijking detectie, veiligheidsincidenten en evaluatieverslagen bedreiging. Een **gratis proefversie gedurende 90 dagen** is beschikbaar zijn voor de standaard laag.

Zie het Beveiligingscentrum [prijzen pagina](https://azure.microsoft.com/pricing/details/security-center/)voor meer informatie.

> [AZURE.NOTE] Beveiligingscentrum gebruikt Azure opslag beveiligingsgegevens gegenereerd op basis van uw beveiligde knooppunten op te slaan. De kosten voor deze opslag niet zijn opgenomen in de prijs van de service en worden afzonderlijk in rekening gebracht op de reguliere [tarieven Azure opslag](https://azure.microsoft.com/pricing/details/storage/blobs/). Opslag geldt ook tijdens de evaluatieperiode.

## <a name="try-standard-free-for-90-days"></a>Standaard 90 dagen gratis proberen

Een gratis proefversie gedurende 90 dagen is beschikbaar zijn voor de standaard laag. Als u de gratis proefversie van de standaard laag, selecteert u het **beleid** naast elkaar op het blad **Security Center** . Selecteer het abonnement dat u wilt upgraden naar standaard. Selecteer op het blad **beveiligingsbeleid** **prijzen laag**. Selecteer **standaard – gratis proefperiode**op de bladeserver **kiezen de prijzen laag** .

![Gratis proefversie][1]

Aan het einde van 90 dagen, moet u kiezen om door te gaan met de service, we automatisch gestart oplaadmogelijkheden voor gebruik.

## <a name="upgrade-to-standard"></a>Upgrade naar standaard

Upgrade naar de Standard laag toe te voegen bedreiging geavanceerde detectie. Als u de standaard laag, selecteert u het **beleid** naast elkaar op het blad **Security Center** . Selecteer het abonnement dat u wilt upgraden naar standaard. Selecteer op het blad **beveiligingsbeleid** **prijzen laag**. Selecteer **standaard**op het blad **kiezen de prijzen laag** .

![Standaard laag][2]

## <a name="why-upgrade-to-standard"></a>Waarom upgraden naar standaard?

De standaard van Security Center biedt alle functies van de gratis tier plus geavanceerde bedreiging detectie. Geavanceerde bedreiging detectie kunt u actieve bedreigingen gericht uw Azure resources identificeren en biedt u de inzichten die nodig zijn om snel te reageren.

Security Center maakt gebruik van geavanceerde beveiliging analytics die ver voorbij de aanpak op basis van een handtekening. Doorbraken in grote gegevens en computer leren werken met technologies wordt gebruikt om gebeurtenissen over de hele cloud fabric – opsporen van bedreigingen die onmogelijk te identificeren met behulp van handmatige methoden en voorspellen van het verloop van de aanvallen te evalueren.

Security analytics die deel van de standaard laag uitmaken zijn:

- Bekende slechte acteurs zoekt **threat intelligence** - met behulp van global threat intelligence van Microsoft-producten en -services, de digitale misdrijven-eenheid van Microsoft, het Microsoft Security Response Center en externe feeds
- **Doorgevoerd analyse** - bekende patronen te ontdekken schadelijke gedrag van toepassing
- **Afwijking detectie** - gebruikt statistische profilering voor het maken van een historische basislijn. Wordt gewaarschuwd over afwijkingen van de vastgestelde basislijnen die aan een mogelijke aanvalsvector voldoen

Beveiligingscentrum is in de onderstaande blade **beveiligingswaarschuwingen** **incident**worden gedetecteerd. Een beveiligingsincident is een samenvoeging van alle waarschuwingen voor een bron uitgelijnd met kill keten patronen. Selecteren van het veiligheidsincident onthult meer details over het incident en geeft een overzicht van de verwante berichten. Een waarschuwing te selecteren vindt u meer informatie over dit exemplaar.

![Security incident][3]

De onderstaande **netwerkcommunicatie** waarschuwing bevat informatie over de waarschuwing. Details bevatten de volledige beschrijving, de ernst, de huidige toestand (die in dit geval wordt geannuleerd, wat betekent dat de gebruiker heeft actie om deze te verwijderen), de resource aangevallen en herstel. Er is ook een lijst met koppelingen naar Microsoft Threat Intelligence rapporten. Deze rapporten kunnen worden gebruikt voor beveiliging, doorvoeren en defensieve doeleinden.

![Security alert details][4]

## <a name="enable-data-collection"></a>Het verzamelen van gegevens inschakelen

Zodat de virtuele machine doorgevoerd analytics moet verzamelen van gegevens worden ingeschakeld. U moet het verzamelen van gegevens bij het openen van Beveiligingscentrum of bij het maken van een beveiligingsbeleid in te schakelen.

Om te valideren dat het verzamelen van gegevens is ingeschakeld, selecteert u de tegel **beleid** . Het **beveiligingsbeleid** blade opent uw Azure abonnementen aanbieden. Selecteer een abonnement. Als **het verzamelen van gegevens** uitgeschakeld is, wijzigen op de wijziging en sla. Zie [verzamelen in Azure Beveiligingscentrum inschakelen](security-center-enable-data-collection.md).

## <a name="next-steps"></a>Volgende stappen

- In dit document, hebt u kennisgemaakt prijzen voor Security Center. Voor meer informatie voor prijzen, Zie het Beveiligingscentrum [prijzen pagina](https://azure.microsoft.com/pricing/details/security-center/).
- Zie voor meer informatie over geavanceerde functies voor Security-Center, [Azure Beveiligingscentrum detectiemogelijkheden](security-center-detection-capabilities.md).
- Hebt u vragen over het gebruik van Security Center, Raadpleeg de [Veelgestelde vragen over het Azure Security Center](security-center-faq.md).
- Hebt u nog vragen over het gebruik van Beveiligingscentrum of Azure, Ga naar de [Forums Azure](https://social.msdn.microsoft.com/Forums/home?forum=AzureSecurityCenter&filter=alltypes&sort=lastpostdesc).

<!--Image references-->
[1]: ./media/security-center-pricing/free-trial.png
[2]: ./media/security-center-pricing/standard.png
[3]: ./media/security-center-pricing/incident.png
[4]: ./media/security-center-pricing/network-alert.png
