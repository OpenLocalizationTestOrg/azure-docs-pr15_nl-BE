<properties
    pageTitle="Wire-oplossing in logboek Analytics | Microsoft Azure"
    description="Wire-gegevens zijn geconsolideerde gegevens in netwerk en de prestaties van computers met OMS stoffen, met inbegrip van Operations Manager en Windows verbonden agenten. Netwerkgegevens worden gecombineerd met uw gegevens aanmelden bij het relateren van gegevens."
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
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="wire-data-solution-in-log-analytics"></a>Wire-oplossing in logboek Analytics

Wire-gegevens zijn geconsolideerde gegevens in netwerk en de prestaties van computers met OMS stoffen, met inbegrip van Operations Manager en Windows verbonden agenten. Netwerkgegevens worden gecombineerd met uw gegevens aanmelden bij het relateren van gegevens. OMS-agents die zijn geïnstalleerd op computers in uw IT-infrastructuur monitor netwerkgegevens verzonden naar en vanaf deze computers voor niveau 2-3 in het [OSI-model](https://en.wikipedia.org/wiki/OSI_model) , met inbegrip van de verschillende protocollen en poorten die worden gebruikt op het netwerk.

>[AZURE.NOTE] De oplossing voor overschrijving is momenteel niet beschikbaar voor werkruimten worden toegevoegd. Klanten die al de draad oplossing ingeschakeld hebben kunnen blijven gebruiken de draad oplossing.

Standaard verzamelt OMS logboekgegevens voor CPU, geheugen, schijf en netwerk prestatiegegevens van items die zijn ingebouwd in Windows. Netwerk- en andere verzamelen van gegevens gebeurt in real-time voor elke agent, met inbegrip van de subnetten en application-niveau-protocollen die worden gebruikt door de computer. U kunt andere prestatiemeteritems toevoegen op de pagina instellingen op het tabblad Logboeken.

Als u een [sFlow](http://www.sflow.org/) of andere software met [NetFlow-protocol van Cisco gebruikt al](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html), statistieken en gegevens u van draad gegevens ziet worden aan u bekend.

Enkele van de soorten ingebouwde logboek zoekquery's zijn:

- Agenten die draad gegevens
- IP-adres van de agenten draad gegevens
- Uitgaande communicatie via IP-adressen
- Het aantal bytes dat is verzonden door toepassingsprotocollen
- Het aantal bytes dat is verzonden door een service aanvraag
- Bytes ontvangen door verschillende protocollen
- Totale aantal bytes dat is verzonden en ontvangen door IP
- IP-adressen die u hebt verstrekt met agenten op het subnet 10.0.0.0/8
- Gemiddelde latentie voor verbindingen die zijn gewaardeerd
- Computerprocessen die zijn gestart of netwerkverkeer ontvangen
- Hoeveelheid netwerkverkeer dat bestemd is voor een proces

Wanneer u met draad gegevens zoekt, kunt u filteren en gegevens weer te geven informatie over de bovenste agenten en bovenste protocollen. Of u kunt zoeken naar wanneer bepaalde computers (IP-adressen/MAC-adressen) ter kennis gebracht met elkaar, hoe lang en hoeveel gegevens is verzonden--in principe u metagegevens over het netwerkverkeer, op basis van een zoekopdracht wordt bekijken.

Aangezien u metagegevens bekijkt, is het echter niet altijd nuttig voor het oplossen van diepgaande. Gegevens in OMS draad is niet een volledige opname van netwerkgegevens. Zo het niet bedoeld voor het oplossen van diep pakketniveau.
Het voordeel van het gebruik van de agent, vergeleken met andere methoden van de collectie is dat er geen apparaten installeren, configureren van uw netwerk-switches of ingewikkelde configuraties uitvoeren. Gegevens van de draad is gewoon agent gebaseerde--u de agent installeren op een computer en wordt het eigen netwerkverkeer controleren. Een ander voordeel is als u controleren werklasten uitgevoerd in Microsoft Azure wilt, waar de gebruiker niet de eigenaar van de laag weefsel of cloud providers of hosting-provider.

Daarentegen, hebt u geen compleet overzicht van wat er in het netwerk gebeurt als u geen agents op alle computers in uw netwerkinfrastructuur installeren.

## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende informatie te installeren en configureren van de oplossing.

- De oplossing voor draad ontvangt gegevens van computers met Windows Server 2012 R2, Windows 8.1 en latere besturingssystemen.
- Microsoft .NET Framework 4.0 of hoger is vereist op computers waar u gegevens uit draad verkrijgen.
- De oplossing voor draad toevoegen aan uw Kantoorbeheersysteem werkruimte met behulp van het proces beschreven in [oplossingen logboek Analytics toevoegen uit de galerie met oplossingen](log-analytics-add-solutions.md).  Er is geen verdere configuratie nodig.
- Als u gegevens weergeven draad voor een specifieke oplossing wilt, moet u de oplossing is al toegevoegd aan de werkruimte OMS.

## <a name="wire-data-data-collection-details"></a>Wire-gegevens gegevens collectie details

Draad gegevens verzamelt metagegevens over het netwerkverkeer met de agenten die u hebt ingeschakeld.

In de volgende tabel bevat de methoden van de collectie gegevens en andere details over de manier waarop gegevens worden verzameld voor gegevens van de draad.


| platform | Directe Agent | SCOM agent | Azure opslag | SCOM nodig? | SCOM agent gegevens verzonden via management groep | frequentie van verzameling |
|---|---|---|---|---|---|---|
|Windows (2012 R2 / 8.1 of hoger)|![Ja](./media/log-analytics-wire-data/oms-bullet-green.png)|![Ja](./media/log-analytics-wire-data/oms-bullet-green.png)|![Nee](./media/log-analytics-wire-data/oms-bullet-red.png)|            ![Nee](./media/log-analytics-wire-data/oms-bullet-red.png)|![Nee](./media/log-analytics-wire-data/oms-bullet-red.png)| elke minuut 1|


## <a name="combining-wire-data-with-other-solution-data"></a>Wire-gegevens combineren met andere oplossingsgegevens

Gegevens die worden geretourneerd uit de ingebouwde query's hierboven mogelijk interessante zelf. Het nut van de gegevens van de draad is echter gerealiseerd wanneer deze worden gecombineerd met gegevens uit andere OMS oplossingen. U kunt bijvoorbeeld beveiligingsgegevens verzameld door de beveiliging en controle oplossing gebruiken en combineren met gegevens om te zoeken naar ongebruikelijke netwerk aanmeldingspogingen voor benoemd processen draad.  In dit voorbeeld gebruikt u de operatoren IN en DISTINCT deelnemen aan gegevenspunten in uw query.

Vereisten: Wilt gebruiken in het volgende voorbeeld, moet u de beveiliging en controle-oplossing geïnstalleerd hebt. Echter, u kunt gegevens uit andere oplossingen combineren met gegevens van de draad om vergelijkbare resultaten te bereiken.

### <a name="to-combine-wire-data-with-security-events"></a>Draad om gegevens te combineren met beveiligingsgebeurtenissen

1. Klik op het **WireData** naast elkaar op de pagina overzicht.
2. Klik in de lijst met **Algemene WireData-query's**, **Bedrag van netwerkverkeer (in Bytes) door het proces** voor het overzicht van geretourneerde processen.
    ![wiredata-query 's](./media/log-analytics-wire-data/oms-wiredata-01.png)
3. Als de lijst met processen niet gemakkelijk kunnen bekijken, kunt u de query in die lijkt op:

    ```
    Type WireData | measure count() by ProcessName | where AggregatedValue <40
    ```
    In het volgende voorbeeld wordt getoond is een proces met de naam DancingPigs.exe, die mogelijk verdacht.
    ![zoekresultaten voor wiredata](./media/log-analytics-wire-data/oms-wiredata-02.png)

4. Gegevens die worden geretourneerd in de lijst en klik op een benoemd proces. In dit voorbeeld is DancingPigs.exe geklikt. De onderstaande resultaten beschrijven het type netwerkverkeer zoals uitgaande communicatie via verschillende protocollen.
    ![wiredata-resultaten weergeven met de naam](./media/log-analytics-wire-data/oms-wiredata-03.png)

5. Aangezien de beveiligings- en controle-oplossing is geïnstalleerd, kunt u in de security-evenementen die dezelfde waarde in het veld procesnaam hebben doordat uw zoekopdracht met behulp van de IN- en DISTINCT search query-operators probe. U kunt dit doen vervolgens wanneer zowel de gegevens van uw draad en andere logboeken oplossing waarden in dezelfde indeling hebben. Wijzig uw zoekopdracht in die lijkt op:

    ```
    Type=SecurityEvent ProcessName IN {Type:WireData "DancingPigs.exe" | distinct ProcessName}
    ```    

    ![wiredata resultaten tonen gecombineerde gegevens](./media/log-analytics-wire-data/oms-wiredata-04.png)
6. In de resultaten, zult u zien dat de gegevens wordt weergegeven. Nu kunt u uw zoekopdracht wilt weten hoe vaak de account, het weergeven van gegevens, beveiliging en controle, werd gebruikt door het proces met een query die lijkt op verfijnen:        

    ```
    Type=SecurityEvent ProcessName IN {Type:WireData "DancingPigs.exe" | distinct ProcessName} | measure count() by Account
    ```

    ![wiredata resultaten met accountgegevens](./media/log-analytics-wire-data/oms-wiredata-05.png)



## <a name="next-steps"></a>Volgende stappen

- [Zoeken in Logboeken](log-analytics-log-searches.md) draad gedetailleerde gegevens zoeken in records weergeven.
- Raadpleeg dat dan de [draad gegevens met behulp van Operations Management Suite logboek zoeken blog post](http://blogs.msdn.com/b/dmuscett/archive/2015/09/09/using-wire-data-in-operations-management-suite.aspx) vindt u aanvullende informatie over hoe vaak de gegevens worden verzameld en hoe kunt u eigenschappen wijzigen voor agenten Operations Manager.
