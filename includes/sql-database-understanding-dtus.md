De Database transactie-eenheid (DTU) is de eenheid in de SQL-Database die de relatieve kracht van databases op basis van een concrete maatregel voorstelt: de database-transactie. We hebben een reeks bewerkingen die kenmerkend zijn voor een online transaction processing (OLTP) aanvragen en vervolgens gemeten hoeveel transacties kunnen worden uitgevoerd per seconde onder volledig voorwaarden geladen (dat is de korte versie, kunt u de details van de categorieas in de [Benchmark-overzicht](../articles/sql-database/sql-database-benchmark-overview.md)lezen). 

Bijvoorbeeld, biedt een premie P11 database met 1750 DTUs 350 x DTU meer rekenkracht dan een eenvoudige database met 5 DTUs. 

![Inleiding tot SQL-Database: één database DTUs door laag en niveau.](./media/sql-database-understanding-dtus/single_db_dtus.png)

>[AZURE.NOTE] Als u van een bestaande SQL Server-database migreert, kunt u een hulpprogramma van derden, [De Azure SQL Database DTU Rekenmachine](http://dtucalculator.azurewebsites.net/), een schatting van het prestatieniveau van de en service niveaus die de database mogelijk in Azure SQL-Database.

### <a name="dtu-vs-edtu"></a>DTU versus eDTU

De DTU voor enkele databases zet direct naar de eDTU voor elastische databases. Een database in een elastische basisfuncties voor database-toepassingen biedt bijvoorbeeld maximaal 5 eDTUs. Dat is dezelfde prestaties als een enkele basisfuncties voor database. Het verschil is dat de database elastische in beslag alle eDTUs uit de groep neemt Won't totdat deze is. 

![Inleiding tot SQL-Database: elastische groepen door laag.](./media/sql-database-understanding-dtus/sqldb_elastic_pools.png)

Kunt u een eenvoudig voorbeeld. Een elastische basisfuncties voor database-toepassingen met 1000 DTUs nemen en 800 databases neerzetten. Prestaties van de database won't afnemen en in de tijd (5 DTU X 200 = 1000), u won't treffers in de capaciteit van de groep zolang slechts 200 van de 800 databases op elk gewenst moment worden gebruikt. In dit voorbeeld wordt voor de duidelijkheid vereenvoudigd. De echte wiskunde is iets gecompliceerder. De portal kan rekenen voor u en een aanbeveling op basis van historische database gebruik maakt. Zie [Overwegingen bij het prijs- prestatieverhouding voor een elastische database-toepassingen](../articles/sql-database/sql-database-elastic-pool-guidance.md) voor meer informatie over de werking van de aanbevelingen of doen de wiskunde zelf. 
