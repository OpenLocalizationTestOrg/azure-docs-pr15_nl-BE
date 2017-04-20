<properties
    pageTitle="Bewaken en beheren van een elastische database toepassingen met Azure portal | Microsoft Azure"
    description="Informatie over het gebruik van de Azure portal en SQL-Database ingebouwde intelligentie te beheren, bewaken en breedte een schaalbare elastische database toepassingen om databaseprestaties te optimaliseren en kosten te beheren."
    keywords=""
    services="sql-database"
    documentationCenter=""
    authors="ninarn"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/22/2016"
    ms.author="ninarn"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="monitor-and-manage-an-elastic-database-pool-with-the-azure-portal"></a>Bewaken en beheren van een elastische database toepassingen met Azure portal

> [AZURE.SELECTOR]
- [Azure portal](sql-database-elastic-pool-manage-portal.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)


U kunt de Azure portal bewaken en beheren van een elastische database-toepassingen en de databases in de groep. U kunt het gebruik van een elastische toepassingen en databases binnen die groep controleren vanuit de portal. U kunt ook een reeks wijzigingen aanbrengen in uw groep elastisch en alle wijzigingen tegelijk verzenden. Deze wijzigingen omvatten het toevoegen of verwijderen van databases, elastische groep instellingen wijzigen of uw database-instellingen wijzigen.

De onderstaande afbeelding ziet u een voorbeeld van de elastische toepassingen. De weergave bevat:

*  Grafieken voor het brongebruik van de elastische toepassingen en van de databases in de groep opgenomen.
*  **Configureren** van toepassingen om de wijzigingen aanbrengen in de elastische toepassingen.
*  De knop **database maken** die een nieuwe database gemaakt en toegevoegd aan de huidige elastische toepassingen.
*  Elastische taken waarmee u beheren een groot aantal databases Transact SQL-scripts uitgevoerd op alle databases in een lijst.

![Toepassingen weergeven][2]

Als u wilt werken via de stappen in dit artikel, moet u een SQL server in Azure met ten minste een database en een elastische toepassingen. Als er niet een elastische toepassingen, Zie [een groep maken](sql-database-elastic-pool-create-portal.md). Als u geen database hebt, ziet u de [zelfstudie voor SQL-database](sql-database-get-started.md).

## <a name="elastic-pool-monitoring"></a>Elastische toepassingen controleren

Gaat u naar een bepaalde groep van toepassingen voor een overzicht van het gebruik van bronnen. De groep is standaard geconfigureerd voor het gebruik van opslag- en eDTU weergeven voor het laatste uur. De grafiek kan worden geconfigureerd om verschillende statistieken over diverse tijdvensters weergeven.

1. Selecteer een verzameling met werken.
2. Onder **Elastische toepassingen controleren** heet een grafiek **brongebruik**. Klik op de grafiek.

    ![Elastische toepassingen controleren][3]

    De blade **Metric** wordt geopend met een gedetailleerde weergave van de opgegeven parameters via de opgegeven tijdsduur.   

    ![Metrische blade][9]

### <a name="to-customize-the-chart-display"></a>De grafiekweergave aanpassen

Kunt u de grafiek en de metrische blade weer te geven van andere criteria zoals CPU-percentage, i/o-percentage gegevens en logboek i/o-percentage gebruikt.

2. Klik op **bewerken**op de metrische blade.

    ![Klik op bewerken][6]

- In de **Grafiek bewerken** -blade een nieuwe periode (afgelopen uur vandaag of afgelopen week), of klik op **aangepast** datumbereik selecteren in de laatste twee weken. Selecteer het grafiektype (de balk of regel) en selecteer vervolgens de middelen om te controleren.

> Opmerking: Alleen metrieken met dezelfde eenheid kunnen worden weergegeven in de grafiek tegelijkertijd. Bijvoorbeeld als u "percentage" eDTU"is vervolgens u alleen mogelijk andere metrieken met percentage als de maateenheid selecteren.

    ![Click edit](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

- Klik vervolgens op **OK**.


## <a name="elastic-database-monitoring"></a>Elastische database controleren

Afzonderlijke databases kunnen ook voor potentiële problemen worden gecontroleerd.

1. Onder **Elastische Database controleren**is er een grafiek waarin de parameters voor de vijf databases. Standaard het diagram toont de top 5-databases in de groep door het gebruik van de gemiddelde eDTU in het afgelopen uur. Klik op de grafiek.

    ![Elastische toepassingen controleren][4]

2. De blade **Database Resourcegebruik** wordt weergegeven. Dit biedt een gedetailleerde weergave van het gebruik van de database in de groep. Met behulp van het raster in het onderste gedeelte van het blad, kunt u een of meer databases in de groep weer te geven van het gebruik ervan in de grafiek (maximaal 5-databases). U kunt ook de maatstaven en de tijd venster door te klikken op de **grafiek bewerken**in de grafiek weergegeven.

    ![Database resource gebruik blade][8]

### <a name="to-customize-the-view"></a>De weergave aanpassen

1. In de blade **brongebruik Database** klikt u op de **grafiek te bewerken**.

    ![Klik in de grafiek bewerken](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

2. In het blad van de grafiek **bewerken** een nieuwe periode (na een uur of na 24 uur), of klik op **aangepast** als u een andere dag in de afgelopen 2 weken weer te geven.

    ![Klik op aangepast](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

3. Klik op de vervolgkeuzelijst **databases door vergelijken** als u wilt een ander metriek te gebruiken bij het vergelijken van databases.

    ![De grafiek bewerken](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="to-select-databases-to-monitor"></a>Selecteer databases controleren

In de databaselijst in de **Database brongebruik** blade vindt u specifieke databases door te kijken door de pagina's in de lijst of typt u de naam van een database. Gebruik het selectievakje in om de database te selecteren.

![Zoeken naar databases controleren][7]


## <a name="add-an-alert-to-a-pool-resource"></a>Een waarschuwing toevoegen aan een resourcegroep resource

U kunt regels toevoegen aan resources die per e-mail aan personen of waarschuwing tekenreeksen eindpunten URL als de bron een drempel die u hebt ingesteld voor gebruik raakt.

> [AZURE.IMPORTANT]Brongebruik controleren voor elastische toepassingen heeft een vertraging van ten minste 20 minuten. Instellen van waarschuwingen van minder dan 30 minuten voor elastische toepassingen wordt momenteel niet ondersteund. Waarschuwingen instellen voor elastisch van toepassingen met een punt (parameter met de naam "-venstergrootte" in PowerShell API) van minder dan 30 minuten wordt niet geactiveerd. Zorg ervoor dat de waarschuwingen die u voor groepen van elastische definieert een periode (WindowSize) van 30 minuten of langer.

**Een waarschuwing toevoegen aan elke resource:**

1. Klik op de grafiek **Resourcegebruik** openen de **Metric** blade, klikt u op **waarschuwing toevoegen**en vervolgens de gegevens invult in het **toevoegen van een waarschuwingsregel** blade (**Resource** wordt automatisch ingesteld op de groep waaraan u werkt worden).
2. Typ een **naam** en **Beschrijving** ter aanduiding van de waarschuwing voor u en de geadresseerden.
3. Een **Metric** die u wilt waarschuwen uit de lijst kiezen.

    De grafiek wordt dynamisch brongebruik voor die waarde kunt u een drempel kiezen.

4. Kies een **voorwaarde** (groter dan, kleiner dan, enz.) en een **drempel**.
5. Klik op **OK**.



## <a name="move-a-database-into-an-elastic-pool"></a>Een database verplaatsen naar een elastische toepassingen

U kunt toevoegen of verwijderen van databases uit een bestaande groep. De databases kunnen worden in andere toepassingen. U kunt echter alleen databases die zich op dezelfde logische server toevoegen.

1. In het blad voor de toepassingen, klikt u op **groepen van toepassingen configureren** **elastische databases** .

    ![Klik op groepen van toepassingen configureren][1]

2. Klik op **toevoegen aan groep**in het blad **van toepassingen configureren** .

    ![Klik op toevoegen aan groep](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)


3. Selecteer de database of databases toevoegen aan de groep in de blade **-databases toevoegen** . Klik vervolgens op **selecteren**.

    ![Selecteer databases toevoegen](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

    Het **configureren van toepassingen** blade bevat nu de database die u hebt geselecteerd om te worden toegevoegd met de status ingesteld op **in behandeling**.

    ![Behandeling van toepassingen toevoegen](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

3. Klik op **Opslaan**in het **configureren van toepassingen blade**.

    ![Klik op Opslaan](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="move-a-database-out-of-an-elastic-pool"></a>Een database uit een elastisch toepassingen verplaatsen

1. Selecteer de database of databases verwijderen in het blad **van toepassingen configureren** .

    ![databases weergeven](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

2. Klik op **verwijderen uit groep**.

    ![databases weergeven](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

    Het **configureren van toepassingen** blade bevat nu de database die u hebt geselecteerd om te worden verwijderd met de status ingesteld op **in behandeling**.

    ![voorbeeld database toevoeging en verwijdering](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

3. Klik op **Opslaan**in het **configureren van toepassingen blade**.

    ![Klik op Opslaan](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="change-performance-settings-of-a-pool"></a>Prestatie-instellingen van een groep wijzigen

Tijdens het controleren van het gebruik van bronnen van een groep van toepassingen is het mogelijk dat bepaalde aanpassingen nodig zijn. Misschien moet de groep een wijziging in de grenzen van prestaties of opslag. Mogelijk wilt u de database wijzigen in de groep. U kunt de instellingen van de groep wijzigen op elk gewenst moment om de beste balans van prestaties en kosten. Zie [Wanneer moet een elastische database-toepassingen worden gebruikt?](sql-database-elastic-pool-guidance.md) voor meer informatie.

**Als u de eDTUs of opslag beperkt per groep van toepassingen en eDTUs per database:**

1. Open de blade **configureren van toepassingen** .

    Gebruik een schuifregelaar wijzigen van de instellingen voor de onder **elastische instellingen voor groep van toepassingen**.

    ![Elastische pool brongebruik](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

2. Wanneer de instelling verandert, ziet de weergave u de geschatte maandelijkse kosten van de wijziging.

    ![Een groep van toepassingen en nieuwe maandelijkse kosten bijwerken](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)


## <a name="create-and-manage-elastic-jobs"></a>Maken en beheren van taken elastisch

Elastische taken kunnen u de Transact-SQL-scripts uitvoeren op een onbeperkt aantal databases in de groep. U kunt nieuwe taken maken of bestaande projecten met behulp van de portal beheren.

![Maken en beheren van taken elastisch][5]


Voordat u taken, taken elastische onderdelen installeren en uw referenties invoeren. Zie [overzicht van elastische database](sql-database-elastic-jobs-overview.md)voor meer informatie.

[Scaling out met Azure SQL-Database](sql-database-elastic-scale-introduction.md)Zie: elastische Databasehulpprogramma's gebruiken om te schalen, verplaatsen van gegevens, te zoeken of te maken.



## <a name="additional-resources"></a>Aanvullende bronnen

- [Elastische toepassingen SQL-Database](sql-database-elastic-pool.md)
- [Een elastische database-toepassingen maken met de portal](sql-database-elastic-pool-create-csharp.md)
- [Maak een elastische database toepassingen met PowerShell](sql-database-elastic-pool-create-powershell.md)
- [Maak een elastische database toepassingen met C#](sql-database-elastic-pool-create-csharp.md)
- [Overwegingen voor prijs- prestatieverhouding voor elastische database toepassingen](sql-database-elastic-pool-guidance.md)


<!--Image references-->
[1]: ./media/sql-database-elastic-pool-manage-portal/configure-pool.png
[2]: ./media/sql-database-elastic-pool-manage-portal/basic.png
[3]: ./media/sql-database-elastic-pool-manage-portal/basic-2.png
[4]: ./media/sql-database-elastic-pool-manage-portal/basic-3.png
[5]: ./media/sql-database-elastic-pool-manage-portal/elastic-jobs.png
[6]: ./media/sql-database-elastic-pool-manage-portal/edit-metric.png
[7]: ./media/sql-database-elastic-pool-manage-portal/select-dbs.png
[8]: ./media/sql-database-elastic-pool-manage-portal/db-utilization.png
[9]: ./media/sql-database-elastic-pool-manage-portal/metric.png
