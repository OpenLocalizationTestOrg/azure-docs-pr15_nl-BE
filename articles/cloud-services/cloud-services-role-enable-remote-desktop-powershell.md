<properties
pageTitle="Verbinding met extern bureaublad inschakelen voor een rol in Azure Cloud Services met PowerShell"
description="De toepassing van azure cloud service met PowerShell om verbindingen met extern bureaublad configureren"
services="cloud-services"
documentationCenter=""
authors="thraka"
manager="timlt"
editor=""/>
<tags
ms.service="cloud-services"
ms.workload="tbd"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="08/05/2016"
ms.author="adegeo"/>

# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a>Verbinding met extern bureaublad inschakelen voor een rol in Azure Cloud Services met PowerShell

>[AZURE.SELECTOR]
- [Azure klassieke portal](cloud-services-role-enable-remote-desktop.md)
- [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
- [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)


Extern bureaublad kunt u toegang tot het bureaublad van een rol in Azure uitgevoerd. U kunt verbinding met extern bureaublad oplossen en diagnosticeren van problemen met uw toepassing terwijl deze wordt uitgevoerd.

In dit artikel wordt beschreven hoe het inschakelen van extern bureaublad op uw Cloud Service rollen met PowerShell. Zie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) op de vereisten die nodig zijn voor dit artikel. PowerShell maakt gebruik van de uitbreidingsmodule Extern bureaublad zodat u extern bureaublad inschakelen kunt nadat de toepassing wordt gedistribueerd.


## <a name="configure-remote-desktop-from-powershell"></a>Extern bureaublad vanuit PowerShell configureren

De cmdlet [Set-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) kunt u extern bureaublad op de opgegeven rollen of alle functies van uw implementatie van cloud-service in te schakelen. De cmdlet kunt u de gebruikersnaam en het wachtwoord opgeven voor de externe bureaubladgebruikers via de *referentie* -parameter die een PSCredential-object accepteert.

Als u PowerShell interactief gebruikt, kunt u eenvoudig het PSCredential-object instellen door het aanroepen van de cmdlet [Get-referenties](https://technet.microsoft.com/library/hh849815.aspx) .

```
$remoteusercredentials = Get-Credential
```

Deze opdracht geeft u een dialoogvenster waarin u de gebruikersnaam en het wachtwoord invoeren voor de externe gebruiker op een veilige manier.

Aangezien PowerShell in automatiseringsscenario's helpt, kunt u ook het **PSCredential** -object op een manier die geen interactie van de gebruiker vereist instellen. Eerst moet u een veilig wachtwoord instellen. U begint met het opgeven van een wachtwoord als tekst zonder opmaak converteren naar een beveiligde tekenreeks met [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx). Vervolgens moet u deze beveiligde tekenreeks converteren naar een gecodeerde standaardzin met [ConverterenVan SecureString](https://technet.microsoft.com/library/hh849814.aspx). U kunt nu deze standaard gecodeerde tekenreeks opslaan in een bestand met de [Set inhoud](https://technet.microsoft.com/library/ee176959.aspx).

U kunt ook een beveiligde wachtwoordbestand maken zodat u niet hoeft te typen in het wachtwoord elke keer. Ook is een beveiligd wachtwoordbestand beter dan een bestand met tekst zonder opmaak. De volgende PowerShell gebruiken voor het maken van een beveiligd wachtwoordbestand:

```
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

>[AZURE.IMPORTANT] Zorg bij het instellen van het wachtwoord dat u voldoet aan de [complexiteitsvereisten](https://technet.microsoft.com/library/cc786468.aspx).

Maken referentie-object uit het bestand beveiligd-wachtwoordverificatie, moet u de inhoud van het bestand te lezen en terug te converteren naar een beveiligde tekenreeks met [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).

De cmdlet [Set-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) accepteert ook een *Verloopdatum* -parameter waarmee een **datum/tijd** waarop de account is verlopen. U kunt bijvoorbeeld de account verloopt een paar dagen na de huidige datum en tijd instellen.

Deze PowerShell-voorbeeld ziet u hoe de uitbreidingsmodule Extern bureaublad instellen voor een cloud-service:

```
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
U kunt desgewenst ook de sleuf voor de implementatie en de rollen die u wilt inschakelen, extern bureaublad opgeven. Als deze parameters worden opgegeven, kan de cmdlet extern bureaublad op alle rollen in de sleuf **productie** -implementatie.

De extern bureaublad-extensie is gekoppeld aan een distributie. Als u een nieuwe installatie voor de service maakt, hebt u extern bureaublad op deze distributie inschakelen. Als u altijd extern bureaublad is ingeschakeld wilt, vervolgens raadzaam de PowerShell-scripts te integreren in uw werkstroom implementatie.


## <a name="remote-desktop-into-a-role-instance"></a>Extern bureaublad in een exemplaar van de rol
De cmdlet [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) wordt gebruikt voor extern bureaublad in een specifieke rol exemplaar van uw cloud-service. U kunt de parameter *LocalPath* lokaal het RDP-bestand te downloaden. Of u kunt de parameter *starten* direct starten van het dialoogvenster verbinding met extern bureaublad om toegang tot de rol van het exemplaar van de wolk.

```
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```


## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a>Controleren of extern bureaublad-extensie is ingeschakeld op een service
De cmdlet [Get-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495261.aspx) weergeven die extern bureaublad is ingeschakeld of uitgeschakeld op de implementatie van een service. De cmdlet retourneert de gebruikersnaam voor het externe bureaubladgebruikers en de rollen die voor de extern bureaublad-extensie is ingeschakeld. Dit gebeurt op de sleuf voor de implementatie van standaard, en kunt u in plaats daarvan gebruikt u de staging-sleuf.

```
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a>Extern bureaublad-extensie van een service verwijderen
Als u de extern bureaublad-extensie op een distributie al hebt ingeschakeld en moet bijwerken, de instellingen van extern bureaublad, moet u eerst de extensie verwijderen. En opnieuw inschakelen met de nieuwe instellingen. Bijvoorbeeld, als u wilt een nieuw wachtwoord instellen voor het externe gebruikersaccount of de account is verlopen. Dit is vereist voor bestaande installaties waarvoor de extern bureaublad-extensie ingeschakeld. Voor nieuwe implementaties, kunt u de extensie gewoon rechtstreeks toepassen.

Als de extern bureaublad-extensie uit de implementatie wilt verwijderen, kunt u de [Software-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495280.aspx) -cmdlet. U kunt desgewenst ook de sleuf voor de implementatie en de rol die u wilt verwijderen van de uitbreidingsmodule voor extern bureaublad opgeven.

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

>[AZURE.NOTE] Om volledig te verwijderen de uitbreiding configuratie, roept u de cmdlet *verwijderen* met de parameter **UninstallConfiguration** .
>
>De parameter **UninstallConfiguration** Hiermee verwijdert u eventuele uitbreiding configuratie die wordt toegepast op de service. Elke configuratie extensie is gekoppeld aan de configuratie van de service. Het aanroepen van de cmdlet *verwijderen* zonder **UninstallConfiguration** instelt, scheidt u de <mark>implementatie</mark> van de configuratie van de uitbreiding, dus verwijderen effectief van de extensie. De uitbreiding configuratie blijft echter gekoppeld aan de service.



## <a name="additional-resources"></a>Aanvullende bronnen

[Cloud-Services configureren](cloud-services-how-to-configure.md)
