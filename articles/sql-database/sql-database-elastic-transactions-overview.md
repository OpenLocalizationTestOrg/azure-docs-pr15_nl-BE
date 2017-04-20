<properties
   pageTitle="Gedistribueerde transacties via cloud-databases"
   description="Overzicht van de elastische databasetransacties met Azure SQL-Database"
   services="sql-database"
   documentationCenter=""
   authors="torsteng"
   manager="jhubbard"
   editor="torsteng"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="sql-database"
   ms.date="05/27/2016"
   ms.author="torsteng"/>

# <a name="distributed-transactions-across-cloud-databases"></a>Gedistribueerde transacties via cloud-databases

Elastische databasetransacties voor Azure SQL-Database (SQL DB) kunnen u transacties die zich uitstrekken over meerdere databases in SQL DB uitvoeren. Elastische databasetransacties voor SQL DB zijn beschikbaar voor .NET-toepassingen met behulp van ADO .NET en integreren met de bekende programmeren met behulp van de klassen [System.Transaction](https://msdn.microsoft.com/library/system.transactions.aspx) . Als u de bibliotheek, Zie [.NET Framework 4.6.1 (Web Installer)](https://www.microsoft.com/download/details.aspx?id=49981).

Op locatie, in zo'n scenario meestal vereist Microsoft Distributed Transaction Coordinator (MSDTC) wordt uitgevoerd. Aangezien MSDTC niet beschikbaar voor toepassing in Azure Platform as a Service is, is de mogelijkheid om gedistribueerde transacties te coördineren nu rechtstreeks geïntegreerd in SQL DB. Toepassingen kunnen verbinding maken met een SQL-Database aan gedistribueerde transacties starten en een van de databases wordt transparant coördineren van de gedistribueerde transactie, zoals in de volgende afbeelding. 

  ![Gedistribueerde transacties met Azure SQL-Database met behulp van elastische databasetransacties ][1]

## <a name="common-scenarios"></a>Gebruikelijke scenario 's

Elastische databasetransacties voor SQL DB inschakelen atomische wijzigingen aanbrengen in de gegevens die zijn opgeslagen in verschillende Databases van verschillende SQL-toepassingen. Het voorbeeld ligt de nadruk op de ontwikkeling van client-side ervaringen in C# en .NET. Een server-side-ervaring met T-SQL is gepland voor een later tijdstip.  
Elastische databasetransacties zijn bedoeld voor de volgende scenario's:

* Database met meerdere toepassingen in Azure: met dit scenario gegevens is verticaal gepartitioneerd in meerdere databases in SQL DB die verschillende soorten gegevens bevinden zich op verschillende databases. Sommige bewerkingen nodig wijzigingen in gegevens die in twee of meer databases wordt bewaard. De toepassing gebruikt elastische databasetransacties atomisch en coördineren van de wijzigingen in de databases.

* Een laptopgeheugen databasetoepassingen in Azure: met dit scenario de gegevenslaag gebruikt de [clientbibliotheek elastische Database](sql-database-elastic-database-client-library.md) of self sharding horizontaal partitioneren van gegevens over veel databases in SQL DB. Een opvallende use-case is de noodzaak voor atomische wijzigingen voor een toepassing met een laptopgeheugen meerdere huurder wanneer wijzigingen huurders omvatten. Beschouw bijvoorbeeld een overboeking van een huurder naar de andere, beide die zich op verschillende databases. Een tweede geval is toegesneden sharding aangepast aan die u nodig hebt voor een grote huurder die op zijn beurt meestal betekent dat sommige atomaire bewerkingen moet zich uitstrekt over meerdere databases die worden gebruikt voor de huurder hetzelfde. Een derde geval is atomic updates om te verwijzen naar gegevens die zijn gerepliceerd in databases. Atomic, transactionele, bewerkingen langs deze lijnen kunnen nu over meerdere databases met behulp van de voorvertoning worden gecoördineerd.
Elastische databasetransacties gebruiken atomiciteit van de transactie tussen databases doorvoeren in twee fasen. Het is een geschikt voor transacties die betrekking hebben op minder dan 100 databases tegelijk binnen een transactie. Deze beperkingen worden niet afgedwongen, maar een verwachten prestaties en succes tarieven voor elastische databasetransacties afnemen bij overschrijding van deze limieten.


## <a name="installation-and-migration"></a>Installatie en migratie

De mogelijkheden voor elastische database in SQL DB zijn opgenomen in updates voor .NET-bibliotheken System.Data.dll en System.Transactions.dll. De DLL's zorgen ervoor dat doorvoering wordt gebruikt, indien nodig om ervoor te zorgen atomisch. Om te beginnen met het ontwikkelen van toepassingen met behulp van elastische databasetransacties, [.NET Framework 4.6.1](https://www.microsoft.com/download/details.aspx?id=49981) of hoger te installeren. Wanneer u in een eerdere versie van .NET framework, transacties ter bevordering van een gedistribueerde transactie mislukt en er treedt een uitzondering.

Na de installatie kunt u de gedistribueerde transactie API's in System.Transactions met verbindingen naar SQL DB. Als u bestaande MSDTC-toepassingen met behulp van deze API's, gewoon uw bestaande toepassingen voor .NET 4.6 opnieuw na het installeren van de 4.6.1 Framework. Wanneer uw projecten .NET 4.6, ze automatisch de bijgewerkte dll-bestanden van de nieuwe versie van het Framework gebruiken en gedistribueerde transactie die API-in combinatie met verbindingen naar SQL DB aanroepen worden nu uitgevoerd.

Houd er rekening mee dat elastische databasetransacties hoeven niet installeren van MSDTC. In plaats daarvan worden elastische databasetransacties rechtstreeks beheerd door en binnen SQL DB. Dit vereenvoudigt scenario's cloud aanzienlijk aangezien een implementatie van MSDTC niet nodig is om het gebruik van gedistribueerde transacties met SQL DB. Punt 4 wordt nader toegelicht in elastische databasetransacties en vereist .NET framework met uw Azure cloud toepassingen implementeren.

## <a name="development-experience"></a>Ontwikkeling ervaring

### <a name="multi-database-applications"></a>Database met meerdere toepassingen

De volgende voorbeeldcode wordt de bekende programmeren met .NET System.Transactions. De klasse TransactionScope wordt een ambient transactie in .NET. ('Ambient transactie' is een die is opgeslagen in de huidige thread.) Alle verbindingen geopend in de TransactionScope deel uitmaken van de transactie. Als u verschillende databases deelnemen, wordt de transactie automatisch verhoogd naar een gedistribueerde transactie. Het resultaat van de transactie wordt beheerd door de scope voltooien om door te geven aan het doorvoeren van gegevens.

    using (var scope = new TransactionScope())
    {
        using (var conn1 = new SqlConnection(connStrDb1))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = new SqlConnection(connStrDb2))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T2 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }

### <a name="sharded-database-applications"></a>Een laptopgeheugen databasetoepassingen.
 
Elastische databasetransacties voor SQL DB bieden ook ondersteuning voor coördinatie van gedistribueerde transacties, waar u kunt de methode OpenConnectionForKey van de client elastische database bibliotheek open verbindingen voor een schaal van laag. Rekening houden met gevallen waarin u moet transactionele consistentie voor wijzigingen in verschillende waarden van verschillende sharding garanderen. Verbindingen met de host van de waarden van de verschillende sharding shards zijn brokered met OpenConnectionForKey. In het algemeen zijn, kunnen de verbindingen zijn aan verschillende shards, dat transactionele garanties te waarborgen een gedistribueerde transactie moet. In het volgende voorbeeld ziet u dat deze aanpak. Hierbij wordt ervan uitgegaan dat er een variabele met de naam shardmap wordt gebruikt voor een shard-kaart uit de bibliotheek elastische database client:

    using (var scope = new TransactionScope())
    {
        using (var conn1 = shardmap.OpenConnectionForKey(tenantId1, credentialsStr))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }
        
        using (var conn2 = shardmap.OpenConnectionForKey(tenantId2, credentialsStr))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T1 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }


## <a name="net-installation-for-azure-cloud-services"></a>Installatie van .NET voor Azure Cloud Services

Azure biedt verschillende aanbiedingen naar .NET-hosttoepassingen. Een vergelijking van de verschillende aanbiedingen is beschikbaar in de [App-Service Azure, Cloud-Services en vergelijking van virtuele Machines](../app-service-web/choose-web-site-cloud-service-vm.md). Als de Gast OS van het aanbod kleiner dan 4.6.1 voor elastische transacties vereist .NET is, moet u de Gast OS upgrade naar 4.6.1. 

Voor Azure App Services, worden de Gast OS upgrades momenteel niet ondersteund. Azure virtuele Machines, meldt u zich gewoon in de VM en voert u het installatieprogramma voor de laatste versie van .NET framework. Azure Cloud Services moet u omvat de installatie van een nieuwere versie van .NET in de opstarttaken van uw implementatie. De concepten en de stappen worden beschreven in [.NET installeren op een Cloud Service rol](../cloud-services/cloud-services-dotnet-install-dotnet.md).  

Houd er rekening mee dat de installer voor .NET 4.6.1 meer tijdelijke opslag tijdens de bootstrapping Azure cloud diensten dan de installer voor .NET 4.6 wellicht nodig. Om te zorgen voor een geslaagde installatie moet u verhogen tijdelijke opslag voor uw Azure cloud-service in het bestand ServiceDefinition.csdef in de sectie LocalResources en de omgevingsinstellingen van uw taak starten, zoals in het volgende voorbeeld:

    <LocalResources>
    ...
        <LocalStorage name="TEMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
        <LocalStorage name="TMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
        <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
            <Environment>
        ...
                <Variable name="TEMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TEMP']/@path" />
                </Variable>
                <Variable name="TMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TMP']/@path" />
                </Variable>
            </Environment>
        </Task>
    </Startup>
    
## <a name="transactions-across-multiple-servers"></a>Transacties over meerdere servers

Elastische databasetransacties worden ondersteund tussen verschillende logische servers in Azure SQL-Database. Meerdere logische server grenzen, moeten de deelnemende servers eerst worden ingevoerd in een relatie van onderlinge communicatie. Zodra de relatie communicatie tot stand is gebracht, kan een database in een van de twee servers elastische transacties met de andere server-databases kan deelnemen. Met transacties die meer dan twee logische servers zijn verspreid, moet een communicatie-relatie in plaats van een paar logische servers.

Gebruik de volgende PowerShell-cmdlets voor het beheren van relaties voor elastische databasetransacties communicatie tussen servers:

* **Nieuw AzureRmSqlServerCommunicationLink**: deze cmdlet gebruiken voor het maken van een nieuwe relatie van de communicatie tussen twee logische servers in Azure SQL DB. De relatie is symmetrisch dat beide servers transacties met de andere server kunnen starten.
* **Get-AzureRmSqlServerCommunicationLink**: deze cmdlet gebruikt om op te halen en de eigenschappen van bestaande communicatie-relaties.
* **Verwijderen AzureRmSqlServerCommunicationLink**: deze cmdlet gebruiken voor het verwijderen van een bestaande relatie met communicatie. 


## <a name="monitoring-transaction-status"></a>Transactiestatus controleren

Dynamische Management weergaven (DMVs) gebruiken in SQL DB monitor status en de voortgang van uw lopende elastische databasetransacties. Alle DMVs die betrekking hebben op transacties zijn van toepassing voor gedistribueerde transacties in SQL DB. U vindt hier de bijbehorende lijst met DMVs: [transactie gerelateerde Dynamic Management weergaven en functies (Transact-SQL)](https://msdn.microsoft.com/library/ms178621.aspx).
 
Deze DMVs zijn bijzonder nuttig:

* **sys.dm\_tran\_active\_transacties**: geeft een overzicht van actieve transacties en hun status. De kolom UOW (werkeenheid) herkent de onderliggende transacties die deel uitmaken van dezelfde gedistribueerde transactie. Alle transacties binnen hetzelfde gedistribueerde transactie bevatten UOW dezelfde waarde. Zie de [documentatie bij de DMV](https://msdn.microsoft.com/library/ms174302.aspx) voor meer informatie.
* **sys.dm\_tran\_database\_transacties**: bevat aanvullende informatie over transacties, zoals de plaatsing van de transactie in het logboek. Zie de [documentatie bij de DMV](https://msdn.microsoft.com/library/ms186957.aspx) voor meer informatie.
* **sys.dm\_tran\_sloten**: informatie over de vergrendelingen die op dit moment worden gehouden door lopende transacties. Zie de [documentatie bij de DMV](https://msdn.microsoft.com/library/ms190345.aspx) voor meer informatie.

## <a name="limitations"></a>Beperkingen 

Op dit moment gelden de volgende beperkingen voor elastische databasetransacties in SQL DB:

* Alleen transacties tussen databases in SQL DB worden ondersteund. Andere [X / Open XA](https://en.wikipedia.org/wiki/X/Open_XA) resource providers en databases die buiten SQL DB kunnen niet deelnemen aan databasetransacties elastische. Dit betekent dat elastische databasetransacties niet kunnen uitrekken in installaties van SQL Server en Azure SQL-Databases. Blijven MSDTC gebruiken voor gedistribueerde transacties in gebouwen. 
* Alleen client gecoördineerd transacties van een .NET-toepassing worden ondersteund. Serverondersteuning voor T-SQL, zoals BEGIN DISTRIBUTED TRANSACTION is gepland, maar nog niet beschikbaar. 
* Alleen databases op Azure SQL DB V12 worden ondersteund.
* Transacties in WCF-services worden niet ondersteund. U hebt bijvoorbeeld een WCF-methode die een transactie wordt uitgevoerd. Om het gesprek in het kader van een transactie mislukt als een [System.ServiceModel.ProtocolException](https://msdn.microsoft.com/library/system.servicemodel.protocolexception).

## <a name="additional-resources"></a>Aanvullende bronnen

Nog niet met de elastische database mogelijkheden voor uw toepassingen Azure? Bekijk onze [Documentatieschema](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/). Voor vragen neem bereiken ons op het [forum van de SQL-Database](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) en de functie-aanvragen, Voeg aan het [Feedbackforum voor SQL-Database](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-transactions-overview/distributed-transactions.png



