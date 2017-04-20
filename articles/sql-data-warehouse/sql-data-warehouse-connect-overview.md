<properties
   pageTitle="Verbinding maken met Azure SQL datawarehouse | Microsoft Azure"
   description="Het zoeken naar de server en de verbinding string voor uw naar Azure SQL Data Warehouse"
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
   ms.date="09/26/2016"
   ms.author="sonyama;barbkess"/>

# <a name="connect-to-azure-sql-data-warehouse"></a>Verbinding maken met Azure SQL datawarehouse

In dit artikel kunt u verbinding maken met SQL Data Warehouse voor de eerste keer.

## <a name="find-your-server-name"></a>De servernaam van uw zoeken

De eerste stap bij het maken van verbinding met SQL Data Warehouse is het zoeken naar de servernaam van uw te weten.  De naam van de server in het volgende voorbeeld is bijvoorbeeld sample.database.windows.net. De volledig gekwalificeerde naam zoeken:

1. Ga naar de [portal Azure][].
2. Klik op **SQL-databases** 
3. Klik op de database die u tot stand wilt brengen.
4. Zoek de naam van de volledige server.

    ![Volledige naam][1]

## <a name="supported-drivers-and-connection-strings"></a>Ondersteunde stuurprogramma's en verbindingsreeksen

Azure SQL Data Warehouse ondersteunt [ADO.NET][], [ODBC][], [PHP][]en [JDBC][]. Klik op een van de vorige stuurprogramma's te zoeken naar de meest recente versie en documentatie. Voor het automatisch genereren van de verbindingsreeks voor het stuurprogramma dat u gebruikt uit de Azure portal, kunt u klikken op de **tekenreeksen voor een databaseverbinding wordt weergegeven** in het bovenstaande voorbeeld.  Hier volgen ook enkele voorbeelden van wat een verbindingsreeks ziet er voor elk stuurprogramma.

> [AZURE.NOTE] U kunt de verbindingstime-out instellen op 300 seconden voor de aansluiting op het overleven van korte perioden niet beschikbaar.

### <a name="adonet-connection-string-example"></a>Voorbeeld van ADO.NET-verbinding

```C#
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

### <a name="odbc-connection-string-example"></a>Voorbeeld van ODBC-verbinding

```C#
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

### <a name="php-connection-string-example"></a>Voorbeeld van PHP verbinding

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

### <a name="jdbc-connection-string-example"></a>Voorbeeld van verbinding JDBC

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## <a name="connection-settings"></a>Verbindingsinstellingen

SQL Data Warehouse standaardiseert sommige instellingen tijdens de verbinding en het maken van objecten. Deze instellingen niet worden overschreven en deze omvatten:

| Database-instelling       | Waarde                        |
| :--------------------- | :--------------------------- |
| [ANSI_NULLS][]         | OP                           |
| [QUOTED_IDENTIFIERS][] | OP                           |
| [DATEFORMAT][]         | MDJ                          |
| [DATEFIRST][]          | 7                            |

## <a name="next-steps"></a>Volgende stappen

Zie verbinding maken en een query met Visual Studio, [Query met Visual Studio][]. Zie voor meer informatie over de verificatieopties, [verificatie voor Azure SQL Data Warehouse][].

<!--Articles-->
[Query met Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[Datawarehouse Azure SQL-verificatie]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[ADO.NET]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx
[ANSI_NULLS]: https://msdn.microsoft.com/library/ms188048.aspx
[QUOTED_IDENTIFIERS]: https://msdn.microsoft.com/library/ms174393.aspx
[DATEFORMAT]: https://msdn.microsoft.com/library/ms189491.aspx
[DATEFIRST]: https://msdn.microsoft.com/library/ms181598.aspx

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->
[1]: media/sql-data-warehouse-connect-overview/get-server-name.png


