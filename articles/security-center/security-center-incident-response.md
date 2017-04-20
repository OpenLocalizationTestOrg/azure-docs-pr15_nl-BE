<properties
   pageTitle="Met Beveiligingscentrum Azure een beveiligingsplan | Microsoft Azure"
   description="Dit document wordt uitgelegd hoe u Azure Beveiligingscentrum gebruiken voor het scenario van een beveiligingsplan."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="yurid"/>

# <a name="using-azure-security-center-for-an-incident-response"></a>Een beveiligingsplan met behulp van Azure Security Center
Veel organisaties ontdekken hoe u wilt reageren op beveiligingsincidenten na het lijden van een aanval. Om kosten te verlagen en schade, is het belangrijk dat u een beveiligingsplan plan op zijn plaats voordat een aanval plaatsvindt. U kunt Beveiligingscentrum Azure in verschillende stadia van een beveiligingsplan.

## <a name="incident-response-planning"></a>Beveiligingsplan planning

Een effectieve planning is afhankelijk van drie kernfuncties: om te kunnen beschermen, detecteren en reageren op bedreigingen. Antwoord is over het verwijderen van de aanvaller en het herstellen van systemen te beperken van de gevolgen van een inbreuk op de beveiliging is over het voorkomen van incidenten en detectie is over het identificeren van bedreigingen vroeg.

In dit artikel wordt security incident response stadia van het artikel [Microsoft Azure Security Response in de Cloud](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678) gebruiken, zoals in het volgende diagram:

![Levenscyclus van een incident response](./media/security-center-incident-response/security-center-incident-response-fig1.png)

Tijdens de fasen van de analyse, beoordeling en Diagnose kunt u Beveiligingscentrum. Hier volgen voorbeelden van hoe Beveiligingscentrum bruikbaar tijdens de drie eerste beveiligingsplan fasen zijn kunnen:

- **Analyse**: de eerste vermelding van een onderzoek van de gebeurtenis te bekijken.
    - Voorbeeld: Lees de eerste verificatie dat een beveiligingswaarschuwing hoge prioriteit in het dashboard Security Center is verheven.
- **Beoordeling**: de eerste beoordeling voor meer informatie over verdachte activiteiten uitvoeren.
    - Voorbeeld: meer informatie verkrijgen over de beveiligingswaarschuwing.
- **Diagnose**: een technisch onderzoek te verrichten en het identificeren van strategieën voor insluiting, risicobeperking en tijdelijke oplossing.
    - Voorbeeld: stappen uit de herstelmaatregelen door Beveiligingscentrum in die bepaalde Beveiligingswaarschuwing beschreven.

Het volgende scenario wordt beschreven hoe u Beveiligingscentrum gebruiken tijdens de analyse, beoordeling en Diagnose/reageren fasen van een veiligheidsincident. In Beveiligingscentrum is een [veiligheidsincident](security-center-incident.md) een samenvoeging van alle waarschuwingen voor een bron uitgelijnd met patronen [kill-keten](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) . Incidenten worden weergegeven in de tegel [beveiligingswaarschuwingen](security-center-managing-and-responding-alerts.md) en blade. Een incident blijkt uit de lijst met verwante waarschuwingen, waarmee u meer informatie verkrijgen over elk exemplaar. Security Center biedt ook zelfstandige beveiligingswaarschuwingen die ook kunnen worden gebruikt voor het opsporen van een verdachte activiteiten.

## <a name="scenario"></a>Scenario

Contoso gemigreerd onlangs enkele van hun bronnen op ruimten naar Azure, met inbegrip van sommige bedrijfs workloads op basis van een virtuele machine en SQL-databases. Van Contoso Core Computer Security Incident Response Team (CSIRT) heeft momenteel een probleem onderzoekt beveiligingsproblemen door security intelligence niet in hun huidige incident response-hulpprogramma's wordt geïntegreerd. Dit gebrek aan integratie introduceert een probleem tijdens de analyse fase (te veel vals-positief zijn), en tijdens de beoordelings- en Diagnose-fasen. Als onderdeel van deze migratie besloten ze voor Security Center om dit probleem te kiezen.

De eerste fase van deze migratie voltooid nadat ze onboarded alle bronnen en alle aanbevelingen voor de beveiliging van Beveiligingscentrum worden behandeld. Contoso CSIRT moet het centrale punt voor het omgaan met computerbeveiliging. Het team bestaat uit een groep van mensen met verantwoordelijkheden voor de behandeling van een veiligheidsincident. De leden van het team beschikken over duidelijk gedefinieerde taken om ervoor te zorgen dat er geen gebied van het antwoord blijft vanuit een ongedekte positie.

Voor dit scenario gaan we focus op de rollen van de volgende personas die deel van het CSIRT Contoso uitmaken:

![Levenscyclus van een incident response](./media/security-center-incident-response/security-center-incident-response-fig2.png)

Judith is bij beveiligingsbewerkingen. Haar verantwoordelijkheden zijn:

- Toezicht op en de klok rond anticiperen op beveiligingsrisico's.
- De wolk werkbelasting eigenaar of security analist zo nodig escaleren.

SAM is een security analist en zijn verantwoordelijkheden opnemen:

- Onderzoekt aanvallen.
- Waarschuwingen opnieuw te proberen.
- Werken met de werkbelasting eigenaars vaststellen en toepassen van beperkende maatregelen.

Zoals u ziet, Judith en Sam hebben verschillende verantwoordelijkheden en ze moeten samenwerken om Beveiligingscentrum informatie te delen.

## <a name="recommended-solution"></a>Aanbevolen oplossing

Nadat Judith en Sam verschillende rollen hebben, ze gebruikt verschillende gebieden van Beveiligingscentrum verkrijgen van relevante informatie voor hun dagelijkse activiteiten. Judith gebruikt als onderdeel van haar dagelijkse controle **beveiligingswaarschuwingen** .

![Beveiligingswaarschuwingen](./media/security-center-incident-response/security-center-incident-response-fig3.png)

Judith beveiligingswaarschuwingen gebruikt tijdens de fasen van de analyse en evaluatie. Nadat Judith de eerste beoordeling is, kan ze het probleem naar Sam escaleren als verder onderzoek nodig is. Op dit moment Sam gebruikt de informatie die is geleverd door Beveiligingscentrum, soms in combinatie met andere gegevensbronnen te verplaatsen naar het werkgebied Diagnose.


## <a name="how-to-implement-this-solution"></a>Het implementeren van deze oplossing

Als u wilt zien hoe u in het geval van een beveiligingsplan Azure Beveiligingscentrum gebruiken, we stappen in de stadia van de analyse en beoordeling van Judith, en dan zien wat Sam doet voor het vaststellen van het probleem.

### <a name="detect-and-assess-incident-response-stages"></a>Detecteren en fasen beveiligingsplan beoordelen

Judith aangemeld bij de Azure portal en werkt in de console Security Center. Als onderdeel van haar dagelijkse activiteiten te controleren, begonnen zij belangrijke waarschuwingen controleren door de volgende stappen uit te voeren:

1. Klik op de tegel **beveiligingswaarschuwingen** en toegang tot de blade **beveiligingswaarschuwingen** .
    ![Security alert blade](./media/security-center-incident-response/security-center-incident-response-fig4.png)

    > [AZURE.NOTE] Voor dit scenario gaat Judith een beoordeling uitvoeren op de waarschuwing die schadelijke SQL activiteit, zoals in de voorgaande afbeelding.
2. Klik op de melding **activiteiten schadelijke SQL** en aangevallen bronnen in de blade **schadelijke SQL activiteit** bekijken:  ![Incident details](./media/security-center-incident-response/security-center-incident-response-fig5.png)

    In deze blade, Judith notities met betrekking tot de bronnen aangevallen, hoe vaak deze aanval heeft plaatsgevonden en wanneer zij is vastgesteld.
3. Klik op de **resource aangevallen** voor meer informatie over deze aanval.

Na het lezen van de beschrijving, Judith ervan overtuigd is dat dit niet een valse melding is en dat zij deze kwestie naar Sam te escaleren.

### <a name="diagnose-incident-response-stage"></a>Beveiligingsplan fase opsporen

SAM het geval van Judith ontvangt en begint met het controleren van Security Center voorgestelde stappen doorvoeren.

![Levenscyclus van een incident response](./media/security-center-incident-response/security-center-incident-response-fig6.png)

### <a name="additional-resources"></a>Aanvullende bronnen

Het incident response team kan ook gebruikmaken van de mogelijkheid [Security Center Power BI](security-center-powerbi.md) voor een overzicht van verschillende soorten rapporten. Deze rapporten helpen ze bij verder onderzoek te visualiseren, analyseren en aanbevelingen en waarschuwingen te filteren. Voor bedrijven die hun oplossingen gebeurtenis (SIEM) voor informatie over beveiliging en tijdens het onderzoek worden gebruikt, kunnen ze ook [Security Center met hun oplossing integreren](security-center-integrating-alerts-with-log-integration.md). Ook kunt u met het [hulpprogramma Azure logboek](https://blogs.msdn.microsoft.com/azuresecurity/2016/07/21/microsoft-azure-log-integration-preview/)Azure controlelogboeken en beveiligingsgebeurtenissen voor virtuele machine (VM) integreren. Als u wilt analyseren in een aanval, kunt u deze informatie in combinatie met de Security Center biedt informatie.


## <a name="conclusion"></a>Conclusie

Een team samenstellen voordat een incident voordoet, is erg belangrijk voor uw organisatie en wordt positief beïnvloeden hoe incidenten worden afgehandeld. Met de juiste hulpmiddelen voor het controleren van resources kunt u dit team nauwkeurige stappen uitvoeren om te herstellen van een veiligheidsincident. Security Center [detectiemogelijkheden](security-center-detection-capabilities.md) kunnen helpen IT om snel te reageren op beveiligingsincidenten en verhelpen van problemen met de beveiliging.
