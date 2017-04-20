<properties
    pageTitle="Implementeren van de MySQL bron provider op Azure Stack | Microsoft Azure"
    description="Gedetailleerde stappen voor de implementatie van de MySQL bron Provider naar Azure Stack."
    services="azure-stack"
    documentationCenter=""
    authors="Dumagar"
    manager="byronr"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="dumagar"/>


# <a name="deploy-the-mysql-resource-provider-on-azure-stack-to-use-with-webapps"></a>De Provider van de MySQL bron op Azure Stack met WebApps implementeren

> [AZURE.NOTE] De volgende informatie geldt alleen voor Azure Stack TP1-implementaties.

In dit artikel te volgen van de gedetailleerde stappen voor het instellen van de MySQL bron Provider op het conformiteitsbewijs Azure Stack haalbaarheidstest zodat u [met behulp van MySQL-databases](azure-stack-mysql-rp-deploy-short.md) op Azure Stack starten kunt, inclusief het gebruik van MySQL als de backend voor WordPress sites met [Azure Web Apps gemaakt](azure-stack-webapps-deploy.md)te gebruiken.

## <a name="set-up-steps-before-you-deploy"></a>Stappen instellen voordat u implementeren

Voordat u de resource provider implementeert, moet u:

- Een standaard Windows Server-installatiekopie met .NET 3.5
- Verbeterde beveiliging van Internet Explorer (IE) uitschakelen
- Installeer de nieuwste versie van Azure PowerShell

### <a name="create-an-image-of-windows-server-including-net-35"></a>Maak een installatiekopie van Windows Server, met inbegrip van .NET 3.5

Als u de bits Azure Stack na 23-2/2016 gedownload omdat de standaard Windows Server 2012 R2 basisinstallatiekopie .NET 3.5 framework in deze download en hoger bevat, kunt u deze stap overslaan.

Als u vóór 23-2/2016 hebt gedownload, moet u een Windows Server 2012 R2 Datacenter VHD maken met .NET 3.5 afbeelding en is ingesteld als de standaard-installatiekopie in de opslagplaats van de afbeelding Platform.

### <a name="turn-off-ie-enhanced-security-and-enable-cookies"></a>Uitschakelen Internet Explorer enhanced security en enable cookies

Voor de implementatie van een resource-provider uitvoeren u PowerShell Integrated Scripting Environment (ISE) als beheerder, dus u moet toestaan van cookies en JavaScript in de Internet Explorer-profiel kunt u voor zowel beheerders- als aanmeldingen aanmelden bij Azure Active Directory.

**Als u wilt uitschakelen in Internet Explorer Verbeterde beveiliging:**

1. Aanmelden bij de computer Azure Stack bewijs van concept (implementatiemodel) als AzureStack/beheerder en opent u Server Manager.

2. **Internet Explorer Enhanced Security Configuration** uitschakelen voor zowel beheerders als gebruikers.

3. **ClientVM.AzureStack.local** virtual machine als beheerder aanmelden en opent u Server Manager.

4. **Internet Explorer Enhanced Security Configuration** uitschakelen voor zowel beheerders als gebruikers.

**Cookies inschakelen:**

1. Op het scherm Start van Windows klikt u op **alle apps** **Windows**Bureau-accessoires, klik met de rechtermuisknop op **Internet Explorer**, **meer**en selecteer **Als administrator uitvoeren**.

2. Als u wordt gevraagd, **Gebruik aanbevolen beveiliging**controleren en klik op **OK**.

3. In Internet Explorer, klikt u op de **hulpmiddelen (vistuig) pictogram** &gt; **Internet-opties** &gt; tabblad **Privacy** .

4. Klik op **Geavanceerd**, zorg ervoor dat beide knoppen **accepteren** zijn geselecteerd, klikt u op **OK**en klik vervolgens op **OK** .

5. Internet Explorer sluiten en opnieuw starten van PowerShell ISE als beheerder.

### <a name="install-an-azure-stack-compatible-release-of-azure-powershell"></a>Een stapel Azure compatibele versie van Azure PowerShell installeren

1. Verwijder alle bestaande Azure PowerShell van uw VM-Client.

2. Aanmelden bij de computer Azure Stack Implementatiemodel als AzureStack/beheerder.

3. Met behulp van extern bureaublad aanmelden bij de **ClientVM.AzureStack.local** virtuele machine als beheerder.

4. Open het Configuratiescherm, klikt u op **een programma verwijderen** &gt; Klik op **Azure PowerShell** &gt; Klik op **verwijderen**.

5. [De nieuwste Azure PowerShell die Azure-Stack ondersteunt downloaden](http://aka.ms/azstackpsh) en te installeren.

    Nadat u PowerShell hebt geïnstalleerd, kunt u deze verificatie PowerShell-script om te controleren of u verbinding kunt maken met de stapel Azure-sessie (login webpagina moet worden weergegeven) uitvoeren.

## <a name="bootstrap-the-resource-provider-deployment-powershell"></a>De implementatie van resource provider PowerShell bootstrap

1. Verbinding maken met het externe bureaublad Azure Stack Implementatiemodel clientVm.AzureStack.Local en meld je aan als azurestack\\azurestackuser.

2. [De binaire bestanden van MySQL RP downloaden](http://aka.ms/masmysqlrp) het bestand en pak het D:\\MySQLRP.

3. D: uitvoeren\\MySQLRP\\Bootstrap.cmd-bestand als een beheerder (azurestack\administrator).

    Hiermee opent u het bestand Bootstrap.ps1 in PowerShell ISE.

4. Wanneer de windows PowerShell ISE voltooit laden, klikt u op de knop 'play' of druk op F5.

    Twee belangrijke tabbladen laden, elk met de scripts en bestanden die u nodig hebt voor de implementatie van uw Provider MySQL bron.

## <a name="prepare-prerequisites"></a>Voorwaarden voor te bereiden

Klik op het tabblad **Vereisten voorbereiden** :

- Vereiste certificaten maken
- MySQL binaire bestanden downloaden naar uw stapel Azure
- Onderdelen uploaden naar een opslag op de Stack Azure
- Galerie items publiceren

### <a name="create-the-required-certificates"></a>De vereiste certificaten maken
Dit **Nieuwe SslCert.ps1** script voegt de \_. AzureStack.local.pfx SSL-certificaat op de D:\\MySQLRP\\voorwaarden\\BlobStorage\\Container map. Het certificaat wordt de communicatie tussen de bron-provider en het lokale exemplaar van de bronnenbeheerder Azure beveiligd.

1. Klik op het tabblad **Nieuw SslCert.ps1** in het tabblad belangrijke **Vereisten voorbereiden** en uitvoeren.

2. Typ een PFX-wachtwoord beveiliging voor de persoonlijke sleutel en **Noteer dit wachtwoord**in de prompt. U zult het later nodig hebt.

### <a name="download-mysql-binaries-to-your-azure-stack"></a>MySQL binaire bestanden downloaden naar uw stapel Azure

1. Selecteer het tabblad **Downloaden MySqlServer.ps1** en uit te voeren.
2. Klik op **Ja** in het dialoogvenster bevestigen om de gebruiksrechtovereenkomst te accepteren.

    Met deze opdracht voegt twee zip-bestanden naar de map D:\MySql\Prerequisites\BlobStorage\Container.

### <a name="upload-all-artifacts-to-a-storage-account-on-azure-stack"></a>Alle artefacten uploaden naar een opslag op de Stack Azure

1. Klik op het tabblad **Upload-Microsoft.MySql-RP.ps1** en uit te voeren.

2. Typ in het dialoogvenster Windows PowerShell referentie verzoek de Stack Azure service administrator-referenties.

3. Wanneer voor de Azure Active Directory huurder-ID wordt gevraagd, typt u de FQDN-naam van Azure Active Directory huurder: bijvoorbeeld microsoftazurestack.onmicrosoft.com.

    Een pop-upvenster wordt u gevraagd om referenties.

    > [AZURE.TIP] U die ofwel niet hebt uitgeschakeld met Internet Explorer Verbeterde beveiliging JavaScript inschakelen op deze computer en de gebruiker als het pop-upmenu wordt niet weergegeven, of u cookies in Internet Explorer nog niet hebt geaccepteerd. Zie [stappen voordat u het implementeert](#set-up-steps-before-you-deploy).

4. Typ uw referenties Azure Stack Service Admin en klik op **Aanmelden**.

### <a name="publish-gallery-items-for-later-resource-creation"></a>Galerijobjecten voor het maken van latere bron publiceren

Klik op het tabblad **Publiceren GalleryPackages.ps1** en uit te voeren. Met dit script voegt twee marketplace items aan van de portal Azure Stack Implementatiemodel marketplace kunt u bronnen als marktplaats items database implementeren.

## <a name="deploy-the-mysql-resource-provider-vm"></a>Implementeren van de MySQL bron Provider VM

Nu dat u de haalbaarheidstest Azure Stack met de nodige certificaten en marketplace items hebt voorbereid, kunt u een Resource-voorziening voor SQL Server implementeren. Klik op het tabblad **provider MySQL implementeren** :

   - Waarden in een JSON-bestand dat wordt verwezen naar het implementatieproces
   - Implementeren van de resource-provider
   - De lokale DNS-server bijwerken
   - Registreren van de Provider SQL Server Resource-Adapter

### <a name="provide-values-in-the-json-file"></a>Geef waarden op in de JSON-bestand

Klik op **Microsoft.MySqlprovider.Parameters.JSON**. Dit bestand heeft de parameters die de sjabloon Azure Resource Manager moet correct implementeren op Azure Stack.

1. Vul de **lege** parameters in de JSON-bestand:

    - Zorg ervoor dat u de **adminusername** en **adminpassword** opgeven voor de MySQL bron Provider VM.

    - Zorg ervoor dat u het wachtwoord opgeven voor de parameter **SetupPfxPassword** die u een notitie van in de stap [prequisites voorbereiden gemaakt](#prepare-prerequisites) .

    - Zorg ervoor dat u de parameters **basicAuthUserName** en **basicAuthPassword** bieden. **Maak een notitie van de volgende waarden.** Moet u ze later te registreren van de provider van de resource.

2. Klik op **Opslaan**.

### <a name="deploy-the-resource-provider"></a>Implementeren van de resource-provider

1. Klik op het tabblad **implementeren-Microsoft.Mysql-provider.PS1** en het script wordt uitgevoerd.
2. Typ de naam van de huurder in Azure Active Directory wanneer daarom wordt gevraagd.
3. In het pop-upvenster dienen uw stapel Azure service admin referenties.

De volledige implementatie duurt tussen 15 en 45 minuten op sommige zeer benutte Azure Stack POCs.

### <a name="update-the-local-dns"></a>De lokale DNS-server bijwerken

1. Klik op het tabblad **Register-Microsoft.MySQL-fqdn.ps1** en het script wordt uitgevoerd.
2. Wanneer u wordt gevraagd voor Azure Active Directory huurder-ID, de volledig gekwalificeerde domeinnaam van uw Azure Active Directory-huurder invoer: bijvoorbeeld **microsoftazurestack.onmicrosoft.com**.

### <a name="register-the-sql-rp-resource-provider"></a>De Provider SQL RP Resource registreren

1. Klik op het tabblad **Register-Microsoft.My-provider.ps1** en het script wordt uitgevoerd.

2. Wanneer u wordt gevraagd om referenties, gebruikt u genoteerd als de parameters **basicAuthUserName** en **basicAuthPassword** .

## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Controleer of de distributie via de Portal Azure Stack

1. Meld u af bij de ClientVM en opnieuw aanmelden als **AzureStack\User**.

2. Op het bureaublad, klik op **Azure Stack Implementatiemodel Portal** en zich aanmelden bij de portal als de beheerder van de service.

3. Controleer of dat de installatie is voltooid. Klik op **Bladeren** &gt; **Resourcegroepen**, klikt u op de resourcegroep die u gebruikt (standaard is **MySQLRP**), en zorg ervoor dat de hoofdzaken deel van de bladeserver (bovenste helft) **implementatie is voltooid lezen**.


4. Controleer of dat de registratie is voltooid. Klik op **Bladeren** &gt; **Resource providers**en zoekt u naar een **Lokale MySQL**.

## <a name="create-your-first-mysql-database-to-test-your-deployment"></a>Maak uw eerste MySQL-database om de implementatie testen

1. Aanmelden op de portal Azure Stack Implementatiemodel als servicebeheerder.

2. Klik op de **+** knop &gt; **aangepaste** &gt; **& Databases MySQL-Server**.

3. Vul in het formulier met informatie over de database.

    **Maak een notitie van het "servernaam".** De reeks verbindingen voor uw database bevat "servernaam" als onderdeel van de gebruikersnaam: bijvoorbeeld ** "user@ <ServerName>'**. U moet een gebruikersnaam in de volgende notatie worden ingevoerd wanneer u verbinding met de database maakt: bijvoorbeeld bij het implementeren van een MySQL-website met de provider website Azure resource


## <a name="next-steps"></a>Volgende stappen

Probeer andere [PaaS-services](azure-stack-tools-paas-services.md) , zoals de [provider voor SQL Server-bron](azure-stack-sql-rp-deploy-short.md) en de [Web Apps resource provider](azure-stack-webapps-deploy.md).
