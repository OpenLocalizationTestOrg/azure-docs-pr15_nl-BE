<properties
   pageTitle="Belangrijke releaseopmerkingen kluis .NET 2.x API | Microsoft Azure"
   description=".NET ontwikkelaars wordt deze API code gebruikt voor Azure sleutel kluis"
   services="key-vault"
   documentationCenter=""
   authors="BrucePerlerMS"
   manager="mbaldwin"
   editor="bruceper" />
<tags
   ms.service="key-vault"
   ms.devlang="CSharp"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/07/2016"
   ms.author="bruceper" />

# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Azure sleutel kluis .NET 2.0 - Release-opmerkingen en gids

De volgende richtlijnen en notities zijn voor ontwikkelaars die werken met de Azure sleutel kluis .NET / C#-bibliotheek. In de wijziging van versie 1.0 naar versie 2.0, zijn een aantal updates aangebracht die wordt vereist migratie werk in uw code in om te kunnen profiteren van de functionele verbeteringen en toevoegingen als sleutel kluis certificaten ondersteuning hebben.

Sleutel kluis certificaten ondersteuning biedt voor het beheer van de x509 certificaten en de volgende problemen:  

-   Hiermee kan een certificaathouder een certificaat via een proces voor het maken van een kluis met sleutel of het importeren van een bestaand certificaat maken. Dit omvat zowel de zelf-ondertekend en certificeringsinstantie certificaten gegenereerd.

- Hiermee kunt u de eigenaar van een sleutel kluis certificaat voor het implementeren van beveiligde opslag en het beheer van X509 certificaten zonder interactie met de persoonlijke sleutel materiaal.  

-   Kan de eigenaar van een certificaat voor het maken van een beleid dat zorgt ervoor dat de sleutel kluis voor het beheren van de levenscyclus van een certificaat.  

-   Kunnen eigenaars certificaat om contactgegevens voor meldingen over gebeurtenissen van de levenscyclus van vervaldatum en vernieuwing van het certificaat.  

-   Ondersteunt automatische verlenging met geselecteerde emittenten - sleutel kluis partner X509 certificaat providers / certificeringsinstanties.
    - Opmerking - providers/diensten niet samen zijn eveneens toegestaan, maar geen ondersteuning voor de functie voor automatisch vernieuwen.


## <a name="net-support"></a>.NET-ondersteuning
- **.NET 4.0** wordt niet ondersteund door versie 2.0 van de Azure sleutel kluis .NET / C#-bibliotheek
- **.NET-core** wordt ondersteund door de versie 2.0 van Azure sleutel kluis .NET / C#-bibliotheek

## <a name="namespaces"></a>Naamruimten
- De naamruimte voor **modellen** van **Microsoft.Azure.KeyVault** gewijzigd in **Microsoft.Azure.KeyVault.Models**.
- De naamruimte **Microsoft.Azure.KeyVault.Internal** is weggehaald.
- De naamruimte Azure SDK afhankelijkheden van **Hyak.Common** en **Hyak.Common.Internals** gewijzigd in **Microsoft.Rest** en **Microsoft.Rest.Serialization**


## <a name="type-changes"></a>Type wijzigingen
- *Geheim* is gewijzigd in *SecretBundle*
- *Woordenlijst* gewijzigd in *IDictionary*
- *Lijst<T>, string []* gewijzigd in *IList<T> *
- *NextList* gewijzigd in *NextPageLink*


## <a name="return-types"></a>Retourtypen
- **KeyList** en **SecretList** *IPage<T> * in plaats van *ListKeysResponseMessage*
- De gegenereerde **BackupKeyAsync** *BackupKeyResult* met *waarde* (blob back-up) als resultaat. Voordat u de methode is verpakt en alleen de waarde als resultaat geven.

## <a name="exceptions"></a>Uitzonderingen
- *KeyVaultClientException* wordt gewijzigd in *KeyVaultErrorException*
- Fout in de service is gewijzigd van de uitzondering *. Fout* met uitzondering van *. Body.Error.Message*.
- Extra info verwijderd uit het foutbericht voor **[JsonExtensionData]**.

## <a name="constructors"></a>Constructors
- In plaats van een *HttpClient* als constructor-argument accepteert, accepteert de constructor alleen *HttpClientHandler* of *DelegatingHandler []*.



## <a name="downloaded-packages"></a>Gedownloade pakketten  
Wanneer een client een afhankelijkheid van een sleutel kluis verwerkt zijn de volgende gedownload
#### <a name="previous-package-list"></a>Vorige pakketlijst
- versie id="Hyak.Common van pakket" = "1.0.2" targetFramework = "net45"
- versie id="Microsoft.Azure.Common van pakket" = "2.0.4" targetFramework = "net45"
- versie id="Microsoft.Azure.Common.Dependencies van pakket" = "1.0.0" targetFramework = "net45"
- versie id="Microsoft.Azure.KeyVault van pakket" = "1.0.0" targetFramework = "net45"
- versie id="Microsoft.Bcl van pakket" = "1.1.9" targetFramework = "net45"
- versie id="Microsoft.Bcl.Async van pakket" = "1.0.168" targetFramework = "net45"
- versie id="Microsoft.Bcl.Build van pakket" = "1.0.14" targetFramework = "net45"
- versie id="Microsoft.Net.Http van pakket" = "2.2.22" targetFramework = "net45"

#### <a name="current-package-list"></a>Huidige pakketlijst
- versie id="Microsoft.Azure.KeyVault van pakket" = "2.0.0-preview" targetFramework = "net45"
- versie id="Microsoft.Rest.ClientRuntime van pakket" = "2.2.0" targetFramework = "net45"
- versie id="Microsoft.Rest.ClientRuntime.Azure van pakket" = "3.2.0" targetFramework = "net45"


## <a name="class-changes"></a>Klasse verandert

- Klasse **UnixEpoch** is verwijderd.
- Klasse **Base64UrlConverter** is gewijzigd in **Base64UrlJsonConverter**

## <a name="other-changes"></a>Andere wijzigingen

- Ondersteuning voor de configuratie van beleid voor KV bewerking opnieuw op fouten van voorbijgaande aard is toegevoegd aan deze versie van de API.



## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault NuGet
- Voor de bewerkingen die een *kluis*wordt geretourneerd, is het retourtype een klasse die de eigenschap van een kluis. Het retourtype is nu een *kluis*.
- *PermissionsToKeys* en *PermissionsToSecrets* zijn nu *Permissions.Keys* en *Permissions.Secrets*
- Sommige van de afzender typen wijzigingen toepassen op het ook control-plane.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions NuGet
- Het pakket is opgesplitst naar **Microsoft.Azure.KeyVault.Extensions** en **Microsoft.Azure.KeyVault.Cryptography** voor de bewerkingen van cryptografie.
