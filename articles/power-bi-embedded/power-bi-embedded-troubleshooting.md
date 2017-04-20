<properties
   pageTitle="Probleemoplossing Microsoft Power BI ingesloten voorbeeld"
   description="Probleemoplossing Microsoft Power BI ingesloten voorbeeld"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="microsoft-power-bi-embedded-preview-troubleshooting"></a>Probleemoplossing Microsoft Power BI ingesloten voorbeeld
Dit artikel bevat antwoorden voor het oplossen van **Power BI ingesloten**.

<a name="connection-string"/>
## <a name="setting-sql-server-connection-strings"></a>Instelling SQL Server-verbindingsreeksen
Instellen op een SQL-Server verbinding maken met de tekenreeks, moet u een specifieke syntaxis. Hieronder vindt u een voorbeeld van de verbindingsreeks voor de SQL Server.

```
"Persist Security Info=False;Integrated Security=true;Initial Catalog=Northwind;server=(local)"
```

Voor meer informatie over verbindingsreeksen SQL Server, raadpleegt u de volgende artikelen:

-   [SQL Server-verbindingsreeksen](https://msdn.microsoft.com/library/jj653752.aspx)
-   [SqlConnection.ConnectionString](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.connectionstring.aspx)

<a name="credentials"/>
## <a name="setting-credentials"></a>Referenties instellen
In het geval waar u referenties voor een ontwikkel- of testomgeving, zoals gebruikersnaam en wachtwoord hebt, moet u mogelijk referenties die overeenkomen met een productie-oplossing bijwerken.

## <a name="see-also"></a>Zie ook
- [Aan de slag met monster](power-bi-embedded-get-started-sample.md)
- [Wat is een Power BI ingesloten](power-bi-embedded-what-is-power-bi-embedded.md)
