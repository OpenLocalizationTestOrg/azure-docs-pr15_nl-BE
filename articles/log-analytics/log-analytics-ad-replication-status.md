<properties
    pageTitle="Active Directory Replication Status oplossing in logboek Analytics | Microsoft Azure"
    description="De Status van Active Directory-replicatie-oplossing pack regelmatig controleert uw Active Directory-omgeving voor eventuele storingen en rapporteert de resultaten op uw dashboard OMS."
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

# <a name="active-directory-replication-status-solution-in-log-analytics"></a>Active Directory Replication Status oplossing in logboek Analytics

Active Directory is een belangrijk onderdeel van een onderneming IT-omgeving. Hoge beschikbaarheid en hoge prestaties, zodat heeft elke domeincontroller een eigen kopie van de Active Directory-database. Domeincontrollers repliceren met elkaar om de wijzigingen doorvoeren in de hele onderneming. Fouten in dit replicatieproces kunnen leiden tot tal van problemen in de hele onderneming.

De replicatiestatus AD oplossing pack regelmatig controleert uw Active Directory-omgeving voor eventuele storingen en rapporteert de resultaten op uw dashboard OMS.

## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende informatie te installeren en configureren van de oplossing.

- Agentia moeten worden geïnstalleerd op domeincontrollers die deel uitmaken van het domein moet worden geëvalueerd of lidservers die zijn geconfigureerd voor het verzenden van replicatiegegevens AD aan OMS. Zie [verbinding maken met Windows-computers aanmelden Analytics](log-analytics-windows-agents.md)inzicht in Windows-computers verbinding met OMS. Als uw domeincontroller al deel van een bestaande omgeving van System Center Operations Manager die u wilt verbinden met OMS uitmaakt, Zie [Operations Manager verbinding maken met Analytics logboek](log-analytics-om-agents.md).
- De Status van Active Directory-replicatie-oplossing toevoegen aan uw Kantoorbeheersysteem werkruimte met behulp van het proces beschreven in [oplossingen logboek Analytics toevoegen uit de galerie met oplossingen](log-analytics-add-solutions.md).  Er is geen verdere configuratie nodig.


## <a name="ad-replication-status-data-collection-details"></a>AD de replicatiestatus gegevens gegevens

De volgende tabel bevat methoden van de collectie gegevens en andere informatie over de manier waarop gegevens worden verzameld voor de replicatiestatus AD.

| platform | Directe Agent | SCOM agent | Azure opslag | SCOM nodig? | SCOM agent gegevens verzonden via management groep | frequentie van verzameling |
|---|---|---|---|---|---|---|
|Windows|![Ja](./media/log-analytics-ad-replication-status/oms-bullet-green.png)|![Ja](./media/log-analytics-ad-replication-status/oms-bullet-green.png)|![Nee](./media/log-analytics-ad-replication-status/oms-bullet-red.png)|![Nee](./media/log-analytics-ad-replication-status/oms-bullet-red.png)|![Ja](./media/log-analytics-ad-replication-status/oms-bullet-green.png)| om de 5 dagen|


## <a name="optionally-enable-a-non-domain-controller-to-send-ad-data-to-oms"></a>U kunt desgewenst een niet-domeincontroller AD om gegevens te verzenden naar OMS inschakelen
Als u niet wilt dat een van uw domeincontrollers rechtstreeks verbinden met OMS, kunt u andere OMS verbonden computer in uw domein te verzamelen van gegevens voor de replicatiestatus AD oplossing pack en het verzenden van de gegevens.

### <a name="to-enable-a-non-domain-controller-to-send-ad-data-to-oms"></a>Een niet-domeincontroller AD om gegevens te verzenden naar OMS inschakelen
1.  Controleer of de computer lid van het domein dat u wilt controleren met de replicatiestatus AD-oplossing.
2.  [Verbinding maken met de Windows-computer OMS](log-analytics-windows-agents.md) of [verbinding maken met uw bestaande omgeving Operations Manager met OMS](log-analytics-om-agents.md), als deze is niet aangesloten.
3.  Op die computer, stel de volgende registersleutel:
    - Sleutel: **groepen HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HealthService\Parameters\Management\<ManagementGroupName > \Solutions\ADReplication**
    - Waarde: **IsTarge**
    - Gegevens: **true**

    >[AZURE.NOTE]Deze wijzigingen worden pas van kracht totdat opnieuw te starten de service Microsoft Monitoring Agent (HealthService.exe).

## <a name="understanding-replication-errors"></a>Informatie over replicatiefouten
Zodra u AD statusgegevens voor replicatie verzonden naar OMS hebt, ziet u een deel van de volgende strekking op het Kantoorbeheersysteem dashboard die aangeeft hoeveel replicatiefouten zijn voor u.  
![Naast de replicatiestatus AD](./media/log-analytics-ad-replication-status/oms-ad-replication-tile.png)

**Kritieke replicatiefouten** zijn op of boven de 75% van de [tombstone-levensduur](https://technet.microsoft.com/library/cc784932%28v=ws.10%29.aspx) van uw Active Directory-forest.

Wanneer u op de tegel klikt, ziet u meer informatie over de fouten.
![AD replicatiestatus dashboard](./media/log-analytics-ad-replication-status/oms-ad-replication-dash.png)


### <a name="destination-server-status-and-source-server-status"></a>Bestemming serverstatus en de Status van de bron Server
De status van de doelserver en de servers die problemen met replicatie zijn weergeven deze bladen Het nummer na elke naam van de domeincontroller geeft het aantal replicatiefouten op de domeincontroller.

De fouten voor zowel bestemming en bronservers worden weergegeven omdat enkele problemen gemakkelijker oplossen vanuit het perspectief van de server bron en anderen vanuit het perspectief van de server bestemming.

In dit voorbeeld kunt u zien dat veel doelserver ongeveer hetzelfde aantal fouten hebben, maar er is één bronserver (ADDC35) met veel meer fouten dan alle andere. Het is waarschijnlijk dat er een probleem op de ADDC35 die wordt veroorzaakt door het is niet om gegevens te verzenden naar de replicatiepartners. Oplossen van de problemen op ADDC35 waarschijnlijk de oplossing van veel van de fouten die worden weergegeven in de server-blade bestemming.

### <a name="replication-error-types"></a>Replicatie fouttypen
Deze blade vindt u informatie over de soorten fouten gevonden in uw onderneming. Elke fout is een unieke numerieke code en een bericht waarmee u de oorzaak van de fout vast te stellen.

De ring aan de bovenkant krijgt u een idee van welke fouten meer verschijnen en minder vaak in uw omgeving.

Dit kunt u zien wanneer meerdere domeincontrollers dezelfde Replicatiefout optreden. In dit geval wordt u mogelijk om een oplossing op een domeincontroller te identificeren en herhaal vervolgens deze op andere domeincontrollers die zijn getroffen door dezelfde fout.

### <a name="tombstone-lifetime"></a>Tombstone-levensduur
De tombstone-levensduur wordt bepaald hoe lang een verwijderd object, aangeduid als een tombstone, wordt opgeslagen in de Active Directory-database. Als u een verwijderd object wordt doorgegeven de tombstone-levensduur, een opschoonproces deze automatisch verwijderd uit de Active Directory-database.

De tombstone-standaardlevensduur 180 dagen voor de meest recente versies van Windows, is maar 60 dagen op oudere versies was dit expliciet kan worden gewijzigd door een beheerder van Active Directory.

Het is belangrijk om te weten als u ondervindt replicatiefouten die bijna zijn bereikt of voorbij de tombstone-levensduur. Als twee domeincontrollers een replicatiefout die nog aanwezig is na de tombstone-levensduur, wordt replicatie tussen twee domeincontrollers, uitgeschakeld zelfs als de onderliggende Replicatiefout is opgelost.

De Tombstone-levensduur blade helpt u bij het identificeren van de plaatsen waar dit gebeurt gevaar is. Elke fout in de **meer dan 100% TSL** categorie vertegenwoordigt een partitie die niet is gerepliceerd tussen de bron- en doeladressen server voor ten minste de tombstone-levensduur voor het forest.

In dit geval gewoon tot vaststelling van de Replicatiefout worden niet voldoende. Ten minste, moet u handmatig onderzoeken om aan te duiden en achtergebleven objecten opschonen voordat u replicatie kunnen starten. Mogelijk moet u ook een domeincontroller buiten gebruik stellen.

Identificeren replicatiefouten die blijven bestaan na de tombstone-levensduur, zult u ook aandacht besteden aan eventuele fouten in de **TSL 50-75%** of **75-100% TSL** categorieën vallen.

Dit zijn de fouten die duidelijk achtergebleven, niet tijdelijk, zodat u moet waarschijnlijk de interventie op te lossen. Het goede nieuws is dat ze nog niet is bereikt de tombstone-levensduur. Als u deze problemen snel oplossen *voordat* die ze de tombstone-levensduur bereiken, replicatie kunt starten met minimale handmatige interventie.

Zoals eerder opgemerkt, bevat de tegel dashboard voor de replicatiestatus AD-oplossing het nummer van *kritieke* replicatiefouten in uw omgeving, die wordt gedefinieerd als de fouten die meer dan 75% van de tombstone-levensduur zijn (inclusief de fouten die meer dan 100% van de TSL zijn). Streven naar dit getal bij 0 te houden.

>[AZURE.NOTE] Alle tombstone levensduur percentage berekeningen zijn gebaseerd op de werkelijke tombstone-levensduur voor uw Active Directory-forest, zodat u vertrouwen kunt dat deze percentages nauwkeurig, zijn zelfs als u een aangepaste tombstone-levensduur-waarde ingesteld.

### <a name="ad-replication-status-details"></a>Statusgegevens voor AD-replicatie
Wanneer u op een willekeurig item in een van de lijsten, ziet u meer informatie over de zoekfunctie gebruiken. De resultaten worden gefilterd zodat alleen de fouten met betrekking tot dat item. Bijvoorbeeld, als u klikt op de eerste domeincontroller die wordt vermeld onder **Bestemming Server Status (ADDC02)**, ziet u zoekresultaten gefilterd, zodat de fouten weergeven met die domeincontroller vermeld als de doelserver:

![AD replicatiefouten status in de zoekresultaten](./media/log-analytics-ad-replication-status/oms-ad-replication-search-details.png)

Vanaf hier, kunt u nog verder filteren, de query wijzigen, enzovoort. Voor meer informatie over het gebruik van het logboek zoeken Zie [zoekacties logboek](log-analytics-log-searches.md).

Het veld **HelpLink** geeft de URL van een TechNet-pagina met details over deze specifieke fout. U kunt kopiëren en plak deze koppeling in het venster van de webbrowser voor informatie over het oplossen van problemen en de vaststelling van de fout.

U kunt ook klikken op **exporteren** om de resultaten naar Excel exporteren. Hiermee kunt u replicatie fout gegevens in elke gewenste manier.

![geëxporteerde AD status replicatiefouten in Excel](./media/log-analytics-ad-replication-status/oms-ad-replication-export.png)

## <a name="ad-replication-status-faq"></a>AD replicatiestatus Veelgestelde vragen
**V: hoe vaak wordt AD status replicatiegegevens bijgewerkt?**
A: de informatie is bijgewerkt om de 5 dagen.

**V: is er een manier om te configureren hoe vaak deze gegevens worden bijgewerkt?**
A: op dit moment niet.

**V: moet ik mijn domeincontrollers toevoegen aan mijn werkruimte OMS om Zie replicatiestatus?**
A: Nee, één domeincontroller moet worden toegevoegd. Als u meerdere domeincontrollers in uw werkruimte OMS hebt, gegevens uit deze verzonden naar OMS.

**V: ik wil niet alle domeincontrollers toevoegen aan mijn werkruimte OMS. Kan ik nog steeds de replicatiestatus AD oplossing gebruiken?**
A: Ja. U kunt de waarde van een registersleutel deze instellen. Zie [een niet - domeincontroller voor het verzenden van gegevens naar OMS AD inschakelen](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).

**V: Wat is de naam van het proces dat het verzamelen van gegevens is?**
A: AdvisorAssessment.exe

**V: hoe lang duurt het om gegevens te verzamelen?**
A: data collectie tijd is afhankelijk van de grootte van de Active Directory-omgeving, maar duurt meestal minder dan 15 minuten.

**V: welk type gegevens wordt verzameld?**
A: replicatie-informatie wordt verzameld via LDAP.

**V: is er een manier om te configureren wanneer gegevens worden verzameld?**
A: op dit moment niet.

**V: welke machtigingen heb ik nodig om gegevens te verzamelen?**
A: als normale gebruikersmachtigingen voor Active Directory zijn meestal voldoende.

## <a name="troubleshoot-data-collection-problems"></a>Verzameling van gegevens oplossen
Verzamelen van gegevens, de replicatiestatus AD oplossing pack moet ten minste één domeincontroller zijn aangesloten op uw werkruimte OMS. Als u dit doet, ziet u een bericht dat aangeeft dat **nog steeds gegevens worden verzameld**.

Als u verbinding maken met een van uw domeincontrollers hulp nodig hebt, kunt u de documentatie weergeven op [verbinding maken met Windows-computers aanmelden Analytics](log-analytics-windows-agents.md). Als uw domeincontroller al is verbonden met een bestaande omgeving met System Center Operations Manager, kunt u ook documentatie weergeven op [Verbinding maken met System Center Operations Manager naar logboek Analytics](log-analytics-om-agents.md).

Als u niet dat een van uw domeincontrollers verbinding rechtstreeks met OMS of SCOM wilt, Zie [een niet - domeincontroller voor het verzenden van gegevens naar OMS AD inschakelen](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).


## <a name="next-steps"></a>Volgende stappen

- [Zoekopdrachten in Analytics logboek logboek](log-analytics-log-searches.md) gebruiken om gedetailleerde gegevens voor Active Directory-replicatie de status weer te geven.
