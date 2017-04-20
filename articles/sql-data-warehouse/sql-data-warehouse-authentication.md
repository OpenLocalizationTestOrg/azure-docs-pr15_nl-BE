<properties
   pageTitle="Verificatie van Azure SQL datawarehouse | Microsoft Azure"
   description="Azure Active Directory (AAD) en SQL Server-verificatie met Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter=""
   authors="byham"
   manager="barbkess"
   editor=""
   tags=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/24/2016"
   ms.author="rickbyh;barbkess;sonyama"/>

# <a name="authentication-to-azure-sql-data-warehouse"></a>Datawarehouse Azure SQL-verificatie

> [AZURE.SELECTOR]
- [Beveiliging-overzicht](sql-data-warehouse-overview-manage-security.md)
- [Verificatie](sql-data-warehouse-authentication.md)
- [Codering (Portal)](sql-data-warehouse-encryption-tde.md)
- [Codering (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

Voor verbinding met SQL Data Warehouse, moet u de beveiligingsreferenties voor verificatiedoeleinden doorgeven. Bij het maken van een verbinding, worden bepaalde instellingen zijn geconfigureerd als onderdeel van de query-sessie tot stand te brengen.  

Zie [een SQL Data Warehouse-database beveiligen][]voor meer informatie over beveiliging en het inschakelen van verbindingen met uw datawarehouse.

## <a name="sql-authentication"></a>SQL-verificatie
Als u verbinding maakt met SQL Data Warehouse, moet u de volgende informatie opgeven:

- Volledige servernaam
- SQL-verificatie opgeven
- Gebruikersnaam
- Wachtwoord
- Standaarddatabase (optioneel)

Standaard wordt uw verbinding maken met de *master* -database en niet uw gebruikersdatabase. Als u wilt verbinden aan uw database, kunt u twee dingen doen:

- De standaarddatabase opgeven bij het registreren van uw server met SQL Server-Object Explorer in SSDT, SSMS, of in de verbindingsreeks van toepassing. Bijvoorbeeld de parameter InitialCatalog voor een ODBC-verbinding.
- Markeer de database voordat u een sessie maakt in SSDT.

> [AZURE.NOTE] De Transact-SQL-instructie **Gebruiken MijnDatabase;** wordt niet ondersteund voor het wijzigen van de database voor een verbinding. Raadpleeg het artikel [Query met Visual Studio][] als leidraad verbinding te maken met SQL Data Warehouse met SSDT.

## <a name="azure-active-directory-aad-authentication"></a>Azure verificatie van Active Directory (AAD)

[Azure Active Directory] [ What is Azure Active Directory] verificatie is een mechanisme dat door het gebruik van identiteiten in Azure Active Directory (AD Azure) verbinding te maken met Microsoft Azure SQL Data Warehouse. Azure Active Directory-verificatie, kunt u de identiteit van gebruikers en andere Microsoft-services op één centrale locatie centraal beheren. Centraal beheer van ID hier één SQL Data Warehouse-gebruikers beheren en vereenvoudigt het beheer van machtigingen. 

### <a name="benefits"></a>Voordelen

Azure voordelen Active Directory:

- Biedt een alternatief voor de SQL Server-verificatie.
- Houdt de verspreiding van de identiteit van gebruikers over databaseservers.
- Wachtwoord draaien op één plaats kunt
- Met behulp van externe (AAD) groepen machtigingen beheren.
- Elimineert wachtwoorden opslaan door geïntegreerde Windows-verificatie en andere vormen van verificatiemethoden die worden ondersteund door Azure Active Directory inschakelen.
- Gebruikers die wordt gebruikt voor de verificatie van identiteiten op databaseniveau.
- Verificatie op basis van het token ondersteunt voor toepassingen die verbinding maken met SQL-datawarehouse.
- Meerledige verificatie door middel van universele verificatie van Active Directory ondersteunt voor SQL Server Management Studio. Zie voor een beschrijving van een meerledige verificatie [SSMS ondersteuning voor Azure AD MVR gesloten met de SQL-Database en SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).

> [AZURE.NOTE] Azure Active Directory is nog relatief nieuw en kent een aantal beperkingen. Zie [AD Azure functies en beperkingen][], met name de aanvullende overwegingen zodat Azure Active Directory is een geschikt voor uw omgeving.

### <a name="configuration-steps"></a>Configuratiestappen

Volg deze stappen als Azure Active Directory-verificatie wilt configureren.

1. Maken en een Azure Active Directory te vullen
2. Optioneel: Koppelen of te wijzigen die is gekoppeld aan uw abonnement Azure active directory
3. Maak een Azure Active Directory-beheerder voor Azure SQL Data Warehouse.
4. De clientcomputers configureren
5. Gebruikers van de opgenomen in de database toegewezen aan AD Azure identiteiten maken
6. Verbinding maken met uw datawarehouse via Azure AD identiteiten

Op dit moment worden Azure Active Directory-gebruikers niet weergegeven in SSDT Object Explorer. Als tijdelijke oplossing de gebruikers in [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx)te bekijken.
  
### <a name="find-the-details"></a>De details
- De gedetailleerde stappen. De gedetailleerde stappen voor het configureren en Azure Active Directory-verificatie zijn vrijwel identiek voor Azure SQL-Database en Azure SQL Data Warehouse. De gedetailleerde stappen in het onderwerp [verbinding maken met een SQL-Database of SQL gegevens magazijn met behulp van Azure Active Directory-verificatie](../sql-database/sql-database-aad-authentication.md).
- Aangepaste database-rollen maken en gebruikers aan rollen toevoegen. Vervolgens gedetailleerde machtigingen verlenen voor de rollen. Zie [Aan de slag met de Database-Engine machtigingen](https://msdn.microsoft.com/library/mt667986.aspx)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Zie [Query met Visual Studio][]starten bij het controleren van uw gegevensmagazijn met Visual Studio en andere toepassingen.

<!-- Article references -->
[Een SQL Data Warehouse-database beveiligen]: ./sql-data-warehouse-overview-manage-security.md
[Query met Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]: ../active-directory/active-directory-whatis.md
[Azure AD functies en beperkingen]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations
