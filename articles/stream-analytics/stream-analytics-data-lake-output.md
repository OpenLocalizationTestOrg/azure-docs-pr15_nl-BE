<properties
    pageTitle="Output stream Analytics gegevensarchief Lake | Microsoft Azure"
    description="Configuratie van verificatie en machtiging van een Azure Lake gegevensarchief in een Stream Analytics taak"
    keywords=""
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="stream-analytics-data-lake-store-output"></a>Output stream Analytics gegevensarchief Lake

Stream Analytics projecten ondersteunen verschillende uitvoermethoden, namelijk een [Azure Lake gegevensarchief](https://azure.microsoft.com/services/data-lake-store/). Azure Lake gegevensarchief is een hele onderneming hyperscale '-opslagplaats voor grote gegevens analytisch werkbelasting. Lake gegevensarchief kunt u gegevens van elke grootte, type en opname snelheid voor operationele en experimentele analytics op te slaan.

## <a name="authorize-a-data-lake-store-account"></a>Toestaan dat een account gegevensarchief Lake

1.  Als gegevensarchief Lake als uitvoer in de portal Management Azure is geselecteerd, wordt u gevraagd te machtigen het gebruik van uw bestaande Lake gegevensarchief of voor het aanvragen van toegang tot de Lake winkel voorbeeld van gegevens via de klassieke Azure-Portal.

    ![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-authorization.png)  

2.  U hebt al toegang tot de gegevensopslag Lake, klikt u op 'Nu toestaan' als voor een korte tijd een pagina verschijnt die aangeeft "Omleidt naar de vergunning..". De pagina automatisch wordt gesloten en u krijgt de pagina waarmee u de uitvoer van het gegevensarchief Lake configureren.

Als u niet hebt aangemeld voor een voorbeeld van gegevens Lake winkel, kunt u de koppeling 'Nu aanmelden' om de aanvraag of volgt u de [instructies gestart](../data-lake-store/data-lake-store-get-started-portal.md).

## <a name="configure-the-data-lake-store-output-properties"></a>Configureer de eigenschappen van de uitvoer gegevensarchief Lake

Wanneer u het gegevensarchief Lake account geverifieerd hebt, kunt u de eigenschappen voor de uitvoer van het gegevensarchief Lake. In de volgende tabel wordt de lijst met namen van eigenschappen en de bijbehorende beschrijvingen voor het configureren van uw uitvoer Lake gegevensarchief.

<table>
<tbody>
<tr>
<td><B>NAAM VAN EIGENSCHAP</B></td>
<td><B>BESCHRIJVING</B></td>
</tr>
<tr>
<td>Uitvoeralias</td>
<td>Dit is een beschrijvende naam in query's worden gebruikt om de query-uitvoer naar deze Lake gegevensopslag.</td>
</tr>
<tr>
<td>Lake gegevensarchief Account</td>
<td>De naam van de opslag account waar u de uitvoer wilt verzenden. U krijgt een vervolgkeuzelijst met Lake gegevensarchief accounts waaraan de gebruiker is aangemeld bij de portal toegang tot heeft.</td>
</tr>
<tr>
<td>Pad Prefix patroon [<I>optioneel</I>]</td>
<td>Het pad van het schrijven van uw bestanden in de opgegeven gegevens Lake Store-Account gebruikt. <BR>{datum}, {time}<BR>Voorbeeld 1: Map1/logboeken / {datum} / {time}<BR>Voorbeeld 2: Map1/logboeken / {datum}</td>
</tr>
<tr>
<td>Datumnotatie [<I>optioneel</I>]</td>
<td>Als het token van de datum in het pad van het voorvoegsel wordt gebruikt, kunt u de notatie waarin de bestanden worden geordend. Voorbeeld: Jjjj/MM/DD</td>
</tr>
<tr>
<td>Tijdnotatie [<I>optioneel</I>]</td>
<td>Als het token van de tijd in het pad van het voorvoegsel wordt gebruikt, geeft u de indeling waarin de bestanden worden geordend. Op dit moment is de enige ondersteunde waarde uu.</td>
</tr>
<tr>
<td>Gebeurtenis serialisatie-indeling</td>
<td>Indeling serialisatie voor uitvoergegevens. JSON, CSV- en Avro worden ondersteund.</td>
</tr>
<tr>
<td>Codering</td>
<td>Als CSV- of JSON opmaken, moet een codering worden opgegeven. UTF-8 is de enige ondersteunde coderingsindeling op dit moment.</td>
</tr>
<tr>
<td>Scheidingsteken</td>
<td>Alleen van toepassing op CSV-serialisatie. Stream Analytics ondersteunt een aantal algemene scheidingstekens voor het serialiseren van CSV-gegevens. Ondersteunde waarden zijn door komma's, puntkomma, spatie, tab en de verticale balk.</td>
</tr>
<tr>
<td>Indeling</td>
<td>Alleen van toepassing op JSON serialisatie. Regel gescheiden geeft aan dat de uitvoer wordt opgemaakt door elk gescheiden door een nieuwe regel JSON-object. Matrix geeft aan dat de uitvoer wordt opgemaakt als een array van objecten JSON.</td>
</tr>
</tbody>
</table>

## <a name="renew-data-lake-store-authorization"></a>Vergunning Lake gegevensarchief vernieuwen

Er is momenteel een beperking waar de verificatietoken moet worden handmatig vernieuwd voor alle taken met uitvoer Lake gegevensarchief 90 dagen. Ook moet u uw account Lake gegevensarchief opnieuw te verifiëren als u uw wachtwoord hebt gewijzigd sinds de taak is gemaakt of laatst geverifieerd. Een symptoom van dit probleem is geen uitvoer van de taak en een fout in de logboeken opnieuw toestemming moet geven.

De actieve taak beëindigen en gaat u naar de uitvoer van het gegevensarchief Lake dit probleem op te lossen. Klik op de link "Vernieuwen vergunning" en voor een korte tijd een pagina verschijnt die aangeeft "Omleidt naar de vergunning..". De pagina wordt wordt automatisch gesloten en als dit lukt, 'Vergunning is vernieuwd'. U en vervolgens klikt u op 'Opslaan' onder aan de pagina wilt en u kunt doorgaan met het opnieuw starten van de taak van de laatste keer gestopt om gegevensverlies te voorkomen.

![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-renew-authorization.png)
