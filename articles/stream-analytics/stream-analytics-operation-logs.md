<properties 
    pageTitle="Fouten opsporen met behulp van de bewerking en geregistreerd in het logboek in Analytics Stream | Microsoft Azure" 
    description="Hoe kan ik gebruik logboeken Stream Analytics" 
    keywords="service, Logboeken"
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
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

# <a name="debug-stream-analytics-jobs-using-service-and-operation-logs"></a>Debug Stream Analytics taken met behulp van Logboeken en de exploitatie

Alle Azure services leveren operationele logboekberichten naar gebruikers te registreren gegevens over beheerswerkzaamheden. Deze informatie kan worden gebruikt voor foutopsporing taakstatus, de voortgang van het project, zoals het in Azure Stream Analytics, en foutberichten voor het bijhouden van de voortgang van een taak na verloop van tijd van verwerking voor uitvoer van start.

## <a name="find-operation-logs-in-the-azure-management-portal"></a>Bewerking worden vastgelegd in de portal Management Azure zoeken

Logboeken u kunt op twee manieren openen:  

- Dashboard van de Stream Analytics taak  
- Diensten in de klassieke Azure portal  

## <a name="dashboard-of-the-stream-analytics-job"></a>Dashboard van de Stream Analytics taak

Een koppeling naar de bijbehorende logboeken van de taak van een Stream Analytics wordt weergegeven op de Dashboard-tabblad van het project. Als u op die koppeling klikt, wordt deze de filters ingesteld op een manier dat het meest recente logs voor die specifieke taak bevat.

  ![Selecteer Services voor logbestanden](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## <a name="management-services"></a>Management Services

Handmatig navigeren naar de bewerking voor de analyse van de stroom en andere diensten in de klassieke Azure portal:

1.  Klik op **Diensten** in de [klassieke Azure portal](https://manage.windowsazure.com).
2.  **Stream Analytics** selecteren voor het **Type** en de naam van de taak voor de **Naam van de Service**.  

  ![Selecteer de Stream Analytics](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## <a name="find-audit-logs-in-the-azure-portal"></a>Controlelogboeken zoeken in de portal voor Azure ##

Operationele logboeken voor uw Stream Analytics-functie in de portal Azure, klik op **Bladeren** en selecteer vervolgens **controlelogboekbestanden**.

  ![Azure portal Stream Analytics selecteren](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

Hiermee opent u een bladeserver met gebeurtenissen van de laatste 7 dagen voor alle resources in uw abonnement.  U kunt filteren om gebeurtenissen te bekijken van een periode of een type opgeven door te klikken op de opdracht **Filter** .

  ![Azure portal Stream Analytics selecteren](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## <a name="get-log-details"></a>Logboek voor meer informatie

U kunt filteren op periode en de Status bekijken van de logboeken voor uw project.

Klik op de knop **Details** onder in het venster meer details weergeven over de geselecteerde gebeurtenis in de portal Management Azure. 

  ![Selecteer Details](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

Klik in de Azure portal op voor een overzicht van de gedetailleerde gegevens van gebeurtenissen in het logboek.

  ![Azure portal Details selecteren](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

Van daaruit kunt u het blad **Detail** openen door te klikken op de gebeurtenis.

  ![Azure portal Details selecteren](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## <a name="debug-a-failed-job"></a>Fouten opsporen in een mislukte taak

Klik op het pictogram zoeken en het type 'mislukte' in de portal Management Azure. Dit geeft een resultaat van alle logboekbestanden met fouten. 

  ![Opsporen van fouten in een mislukte taak](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

In de Azure portal kunt u filteren op het niveau van bericht **kritieke** gebeurtenissen wilt weergeven.

  ![Azure portal foutopsporing](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

U kunt selecteren een van de fouten, en klik op **Details** voor meer informatie over de fout.  Bepaalde foutberichten bevatten ook informatie over het oplossen van het probleem. 

  ![Bewerkingsdetails](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

In het geval u moet contact opnemen met [Support](https://azure.microsoft.com/support/options/) of informatie aan het team via het [MSDN-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics), Let op de Details van de bewerking, met name de **Correlatie-ID**. 

## <a name="get-help"></a>Help-informatie opvragen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) voor verdere ondersteuning

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Analytics Azure Stream](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)
