<properties 
   pageTitle="Gebruikt u de weergave van de uitvoering van hoekpunt in Lake hulpmiddelen voor Visual Studio | Microsoft Azure" 
   description="Informatie over het werken met de weergave van de uitvoering van hoekpunt aan examen gegevens Lake Analytics projecten." 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/13/2016"
   ms.author="jgao"/>

# <a name="use-the-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a>Gebruik de weergave hoekpunt uitvoering in Lake hulpmiddelen voor Visual Studio

Informatie over het werken met de weergave van de uitvoering van hoekpunt aan examen gegevens Lake Analytics projecten.

## <a name="prerequisites"></a>Vereisten

- Basiskennis op het gebied van het gebruik van hulpmiddelen voor Visual Studio Lake U SQL-script ontwikkelen.  Zie [Zelfstudie: U SQL scripts schrijven met behulp van hulpmiddelen voor gegevens Lake voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

## <a name="open-the-vertex-execution-view"></a>Open de Vertex uitvoering

Voor een bepaalde functie, kunt u de koppeling 'Hoekpunt uitvoering weergeven' in de linkerbenedenhoek. Mogelijk moet u eerst laden van profielen en het kan even duren afhankelijk van uw verbinding met het netwerk.

![Gegevens Lake Analytics extra hoekpunt uitvoering weergave](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a>Begrijpt u hoekpunt uitvoering weergeven

Na het invoeren van de weergave hoekpunt worden uitgevoerd, zijn er drie delen:

![Gegevens Lake Analytics extra hoekpunt uitvoering weergave](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

- Hoekpunt selector: links ziet u de selector hoekpunt.  U kunt de hoekpunten door functies selecteren (zoals top 10-gegevens lezen, of kies fase).

    Een van de meest gebruikte filters is de hoekpunten op het kritieke pad. Kritieke pad is de langste pad van een project U SQL. Het is handig voor het optimaliseren van uw taken door te controleren welke hoekpunt duurt het langst.

- Het deelvenster midden boven:

    ![Gegevens Lake Analytics extra hoekpunt uitvoering weergave](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

    Deze weergave toont ook de huidige status van alle hoekpunten. Dienovereenkomstig converteert de tijd op uw lokale computer en toont verschillende status in verschillende kleuren.

- Het middenvenster onder:

    ![Gegevens Lake Analytics extra hoekpunt uitvoering weergave](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

    - Naam: De naam van het exemplaar van het hoekpunt. Het bestaat uit verschillende delen in StageName | VertexName | VertexRunInstance. Het SV7_Split [62] .v1 hoekpunt staat bijvoorbeeld voor de tweede sessie (.v1, index begint bij 0) van hoekpunt nummer 62 in de SV7_Split fase.
    - Totale gegevens lezen/schrijven: De gegevens zijn gelezen of weggeschreven door dit hoekpunt.
    - Status/Exit Status: De definitieve status als het hoekpunt is beëindigd.
    - : Exit Code/fout de fout wanneer het hoekpunt is mislukt.
    - Maken: de reden Waarom het hoekpunt is gemaakt.
    - Resource latentie/proces latentie/PN wachtrij vertraging: de tijd voor de wachttijd voor resources, om gegevens te verwerken en om te blijven in de wachtrij.
    - GUID proces/Creator: GUID voor de huidige actieve hoekpunt of de maker.
    - Versie: de N-de instantie van het actieve hoekpunt (het systeem kunt plannen nieuwe exemplaren van een hoekpunt voor vele redenen, bijvoorbeeld failover redundantie, enz berekenen.)
    - Versie gemaakt van de tijd.
    - Maken Start tijd/proces in wachtrij tijd/proces Start tijd/proces volledig verwerken tijd: bij het starten van het proces van het hoekpunt maken; Als het hoekpunt begint in de wachtrij; Wanneer het bepaalde hoekpunt begint; Als het hoekpunt bepaalde is voltooid.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van gegevens Lake Analytics, [Azure gegevens Lake Analytics overzicht](data-lake-analytics-overview.md).
- Zie [ontwikkelt U SQL - scripts met behulp van hulpmiddelen voor gegevens Lake voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)om te beginnen U SQL-toepassingen ontwikkelen.
- Zie informatie over U SQL, [aan de slag met Azure gegevens Lake Analytics U SQL - taal](data-lake-analytics-u-sql-get-started.md).
- Zie voor beheertaken, [Azure gegevens Lake Analytics beheren met behulp van Azure portal](data-lake-analytics-manage-use-portal.md).
- Zie [toegang tot diagnostische logboeken voor Azure gegevens Lake Analytics](data-lake-analytics-diagnostic-logs.md) om diagnostische gegevens vastleggen,
- Een complexe query's, Zie [Logboeken van Website analyseren met behulp van Azure gegevens Lake Analytics](data-lake-analytics-analyze-weblogs.md).
- Taakdetails Zie [Gebruik taak Browser en taak weergeven voor taken van Azure gegevens lake Analytics](data-lake-analytics-data-lake-tools-view-jobs.md)