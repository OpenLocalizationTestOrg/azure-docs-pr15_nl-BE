<properties
    pageTitle="Verbinding maken met de SQL-Database met behulp van Python | Microsoft Azure"
    description="Geeft een voorbeeld van Python code die kunt u verbinding maken met Azure SQL-Database."
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="meetb"/>


# <a name="connect-to-sql-database-by-using-python"></a>Verbinding maken met de SQL-Database met behulp van Python


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


In dit onderwerp ziet u hoe verbinding te maken en query Azure SQL-Database met behulp van Python. U kunt dit voorbeeld uitvoeren van Windows, Ubuntu Linux of Mac-platforms.


## <a name="step-1-create-a-sql-database"></a>Stap 1: Maak een SQL-database

Zie de [pagina aan de slag](sql-database-get-started.md) voor meer informatie over het maken van een voorbeelddatabase.  Het is belangrijk dat u de richtlijnen voor het maken van een **database AdventureWorks-sjabloon**volgen. De onderstaande voorbeelden werken alleen met de **AdventureWorks-schema**. Zodra u het maken van uw database of maken inschakelen u toegang tot uw IP-adres door het inschakelen van de firewall-regels, zoals beschreven in de [pagina aan de slag](sql-database-get-started.md)

## <a name="step-2-configure-development-environment"></a>Stap 2: Configureer ontwikkelomgeving

### <a name="mac-os"></a>**Mac OS**   
### <a name="install-the-required-modules"></a>De vereiste modules installeren
Open de terminal en installeren

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    brew install FreeTDS
    sudo -H pip install pymssql=2.1.1

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**

Open de terminal en Ga naar een map waarin u van plan bent uw python script maken. Voer de volgende opdrachten **FreeTDS** en **pymssql**installeren. pymssql maakt FreeTDS verbinding met SQL-Databases.

    sudo apt-get --assume-yes update
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    sudo apt-get --assume-yes install python-dev python-pip
    sudo pip install pymssql=2.1.1
    
### <a name="windows"></a>**Windows**

Pymssql installeren vanaf [**hier**](http://www.lfd.uci.edu/~gohlke/pythonlibs/#pymssql). 

Zorg ervoor dat u de juiste whl-bestand kiest. Bijvoorbeeld: als u op een 64-bits computer met Python 2.7 kiezen: pymssql‑2.1.1‑cp27‑none‑win_amd64.whl. Zodra u downloadt het bestand .whl dat in de map C:/Python27 plaats.

Nu het stuurprogramma pymssql pip vanaf opdrachtregel gebruiken. cd C:/Python27 en uitvoeren van de volgende
    
    pip install pymssql‑2.1.1‑cp27‑none‑win_amd64.whl

Instructies voor het inschakelen van de pip gebruiken vindt u [hier](http://stackoverflow.com/questions/4750806/how-to-install-pip-on-windows)

## <a name="step-3-run-sample-code"></a>Stap 3: Voorbeeldcode uitvoeren

Maak een bestand met de naam **sql_sample.py** en plak de volgende code erin. U kunt deze uitvoeren via de opdrachtregel:
    
    python sql_sample.py

### <a name="connect-to-your-sql-database"></a>Verbinding maken met de SQL-Database

De functie [pymssql.connect](http://pymssql.org/en/latest/ref/pymssql.html) wordt gebruikt voor verbinding met een SQL-Database.

    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')


### <a name="execute-an-sql-select-statement"></a>Een SQL SELECT-instructie uitvoeren

De functie [cursor.execute](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.execute) kan worden gebruikt voor het ophalen van een resultaatset van een query met een SQL-Database. Deze functie is in feite elke query accepteert en geeft als resultaat die een resultaatset iteratie kunnen worden met het gebruik van [cursor.fetchone()](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.fetchone).


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute('SELECT c.CustomerID, c.CompanyName,COUNT(soh.SalesOrderID) AS OrderCount FROM SalesLT.Customer AS c LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID GROUP BY c.CustomerID, c.CompanyName ORDER BY OrderCount DESC;')
    row = cursor.fetchone()
    while row:
        print str(row[0]) + " " + str(row[1]) + " " + str(row[2])   
        row = cursor.fetchone()


### <a name="insert-a-row-pass-parameters-and-retrieve-the-generated-primary-key"></a>Een rij invoegen, parameters doorgeven en de gegenereerde primaire sleutel ophalen

In de SQL-Database worden de eigenschap [identiteit](https://msdn.microsoft.com/library/ms186775.aspx) en het [SEQUENCE](https://msdn.microsoft.com/library/ff878058.aspx) -object gebruikt om [primaire-sleutel](https://msdn.microsoft.com/library/ms179610.aspx) waarden automatisch te genereren. 


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express', 'SQLEXPRESS', 0, 0, CURRENT_TIMESTAMP)")
    row = cursor.fetchone()
    while row:
        print "Inserted Product ID : " +str(row[0])
        row = cursor.fetchone()


### <a name="transactions"></a>Transacties


Dit codevoorbeeld laat zien hoe u transacties waarin u:

* Een transactie start
* Een rij met gegevens invoegen
* Rollback uw transactie ongedaan maken van het menu invoegen 

Plak de volgende code in sql_sample.py.
    
    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("BEGIN TRANSACTION")
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express New', 'SQLEXPRESS New', 0, 0, CURRENT_TIMESTAMP)")
    cnxn.rollback()

## <a name="next-steps"></a>Volgende stappen

* Bekijk het [overzicht van SQL-Database ontwikkeling](sql-database-develop-overview.md)
* Meer informatie over het [Microsoft Python-stuurprogramma voor SQL Server](https://msdn.microsoft.com/library/mt652092.aspx)
* Ga naar de [Python Developer Center](/develop/python/).

## <a name="additional-resources"></a>Aanvullende bronnen 

* [Ontwerppatronen voor meerdere huurder SaaS-toepassingen met Azure SQL-Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Ontdek alle [mogelijkheden van SQL-Database](https://azure.microsoft.com/services/sql-database/)
