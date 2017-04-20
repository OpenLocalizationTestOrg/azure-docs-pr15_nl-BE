<properties
    pageTitle="Analytics platforms: Apache Storm vergelijking Stream Analytics | Microsoft Azure"
    description="Lees hoe u een wolk analytics platform kiezen met behulp van een Apache Storm vergelijking met Analytics Stream. Functies en de verschillen begrijpt."
    keywords="Analytics platform, analytics platforms, cloud analytics platform, storm vergelijking"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="help-choosing-a-streaming-analytics-platform-apache-storm-comparison-to-azure-stream-analytics"></a>Helpen bij het kiezen van een streaming analytics platform: Apache Storm vergelijking met Azure Stream Analytics

Lees hoe u een wolk analytics platform kiezen met behulp van deze vergelijking Apache Storm Azure Stream Analytics. Begrijp de proposities van Stream Analytics versus Storm Apache als een beheerde service op Azure HDInsight, zodat u de juiste oplossing voor uw bedrijf kunt use-cases.

Beide analytics platforms bieden de voordelen van een PaaS-oplossing, er zijn een paar grote opvallende mogelijkheden die ze onderscheiden. Mogelijkheden en de beperkingen van deze services worden hieronder weergegeven waarmee u neerzetten op de oplossing die u nodig hebt om uw doelen te bereiken.

## <a name="storm-comparison-to-stream-analytics-general-features"></a>Storm-Stream Analytics vergelijking: algemene functies ##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm op HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Bron openen</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Nee, Azure Stream Analytics is een farmaceutische Microsoft bieden.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ja, Apache Storm is een technologie voor de Apache-licentie.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Microsoft ondersteund</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ja </p>
            </td>
            <td width="246" valign="top">
                <p>
Ja </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Hardwarevereisten</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Er zijn geen hardwarevereisten. Azure Stream Analytics is een Service Azure.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Er zijn geen hardwarevereisten. Apache Storm is een Service Azure.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Eenheid van het hoogste niveau</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Met Azure Stream Analytics klanten implementeren en bewaken van streaming projecten.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Met Apache Storm op HDInsight klanten implementeren en bewaken van een hele cluster die host meerdere Storm taken, alsmede andere werkbelasting (incl. batch) kan optreden.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Prijs</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Stream Analytics is geprijsd door de omvang van de gegevens die worden verwerkt en het aantal streaming (per uur die de taak wordt uitgevoerd) vereist.
                </p>
                <p>
                    <a href="http://azure.microsoft.com/en-us/pricing/details/stream-analytics/">Prijzen meer informatie vindt u hier.</a>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Storm-Apache op HDInsight, de eenheid van de aankoop is gebaseerd op het cluster en wordt berekend op basis van de tijd die het cluster actief is, onafhankelijk van de taken die zijn geïmplementeerd.
                </p>
                <p>
                    <a href="http://azure.microsoft.com/en-us/pricing/details/hdinsight/">Prijzen meer informatie vindt u hier.</a>
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Op elk platform analytics ontwerpen##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm op HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>-Mogelijkheden: SQL-DSL</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ja, een eenvoudig te gebruiken SQL ondersteuning beschikbaar is.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Nee, moeten gebruikers code schrijven in Java C# of API's van Trident gebruiken.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Mogelijkheden: Tijdelijke operators</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Venstermodus aggregaten en tijdelijke joins worden ondersteund uit de doos.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Tijdelijke marktdeelnemers moeten worden uitgevoerd door de gebruiker.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Ontwikkeling ervaring</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Interactieve ontwerpen en foutopsporing via Azure Portal-ervaring op voorbeeldgegevens.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ontwikkeling, foutopsporing en ervaring controleren wordt verstrekt via de Visual Studio .NET-gebruikers, voor Java en andere talen-ontwikkelaars de IDE van hun keuze kan gebruiken.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Ondersteuning voor foutopsporing</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Stream Analytics biedt elementaire taakstatus en bewerkingen Logboeken bij wijze van het opsporen van fouten, maar momenteel niet de mogelijkheid biedt flexibiliteit in wat en hoe veel is opgenomen in de logboeken ie: uitgebreide modus.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Gedetailleerde logboeken zijn voor foutopsporing beschikbaar. Op twee manieren aan oppervlak logboeken aan gebruiker, via visual Studio of gebruiker kunt u RDP in het cluster toegang tot logboeken.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Ondersteuning voor UDF (User Defined functies)</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Er is momenteel geen ondersteuning voor UDF's.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
UDF's kunnen worden geschreven in C#, Java of de taal van uw keuze.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Uitbreidbaar - aangepaste code</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Er is geen ondersteuning voor extensible code in Analytics Stream.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ja, is de beschikbaarheid van aangepaste code schrijven in C#, Java of andere ondersteunde talen op Storm.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Gegevensbronnen en uitgangen##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm op HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                 <strong>Ingevoerde gegevens-bronnen</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>De ondersteunde invoerbronnen zijn Azure gebeurtenis Hubs en Azure BLOB's.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Verbindingslijnen zijn beschikbaar voor de gebeurtenis Hubs, Service Bus, Kafka, enz. Niet-ondersteunde verbindingslijnen kunnen worden uitgevoerd via een aangepaste code.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Invoergegevens opgemaakt</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ondersteunde indelingen voor invoer zijn Avro, JSON, CSV.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Elke indeling kan via aangepaste code worden uitgevoerd.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Uitgangen</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Een streaming project kan meerdere uitgangen hebben. Ondersteunde uitgangen: Azure gebeurtenis Hubs, Azure Blob-opslag Azure tabellen, Azure SQL DB en PowerBI.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ondersteuning voor veel uitgangen in een topologie voor elke uitvoer aangepaste logica voor downstream-verwerkingen mogelijk. Uit de doos bevat Storm connectoren voor PowerBI, Azure gebeurtenis Hubs, Azure Blob opslag, Azure, DocumentDB, SQL en HBase. Niet-ondersteunde verbindingslijnen kunnen worden uitgevoerd via een aangepaste code.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Bestandsindelingen voor codering van gegevens</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Stream Analytics moet UTF-8-indeling te gebruiken.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Gegevens coderingsindeling via aangepaste code kan worden uitgevoerd.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Beheer en de exploitatie##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm op HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Model implementatie project</strong>
                </p>
                <p>
                    - <strong>Azure Portal</strong>
                </p>
                <p>
                    - <strong>Visual Studio</strong>
                </p>
                <p>
                    - <strong>PowerShell</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Implementatie is geïmplementeerd via de Portal Azure, PowerShell en REST API's.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Depolyment wordt geïmplementeerd via de Portal Azure, PowerShell, Visual Studio en REST API's.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Controle (statistieken, tellers, enz.)</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Controle wordt geïmplementeerd via Azure Portal en REST API's.
                </p>
                <p>
De gebruiker kan ook Azure waarschuwingen configureren.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Controle wordt geïmplementeerd via de gebruikersinterface van Storm en REST API's.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Schaalbaarheid</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Aantal eenheden Streaming voor elke taak. Elke eenheid Streaming verwerkt tot 1 MB/s. Maximum van 50 eenheden standaard. Aanroep limiet verhogen.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Het aantal knooppunten in het cluster HDI Storm. Geen limiet op het aantal knooppunten (bovenste limiet wordt gedefinieerd door uw Azure quotum). Aanroep limiet verhogen.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Grenzen van de gegevensverwerking</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Gebruikers kunnen schaal hoger of lager aantal eenheden Streaming gegevensverwerking verhogen of kosten te optimaliseren.
                </p>
                <p>
Schalen tot 1 GB/s </p>
            </td>
            <td width="246" valign="top">
                <p>
Gebruiker kan schaal omhoog of omlaag clustergrootte behoeften.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Stoppen/hervatten</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Stoppen en hervatten vanaf laatste plaats gestopt.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Stoppen en hervatten vanaf laatste plaats gestopt op basis van het watermerk.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Update service en kader</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Automatisch herstellen met geen downtime.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Automatisch herstellen met geen downtime.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Bedrijfscontinuïteit via een uiterst Service met gegarandeerde SLA</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
SLA van 99,9% uptime </p>
                <p>
Automatisch herstellen na storingen </p>
                <p>
Herstel van stateful temporal operators is ingebouwd.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
SLA van 99,9% uptime van de Storm-cluster. Apache Storm is een fout met betrekking tot fouttolerante streaming-platform, maar het is de verantwoordelijkheid van de klanten om dat hun streaming taken ononderbroken uitgevoerd.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Geavanceerde functies##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure Stream Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache Storm op HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Late aankomst en volgorde gebeurtenisafhandeling</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ingebouwde configureerbare beleid opnieuw rangschikken, neerzetten of gebeurtenistijd aanpassen.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Gebruiker kan de logica voor het verwerken van dit scenario moet implementeren.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Referentiegegevens</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
De referentiegegevens van Azure BLOB's beschikbaar voor de maximale grootte van 100 MB cache opzoeken in het geheugen. Verwijzingsgegevens worden vernieuwd wordt door de service beheerd.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Geen beperkingen op de gegevensgrootte. Verbindingslijnen zijn beschikbaar voor HBase, DocumentDB, SQL Server en Azure. Niet-ondersteunde verbindingslijnen kunnen worden uitgevoerd via een aangepaste code.
                </p>
                <p>
Verwijzingsgegevens worden vernieuwd moet worden behandeld door de aangepaste code.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Integratie met de computer leren</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Gepubliceerd door het configureren van Azure Machine Learning modellen als functies tijdens ASA taak maken <a href="http://blogs.msdn.com/b/streamanalytics/archive/2015/05/24/real-time-scoring-of-streaming-data-using-machine-learning-models.aspx">(persoonlijke voorbeeld)</a>.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Beschikbaar via Storm bouten.
                </p>
            </td>
        </tr>
    </tbody>
</table>
