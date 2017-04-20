<properties
    pageTitle="Zelfstudie: Web app met een entiteit Framework en beveiliging met meerdere huurder-database"
    description="Informatie over het ontwikkelen van een ASP.NET MVC 5 web app met een multi huurder SQL Database backent, met behulp van entiteit Framework en beveiliging."
  metaKeywords="azure asp.net mvc entity framework multi tenant row level security rls sql database"
    services="app-service\web"
    documentationCenter=".net"
    manager="jeffreyg"
  authors="tmullaney"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="04/25/2016"
    ms.author="thmullan"/>

# <a name="tutorial-web-app-with-a-multi-tenant-database-using-entity-framework-and-row-level-security"></a>Zelfstudie: Web app met een entiteit Framework en beveiliging met meerdere huurder-database

Deze zelfstudie laat zien hoe een huurder met meerdere web app met een "[gedeelde database, gedeelde schema](https://msdn.microsoft.com/library/aa479086.aspx)" pacht model, entiteit Framework en [Beveiliging op](https://msdn.microsoft.com/library/dn765131.aspx)te bouwen. In dit model wordt één database gegevens bevat voor veel huurders en elke rij in elke tabel is gekoppeld aan een "huurder-id '. Rij-niveau beveiliging (RLS), een nieuwe functie voor Azure SQL-Database wordt gebruikt om te voorkomen dat toegang tot gegevens van andere huurders. Dit vereist slechts een enkele, kleine wijziging aan de toepassing. Door het centraliseren van de logica van de huurder toegang vanuit de database zelf, RLS vereenvoudigt de toepassingscode en vermindert het risico van lekkage tussen huurders gegevens per ongeluk.

Laten we beginnen met de eenvoudige toepassing Contact Manager vanuit [een ASP.NET MVP app met verificatie en SQL DB maken en implementeren met Azure App-Service](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Rechts, de toepassing kan nu alle gebruikers (huurders) voor een overzicht van alle contactpersonen:

![Contact Manager toepassing voordat RLS inschakelen](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-Before.png)

Met een paar kleine wijzigingen, zullen we ondersteuning toevoegen voor meerdere pacht, zodat gebruikers kunnen alleen de contactpersonen die deel uitmaken van deze.

## <a name="step-1-add-an-interceptor-class-in-the-application-to-set-the-sessioncontext"></a>Stap 1: Een klasse Interceptor toevoegen aan de toepassing de SESSION_CONTEXT instellen

Er is een toepassing wijzigen die we hoeven te maken. Omdat alle toepassingsgebruikers verbinding kan maken met de database met dezelfde verbindingsreeks (d.w.z. dezelfde SQL-aanmelding), is er momenteel geen manier om een beleid van RLS te weten welke gebruiker moet filteren. Deze aanpak wordt in webtoepassingen omdat u hiermee efficiënt groepsgewijze verbindingen kunnen, maar dan moeten we een andere manier voor het identificeren van de huidige toepassingsgebruiker in de database. De oplossing is de toepassing die een sleutel / waarde-paar ingesteld voor de huidige gebruikers-id in de [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) direct na het openen van een verbinding, voordat de query wordt uitgevoerd. SESSION_CONTEXT is een sessiebereik sleutel / waarde-winkel en ons beleid voor RLS kunt de gebruikers-id opgeslagen in het identificeren van de huidige gebruiker.

We een [interceptor](https://msdn.microsoft.com/data/dn469464.aspx) (met name een [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), voegt een nieuwe functie in dienst Framework (EF) 6, de huidige gebruikersnaam automatisch ingesteld in de SESSION_CONTEXT door het uitvoeren van een T-SQL-instructie als EF een verbinding wordt geopend.

1.  Open het ContactManager-project in Visual Studio.
2.  Klik met de rechtermuisknop op de map modellen in de Solution Explorer en kies toevoegen > klasse.
3.  Naam van de nieuwe klasse 'SessionContextInterceptor.cs' en klik op toevoegen.
4.  De inhoud van de SessionContextInterceptor.cs vervangen door de volgende code.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure.Interception;
using Microsoft.AspNet.Identity;

namespace ContactManager.Models
{
    public class SessionContextInterceptor : IDbConnectionInterceptor
    {
        public void Opened(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
            // Set SESSION_CONTEXT to current UserId whenever EF opens a connection
            try
            {
                var userId = System.Web.HttpContext.Current.User.Identity.GetUserId();
                if (userId != null)
                {
                    DbCommand cmd = connection.CreateCommand();
                    cmd.CommandText = "EXEC sp_set_session_context @key=N'UserId', @value=@UserId";
                    DbParameter param = cmd.CreateParameter();
                    param.ParameterName = "@UserId";
                    param.Value = userId;
                    cmd.Parameters.Add(param);
                    cmd.ExecuteNonQuery();
                }
            }
            catch (System.NullReferenceException)
            {
                // If no user is logged in, leave SESSION_CONTEXT null (all rows will be filtered)
            }
        }
        
        public void Opening(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void BeganTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void BeginningTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void Closed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Closing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void ConnectionStringGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSet(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSetting(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionTimeoutGetting(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void ConnectionTimeoutGot(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void DataSourceGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DataSourceGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void Disposed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Disposing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void EnlistedTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void EnlistingTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void ServerVersionGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ServerVersionGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void StateGetting(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }

        public void StateGot(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }
    }

    public class SessionContextConfiguration : DbConfiguration
    {
        public SessionContextConfiguration()
        {
            AddInterceptor(new SessionContextInterceptor());
        }
    }
}
```

Dat is de enige toepassing wijziging vereist. Ga verder en samenstellen en publiceren van de toepassing.

## <a name="step-2-add-a-userid-column-to-the-database-schema"></a>Stap 2: Een gebruikers-id-kolom toevoegen aan het schema van de database

Vervolgens moet een gebruikers-id-kolom toevoegen aan de tabel contactpersonen in elke rij koppelen aan een gebruiker (huurder). We zullen het schema rechtstreeks in de database, wijzigen en hoeft niet aan dit veld opnemen in onze EF-gegevensmodel.

Verbinding maken met de database rechtstreeks met behulp van SQL Server Management Studio of Visual Studio en vervolgens de volgende T-SQL uitvoeren:

```
ALTER TABLE Contacts ADD UserId nvarchar(128)
    DEFAULT CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
```

Hiermee voegt u een kolom gebruikers-id toe aan de tabel Contactpersonen. We gebruiken het gegevenstype nvarchar(128) overeenkomt met de gebruikersnamen opgeslagen in de tabel AspNetUsers en maken we een standaardbeperking die de gebruikers-id van nieuw ingevoegde rijen aan de gebruikers-id die is opgeslagen in de SESSION_CONTEXT worden automatisch ingesteld.

Nu in de tabel ziet er als volgt:

![Tabel SSMS contactpersonen](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-Contacts.png)

Wanneer u nieuwe contactpersonen worden gemaakt, worden ze automatisch toegewezen de juiste gebruikers-id. Voor demodoeleinden echter we toewijzen een paar van deze bestaande contactpersonen aan een bestaande gebruiker.

Als u een paar gebruikers in de toepassing al hebt gemaakt (bijvoorbeeld met behulp van lokale, Google of Facebook accounts), ziet u deze in de tabel AspNetUsers. In het screenshot hieronder is er slechts één gebruiker tot nu toe.

![SSMS-AspNetUsers-tabel](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-AspNetUsers.png)

Kopieer de Id voor de user1@contoso.com, en plak deze in de onderstaande T-SQL-instructie. Deze instructie om de drie contactpersonen koppelen aan deze gebruikers-id worden uitgevoerd.

```
UPDATE Contacts SET UserId = '19bc9b0d-28dd-4510-bd5e-d6b6d445f511'
WHERE ContactId IN (1, 2, 5)
```

## <a name="step-3-create-a-row-level-security-policy-in-the-database"></a>Stap 3: Maak een beleid voor beveiliging in de database

De laatste stap is voor het maken van een beveiligingsbeleid dat de gebruikers-id in de SESSION_CONTEXT wordt automatisch filteren van de resultaten van query's.

Terwijl u nog steeds verbonden met de database, voeren de volgende T-SQL:

```
CREATE SCHEMA Security
go

CREATE FUNCTION Security.userAccessPredicate(@UserId nvarchar(128))
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS accessResult
    WHERE @UserId = CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
go

CREATE SECURITY POLICY Security.userSecurityPolicy
    ADD FILTER PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts,
    ADD BLOCK PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts
go

```

In deze code gebeuren drie dingen. Eerst wordt een nieuw schema gemaakt als een best practice voor centraliseren en het beperken van toegang tot de objecten van RLS. Vervolgens wordt een predikaat functie die wordt '1' geretourneerd als de gebruiker van een rij-id overeenkomt met de gebruikersnaam in de SESSION_CONTEXT gemaakt. Ten slotte wordt een beveiligingsbeleid dat deze functie toegevoegd als een filter en een blok predikaat in de tabel Contactpersonen gemaakt. Het predikaat filter zorgt ervoor dat query's worden alleen de rijen die deel uitmaken van de huidige gebruiker retourneren en het predikaat blok fungeert als veiligheidsmaatregel om te voorkomen dat de toepassing van het invoegen van een rij voor de verkeerde gebruiker ooit per ongeluk.

Nu de toepassing start en meld je aan als user1@contoso.com. Deze gebruiker ziet nu alleen de contactpersonen die we eerder aan deze gebruikers-id toegewezen:

![Contact Manager toepassing voordat RLS inschakelen](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-After.png)

Om dit verder valideren, kunt u een nieuwe gebruiker registreren. Zien ze geen contactpersonen, omdat er geen aan hen zijn toegewezen. Als ze een nieuwe contactpersoon maakt, krijgt deze en alleen moeten kunnen zien.

## <a name="next-steps"></a>Volgende stappen

Dat is alles. De eenvoudige Contact Manager web app is omgezet in een multi-huurder een waar elke gebruiker een eigen lijst met contactpersonen heeft. Met behulp van de beveiliging hebben we de complexiteit van de huurder toegang logica in onze toepassingscode af te dwingen vermeden. Deze transparantie kan de toepassing gericht op het zakelijke probleem bij de hand en het vermindert ook het risico dat gegevens per ongeluk lekt als de toepassing de codebase groeit.

Deze zelfstudie is het oppervlak van de mogelijkheden met RLS horen. Bijvoorbeeld, is het mogelijk te hebben meer geavanceerde of gedetailleerde access logica en de meer dan alleen de huidige gebruikers-id opslaan in de SESSION_CONTEXT. Het is ook mogelijk te [integreren RLS met de clientbibliotheken elastische database tools](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) ter ondersteuning van meerdere huurder shards in een gegevenslaag schalen.

Naast deze mogelijkheden werkt we ook zelfs beter om RLS. Als u vragen hebt, ideeën of zaken die u zien willen zou, laat het ons weten in de opmerkingen. Wij stellen uw feedback op prijs!
