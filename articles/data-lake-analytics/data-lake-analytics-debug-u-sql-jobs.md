<properties 
   pageTitle="Fouten opsporen in projecten U SQL | Microsoft Azure" 
   description="Informatie over hoe U SQL mislukte hoekpunt met behulp van Visual Studio voor foutopsporing. " 
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
   ms.date="09/02/2016"
   ms.author="jgao"/>



#<a name="debug-c-code-in-u-sql-for-data-lake-analytics-jobs"></a>C#-code in U SQL voor gegevens Lake Analytics taken voor foutopsporing 

Leren werken met Azure gegevens Lake Visual Studio tools voor foutopsporing van mislukte U SQL-opdrachten als gevolg van fouten in de gebruikerscode. 

Het hulpprogramma Visual Studio kunt u gecompileerde code en gegevens nodig vertex downloaden van cluster traceren en opsporen van mislukte taken.

Grote gegevenssystemen leveren gewoonlijk extensibility-objectmodel via talen zoals Java, C#, Python, enz. Veel van deze systemen bieden beperkte runtime debugging informatie, het moeilijk is om runtimefouten opsporen in aangepaste code. De meest recente hulpprogramma's van Visual Studio wordt geleverd met een functie genaamd "Hoekpunt foutopsporing is mislukt". Met deze functie kunt u downloaden de runtime-gegevens van Azure op het lokale werkstation zodat kunt u fouten opsporen in mislukte aangepaste C#-code met dezelfde runtime en exacte invoergegevens vanuit de cloud.  Nadat de problemen zijn opgelost, kunt u de gewijzigde code opnieuw in Azure uitvoeren van de hulpprogramma's.

Zie voor een videopresentatie van deze functie [fouten opsporen in uw aangepaste code in Azure gegevens Lake Analytics](https://mix.office.com/watch/1bt17ibztohcb).

>[AZURE.NOTE] Visual Studio kan vastlopen of crashen als u geen van de volgende twee windows-upgrades: [Microsoft Visual C++ 2015 Redistributable Update 2](https://www.microsoft.com/download/details.aspx?id=51682), [Universele C Runtime voor Windows](https://www.microsoft.com/download/details.aspx?id=50410&wa=wsignin1.0).


##<a name="prerequisites"></a>Vereisten
-   Het artikel [aan de slag](data-lake-analytics-data-lake-tools-get-started.md) zijn gebleven.

## <a name="create-and-configure-debug-projects"></a>Maakt en configureert u foutopsporing projecten

Wanneer u een mislukte taak in gegevens Lake Visual Studio hulpprogramma opent, krijgt u een waarschuwing. De uitgebreide foutinformatie wordt weergegeven in het tabblad fout en de gele balk boven aan het venster. 

![Azure gegevens Lake Analytics U SQL foutopsporing visual studio downloaden hoekpunt](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

**Vertex downloaden en maken van een oplossing voor foutopsporing**

1.  Een mislukte U SQL-taak openen in Visual Studio.
2.  Klik op **downloaden** om de vereiste resources en invoer-streams downloaden. Klik op **opnieuw** als het downloaden is mislukt.
3.  Klik op **openen** nadat het downloaden is voltooid om een debug lokaal project te maken. Een nieuwe Visual Studio-oplossing **VertexDebug** aangeroepen met een leeg project met de naam **LocalVertexHost** gemaakt.

Door gebruiker gedefinieerde operators worden gebruikt in U SQL-code achter (Script.usql.cs), moet u een Class Library C#-project maken met de gebruiker gedefinieerde operators code als het project opnemen in de VertexDebug-oplossing.

Als u dll-assembly's hebt geregistreerd in de database gegevens Lake Analytics, moet u de broncode van de assembly's toevoegen aan de oplossing van VertexDebug.
 
Als u een afzonderlijke C# class library voor uw U SQL-code en geregistreerde dll-assembly's in de database gegevens Lake Analytics gemaakt, moet u de bron C#-project van de assembly's toevoegen aan de oplossing van VertexDebug.

In sommige gevallen kunt u door de gebruiker gedefinieerd operatoren gebruiken in U SQL-code achter het bestand in de oorspronkelijke oplossing (Script.usql.cs). Als u wilt om het werk te maken, moet u een C#-bibliotheek met de broncode maken en wijzigen in de assembly-naam een geregistreerd in het cluster. U kunt de assembly-naam geregistreerd in het cluster aan de hand van het script dat u hebt uitgevoerd in het cluster. U kunt dit doen door het openen van de taak U SQL en klikt u op 'script' in het deelvenster Project. 

**Voor het configureren van de oplossing**

1.  In Solution explorer met de rechtermuisknop op de C#-project dat u zojuist hebt gemaakt en klik vervolgens op **Eigenschappen**.
2.  Het uitvoerpad instellen als LocalVertexHost project pad werkt. U kunt LocalVertexHost project werken pad via LocalVertexHost eigenschappen opvragen.
3.  Het C#-project bouwen zodat het bestand .pdb in het LocalVertexHost project Directory werkt, of u kunt het bestand .pdb handmatig naar deze map kopiëren.
4.  Controleer in **Instellingen voor uitzonderingen**, Common Language Runtime-uitzonderingen:

![Azure gegevens Lake Analytics U SQL foutopsporing visual studio-instelling](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)
 
##<a name="debug-the-job"></a>De functie voor foutopsporing

Nadat u een oplossing voor foutopsporing hebt gemaakt door het downloaden van het hoekpunt en de omgeving hebt geconfigureerd, kunt u beginnen met het opsporen van fouten in uw code U SQL.

1.  In Solution Explorer, met de rechtermuisknop op het **LocalVertexHost** -project dat u zojuist hebt gemaakt, wijst u **Foutopsporing**en klikt u op **nieuwe sessie starten**. De LocalVertexHost moet worden ingesteld als het project is gestart. Ziet u soms het volgende bericht voor de eerste keer dat u kunt negeren. Het kost maximaal één minuut om naar het venster Foutopsporing.
 
    ![Azure gegevens Lake Analytics U SQL foutopsporing visual studio-waarschuwing](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

4.  Gebruik Visual Studio op basis van foutopsporing ervaring (watch, variabelen, enz.) het probleem op te lossen. 
5.  Nadat u een probleem hebt geïdentificeerd, de code vast en wijzigt u de C#-project voordat u het opnieuw totdat alle problemen opgelost zijn. Na de opdracht debug is voltooid, het uitvoervenster met het volgende bericht 

        The Program ‘LocalVertexHost.exe’ has exited with code 0 (0x0).
 
##<a name="resubmit-the-job"></a>De taak opnieuw indienen

Nadat u de foutopsporing U SQL-programmacode hebt voltooid, kunt u de mislukte taak opnieuw indienen.

1. Registreren nieuwe dll-assembly's in de database ADLA.

    1.  Vouw het knooppunt **Databases** vanuit Server Explorer/Cloud Explorer in Lake Visual Studio gereedschap, 
    2.  Klik met de rechtermuisknop assembly's registreren assembly's. 
    3.  Registreer uw nieuwe dll-assembly's in de database ADLA.
 
2.  Of de C#-code kopiëren naar script.usql.cs--C#-code achter het bestand.
3.  De taak opnieuw.

##<a name="next-steps"></a>Volgende stappen

- [Zelfstudie: Aan de slag met Azure gegevens Lake Analytics U SQL-taal](data-lake-analytics-u-sql-get-started.md)
- [Zelfstudie: ontwikkelt U SQL-scripts met behulp van hulpmiddelen voor gegevens Lake voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Door de gebruiker gedefinieerd U SQL-operators voor Azure gegevens Lake Analytics taken ontwikkelen](data-lake-analytics-u-sql-develop-user-defined-operators.md)

