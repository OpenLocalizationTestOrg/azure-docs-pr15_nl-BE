<properties
    pageTitle="Optimalisatie van uw omgeving met de Active Directory-beoordeling-oplossing in logboek Analytics | Microsoft Azure"
    description="U kunt de beoordeling van Active Directory-oplossing voor de beoordeling van de risico's en de gezondheid van uw serveromgevingen met regelmatige tussenpozen."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="optimize-your-environment-with-the-active-directory-assessment-solution-in-log-analytics"></a>Optimalisatie van uw omgeving met de Active Directory-beoordeling-oplossing in logboek Analytics

U kunt de beoordeling van Active Directory-oplossing voor de beoordeling van de risico's en de gezondheid van uw serveromgevingen met regelmatige tussenpozen. Dit artikel helpt u bij het installeren en gebruiken van de oplossing, zodat u corrigerende maatregelen voor mogelijke problemen nemen kunt.

Deze oplossing biedt een naar prioriteit gerangschikte lijst van aanbevelingen die specifiek zijn voor uw serverinfrastructuur geïmplementeerd. De aanbevelingen worden onderverdeeld in vier focus gebieden waarmee u snel inzicht in de risico's en actie ondernemen.

De aanbevelingen zijn gebaseerd op de kennis en ervaring door technici van Microsoft uit duizenden klanten bezoeken. Elke aanbeveling verschaft leidraden over waarom het probleem mogelijk interessant voor u en het implementeren van de voorgestelde wijzigingen.

U kunt de focus gebieden die het meest belangrijk zijn voor uw organisatie en de voortgang ten opzichte van een risico-vrije en gezonde omgeving wordt uitgevoerd.

Nadat u de oplossing hebt toegevoegd en een evaluatie voltooid, samenvatting is gegevens voor focus gebieden weergegeven in het dashboard **AD beoordeling** voor de infrastructuur in uw omgeving. De volgende secties wordt beschreven hoe u de gegevens op het dashboard **AD beoordeling** , waar u kunt bekijken en aanbevolen acties uitvoeren voor de infrastructuur van Active Directory-server.

![afbeelding van de tegel SQL beoordeling](./media/log-analytics-ad-assessment/ad-tile.png)

![afbeelding van de beoordeling van de SQL-dashboard](./media/log-analytics-ad-assessment/ad-assessment.png)


## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende informatie te installeren en configureren van de oplossingen.

- Agenten moeten zijn geïnstalleerd op domeincontrollers die deel uitmaken van het domein moet worden geëvalueerd.
- De oplossing voor de beoordeling van Active Directory is geïnstalleerd op elke computer waarop een agent OMS van .NET Framework 4 nodig.
- De oplossing voor de beoordeling van Active Directory toevoegen aan uw Kantoorbeheersysteem werkruimte met behulp van het proces beschreven in [oplossingen logboek Analytics toevoegen uit de galerie met oplossingen](log-analytics-add-solutions.md).  Er is geen verdere configuratie nodig.

    >[AZURE.NOTE] Nadat u de oplossing hebt toegevoegd, wordt het bestand AdvisorAssessment.exe toegevoegd aan de servers met agenten. Configuratiegegevens lezen en vervolgens verzonden naar de OMS-service in de cloud voor verwerking. Logica wordt toegepast op de gegevens ontvangen en de service cloud gegevens.

## <a name="active-directory-assessment-data-collection-details"></a>Active Directory-beoordeling collectie detailgegevens

Beoordeling van Active Directory worden verzameld, WMI-gegevens, registergegevens en prestatiegegevens met de agenten die u hebt ingeschakeld.

De volgende tabel bevat methoden van de collectie gegevens voor agenten, of Operations Manager (SCOM) vereist is en hoe vaak de gegevens worden verzameld door een agent.

| platform | Directe Agent | SCOM agent | Azure opslag | SCOM nodig? | SCOM agent gegevens verzonden via management groep | frequentie van verzameling |
|---|---|---|---|---|---|---|
|Windows|![Ja](./media/log-analytics-ad-assessment/oms-bullet-green.png)|![Ja](./media/log-analytics-ad-assessment/oms-bullet-green.png)|![Nee](./media/log-analytics-ad-assessment/oms-bullet-red.png)|   ![Nee](./media/log-analytics-ad-assessment/oms-bullet-red.png)|![Ja](./media/log-analytics-ad-assessment/oms-bullet-green.png)| 7 dagen|


## <a name="understanding-how-recommendations-are-prioritized"></a>Begrijpen hoe prioriteit van aanbevelingen

Elke aanbeveling krijgt een waarde bij weging waarmee het relatieve belang van de aanbeveling. Alleen de tien belangrijkste aanbevelingen worden weergegeven.

### <a name="how-weights-are-calculated"></a>Berekening van gewichten

Wegingen worden samengestelde waarden op basis van drie belangrijke factoren:

- De *kans* dat een probleem vastgesteld zal problemen veroorzaken. Er is een grotere kans gelijk aan een grotere algemene score voor de aanbeveling.

- De *impact* van het probleem in uw organisatie als dit leidt een probleem tot. Een hogere impact is gelijk aan een grotere algemene score voor de aanbeveling.

- De *inspanning* vereist voor de uitvoering van de aanbeveling. Er is een hogere inzet gelijk aan een kleinere algemene score voor de aanbeveling.

De weging voor elke aanbeveling wordt uitgedrukt als een percentage van de totale score die beschikbaar zijn voor elk focusgebied. Bijvoorbeeld, als een aanbeveling in de beveiliging en naleving focusgebied heeft een score van 5%, verhoogt ter uitvoering van deze aanbeveling de algehele beveiliging en naleving score door 5%.

### <a name="focus-areas"></a>Focus gebieden

**Beveiliging en naleving** - dit aandachtsgebied bevat aanbevelingen voor de mogelijke beveiligingsrisico's en overtredingen, bedrijfsbeleid en aan technische, wettelijke en reglementaire vereisten.

**Beschikbaarheid en bedrijfscontinuïteit** - dit aandachtsgebied bevat aanbevelingen voor de beschikbaarheid van diensten, een betere beveiliging van uw infrastructuur en bescherming van de business.

**Prestaties en schaalbaarheid** - dit aandachtsgebied bevat aanbevelingen om u te helpen uw organisatie IT-infrastructuur groeit, zorg ervoor dat uw IT-omgeving voldoet aan de huidige prestatie-eisen, en reageren op veranderende behoeften van de infrastructuur.

**Upgrade, migratie en implementatie** - dit aandachtsgebied bevat aanbevelingen om te upgraden, migratie en implementatie van Active Directory op uw bestaande infrastructuur.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Moet u zijn gericht op 100% score in elk focusgebied?

Dat hoeft niet. De aanbevelingen zijn gebaseerd op de kennis en ervaring opgedaan door technici van Microsoft in duizenden klanten bezoeken. Geen twee server infrastructuren zijn echter hetzelfde, en specifieke aanbevelingen kunnen meer of minder relevant voor u zijn. Enkele beveiligingsaanbevelingen worden minder relevant als uw virtuele machines worden niet blootgesteld aan Internet. Enkele aanbevelingen beschikbaarheid mogelijk minder relevant zijn voor de diensten die zorgen voor lage prioriteit ad hoc-gegevensverzameling en rapportage. Kwesties die belangrijk voor een volwassen bedrijf zijn kunnen een startfase minder belangrijk zijn. U kunt bepalen welke gebieden focus zijn uw prioriteiten en bekijk hoe uw scores in de tijd wijzigen.

Elke aanbeveling bevat richtlijnen over waarom het belangrijk is. Gebruik deze richtlijnen om te bepalen of de implementatie van de aanbeveling is geschikt voor u, gezien de aard van uw IT-services en de zakelijke behoeften van uw organisatie.

## <a name="use-assessment-focus-area-recommendations"></a>Beoordeling focus gebied aanbevelingen gebruiken

Voordat u een oplossing voor de beoordeling in OMS gebruiken kunt, moet u de oplossing geïnstalleerd hebben. Meer informatie over het installeren van oplossingen, Zie [oplossingen voor logboek Analytics toevoegen uit de galerie met oplossingen](log-analytics-add-solutions.md). Nadat deze is geïnstalleerd, kunt u de samenvatting van de aanbevelingen weergeven met behulp van de beoordeling naast elkaar op de pagina overzicht van OMS.

De samengevatte naleving beoordelingen voor uw infrastructuur en inzoomen in aanbevelingen weergeven.


### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Aanbevelingen voor een focusgebied weergeven en corrigerende maatregelen nemen

1. Klik op de pagina **Overzicht** op de tegel van de **beoordeling** van de serverinfrastructuur.
2. Neem de samenvattingsinformatie in een van de bladen focus gebied op de pagina **beoordeling** en klik op een om aanbevelingen voor dat focusgebied weer te geven.
3. U kunt op een van de focus gebied's prioriteit aanbevelingen voor uw omgeving bekijken. Klik op een aanbeveling onder **Invloed op objecten** weer te geven informatie over waarom de aanbeveling wordt gedaan.  
    ![afbeelding van de aanbevelingen van de evaluatie](./media/log-analytics-ad-assessment/ad-focus.png)
4. U kunt nemen corrigerende maatregelen voorgesteld in de **Voorgestelde acties**. Als het item is behandeld, record latere beoordelingen die aanbevolen acties zijn ondernomen en uw score naleving zal toenemen. Herstelde items worden weergegeven als **Objecten doorgegeven**.

## <a name="ignore-recommendations"></a>Aanbevelingen negeren

Als u aanbevelingen die u wilt negeren, kunt u een tekstbestand dat OMS wordt gebruikt om te voorkomen dat de aanbevelingen die worden weergegeven in de resultaten van de beoordeling.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Aanbevelingen die u negeert identificeren

1.  Aanmelden bij uw werkruimte en logboek zoeken te openen. Gebruik de volgende query om een lijst met aanbevelingen die niet zijn voor computers in uw omgeving.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    Hier is een Schermafdruk waarin wordt de query Log: ![aanbevelingen is mislukt](./media/log-analytics-ad-assessment/ad-failed-recommendations.png)

2.  Kies de aanbevelingen die u wilt negeren. Gebruikt u de waarden voor RecommendationId in de volgende procedure.


### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Maken en gebruiken van een tekstbestand IgnoreRecommendations.txt

1.  Maak een bestand met de naam IgnoreRecommendations.txt.
2.  Plak of typ elke RecommendationId voor elke aanbeveling dat logboek Analytics te negeren op een afzonderlijke regel en vervolgens op te slaan en sluit het bestand.
3.  Plaats het bestand in de volgende map op elke computer waar u OMS aanbevelingen negeren.
    - Op computers met Microsoft Monitoring Agent (rechtstreeks of via Operations Manager verbonden) - *systeemstation*: \Program Files\Microsoft Monitoring Agent\Agent
    - Op de server voor documentbeheer Operations Manager - *systeemstation*: \Program Files\Microsoft R2\Operations Manager\Server van System Center 2012

### <a name="to-verify-that-recommendations-are-ignored"></a>Om te verifiëren dat de aanbevelingen worden genegeerd

Na de volgende evaluatie wordt uitgevoerd, standaard om de 7 dagen geplande, wordt de opgegeven aanbevelingen *genegeerd* zijn gemarkeerd en worden niet weergegeven in het dashboard beoordeling.

1. Overzicht van alle genegeerde aanbevelingen kunt u het volgende logboek zoekquery's.

    ```
    Type=ADAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

2.  Als u later besluit dat u wilt zien van de aanbevelingen genegeerd, IgnoreRecommendations.txt bestanden verwijderen of u kunt RecommendationIDs verwijderen uit.

## <a name="ad-assessment-solutions-faq"></a>AD beoordeling oplossingen Veelgestelde vragen

*Hoe vaak wordt een beoordeling uitgevoerd?*
- De beoordeling wordt uitgevoerd om de 7 dagen.

*Is er een manier om te configureren hoe vaak de beoordeling wordt uitgevoerd?*
- Niet op dit moment.

*Als u een andere server wordt ontdekt nadat ik een oplossing voor beoordeling hebt toegevoegd, wordt het beoordeeld?*
- Ja, zodra blijkt dat is het 7 dagen vanaf dan op, beoordeeld.

*Als een server buiten gebruik is gesteld, wanneer deze verwijderd uit de beoordeling?*
- Als een server niet voor 3 weken gegevens overlegt, wordt deze verwijderd.

*Wat is de naam van het proces dat het verzamelen van gegevens is?*
- AdvisorAssessment.exe

*Hoe lang duurt het om gegevens te verzamelen?*
- De collectie van de werkelijke gegevens op de server duurt ongeveer 1 uur. Kan het langer duren op servers met een groot aantal servers in Active Directory.

*Welk type gegevens wordt verzameld?*
- De volgende typen gegevens worden verzameld:
    - WMI
    - Register
    - Prestatiemeteritems

*Is er een manier om te configureren wanneer gegevens worden verzameld?*
- Niet op dit moment.

*Waarom alleen de top 10 aanbevelingen weergeven?*
- In plaats van u overweldigend uitputtende lijst van taken, is het raadzaam dat u zich richten op de prioriteit aanbevelingen eerst adressering. Extra aanbevelingen weer beschikbaar nadat u deze oplossen. Als u liever het gedetailleerde overzicht, kunt u alle aanbevelingen met logboek zoeken weergeven.

*Is er een manier om een aanbeveling te negeren?*
- Ja, Zie [aanbevelingen negeren](#ignore-recommendations) hierboven.


## <a name="next-steps"></a>Volgende stappen

- Met [zoekacties in Analytics logboek logboek](log-analytics-log-searches.md) kunt u gedetailleerde gegevens van AD-beoordeling en aanbevelingen.
