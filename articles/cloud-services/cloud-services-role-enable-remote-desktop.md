<properties 
pageTitle="Verbinding met extern bureaublad voor een rol in Azure Cloud Services inschakelen" 
description="Uw toepassing azure cloud service waarmee verbindingen met extern bureaublad configureren" 
services="cloud-services" 
documentationCenter="" 
authors="sbtron" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="02/17/2016" 
ms.author="saurabh"/>

# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Verbinding met extern bureaublad voor een rol in Azure Cloud Services inschakelen

>[AZURE.SELECTOR]
- [Azure klassieke portal](cloud-services-role-enable-remote-desktop.md)
- [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
- [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)


Extern bureaublad kunt u toegang tot het bureaublad van een rol in Azure uitgevoerd. U kunt verbinding met extern bureaublad oplossen en diagnosticeren van problemen met uw toepassing terwijl deze wordt uitgevoerd. 

Kunt u een verbinding met extern bureaublad in uw rol tijdens de ontwikkeling door de extern bureaublad-modules in de definitie van service of kunt u extern bureaublad via de uitbreidingsmodule Extern bureaublad inschakelen. De beste benadering is de extensie extern bureaublad gebruiken als u extern bureaublad inschakelen kunt, zelfs nadat de toepassing wordt gedistribueerd zonder de toepassing opnieuw distribueren. 


## <a name="configure-remote-desktop-from-the-azure-classic-portal"></a>Extern bureaublad configureren via de klassieke Azure portal
De klassieke Azure portal wordt gebruikt de aanpak van de uitbreidingsmodule Extern bureaublad kunt u extern bureaublad inschakelen, zelfs nadat de toepassing wordt gedistribueerd. De pagina **configureren** voor uw cloud-service kunt u extern bureaublad inschakelen, wijzigt u de lokale Administrator-account gebruikt voor verbinding met de virtuele machines, het certificaat bij de verificatie worden gebruikt en de vervaldatum instellen. 


1. Klik op **Cloud-Services**en klik vervolgens op **configureren**klikt u op de naam van de service cloud.

2. Klik op **RAS**.
    
    ![Cloud services extern](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)
    
    > [AZURE.WARNING] Alle exemplaren van de rol wordt opnieuw opgestart wanneer u eerst Extern bureaublad inschakelen en klik op OK (vinkje). Om te voorkomen dat de computer opnieuw wordt opgestart, moet het certificaat gebruikt voor de codering van het wachtwoord op de rol worden geïnstalleerd. Om te voorkomen dat opnieuw [een certificaat voor de service cloud uploaden](cloud-services-how-to-create-deploy/#how-to-upload-a-certificate-for-a-cloud-service) en vervolgens terugkeren naar dit dialoogvenster.
    

3. Selecteer de rol die u wilt bijwerken of **alle** selecteren voor alle rollen in **rollen**.

4. Een van de volgende wijzigingen aanbrengen:
    
    - Als u extern bureaublad, selecteert u het selectievakje **Extern bureaublad inschakelen** . Schakel het selectievakje uit als u wilt uitschakelen van extern bureaublad.
    
    - Maak een account te gebruiken in een extern bureaublad-verbindingen met de exemplaren van de rol.
    
    - Werk het wachtwoord voor een bestaande account.
    
    - Selecteer een geüploade certificaat gebruiken voor verificatie (het certificaat **uploaden** op de pagina **certificaten** uploaden) of een nieuw certificaat maken. 
    
    - Wijzig de vervaldatum voor de configuratie van extern bureaublad.

5. Wanneer u klaar bent met het bijwerken van de configuratie, klikt u op **OK** (vinkje).


## <a name="remote-into-role-instances"></a>Afstand naar exemplaren van de rol
Als Extern bureaublad is ingeschakeld op de rollen kunt u de afstand in een exemplaar van de rol in diverse hulpprogramma's.

Verbinding maken met een exemplaar van de rol van de klassieke Azure portal:
    
  1.   Klik op **instanties** om de **exemplaren** pagina te openen.
  2.   Selecteer een rol exemplaar waarop Extern bureaublad is geconfigureerd.
  3.   Klik op **verbinding maken**en volg de instructies voor het openen van het bureaublad. 
  4.   Klik op **openen** en vervolgens op **verbinden** om de verbinding met extern bureaublad te starten. 


### <a name="use-visual-studio-to-remote-into-a-role-instance"></a>Visual Studio gebruikt om externe naar een exemplaar van de rol

In Visual Studio Server Explorer:

1. Vouw de **Azure\\Cloud Services\\[naam cloud service]** knooppunt.
2. Vouw **gefaseerde installatie** of **productie**.
3. Vouw de afzonderlijke rol.
4. Met de rechtermuisknop op een van de exemplaren van de rol, klikt u op **verbinding maken via Extern bureaublad...**en geef vervolgens de gebruikersnaam en het wachtwoord. 

![Server explorer extern bureaublad](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)


### <a name="use-powershell-to-get-the-rdp-file"></a>PowerShell gebruiken om het RDP-bestand
U kunt de cmdlet [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) voor het ophalen van het RDP-bestand. Vervolgens kunt u het RDP-bestand verbinding met extern bureaublad om toegang tot de service cloud.

### <a name="programmatically-download-the-rdp-file-through-the-service-management-rest-api"></a>Het RDP-bestand via de Service Management REST API softwarematig downloaden
De werking van de REST [RDP-bestand downloaden](https://msdn.microsoft.com/library/jj157183.aspx) kunt u het RDP-bestand te downloaden. 



## <a name="to-configure-remote-desktop-in-the-service-definition-file"></a>Extern bureaublad configureren in het bestand van de definitie

Met deze methode kunt u extern bureaublad inschakelen voor de toepassing tijdens de ontwikkeling. Deze aanpak is vereist voor gecodeerde wachtwoorden worden opgeslagen in de configuratie van de service Bestands- en updates van de configuratie voor extern bureaublad moet een nieuwe installatie van de toepassing. Als u wilt voorkomen dat deze nadelen moet u de extern bureaublad-extensie op basis van aanpak hierboven beschreven.  

Visual Studio kunt u [een verbinding met extern bureaublad inschakelen](../vs-azure-tools-remote-desktop-roles.md) met behulp van de service definitie bestand aanpak.  
De volgende stappen beschrijven de wijzigingen die nodig zijn voor de model-bestanden extern bureaublad inschakelen. Visual Studio worden deze wijzigingen automatisch maakt wanneer u publiceert.

### <a name="set-up-the-connection-in-the-service-model"></a>Instellen van de verbinding in het service-model 
Met de **invoer** -element kunt u de **RemoteForwarder** -module en de module **Remote Access** importeren naar het bestand [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) .

Het definitiebestand moet er ongeveer als volgt met de `<Imports>` element toegevoegd.

```xml
<ServiceDefinition name="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2013-03.2.0">
    <WebRole name="WebRole1" vmsize="Small">
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="Endpoint1" endpointName="Endpoint1" />
                </Bindings>
            </Site>
        </Sites>
        <Endpoints>
            <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
            <Import moduleName="Diagnostics" />
            <Import moduleName="RemoteAccess" />
            <Import moduleName="RemoteForwarder" />
        </Imports>
    </WebRole>
</ServiceDefinition>
```
Het bestand [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) moet worden, zoals in het volgende voorbeeld, Let op de `<ConfigurationSettings>` en `<Certificates>` elementen. Het certificaat dat moet worden [geüpload naar de cloud-service](../cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2013-03.2.0">
    <Role name="WebRole1">
        <Instances count="2" />
        <ConfigurationSettings>
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="[name-of-user-account]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="[base-64-encrypted-user-password]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="[certificate-expiration]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" />
        </ConfigurationSettings>
        <Certificates>
            <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="[certificate-thumbprint]" thumbprintAlgorithm="sha1" />
        </Certificates>
    </Role>
</ServiceConfiguration>
```


## <a name="additional-resources"></a>Aanvullende bronnen

[Cloud-Services configureren](cloud-services-how-to-configure.md)