<properties
   pageTitle="Het maken van een ticket voor de ondersteuning voor SQL Data Warehouse | Microsoft Azure"
   description="Het maken van een support ticket in Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/01/2016"
   ms.author="sonyama;barbkess"/>

# <a name="how-to-create-a-support-ticket-for-sql-data-warehouse"></a>Het maken van een ticket voor de ondersteuning voor SQL Data Warehouse
 
Als u problemen met uw datawarehouse SQL hoeft Maak ticket een ondersteuning zodat onze engineering-team kan u helpen.

## <a name="create-a-support-ticket"></a>Maak een support ticket

1. Open de [Azure portal][].

2. Klik op de tegel **Help + ondersteuning** op het beginscherm.

    ![Help + ondersteuning](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)

3. In het menu Help + ondersteuning blade, klik op **verzoek voor ondersteuning van maken**.

    ![Nieuw ondersteuningsverzoek](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
    
    <a name="request-quota-change"></a> 

4. Selecteer het **Type aanvragen**.

    ![Aanvraagtype](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
    
    >[AZURE.NOTE]  Elke SQL server (bijv. myserver.database.windows.net) heeft een **Quotum DTU** van 45,000. Dit quotum is gewoon een limiet van de veiligheid. Door een support ticket maken en *Quota* te selecteren als het aanvraagtype, kunt u uw quota verhogen. Voor het berekenen van de DTU nodig heeft, de 7.5 te vermenigvuldigen met het totale [dat DWU][] nodig. Bijvoorbeeld u wilt twee DW6000s op een SQL-server als host, en vervolgens moet u een quotum DTU van 90,000 aanvragen.  U kunt uw huidige DTU verbruik van de SQL server-blade weergeven in de portal. Onderbroken en voortgezet databases tellen naar de DTU-quota. 

5. Selecteer het **abonnement** die fungeert als host voor de database met de probleemrapportage.

    ![Abonnement](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)

6. **SQL datawarehouse** als de Resource selecteren.

    ![Resource](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)

7. Selecteer uw [Azure plan ondersteunen][].

    - Ondersteuning voor **Facturering, beheer van quota en het abonnement** is beschikbaar op alle niveaus van ondersteuning.
    - **Break-fix** ondersteuning is beschikbaar via de [ontwikkelaar][], [standaard][], [Directe Professional][] of [Premier][] support. Break-fix problemen zijn problemen opgetreden door klanten tijdens het gebruik van Azure indien er een redelijke verwachting dat Microsoft het probleem veroorzaakt.
    - **Ontwikkelaar begeleiding** en **adviezen** zijn beschikbaar op [Directe Professional][] - en [Premier][] support. 
    
    Als u een Premier support plan hebt, kunt u ook SQL Data Warehouse melden problemen op [Microsoft Premier online portal][].  Zie[ondersteuning van Azure plan] [Azure ondersteuning van plannen]voor meer informatie over de verschillende ondersteuningsplannen, met inbegrip van de scope, responstijden, prijzen, enz.  Veelgestelde vragen over Azure ondersteuning, Zie de [Veelgestelde vragen over ondersteuning van Azure][].  

    ![Ondersteuningsplan](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)

8. Selecteer het **probleemtype** en de **categorie**.

    ![De categorie type probleem](./media/sql-data-warehouse-get-started-create-support-ticket/problem-type-category.png)

9. Beschrijf het probleem en kies het niveau van de impact op uw bedrijf.

    ![Beschrijving van het probleem](./media/sql-data-warehouse-get-started-create-support-ticket/problem-description.png)

10. Uw **contactgegevens** voor dit ticket support zijn al ingevuld. Werk dit indien nodig.

    ![Contactgegevens](./media/sql-data-warehouse-get-started-create-support-ticket/contact-info.png)

11. Klik op **maken** om de aanvraag voor ondersteuning indienen.


## <a name="monitor-a-support-ticket"></a>Monitor een support ticket

Nadat u het verzoek om ondersteuning hebben ingediend, de Azure ondersteuningsteam neemt contact met u. Om te controleren uw aanvraag en details, klikt u op het dashboard op **ondersteuningsverzoeken beheren** .

![Status controleren](./media/sql-data-warehouse-get-started-create-support-ticket/check-status.png)

## <a name="other-resources"></a>Andere bronnen

Bovendien kunt u met de Gemeenschap SQL Data Warehouse op [Stack Overflow][] of in het [forum MSDN Azure SQL Data-magazijn][].

<!--Image references--> 

<!--Article references--> 
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

<!--MSDN references--> 

<!--Other web references--> 
[Azure portal]: https://portal.azure.com/
[Azure ondersteuningsplan]: https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/  
[Ontwikkelaar]: https://azure.microsoft.com/support/plans/developer/  
[Standaard]: https://azure.microsoft.com/support/plans/standard/  
[Professionele Direct]: https://azure.microsoft.com/support/plans/prodirect/  
[Premier]: https://azure.microsoft.com/support/plans/premier/  
[Veelgestelde vragen over ondersteuning van Azure]: https://azure.microsoft.com/support/faq/
[Microsoft Premier online portal]: https://premier.microsoft.com/
[Stack Overflow]: https://stackoverflow.com/questions/tagged/azure-sqldw/
[Azure SQL gegevens magazijn MSDN-forum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/

