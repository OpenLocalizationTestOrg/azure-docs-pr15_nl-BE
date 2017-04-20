<properties
   pageTitle="Azure SQL met Azure RemoteApp | Microsoft Azure"
   description="Informatie over hoe SQL Azure met Azure RemoteApp gebruiken."
   services="remoteapp"
   documentationCenter=""
   authors="ericorman"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="sql-azure-with-azure-remoteapp"></a>Azure SQL met Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Vaak wanneer klanten kiezen voor het hosten van hun Windows-toepassingen in de cloud met Azure RemoteApp wilt ze ook hun gegevens zoals SQL-servers migreren naar de cloud voor de implementatie van een hele cloud. Hierdoor kunnen hele cloud gehost oplossing die toegankelijk is op elk gewenst moment door elk apparaat overal met Azure RemoteApp. Hieronder vindt u koppelingen en verwijzingen met richtlijnen om u te helpen met dit proces.  

## <a name="migrate-your-sql-data"></a>De SQL-gegevens migreren

Beginnen met [een SQL Server-database met Azure SQL-Database migreren](../sql-database/sql-database-cloud-migrate.md). 

## <a name="configure-azure-remoteapp"></a>Azure RemoteApp configureren
Uw Windows-toepassing in Azure RemoteApp-host. Hieronder vindt u een zeer hoog niveau stapsgewijs:

1.     Maak de [sjabloon RemoteApp Azure VM](remoteapp-imageoptions.md). 
2.     Installeer de vereiste toepassing op de VM.
3.     De toepassing configureren zodat deze verbinding met de SQL DB maakt en Bevestig dat het werkt.
4.     Sysprep en het afsluiten van de VM. Dit vastleggen als afbeelding voor gebruik met Azure. **Opmerking:** U moet ervoor zorgen dat de toepassing kunnen behouden de informatie over connectiviteit DB tot en met het sysprep-proces is. Als de toepassing kan niet worden overgenomen van de gegevens van de DB, is het raadzaam tot het verrichten van de leverancier van de toepassing om te controleren hoe kunnen we de verbindingsreeks opgeven.
5.     De aangepaste afbeelding importeren in uw Azure RemoteApp-bibliotheek selecteren van de juiste geografische locatie die de implementatie van SQL Azure zich bevindt. 
6.     Een RemoteApp-collectie in het datacenter hetzelfde als uw SQL Azure-implementatie met behulp van de bovenstaande template te implementeren en publiceren van de toepassing. RemoteApp in het datacenter hetzelfde als uw Azure SQL Azure implementeren helpt implementatie ervoor te zorgen de snelste snelheden en latentie te verlagen. 

## <a name="app-and-sql-configuration-considerations"></a>Overwegingen voor App en SQL-configuratie:
Er zijn een aantal punten rekening houden bij het gebruik van SQL Azure met RemoteApp:

Informatie over [het configureren van een firewall Azure SQL-database](../sql-database/sql-database-firewall-configure.md). Een uittreksel uit de lidstaten artikel 'in eerste instantie alle toegang tot uw server Azure SQL-Database wordt geblokkeerd door de firewall. U moet om te beginnen met uw server Azure SQL-Database, gaat u naar de klassieke Portal en geeft u een of meer server-niveau firewallregels die toegang tot uw server Azure SQL-Database. De firewall-regels gebruiken om op te geven welke IP-adresbereiken van het Internet zijn toegestaan en of Azure toepassingen verbinding mogen maken met uw server Azure SQL-Database."

Ook wanneer een computer verbinding probeert te maken met de databaseserver van het Internet, de firewall controleert het oorspronkelijke IP-adres van de aanvraag ten opzichte van de volledige set van het niveau van de server en (indien nodig) databaseniveau firewallregels. "Als het IP-adres van de aanvraag binnen een van de bereiken in het niveau van de server firewall-regels, de verbinding wordt verleend aan de Azure SQL-databaseserver." Daarom kunnen we het gebruik van IP-bereiken en niet alleen afzonderlijke IP-adressen.

Volg de stapsgewijze instructies in [hoe: firewall-instellingen configureren op de SQL-Database via de Portal Azure](../sql-database/sql-database-configure-firewall-settings.md) het IP-adresbereik opgeven. Geef het IP-adresbereik van het subnet dat is opgegeven voor de Azure RemoteApp-collectie als u firewallregels van SQL configureert. Hierdoor de ARA servers verbinding maken met de SQL-database, hoewel zij hebben dynamisch IP-adressen toegewezen zal.

## <a name="troubleshooting"></a>Het oplossen van problemen
Als de ervaring van het gebruik van een clienttoepassing die worden gehost in Azure RemoteApp die is verbonden met een SQL-database wanneer gehost op Azure of in ruimten wordt langzaam kan er een paar redenen waarom.  

- Netwerkvertraging van uw apparaat naar Azure is hoog. Verplaatsen naar de beste en snelste netwerkverbinding u kunt voor de beste prestaties. Gebruik [azurespeed.com](http://azurespeed.com/) als een algemeen hulpprogramma voor het testen van de latentie apparaten Azure data Center.  
- Stress wordt gehost in Azure RemoteApp-clienttoepassing. Een ander factuuradres schema zoals Premium facturering te selecteren, worden de prestaties. Een andere truc is om te controleren welke bronnen uw toepassing verbruikt: uitvoeren tijdens een actieve sessie een toetsencombinatie ctrl-alt-end die het scherm SAS starten, selecteert u Taakbeheer en brongebruik voor uw app observeren.
- SQL server is onder belasting of niet geoptimaliseerd. Voer de SQL-instructies voor het oplossen van problemen. 

