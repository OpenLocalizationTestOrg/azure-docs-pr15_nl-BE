<properties
    pageTitle="Logboekregistratie van Azure sleutel kluis | Microsoft Azure"
    description="Deze zelfstudie gebruiken om u aan de slag met Azure sleutel kluis te registreren."
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
    ms.date="08/31/2016"
    ms.author="cabailey"/>

# <a name="azure-key-vault-logging"></a>Logboekregistratie van Azure sleutel kluis #
Azure sleutel kluis is beschikbaar in de meeste regio's. Zie de [sleutel kluis prijzen pagina](https://azure.microsoft.com/pricing/details/key-vault/)voor meer informatie.

## <a name="introduction"></a>Inleiding  
Nadat u een of meer sleutels kluizen hebt gemaakt, zult u waarschijnlijk om te controleren hoe en wanneer uw sleutel kluizen toegankelijk is, en door wie zijn. U kunt dit doen door het inschakelen van logboekregistratie voor sleutel kluis, waarop informatie wordt opgeslagen in een opslag Azure-account die u opgeeft. Een nieuwe container **inzichten-logboeken-auditevent** met de naam wordt automatisch gemaakt voor uw account opgegeven opslag en u kunt deze rekening opslag gebruiken voor het verzamelen van Logboeken voor meerdere sleutels kluizen.

U toegang hebt tot uw gegevens aanmelden hooguit 10 minuten nadat de sleutel kluis bewerking. In de meeste gevallen worden sneller dan dit.  Is het aan u de logboeken in uw account opslag beheren:

- Standaard Azure access controlemethoden gebruiken voor het beveiligen van uw logboeken door te beperken wie toegang heeft tot deze.
- Verwijderen logboeken die u niet langer wilt behouden in uw account voor opslag.

Gebruik deze zelfstudie om u te helpen aan de slag met Azure sleutel kluis registreren om uw account opslag maken logboekregistratie inschakelen en interpreteren van gegevens in het logboek verzameld.  


>[AZURE.NOTE]  Deze zelfstudie bevat geen instructies voor het maken van sleutels kluizen, -sleutels of geheimen. Zie voor deze informatie [aan de slag met Azure sleutel kluis](key-vault-get-started.md). Of Zie voor instructies de opdrachtregelinterface Cross-Platform [in deze zelfstudie gelijkwaardig](key-vault-manage-with-cli.md).
>
>U kunt op dit moment Azure sleutel kluis in Azure portal niet configureren. Deze instructies Azure PowerShell gebruiken.

De logboeken die u verzamelt, kunnen met behulp van analytics van het Operations Management Suite logboek worden weergegeven. Zie [oplossing in logboek Analytics Azure sleutel kluis (voorbeeld)](../log-analytics/log-analytics-azure-key-vault.md)voor meer informatie.

Zie voor informatie over Azure sleutel kluis, [Wat is Azure sleutel kluis?](key-vault-whatis.md)

## <a name="prerequisites"></a>Vereisten

Deze zelfstudie hebt u het volgende:

- Een bestaande sleutel kluis die u hebt gebruikt.  
- Azure PowerShell **minimumversie van 1.0.1**. Azure PowerShell te installeren en te koppelen aan uw abonnement Azure, Zie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md). Als u Azure PowerShell al hebt geïnstalleerd en de versie, vanuit de console Azure PowerShell niet kent, typt u `(Get-Module azure -ListAvailable).Version`.  
- Voldoende opslagruimte op Azure voor uw sleutel kluis Logboeken.


## <a id="connect"></a>Verbinding maken met uw abonnementen ##

Een Azure PowerShell-sessie start en aanmelden bij uw account Azure met de volgende opdracht:  

    Login-AzureRmAccount

Voer uw Azure gebruikersnaam en wachtwoord in het pop-browservenster. Alle abonnementen die zijn gekoppeld aan deze account en standaard, gebruikt de eerste die krijgen Azure PowerShell.

Als u meerdere abonnementen hebt, is het mogelijk om op te geven van een specifieke naam die is gebruikt voor het maken van uw Azure sleutel kluis. Typ het volgende als u wilt zien van de abonnementen voor uw account:

    Get-AzureRmSubscription

Als u het abonnement dat is gekoppeld aan uw sleutel kluis die u meldt zich aan, typ:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Zie voor meer informatie over het configureren van Azure PowerShell [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).


## <a id="storage"></a>Maak een nieuwe account opslag voor uw logboeken ##

Hoewel u met een bestaande account voor de opslag voor uw Logboeken, maken we een nieuwe opslag-account die zullen worden toegewezen aan de sleutel kluis Logboeken. Voor het gemak voor wanneer hebben we dit later opgeven, slaan we de details in een variabele met de naam **sa**.

Voor extra gemak van beheer ook gebruikt dezelfde resourcegroep als met onze belangrijkste kluis. Deze groep met de naam **ContosoResourceGroup** uit de [handleiding aan de slag](key-vault-get-started.md)en zullen wij de vestiging Oost-Azië. Vervang deze waarden voor uw eigen, indien van toepassing:

    $sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup -Name ContosoKeyVaultLogs -Type Standard_LRS -Location 'East Asia'


>[AZURE.NOTE]  Als u een bestaande opslagruimte gebruikt, moet hiervoor het abonnement hetzelfde als uw sleutel kluis en het moet het implementatiemodel Resource Manager in plaats van het model Klassiek implementatie gebruiken.

## <a id="identify"></a>De sleutel kluis voor uw logboeken identificeren ##

In onze ophalen zelfstudie begonnen was onze naam sleutel kluis **ContosoKeyVault**, dus zullen wij met die naam en de gegevens opslaan in een variabele met de naam **kv**:

    $kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'


## <a id="enable"></a>Logboekregistratie inschakelen ##

Als logboekregistratie wilt inschakelen voor de sleutel kluis, gebruikt u de cmdlet Set-AzureRmDiagnosticSetting, samen met de variabelen die we voor onze nieuwe opslag en onze sleutel kluis gemaakt. We ook stelt u de **-ingeschakeld** markering **$true** en de categorie op AuditEvent (de enige categorie voor de sleutel kluis):


    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

De uitvoer omvat:

    StorageAccountId   : /subscriptions/<subscription-GUID>/resourceGroups/ContosoResourceGroup/providers/Microsoft.Storage/storageAccounts/ContosoKeyVaultLogs
    ServiceBusRuleId   :
    StorageAccountName :
        Logs
        Enabled           : True
        Category          : AuditEvent
        RetentionPolicy
        Enabled : False
        Days    : 0


Dit bevestigt de logboekregistratie is nu ingeschakeld voor uw sleutel kluis, informatie opslaan op uw account voor opslag.

U kunt desgewenst ook bewaarbeleid instellen voor uw Logboeken, dat oudere logboeken worden automatisch verwijderd. Bijvoorbeeld, met **RetentionEnabled -** vlag op **$true** bewaarbeleid instellen en **RetentionInDays -** parameter ingesteld op **90** zodat logboeken die ouder zijn dan 90 dagen worden automatisch verwijderd.

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent -RetentionEnabled $true -RetentionInDays 90

Wat wordt vastgelegd:

- Alle geverifieerde aanvragen voor REST-API worden vastgelegd, waaronder de mislukte aanvragen, toegangsmachtigingen, fouten in het bestandssysteem of de beschadigde aanvragen.
- Bewerkingen op de sleutel zelf, inclusief het maken, verwijderen, instelling sleutel kluis-beleid, vault en tags sleutel kluis kenmerken, zoals bijwerken.
- Bewerkingen op toetsen en geheimen in de sleutel kluis, inclusief het maken, wijzigen of verwijderen van deze sleutels of geheimen; bewerkingen als ondertekenen, controleren, coderen, decoderen, tekstomloop en pak sleutels, geheimen, lijst met sleutels en geheimen en hun versies.
- Niet-geverifieerde aanvragen die in een 401-respons resulteren. Bijvoorbeeld aanvragen die beschikt niet over een token aan toonder of hebben een ongeldige token ongeldig of verlopen.  


## <a id="access"></a>Toegang tot de logboeken ##

Sleutel kluis logboeken worden opgeslagen in de container **inzichten-logboeken-auditevent** in de opslag account die u hebt opgegeven. Als alle BLOB's in deze container, typt u:

    Get-AzureStorageBlob -Container 'insights-logs-auditevent' -Context $sa.Context

De uitvoer ziet er ongeveer zo ongeveer:

**Container-Uri: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent**


**Naam**

**----**

**resourceId = / ABONNEMENTEN/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json**

**resourceId = / ABONNEMENTEN/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json**

** resourceId = / ABONNEMENTEN/361DA5D4-A47A-4C 79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT. KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json***


Zoals u uit deze uitvoer zien kunt, de BLOB's gaat u als volgt een naamgevingsconventie: **resourceId =<ARM resource ID>/y =<year>/m =<month>/d =<day of month>/h =<hour>/m =<minute>/filename.json**

De datum- en tijdwaarden UTC gebruiken.

Omdat dezelfde account opslag kan worden gebruikt voor het verzamelen van Logboeken voor meerdere resources, is de volledige bron-ID in de naam van de blob erg handig voor toegang tot of downloaden van alleen de BLOB's die u nodig hebt. Maar voordat we dat doen, het downloaden van alle BLOB's eerst komen.

Maak eerst een map om de BLOB's downloaden. Bijvoorbeeld:

    New-Item -Path 'C:\Users\username\ContosoKeyVaultLogs' -ItemType Directory -Force

Vervolgens krijgt u een overzicht van alle BLOB's:  

    $blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context

Deze lijst via 'Get-AzureStorageBlobContent' voor het downloaden van de BLOB's in onze doelmap pipe:

    $blobs | Get-AzureStorageBlobContent -Destination 'C:\Users\username\ContosoKeyVaultLogs'

Tijdens het uitvoeren van deze tweede opdracht, de **/** als scheidingsteken in de blob-namen maken een volledige mapstructuur onder de doelmap en deze structuur wordt gebruikt om te downloaden en de BLOB's worden opgeslagen als bestanden.

BLOB's selectief downloaden, kunt u jokertekens gebruiken. Bijvoorbeeld:

- Meerdere sleutels kluizen als Logboeken voor één sleutel kluis downloaden, met de naam CONTOSOKEYVAULT3:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3

- Als er meerdere bronnengroepen en wilt u zich aanmeldt voor een resourcegroep te downloaden, gebruikt u `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'

- Als u downloaden van alle logboeken voor de maand januari 2016 wilt, gebruikt u `-Blob '*/year=2016/m=01/*'`:

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'

U bent nu klaar om te kijken wat er in de Logboeken starten. Maar voordat u naar die twee meer parameters voor Get-AzureRmDiagnosticSetting die u wilt weten:

- De status van diagnostische instellingen voor uw sleutel kluis resource opvragen:`Get-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId`

- Uitschakelen van logboekregistratie voor de sleutel kluis resource:`Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories AuditEvent`


## <a id="interpret"></a>De sleutel kluis logboekbestanden interpreteren ##

Afzonderlijke BLOB's worden opgeslagen als tekst opgemaakt als een blob JSON. Dit is een voorbeeld van de vermelding wordt uitgevoerd `Get-AzureRmKeyVault -VaultName 'contosokeyvault'`:

    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }


De volgende tabel bevat de namen en beschrijvingen.


| Veldnaam        | Beschrijving |
| ------------- |-------------|
| tijd      | Datum en tijd (UTC).|
| resourceId      | Azure Resource Manager Resource-ID. Voor logboeken sleutel kluis is dit altijd de sleutel kluis resource-ID.|
| operationName      | De naam van de bewerking, zoals beschreven in de volgende tabel.|
| operationVersion      | Dit is de REST API versie door de client is aangevraagd.|
| categorie      | Voor logboeken sleutel kluis is AuditEvent de waarde één, beschikbaar.|
| resultType      | Resultaat van de REST API.|
| resultSignature      | HTTP-status.|
| resultDescription     | Extra omschrijving van het resultaat, indien beschikbaar.|
| durationMs      | Benodigde tijd om de REST API-aanvraag in milliseconden. Dit omvat niet de netwerklatentie, zodat de tijd die u op de client meet mogelijk niet overeen met deze tijd.|
| callerIpAddress      | IP-adres van de client die de aanvraag hebben gedaan.|
| correlationId      | Een optionele GUID die de client doorgeven kunt voor de bijbehorende client-side logboeken met Logboeken voor service-side (sleutel kluis).|
| identiteit      | De identiteit van het token dat bij de REST API-aanvraag is ingediend. Dit is meestal een "gebruiker", 'service principal' of een combinatie "gebruiker + appId" in geval van een aanvraag die voortvloeien uit een Azure PowerShell-cmdlet.|
| Eigenschappen      | Dit veld bevat de verschillende gegevens op basis van de bewerking (operationName). In de meeste gevallen bevat informatie over de client (de useragent tekenreeks doorgegeven door de client), de exacte REST API aanvraag URI en de HTTP-statuscode. Ook wanneer een object wordt geretourneerd als gevolg van een aanvraag (bijvoorbeeld KeyCreate of VaultGet) bevat ook de URI van de sleutel (als "id"), kluis URI of geheim URI.|




De waarden voor de **operationName** hebben ObjectVerb indeling. Bijvoorbeeld:

- Alle bewerkingen van de sleutel kluis hebben de "Vault`<action>`' opmaken, zoals `VaultGet` en `VaultCreate`.

- Alle belangrijke bewerkingen hebben de ' sleutel`<action>`' opmaken, zoals `KeySign` en `KeyList`.

- Alle geheime bewerkingen hebben de ' geheim`<action>`' opmaken, zoals `SecretGet` en `SecretListVersions`.

De volgende tabel worden de operationName en de bijbehorende REST API-opdracht.

| operationName        | REST API-opdracht |
| ------------- |-------------|
| Verificatie      | Via Azure Active Directory-eindpunt|
| VaultGet      | [Informatie opvragen over een sleutel kluis](https://msdn.microsoft.com/en-us/library/azure/mt620026.aspx)|
| VaultPut      | [Maken of bijwerken van een sleutel kluis](https://msdn.microsoft.com/en-us/library/azure/mt620025.aspx)|
| VaultDelete      | [Verwijderen van een sleutel kluis](https://msdn.microsoft.com/en-us/library/azure/mt620022.aspx)|
| VaultPatch      | [Een sleutel kluis bijwerken](https://msdn.microsoft.com/library/azure/mt620025.aspx)|
| VaultList      | [Lijst van alle belangrijke kluizen in een resourcegroep](https://msdn.microsoft.com/en-us/library/azure/mt620027.aspx)|
| KeyCreate      | [Maak een sleutel](https://msdn.microsoft.com/en-us/library/azure/dn903634.aspx)|
| KeyGet      | [Informatie opvragen over een sleutel](https://msdn.microsoft.com/en-us/library/azure/dn878080.aspx)|
| KeyImport      | [Importeren van een sleutel in een kluis](https://msdn.microsoft.com/en-us/library/azure/dn903626.aspx)|
| KeyBackup      | [Back-up van een sleutel](https://msdn.microsoft.com/en-us/library/azure/dn878058.aspx).|
| KeyDelete      | [Een sleutel verwijderen](https://msdn.microsoft.com/en-us/library/azure/dn903611.aspx)|
| KeyRestore      | [Een sleutel herstellen](https://msdn.microsoft.com/en-us/library/azure/dn878106.aspx)|
| KeySign      | [Meld u met een sleutel](https://msdn.microsoft.com/en-us/library/azure/dn878096.aspx)|
| KeyVerify      | [Controleer of met een sleutel](https://msdn.microsoft.com/en-us/library/azure/dn878082.aspx)|
| KeyWrap      | [Een sleutel laten teruglopen](https://msdn.microsoft.com/en-us/library/azure/dn878066.aspx)|
| KeyUnwrap      | [Pak een sleutel](https://msdn.microsoft.com/en-us/library/azure/dn878079.aspx)|
| KeyEncrypt      | [Coderen met een sleutel](https://msdn.microsoft.com/en-us/library/azure/dn878060.aspx)|
| KeyDecrypt      | [Met een sleutel decoderen](https://msdn.microsoft.com/en-us/library/azure/dn878097.aspx)|
| KeyUpdate      | [Een sleutel wordt bijgewerkt](https://msdn.microsoft.com/en-us/library/azure/dn903616.aspx)|
| KeyList      | [Lijst van de sleutels in een kluis](https://msdn.microsoft.com/en-us/library/azure/dn903629.aspx)|
| KeyListVersions      | [De versies van een sleutel](https://msdn.microsoft.com/en-us/library/azure/dn986822.aspx)|
| SecretSet      | [Een geheim maken](https://msdn.microsoft.com/en-us/library/azure/dn903618.aspx)|
| SecretGet      | [Geheime ophalen](https://msdn.microsoft.com/en-us/library/azure/dn903633.aspx)|
| SecretUpdate      | [Update een geheim](https://msdn.microsoft.com/en-us/library/azure/dn986818.aspx)|
| SecretDelete      | [Een geheim verwijderen](https://msdn.microsoft.com/en-us/library/azure/dn903613.aspx)|
| SecretList      | [Lijst van geheimen in een kluis](https://msdn.microsoft.com/en-us/library/azure/dn903614.aspx)|
| SecretListVersions      | [Lijst met versies van een geheim](https://msdn.microsoft.com/en-us/library/azure/dn986824.aspx)|




## <a id="next"></a>Volgende stappen ##

Zie voor een zelfstudie met Azure sleutel kluis in een webtoepassing, [Met Azure sleutel kluis uit een webtoepassing](key-vault-use-from-web-application.md).

Zie [de Azure sleutel kluis developer's guide](key-vault-developers-guide.md)voor het programmeren van verwijzingen.

Zie voor een lijst met 1.0 van Azure PowerShell cmdlets voor Azure sleutel kluis, [Azure sleutel kluis Cmdlets](https://msdn.microsoft.com/library/azure/dn868052.aspx).

Zie voor een zelfstudie over het logboek controle met Azure sleutel kluis en key rotatie, [How to setup sleutel kluis met sleutel complete rotatie en controle](key-vault-key-rotation-log-monitoring.md).
