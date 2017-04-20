<properties 
    pageTitle="Azure Media serviceaccounts met PowerShell beheren" 
    description="Informatie over het beheren van accounts met PowerShell-cmdlets Azure Media Services." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016"
    ms.author="juliako"/>


#<a name="manage-azure-media-services-accounts-with-powershell"></a>Azure Media serviceaccounts met PowerShell beheren

> [AZURE.SELECTOR]
- [Portal](media-services-portal-create-account.md)
- [PowerShell](media-services-manage-with-powershell.md)
- [REST](http://msdn.microsoft.com/library/azure/dn194267.aspx)

> [AZURE.NOTE] Als u een Azure Media Services-account te maken, moet u een Azure account hebben. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Gratis proefperiode van Azure</a>.

##<a name="overview"></a>Overzicht 

In dit artikel worden de Azure PowerShell-cmdlets voor Azure Media Services (AMS) in het kader van Azure Resource Manager. De cmdlets bestaan in de naamruimte **Microsoft.Azure.Commands.Media** .

## <a name="versions"></a>Versies

**ApiVersion**: '01-10-2015'
               

## <a name="new-azurermmediaservice"></a>Nieuwe AzureRmMediaService

Hiermee maakt u een media-service.

### <a name="syntax"></a>Syntaxis

Parameter is ingesteld: StorageAccountIdParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccountId] <string> [-Tags <hashtable>]  [<CommonParameters>]

Parameter is ingesteld: StorageAccountsParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccounts] <PSStorageAccount[]> [-Tags <hashtable>]  [<CommonParameters>]

### <a name="parameters"></a>Parameters

**ResourceGroupName - &lt;String&gt;**

Hiermee geeft u de naam van de groep waartoe deze service media behoort.

Aliassen | geen
---|---
Vereist?   |  True
Positie?   |  0
Standaardwaarde |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Jokertekens accepteren?  |False

**-Accountnaam &lt;String&gt;**

Hiermee geeft u de naam van de media-service.

Aliassen |Naam
---|---
Vereist? |True
Positie? |1
Standaardwaarde |geen
Invoer pijplijn accepteren? |False
Jokertekens accepteren? |False

**-Locatie &lt;String&gt;**

Hiermee geeft u de locatie van de bron van de media-service.

Aliassen |geen
---|---
Vereist? |True
Positie? |2
Standaardwaarde  |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Jokertekens accepteren? |False

**StorageAccountId - &lt;String&gt;**

Hiermee geeft u een primaire opslag-account die is gekoppeld aan de media-service.

- Nieuwe opslag-account (gemaakt met de API voor bronbeheer) alleen ondersteund.

- De opslag moet bestaan en heeft dezelfde locatie met de media-service.

Aliassen |geen
---|---
Vereist? |True
Positie? |3
Standaardwaarde  |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Parameternaam |StorageAccountIdParamSet
Jokertekens accepteren?|False

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Hiermee geeft u een opslag-accounts die zijn gekoppeld aan de media-service.

- Nieuwe opslag-account (gemaakt met de API voor bronbeheer) alleen ondersteund.

- De opslag moet bestaan en heeft dezelfde locatie met de media-service.

- Slechts één account van opslag kan als primair worden opgegeven.

Aliassen |geen
---|---
Vereist?  |True
Positie?  |3
Standaardwaarde |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Parameternaam |StorageAccountsParamSet
Jokertekens accepteren? |False

**-Codes &lt;hash-tabel&gt;**

Hiermee geeft u een hash-tabel van de codes die gekoppeld aan de media-service zijn.

- Voorbeeld:@{"tag1"="value1";"tag2"=:value2"}

Aliassen |geen
---|---
Vereist?  |False
Positie?  |met de naam
Standaardwaarde |geen
Invoer pijplijn accepteren? |False
Jokertekens accepteren? |False

**&lt;Opdrachtparameters&gt;**

Deze cmdlet ondersteund de algemene parameters:-Debug - ErrorAction, ErrorVariable-, InformationAction-, InformationVariable-, - OutVariable ondersteund,-OutBuffer, PipelineVariable-, - Verbose, - WarningAction, en -WarningVariable.

### <a name="inputs"></a>Ingangen

Het invoertype is het type van de objecten die u aan de cmdlet kunt aanvoeren.

### <a name="outputs"></a>Uitgangen

Het uitvoertype is het type van de objecten die de cmdlet uitvoert.

## <a name="set-azurermmediaservice"></a>Set AzureRmMediaService

Media-service bijgewerkt.

### <a name="syntax"></a>Syntaxis

    Set-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Tags <hashtable>] [-StorageAccounts <PSStorageAccount[]>]  [<CommonParameters>]

### <a name="parameters"></a>Parameters

**ResourceGroupName - &lt;String&gt;**

Hiermee geeft u de naam van de groep waartoe deze service media behoort.

Aliassen |geen
---|---
Vereist?  |True
Positie?  |0
Standaardwaarde |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Jokertekens accepteren? |False

**-Accountnaam &lt;String&gt;**

Hiermee geeft u de naam van de media-service.

Aliassen |Naam
---|---
Vereist? |True
Positie? |1
Standaardwaarde |Geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Jokertekens accepteren? |False

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Hiermee geeft u een opslag-accounts die zijn gekoppeld aan de media-service.

- Nieuwe opslag-account (gemaakt met de API voor bronbeheer) alleen ondersteund.

- De opslag moet bestaan en heeft dezelfde locatie met de media-service.

- Slechts één account van opslag kan als primair worden opgegeven.

Aliassen |geen
---|---
Vereist? |False
Positie? |Met de naam
Standaardwaarde |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Parameternaam |StorageAccountsParamSet
Jokertekens accepteren? |False

**-Codes &lt;hash-tabel&gt;**

Hiermee geeft u een hash-tabel van de codes die gekoppeld aan deze media service zijn.

- De codes die gekoppeld aan de media-service zijn worden vervangen door de waarde die is opgegeven door de klant.

Aliassen |geen
---|---
Vereist? |False
Positie?  |Met de naam
Standaardwaarde |Geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Jokertekens accepteren? |False

**&lt;Opdrachtparameters&gt;**

Deze cmdlet ondersteund de algemene parameters:-Debug - ErrorAction, ErrorVariable-, InformationAction-, InformationVariable-, - OutVariable ondersteund,-OutBuffer, PipelineVariable-, - Verbose, - WarningAction, en -WarningVariable.

### <a name="inputs"></a>Ingangen

Het invoertype is het type van de objecten die u aan de cmdlet kunt aanvoeren.

### <a name="outputs"></a>Uitgangen

Het uitvoertype is het type van de objecten die de cmdlet uitvoert.

## <a name="remove-azurermmediaservice"></a>Verwijderen AzureRmMediaService

Hiermee verwijdert u een media-service.

### <a name="syntax"></a>Syntaxis

    Remove-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parameters

**ResourceGroupName - &lt;String&gt;**

Hiermee geeft u de naam van de groep waartoe deze service media behoort.

Aliassen |geen
---|---
Vereist? |True
Positie? |0
Standaardwaarde |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Jokertekens accepteren? |False

**-Accountnaam &lt;String&gt;**

Hiermee geeft u de naam van de media-service.

Aliassen |geen
---|---
Vereist? |True
Positie? |2
Standaardwaarde |Geen
Invoer pijplijn accepteren?  |True(ByPropertyName)
Jokertekens accepteren? |False

**&lt;Opdrachtparameters&gt;**

Deze cmdlet ondersteund de algemene parameters:-Debug - ErrorAction, ErrorVariable-, InformationAction-, InformationVariable-, - OutVariable ondersteund,-OutBuffer, PipelineVariable-, - Verbose, - WarningAction, en -WarningVariable.

### <a name="inputs"></a>Ingangen

Het invoertype is het type van de objecten die u aan de cmdlet kunt aanvoeren.

### <a name="outputs"></a>Uitgangen

Het uitvoertype is het type van de objecten die de cmdlet uitvoert.

## <a name="get-azurermmediaservice"></a>Get-AzureRmMediaService

Alle media-services in een resourcegroep of een media-service met een opgegeven naam opgehaald.

### <a name="syntax"></a>Syntaxis

ParameterSet: ResourceGroupParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string>  [<CommonParameters>] 

ParameterSet: AccountNameParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parameters

**ResourceGroupName - &lt;String&gt;**

Hiermee geeft u de naam van de groep waartoe deze service media behoort.

Aliassen |geen
---|---
Vereist? |True
Positie?  |0
Standaardwaarde |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Parameternaam |ResourceGroupParameterSet, AccountNameParameterSet
Jokertekens accepteren?   False

**-Accountnaam &lt;String&gt;**

Hiermee geeft u de naam van de media-service.

Aliassen |geen
---|---
Vereist? |True
Positie?  |1
Standaardwaarde |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Parameternaam  |AccountNameParameterSet
Jokertekens accepteren? |False

**&lt;Opdrachtparameters&gt;**

Deze cmdlet ondersteund de algemene parameters:-Debug - ErrorAction, ErrorVariable-, InformationAction-, InformationVariable-, - OutVariable ondersteund,-OutBuffer, PipelineVariable-, - Verbose, - WarningAction, en -WarningVariable.

### <a name="inputs"></a>Ingangen

Het invoertype is het type van de objecten die u aan de cmdlet kunt aanvoeren.

### <a name="outputs"></a>Uitgangen

Het uitvoertype is het type van de objecten die de cmdlet uitvoert.

## <a name="get-azurermmediaservicekeys"></a>Get-AzureRmMediaServiceKeys

Deze eigenschap haalt sleutels van media-service.

### <a name="syntax"></a>Syntaxis

    Get-AzureRmMediaServiceKeys [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parameters

**ResourceGroupName - &lt;String&gt;**

Hiermee geeft u de naam van de groep waartoe deze service media behoort.

Aliassen |geen
---|---
Vereist? |True
Positie?  |0
Standaardwaarde |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Jokertekens accepteren? |False

**-Accountnaam &lt;String&gt;**

Hiermee geeft u de naam van de media-service.

Aliassen |geen
---|---
Vereist? |True
Positie? |1
Standaardwaarde |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Jokertekens accepteren? |False

**&lt;Opdrachtparameters&gt;**

Deze cmdlet ondersteund de algemene parameters:-Debug - ErrorAction, ErrorVariable-, InformationAction-, InformationVariable-, - OutVariable ondersteund,-OutBuffer, PipelineVariable-, - Verbose, - WarningAction, en -WarningVariable.

### <a name="inputs"></a>Ingangen

Het invoertype is het type van de objecten die u aan de cmdlet kunt aanvoeren.

### <a name="outputs"></a>Uitgangen

Het uitvoertype is het type van de objecten die de cmdlet uitvoert.

## <a name="set-azurermmediaservicekey"></a>Set AzureRmMediaServiceKey

Genereert een primaire of secundaire sleutel van een media-service opnieuw.

### <a name="syntax"></a>Syntaxis

    Set-AzureRmMediaServiceKey [-ResourceGroupName] <string> [-AccountName] <string> [-KeyType] <KeyType> {Primary | Secondary}  [<CommonParameters>]

### <a name="parameters"></a>Parameters

**ResourceGroupName - &lt;String&gt;**

Hiermee geeft u de naam van de groep waartoe deze service media behoort.

Aliassen |geen
---|---
Vereist?  |True
Positie?  |0
Standaardwaarde |geen
Invoer pijplijn accepteren?  |True(ByPropertyName)
Jokertekens accepteren? |False

**-Accountnaam &lt;String&gt;**

Hiermee geeft u de naam van de media-service.

Aliassen |geen
---|---
Vereist? |True
Positie?  |1
Standaardwaarde |geen
Invoer pijplijn accepteren?   |True(ByPropertyName)
Jokertekens accepteren? |False

**KeyType - &lt;KeyType&gt;**

Hiermee geeft u het type sleutel van de media-service.

- Primair of secundair

Aliassen |geen
---|---
Vereist?  |True
Positie?  |2
Standaardwaarde |geen
Invoer pijplijn accepteren? |False
Jokertekens accepteren? |False

**&lt;Opdrachtparameters&gt;**

Deze cmdlet ondersteund de algemene parameters:-Debug - ErrorAction, ErrorVariable-, InformationAction-, InformationVariable-, - OutVariable ondersteund,-OutBuffer, PipelineVariable-, - Verbose, - WarningAction, en -WarningVariable.

### <a name="inputs"></a>Ingangen

Het invoertype is het type van de objecten die u aan de cmdlet kunt aanvoeren.

### <a name="outputs"></a>Uitgangen

Het uitvoertype is het type van de objecten die de cmdlet uitvoert.

## <a name="sync-azurermmediaservicestoragekeys"></a>Sync-AzureRmMediaServiceStorageKeys

Opslag account sleutels voor een opslag die is gekoppeld aan de media-service synchroniseert.

### <a name="syntax"></a>Syntaxis

    Sync-AzureRmMediaServiceStorageKeys [-ResourceGroupName] <string> [-MediaServiceAccountName] <string>    [-StorageAccountId] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parameters

**ResourceGroupName - &lt;String&gt;**

Hiermee geeft u de naam van de groep waartoe deze service media behoort.

Aliassen |geen
---|---
Vereist? |True
Positie? |0
Standaardwaarde |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Jokertekens accepteren? |False

**-Accountnaam &lt;String&gt;**

Hiermee geeft u de naam van de media-service.

Aliassen |geen
---|---
Vereist? |True
Positie? |1
Standaardwaarde |geen
Invoer pijplijn accepteren? |True(ByPropertyName)
Jokertekens accepteren? |False

**StorageAccountId - &lt;String&gt;**

Hiermee geeft u de opslag-account die is gekoppeld aan de media-service.

Aliassen |ID
---|---
Vereist? |True
Positie?  |2
Standaardwaarde |geen
Invoer pijplijn accepteren? |      True(ByPropertyName)
Jokertekens accepteren? |False

**&lt;Opdrachtparameters&gt;**

Deze cmdlet ondersteund de algemene parameters:-Debug - ErrorAction, ErrorVariable-, InformationAction-, InformationVariable-, - OutVariable ondersteund,-OutBuffer, PipelineVariable-, - Verbose, - WarningAction, en -WarningVariable.

### <a name="inputs"></a>Ingangen

Het invoertype is het type van de objecten die u aan de cmdlet kunt aanvoeren.

### <a name="outputs"></a>Uitgangen

Het uitvoertype is het type van de objecten die de cmdlet uitvoert.

## <a name="next-step"></a>Volgende stap 

Bekijk Media Services paden te leren.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
