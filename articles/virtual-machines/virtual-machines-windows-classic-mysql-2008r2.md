<properties
    pageTitle="Maak een VM met MySQL | Microsoft Azure"
    description="Maak een Azure virtuele machine met Windows Server 2012 R2 en de MySQL-database met behulp van het implementatiemodel klassiek."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="cynthn"/>


# <a name="install-mysql-on-a-virtual-machine-created-with-the-classic-deployment-model-running-windows-server-2012-r2"></a>MySQL installeren op een virtuele machine gemaakt met het klassieke implementatiemodel met Windows Server 2012 R2

[MySQL](http://www.mysql.com) is een SQL-database, een populaire open-source. In deze zelfstudie wordt beschreven hoe u wilt installeren en uitvoeren van de communautaire versie van MySQL 5.6.23 als een MySQL-Server op een virtuele machine met Windows Server 2012 R2. Raadpleeg voor instructies over het installeren van MySQL op Linux: [het installeren van MySQL op Azure](virtual-machines-linux-mysql-install.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="create-a-virtual-machine-running-windows-server-2012-r2"></a>Maak een virtuele machine met Windows Server 2012 R2

Als u nog niet een VM met Windows Server 2012 R2, kunt u deze [zelfstudie](virtual-machines-windows-classic-tutorial.md) voor het maken van de virtuele machine. 

## <a name="attach-a-data-disk"></a>Een schijf met gegevens koppelen

Nadat u de virtuele machine hebt gemaakt, kunt u desgewenst een aanvullende gegevensschijf koppelen. Dit wordt aanbevolen voor de werkbelasting van de productie en om te voorkomen dat met de beschikbare ruimte op de OS-station (C:), inclusief het besturingssysteem.

Zie [het koppelen van een schijf met gegevens naar een virtuele Windows-computer](virtual-machines-windows-classic-attach-disk.md) en volg de instructies voor het koppelen van een lege schijf. De host-cache-instelling ingesteld op **geen** of **alleen-lezen**.

## <a name="log-on-to-the-virtual-machine"></a>Meld u aan bij de virtuele machine

Vervolgens gaat u [naar de virtuele machine aanmelden](virtual-machines-windows-classic-connect-logon.md) zodat u MySQL installeren kunt.

##<a name="install-and-run-mysql-community-server-on-the-virtual-machine"></a>Installeren en uitvoeren van MySQL-Community-Server op de virtuele machine

Ga als volgt te werk om te installeren, configureren en uitvoeren van de communautaire versie van MySQL-Server:

> [AZURE.NOTE] Deze stappen zijn voor het 5.6.23.0 communautaire versie van MySQL en Windows Server 2012 R2. Uw ervaring kan afwijken voor verschillende versies van MySQL of Windows Server.

1.  Nadat u verbinding hebt gemaakt met de virtuele machine met behulp van extern bureaublad, klikt u op **Internet Explorer** van het startscherm.
2.  Klik op de knop **Opties** in de rechterbovenhoek klikken (het wielpictogram cogged) en klik vervolgens op **Internet-opties**. Klik op het tabblad **beveiliging** , klik op het pictogram **Vertrouwde websites** en klik vervolgens op de knop **websites** . Toevoegen van http://*. mysql.com aan de lijst met vertrouwde sites. Klik op * *sluiten**, en klik op * *OK**.
3.  In de adresbalk van Internet Explorer, typt u http://dev.mysql.com/downloads/mysql/.
4.  De MySQL-site gebruiken om te zoeken en downloaden van de nieuwste versie van de MySQL-Installer voor Windows. Download de versie die het volledige bestand instellen (bijvoorbeeld de mysql-installer-Gemeenschap-5.6.23.0.msi met een grootte van 282.4 MB) en sla het installatieprogramma heeft bij het kiezen van de installateur MySQL.
5.  Wanneer het downloaden van het installatieprogramma is voltooid, klikt u op **uitvoeren** om het installatieprogramma starten.
6.  Accepteer de gebruiksrechtovereenkomst en klik op **volgende**op de pagina **Gebruiksrechtovereenkomst** .
7.  Op de pagina **Type installatie te kiezen** , klikt u op het gewenste installatietype en klik op **volgende**. De volgende stappen wordt ervan uitgegaan dat de keuze van het type **alleen Server** setup.
8.  Klik op de pagina **installatie** **uitvoeren**. Wanneer de installatie is voltooid, klikt u op **volgende**.
9.  Klik op **volgende**op de pagina **Configuratie van Product** .
10. Geef de gewenste configuratie en de connectiviteit opties, waaronder de TCP-poort zo nodig op de pagina **Type en netwerken** . Selecteer **Geavanceerde opties tonen**en klik vervolgens op **volgende**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_TypeNetworking.png)

11. Geef een sterk wachtwoord voor MySQL root op de pagina **Accounts en rollen** . Meer MySQL-gebruikersaccounts toevoegen als nodig is en klik op **volgende**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_AccountsRoles_Filled.png)

12. Wijzigingen in de standaardinstellingen opgeven voor het uitvoeren van de MySQL-Server als een Windows-service zo nodig op de pagina **Windows-Service** en klik op **volgende**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_WindowsService.png)

13. Geef de gewenste wijzigingen in de opties voor logboekregistratie op op de pagina **Geavanceerde opties** en klik op **volgende**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_AdvOptions.png)

14. Klik op de pagina **Serverconfiguratie van toepassing** **uitvoeren**. Als de configuratiestappen voltooid is, klikt u op **Voltooien**.
15. Klik op **volgende**op de pagina **Configuratie van Product** .
16. Op de pagina **Installatie voltooid** , klikt u op **Logboek naar Klembord kopiëren** als u deze later te bekijken en klik op **Voltooien**.
17. Van het startscherm, typ **mysql**en klik vervolgens op **MySQL 5.6 opdrachtregel-Client**.
18. Voer het wachtwoord in de hoofdmap die u hebt opgegeven in stap 11 en u zult worden gepresenteerd met een prompt waarbij u opdrachten voor het configureren van MySQL kan uitgeven. Zie voor een overzicht van opdrachten en syntaxis, [MySQL referentiehandleidingen](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html).

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_CommandPrompt.png)

19. U kunt ook server configuratie standaardinstellingen, zoals de basis- en mappen en stations, met de vermeldingen in het bestand C:\Program Files (x86) \MySQL\MySQL Server 5.6\my-default.ini. Zie voor meer informatie [5.1.2 Server configuratie standaard](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html).

## <a name="configure-endpoints"></a>Eindpunten configureren

Als u wilt dat de MySQL-Server-service beschikbaar voor MySQL-client-computers op het Internet, moet u een eindpunt configureert voor de TCP-poort waarop de MySQL-Server-service luistert en het maken van een extra regel voor Windows Firewall. Dit is de TCP-poort 3306 tenzij u een andere poort hebt opgegeven op de pagina **Type en netwerken** (stap 10 van de vorige procedure).


> [AZURE.NOTE] De gevolgen van dit, moet u zorgvuldig overwegen omdat dit de MySQL-Server-service beschikbaar op alle computers op het Internet. Hier kunt u de reeks IP-adressen die zijn toegestaan voor gebruik van het eindpunt met een lijst met ACL (Access Control). Zie [How to Set Up eindpunten op een virtuele Machine](virtual-machines-windows-classic-setup-endpoints.md)voor meer informatie.


Een eindpunt voor de MySQL-Server-service configureren:

1.  In de klassieke Azure portal, klikt u op **virtuele Machines**, klik op de naam van de virtuele machine van MySQL en klik op **eindpunten**.
2.  In de opdrachtbalk en klik op **toevoegen**.
3.  Klik op de pijl-rechts op de pagina **toevoegen een eindpunt op een virtuele machine** .
4.  Als u de standaard poort TCP MySQL 3306 Klik op **MySQL** in **naam**en klik op het vinkje.
5.  Als u een andere TCP-poort gebruikt, typt u een unieke naam in het vak **naam**. Selecteert u **TCP** in protocol, typ het poortnummer in **Poort van openbare** en **Particuliere poort**en klikt u op het vinkje.

## <a name="add-a-windows-firewall-rule-to-allow-mysql-traffic"></a>Een Windows Firewall-regel voor MySQL verkeer toevoegen

Als u wilt toevoegen een Windows Firewall-regel die de MySQL gegevensverkeer van het Internet, moet u de volgende opdracht uitvoeren vanaf een opdrachtprompt van Windows PowerShell op de MySQL server virtuele machine.

    New-NetFirewallRule -DisplayName "MySQL56" -Direction Inbound –Protocol TCP –LocalPort 3306 -Action Allow -Profile Public


    
## <a name="test-your-remote-connection"></a>Test uw externe verbinding


Test uw externe verbinding met de MySQL-Server-service op de Azure virtuele machine wordt uitgevoerd, moet u eerst de DNS-naam overeenkomt met de cloud-service met de virtuele machine met MySQL Server bepalen.

1.  In de klassieke Azure portal, klikt u op **virtuele Machines**, klik op de naam van uw MySQL server virtuele machine en klik op **Dashboard**.
2.  Opmerking de waarde van de **DNS-naam** in de sectie **Snel bekijken** van het dashboard virtuele machine. Hier volgt een voorbeeld:

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_DNSName.png)

3.  Voer de volgende opdracht als een MySQL-gebruiker aanmelden vanaf een lokale computer met MySQL of MySQL-client.

        mysql -u <yourMysqlUsername> -p -h <yourDNSname>

    Gebruik bijvoorbeeld de volgende opdracht voor de MySQL gebruiker naam dbadmin3 en de testmysql.cloudapp.net DNS-naam voor de virtuele machine wordt.

        mysql -u dbadmin3 -p -h testmysql.cloudapp.net


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het uitvoeren van MySQL, de [MySQL documentatie](http://dev.mysql.com/doc/).
