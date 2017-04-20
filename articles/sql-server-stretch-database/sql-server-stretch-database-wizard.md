<properties
    pageTitle="Aan de slag door het uitvoeren van de Database inschakelen voor uitrekken Wizard | Microsoft Azure"
    description="Informatie over het configureren van een database voor uitrekken Database door de Database inschakelen voor uitrekken Wizard uit te voeren."
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
    ms.topic="hero-article"
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="get-started-by-running-the-enable-database-for-stretch-wizard"></a>Aan de slag door het uitvoeren van de Database inschakelen voor de Wizard uitrekken

Voor meer informatie over het configureren van een database voor uitrekken Database de Database inschakelen voor uitrekken Wizard worden uitgevoerd.  Dit onderwerp beschrijft de informatie die u moet invoeren en de keuzes die u moet maken in de wizard.

Zie voor meer informatie over de Database uitrekken, [Stretch-Database](sql-server-stretch-database-overview.md).

 >   [AZURE.NOTE] Later, als uitrekken Database is uitgeschakeld, moet u die Database uitrekken uit te schakelen voor een tabel of voor een database niet het externe object verwijdert. Als u verwijderen van de externe tabel of de externe database wilt, hebt u via de portal management Azure neer. De externe objecten blijven Azure kosten totdat u deze handmatig verwijderen. 

## <a name="launch-the-wizard"></a>De wizard starten

1.  Selecteer de database die u wilt uitrekken inschakelen in SQL Server Management Studio in Object Explorer.

2.  Rechts\-Klik op **taken**, en selecteer vervolgens **Uitrekken**en selecteer Selecteer **inschakelen** om de wizard te starten.

## <a name="Intro"></a>Inleiding
Het doel van de wizard en de vereisten bekijken.

De belangrijke vereisten zijn:

-   U moet een beheerder zijn om database-instellingen te wijzigen.
-   U hebt een Microsoft Azure-abonnement hebben.
-   De SQL-Server heeft om te kunnen communiceren met de externe server Azure.

![Introductiepagina van de wizard uitrekken][StretchWizardImage1]

## <a name="Tables"></a>Tabellen selecteren
Selecteer de tabellen die u wilt inschakelen voor uitrekken.

Tabellen met een groot aantal rijen worden weergegeven aan de bovenkant van de gesorteerde lijst. Voordat de lijst van tabellen in de Wizard wordt weergegeven, wordt deze voor de gegevenstypen die momenteel niet worden ondersteund door uitrekken Database geanalyseerd.

![Pagina van de wizard uitrekken tabellen selecteren][StretchWizardImage2]

|Kolom|Beschrijving|
|----------|---------------|
|(geen titel)|Schakel het selectievakje in als u in deze kolom om de geselecteerde tabel inschakelen voor uitrekken.|
|**Naam**|Hiermee geeft u de naam van de kolom in de tabel.|
|(geen titel)|Een symbool in deze kolom kan een waarschuwing worden aangeduid die\'t voorkomen dat u de geselecteerde tabel inschakelen voor uitrekken. Het kan ook een blokkering probleem dat verhindert dat u de geselecteerde tabel inschakelen voor uitrekken vertegenwoordigen \- bijvoorbeeld, omdat de tabel maakt gebruik van een niet-ondersteund gegevenstype. De muisaanwijzer op het symbool om meer info in de knopinfo weer te geven. Zie [beperkingen voor uitrekken Database](sql-server-stretch-database-limitations.md)voor meer informatie.|
|**Uitgerekt**|Hiermee wordt aangegeven of de tabel al is ingeschakeld voor uitrekken.|
|**Migreren**|U kunt een gehele tabel (**Hele tabel**) migreren of u kunt een filter instellen voor een bestaande kolom in de tabel. Als u wilt een ander filter-functie gebruiken om rijen te migreren, selecteer uitvoeren van de instructie ALTER TABLE om op te geven van de filter-functie nadat u de wizard af te sluiten. Zie [rijen te migreren met behulp van de filterfunctie van een selecteren](sql-server-stretch-database-predicate-function.md)voor meer informatie over de filterfunctie. Zie voor meer informatie over het toepassen van de functie [Uitrekken Database inschakelen voor een tabel](sql-server-stretch-database-enable-table.md) of [ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx).|
|**Rijen**|Hiermee geeft u het aantal rijen in de tabel.|
|**Grootte (KB)**|Hiermee geeft u de grootte van de tabel in KB.|

## <a name="Filter"></a>Geef desgewenst een filter rij

Als u voorzien van een filter-functie om rijen wilt te migreren te selecteren, het volgende doen op de pagina **Selecteer tabellen** .

1.  Klik op **Hele tabel** in de rij van de tabel in de lijst **Selecteer de tabellen die u wilt uitrekken** . Hiermee opent u het dialoogvenster **selecteert u de rijen worden uitgerekt** .

    ![Een filterfunctie definiëren][StretchWizardImage2a]

2.  Selecteer **Rijen kiezen**in het dialoogvenster **selecteert u de rijen worden uitgerekt** .

3.  Geef in het **veld Naam**een naam voor de filter-functie.

4.  Een kolom uit de tabel kiezen, kiest u een operator en een waarde voor de **Where** -component.

5. Klik op **controleren** om de functie te testen. Als de functie resultaten worden geretourneerd uit de tabel - dat wil zeggen, rapporten als er voor het migreren van rijen die voldoen aan de voorwaarde - de test **geslaagd**.

    >   [AZURE.NOTE] Het tekstvak waarin u de filterquery is alleen-lezen. U kunt de query in het tekstvak niet bewerken.

6.  Klik op gereed om terug te keren naar de pagina **Selecteer tabellen** .

De filter-functie wordt gemaakt in SQL Server alleen wanneer u de wizard hebt voltooid. Tot die tijd kunt u terugkeren naar de pagina **Selecteer tabellen** wijzigen of wijzig de naam van de filter-functie.

![Selecteer tabellen pagina na het definiëren van een filter-functie][StretchWizardImage2b]

Als u een ander type filter-functie gebruiken om rijen te migreren selecteren, doet u een van de volgende dingen.  

-   De wizard af te sluiten en uitvoeren van de instructie ALTER TABLE uitrekken inschakelen voor de tabel en een filter-functie opgeven. Zie voor meer info [Uitrekken Database inschakelen voor een tabel](sql-server-stretch-database-enable-table.md).  

-   Uitvoeren van de instructie ALTER TABLE een filterfunctie opgeven nadat u de wizard af te sluiten. Zie [een filterfunctie na het uitvoeren van de Wizard toevoegen](sql-server-stretch-database-predicate-function.md#addafterwiz)voor de vereiste stappen.

## <a name="Configure"></a>Azure-implementatie configureren

1.  Aanmelden bij Microsoft Azure met een Microsoft-account.

    ![Aanmelden bij Azure - wizard Database uitrekken][StretchWizardImage3]

2.  Selecteer de bestaande Azure abonnement voor uitrekken.

3.  Selecteer een regio Azure.
    -   Als u een nieuwe server maakt, wordt de server gemaakt in dit gebied.  
    -   Als u bestaande servers in het geselecteerde gebied, de wizard worden ze als u een **bestaande server**kiest.

    Kies de Azure regio waarin de SQL-Server zich bevindt om te beperken. Voor meer informatie over de regio's Zie [Regio's Azure](https://azure.microsoft.com/regions/).

4.  Geef op of u wilt een bestaande server gebruiken of een nieuwe Azure server maken.

    Als Active Directory op de SQL Server met Azure Active Directory federated is, kunt u desgewenst een federatieve serviceaccount voor SQL Server gebruiken om te communiceren met de externe server Azure. Zie voor meer informatie over de vereisten voor deze optie [ALTER databaseopties instellen (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx).

    -   **Nieuwe server maken**

        1.  Maak een aanmeldings-id en wachtwoord voor de beheerder van de server.

        2.  Gebruik eventueel een federatieve serviceaccount voor SQL Server om te communiceren met de externe server Azure.

        ![Maak nieuwe Azure server - wizard Database uitrekken][StretchWizardImage4]

    -   **Bestaande server**

        1.  Selecteer de bestaande server Azure.

        2.  Selecteer de verificatiemethode.

            -   Als u **SQL Server-verificatie**selecteert, geven de aanmelding als beheerder en het wachtwoord.

            -   Selecteer een federatieve serviceaccount voor SQL Server gebruiken om te communiceren met de externe server Azure- **Geïntegreerde verificatie van Active Directory** . Als de geselecteerde server niet met Azure Active Directory is geïntegreerd, is deze optie niet wordt weergegeven.

        ![Selecteer bestaande Azure server - wizard Database uitrekken][StretchWizardImage5]

## <a name="Credentials"></a>Veilige referenties
U moet de sleutel voor het beveiligen van de referenties die uitrekken Database verbinding maakt met de externe database database master.  

Als een database model sleutel al bestaat, moet u het wachtwoord invoeren voor het.  

![Veilige referenties pagina van de wizard Database uitrekken][StretchWizardImage6b]

Als de database niet een bestaande hoofdsleutel, Geef een sterk wachtwoord als u wilt een databasehoofdsleutel maken.  

![Veilige referenties pagina van de wizard Database uitrekken][StretchWizardImage6]

Zie voor meer informatie over de databasehoofdsleutel [hoofdsleutel maken (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) en [maken een hoofdsleutel Database](https://msdn.microsoft.com/library/aa337551.aspx). Zie voor meer info over de referenties die worden gemaakt met de wizard [Maakt DATABASE binnen het bereik van referentie (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx).

## <a name="Network"></a>Selecteer IP-adres
Gebruik het subnet-IP-adresbereik, (aanbevolen) of het openbare IP-adres van uw SQL Server aan een firewallregel maken op Azure waarmee SQL Server communiceren met de externe server Azure.

Vertel de Azure-server kunnen binnenkomende gegevens, query's en beheertaken uit te voeren die is geïnitieerd door SQL Server worden doorgegeven via de firewall Azure of meer IP-adressen die u op deze pagina. De wizard wijzigen niet niets in de instellingen van de firewall op de SQL Server.

![Selecteer een IP-adres pagina van de wizard uitrekken][StretchWizardImage7]

## <a name="Summary"></a>Samenvatting
Controleer de waarden die u hebt ingevoerd en de opties die u hebt geselecteerd in de wizard en de geraamde kosten voor Azure. Selecteer vervolgens **Voltooien** uitrekken inschakelen.

![De overzichtspagina van de wizard uitrekken][StretchWizardImage8]

## <a name="Results"></a>Resultaten
Bekijk de resultaten.

Zie voor het controleren van de status van de gegevensmigratie, [controleren en oplossen van problemen met gegevensmigratie (uitrekken Database)](sql-server-stretch-database-monitor.md).

![Pagina met resultaten van de wizard uitrekken][StretchWizardImage9]

## <a name="KnownIssues"></a>De wizard probleemoplossing
**De wizard Database uitrekken is mislukt.**
Als uitrekken Database nog niet is ingeschakeld op het serverniveau van de en uitvoeren van de wizard zonder de system administrator-machtigingen in te schakelen, wordt de wizard niet. Vraag de systeembeheerder Stretch-Database te activeren op de lokale server-instantie en vervolgens de wizard opnieuw uitvoeren. Zie voor meer info [vereisten: machtiging uitrekken Database inschakelen op de server](sql-server-stretch-database-enable-database.md#EnableTSQLServer).

## <a name="next-steps"></a>Volgende stappen
Extra tabellen voor uitrekken Database inschakelen. Gegevensmigratie bewaken en beheren van Stretch\--compatibele databases en tabellen.

-   [Uitrekken Database inschakelen voor een tabel](sql-server-stretch-database-enable-table.md) inschakelen extra tabellen.

-   [Monitor en gegevensmigratie oplossen](sql-server-stretch-database-monitor.md) voor de status van de gegevensmigratie.

-   [Onderbreken en hervatten van de Database uitrekken](sql-server-stretch-database-pause.md)

-   [Beheren en oplossen van problemen met Database uitrekken](sql-server-stretch-database-manage.md)

-   [Back-up uitrekken geschikte databases](sql-server-stretch-database-backup.md)

## <a name="see-also"></a>Zie ook

[Uitrekken Database voor een database inschakelen](sql-server-stretch-database-enable-database.md)

[Uitrekken Database inschakelen voor een tabel](sql-server-stretch-database-enable-table.md)

[StretchWizardImage1]: ./media/sql-server-stretch-database-wizard/stretchwiz1.png
[StretchWizardImage2]: ./media/sql-server-stretch-database-wizard/stretchwiz2.png
[StretchWizardImage2a]: ./media/sql-server-stretch-database-wizard/stretchwiz2a.png
[StretchWizardImage2b]: ./media/sql-server-stretch-database-wizard/stretchwiz2b.png
[StretchWizardImage3]: ./media/sql-server-stretch-database-wizard/stretchwiz3.png
[StretchWizardImage4]: ./media/sql-server-stretch-database-wizard/stretchwiz4.png
[StretchWizardImage5]: ./media/sql-server-stretch-database-wizard/stretchwiz5.png
[StretchWizardImage6]: ./media/sql-server-stretch-database-wizard/stretchwiz6.png
[StretchWizardImage6b]: ./media/sql-server-stretch-database-wizard/stretchwiz6b.png
[StretchWizardImage7]: ./media/sql-server-stretch-database-wizard/stretchwiz7.png
[StretchWizardImage8]: ./media/sql-server-stretch-database-wizard/stretchwiz8.png
[StretchWizardImage9]: ./media/sql-server-stretch-database-wizard/stretchwiz9.png
