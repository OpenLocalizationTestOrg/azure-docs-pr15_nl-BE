<properties 
   pageTitle="Aangepaste worden vastgelegd in het logboek Analytics | Microsoft Azure"
   description="Logboek Analytics kunt gebeurtenissen verzamelen van bestanden op computers met Windows en Linux.  In dit artikel beschreven hoe u een nieuwe aangepaste logboek en de details van de records die zij in de opslagplaats OMS maken."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="custom-logs-in-log-analytics"></a>Aangepaste worden vastgelegd in het logboek Analytics

Gegevensbron in logboek Analytics aangepaste logboeken kunt u gebeurtenissen verzamelen van bestanden op computers met Windows en Linux. Veel toepassingen vastleggen informatie in een tekstbestand opslaan in plaats van standaard registratie-services zoals Windows-gebeurtenislogboek of Syslog.  Eenmaal verzameld, kunt u elke record in het logboekbestand parseren in afzonderlijke velden met de functie [Aangepaste velden](log-analytics-custom-fields.md) van logboek Analytics.

![De collectie aangepaste log](media/log-analytics-data-sources-custom-logs/overview.png)

De logboekbestanden moeten worden verzameld, moeten overeenkomen met de volgende criteria.

- Het logboek moet één vermelding per regel hebt of gebruik een tijdstempel die overeenkomt met een van de volgende indelingen aan het begin van elke vermelding.

    JJJJ-MM-DD: MM: SS <br>
  M/D/JJJJ UU: MM: SS AM/PM <br>
  Ma DD, jjjj: mm: ss
    
- Het logboekbestand mag geen ronde updates toestaan waar het bestand wordt overschreven met nieuwe gegevens. 

## <a name="defining-a-custom-log"></a>Een aangepaste logboek definiëren

Gebruik de volgende procedure kunt u een aangepaste logboekbestand definiëren.  Ga naar het einde van dit artikel voor een overzicht van een voorbeeld van het toevoegen van een aangepaste logboek.

### <a name="step-1-open-the-custom-log-wizard"></a>Stap 1. De Wizard aangepaste logboek openen

De Wizard voor aangepaste logboek wordt uitgevoerd in de portal OMS en kunt u een nieuwe aangepaste logboek verzamelen definiëren.

1.  Ga naar **Instellingen**in de portal OMS.
2.  Klik op **gegevens** en vervolgens **aangepaste logboeken**.
3.  Standaard worden alle wijzigingen in de configuratie automatisch geduwd naar alle agenten.  Voor Linux-agenten wordt een configuratiebestand verzonden naar de gegevensverzamelaarset Fluentd.  Als u wilt dat dit bestand handmatig op elke Linux-agent wijzigen, schakel dan het selectievakje *toepassen onder configuratie van mijn Linux machines*in.
4.  Klik op **Add +** om het logboek aangepaste Wizard te openen.

### <a name="step-2-upload-and-parse-a-sample-log"></a>Stap 2. Uploaden en een logboek monster parseren

Begint u met een monster van het aangepaste logboek uploaden.  De wizard parseren en de gegevens worden weergegeven in dit bestand te valideren.  Logboek Analytics wordt gebruikt als scheidingsteken die u opgeeft als u elke record.

**Nieuwe regel** wordt het scheidingsteken en wordt gebruikt voor het logboek met één vermelding per regel.  Als de regel met een datum en tijd in een van de beschikbare indelingen begint, kunt u een **tijdstempel** scheidingstekens dat ondersteuning biedt voor posten die zich uitstrekken over meer dan één regel opgeven. 

Een tijdstempel als scheidingsteken wordt gebruikt, wordt de eigenschap TimeGenerated van elke record in de OMS worden gevuld met de datum of tijd opgegeven voor die vermelding in het logboekbestand.  Als u een nieuwe regel als scheidingsteken wordt gebruikt, wordt TimeGenerated gevuld met datum en tijd dat logboek Analytics verzameld van de post. 

>[AZURE.NOTE]Logboek Analytics behandelt momenteel de datum en tijd van een logboek met behulp van een tijdstempel scheidingstekens als UTC verzameld.  Dit zal spoedig worden gewijzigd als u de tijdzone op de agent. 
 
1.  Klik op **Bladeren** en blader naar een voorbeeld van een bestand.  Opmerking: deze knop kan wellicht **Bestand kiezen** in sommige browsers wordt aangeduid.
2.  Klik op **volgende**. 
3.  De Wizard voor aangepaste Log wordt het bestand uploaden en de records die worden aangeduid.
4.  Het scheidingsteken dat wordt gebruikt voor het identificeren van een nieuwe record en selecteer het scheidingsteken die het beste welke records in het logboekbestand aangeeft wijzigen.
5.  Klik op **volgende**.

### <a name="step-3-add-log-collection-paths"></a>Stap 3. Logboek collectie paden toevoegen

U moet een of meer paden definiëren op de agent waar het logboek voor de aangepaste kunt vinden.  Kunt u ofwel een specifiek pad en naam voor het logboekbestand opgeven of u kunt een pad opgeven met een joker voor de naam.  Dit ondersteunt toepassingen die een nieuw bestand maakt elke dag of als een bestand een bepaalde grootte heeft bereikt.  U kunt ook meerdere paden opgeven voor één logboekbestand.

Bijvoorbeeld een toepassing kan een logboekbestand maken elke dag met de datum die is opgenomen in de naam in log20100316.txt. Een patroon voor een dergelijke logboek kan worden *log\*.txt* regeling die ook voor een logboekbestand na de toepassing de naamgeving.

De volgende tabel bevat voorbeelden van geldige patronen verschillende logboekbestanden opgeven. 

| Beschrijving | Pad |
|:--|:--|
| Alle bestanden in *C:\Logs* met de extensie .txt in Windows-agent | C:\Logs\\\*.txt |
| Alle bestanden in *C:\Logs* met een naam die begint met het logboek en de extensie .txt in Windows-agent | C:\Logs\log\*.txt |
| Alle bestanden in */var/log/audit* met de extensie .txt in Linux-agent | /var/log/audit/*.txt |
| Alle bestanden in */var/log/audit* met een naam die begint met het logboek en de extensie .txt op Linux-agent | /var/log/audit/log\*.txt |
  

1.  Selecteer Windows- of Linux kunt u opgeven welk padindeling die u wilt toevoegen.
2.  Typ het pad en klik op de **+** knop.
3.  Herhaal dit proces voor elk extra paden.

### <a name="step-4-provide-a-name-and-description-for-the-log"></a>Stap 4. Geef een naam en beschrijving voor het logboek

De naam die u opgeeft wordt gebruikt voor het logboektype zoals hierboven beschreven.  Het eindigt altijd met _CL om deze te onderscheiden als een aangepaste logboek.

1.  Typ een naam voor het logboek.  De ** \_CL** achtervoegsel automatisch wordt geleverd.
2.  Voeg een optionele **Beschrijving**.
3.  Klik op **volgende** om de definitie van aangepaste logboek opslaan.

### <a name="step-5-validate-that-the-custom-logs-are-being-collected"></a>Stap 5. Valideren dat de aangepaste logboeken worden verzameld
Het kan duren tot een uur voor de initiële gegevens uit een nieuwe aangepaste logboek worden weergegeven in het logboek voor Analytics.  Start het verzamelen van gegevens uit de logboeken gevonden in het pad dat u hebt opgegeven vanaf het punt dat u het logboek voor aangepaste gedefinieerd.  Deze niet behouden blijft voor de posten die u tijdens het maken van aangepaste log geüpload, maar verzamelt het al bestaande vermeldingen in logboekbestanden wordt gezocht naar.

Als het logboek Analytics verzamelen uit het aangepaste logboek is gestart, kunnen de records worden met een zoekfunctie.  De naam die u het logboek voor aangepaste als het **Type** in uw query gegeven gebruiken.

>[AZURE.NOTE] Als de eigenschap Nieuwrap: in de zoekresultaten ontbreekt, moet u sluiten en opnieuw openen van de browser.

### <a name="step-6-parse-the-custom-log-entries"></a>Stap 6. De aangepaste vermeldingen parseren

In een enkele eigenschap met de naam **Nieuwrap:**het hele logboek opgeslagen.  Waarschijnlijk zult u voor het scheiden van de verschillende onderdelen van de gegevens in elke vermelding in de afzonderlijke eigenschappen in de record opgeslagen.  U doet dit met behulp van de functie voor [Aangepaste velden](log-analytics-custom-fields.md) van logboek Analytics.

Stapsgewijze instructies voor het parseren van de aangepaste vermelding die niet hier.  Raadpleeg de documentatie bij de [Aangepaste velden](log-analytics-custom-fields.md) voor deze informatie.

## <a name="disabling-a-custom-log"></a>Een aangepaste logboek uitschakelen

U kunt een logboek met aangepaste definitie verwijderen nadat deze is gemaakt, maar u deze optie uitschakelen kunt door het verwijderen van alle de paden van de collectie.

1.  Ga naar **Instellingen**in de portal OMS.
2.  Klik op **gegevens** en vervolgens **aangepaste logboeken**.
3.  Klik op **Details** naast de definitie van aangepaste logboek uit te schakelen.
4.  De collectie paden voor de definitie van aangepaste logboek verwijderen.


## <a name="data-collection"></a>Het verzamelen van gegevens

Logboek Analytics verzamelt nieuwe posten van elke aangepaste log ongeveer elke 5 minuten.  De agent wordt zijn plaats in elk logboek wordt verzameld uit opgenomen.  Als de agent voor een periode van tijd off line gaat vervolgens verzamelt logboek Analytics posten vanaf waar het laatst gestopt, zelfs als deze posten zijn gemaakt, terwijl de agent off line was.

De volledige inhoud van de vermelding zijn naar een enkele eigenschap met de naam **Nieuwrap:**geschreven.  U kunt dit parseren in meerdere eigenschappen die kunnen worden geanalyseerd en afzonderlijk worden gezocht door het definiëren van [Aangepaste velden](log-analytics-custom-fields.md) wanneer u het aangepaste logboek hebt gemaakt.


## <a name="custom-log-record-properties"></a>Aangepaste eigenschappen van record

Aangepaste logboekrecords is een type met de naam van het logboek dat u opgeeft en de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| TimeGenerated | Datum en tijd waarop de record is verzameld door logboek Analytics.  Als het logboek een scheidingsteken wordt gebruikt op basis van de is dit de tijd die zijn verzameld via de post. |
| SourceSystem  | Type agent die van de record is verzameld. <br> OpsManager – Windows agent, een directe verbinding of SCOM <br> Linux – alle Linux-agenten  |
| Nieuwrap:             | Volledige tekst van het verzamelde gegeven. |
| ManagementGroupName | De naam van de groep management voor SCOM agenten.  Voor andere functionarissen, is dit AOI -\<-ID voor werkruimte\> |


## <a name="log-searches-with-custom-log-records"></a>Zoekopdrachten met aangepaste logboekrecords logboek

Records uit aangepaste logboeken worden opgeslagen in de opslagplaats OMS net zoals records uit een andere gegevensbron.  Hebben ze een type dat overeenkomt met de naam die u opgeeft bij het definiëren van het logboekbestand, zodat u de eigenschap Type in uw zoekopdracht kunt op te halen records van een bepaald logboek verzameld.

De volgende tabel bevat voorbeelden van logboek zoekopdrachten waarmee u records uit aangepaste logboeken ophaalt.

| Query | Beschrijving |
|:--|:--|
| Type = MyApp_CL | Alle gebeurtenissen uit een aangepaste zich aanmelden met de naam MyApp_CL. |
| Type = MyApp_CL Severity_CF = fout | Alle gebeurtenissen uit een aangepaste logbestand met de naam MyApp_CL met een waarde van een *fout* in een aangepast veld met de naam *Severity_CF*. |




## <a name="sample-walkthrough-of-adding-a-custom-log"></a>Voorbeeld scenario van het toevoegen van een aangepaste logboek

De volgende sectie helpt bij een voorbeeld van een aangepaste logboek te maken.  Het monster logboek verzameld heeft één vermelding per regel begint met een datum en tijd en klik vervolgens met door komma's gescheiden velden voor de code, de status en het bericht.  Hieronder ziet u enkele voorbeelden van vermeldingen.

    2016-03-10 01:34:36 207,Success,Client 05a26a97-272a-4bc9-8f64-269d154b0e39 connected
    2016-03-10 01:33:33 208,Warning,Client ec53d95c-1c88-41ae-8174-92104212de5d disconnected
    2016-03-10 01:35:44 209,Success,Transaction 10d65890-b003-48f8-9cfc-9c74b51189c8 succeeded
    2016-03-10 01:38:22 302,Error,Application could not connect to database
    2016-03-10 01:31:34 303,Error,Application lost connection to database

### <a name="upload-and-parse-a-sample-log"></a>Uploaden en een logboek monster parseren

Wij bieden een van de logboekbestanden en de gebeurtenissen die zal worden verzamelen kunnen zien.  Nieuwe regel is in dit geval een voldoende scheidingsteken.  Als u één vermelding in het logboek kan verdeeld over meerdere regels, moet een tijdstempel scheidingstekens worden gebruikt.

![Uploaden en een logboek monster parseren](media/log-analytics-data-sources-custom-logs/delimiter.png)

### <a name="add-log-collection-paths"></a>Logboek collectie paden toevoegen

De logboekbestanden bevindt zich in *C:\MyApp\Logs*.  Elke dag met een naam met de datum in het patroon *appYYYYMMDD.log*wordt een nieuw bestand gemaakt.  Een voldoende patroon voor dit logboek worden *C:\MyApp\Logs\\\*.log*.

![Logboekpad-collectie](media/log-analytics-data-sources-custom-logs/collection-path.png)

### <a name="provide-a-name-and-description-for-the-log"></a>Geef een naam en beschrijving voor het logboek

We gebruiken een naam van *MyApp_CL* en typ een **Beschrijving**.

![Naam van het logboek](media/log-analytics-data-sources-custom-logs/log-name.png)


### <a name="validate-that-the-custom-logs-are-being-collected"></a>Valideren dat de aangepaste logboeken worden verzameld

We gebruiken een query van het *Type = MyApp_CL* voor alle records uit het logboek verzameld.

![Logboek-query zonder aangepaste velden](media/log-analytics-data-sources-custom-logs/query-01.png)

### <a name="parse-the-custom-log-entries"></a>De aangepaste vermeldingen parseren

We aangepaste velden gebruiken voor het definiëren van de *EventTime*, de *Code*, de *Status*en de velden *bericht* en we zien het verschil in de records die worden geretourneerd door de query.

![Query voor logboek met aangepaste velden](media/log-analytics-data-sources-custom-logs/query-02.png)

## <a name="next-steps"></a>Volgende stappen

- [Aangepaste velden](log-analytics-custom-fields.md) worden de vermeldingen in het logboek voor aangepaste geparseerd in afzonderlijke velden gebruiken.
- Informatie over het [logboek zoeken](log-analytics-log-searches.md) voor het analyseren van de gegevens die worden verzameld uit gegevensbronnen en oplossingen. 