<properties
    pageTitle="Het admin-taken uit te voeren, bijvoorbeeld admin-wachtwoord opnieuw instellen | Microsoft Azure"
    description="Wordt beschreven hoe u algemene beheertaken uitvoert in een SQL-Database. Bijvoorbeeld: admin-wachtwoord opnieuw instellen, verlenen en toegang verwijderen."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    keywords="admin-wachtwoord opnieuw instellen"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="how-to-perform-common-administrative-tasks-such-as-resetting-admin-password-in-azure-sql-database"></a>Het uitvoeren van veelvoorkomende beheertaken, zoals het opnieuw instellen van wachtwoord admin in Azure SQL-Database
Gebruik dit onderwerp voor de snelle stappen te verlenen en toegang tot een Azure SQL-database verwijderen. Zie voor uitgebreide informatie:

- [Beheer van databases en Azure SQL-Database-aanmeldingen](sql-database-manage-logins.md)
- [De SQL-database beveiligen](sql-database-security.md)
- [Security Center voor SQL Server-Database-Engine en Azure SQL-Database](https://msdn.microsoft.com/library/bb510589)


[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="to-reset-admin-password-for-a-logical-server"></a>Voor een logische server admin-wachtwoord opnieuw instellen

- Klik op **SQL-Servers**in de [Azure Portal](https://portal.azure.com) de server selecteren uit de lijst en klik vervolgens op **Wachtwoord opnieuw instellen**.

## <a name="to-help-make-sure-only-authorized-ip-addresses-are-allowed-to-access-the-server"></a>Als u er zeker van dat alleen geautoriseerde IP adressen krijgen toegang tot de server
- Zie [hoe: firewall-instellingen configureren op de SQL-Database](sql-database-configure-firewall-settings.md).

## <a name="to-create-contained-database-users-in-the-user-database"></a>Opgenomen om databasegebruikers te maken in de database
- Gebruik de instructie [CREATE USER](https://msdn.microsoft.com/library/ms173463.aspx) en [Bevatte databasegebruikers - maken van uw Database draagbare](https://msdn.microsoft.com/library/ff929188.aspx)zien.

## <a name="to-authenticate-contained-database-users-by-using-your-azure-active-directory"></a>Opgenomen om databasegebruikers te verifiëren met behulp van uw Azure Active Directory
- Zie [verbinding maakt met een SQL-Database via Azure Active Directory-verificatie](sql-database-aad-authentication.md).

## <a name="to-create-additional-logins-for-high-privileged-users-in-the-virtual-master-database"></a>Extra aanmeldingen voor de gebruikers veel rechten in de hoofddatabase virtueel maken
- Gebruik de instructie [Aanmelding maken](https://msdn.microsoft.com/library/ms189751.aspx) en Zie de sectie aanmeldingen beheren van [databases beheren en Azure SQL-Database-aanmeldingen](sql-database-manage-logins.md) voor meer details.
