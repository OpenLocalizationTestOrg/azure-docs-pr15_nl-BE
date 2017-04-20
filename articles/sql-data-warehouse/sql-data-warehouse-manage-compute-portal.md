<properties
   pageTitle="Beheren van rekenkracht in Azure SQL Data Warehouse (Azure portal) | Microsoft Azure"
   description="Azure portal taken voor het beheren van rekenkracht. Schaal berekenen bronnen door DWUs aan te passen. Of onderbreken en hervatten middelen om op te slaan van de kosten berekenen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/22/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a>Beheren van rekenkracht in Azure SQL Data Warehouse (Azure portal)

> [AZURE.SELECTOR]
- [Overzicht](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [REST](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Prestaties door schalen schaal berekenen bronnen en het geheugen om te voldoen aan de veranderende behoeften van uw werkbelasting. Kosten besparen door de schaal terug resources tijdens niet-piekuren of onderbreken compute helemaal. 

Deze verzameling taken Azure portal gebruikt:

- Schaal berekenen
- Compute onderbreken
- Cv berekenen

Voor meer informatie, Zie [beheren overzicht berekenen][].

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Schaal rekenkracht

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Compute resources wijzigen:

1. De [Azure portal][]openen, opent u de database en klik op **schaal**.

    ![Klik op schaal][1]

1. Verplaats de schuifknop naar links of rechts te wijzigen van de instelling van de DWU in het blad schaal.

    ![Schuifregelaar][2]

1. Klik op **Opslaan**. Er verschijnt een bevestigingsbericht. Klik op **Ja** om te bevestigen of **niet** annuleren.

    ![Klik op Opslaan][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Compute onderbreken

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Om een database te onderbreken:

1. Open de [Azure portal][] en uw database. U ziet dat de Status **on line**is. 

    ![On line status][6]

1. Compute-en geheugenbronnen onderbreken, klikt u op **onderbreken**en een bericht wordt weergegeven. Klik op **Ja** om te bevestigen of **niet** annuleren.

    ![Onderbreken bevestigen][7]

1. Terwijl de database is SQL Data Warehouse wordt gestart, wordt de status **onderbreken**.
2. Wanneer de status **onderbroken**, de bewerking pause wordt uitgevoerd en u bent niet meer in rekening worden gebracht voor de DWUs.

    ![Status van de pauze][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Cv berekenen

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]Hervatten van een database:

1. Open de [Azure portal][] en uw database. U ziet dat de Status **onderbroken**. 

    ![Pause-database][4]

1. Als u wilt doorgaan met de database klikt u op **Start**en vervolgens een bevestigingsbericht verschijnt. Klik op **Ja** om te bevestigen of **niet** annuleren.

    ![Cv bevestigen][5]

1. Terwijl de database is SQL Data Warehouse wordt gestart, wordt de status "Resuming".
2. Wanneer de status **on line**is, wordt de database gereed is.

    ![On line status][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Volgende stappen
Zie [beheer-overzicht][]voor meer informatie.

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png
[4]: ./media/sql-data-warehouse-manage-compute-portal/resume-database.png
[5]: ./media/sql-data-warehouse-manage-compute-portal/resume-confirm.png
[6]: ./media/sql-data-warehouse-manage-compute-portal/pause-database.png
[7]: ./media/sql-data-warehouse-manage-compute-portal/pause-confirm.png

<!--Article references-->
[Beheer-overzicht]: ./sql-data-warehouse-overview-manage.md
[Overzicht compute beheren]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
