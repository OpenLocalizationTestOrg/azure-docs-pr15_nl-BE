## <a name="repeatability-during-copy"></a>Herhaalbaarheid tijdens kopiëren

Bij het kopiëren van gegevens naar Azure SQL/SQL Server van de andere gegevens worden opgeslagen, moet één herhaalbaarheid moet u rekening houden om te voorkomen dat ongewenste resultaten. 

Wanneer u gegevens kopieert naar Azure SQL/SQL Server-Database, wordt de kopie activiteit standaard APPEND van de gegevensset naar de tabel sink standaard. Bijvoorbeeld, als u gegevens uit een CSV-(door komma's gescheiden waarden) bestand gegevensbron die twee records met Azure SQL/SQL Server-Database, is dit de tabel ziet er als:
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   2           2015-05-01 00:00:00

Stel dat u fouten gevonden in het bronbestand en de hoeveelheid van de buis naar beneden van 2 tot en met 4 in het bronbestand wordt bijgewerkt. Als u het segment opnieuw voor die periode uitvoeren, zult u twee nieuwe records toegevoegd aan Azure SQL/SQL Server-Database. De hieronder wordt ervan uitgegaan dat geen van de kolommen in de tabel een primaire sleutel.
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   2           2015-05-01 00:00:00
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   4           2015-05-01 00:00:00

Om dit te voorkomen, moet u UPSERT semantiek opgeven door gebruik te maken van een van de onder 2 mechanismen die hieronder vermeld.

> [AZURE.NOTE] Een segment kan automatisch worden opnieuw uitgevoerd in Azure Data Factory aan de hand van het opgegeven beleid voor opnieuw proberen.

### <a name="mechanism-1"></a>Mechanisme 1

U kunt gebruikmaken van de eigenschap **sqlWriterCleanupScript** eerst opruimen om actie te voeren wanneer een segment wordt uitgevoerd. 

    "sink":  
    { 
      "type": "SqlSink", 
      "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
    }

Het script opruimen zou worden uitgevoerd tijdens het kopiëren van een bepaald segment waarvan u de gegevens wilt verwijderen uit de SQL-tabel overeenkomt met dat segment eerste. De activiteit wordt de gegevens vervolgens invoegen in de SQL-tabel. 

Als het segment is nu opnieuw uitvoeren en vervolgens dat de hoeveelheid wordt bijgewerkt vindt als gewenst.
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   4           2015-05-01 00:00:00

Stel dat de vlakke sluitring record wordt verwijderd uit de oorspronkelijke csv. Het segment opnieuw uit te voeren, zou het volgende resultaat opleveren: 
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    7   Down Tube   4           2015-05-01 00:00:00

Niets nieuwe moest worden gedaan. De activiteit kopiëren hebt uitgevoerd het script Schijfopruiming verwijdert u de bijbehorende gegevens voor dat segment. Deze invoer gelezen vanuit het csv (die vervolgens slechts 1 record) en in de tabel ingevoegd. 

### <a name="mechanism-2"></a>Mechanisme 2
> [AZURE.IMPORTANT] sliceIdentifierColumnName wordt niet ondersteund voor Azure SQL Data Warehouse op dit moment. 

Een ander mechanisme herhaalbaarheid te bereiken is door een speciale kolom (**sliceIdentifierColumnName**) in de tabel van het doel. Deze kolom kan worden gebruikt door Azure Data Factory om ervoor te zorgen dat de bron- en doeladressen gesynchroniseerd blijven. Deze benadering werkt wanneer de flexibiliteit bij het wijzigen of definiëren van het schema van de SQL-tabel bestemming. 

Deze kolom door Azure Data Factory zou worden gebruikt voor doeleinden van de herhaalbaarheid en in het proces Azure Data Factory wordt geen schemawijzigingen aanbrengen aan de tabel. Deze benadering kunt ook:

1.  Een kolom van het type binair (32) in het doel-SQL-tabel definiëren. Er mag geen beperkingen op deze kolom. Laten we de naam in deze kolom als 'ColumnForADFuseOnly' in dit voorbeeld.
2.  Gebruiken in de activiteit kopiëren als volgt:

        "sink":  
        { 
          "type": "SqlSink", 
          "sliceIdentifierColumnName": "ColumnForADFuseOnly"
        }

Azure Data Factory vult deze kolom aan de hand van de noodzaak te zorgen voor dat de bron- en doeladressen gesynchroniseerd blijven. De waarden in deze kolom moeten door de gebruiker niet buiten deze context worden gebruikt. 

Net als 1 mechanisme, kopie activiteit wordt automatisch opschonen eerst de gegevens voor het opgegeven segment van de SQL-tabel van de bestemming en voer vervolgens de kopie activiteit gewoonlijk om de gegevens invoegen van bron tot bestemming voor dat segment. 
