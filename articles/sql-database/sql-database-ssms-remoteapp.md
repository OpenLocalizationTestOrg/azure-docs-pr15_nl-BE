<properties
    pageTitle="Verbinding maken met een SQL-Database met behulp van SQL Server Management Studio in Azure RemoteApp | Microsoft Azure"
    description="Deze zelfstudie gebruiken voor informatie over het gebruik van SQL Server Management Studio in Azure RemoteApp voor beveiliging en prestaties wanneer u verbinding maakt met SQL-Database"
    services="sql-database"
    documentationCenter=""
    authors="adhurwit"
    manager="jhubbard"/>

<tags
    ms.service="sql-database"
    ms.workload="data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="adhurwit"/>

# <a name="use-sql-server-management-studio-in-azure-remoteapp-to-connect-to-sql-database"></a>Verbinding maken met de SQL-Database via SQL Server Management Studio in Azure RemoteApp-

## <a name="introduction"></a>Inleiding  
In deze zelfstudie wordt beschreven hoe u SQL Server Management Studio (SSMS) in Azure RemoteApp gebruiken voor verbinding met een SQL-Database. Het helpt u bij het proces van het instellen van SQL Server Management Studio in Azure RemoteApp-voordelen wordt uitgelegd en beveiligingsfuncties die u kunt gebruiken in Azure Active Directory bevat.

**Geschatte tijd:** 45 minuten

## <a name="ssms-in-azure-remoteapp"></a>SSMS in Azure RemoteApp

Azure RemoteApp is een service RDS in Azure die toepassingen biedt. Voor meer informatie over het hier: [Wat is RemoteApp?](../remoteapp/remoteapp-whatis.md)

Uitgevoerd in Azure RemoteApp SSMS biedt u dezelfde mogelijkheden als SSMS lokaal uitgevoerd.

![Schermafbeelding van SSMS uitvoert in Azure RemoteApp][1]



## <a name="benefits"></a>Voordelen

Er zijn veel voordelen aan het gebruik van SSMS in Azure RemoteApp, met inbegrip van:

- Poort 1433 op Azure SQL Server geen extern (buiten Azure) worden blootgesteld.
- Hoeft niet te laten toevoegen en verwijderen van IP-adressen in de Azure SQL Server-firewall.
- Alle Azure RemoteApp-verbindingen optreden via HTTPS op poort 443 met behulp van Remote Desktop protocol gecodeerd
- Het is voor meerdere gebruikers en kan worden geschaald.
- Er is een prestatieverbetering van SSMS dat in hetzelfde gebied, als de SQL-Database.
- Gebruik van Azure RemoteApp met de Premium edition van Azure Active Directory waarvoor de gebruiker activiteitsrapporten kunt u controleren.
- Hier kunt u meerledige verificatie (MVR gesloten).
- SSMS overal wanneer u een van de ondersteunde clients RemoteApp-Azure iOS, Android, Mac, Windows Phone en Windows-PC met toegang.


## <a name="create-the-azure-remoteapp-collection"></a>Maak de Azure RemoteApp-verzameling

Hier volgen de stappen voor het maken van uw Azure RemoteApp-collectie met SSMS:


### <a name="1-create-a-new-windows-vm-from-image"></a>1. Maak een nieuwe Windows VM uit afbeelding
Met de "Windows Server Remote Desktop Session Host Windows Server 2012 R2" afbeelding uit de galerie kunt u uw nieuwe VM.


### <a name="2-install-ssms-from-sql-express"></a>2. Installeer SSMS vanuit SQL Express

Ga naar de nieuwe VM en Ga naar deze downloadpagina: [Microsoft® SQL Server® 2014 Express](https://www.microsoft.com/en-us/download/details.aspx?id=42299)

Er is een optie voor het downloaden van alleen SSMS. Na het downloaden, gaat u naar de installatiemap en voer Setup uit om de installatie van SSMS.

Ook moet u SQL Server 2014 Service Pack 1 installeren. U kunt het hier downloaden: [Microsoft SQL Server 2014 Service Pack 1 (SP1)](https://www.microsoft.com/en-us/download/details.aspx?id=46694)

Service Pack 1 voor SQL Server 2014 bevat essentiële functionaliteit voor het werken met Azure SQL-Database.


### <a name="3-run-validate-script-and-sysprep"></a>3. script valideren en Sysprep uitvoeren

Op het bureaublad van de VM is een PowerShell script valideren genoemd. Deze uitvoeren door te dubbelklikken. Deze zal controleren of de VM klaar om te worden gebruikt voor het hosten van externe toepassingen. Wanneer de controle is voltooid, wordt u gevraagd naar het uitvoeren van sysprep - kiezen uit te voeren.

Wanneer sysprep is voltooid, wordt deze afgesloten de VM.

Zie voor meer informatie over het maken van een installatiekopie Azure RemoteApp: [het maken van een installatiekopie RemoteApp-sjabloon in Azure](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)


### <a name="4-capture-image"></a>4. de installatiekopie vastleggen

Wanneer de VM is gestopt, vinden in de huidige portal en vastlegt.

Zie voor meer informatie over het vastleggen van een afbeelding, [een installatiekopie van een Windows Azure virtual machine gemaakt met het implementatiemodel klassiek](../virtual-machines/virtual-machines-windows-classic-capture-image.md)


### <a name="5-add-to-azure-remoteapp-template-images"></a>5. toevoegen aan afbeeldingen Azure RemoteApp-sjabloon

De Azure RemoteApp-sectie van de huidige portal, Ga naar het tabblad afbeeldingen van de sjabloon en klik op toevoegen. Selecteer 'Een afbeelding importeren in uw bibliotheek van virtuele Machines' in het pop- en kies vervolgens de afbeelding die u zojuist hebt gemaakt.



### <a name="6-create-cloud-collection"></a>6. Maak cloud-collectie

In de huidige portal maken een nieuwe collectie van Azure RemoteApp-wolk. Kies de Sjabloonafbeelding die u zojuist hebt geïmporteerd met SSMS is geïnstalleerd.

![Nieuwe collectie van cloud maken][2]


### <a name="7-publish-ssms"></a>7. SSMS publiceren

Het publiceren tabblad van uw nieuwe cloud-collectie Selecteer publiceren van de toepassing in het Menu Start en kies SSMS uit de lijst.

![App publiceren][5]

### <a name="8-add-users"></a>8. Voeg gebruikers toe

Klik op het tabblad toegang voor gebruikers kunt u de gebruikers die toegang hebben tot deze Azure RemoteApp-collectie alleen SSMS bevat.

![Gebruiker toevoegen][6]


### <a name="9-install-the-azure-remoteapp-client-application"></a>9. de Azure RemoteApp-clienttoepassing installeren

U kunt downloaden en installeren van een RemoteApp-Azure-client: Download [| Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)



## <a name="configure-azure-sql-server"></a>Azure SQL-Server configureren

De enige configuratie nodig is om ervoor te zorgen dat de Azure Services voor de firewall is ingeschakeld. Als u deze oplossing gebruikt, hebt u niet nodig toe te voegen IP-adressen om de firewall te openen. Het netwerkverkeer dat is toegestaan voor de SQL Server is afkomstig van andere Azure diensten.


![Azure toestaan][4]



## <a name="multi-factor-authentication-mfa"></a>Meerledige verificatie (MVR gesloten)

MVR gesloten kan specifiek voor deze toepassing worden ingeschakeld. Ga naar het tabblad toepassingen van Azure Active Directory. Vindt u een vermelding voor Microsoft Azure RemoteApp. Als u op die toepassing en vervolgens configureren, ziet u de pagina waar u de MVR gesloten voor deze toepassing kunt inschakelen.

![Inschakelen van de MVR gesloten][3]



## <a name="audit-user-activity-with-azure-active-directory-premium"></a>Gebruikersactiviteit met Azure Active Directory Premium controleren

Als u nog geen Azure AD Premium, hebt u inschakelen in het gedeelte van de licenties van de map. Met Premium is ingeschakeld, kunt u gebruikers toewijzen aan het niveau van de premie.

Wanneer u naar een gebruiker in uw Azure Active Directory gaat, kunt u vervolgens Ga naar het tabblad activiteit aanmeldingsgegevens voor RemoteApp-Azure zien.



## <a name="next-steps"></a>Volgende stappen

Na het voltooien van de bovenstaande stappen, zult u de Azure RemoteApp-client uitvoeren en log in met een toegewezen gebruiker. U krijgt SSMS als één van uw toepassingen en kunt u deze uitvoeren zoals u zou doen als deze zijn geïnstalleerd op uw computer met toegang tot Azure SQL Server.

Zie voor meer informatie over het maken van de verbinding met SQL-Database [verbinding maken met een SQL-Database met SQL Server Management Studio en een steekproef T-SQL-query uit te voeren](sql-database-connect-query-ssms.md).


Dat is alles voor nu. Veel plezier!



<!--Image references-->
[1]: ./media/sql-database-ssms-remoteapp/ssms.png
[2]: ./media/sql-database-ssms-remoteapp/newcloudcollection.png
[3]: ./media/sql-database-ssms-remoteapp/mfa.png
[4]: ./media/sql-database-ssms-remoteapp/allowazure.png
[5]: ./media/sql-database-ssms-remoteapp/publish.png
[6]: ./media/sql-database-ssms-remoteapp/user.png
