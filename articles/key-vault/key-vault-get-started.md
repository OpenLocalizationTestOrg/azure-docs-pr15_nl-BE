<properties
    pageTitle="Aan de slag met Azure sleutel kluis | Microsoft Azure"
    description="Met deze zelfstudie kunt u aan de slag met Azure sleutel kluis voor het maken van een harde container in Azure, opslaan en beheren van cryptografische sleutels en geheimen in Azure."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/24/2016"
    ms.author="cabailey"/>

# <a name="get-started-with-azure-key-vault"></a>Aan de slag met Azure sleutel kluis #
Azure sleutel kluis is beschikbaar in de meeste regio's. Zie de [sleutel kluis prijzen pagina](https://azure.microsoft.com/pricing/details/key-vault/)voor meer informatie.

## <a name="introduction"></a>Inleiding  
Met deze zelfstudie kunt u aan de slag met Azure sleutel kluis voor het maken van een harde container (kluis) in Azure, opslaan en beheren van cryptografische sleutels en geheimen in Azure. Het helpt u bij het proces van Azure PowerShell gebruiken voor het maken van een kluis met een sleutel of het wachtwoord dat u een toepassing Azure kunt. Deze vervolgens ziet u hoe een toepassing kan gebruiken die sleutel of wachtwoord.

**Geschatte tijd:** 20 minuten

>[AZURE.NOTE]  Deze zelfstudie bevat geen instructies voor het schrijven van de Azure toepassing dat een van de stappen omvat, namelijk het toestaan van een toepassing voor het gebruik van een sleutel of een geheim in de sleutel kluis.
>
>In deze zelfstudie wordt Azure PowerShell. Zie voor instructies de opdrachtregelinterface Cross-Platform, [deze gelijkwaardig zelfstudie](key-vault-manage-with-cli.md).

Zie voor informatie over Azure sleutel kluis, [Wat is Azure sleutel kluis?](key-vault-whatis.md)

## <a name="prerequisites"></a>Vereisten

Deze zelfstudie hebt u het volgende:

- Een abonnement op Microsoft Azure. Als u dit niet hebt nog, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).
- Azure PowerShell **minimumversie van 1.1.0**. Azure PowerShell te installeren en te koppelen aan uw abonnement Azure, Zie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md). Als u Azure PowerShell al hebt geïnstalleerd en de versie, vanuit de console Azure PowerShell niet kent, typt u `(Get-Module azure -ListAvailable).Version`. Wanneer u Azure PowerShell versie 0.9.1 via 0.9.8 geïnstalleerd hebt, kunt u deze zelfstudie met enkele kleine wijzigingen. Bijvoorbeeld, moet u de `Switch-AzureMode AzureResourceManager` opdracht en sommige opdrachten Azure sleutel kluis zijn gewijzigd. Zie voor een lijst van de sleutel kluis cmdlets voor versie 0.9.1 via 0.9.8 [Azure sleutel kluis Cmdlets](https://msdn.microsoft.com/library/azure/dn868052\(v=azure.98\).aspx). 
- Een toepassing die wordt geconfigureerd voor het gebruik van de sleutel of wachtwoord die u in deze zelfstudie maakt. Een voorbeeldtoepassing is beschikbaar via het [Microsoft Download Center](http://www.microsoft.com/en-us/download/details.aspx?id=45343). Zie de bijbehorende Leesmij-bestand voor instructies.


Deze zelfstudie is ontworpen voor beginners Azure PowerShell, maar wordt ervan uitgegaan dat u de basisconcepten, zoals modules, cmdlets en sessies begrijpt. Zie [aan de slag met Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx)voor meer informatie.

Voor gedetailleerde informatie over de cmdlet die u in deze zelfstudie ziet, gebruikt u de cmdlet **Get-Help** .

    Get-Help <cmdlet-name> -Detailed

Bijvoorbeeld, voor informatie over de cmdlet **Aanmelding AzureRmAccount** , typt u:

    Get-Help Login-AzureRmAccount -Detailed

U kunt ook de volgende zelfstudies om vertrouwd te raken met Azure Resource Manager in Azure PowerShell lezen:

- [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md)
- [Met behulp van Azure PowerShell met Resource Manager](../powershell-azure-resource-manager.md)


## <a id="connect"></a>Verbinding maken met uw abonnementen ##

Een Azure PowerShell-sessie start en aanmelden bij uw account Azure met de volgende opdracht:  

    Login-AzureRmAccount 

Houd er rekening mee dat als u een specifiek exemplaar van Azure, bijvoorbeeld Azure overheid gebruik de omgeving parameter - met deze opdracht. Bijvoorbeeld:`Login-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)`

Voer uw Azure gebruikersnaam en wachtwoord in het pop-browservenster. Azure PowerShell haalt alle abonnementen die zijn gekoppeld aan deze account en standaard, de eerste die wordt gebruikt.

Meerdere abonnementen als een specifieke opgeven voor Azure sleutel kluis, typt u het volgende overzicht van de abonnementen voor uw account:

    Get-AzureRmSubscription

Als u het abonnement wilt gebruiken, typt u vervolgens:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Zie voor meer informatie over het configureren van Azure PowerShell [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).


## <a id="resource"></a>Een nieuwe resourcegroep maken ##

Wanneer u Azure Resource Manager gebruikt, worden alle verwante resources in een resourcegroep gemaakt. We gaan een nieuwe groep met de naam **ContosoResourceGroup** voor deze zelfstudie maken:

    New-AzureRmResourceGroup –Name 'ContosoResourceGroup' –Location 'East Asia'


## <a id="vault"></a>Maak een sleutel kluis ##

Gebruik de cmdlet [New-AzureRmKeyVault](https://msdn.microsoft.com/library/azure/mt603736\(v=azure.300\).aspx) voor het maken van een sleutel kluis. Deze cmdlet heeft drie verplichte parameters: een **Resourcegroepnaam**, een **naam sleutel kluis**en de **geografische locatie**.

Bijvoorbeeld, als u de naam van de kluis van **ContosoKeyVault**, de naam van de resource van **ContosoResourceGroup**en de locatie van **Oost-Azië**, typt u:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

De uitvoer van deze cmdlet worden de eigenschappen van de sleutel kluis die u net hebt gemaakt. De twee belangrijkste eigenschappen zijn:

- **Naam van de kluis**: In het voorbeeld is dit **ContosoKeyVault**. U gebruikt deze naam voor een andere sleutel kluis cmdlets.
- **Kluis URI**: In het voorbeeld is dit https://contosokeyvault.vault.azure.net/. Toepassingen die gebruikmaken van de kluis via de REST API, moeten deze URI gebruiken.

Uw Azure account mag zich nu alle bewerkingen uitvoeren op deze sleutel kluis. Nog is niemand anders.

>[AZURE.NOTE]  Als de fout **het abonnement is niet geregistreerd voor het gebruik van 'Microsoft.KeyVault' naamruimte** wordt weergegeven wanneer u probeert te maken van uw nieuwe sleutel kluis, uitgevoerd `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"` en voer de opdracht Nieuw AzureRmKeyVault. Zie [Register-AzureRmResourceProvider](https://msdn.microsoft.com/library/azure/mt759831\(v=azure.300\).aspx)voor meer informatie.
>

## <a id="add"></a>Een sleutel of een geheim om de sleutel toevoegen ##

Als u wilt dat Azure sleutel kluis met een sleutel software beschermd voor u maken, gebruikt u de cmdlet [Add-AzureKeyVaultKey](https://msdn.microsoft.com/library/azure/dn868048\(v=azure.300\).aspx) en typt u het volgende:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -Destination 'Software'

Echter, als u een bestaande sleutel voor software beschermd hebben in een. PFX-bestand opgeslagen op de schijf C:\ in een bestand met de naam softkey.pfx die u wilt uploaden naar Azure sleutel kluis, typt u het volgende in te stellen voor een wachtwoord van **123** voor de variabele **securepfxpwd** de. PFX-bestand:

    $securepfxpwd = ConvertTo-SecureString –String '123' –AsPlainText –Force

Typ het volgende als u wilt importeren, de sleutel uit het. PFX-bestand, die de sleutel worden beveiligd door de software in de sleutel kluis-service:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd


Nu kunt u verwijzen naar deze sleutel die u hebt gemaakt of geüpload naar Azure sleutel kluis, met behulp van de URI. Altijd beschikt over de huidige versie met **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** en **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** gebruiken om deze specifieke versie.  

De URI voor deze sleutel, typ:

    $Key.key.kid

Een geheim toevoegen aan de kluis die een wachtwoord met de naam SQLPassword is de waarde van Pa$ w0rd naar Azure sleutel kluis, eerst de waarde van Pa$ $w0rd naar een beveiligde tekenreeks converteren door het volgende te typen:

    $secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force

Typ het volgende:

    $secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword' -SecretValue $secretvalue

Nu kunt u verwijzen naar dit wachtwoord die u hebt toegevoegd aan de Azure sleutel kluis, met behulp van de URI. Altijd beschikt over de huidige versie met **https://ContosoVault.vault.azure.net/secrets/SQLPassword** en **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** gebruiken om deze specifieke versie.

De URI voor dit geheim, typ:

    $secret.Id

We bekijken de sleutel of een geheim dat u zojuist hebt gemaakt:

- Als u wilt weergeven in uw sleutel, typt u:`Get-AzureKeyVaultKey –VaultName 'ContosoKeyVault'`
- Als u wilt uw geheime weergeven, typt u:`Get-AzureKeyVaultSecret –VaultName 'ContosoKeyVault'`

Uw sleutel kluis en code of geheim is nu gereed om toepassingen te gebruiken. Toepassingen wilt gebruiken, moet u machtigen.  

## <a id="register"></a>Een toepassing met Azure Active Directory registreren ##

Deze stap zou worden meestal gebruikt door een ontwikkelaar, op een afzonderlijke computer. Het is niet specifiek voor Azure sleutel kluis, maar is hier opgenomen voor de volledigheid.


>[AZURE.IMPORTANT] Om de zelfstudie, uw account de kluis en de toepassing die u in deze stap wordt registreert moet alle in Azure Active directory.

Toepassingen die gebruikmaken van een sleutel kluis moeten worden geverifieerd met behulp van een token uit Azure Active Directory. Hiervoor moet de eigenaar van de toepassing eerst de toepassing in Azure Active Directory registreren. Aan het einde van de registratie wordt de eigenaar van de toepassing de volgende waarden:


- Een **Toepassings-ID** (ook wel een Client-ID) en de **verificatiesleutel** (ook bekend als het gedeelde geheim). De toepassing biedt deze beide waarden naar Azure Active Directory een token ophalen. Hoe de toepassing hiervoor is geconfigureerd, is afhankelijk van de toepassing. Voor de voorbeeldtoepassing sleutel kluis stelt de eigenaar van de toepassing deze waarden in het bestand app.config.

De toepassing in Azure Active Directory registreren:

1. Log in om de klassieke Azure portal.
2. Klik op **Active Directory**aan de linkerkant en selecteer vervolgens de directory waarin u uw toepassing wordt geregistreerd. <br> <br> **Opmerking:** Selecteer de directory waarin de Azure abonnement waarmee u uw sleutel kluis gemaakt. Als u niet welke map dit weet is, klikt u op **Instellingen**en noteer de naam van de map weergegeven in de laatste kolom herkennen het abonnement waarmee u uw sleutel kluis gemaakt.

3. Klik op **toepassingen**. Als er geen apps zijn toegevoegd aan de map, wordt deze pagina alleen de koppeling **toevoegen** op een App. Klik op de koppeling of als alternatief kunt u **toevoegen** op de opdrachtbalk.
4.  In de wizard **Toepassing toevoegen** op de **Wat wilt u doen?** pagina, klikt u op **het ontwikkelen van mijn organisatie toepassing toevoegen**.
5.  Geef een naam op voor uw toepassing en **WEB toepassing en/of WEB-API** (de standaardwaarde) selecteert op de pagina **Vertel ons over uw toepassing** . Klik op het **volgende** pictogram.
6.  Geef de **SIGN-ON-URL** en **APP-ID-URI** voor uw webtoepassing op de pagina **Eigenschappen van App** . Als uw toepassing niet over deze waarden, kunt u ze voor deze stap (bijvoorbeeld, kon u http://test1.contoso.com voor beide vakken). Het maakt niet uit als deze sites bestaan. Wat is het belangrijk is dat de app URI-ID voor elke toepassing verschillend voor elke toepassing in de map. De map wordt deze tekenreeks gebruikt voor het identificeren van uw app.
7.  Klik op het pictogram **Voltooien** om de wijzigingen opslaan in de wizard.
8.  Op de pagina **Quick Start** , klikt u op **configureren**.
9.  Schuif naar de sectie **sleutels** , selecteert u de duur en klik vervolgens op **Opslaan**. De pagina wordt vernieuwd en bevat nu de waarde van een sleutel. U moet uw toepassing configureren met de waarde van deze sleutel en de waarde van de **CLIENT-ID** . (Instructies voor deze configuratie zijn toepassingsspecifiek.)
10. Kopieer de waarde van de client-ID op deze pagina wordt u in de volgende stap machtigingen in te stellen op de kluis.

## <a id="authorize"></a>De toepassing voor het gebruik van de sleutel of een geheim toestaan ##

Als u wilt toestaan dat de toepassing toegang tot de sleutel of een geheim in de kluis, gebruikt u de cmdlet  [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/mt603625\(v=azure.300\).aspx) .

Bijvoorbeeld als de naam van uw kluis is **ContosoKeyVault** en de toepassing die u wilt toestaan dat een client-ID van de 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed heeft en u wilt toestaan dat de toepassing voor het decoderen en ondertekenen met sleutels in uw kluis, het volgende uitvoeren:


    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Als u toestaan dat dezelfde toepassing wilt lezen geheimen in de kluis, het volgende uitvoeren:


    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get

## <a id="HSM"></a>Als u wilt gebruiken een hardwarebeveiligingsmodule (HSM) ##

U kunt importeren of het genereren van sleutels in hardware security modules (HSM's) die nooit de HSM-grens laat voor toegevoegde assurance. De HSM's zijn FIPS 140-2 niveau 2 gevalideerd. Als deze eis niet voor u geldt, kunt u deze sectie overslaan en Ga naar [de sleutel kluis te verwijderen en het bijbehorende sleutels en geheimen](#delete).

Om deze sleutels beveiligd met een HSM maakt, moet u de [Azure sleutel kluis Premium service tier beveiligd met een HSM sleutels ondersteunen](https://azure.microsoft.com/pricing/free-trial/). Bovendien ziet deze functionaliteit is niet beschikbaar voor China Azure.


Als u de sleutel kluis maakt, moet u de parameter **- SKU** toevoegen:


    New-AzureRmKeyVault -VaultName 'ContosoKeyVaultHSM' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -SKU 'Premium'



U kunt software beschermd (zoals eerder) en beveiligd met een HSM sleutels toevoegen aan deze sleutel kluis. Als u een sleutel beveiligd met een HSM, stelt de **-bestemming** met de parameter 'HSM':

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -Destination 'HSM'

U kunt de volgende opdracht importeert een sleutel uit een. PFX-bestand op uw computer. Met deze opdracht importeert de sleutel in HSM's in de sleutel kluis-service:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd -Destination 'HSM'


De volgende opdracht importeert u een "doen om uw eigen sleutel' pakket (BYOK). In dit scenario kunt u uw sleutel in uw lokale HSM genereren en overbrengen naar HSM's in de sleutel kluis-service zonder het verlaten van de rand van de HSM-sleutel:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\ITByok.byok' -Destination 'HSM'

Zie voor gedetailleerde instructies over het genereren van dit pakket BYOK [genereren en de overdracht beveiligd met een HSM sleutels voor Azure sleutel kluis](key-vault-hsm-protected-keys.md).

## <a id="delete"></a>Verwijder de sleutel kluis en de bijbehorende sleutels en geheimen ##

Als u niet langer de sleutel kluis en de sleutel of een geheim dat het bevat, kunt u de sleutel kluis verwijderen met behulp van de cmdlet [Verwijderen AzureRmKeyVault](https://msdn.microsoft.com/library/azure/mt619485\(v=azure.300\).aspx) :

    Remove-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Of u kunt een hele Azure resourcegroep, waaronder de sleutel kluis en andere bronnen die u hebt opgenomen in de groep verwijderen:

    Remove-AzureRmResourceGroup -ResourceGroupName 'ContosoResourceGroup'


## <a id="other"></a>Andere Azure PowerShell-Cmdlets ##

Andere opdrachten die nuttig voor het beheren van Azure sleutel kluis:

- `$Keys = Get-AzureKeyVaultKey -VaultName 'ContosoKeyVault'`: Met deze opdracht haalt een in tabelvorm weergegeven van alle geselecteerde eigenschappen en sleutels.
- `$Keys[0]`: Met deze opdracht geeft u een volledige lijst van eigenschappen voor de opgegeven sleutel
- `Get-AzureKeyVaultSecret`: Met deze opdracht wordt een tabelvorm weergave van alle geselecteerde eigenschappen en geheime namen.
- `Remove-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey'`: Voorbeeld het verwijderen van een specifieke sleutel.
- `Remove-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword'`: Voorbeeld het verwijderen van een specifieke geheim.


## <a id="next"></a>Volgende stappen ##

Zie voor een zelfstudie voor opvolging met Azure sleutel kluis in een webtoepassing, [Gebruik Azure sleutel kluis uit een webtoepassing](key-vault-use-from-web-application.md).

Hoe uw sleutel kluis wordt gebruikt, Zie [Logboekregistratie van Azure sleutel kluis](key-vault-logging.md).

Zie voor een lijst met de meest recente Azure PowerShell-cmdlets voor Azure sleutel kluis, [Azure sleutel kluis Cmdlets](https://msdn.microsoft.com/library/azure/dn868052\(v=azure.300\).aspx). 
 

Zie [de Azure sleutel kluis developer's guide](key-vault-developers-guide.md)voor het programmeren van verwijzingen.
