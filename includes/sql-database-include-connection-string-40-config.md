
<!--
includes/sql-database-include-connection-string-40-config.md

Latest Freshness check:  2015-09-04 , GeneMi.

## Connection string
-->


### <a name="example-config-file-for-connection-string-security"></a>Voorbeeld van de config-bestand voor de beveiliging van de verbinding string


Het is ongezond om de verbindingsreeks als letterlijke waarden in de C#-code. Het is beter om de verbindingsreeks in een configuratiebestand. Er kunt u de tekenreeks tijd hoeft te compileren.

Laten we uitgaan van de gecompileerde C#-programma met de naam **ConsoleApplication1.exe**, en dat deze .exe bevindt zich een **bin\debug\* * directory.

In dit voorbeeld worden de meeste onderdelen van de verbindingsreeks opgeslagen in een configuratiebestand met de naam precies **ConsoleApplication1.exe.config**. Dit configuratiebestand moet zich ook bevinden **bin\debug\**.

In het XML-bestand van de volgende configuratiebestand ziet u een verbindingsreeks met de naam **ConnectionString4NoUserIDNoPassword**. De C#-code voor deze tekenreeks wordt gezocht.

U moet de echte namen in de tijdelijke aanduidingen bewerken:

- {your_serverName_here}
- {your_databaseName_here}



        <?xml version="1.0" encoding="utf-8" ?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
            </startup>
        
            <connectionStrings>
                <clear />
                <add name="ConnectionString4NoUserIDNoPassword"
                providerName="System.Data.ProviderName"
        
                connectionString=
                "Server=tcp:{your_serverName_here}.database.windows.net,1433;
                Database={your_databaseName_here};
                Connection Timeout=30;
                Encrypt=True;
                TrustServerCertificate=False;" />
            </connectionStrings>
        </configuration>



We kozen voor deze afbeelding twee parameters weglaten:

- Gebruikers-ID = {your_userName_here};
- Wachtwoord = {your_password_here};


Kunt u deze opnemen, maar soms is het beter om het programma deze waarden ophalen uit toetsenbordinvoer van de gebruiker. In bepaalde gevallen.



<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
