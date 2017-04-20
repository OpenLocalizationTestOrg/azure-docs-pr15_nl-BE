<properties 
   pageTitle="DNS-zones maken en recordsets in Azure DNS met behulp van de SDK voor .NET | Microsoft Azure" 
   description="Het maken van DNS-zones en -recordsets in Azure DNS met behulp van de SDK voor .NET." 
   services="dns" 
   documentationCenter="na" 
   authors="jtuliani" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/19/2016"
   ms.author="jtuliani"/>


# <a name="create-dns-zones-and-record-sets-using-the-net-sdk"></a>DNS-zones en met de .NET SDK recordsets maken

U kunt bewerkingen maken, verwijderen of bijwerken van DNS-zones, recordsets en records met behulp van DNS SDK met bibliotheek voor DNS-beheer .NET automatiseren. Een volledige Visual Studio-project is beschikbaar [hier.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)

## <a name="create-a-service-principal-account"></a>Een service principal-account maken

Programmatische toegang tot bronnen Azure wordt meestal verleend via een speciale rekening in plaats van uw eigen referenties van de gebruiker. Deze speciale rekeningen worden accounts 'hoofd service' genoemd. Het voorbeeldproject Azure DNS-SDK gebruikt, moet u eerst een principal service-account maakt en deze op de juiste machtigingen toewijzen.

1. Volg [deze instructies](../resource-group-authenticate-service-principal.md) om het maken van een serviceaccount principal (het voorbeeldproject Azure DNS-SDK wordt ervan uitgegaan dat de verificatie op basis van een wachtwoord.)

2. Een resourcegroep maken ([is hier hoe](../azure-portal/resource-group-portal.md)).

3. Azure RBAC gebruikt de serviceaccount voor principal 'DNS-Zone Inzender' om machtigingen te verlenen aan de resourcegroep ([is hier hoe](../active-directory/role-based-access-control-configure.md).)

4. Bewerk het bestand 'program.cs' als volgt als het voorbeeldproject Azure DNS-SDK:
    * Plaats de juiste waarden voor de tenantId, clientId (ook bekend als account-ID), geheim (service principal wachtwoord) en subscriptionId, zoals gebruikt in stap 1.
    * Voer de naam van de resourcegroep hebt gekozen in stap 2.
    * Voer de naam van een DNS-zone van uw keuze.

## <a name="nuget-packages-and-namespace-declarations"></a>NuGet pakketten en naamruimtedeclaraties

Voor het gebruik van de .NET SDK Azure DNS, moet u het pakket **Azure DNS-beheer bibliotheek** NuGet en andere vereiste Azure pakketten installeren.
 
1. Open een project of een nieuw project in **Visual Studio**. 

2. Ga naar **Extra** **>** **NuGet Package Manager** **>** **NuGet pakketten voor oplossing... beheren**. 

3. Typ **Microsoft.Azure.Management.Dns** in het zoekvak klikt u op **Bladeren**en activeert u het vakje **opnemen voorlopige versie** .

4. Het pakket en klik op **installeren** om het toe te voegen aan het Visual Studio-project.
 
5. Herhaal de bovenstaande te installeren de volgende pakketten: **Microsoft.Rest.ClientRuntime.Azure.Authentication** en **Microsoft.Azure.Management.ResourceManager**.

## <a name="add-namespace-declarations"></a>Naamruimtedeclaraties toevoegen

De volgende naamruimtedeclaraties toevoegen

    using Microsoft.Rest.Azure.Authentication;
    using Microsoft.Azure.Management.Dns;
    using Microsoft.Azure.Management.Dns.Models;

## <a name="initialize-the-dns-management-client"></a>Initialiseren van de DNS-client management

De *DnsManagementClient* bevat de methoden en eigenschappen die nodig zijn voor het beheren van DNS-zones en -recordsets.  De volgende code zich aanmelden bij de belangrijkste service-account en een DnsManagementClient-object gemaakt.

    // Build the service credentials and DNS management client
    var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
    var dnsClient = new DnsManagementClient(serviceCreds);
    dnsClient.SubscriptionId = subscriptionId;

## <a name="create-or-update-a-dns-zone"></a>Maken of bijwerken van een DNS-zone

Als een DNS-zone maakt, eerst een "Zone"-object gemaakt om de DNS-zone parameters bevatten. Omdat DNS-zones zijn niet gekoppeld aan een bepaalde regio, is de locatie ingesteld op 'global'. In dit voorbeeld wordt ook een [Bronnenbeheerder Azure 'tag'](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) toegevoegd aan de zone.

De zone-object waarin de parameters voor de zone is werkelijk maken of bijwerken van de zone in DNS Azure, doorgegeven aan de methode *DnsManagementClient.Zones.CreateOrUpdateAsyc* .

>[AZURE.NOTE] DnsManagementClient ondersteunt drie werkmodi: synchrone ('CreateOrUpdate'), asynchrone ('CreateOrUpdateAsync'), of asynchroon met toegang tot het HTTP-antwoord (CreateOrUpdateWithHttpMessagesAsync).  U kunt een van deze modi, afhankelijk van de behoeften van uw toepassing.

Azure DNS ondersteunt optimistische gelijktijdigheid, [Etags](dns-getstarted-create-dnszone.md)genoemd. In dit voorbeeld geven ' * ' kop geeft voor de 'If-None-Match' Azure DNS een DNS-zone maken als dit nog niet bestaat.  De aanroep mislukt als een zone met de opgegeven naam al in de opgegeven resourcegroep bestaat.

    // Create zone parameters
    var dnsZoneParams = new Zone("global"); // All DNS zones must have location = "global"
    
    // Create a Azure Resource Manager 'tag'.  This is optional.  You can add multiple tags
    dnsZoneParams.Tags = new Dictionary<string, string>();
    dnsZoneParams.Tags.Add("dept", "finance");
    
    // Create the actual zone.
    // Note: Uses 'If-None-Match *' ETAG check, so will fail if the zone exists already.
    // Note: For non-async usage, call dnsClient.Zones.CreateOrUpdate(resourceGroupName, zoneName, dnsZoneParams, null, "*")
    // Note: For getting the http response, call dnsClient.Zones.CreateOrUpdateWithHttpMessagesAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*")
    var dnsZone = await dnsClient.Zones.CreateOrUpdateAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*");

## <a name="create-dns-record-sets-and-records"></a>DNS-record sets en records maken

DNS-records worden beheerd als een verzameling. Een verzameling is een verzameling records met dezelfde naam en type record binnen een zone.  De naam van de record is ten opzichte van de naam van de zone, niet de volledig gekwalificeerde DNS-naam.

Maken of bijwerken van een record ingesteld 'RecordSet' parameters-object wordt gemaakt en doorgegeven aan de *DnsManagementClient.RecordSets.CreateOrUpdateAsync*. Met DNS-zones, zijn er drie werkmodi: synchrone ('CreateOrUpdate'), asynchrone ('CreateOrUpdateAsync'), of asynchroon met toegang tot het HTTP-antwoord (CreateOrUpdateWithHttpMessagesAsync).

Net als bij DNS-zones, zijn de bewerkingen op recordsets ondersteuning voor optimistische gelijktijdigheid.  In dit voorbeeld omdat noch If-Match 'If-None-Match' worden opgegeven, wordt de verzameling altijd gemaakt.  Deze oproep overschrijft alle bestaande records in deze DNS-zone met dezelfde naam en het recordtype ingesteld.

    // Create record set parameters
    var recordSetParams = new RecordSet();
    recordSetParams.TTL = 3600;

    // Add records to the record set parameter object.  In this case, we'll add a record of type 'A'
    recordSetParams.ARecords = new List<ARecord>();
    recordSetParams.ARecords.Add(new ARecord("1.2.3.4"));

    // Add metadata to the record set.  Similar to Azure Resource Manager tags, this is optional and you can add multiple metadata name/value pairs
    recordSetParams.Metadata = new Dictionary<string, string>();
    recordSetParams.Metadata.Add("user", "Mary");

    // Create the actual record set in Azure DNS
    // Note: no ETAG checks specified, will overwrite existing record set if one exists
    var recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSetParams);

## <a name="get-zones-and-record-sets"></a>Zones en recordsets ophalen

De methoden *DnsManagementClient.Zones.Get* en *DnsManagementClient.RecordSets.Get* ophalen van afzonderlijke zones en recordsets, respectievelijk. RecordSets worden geïdentificeerd door hun type, naam en de zone- en groep aanwezig zijn in. Zones worden aangeduid met hun naam en de resourcegroep die aanwezig zijn in.

    var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
    
## <a name="update-an-existing-record-set"></a>Een bestaande set records bijwerken

Als het bijwerken van een bestaande DNS-record set, eerst de recordreeks ophalen en bijwerken van de inhoud van de recordset, vervolgens de wijziging indienen.  In dit voorbeeld geeft u de Etag uit de set opgehaalde record in de parameter If-Match. De aanroep mislukt als de recordset in de tussentijd is gewijzigd door een gelijktijdige bewerking.

    var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

    // Add a new record to the local object.  Note that records in a record set must be unique/distinct
    recordSet.ARecords.Add(new ARecord("5.6.7.8"));

    // Update the record set in Azure DNS
    // Note: ETAG check specified, update will be rejected if the record set has changed in the meantime
    recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);

## <a name="list-zones-and-record-sets"></a>Lijst zones en recordsets

Lijst van de zones, gebruikt u de methoden *DnsManagementClient.Zones.List...* , die ondersteuning bieden voor alle zones in een bepaalde groep óf alle zones in een bepaald abonnement Azure aanbieding (in verschillende resourcegroepen.) *... DnsManagementClient.RecordSets.List* methoden, die ondersteuning bieden voor een lijst met alle recordsets in een bepaalde zone of alleen die record sets van een bepaald type gebruiken om recordsets op te geven.

Let op bij het aanbieden van zones en recordsets die het resultaat kunnen worden gepagineerd.  In het volgende voorbeeld ziet u hoe u door de pagina's van de resultaten. (Een kunstmatig klein paginaformaat van de '2' wordt gebruikt om te forceren de paginering; in de praktijk deze parameter moet worden weggelaten en de standaardgrootte van de pagina worden gebruikt.)

    // Note: in this demo, we'll use a very small page size (2 record sets) to demonstrate paging
    // In practice, to improve performance you would use a large page size or just use the system default
    int recordSets = 0;
    var page = await dnsClient.RecordSets.ListAllInResourceGroupAsync(resourceGroupName, zoneName, "2");
    recordSets += page.Count();

    while (page.NextPageLink != null)
    {
        page = await dnsClient.RecordSets.ListAllInResourceGroupNextAsync(page.NextPageLink);
        recordSets += page.Count();
    }

## <a name="next-steps"></a>Volgende stappen

Download het [voorbeeldproject Azure DNS .NET SDK](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), inclusief verdere voorbeelden van het gebruik van Azure DNS .NET SDK, inclusief voorbeelden voor andere DNS-recordtypen.
