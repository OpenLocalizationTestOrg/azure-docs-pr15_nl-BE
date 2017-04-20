<properties
   pageTitle="Introduceren Azure gegevens Lake Analytics U SQL-catalogus | Azure"
   description="Introduceren Azure gegevens Lake Analytics U SQL-catalogus"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="use-u-sql-catalog"></a>Gebruikt U SQL-catalogus

De catalogus U SQL gebruikt voor het structureren van gegevens en code zodat ze kunnen worden gedeeld door U SQL-scripts. De catalogus kunt de hoogste prestaties met gegevens in Azure gegevens meer mogelijk.

Elke Azure gegevens Lake Analytics-account heeft gekoppeld aan precies één U SQL-catalogus. U kunt de catalogus U SQL niet verwijderen. Op dit moment kan niet U SQL catalogussen worden gedeeld tussen Lake gegevensarchief rekeningen.

Elke U SQL-catalogus bevat een database met de naam **Master**. De Master-Database kan niet worden verwijderd.  Elke catalogus U SQL kan meer extra databases bevatten.

U SQL-database bevat:

- Assembly's – delen .NET code tussen U SQL-scripts.
- Waarden functies – delen U SQL-code tussen U SQL-scripts.
- Tabellen: gegevens delen tussen U SQL-scripts.
- Schema's - delen tabelschema tussen U SQL-scripts.

## <a name="manage-catalogs"></a>Catalogussen beheren
Elke Azure gegevens Lake Analytics-account heeft een standaard Azure Lake gegevensarchief account gekoppeld. Dit gegevensarchief Lake account wordt als standaardaccount Lake gegevensarchief verwezen. U SQL catalogus wordt opgeslagen in het gegevensarchief Lake standaardaccount onder de map van de map/catalogus. Alle bestanden in de map/catalogus-map niet verwijderen.

### <a name="use-azure-portal"></a>Azure portal gebruiken

Zie [gegevens Lake Analytics beheren met behulp van portal](data-lake-analytics-manage-use-portal.md#view-u-sql-catalog)


### <a name="use-data-lake-tools-for-visual-studio"></a>Meer hulpmiddelen gebruiken voor Visual Studio.

Meer hulpmiddelen voor Visual Studio kunt u de catalogus beheren.  Zie voor meer informatie over de hulpprogramma's [Gebruiken meer hulpmiddelen voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

**Voor het beheren van de catalogus**

1. Visual Studio openen en verbinding maken met azure. Zie [verbinding maken met Azure](data-lake-analytics-data-lake-tools-get-started.md#connect-to-azure)voor de instructies.
1. Druk op **CTRL + ALT + S** **Server Explorer** openen.
2. In **Server Explorer** **Azure**uitvouwen, vouw **Gegevens Lake Analytics**uw account gegevens Lake Analytics uitvouwen, vouw **Databases**uit en vouw **master**.



    - Voor het toevoegen van een nieuwe Database en klik vervolgens op **Database maken**met de rechtermuisknop op de **Database**.
    - Als u wilt toevoegen een nieuwe assemblage, **assembly's**met de rechtermuisknop en klik vervolgens op **Assemblage registreren**.
    - Om een nieuw schema wilt toevoegen, klik met de rechtermuisknop op **schema**en klik vervolgens op 'Schema maken **.
    - Een nieuwe tabel toevoegen, klik met de rechtermuisknop op **tabellen**en klik vervolgens op "" maken tabel **.
    - Zie een nieuwe tabelwaardefunctie toevoegen, [ontwikkelt U SQL-door de gebruiker gedefinieerde operators voor gegevens Lake Analytics taken](data-lake-analytics-u-sql-develop-user-defined-operators.md).


![Visual Studio U SQL catalogussen zoeken](./media/data-lake-analytics-use-u-sql-catalog/data-lake-analytics-browse-catalogs.png)


## <a name="see-also"></a>Zie ook

- Aan de slag
    - [Aan de slag met Data Lake Analytics met Azure portal](data-lake-analytics-get-started-portal.md)
    - [Aan de slag met Data Lake Analytics met Azure PowerShell](data-lake-analytics-get-started-powershell.md)
    - [Aan de slag met Data Lake Analytics met Azure .NET SDK](data-lake-analytics-get-started-net-sdk.md)
    - [U SQL scripts schrijven met behulp van hulpmiddelen voor gegevens Lake voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
    - [Aan de slag met Azure gegevens Lake Analytics U SQL-taal](data-lake-analytics-u-sql-get-started.md)

- U SQL en -ontwikkeling
    - [Aan de slag met Azure gegevens Lake Analytics U SQL-taal](data-lake-analytics-u-sql-get-started.md)
    - [U SQL venster functies gebruiken voor taken Azure gegevens Lake Analytics](data-lake-analytics-use-window-functions.md)
    - [U SQL door gebruiker gedefinieerde operators voor gegevens Lake Analytics projecten ontwikkelen](data-lake-analytics-u-sql-develop-user-defined-operators.md)

- Management
    - [Azure gegevens Lake Analytics met Azure portal beheren](data-lake-analytics-manage-use-portal.md)
    - [Azure gegevens Lake Analytics met Azure PowerShell beheren](data-lake-analytics-manage-use-powershell.md)
    - [Controleren en oplossen van problemen met Azure gegevens Lake Analytics taken met Azure portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

- End-to-end-zelfstudie
    - [Interactieve handleiding Azure gegevens Lake Analytics gebruiken](data-lake-analytics-use-interactive-tutorials.md)
    - [Met behulp van Azure gegevens Lake Analytics Website-logboekbestanden analyseren](data-lake-analytics-analyze-weblogs.md)
