<properties
   pageTitle="SQL Database beveiliging-overzicht"
   description="Informatie over Azure SQL-Database en SQL Server-beveiliging, met inbegrip van de verschillen tussen de wolk en SQL Server op-ruimten als het gaat om verificatie, machtiging, beveiliging van verbindingen, codering en conformiteit."
   services="sql-database"
   documentationCenter=""
   authors="tmullaney"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="06/09/2016"
   ms.author="thmullan;jackr"/>


# <a name="securing-your-sql-database"></a>De SQL-Database beveiligen

## <a name="overview"></a>Overzicht

Dit artikel helpt bij de basisbeginselen van de beveiliging van de gegevenslaag van een toepassing met behulp van Azure SQL-Database. In het bijzonder in dit artikel krijgt u aan de slag met de bronnen voor het beveiligen van gegevens, toegang beperken en toezicht op de activiteiten van een database gemaakt in de [SQL-Database zelfstudie aan de slag](sql-database-get-started.md). Zie voor een volledig overzicht van de beveiligingsfuncties beschikbaar op alle versies van SQL [Security Center voor SQL Server-Database-Engine en Azure SQL-Database](https://msdn.microsoft.com/library/bb510589). Meer informatie is ook beschikbaar in het [technisch rapport van beveiligings- en Azure SQL-Database](https://download.microsoft.com/download/A/C/3/AC305059-2B3F-4B08-9952-34CDCA8115A9/Security_and_Azure_SQL_Database_White_paper.pdf) (PDF).

## <a name="connection-security"></a>Beveiliging van de verbinding

Beveiliging van de verbinding verwijst naar hoe u beperken en beveiligde verbindingen met uw database met behulp van firewall-regels en versleuteling voor verbinding.

Firewallregels worden gebruikt door de server en de database te weigeren van verbindingspogingen van IP-adressen die niet expliciet whitelisted zijn. Als u wilt toestaan dat uw toepassing of het openbare IP-adres van de clientcomputer probeert verbinding te maken met een nieuwe database, moet u eerst een firewallregel voor een niveau van de server met de klassieke Portal Azure, PowerShell of REST API maken. Beste, moet u de IP-adresbereiken die via de firewall van uw server toegestaan voor zoveel mogelijk te beperken. Zie [Firewall voor Azure SQL-Database](https://msdn.microsoft.com/library/ee621782)voor meer informatie.

Alle verbindingen met Azure SQL-Database vereisen codering (SSL/TLS) op alle tijden terwijl gegevens "doorvoer" en naar de database wordt. In de verbindingsreeks van uw toepassing, moet u de parameters voor het coderen van de verbinding en *niet* te vertrouwen het servercertificaat (dit doet u als u de verbindingsreeks van de klassieke Portal Azure), anders is de verbinding niet controleert of de identiteit van de server en is wel gevoeliger voor 'man-in-the-middle'-aanvallen. Voor het stuurprogramma ADO.NET bijvoorbeeld deze parameters voor de verbindingsreeks zijn **coderen = True** en **TrustServerCertificate = False**. Voor meer informatie Zie [gecodeerde verbinding Azure SQL-Database en certificaatverificatie](https://msdn.microsoft.com/library/azure/ff394108#encryption).


## <a name="authentication"></a>Verificatie

Verificatie verwijst naar de manier waarop u uw identiteit bewijzen wanneer u verbinding maakt met de database. SQL-Database ondersteunt twee soorten verificatie:

 - **SQL-verificatie**maakt gebruik van een gebruikersnaam en wachtwoord
 - **Azure Active Directory-verificatie**door Azure Active Directory beheerde identiteiten worden gebruikt en wordt ondersteund voor domeinen beheerd en geïntegreerd

Wanneer u de logische server voor de database hebt gemaakt, moet u een 'server admin' aanmelding met een gebruikersnaam en wachtwoord opgegeven. Deze referenties gebruikt, kunt u verifiëren met de database op die server als database-eigenaar of 'dbo'. Als u Azure Active Directory-verificatie gebruiken wilt, moet u een andere server admin genoemd de "Azure AD admin," Wat is toegestaan Azure AD-gebruikers en groepen worden beheerd. Deze beheerder kan ook alle bewerkingen die door een normale-Serverbeheer kunt uitvoeren. Zie [verbinding maken met SQL Database met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md) voor een overzicht van het maken van een admin Azure AD zodat Azure Active Directory-verificatie.

Beste, moet uw toepassing een andere account gebruiken om te verifiëren--op deze manier kunt u de machtigingen die zijn toegekend aan de toepassing beperken en verminderen de risico's van schadelijke activiteiten als uw toepassingscode kwetsbaar voor een aanval van SQL-injecties is. De aanbevolen werkwijze is het maken van een [databasegebruiker opgenomen](https://msdn.microsoft.com/library/ff929188), waardoor de app om één database rechtstreeks te verifiëren. U kunt een ingesloten databasegebruiker die gebruikmaakt van SQL-verificatie door het uitvoeren van de volgende T-SQL-opdracht terwijl verbonden aan uw database als de server admin-aanmelding maken:

```
CREATE USER ApplicationUser WITH PASSWORD = 'strong_password'; -- SQL Authentication
```

Als u een beheerder AD Azure hebt gemaakt, kunt u een ingesloten databasegebruiker die Azure Active Directory-verificatie gebruikt bij het uitvoeren van de volgende T-SQL-opdracht terwijl verbonden aan uw database als de admin Azure AD:

```
CREATE USER [Azure_AD_principal_name | Azure_AD_group_display_name] FROM EXTERNAL PROVIDER; -- Azure Active Directory Authentication
```

In beide gevallen moet de verbindingsreeks van de toepassing de referenties van deze gebruiker in plaats van de server admin-aanmelding, verbinding maken met de database opgeven.

Zie voor meer informatie over het verifiëren van een SQL-Database [beheren van Databases en Azure SQL-Database-aanmeldingen](sql-database-manage-logins.md).


## <a name="authorization"></a>Autorisatie
Vergunning verwijst naar wat u binnen een Azure SQL-Database doen kunt, en dit wordt bepaald door de machtigingen en lidmaatschappen van uw gebruikersaccount rol. Beste, moet u gebruikers de minste bevoegdheden verlenen. Azure SQL-Database is dit een eenvoudig te beheren met T-SQL-rollen:

```
ALTER ROLE db_datareader ADD MEMBER ApplicationUser; -- allows ApplicationUser to read data
ALTER ROLE db_datawriter ADD MEMBER ApplicationUser; -- allows ApplicationUser to write data
```

De u verbinding met maakt server admin-account is lid van db_owner, die bevoegd is te ondernemen binnen de database. Sla deze account voor het implementeren van schema-upgrades en andere beheertaken uit te voeren. "ApplicationUser"-account met beperkte machtigingen vanuit de toepassing naar de database met de minste bevoegdheden die nodig zijn voor uw toepassing verbinding te gebruiken.

Er zijn manieren om verder te beperken wat een gebruiker kan doen met Azure SQL-Database:

* [Databaserollen](https://msdn.microsoft.com/library/ms189121) dan db_datareader en db_datawriter kunnen worden gebruikt voor het maken van krachtige toepassing gebruikersaccounts of minder krachtige beheer rekeningen.
* Gedetailleerde [machtigingen](https://msdn.microsoft.com/library/ms191291) kunt u bepalen welke bewerkingen kunt u doen in afzonderlijke kolommen, tabellen, weergaven, procedures en andere objecten in de database.
* [Imitatie](https://msdn.microsoft.com/library/vstudio/bb669087) en [ondertekenen van de module](https://msdn.microsoft.com/library/bb669102) kunnen worden gebruikt om machtigingen veilig tijdelijk verhogen.
* [Beveiliging](https://msdn.microsoft.com/library/dn765131) kan worden gebruikt beperken dat een gebruiker rijen kunt openen.
* [Maskeren van gegevens](sql-database-dynamic-data-masking-get-started.md) kan worden gebruikt om blootstelling van gevoelige gegevens te beperken.
* [Opgeslagen procedures](https://msdn.microsoft.com/library/ms190782) kunnen worden gebruikt om de acties beperken die kunnen worden genomen in de database.

Databases en logische servers beheren vanaf de Azure klassieke Portal of met behulp van de API Azure Resource Manager wordt beheerd door uw portal gebruikersaccount roltoewijzingen. Zie [op rollen gebaseerde toegangscontrole in Azure Portal](../active-directory./role-based-access-control-configure.md)voor meer informatie over dit onderwerp.


## <a name="encryption"></a>Codering

Uw gegevens beveiligen door de gegevens te coderen wanneer deze 'in rust is' Azure SQL-Database of opgeslagen in de databasebestanden en de back-ups, [Transparante gegevenscodering](http://go.microsoft.com/fwlink/?LinkId=526242). Voor het coderen van uw database als de eigenaar van een database verbinden en uitvoeren:

```
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

Voor andere manieren voor het coderen van de geheimen van uw gegevens, kunt u overwegen:

* [Cel-niveau van codering](https://msdn.microsoft.com/library/ms179331.aspx) voor het coderen van specifieke kolommen of zelfs cellen van gegevens met verschillende codeersleutels.
* Als u een hardwarebeveiligingsmodule of Centraal beheer van uw belangrijkste hiërarchie codering, kunt u overwegen [Azure sleutel kluis met SQL Server in een VM Azure](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx).
* [Altijd gecodeerd](https://msdn.microsoft.com/library/mt163865.aspx) (in het voorbeeld) wordt codering transparant voor toepassingen en kunnen clients vertrouwelijke gegevens te coderen in toepassingen zonder de coderingssleutels delen met SQL-Database.

## <a name="auditing"></a>Controle

Controle en het bijhouden van databasegebeurtenissen kunt u onderhouden naleving van regelgeving en verdachte activiteiten te identificeren. Controle van SQL-Database kunt dat u vastleggen van gebeurtenissen in uw database de controle uw Azure opslag account aanmelden. Controle van SQL-Database is ook geïntegreerd met Microsoft Power BI te vergemakkelijken, diepgaande verslagen en analyses. Zie [aan de slag met SQL-Database controleren](sql-database-auditing-get-started.md)voor meer informatie.

SQL Database dreiging detectie biedt een extra beveiligingslaag op controle. U kunt reageren op bedreigingen, zoals die door middel van waarschuwingen op afwijkende activiteiten plaatsvinden. Zie voor meer informatie, [aan de slag met SQL Database dreiging detectie](sql-database-threat-detection-get-started.md).  

## <a name="compliance"></a>Conformiteit

Naast de bovenstaande functies en functionaliteit waarmee uw toepassing voldoen aan de verschillende security conformiteitsvereisten Azure SQL-Database ook deelneemt aan regelmatige controles en tegen een aantal van de naleving van normen is goedgekeurd. Zie voor meer informatie, het [Vertrouwenscentrum van Microsoft Azure](https://azure.microsoft.com/support/trust-center/), u de meest recente lijst met [SQL-Database naleving certificeringen vindt](https://azure.microsoft.com/support/trust-center/services/).
