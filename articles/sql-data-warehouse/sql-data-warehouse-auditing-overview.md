<properties
   pageTitle="Controle in Azure SQL datawarehouse | Microsoft Azure"
   description="Aan de slag met Azure SQL Data Warehouse controle"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016" 
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="auditing-in-azure-sql-data-warehouse"></a>Controle in Azure SQL datawarehouse

> [AZURE.SELECTOR]
- [Controle](sql-data-warehouse-auditing-overview.md)
- [Detectie van bedreiging](sql-data-warehouse-security-threat-detection.md)

Datawarehouse SQL-controle kunt u aan een record in de database naar een audit-gebeurtenislogboek in uw account Azure opslag. Controle, kunt u de naleving van de regelgeving te onderhouden, te begrijpen activiteit in een database en inzicht in de verschillen en afwijkingen die kunnen duiden op zakelijke problemen of vermoedelijke schendingen van de beveiliging. SQL Data Warehouse-controle ook geïntegreerd met Microsoft Power BI voor drill-down rapportage en analyse.

Extra controle inschakelen en vergemakkelijken de hechting aan de normen voldoen, maar niet garanderen dat. Zie voor meer informatie over Azure programma's die ondersteuning bieden voor naleving van standaarden <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Azure Vertrouwenscentrum</a>.

+ [Grondbeginselen van de database controleren]
+ [Controle voor uw database instellen]
+ [Controlelogboeken en rapporten analyseren]

##<a id="subheading-1"></a>Grondbeginselen van Azure SQL gegevens magazijn Database controleren


Controle van SQL Data Warehouse-database kunt u:

- **Bewaren** een controlepad van de geselecteerde gebeurtenissen. U kunt definiëren categorieën van databaseacties moeten worden gecontroleerd.
- **Rapport** over de activiteit in een database. U kunt vooraf geconfigureerde rapporten en een dashboard snel aan de slag met een activiteit en gebeurtenisrapportage.
- De rapporten **analyseren** . U kunt vinden op verdachte gebeurtenissen, ongebruikelijke activiteiten en trends.

U kunt configureren voor de volgende gebeurteniscategorieën controleren:

**Gewone SQL** - en **SQL met parameters** waarvoor de verzamelde audit registreert, worden geclassificeerd als  

- **Toegang tot gegevens**
- **Wijzigingen in het schema (DDL)**
- **Wijzigingen in gegevens (DML)**
- **Accounts, rollen en machtigingen (DCL)**
- **Opgeslagen Procedure**, **Login** en **beheer van transacties**.

Controle van **geslaagde** en **mislukte** bewerkingen worden voor elke gebeurteniscategorie afzonderlijk geconfigureerd.

Zie voor meer informatie over de activiteiten en gebeurtenissen die worden gecontroleerd, de <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">Indeling verwijzing naar controlelogboek (doc-bestand downloaden)</a>.

Controlelogboeken worden opgeslagen in uw account Azure opslag. U kunt de bewaartermijn van een audit-logboek definiëren.

Een controlebeleid kan worden gedefinieerd voor een specifieke database of als een standaardbeleid voor de server. Een controlebeleid instellen voor standaard-server wordt toegepast op alle databases op een server die niet over een specifieke database controle beleid gedefinieerd.

Voordat u controle omhoog controle selectievakje als u een [' Downlevel Client '](sql-data-warehouse-auditing-downlevel-clients.md).


##<a id="subheading-2"></a>Controle voor uw database instellen

1. Start de <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.

2. Ga naar het blad van de configuratie van de database SQL Data Warehouse / SQL Server die u wilt controleren. Klik op de knop **Instellingen** op de voorgrond en vervolgens in het blad van de instelling en selecteer **controlebeleid**.

    ![][1]

3. In de audit configuratie-blade eerst Hef de selectie van het selectievakje **Overgenomen controle-instellingen van de Server** . Hiermee kunt u de instellingen voor een bepaalde database opgeven.

    ![][2]

4. Vervolgens wordt de controle door te klikken **op** inschakelen.

    ![][3]

5. Selecteer in de audit configuratie-blade **STORAGE DETAILS** voor het openen van de bladeserver opslag van Audit Logs. Selecteer de Azure opslag rekening waar de logboekbestanden moeten worden opgeslagen en de verloopperiode is verstreken. **Tip:** De opslag rekening voor alle gecontroleerde databases gebruiken om optimaal van de vooraf geconfigureerde rapporten sjablonen.

    ![][4]

6. Klik op de knop **OK** om de configuratie van opslag gegevens opslaan.


7. Klik op om alle gebeurtenissen in logboek van **GESLAAGDE** en **MISLUKTE** onder **LOGBOEKREGISTRATIE door GEBEURTENIS**, of afzonderlijke gebeurteniscategorieën kiezen.


8. Als u controle voor een database configureert, moet u mogelijk de verbindingsreeks van de client om controle van de gegevens juist is vastgelegd te wijzigen. Raadpleeg het onderwerp [FULLY op de Server wijzigen in de verbindingsreeks](sql-data-warehouse-auditing-downlevel-clients.md) voor downlevel client-verbindingen.

9. Klik op **OK**.


##<a id="subheading-3">Controlelogboeken en rapporten analyseren</a>

Controlelogboeken worden samengevoegd in een verzameling tabellen winkel met het voorvoegsel **SQLDBAuditLogs** in de Azure opslag account dat u tijdens de installatie hebt gekozen. U kunt met behulp van een hulpprogramma zoals <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure Opslagverkenner</a>logboekbestanden weergeven.

Een rapportsjabloon vooraf geconfigureerde dashboard is beschikbaar als een <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">downloadbaar werkblad van Excel</a> kunt u gegevens snel analyseren. De sjabloon wilt gebruiken in de controlelogboeken, moet u Excel 2013 of later en Power Query die u kunt downloaden <a href="http://www.microsoft.com/download/details.aspx?id=39379">hier</a>.

De sjabloon bevat, fictieve voorbeeldgegevens en kunt u de Power Query instellen voor het importeren van het controlelogboek rechtstreeks vanuit uw account Azure opslag.

Voor gedetailleerde instructies voor het werken met de sjabloon lezen <a href="http://go.microsoft.com/fwlink/?LinkId=506731">How To (doc te downloaden)</a>.

![][5]


##<a id="subheading-4">Procedures voor het gebruik in productie</a>
Beschrijving van de in deze sectie wordt verwezen naar de bovenstaande schermvastleggingen. <a href="https://portal.azure.com" target="_blank">Azure Portal</a> of <a href= "https://manage.windowsazure.com/" target="_bank">Klassieke Azure klassieke Portal</a> kan worden gebruikt.


##<a id="subheading-5"></a>Opslag van sessiesleutels

In de productie bent u waarschijnlijk uw opslag sleutels periodiek vernieuwen. Wanneer de sleutels te vernieuwen moet u het beleid opnieuw opslaan. Het proces is als volgt:.


1. Schakelen de **Toegangstoets voor opslag** van *primaire* *secundaire* en **Opslaan**in de audit configuratie blade (Zie eerder in de reeks van de controle van de sectie).
![][4]
2. Ga naar de opslag configuratie blade en **regenereren** de *Primaire sleutel van Access*.

3. Ga terug naar de bladeserver controleren configuratie, gaan de **Toegangstoets voor opslag** van *secundaire* *primaire* en drukt u op **Opslaan**.

4. Ga terug naar de opslag UI en **genereren** de *Secundaire sneltoets* (als voorbereiding voor de volgende cyclus van sleutels vernieuwen.

##<a id="subheading-6"></a>Automatisering
Er zijn verschillende PowerShell-cmdlets die kunt u controle configureert in Azure SQL-Database. Voor toegang tot de controle-cmdlets moet PowerShell worden uitgevoerd in de modus Azure Resource Manager.

> [AZURE.NOTE] De module [Azure Resource Manager](https://msdn.microsoft.com/library/dn654592.aspx) is momenteel in de voorvertoning. Deze bieden dezelfde beheermogelijkheden als de Azure module mogelijk niet.

Wanneer u in de modus Azure Resource Manager bent, voert `Get-Command *AzureSql*` om de cmdlets beschikbaar.


<!--Anchors-->
[Grondbeginselen van de database controleren]: #subheading-1
[Controle voor uw database instellen]: #subheading-2
[Controlelogboeken en rapporten analyseren]: #subheading-3


<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png


<!--Link references-->
