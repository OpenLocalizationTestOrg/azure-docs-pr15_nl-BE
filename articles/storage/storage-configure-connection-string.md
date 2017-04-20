<properties 
    pageTitle="Een verbindingsreeks voor Azure opslag configureren | Microsoft Azure"
    description="Een verbindingsreeks voor een account Azure opslag configureren. Een verbindingsreeks bevat de informatie die nodig is voor het verifiëren van verbindingen naar een opslag van de toepassing in runtime."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="configure-azure-storage-connection-strings"></a>Verbindingsreeksen Azure opslag configureren

## <a name="overview"></a>Overzicht

Een verbindingsreeks bevat de verificatiegegevens die nodig zijn voor toegang tot gegevens in een account Azure opslag van de toepassing in runtime. U kunt een verbindingsreeks te configureren:

- Verbinding maken met de emulator Azure opslag.
- Toegang tot de account van een opslag in Azure.
- Toegang tot de opgegeven resources in Azure via een gedeelde access-handtekening (SAS).

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a>De verbindingsreeks op te slaan

De toepassing moet toegang hebben tot de verbindingsreeks in runtime te verifiëren, aanvragen voor de opslag van Azure. U hebt een aantal verschillende opties voor het opslaan van de verbindingsreeks:

- Voor een toepassing op het bureaublad of op een apparaat, kunt u de verbindingsreeks in opslaan een `app.config `bestand of een `web.config` bestand. De verbindingsreeks toevoegen aan de sectie **AppSettings** .
- Voor een toepassing in een Azure cloud-service wordt uitgevoerd, kunt u de verbindingsreeks opslaan in het [bestand Azure service schema (.cscfg)](https://msdn.microsoft.com/library/ee758710.aspx). De verbindingsreeks toevoegen aan de sectie **ConfigurationSettings** van het configuratiebestand van de service.
- U kunt de verbindingsreeks ook direct in uw code gebruiken. Voor de meeste scenario's wordt echter aangeraden uw configuratietekenreeks op te slaan in een configuratiebestand.

De verbindingsreeks in een configuratiebestand opslaan, kunt gemakkelijk de verbindingsreeks om te schakelen tussen de emulator opslag en een account Azure opslag in de cloud werken. U hoeft alleen de verbindingsreeks om te verwijzen naar uw doelomgeving.

De klasse [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) kunt u toegang krijgen tot de verbindingsreeks in runtime, ongeacht waar de toepassing wordt uitgevoerd.

## <a name="create-a-connection-string-to-the-storage-emulator"></a>Een verbindingsreeks voor de emulator opslag maken

[AZURE.INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Zie [werken met de Emulator Azure opslag voor ontwikkeling en testen](storage-use-emulator.md) voor meer informatie over de opslag-emulator.

## <a name="create-a-connection-string-to-an-azure-storage-account"></a>Een verbindingsreeks voor een account Azure opslag maken

U maakt een verbindingstekenreeks aan uw account Azure opslag, met onderstaande indeling van de verbindingsreeks. Aangeven of u wilt verbinden met de opslag rekening via HTTPS (aanbevolen) of HTTP, vervangen `myAccountName` met de naam van uw account voor opslag en vervang `myAccountKey` met uw account toegang tot sleutel:

    DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey

Bijvoorbeeld, de verbindingsreeks ziet er vergelijkbaar met de volgende voorbeeld-verbindingsreeks:

    DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>

> [AZURE.NOTE] Azure opslag ondersteunt zowel HTTP en HTTPS in een verbindingsreeks; met behulp van HTTPS wordt echter sterk aanbevolen.

## <a name="create-a-connection-string-using-a-shared-access-signature"></a>Een verbindingsreeks met een gedeelde access-handtekening maken

[AZURE.INCLUDE [storage-use-sas-in-connection-string-include](../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="creating-a-connection-string-to-an-explicit-storage-endpoint"></a>Een verbindingsreeks voor een eindpunt expliciete opslag maken

U kunt de eindpunten expliciet in de verbindingsreeks in plaats van de Standaardeindpunten opgeven. Geef om een verbindingstekenreeks waarmee een expliciete eindpunt, de volledige service-eindpunt voor elke service, met inbegrip van de specificatie van het protocol (HTTPS (aanbevolen) of HTTP) in de volgende notatie:

    DefaultEndpointsProtocol=[http|https];
    BlobEndpoint=myBlobEndpoint;
    QueueEndpoint=myQueueEndpoint;
    TableEndpoint=myTableEndpoint;
    FileEndpoint=myFileEndpoint;
    AccountName=myAccountName;
    AccountKey=myAccountKey

Een scenario waar wilt u mogelijk een expliciete eindpunt hoeft op te geven is als u uw eindpunt Blob-opslag hebt toegewezen aan een aangepast domein. In dat geval kunt u uw aangepaste eindpunt voor Blob-opslag in de verbindingsreeks opgeven en eventueel de Standaardeindpunten voor de andere service opgeven als de toepassing worden gebruikt.

Hier vindt u voorbeelden van geldige verbindingsreeksen die een expliciete eindpunt voor de Blob-service opgeven:

    # Blob endpoint only
    DefaultEndpointsProtocol=https;
    BlobEndpoint=www.mydomain.com;
    AccountName=storagesample;
    AccountKey=account-key

    # All service endpoints
    DefaultEndpointsProtocol=https;
    BlobEndpoint=www.mydomain.com;
    FileEndpoint=myaccount.file.core.windows.net;
    QueueEndpoint=myaccount.queue.core.windows.net;
    TableEndpoint=myaccount;
    AccountName=storagesample;
    AccountKey=account-key

De endpoint-waarde die wordt vermeld in de verbindingsreeks wordt gebruikt om de aanvraag-URI's aan de Blob-service samen te stellen en het bepaalt de vorm van een URI's die worden geretourneerd aan de code.

Houd er rekening mee dat als u een service-eindpunt van de verbindingstekenreeks weglaten, klikt u niet zal kunnen deze verbindingsreeks gebruiken voor toegang tot gegevens in die service vanuit uw code.

### <a name="creating-a-connection-string-with-an-endpoint-suffix"></a>Een verbindingsreeks maken met een achtervoegsel eindpunt

Gebruik maken van een verbindingstekenreeks voor storage-service in regio's of exemplaren met achtervoegsels eindpunt, zoals voor Azure China of Azure Governance, indeling van de volgende verbindingsreeks. Aangeven of u wilt verbinding maken met de account opslag via HTTP of HTTPS, vervangen `myAccountName` vervangen door de naam van uw account voor opslag, `myAccountKey` met uw account toegangssleutel en vervangen `mySuffix` met het achtervoegsel URI:


    DefaultEndpointsProtocol=[http|https];
    AccountName=myAccountName;
    AccountKey=myAccountKey;
    EndpointSuffix=mySuffix;


Bijvoorbeeld, moet de verbindingsreeks uitzien naar de volgende verbindingsreeks:

    DefaultEndpointsProtocol=https;
    AccountName=storagesample;
    AccountKey=<account-key>;
    EndpointSuffix=core.chinacloudapi.cn;

## <a name="parsing-a-connection-string"></a>Parseren van een verbindingsreeks.

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]


## <a name="next-steps"></a>Volgende stappen

- [De Emulator Azure opslag gebruikt voor het ontwikkelen en testen](storage-use-emulator.md)
- [Azure opslag Explorers](storage-explorers.md)
- [Gebruik van gedeelde toegang handtekeningen (SAS)](storage-dotnet-shared-access-signature-part-1.md)
