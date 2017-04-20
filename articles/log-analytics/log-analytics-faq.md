<properties
    pageTitle="Meld u Analytics Veelgestelde vragen | Microsoft Azure"
    description="Antwoorden op veelgestelde vragen over het logboek Analytics-service."
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

# <a name="log-analytics-faq"></a>Veelgestelde vragen over Analytics melden

Deze Microsoft-FAQ is een lijst met veelgestelde vragen over het logboek Analytics in Microsoft Operations Management Suite (OMS). Hebt u nog vragen hebt over het logboek Analytics, gaat u naar het [forum voor discussie](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights) en vragen stellen. Iemand uit onze community helpen u uw antwoorden. Als een vraag wordt vaak gevraagd, zullen we het toevoegen aan dit artikel zodat u snel en gemakkelijk kan worden gevonden.

## <a name="general"></a>Algemeen

**V. welke controles worden uitgevoerd door de AD en evaluatie van SQL-oplossingen?**

A. De volgende query bevat een omschrijving van alle controles die momenteel worden uitgevoerd:

```
(Type=SQLAssessmentRecommendation OR Type=ADAssessmentRecommendation) | dedup RecommendationId | select FocusArea, ActionArea, Recommendation, Description | sort Type, FocusArea,ActionArea, Recommendation
```

De resultaten kunnen vervolgens worden geëxporteerd naar Excel voor verder onderzoek.

* *V: Waarom zie ik iets anders dan *OMS* in SCOM Administration? **

A: afhankelijk van welke Update Rollup van SCOM in, ziet u mogelijk een knooppunt voor *System Center Advisor*, *Operationele inzichten*of *Logboek Analytics*.

De *OMS* tekst string update is opgenomen in een management pack, dat moet handmatig worden ingevoerd. Volg de aanwijzingen op het meest recente SCOM Update Rollup KB-artikel en het overzicht van de meest recente updates in het knooppunt *OMS* OMS-console vernieuwen.

* *V: Is er een *op de locatie* versie van OMS? **

A: Nee. Logboek Analytics zeer grote hoeveelheden gegevens worden opgeslagen en verwerkt. Als een cloud-service kan logboek Analytics opschaling indien nodig en invloed op de prestaties voor het milieu te voorkomen.

Bovendien wordt een cloud service betekent dat u hoeft niet te houden de infrastructuur logboek Analytics en uitgevoerd functie regelmatig updates en correcties kunt ontvangen.

## <a name="configuration"></a>Configuratie
**V. kan ik de naam van de tabel/blob-container gebruikt voor het lezen van Azure Diagnostics (af) wijzigen?**  

A.  Nee, dit is nog niet mogelijk, maar in een toekomstige release is gepland.

**V. welke IP-adressen gebruiken de OMS doen? Hoe zorg ik ervoor dat mijn firewall alleen toestaat dat verkeer naar de OMS-Services**  

A. Het logboek Analytics-service is gebouwd op Azure en de eindpunten in het [Microsoft Azure Datacenter IP-bereik](http://www.microsoft.com/download/details.aspx?id=41653)IP-adressen ontvangen.

Als service-implementaties worden uitgevoerd, wordt de werkelijke IP-adressen van de OMS-services wijzigen. De DNS-namen wilt toestaan via de firewall worden op [de proxy- en firewall instellingen configureren in het logboek voor Analytics](log-analytics-proxy-firewall.md)beschreven.

**V. ik ExpressRoute gebruiken voor de verbinding met Azure. Wordt mijn logboek Analytics verkeer mijn ExpressRoute verbinding gebruiken?**  

A. De verschillende soorten verkeer ExpressRoute worden beschreven in de [documentatie van ExpressRoute](./expressroute/expressroute-faqs.md#supported-services).

Verkeer naar logboek Analytics maakt gebruik van de openbare peering ExpressRoute circuit.

**V. is er een eenvoudige en handige manier om een bestaande werkruimte voor logboek Analytics verplaatsen naar een ander logboek Analytics werkruimte/Azure-abonnement?**  We hebben verschillende klant OMS werkruimten die we zijn testen en trialing in onze Azure abonnement en ze naar productie verplaatst zodat we willen naar hun eigen Azure/OMS-abonnement.  

A. De `Move-AzureRmResource` cmdlet kunt u ook een werkruimte logboek Analytics en een account voor de automatisering van een Azure-abonnement naar de andere verplaatsen. Zie voor meer informatie [Verplaatsen AzureRmResource](http://msdn.microsoft.com/library/mt652516.aspx).

Deze wijziging kan ook plaatsvinden in de portal Azure.

U kan gegevens uit een logboek Analytics werkruimte naar de andere verplaatsen of wijzigen van het gebied dat logboek Analytics-gegevens worden opgeslagen in.

**V: hoe voeg ik OMS voor SCOM?**

A: bijwerken naar de meest recente updatepakket en beheerpakketten importeren kunt u verbinding maken met SCOM logboek Analytics.

Opmerking de SCOM verbinding met Analytics logboek is alleen beschikbaar voor SCOM 2012 SP1 en hoger.

**V: hoe kan ik bevestigen dat een agent kan communiceren met het logboek Analytics?**

A: om ervoor te zorgen dat de agent met OMS communiceren kan, gaat u naar: Configuratiescherm, beveiliging en **Microsoft Monitoring Agent**-instellingen.

Zoek onder het tabblad **Azure logboek Analytics (OMS)** naar een groen vinkje. Een groen vinkje wordt bevestigd dat de agent kan communiceren met de OMS-service.

Een geel waarschuwingspictogram betekent dat de agent heeft problemen met communicatie met OMS. Een veelvoorkomende reden is de Microsoft Monitoring Agent-service is gestopt en moet opnieuw worden gestart.

**V: hoe stop ik een agent niet communiceren met Analytics logboek**

A: In SCOM, de computer verwijderen uit de lijst met beheerde OMS. Hiermee wordt alle verkeer via SCOM voor die medewerker. Voor de agents rechtstreeks verbonden met OMS, kunt u ze stoppen niet communiceren met OMS via: Configuratiescherm, beveiliging en **Microsoft Monitoring Agent**-instellingen.
Verwijder alle werkruimten die worden vermeld onder **Azure logboek Analytics (OMS)**.

## <a name="agent-data"></a>Agent gegevens

**V. hoeveel gegevens kan ik verzenden via de agent naar logboek Analytics? Is er een maximale hoeveelheid gegevens per klant?**  
A. Het vrije plan stelt een initiaal dagelijks van 500 MB per werkruimte. De standard en premium plannen hebben geen limiet voor de hoeveelheid gegevens die is geüpload. Als een cloud-service, logboek Analytics in OMS ontworpen om automatisch de schaal tot en met ingang van het volume die afkomstig zijn van een klant – zelfs als het terabytes per dag.

De agent logboek Analytics is zodanig heeft een ruimtebesparend en enkele basisgegevens compressie biedt. Een van onze klanten daadwerkelijk een blog geschreven over de tests die zijn verricht tegen onze agent en hoe onder de indruk zijn. Het gegevensvolume varieert afhankelijk van de oplossingen kunnen uw klanten. U kunt gedetailleerde informatie over het gegevensvolume vinden en Zie de verdeling door de oplossing bij het **gebruik van** naast elkaar op de pagina OMS-overzicht.

Voor meer informatie kunt u een [klant blog](http://thoughtsonopsmgr.blogspot.com/2015/09/one-small-footprint-for-server-one.html) over de lage uitstoot van de agent OMS lezen.

**V. hoeveel bandbreedte wordt door Microsoft Management Agent (MMA) gebruikt bij het verzenden van gegevens naar logboekbestanden Analytics?**

A. Bandbreedte is een functie van de hoeveelheid gegevens die worden verzonden. Gegevens worden gecomprimeerd wanneer deze worden verzonden via het netwerk.

**V. hoeveel gegevens worden verzonden per agent?**

A. Dit afhankelijk grotendeels van:

- de oplossingen die u hebt ingeschakeld.
- het aantal logboekbestanden en prestatiemeteritems worden verzameld
- het volume van de gegevens in de logboeken

De prijzen vrij laag is een goede manier om de on-board verschillende servers en de typische gegevensvolume meter. Algemene wordt syntaxis weergegeven op de pagina **Gebruik** .
Voor computers die de agent WireData uitvoeren, kunt u zien hoeveel gegevens worden verzonden met behulp van de volgende query:

```
Type=WireData (ProcessName="C:\\Program Files\\Microsoft Monitoring Agent\\Agent\\MonitoringHost.exe") (Direction=Outbound) | measure Sum(TotalBytes) by Computer
```



## <a name="next-steps"></a>Volgende stappen

- [Aan de slag met Analytics logboek](log-analytics-get-started.md) voor meer informatie over logboekbestanden Analytics en get up en wordt uitgevoerd in minuten.
