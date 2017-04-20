<properties
    pageTitle="Azure Stack Haalbaarheidstest implementeren | Microsoft Azure"
    description="Informatie over de Stack Azure Haalbaarheidstest voorbereiden en uitvoeren van het PowerShell script Azure Stack Haalbaarheidstest implementeren."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="erikje"/>

# <a name="deploy-azure-stack-poc"></a>Azure Stack Haalbaarheidstest implementeren
De Stack Azure Haalbaarheidstest implementeren, moet u eerst [de machine implementatie voorbereiden](#prepare-the-deployment-machine) en [de implementatie van PowerShell script uitvoert](#run-the-powershell-deployment-script).

## <a name="download-and-extract-microsoft-azure-stack-poc-tp2"></a>Downloaden en uitpakken Microsoft Azure Stack Implementatiemodel TP2

Voordat u begint, zorg ervoor dat u ten minste 85 GB schijfruimte.

1. Het downloaden van Azure Stack Implementatiemodel TP2 bestaat uit een zip-bestand met de volgende 12 bestanden ~ 20 GB totaal:
    - 1 MicrosoftAzureStackPOC.EXE
2. Lees de gebruiksrechtovereenkomst, scherm en informatie van de Wizard Self-Extractor en klik op **volgende**.
3. Lees de privacyverklaring van scherm en informatie van de Wizard Self-Extractor en klik op **volgende**.
4. Selecteer de bestemming voor de bestanden worden uitgepakt, klikt u op **volgende**.
    - De standaardwaarde is: <drive letter>:\<huidige map > \Microsoft Azure Stack Haalbaarheidstest
5. Het scherm locatie van bestemming en de informatie van de Wizard Self-Extractor en klik vervolgens op **uitpakken** om uit te pakken van de bestanden ThirdPartyLicenses.rtf en CloudBuilder.vhdx (~44.5 GB).

> [AZURE.NOTE] Nadat u de bestanden hebt uitgepakt, kunt u het zip-bestand als u ruimte op de computer wilt verwijderen. Of u kunt verplaatsen naar een andere locatie, dus als u moet u opnieuw het zip-bestand hoeft niet opnieuw te downloaden zip-bestanden.

## <a name="prepare-the-deployment-machine"></a>De machine implementatie voorbereiden

1. Zorg ervoor dat u kunt fysiek verbinding met de computer van de implementatie maken, of hebt toegang tot de fysieke console (zoals KVM). U moet deze toegang nadat u de computer implementatie in stap 9 opnieuw starten.

2. Zorg ervoor dat de implementatie machine voldoet aan de [minimale vereisten](azure-stack-deploy.md). Te bevestigen van uw vereisten kunt u de [Implementatie Checker voor Azure Stack technische Preview 2](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) .

3. Meld u aan als lokale beheerder op uw computer Implementatiemodel.

4. Kopieer het bestand CloudBuilder.vhdx naar C:\CloudBuilder.vhdx.

    > [AZURE.NOTE] Als u ervoor kiest niet het aanbevolen script gebruiken voor het voorbereiden van de hostcomputer Implementatiemodel (stap 5: stap 7), voer de licentiesleutel op de activeringspagina niet. Een evaluatieversie van Windows Server 2016 afbeelding is opgenomen en vervaldatum waarschuwingsberichten een licentiesleutel invoeren veroorzaakt.

5. Op de Haalbaarheidstest machine de volgende PowerShell script uitvoeren om te downloaden van de ondersteuningsbestanden Azure Stack TP2:

    ```powershell
    # Variables
    $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/'
    $LocalPath = 'c:\AzureStack_TP2_SupportFiles'

    # Create folder
    New-Item $LocalPath -type directory

    # Download files
    ( 'BootMenuNoKVM.ps1', 'PrepareBootFromVHD.ps1', 'Unattend.xml', 'unattend_NoKVM.xml') | foreach { Invoke-WebRequest ($uri + $_) -OutFile ($LocalPath + '\' + $_) } 
    ```

    Dit script downloadt de ondersteuningsbestanden Azure Stack TP2 naar de map die is opgegeven met de parameter $LocalPath.
    
6. Een verhoogde PowerShell-console openen en wijzigen in de map waarnaar u de bestanden hebt gekopieerd.
    - 11 MicrosoftAzureStackPOC-N.BIN (waarbij N staat voor 1-11)
7. Klik met de rechtermuisknop op de MicrosoftAzureStackPOC.EXE > uitvoeren als administrator.

8. Voer het script PrepareBootFromVHD.ps1. Dit script en de bestanden voor installatie zonder toezicht worden met de andere scripts ondersteuning met deze build verstrekt.
    Er zijn vijf parameters voor deze PowerShell-script:
    - CloudBuilderDiskPath (vereist): het pad naar de CloudBuilder.vhdx op de HOST.
    - DriverPath (optioneel) – kunt u extra stuurprogramma's voor de host in de virtuele HD toevoegen.
    - ApplyUnattend (optioneel): deze parameter schakeloptie voor het automatiseren van de configuratie van het besturingssysteem opgeven. Als u opgeeft, moet de gebruiker AdminPassword configureren van het besturingssysteem wordt opgestart (mits het bijgevoegde bestand unattend_NoKVM.xml vereist) opgeven.
    Als u deze parameter niet gebruikt, wordt het bestand unattend.xml algemene gebruikt zonder verdere aanpassing. U moet KVM volledig aanpassen nadat deze opnieuw is opgestart.
    - AdminPassword (optioneel) – alleen gebruikt als de parameter ApplyUnattend is ingesteld, moet ten minste zes tekens.
    - VHDLanguage (optioneel) – geeft de VHD-taal, standaard ingesteld op 'en-US'.
    Het script wordt beschreven en het gebruik van het voorbeeld bevat, al is het meest gebruikt:
    
        `.\PrepareBootFromVHD.ps1 -CloudBuilderDiskPath C:\CloudBuilder.vhdx -ApplyUnattend`
    
        Als u deze exacte opdracht uitvoert, moet u AdminPassword achter de opdrachtprompt.

9. Wanneer het script voltooid is, moet u bevestigen dat de computer opnieuw wordt opgestart. Als er andere gebruikers die zijn aangemeld, wordt deze opdracht mislukt. Als de opdracht niet werkt, kunt u de volgende opdracht uitvoeren:`Restart-Computer -force` 

10. De HOST opnieuw is opgestart in het besturingssysteem van de CloudBuilder.vhdx, waar de installatie wordt voortgezet.

> [AZURE.IMPORTANT] Azure Stack vereist toegang tot het Internet, rechtstreeks of via een transparante proxy. De implementatie van TP2 Implementatiemodel ondersteunt precies één NIC voor netwerken. Als u meerdere netwerkkaarten, zorg ervoor dat slechts één is ingeschakeld (en alle andere uitgeschakeld) voordat u het script voor implementatie in de volgende sectie.

## <a name="run-the-powershell-deployment-script"></a>De implementatie van PowerShell script uitvoeren

1. Meld u aan als lokale beheerder op uw computer Implementatiemodel. De referenties die zijn opgegeven in de vorige stappen gebruiken.

2. Open de console van een verhoogde PowerShell.

3. In PowerShell, kunt u deze opdracht uitvoert:`cd C:\CloudDeployment\Configuration`

4. Voer de opdracht distribueren:`.\InstallAzureStackPOC.ps1`

5. Geef een wachtwoord op bij de prompt **Geef het wachtwoord** en Bevestig het. Dit is het wachtwoord voor de virtuele machines. Zorg ervoor dat het opnemen.

6. Voer de referenties voor uw Azure Active Directory-account. Deze gebruiker moet de globale beheerder in de directory huurder.

7. De implementatie kan een paar uur, gedurende welke het systeem automatisch opnieuw opgestart eenmaal duren.

    > [AZURE.IMPORTANT] Als u de voortgang van implementatie wilt, moet u zich aanmelden als azurestack\AzureStackAdmin. Als u bent aangemeld als een lokale beheerder nadat de computer lid is van het domein, kunt u de voortgang van de implementatie niet zien. Voer de installatie opnieuw uit, in plaats daarvan zich aanmelden als azurestack\AzureStackAdmin om te controleren dat wordt uitgevoerd niet.

    Wanneer de installatie is voltooid, wordt de PowerShell-console weergegeven: **voltooid: actie 'Inzet'**.

    Als de installatie mislukt, kunt u proberen om [opnieuw te starten](azure-stack-rerun-deploy.md). U kunt [opnieuw](azure-stack-redeploy.md) het helemaal.

### <a name="deployment-script-examples"></a>Voorbeelden van de implementatie van scripts

Als uw identiteit AAD alleen is gekoppeld aan een bepaalde map AAD:

    cd C:\CloudDeployment\Configuration
    $adminpass = ConvertTo-SecureString "<LOCAL ADMIN PASSWORD>" -AsPlainText -Force
    $aadpass = ConvertTo-SecureString "<AAD GLOBAL ADMIN ACCOUNT PASSWORD>" -AsPlainText -Force
    $aadcred = New-Object System.Management.Automation.PSCredential ("<AAD GLOBAL ADMIN ACCOUNT>", $aadpass)
    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred

Als uw identiteit AAD gekoppeld aan meer dan één AAD Directory is:

    cd C:\CloudDeployment\Configuration
    $adminpass = ConvertTo-SecureString "<LOCAL ADMIN PASSWORD>" -AsPlainText -Force
    $aadpass = ConvertTo-SecureString "<AAD GLOBAL ADMIN ACCOUNT PASSWORD>" -AsPlainText -Force
    $aadcred = New-Object System.Management.Automation.PSCredential ("<AAD GLOBAL ADMIN ACCOUNT> example: user@AADDirName.onmicrosoft.com>", $aadpass)
    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred -AADDirectoryTenantName "<SPECIFIC AAD DIRECTORY example: AADDirName.onmicrosoft.com>"

Als uw omgeving geen DHCP ingeschakeld, moet u de volgende aanvullende parameters aan een van de opties hierboven (voorbeeld gebruik verstrekt) bevatten:

    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred
    -NatIPv4Subnet 10.10.10.0/24 -NatIPv4Address 10.10.10.3 -NatIPv4DefaultGateway 10.10.10.1


### <a name="installazurestackpocps1-optional-parameters"></a>Optionele parameters InstallAzureStackPOC.ps1

| Parameter | Vereist/optioneel | Beschrijving |
| --------- | ----------------- | ----------- |
| AADAdminCredential | Optioneel | Hiermee stelt u de Azure Active Directory-gebruikersnaam en wachtwoord. Deze referenties Azure is een organisatie-ID of een Microsoft-Account. Om de referenties van de Microsoft-Account gebruikt, kunt u deze parameter in de cmdlet weglaat. Als u deze parameter weglaat, wordt de pop-up Azure verificatie gevraagd tijdens de implementatie. Hiermee maakt u de verificatie en vernieuwen tokens gebruikt tijdens de implementatie. |
| AADDirectoryTenantName | Vereist | De huurder map instellen. Gebruik deze parameter om op te geven van een specifieke map waar de rekening AAD heeft machtigingen voor het beheren van meerdere mappen. Volledige naam van een huurder AAD Directory in de indeling van <directoryName>. onmicrosoft.com. |
| AdminPassword | Vereist | Hiermee stelt u de lokale administrator-account en alle andere gebruikersaccounts op alle virtuele machines die worden gemaakt als onderdeel van de implementatie van Implementatiemodel. Dit wachtwoord moet overeenkomen met de huidige lokale administrator-wachtwoord op de host. |
| AzureEnvironment | Optioneel | Selecteer de Azure-omgeving die u wilt registreren deze stapel Azure-implementatie. Beschikbare opties zijn *Openbare Azure*, *Azure - China*, *Azure - Amerikaanse regering*. |
| EnvironmentDNS | Optioneel | Een DNS-server wordt gemaakt als onderdeel van de Stack Azure-implementatie. Als u wilt dat computers in de oplossing voor namen buiten het stempel, bieden de bestaande infrastructuur DNS-server. De DNS-server in het stempel onbekende aanvragen voor naamomzetting naar deze server worden doorgestuurd. |
| NatIPv4Address | Vereist voor ondersteuning van DHCP NAT | Hiermee stelt een statisch IP-adres voor MAS BGPNAT01. Gebruik deze parameter alleen als een geldig IP-adres voor toegang tot het Internet kan niet worden toegewezen door de DHCP. |
| NatIPv4DefaultGateway | Vereist voor ondersteuning van DHCP NAT | Hiermee stelt u de standaard-gateway voor MAS BGPNAT01 met het statische IP-adres gebruikt. Gebruik deze parameter alleen als een geldig IP-adres voor toegang tot het Internet kan niet worden toegewezen door de DHCP.  |
| NatIPv4Subnet | Vereist voor ondersteuning van DHCP NAT | IP-Subnet voorvoegsel dat wordt gebruikt voor DHCP via NAT-ondersteuning. Gebruik deze parameter alleen als een geldig IP-adres voor toegang tot het Internet kan niet worden toegewezen door de DHCP.  |
| PublicVLan | Optioneel | Hiermee stelt u de VLAN-ID. Gebruik deze parameter alleen als host en MAS BGPNAT01 VLAN-ID voor toegang tot het fysieke netwerk (en Internet) moet configureren. Bijvoorbeeld:`.\InstallAzureStackPOC.ps1 –Verbose –PublicVLan 305` |
| Opnieuw uitvoeren | Optioneel | Deze vlag implementatie opnieuw wilt gebruiken.  Alle vorige input wordt gebruikt. Opnieuw invoeren van de gegevens eerder hebt opgegeven, wordt niet ondersteund omdat verschillende unieke waarden worden gegenereerd en voor de implementatie gebruikt. |
| TimeServer | Optioneel | Gebruik deze parameter als u een bepaalde tijdserver opgeven. |

## <a name="next-steps"></a>Volgende stappen

[Verbinding maken met Azure Stack](azure-stack-connect-azure-stack.md)
