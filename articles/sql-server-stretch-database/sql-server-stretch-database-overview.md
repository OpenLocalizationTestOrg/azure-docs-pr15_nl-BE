<properties
    pageTitle="Overzicht van Database uitrekken | Microsoft Azure"
    description="Informatie over hoe uitrekken Database gemigreerd koude gegevens transparant en veilig naar de cloud met Microsoft Azure."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/27/2016"
    ms.author="douglasl"/>

# <a name="stretch-database-overview"></a>Overzicht van de Database vullen

Uitrekken Database gemigreerd koude gegevens transparant en veilig naar de cloud met Microsoft Azure.

Als u direct beginnen met de Database uitrekken wilt, Zie [aan de slag door de Database inschakelen voor uitrekken Wizard uit te voeren](sql-server-stretch-database-wizard.md).

## <a name="what-are-the-benefits-of-stretch-database"></a>Wat zijn de voordelen van uitrekken Database?
Uitrekken Database biedt de volgende voordelen:

### <a name="provides-cost-effective-availability-for-cold-data"></a>Kosten biedt\-effectief beschikbaarheid voor koude gegevens
Warme en koude transactionele gegevens dynamisch vanuit SQL Server naar Microsoft Azure uitrekken met uitrekken van SQL Server-Database. In tegenstelling tot de typische koude gegevensopslag, uw gegevens zijn altijd on line en beschikbaar zijn voor query's. Zonder verbreking van de bank voor grote tabellen zoals Historie klant kunt u meer gegevens bewaren tijdlijnen bieden. Profiteren van de lage kosten van Azure in plaats van schalen, dure op\-opslag bedrijfsruimten. U kiest de prijzen laag en -instellingen configureren in de Portal Azure te handhaven, controle over de kosten. Schaal omhoog of omlaag wanneer dat nodig is. Ga naar de pagina met [Prijzen van SQL Server uitrekken Database](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/) voor meer informatie.

### <a name="doesnt-require-changes-to-queries-or-applications"></a>Wijzigingen in query's of toepassingen nodig niet
Toegang tot uw SQL Server-gegevens naadloos ongeacht of deze zich op\-bedrijfsruimten of uitrekken naar de cloud.  U instellen het beleid dat bepaalt waar gegevens worden opgeslagen en de SQL-Server verwerkt de verplaatsing van gegevens op de achtergrond. De volledige tabel is altijd online en query. En, uitrekken Database geen wijzigingen in bestaande query's of toepassingen vereist: de locatie van de gegevens is volledig transparant voor de toepassing.

### <a name="streamlines-on-premises-data-maintenance"></a>Stroomlijnt op\-gegevensonderhoud bedrijfsruimten
Reduceren\-bedrijfsruimten onderhoud en opslag van uw gegevens. Back-ups voor uw op\-gebouwen gegevens sneller uitvoeren en voltooien binnen het onderhoudsvenster. Back-ups voor de cloud-gedeelte van uw gegevens automatisch worden uitgevoerd. Uw op\-ruimten opslagbehoeften aanzienlijk worden verminderd. Azure opslag is 80% goedkoper dan toe te voegen aan op\-SSD bedrijfsruimten.

### <a name="keeps-your-data-secure-even-during-migration"></a>Beveiligt uw gegevens zelfs tijdens de migratie
Geniet van gemoedsrust als u uw belangrijkste toepassingen naar de cloud veilig uitrekken. SQL-Server altijd gecodeerd biedt codering voor de gegevens in beweging. Rij niveau beveiliging (RLS) en andere geavanceerde beveiligingsvoorzieningen van SQL Server ook werken met de Database uitrekken om uw gegevens te beschermen.

## <a name="what-does-stretch-database-do"></a>Wat is Database uitrekken?
Nadat u de Database uitrekken voor een exemplaar van SQL Server, een database en ten minste één tabel hebt ingeschakeld, begint uitrekken Database zonder de koude om gegevens te migreren naar Azure.

-   Als u koude gegevens in een afzonderlijke tabel opslaat, kunt u de hele tabel te migreren.

-   Als uw tabel koud en warm gegevens bevat, kunt u een filterfunctie selecteert u de rijen wilt migreren.

**U hebt geen bestaande query's en de clienttoepassingen wijzigen.** U nog steeds naadloze toegang tot lokale en externe gegevens, zelfs tijdens de gegevensmigratie. Er is een kleine hoeveelheid latentie voor externe query's, maar u alleen deze vertraging optreden bij de koude querygegevens.

**Dat uitrekken Database zorgt ervoor dat er geen gegevens verloren gaan** als er een fout optreedt tijdens de migratie. Het heeft ook de logica voor het verwerken van problemen die tijdens de migratie optreden kunnen opnieuw. Een weergave van dynamische management geeft de status van de migratie.

**U kunt onderbreken gegevensmigratie** problemen op de lokale server of maximaliseren van de beschikbare netwerkbandbreedte.

![Overzicht van de database vullen][StretchOverviewImage1]

## <a name="is-stretch-database-for-you"></a>Uitrekken Database is voor u?
Als u de volgende instructies maken kunt, kunt uitrekken Database om te voldoen aan uw vereisten en uw problemen op te lossen.

|Als u een beslisser|Als u een DBA|
|------------------------------|-------------------|
|Ik heb transactionele gegevens bewaren gedurende een lange tijd.|De grootte van tabellen is ophalen uit.|
|Soms heb ik de koude gegevens opvragen.|Mijn gebruikers zeggen dat ze toegang tot gegevens met koud willen, maar zij het slechts zelden gebruiken.|
|Ik heb apps, met inbegrip van oudere toepassingen, die niet moeten worden bijgewerkt.|Ik moet kopen en meer opslagruimte toe te voegen.|
|Ik wil een andere manier om geld te besparen op de opslag.|Ik kan een back-up of terugzetten van dergelijke grote tabellen binnen de SLA.|

## <a name="what-kind-of-databases-and-tables-are-candidates-for-stretch-database"></a>Welke databases en tabellen zijn kandidaten voor uitrekken Database?
Ambitieuze doelen transactionele databases Database met veel koude gegevens, meestal opgeslagen in een klein aantal tabellen. Deze tabellen kunnen meer dan een miljard rijen bevatten.

Als u de tijdelijke tabel functie van SQL Server 2016, uitrekken Database gebruiken voor het migreren van alle of een deel van de historietabel gekoppeld aan de kosten\-effectieve opslag in Azure. Voor meer info, Zie [Beheren van behoud van historische gegevens in systeem Versioned tijdelijke tabellen](https://msdn.microsoft.com/library/mt637341.aspx).

Uitrekken Database adviseur, een functie van SQL Server 2016 Upgrade Advisor, gebruiken om tabellen en databases voor uitrekken Database. Zie voor meer info [identificeren databases en tabellen voor de Database uitrekken](sql-server-stretch-database-identify-databases.md). Zie voor meer informatie over potentiële belemmeringen, [beperkingen voor uitrekken Database](sql-server-stretch-database-limitations.md).

## <a name="test-drive-stretch-database"></a>Station uitrekken Database testen
**Database met de AdventureWorks-voorbeelddatabase uitrekken station testen.** Als u de AdventureWorks-voorbeelddatabase, ten minste het bestand en het bestand monsters en scripts van downloaden [hier](https://www.microsoft.com/download/details.aspx?id=49502). Nadat u de database op een exemplaar van SQL Server 2016 terugzet, unzip het bestand en open het bestand DB uitrekken uit de map uitrekken DB. Voer de scripts in dit bestand om te controleren, de ruimte gebruikt door uw gegevens voordat en nadat u Stretch-Database voor het bijhouden van de voortgang van de gegevensmigratie en om te bevestigen dat u kunt blijven bestaande gegevens opvragen en invoegen van nieuwe gegevens, zowel tijdens als na de gegevensmigratie.

## <a name="next-step"></a>Volgende stap
**Databases en tabellen die kandidaten voor uitrekken Database zijn identificeren.** SQL Server 2016 Upgrade Advisor downloaden en uitvoeren van de adviseur van de Database uitrekken om databases en tabellen die kandidaten voor uitrekken Database zijn te identificeren. Uitrekken Database Advisor geeft ook de problematische kwesties. Zie voor meer info [identificeren databases en tabellen voor de Database uitrekken](sql-server-stretch-database-identify-databases.md).

<!--Image references-->
[StretchOverviewImage1]: ./media/sql-server-stretch-database-overview/StretchDBOverview.png
[StretchOverviewImage2]: ./media/sql-server-stretch-database-overview/StretchDBOverview1.png
[StretchOverviewImage3]: ./media/sql-server-stretch-database-overview/StretchDBOverview2.png
