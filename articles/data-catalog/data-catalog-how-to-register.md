<properties
   pageTitle="Het registreren van gegevensbronnen | Microsoft Azure"
   description="Hoe kan ik artikel markeren het registreren van gegevensbronnen met Azure gegevenscatalogus, inclusief de metagegevensvelden tijdens de registratie hebt uitgepakt."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>


# <a name="how-to-register-data-sources"></a>Het registreren van gegevensbronnen

## <a name="introduction"></a>Inleiding
**Catalogus gegevens van Microsoft Azure** is een volledig beheerde cloud service die als een systeem van registratie en het systeem van discovery voor gegevensbronnen van de onderneming fungeert. **Catalogus met Azure gegevens** is met andere woorden, helpen mensen ontdekken, te begrijpen en gegevensbronnen en helpt organisaties gebruikt om meer waarde uit hun bestaande gegevens bij. En de eerste stap bij het maken van een gegevensbron kan worden gevonden via een **Catalogus met Azure gegevens** te registreren die gegevensbron.
## <a name="registering-data-sources"></a>Registreren van gegevensbronnen
Registratie is het proces van het extraheren van metagegevens uit de gegevensbron en deze gegevens kopiëren naar de service **Catalogus Azure-gegevens** . De gegevens blijft waar het momenteel is opgeslagen en blijft onder de controle van de groep administrators en het beleid van het huidige systeem.

Als u wilt registreren in een gegevensbron, **Azure gegevenscatalogus** gegevensbron registratiehulpprogramma vanaf de portal **Catalogus met Azure gegevens** gewoon te starten. Log in met uw werk of school-account (dezelfde Azure Active Directory referenties kunt u zich aanmelden bij de portal) en selecteer vervolgens de gegevensbron die u wilt registreren.
Zie de handleiding [Aan de slag met Azure gegevenscatalogus](data-catalog-get-started.md) voor meer meer informatie.

Nadat de gegevensbron is geregistreerd, wordt de catalogus bijgehouden van de locatie en indexeert de metagegevens, zodat gebruikers kunnen zoeken, bladeren, de gegevensbron ontdekken en gebruik vervolgens de locatie verbinding maken met behulp van de toepassing of het hulpprogramma van hun keuze.

## <a name="sources-supported"></a>Bronnen die worden ondersteund
Zie [Gegevens catalogus DSR](data-catalog-dsr.md) voor de lijst met ondersteunde gegevensbronnen.
<br/>


## <a name="structural-metadata"></a>Structurele metagegevens
Wanneer u een gegevensbron registreren wilt, informatie over de structuur van de objecten die u selecteert wordt opgehaald door het hulpprogramma voor inschrijving – dit is bedoeld als structurele metagegevens.

Voor alle objecten bevatten deze structurele metagegevens van de locatie van het object, zodat gebruikers die de gegevens vinden die informatie gebruiken kunnen om verbinding met het object in de clienthulpprogramma's van hun keuze te. Andere structurele metagegevens bevat de naam en het type en typ kenmerk/kolom naam en gegevens.

## <a name="descriptive-metadata"></a>Beschrijvende metadata
Naast de core structurele metagegevens opgehaald uit de gegevensbron, wordt het hulpprogramma voor de registratie van gegevens ook ook beschrijvende metagegevens uitpakken. Voor SQL Server Analysis Services en SQL Server Reporting Services, is deze afkomstig van de eigenschappen beschrijving van deze services. Voor SQL Server, worden waarden die zijn opgegeven met de eigenschap extended ms_description uitgepakt. Voor Oracle-Database, wordt het hulpprogramma voor de registratie van gegevens de kolom opmerkingen uit de weergave ALL_TAB_COMMENTS uitgepakt.

Naast de beschrijvende metagegevens opgehaald uit de gegevensbron, kunnen gebruikers ook beschrijvende metadata met behulp van het hulpprogramma voor de registratie van gegevens invoeren. Gebruikers kunnen tags toevoegen en deskundigen voor de objecten die worden geregistreerd kunnen identificeren. Al deze beschrijvende metagegevens wordt gekopieerd naar de service **Catalogus met Azure gegevens** samen met de structurele metagegevens.

## <a name="including-previews"></a>Inclusief voorbeelden

Standaard alleen metagegevens uit de gegevensbronnen opgehaald en gekopieerd naar de service **Catalogus Azure-gegevens** , maar wat dat een gegevensbron wordt vaak eenvoudiger gemaakt door ziet een voorbeeld van de gegevens bevat.

Het hulpprogramma **Azure gegevenscatalogus** data source registratie kan gebruikers bevatten een voorbeeld van de momentopname van de gegevens in elke tabel en de weergave die is geregistreerd. Als de gebruiker ervoor kiest om op te nemen van voorbeelden tijdens de registratie, in het hulpprogramma voor inschrijving maximaal 20 records uit elke tabel en elk beeld opgenomen. Deze momentopname wordt vervolgens gekopieerd naar de catalogus en de structurele en beschrijvende metagegevens.


> [AZURE.NOTE]  Grote tabellen met een groot aantal kolommen wellicht minder dan 20 records opgenomen in de voorvertoning.


## <a name="including-data-profiles"></a>Inclusief gegevens profielen

Net als inclusief voorbeelden waardevolle context voor gebruikers die zoeken naar gegevensbronnen in **Azure gegevenscatalogus bieden kunnen**, kunt met inbegrip van een profiel met gegevens ook gemakkelijker te begrijpen ontdekte gegevensbronnen.

Het hulpprogramma **Azure gegevenscatalogus** data source registratie kan gebruikers een profiel met gegevens voor elke tabel en de weergave die is geregistreerd. Als de gebruiker een profiel gegevens opnemen tijdens de registratie, het hulpprogramma voor inschrijving nemen op statistische gegevens over de gegevens in elke tabel en de view, met inbegrip van:

* Het aantal rijen en de grootte van de gegevens in het object
* De datum voor de meest recente update van de gegevens en het schema van het object
* Het aantal records met null en afzonderlijke waarden voor kolommen
* De waarden voor het minimum, maximum, gemiddelde en standaardafwijking voor kolommen

Deze statistieken worden vervolgens gekopieerd naar de catalogus en de structurele en beschrijvende metagegevens.

> [AZURE.NOTE]  Tekst en datum kolommen bevatten statistieken gemiddelde of standaarddeviatie niet in hun profiel gegevens.

## <a name="updating-registrations"></a>Bijwerken van registraties

Registreren van een gegevensbron wordt detecteerbaar in de **Catalogus van Azure gegevens** met behulp van metagegevens en optionele preview tijdens de registratie hebt uitgepakt. Als de gegevensbron worden bijgewerkt in de catalogus moet (bijvoorbeeld als een gebruiker wil bijwerken van de gegevens in de voorbeelden is het schema van een object gewijzigd, of tabellen oorspronkelijk uitgesloten opgenomen worden moeten) kan het hulpprogramma voor de registratie van gegevens opnieuw worden uitgevoerd.

Opnieuw registreren van een al ingeschreven gegevensbron wordt een uitgevoerd samenvoegen 'upsert': bestaande objecten bijgewerkt, terwijl de nieuwe objecten worden gemaakt. Alle metagegevens die door gebruikers via de portal **Catalogus Azure-gegevens** blijven behouden.

## <a name="summary"></a>Samenvatting
Registreren van een gegevensbron met **Azure catalogus met gegevens** eenvoudiger die gegevensbron te ontdekken en te begrijpen, door structurele en beschrijvende metagegevens uit de gegevensbron te kopiëren in de catalogus service. Nadat u een gegevensbron is geregistreerd, kan deze vervolgens aangevuld, beheren en ontdekt via de portal **Catalogus Azure-gegevens** .

## <a name="see-also"></a>Zie ook
- Zelfstudie [Aan de slag met Azure gegevenscatalogus](data-catalog-get-started.md) voor meer informatie over het registreren van gegevensbronnen.
