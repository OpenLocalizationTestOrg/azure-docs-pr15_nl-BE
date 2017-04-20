<properties
   pageTitle="Ondersteuning van SSMS voor Azure AD MVR gesloten met de SQL-Database en SQL Data Warehouse | Microsoft Azure"
   description="Rekening gehouden met Multi verificatie gebruiken met SSMS voor SQL-Database en SQL datawarehouse."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="10/04/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="ssms-support-for-azure-ad-mfa-with-sql-database-and-sql-data-warehouse"></a>SSMS-ondersteuning voor Azure AD MVR gesloten met de SQL-Database en SQL Data Warehouse

Azure SQL-Database en Azure SQL Data Warehouse nu ondersteuning voor verbindingen van SQL Server Management Studio (SSMS) met *Universele verificatie van Active Directory*. Universele verificatie van Active Directory is een interactieve procesverloop die *Azure meerledige verificatie* (MVR gesloten) ondersteunt. Azure MVR gesloten helpt beschermen toegang tot gegevens en toepassingen voldoen aan eisen van de gebruiker voor een eenvoudig aanmelden proces. Sterke verificatie met tal van opties voor eenvoudige verificatie levert, telefoongesprek, tekstbericht, smartcards met pin of mobiele app-meldingen, zodat gebruikers kunnen kiezen hoe ze de voorkeur geven. Zie voor een beschrijving van een meerledige verificatie [Meerledige verificatie](../multi-factor-authentication/multi-factor-authentication.md).

SSMS biedt nu ondersteuning voor:

- Interactieve MVR gesloten met Azure Active Directory met de mogelijkheden voor de validatie van een pop-upvenster.
- Niet-interactieve Active Directory-wachtwoord en geïntegreerde verificatie van Active Directory methoden die kunnen worden gebruikt in verschillende toepassingen (ADO.NET, JDBC, ODBC, enz.). Deze twee methoden nooit leiden tot pop-updialoogvensters.

Als de gebruikersaccount is geconfigureerd voor de MVR gesloten is de werkstroom interactief verificatie vereist gebruikersinteractie via pop-updialoogvensters, het gebruik van smartcards, enz. Als de gebruikersaccount is geconfigureerd voor de MVR gesloten, moet de gebruiker universele Azure-verificatie om verbinding te selecteren. Als de gebruikersaccount geen MVR gesloten vereist, kan de gebruiker nog steeds de andere twee Azure Active Directory-verificatie-opties gebruiken.

## <a name="universal-authentication-limitations-for-sql-database-and-sql-data-warehouse"></a>Universele verificatie beperkingen voor SQL-Database en SQL Data Warehouse

- SSMS is het enige hulpmiddel voor MVR gesloten door middel van universele verificatie van Active Directory ingeschakeld.
- Slechts één Azure Active Directory-account kunt aanmelden voor een instantie van SSMS Universal-verificatie. Te melden als een andere Azure AD-account moet u een ander exemplaar van SSMS. (Deze beperking is beperkt tot universele verificatie van Active Directory, kunt u aanmelden bij verschillende servers met Active Directory-wachtwoordverificatie of geïntegreerde verificatie van Active Directory SQL Server-verificatie).
- SSMS biedt ondersteuning voor universele verificatie van Active Directory voor visualisatie in Object Explorer met Query Editor en Query opslaan.
- Ondersteuning voor universele verificatie DacFx noch de ontwerper van het Schema.
- MSA-accounts worden niet ondersteund voor universele verificatie van Active Directory.
- Universele verificatie van Active Directory wordt niet ondersteund in SSMS voor gebruikers die worden geïmporteerd in de huidige Active Directory uit andere Azure Active Directories. Deze gebruikers worden niet ondersteund omdat het vereist een huurder-ID voor het valideren van de rekeningen en er geen mechanisme is te bepalen dat.
- Er zijn geen extra software-vereisten voor verificatie van Active Directory Universal, behalve dat u een ondersteunde versie van SSMS moet gebruiken.

## <a name="configuration-steps"></a>Configuratiestappen

Meerledige verificatie implementeren, moet vier eenvoudige stappen.

1. **Een Azure Active Directory configureren** : Zie [identiteiten in ruimten met Azure Active Directory integreren](../active-directory/active-directory-aadconnect.md), [toevoegen van uw eigen domeinnaam naar Azure AD](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/) [nu Microsoft Azure federation met Windows Server Active Directory ondersteunt](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [beheert de directory Azure AD](https://msdn.microsoft.com/library/azure/hh967611.aspx)en [Azure AD met Windows PowerShell beheren](https://msdn.microsoft.com/library/azure/jj151815.aspx)voor meer informatie.

2. **MVR gesloten configureren** : Zie [Azure meerledige verificatie configureren](../multi-factor-authentication/multi-factor-authentication-whats-next.md)voor stapsgewijze instructies. 

3. **SQL-Database configureren of SQL Data Warehouse voor Azure AD verificatie** – Zie [verbinding maken met een SQL-Database of SQL gegevens magazijn met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md)voor stapsgewijze instructies.

4. **SSMS downloaden** : download de nieuwste SSMS op de clientcomputer (ten minste augustus 2016), van de [Download SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="connecting-by-using-universal-authentication-with-ssms"></a>Verbinding maken met behulp van universele verificatie met SSMS

De volgende stappen laten zien hoe verbinding met de SQL-Database of SQL Data Warehouse met behulp van de nieuwste SSMS.

1. Voor gebruik van universele-verificatie in het dialoogvenster **verbinding maken met Server** , selecteert u de **Universele verificatie van Active Directory**.
![1mfa universele verbinding][1]

2. Zoals gebruikelijk voor SQL-Database en SQL Data Warehouse moet u op **Opties** en de database opgeven in het dialoogvenster **Opties** . Klik op **verbinding maken**.
3. Wanneer het dialoogvenster **aanmelden bij uw account** wordt weergegeven, geven de account en het wachtwoord van uw identiteit Azure Active Directory.
![2mfa-aanmelden][2]

    > [AZURE.NOTE] Voor universele verificatie met een account die niet nodig MVR gesloten u op dit moment. Voor gebruikers die behoefte hebben MVR gesloten, gaat u verder met de volgende stappen uit.
 
4. Twee MVR gesloten setup dialoogvensters kunnen worden weergegeven. Dit één keer bewerking is afhankelijk van de instelling beheerder van de MVR gesloten en daarom is niet verplicht. Voor een domein van de MVR gesloten ingeschakeld in deze stap is het soms vooraf gedefinieerde (bijvoorbeeld het domein gebruikers gebruiken een smartcard en de pincode vereist).  
![3mfa setup][3]

5. De tweede mogelijke één keer in het dialoogvenster kunt u de details van uw verificatiemethode selecteren. De mogelijke opties worden geconfigureerd door de beheerder.
![4mfa-Controleer of-1][4]
 
6. Azure Active Directory stuurt de gegevens waarin u. Wanneer u de verificatiecode ontvangt, voert u deze in het vak **Voer de verificatiecode** en klik op **aanmelden**.
![5mfa-Controleer of 2][5]

Wanneer de controle is voltooid, verbindt SSMS normaal vermoeden geldige referenties en toegang tot de firewall.

##<a name="next-steps"></a>Volgende stappen  

Anderen toegang verlenen tot uw database: [-Database SQL-verificatie en autorisatie: toegang verlenen](sql-database-manage-logins.md)  
Zorg ervoor dat andere gebruikers verbinding kunnen maken via de firewall: [een Azure SQL Database server niveau firewallregel met behulp van de portal Azure configureren](sql-database-configure-firewall-settings.md)


[1]: ./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png
[2]: ./media/sql-database-ssms-mfa-auth/2mfa-sign-in.png
[3]: ./media/sql-database-ssms-mfa-auth/3mfa-setup.png
[4]: ./media/sql-database-ssms-mfa-auth/4mfa-verify-1.png
[5]: ./media/sql-database-ssms-mfa-auth/5mfa-verify-2.png

