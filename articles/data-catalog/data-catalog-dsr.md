<properties
   pageTitle="Catalogus Azure gegevens ondersteunde gegevensbronnen | Microsoft Azure"
   description="Specificatie van de ondersteunde gegevensbronnen."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="jstrauss"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/15/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-supported-data-sources"></a>Catalogus Azure gegevens ondersteunde gegevensbronnen

Gebruikers van de catalogus met Azure gegevens kunnen publiceren metagegevens voor een openbare API, een klik-eenmaal registratie tool of door het handmatig invoeren van gegevens rechtstreeks naar de catalogus gegevens web-portal. In het volgende raster geeft een overzicht van alle bronnen die worden ondersteund door de catalogus vandaag en de publicatiemogelijkheden voor elk.  Ook zijn de hulpprogramma's voor externe gegevens die elke bron uit portal "openen in" ervaring kan starten. Het tweede raster in het artikel heeft meer technische specificatie van de eigenschappen van de verbinding gegevensbronnen.


## <a name="list-of-supported-data-sources"></a>Lijst met ondersteunde gegevensbronnen

<table>

    <tr>
       <td><b>Data Source Object</b></td>
       <td><b>API</b></td>
       <td><b>Handmatige invoer</b></td>
       <td><b>Registratiehulpprogramma</b></td>
       <td><b>Hulpprogramma's openen In</b></td>
       <td><b>Notities</b></td>
    </tr>

    <tr>
      <td>Directory-gegevensarchief Lake Azure</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure Lake gegevensarchief bestand</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Opslag van Azure Blob</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure opslag Directory</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tabel Azure opslag</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>
        <font size="2"></font>
      </td>
      <td>
        <font size="2"></font>
      </td>
    </tr>

    <tr>
      <td>HDFS Directory</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HDFS bestand</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Component-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Component weergeven</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>MySQL tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>MySQL weergeven</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Oracle-databasetabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Oracle-Database weergeven</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Andere (algemene actief)</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL-gegevenstabellen magazijn</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL-gegevensweergave magazijn</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services-dimensie</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server analyseservices KPI</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Maatregel van SQL Server Analysis Services</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Reporting Services-rapport</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Browser</font></td>
      <td><font size=2>Native modus servers. SharePoint-modus wordt niet ondersteund.</font></td>
    </tr>

    <tr>
      <td>SQL Server-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server-weergave</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tabel Teradata</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Teradata weergeven</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SAP Hana weergeven</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2>Berekening en analytische weergaven. Kenmerk weergaven worden niet ondersteund.</font></td>
    </tr>

    <tr>
      <td>Db2-tabel</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Db2-weergave</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Systeembestand</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>FTP-map</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>FTP-bestand</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HTTP-rapport</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HTTP-eindpunt</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HTTP-bestand</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>OData-entiteit instellen</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>OData-functie</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Postgresql-tabel</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Postgresql weergeven</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SAP Hana weergeven</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td> Televergaderingen, Object</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SharePoint-lijst </td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

</table>

Als u ondersteuning voor aanvullende bronnen nodig, ingediend verzoek om een functie met behulp van de [catalogus met Azure gegevens forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).


<br>
<br>
## <a name="data-source-reference-specification"></a>Verwijzing gegevensbronspecificatie
> [AZURE.NOTE] Kolom 'DSL structuur' in de volgende tabel alleen lijsten verbindingseigenschappen voor 'adres' eigenschap zak die worden gebruikt door de catalogus Azure-gegevens (dat wil zeggen "adres" eigenschap zak kan bevatten andere eigenschappen van de verbinding van de gegevensbron die Azure gegevenscatalogus blijft bestaan, maar wordt niet gebruikt.)
<table>
    <tr>
       <td><b>Brontype</b></td>
       <td><b>Activatype</b></td>
       <td><b>Objecttypen</b></td>
       <td><b>DSL-structuur<b></td>
    </tr>
    <tr>
      <td>Azure Lake gegevensarchief</td>
      <td>Container</td>
      <td>Meer gegevens</td>
      <td>
        <font size=2>Protocol: webhdfs <br>verificatie: {basic oauth} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Azure Lake gegevensarchief</td>
      <td>Tabel</td>
      <td>Map en bestand</td>
      <td>
        <font size=2>Protocol: webhdfs <br>verificatie: {basic oauth} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Azure opslag</td>
      <td>Container</td>
      <td>Container</td>
      <td>
        <font size=2>Protocol: azure-BLOB's <br>verificatie: {azure--sneltoets} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domein <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;account <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;container</font>
      </td>
    </tr>
    <tr>
      <td>Azure opslag</td>
      <td>Tabel</td>
      <td>BLOB, Directory</td>
      <td>
        <font size=2>Protocol: azure-BLOB's <br>verificatie: {azure--sneltoets} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domein <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;account <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;container <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;naam</font>
      </td>
    </tr>
    <tr>
      <td>Azure opslag</td>
      <td>Container</td>
      <td>Container</td>
      <td>
        <font size=2>Protocol: azure-tabellen <br>verificatie: {azure--sneltoets} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domein <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;account</font>
      </td>
    </tr>
    <tr>
      <td>Azure opslag</td>
      <td>Tabel</td>
      <td>Tabel</td>
      <td>
        <font size=2>Protocol: azure-tabellen <br>verificatie: {azure--sneltoets} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domein <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;account <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;naam</font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Container</td>
      <td>Virtueel Cluster</td>
      <td>
        <font size=2>Protocol: cosmos <br>verificatie: {windows basic} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Tabel</td>
      <td>Stream Stream is ingesteld, weergeven</td>
      <td>
        <font size=2>Protocol: cosmos <br>verificatie: {windows basic} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>DataZen</td>
      <td>Container</td>
      <td>Site</td>
      <td>
        <font size=2>Protocol: http <br>verificatie: {none, basic, windows, oauth} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>DataZen</td>
      <td>Rapport</td>
      <td>Rapport, Dashboard</td>
      <td>
        <font size=2>Protocol: http <br>verificatie: {none, basic, windows, oauth} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Db2</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        <font size=2>Protocol: db2 <br>verificatie: {windows basic} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database</font>
      </td>
    </tr>
    <tr>
      <td>Db2</td>
      <td>Tabel</td>
      <td>Tabel, weergave</td>
      <td>
        <font size=2>Protocol: db2 <br>verificatie: {windows basic} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schema</font>
      </td>
    </tr>
    <tr>
      <td>Bestandssysteem</td>
      <td>Tabel</td>
      <td>Bestand</td>
      <td>
        <font size=2>Protocol: bestand <br>verificatie: {none, basic, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;pad</font>
      </td>
    </tr>
    <tr>
      <td>FTP</td>
      <td>Tabel</td>
      <td>Map en bestand</td>
      <td>
        <font size=2>Protocol: ftp <br>verificatie: {none, basic, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Container</td>
      <td>Cluster</td>
      <td>
        <font size=2>Protocol: webhdfs <br>verificatie: {basic oauth} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Tabel</td>
      <td>Map en bestand</td>
      <td>
        <font size=2>Protocol: webhdfs <br>verificatie: {basic oauth} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Component</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        <font size=2>Protocol: component <br>verificatie: {hdinsight, basic, gebruikersnaam, none} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>Component</td>
      <td>Tabel</td>
      <td>Tabel, weergave</td>
      <td>
        <font size=2>Protocol: component <br>verificatie: {hdinsight, basic, gebruikersnaam, none} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>Http</td>
      <td>Container</td>
      <td>Site</td>
      <td>
        <font size=2>Protocol: http <br>verificatie: {none, basic, windows, oauth} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Http</td>
      <td>Rapport</td>
      <td>Rapport, Dashboard</td>
      <td>
        <font size=2>Protocol: http <br>verificatie: {none, basic, windows, oauth} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Http</td>
      <td>Tabel</td>
      <td>Eindpunt, bestand</td>
      <td>
        <font size=2>Protocol: http <br>verificatie: {none, basic, windows, oauth} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        <font size=2>Protocol: mysql <br>verificatie: {protocol, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Tabel</td>
      <td>Tabel, weergave</td>
      <td>
        <font size=2>Protocol: mysql <br>verificatie: {protocol, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Container</td>
      <td>Entiteit-Container</td>
      <td>
        <font size=2>Protocol: odata <br>verificatie: {none, basic, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Tabel</td>
      <td>Entiteit is ingesteld, functie</td>
      <td>
        <font size=2>Protocol: odata <br>verificatie: {none, basic, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;resource</font>
      </td>
    </tr>
    <tr>
      <td>Oracle-Database</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        <font size=2>Protocol: oracle <br>verificatie: {protocol, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database</font>
      </td>
    </tr>
    <tr>
      <td>Oracle-Database</td>
      <td>Tabel</td>
      <td>Tabel, weergave</td>
      <td>
        <font size=2>Protocol: oracle <br>verificatie: {protocol, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object</font>
      </td>
    </tr>
    <tr>
      <td>Postgresql</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        <font size=2>Protocol: postgresql <br>verificatie: {windows basic} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database</font>
      </td>
    </tr>
    <tr>
      <td>Postgresql</td>
      <td>Tabel</td>
      <td>Tabel, weergave</td>
      <td>
        <font size=2>Protocol: postgresql <br>verificatie: {windows basic} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Container</td>
      <td>Site</td>
      <td>
        <font size=2>Protocol: http <br>verificatie: {none, basic, windows, oauth} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Rapport</td>
      <td>Rapport, Dashboard</td>
      <td>
        <font size=2>Protocol: http <br>verificatie: {none, basic, windows, oauth} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Power Query</td>
      <td>Tabel</td>
      <td>Gegevens Mashup</td>
      <td>
        <font size=2>Protocol: power query <br>verificatie: {oauth} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Televergaderingen</td>
      <td>Tabel</td>
      <td>Object</td>
      <td>
        <font size=2>Protocol: televergaderingen com <br>verificatie: {windows basic} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;loginServer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;klasse <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Itemnaam</font>
      </td>
    </tr>
    <tr>
      <td>SAP Hana</td>
      <td>Container</td>
      <td>Server</td>
      <td>
        <font size=2>Protocol: sap hana sql <br>verificatie: {protocol, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</font>
      </td>
    </tr>
    <tr>
      <td>SAP Hana</td>
      <td>Tabel</td>
      <td>Weergave</td>
      <td>
        <font size=2>Protocol: sap hana sql <br>verificatie: {protocol, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object</font>
      </td>
    </tr>
    <tr>
      <td>SharePoint</td>
      <td>Tabel</td>
      <td>Lijst</td>
      <td>
        <font size=2>Protocol: sharepoint-lijst <br>verificatie: {windows basic} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>SQL datawarehouse</td>
      <td>Opdracht</td>
      <td>Opgeslagen Procedure</td>
      <td>
        <font size=2>Protocol: tds <br>verificatie: {protocol, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object</font>
      </td>
    </tr>
    <tr>
      <td>SQL datawarehouse</td>
      <td>TableValuedFunction</td>
      <td>Tabelwaardefunctie</td>
      <td>
        <font size=2>Protocol: tds <br>verificatie: {protocol, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object</font>
      </td>
    </tr>
    <tr>
      <td>SQL datawarehouse</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        <font size=2>Protocol: tds <br>verificatie: {protocol, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database</font>
      </td>
    </tr>
    <tr>
      <td>SQL datawarehouse</td>
      <td>Tabel</td>
      <td>Tabel, weergave</td>
      <td>
        <font size=2>Protocol: tds <br>verificatie: {protocol, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Opdracht</td>
      <td>Opgeslagen Procedure</td>
      <td>
        <font size=2>Protocol: tds <br>verificatie: {protocol, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>TableValuedFunction</td>
      <td>Tabelwaardefunctie</td>
      <td>
        <font size=2>Protocol: tds <br>verificatie: {protocol, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        <font size=2>Protocol: tds <br>verificatie: {protocol, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Tabel</td>
      <td>Tabel, weergave</td>
      <td>
        <font size=2>Protocol: tds <br>verificatie: {protocol, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server analyseservices multidimensionale</td>
      <td>Container</td>
      <td>Model</td>
      <td>
        <font size=2>Protocol: analysis services <br>verificatie: {windows, basic, anoniem, none} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server analyseservices multidimensionale</td>
      <td>KPI</td>
      <td>KPI</td>
      <td>
        <font size=2>Protocol: analysis services <br>verificatie: {windows, basic, anoniem, none} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {KPI}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server analyseservices multidimensionale</td>
      <td>Maatregel</td>
      <td>Maatregel</td>
      <td>
        <font size=2>Protocol: analysis services <br>verificatie: {windows, basic, anoniem, none} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {maatregel}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server analyseservices multidimensionale</td>
      <td>Tabel</td>
      <td>Dimensie</td>
      <td>
        <font size=2>Protocol: analysis services <br>verificatie: {windows, basic, anoniem, none} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {dimensie}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server analyseservices in tabelvorm</td>
      <td>Container</td>
      <td>Model</td>
      <td>
        <font size=2>Protocol: analysis services <br>verificatie: {windows, basic, anoniem, none} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server analyseservices in tabelvorm</td>
      <td>KPI</td>
      <td>KPI</td>
      <td>
        <font size=2>Protocol: analysis services <br>verificatie: {windows, basic, anoniem, none} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {KPI}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server analyseservices in tabelvorm</td>
      <td>Maatregel</td>
      <td>Maatregel</td>
      <td>
        <font size=2>Protocol: analysis services <br>verificatie: {windows, basic, anoniem, none} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {maatregel}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server analyseservices in tabelvorm</td>
      <td>Tabel</td>
      <td>Tabel</td>
      <td>
        <font size=2>Protocol: analysis services <br>verificatie: {windows, basic, anoniem, none} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {tabel}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services</td>
      <td>Container</td>
      <td>Server</td>
      <td>
        <font size=2>Protocol: reporting services <br>verificatie: {windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;versie: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services</td>
      <td>Rapport</td>
      <td>Rapport</td>
      <td>
        <font size=2>Protocol: reporting services <br>verificatie: {windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;pad <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;versie: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        <font size=2>Protocol: teradata <br>verificatie: {protocol, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Tabel</td>
      <td>Tabel, weergave</td>
      <td>
        <font size=2>Protocol: teradata <br>verificatie: {protocol, windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server-Master Data Services</td>
      <td>Container</td>
      <td>Model</td>
      <td>
        <font size="2">Protocol: mssql mds <br>verificatie: {windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Versie</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server-Master Data Services</td>
      <td>Tabel</td>
      <td>Entiteit</td>
      <td>
        <font size="2">Protocol: mssql mds <br>verificatie: {windows} <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Versie <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;entiteit</font>
      </td>
    </tr>
    <tr>
      <td>Andere (niet een van de bovenstaande)</td>
      <td>\*</td>
      <td>\*</td>
      <td>
        <font size=2>Protocol: Algemeen actief <br>adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;item-id hebt</font>
      </td>
    </tr>
</table>
