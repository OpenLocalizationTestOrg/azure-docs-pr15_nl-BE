<properties
   pageTitle="Aanbevolen voor naamgeving van Azure resources | Richtlijnen | Microsoft Azure"
   description="Aanbevolen voor naamgeving van Azure bronnen. Het benoemen van virtuele machines, rekeningen voor opslag, netwerken, virtuele netwerken, subnetten en andere Azure diensten"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/27/2016"
   ms.author="christb"/>
   
# <a name="recommended-naming-conventions-for-azure-resources"></a>Aanbevolen voor naamgeving van Azure resources

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

De keuze van een naam voor de resource in Microsoft Azure is belangrijk omdat:

- Is het moeilijk om een naam later wijzigen.
- Namen moeten voldoen aan de eisen van hun specifiek brontype.

Consistente naamgeving gemakkelijker bronnen vinden. Ze kunnen ook geven aan de rol van een resource in een oplossing.

Dit artikel is een samenvatting van de regels voor naamgeving en beperkingen voor Azure bronnen en een basislijn reeks aanbevelingen voor de naamgeving.  U kunt deze aanbevelingen als uitgangspunt voor uw eigen specifieke overeenkomsten op uw behoeften.  

De sleutel tot succes met naamgevingsregels is tot stand te brengen en ze volgen in uw toepassingen en organisaties. 

## <a name="naming-subscriptions"></a>Naamgeving van abonnementen

Bij het benoemen van Azure abonnementen maken uitgebreide namen begrijpen van de context en doel van elk abonnement wissen.  Als u werkt in een omgeving met veel abonnementen, na een gedeelde naamgevingsconventie duidelijkheid te kan verbeteren.

Er is een aanbevolen patroon voor naamgeving abonnementen:

`<Company> <Department (optional)> <Product Line (optional)> <Environment>`

- Bedrijf is meestal hetzelfde voor elk abonnement. Sommige bedrijven hebben echter onderliggende bedrijven binnen de organisatiestructuur. Deze bedrijven kunnen worden beheerd door een centrale IT-groep. In deze gevallen kunnen zij onderscheiden door de naam van moederbedrijf (*Contoso*) en de onderliggende bedrijfsnaam (*North Wind*).

- Afdeling is een naam binnen de organisatie waar een groep personen werken. Dit item binnen de naamruimte als optioneel.

- Productlijn is specifiek voor een product of een functie die wordt uitgevoerd vanuit binnen de afdeling.
Dit is meestal optioneel voor interne services en toepassingen. Het is echter nadrukkelijk aanbevolen voor openbare diensten waarvoor eenvoudig scheiding en identificatie (bijvoorbeeld voor een duidelijke scheiding van de facturering records).

- Omgeving is de naam van de beschrijving van de levenscyclus van de implementatie van toepassingen of services, zoals Dev, QA of Prod.

| Bedrijf | Afdeling | Regel voor product of Service | Omgeving | Volledige naam  |
----------| ---------- | ----------------------- | ----------- | ---------- |
| Contoso | SocialGaming | AwesomeService | Productie | Contoso SocialGaming AwesomeService productie |
| Contoso | SocialGaming | AwesomeService | Dev | Contoso SocialGaming AwesomeService-Dev |
| Contoso | DEZE | InternalApps | Productie | IT Contoso InternalApps productie |
| Contoso | DEZE | InternalApps | Dev | Contoso IT InternalApps-Dev |

<!-- TODO; include more information about organizing subscriptions for application deployment, pods, etc. -->

## <a name="use-affixes-to-avoid-ambiguity"></a>Voor-en achtervoegsel gebruikt om dubbelzinnigheid te vermijden

Bij het benoemen van bronnen in Azure, wenselijk is algemene voor- of achtervoegsels te gebruiken ter identificatie van het type en de context van de resource.  Terwijl de gegevens over het type, metagegevens, context, is beschikbaar via de programmacode, visuele identificatie toe te passen gemeenschappelijke en achtervoegsel vereenvoudigt.  Bij het opnemen van voor-en achtervoegsel in de naamgevingsconventie, is het belangrijk om duidelijk aan te geven of het voorvoegsel aan het begin van de naam (voorvoegsel) of aan het einde (achtervoegsel wordt).  

Dit zijn bijvoorbeeld twee verschillende namen voor een service die als host fungeert voor een berekening-engine:

- SvcCalculationEngine (prefix)
- CalculationEngineSvc (achtervoegsel)

Voor-en achtervoegsel kunnen verwijzen naar verschillende aspecten die bepaalde resources te beschrijven. De volgende tabel ziet u enkele voorbeelden die meestal wordt gebruikt.

| Aspect | Voorbeeld | Notities |
| ------ | ------- | ----- |
| Omgeving | dev, prod, QA | Identificeert de omgeving voor de resource |
| Locatie | uw (ons West), ue (US East) | Geeft de regio waarin de resource wordt geïmplementeerd. |
| Exemplaar | 01, 02 | Voor bronnen die meer dan een benoemd exemplaar (web-servers, enz.) hebben. |
| Product of Service | Service | Identificatie van het product, de toepassing of service die de bron wordt ondersteund |
| Rol | SQL, web-, messaging | De rol van de bijbehorende resource identificeert |

Bij het ontwikkelen van een bepaalde naamgevingsconventie voor uw bedrijf of projecten is belangrijker is een gemeenschappelijke set van voor-en achtervoegsel en hun positie (achtervoegsel of voorvoegsel) kiezen.

## <a name="naming-rules-and-restrictions"></a>Naamgevingsregels en beperkingen

Elk type bron of service in Azure afdwingt reeks naamgeving beperkingen en bereik; de naamgevingsregel of patroon moet voldoen aan de vereiste regels voor naamgeving en de reikwijdte.  Bijvoorbeeld, terwijl de naam van een VM wordt toegewezen aan een DNS-naam (en dus moet uniek zijn voor alle Azure), de naam van een VNET is binnen het bereik van de resourcegroep die wordt gemaakt in.

In het algemeen hoeft geen speciale tekens (`-` of `_`) als eerste of laatste teken in een naam. Deze tekens worden de meeste regels voor gegevensvalidatie mislukt.

| Categorie | Service of entiteit | Toepassingsgebied | Lengte | Behuizing | Geldige tekens | Voorgestelde patroon | Voorbeeld |
| ------------- | ----------------- | ----- | ------ | ------ | ---------------- | ----------------- | ------- |
| Resourcegroep | Resourcegroep | Globale | 1-64 | Hoofdlettergevoelig | Alfanumeriek, onderstrepingstekens en koppelteken | `<service short name>-<environment>-rg` | `profx-prod-rg` |
| Resourcegroep | Beschikbaarheid instellen | Resourcegroep | 1-80 | Hoofdlettergevoelig | Alfanumeriek, onderstrepingstekens en koppelteken | `<service-short-name>-<context>-as` | `profx-sql-as` |
| Algemeen | Label | Gekoppelde entiteit | 512 (naam), 256 (waarde) | Hoofdlettergevoelig | Alfanumerieke | `"key" : "value"` | `"department" : "Central IT"` |
| Berekenen | Virtuele Machine | Resourcegroep | 1-15 | Hoofdlettergevoelig | Alfanumeriek, onderstrepingstekens en koppelteken | `<name>-<role>-<instance>` | `profx-sql-001` |
| Opslag | Opslag-accountnaam (gegevens) | Globale | 3-24 | Kleine letters | Alfanumerieke | `<service short name><type><number>` | `profxdata001` |
| Opslag | Opslag-accountnaam (schijven) | Globale | 3-24 | Kleine letters | Alfanumerieke | `<vm name without dashes>st<number>` | `profxsql001st0` |
| Opslag | Naam van container | Opslag account | 3-63 |   Kleine letters | Alfanumeriek en -streepje | `<context>` | `logs` |
| Opslag | BLOB-naam | Container | 1-1024 | Hoofdletters en kleine letters | Elke URL char | `<variable based on blob usage>` | `<variable based on blob usage>` |
| Opslag | Wachtrijnaam | Opslag account | 3-63 | Kleine letters | Alfanumeriek en -streepje | `<service short name>-<context>-<num>` | `awesomeservice-messages-001` |
| Opslag | Naam van de tabel | Opslag account | 3-63 |Hoofdlettergevoelig | Alfanumerieke | `<service short name>-<context>` | `awesomeservice-logs` |
| Opslag | Bestandsnaam | Opslag account | 3-63 | Kleine letters | Alfanumerieke | `<variable based on blob usage>` | `<variable based on blob usage>` |
| Netwerken | Virtueel netwerk (VNet) | Resourcegroep | 2-64 | Niet hoofdlettergevoelig | Alfanumeriek, streepjes, onderstrepingstekens en periode | `<service short name>-[section]-vnet` | `profx-vnet` |
| Netwerken | Subnet | Bovenliggende VNet | 2-80 | Niet hoofdlettergevoelig | Alfanumeriek, onderstrepingstekens, streepjes en periode | `<role>-subnet` | `gateway-subnet` |
| Netwerken | Netwerk-Interface | Resourcegroep | 1-80 | Niet hoofdlettergevoelig | Alfanumeriek, streepjes, onderstrepingstekens en periode | `<vmname>-<num>nic` | `profx-sql1-1nic` |
| Netwerken | Network Security Group | Resourcegroep | 1-80 | Niet hoofdlettergevoelig | Alfanumeriek, streepjes, onderstrepingstekens en periode | `<service short name>-<context>-nsg` | `profx-app-nsg` |
| Netwerken | Network Security Group-regel | Resourcegroep | 1-80 | Niet hoofdlettergevoelig | Alfanumeriek, streepjes, onderstrepingstekens en periode | `<descriptive context>` | `sql-allow` |
| Netwerken | Openbare IP-adres | Resourcegroep | 1-80 | Niet hoofdlettergevoelig | Alfanumeriek, streepjes, onderstrepingstekens en periode | `<vm or service name>-pip` | `profx-sql1-pip` |
| Netwerken | Taakverdeling | Resourcegroep | 1-80 | Niet hoofdlettergevoelig | Alfanumeriek, streepjes, onderstrepingstekens en periode | `<service or role>-lb` | `profx-lb` |
| Netwerken | Evenwichtige regeling Config laden | Taakverdeling | 1-80 | Niet hoofdlettergevoelig | Alfanumeriek, streepjes, onderstrepingstekens en periode | `descriptive context` | `http` |

<!-- TODO fill in the rest of these resources
| Networking | Azure Application Gateway | Resource Group | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | `<service or role>-aag` | `profx-aag`
| Networking | Azure Application Gateway Connection | Azure Application Gateway | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | `` | TODO
| Networking | Traffic Manager Profile | Resource Group | 1-80 | Case-insensitive | Alphanumeric, dash, underscore, and period | TODO | TODO
-->

## <a name="organizing-resources-with-tags"></a>Ordenen van bronnen met Tags

De bronnenbeheerder Azure ondersteunt tagging entiteiten met willekeurige tekenreeksen context te identificeren en te stroomlijnen automatisering.  Bijvoorbeeld de code `"sqlVersion: "sql2014ee"` VMs in een implementatie met de Enterprise Edition van SQL Server 2014 voor het uitvoeren van een geautomatiseerd script tegen hen kan identificeren.  Tags moeten worden gebruikt voor het uitbreiden en verbeteren van context aan de zijkant van de gekozen voor naamgeving.

> [AZURE.TIP]Een ander voordeel van labels is dat tags resourcegroepen, zodat u kunt koppelen en diensten tussen verschillende implementaties correleren beslaan.

Elke resource of resourcegroep kan maximaal **15** codes hebben. De labelnaam mag maximaal 512 tekens en de waarde van de tag is beperkt tot 256 tekens.

Raadpleeg voor meer informatie over resource-tagging, [labels gebruiken voor het ordenen van uw resources Azure](../resource-group-using-tags.md).

Enkele van de gemeenschappelijke tagging use-cases zijn:

- **Factuuradres**; Resources groeperen en deze te koppelen aan factuur- of toeslag back-codes.
- **Service-Context-id**; Groepen resources in resourcegroepen identificeren voor veelvoorkomende bewerkingen en groeperen
- **Toegangscontrole en beveiligingscontext**; Administratieve rol-id op basis van de portefeuille, systeem, service, app, exemplaar, enz.

> [AZURE.TIP]Label vroeg - tag vaak.  Beter om een basislijn voor het labelen van schema in plaats en pas over tijd plaats moeten optimaal na het feit.  

Een voorbeeld van enkele algemene benaderingen van labels:

| Labelnaam | Sleutel | Voorbeeld | Opmerking |
| -------- | --- | ------- | ------- |
| Bill / interne ID terugboeking | billTo  | `IT-Chargeback-1234` | Een interne i/o- of factureringscode |
| Operator of rechtstreeks verantwoordelijke persoon (DRI) | managedBy | `joe@contoso.com`  | Alias of e-mailadres |
| Naam van het project | naam van project | `myproject`  | De naam van het project of product |
| Project-versie | versie van project | `3.4`  | Versie van het project of product |
| Omgeving | omgeving | `<Production, Staging, QA >` | Milieu-id | 
| Laag | laag | `Front End, Back End, Data` | Laag of rol/context-id |
| Profiel gegevens | dataProfile | `Public, Confidential, Restricted, Internal` | Gevoeligheid van de gegevens die zijn opgeslagen in de bron |
 
## <a name="tips-and-tricks"></a>Tips en trucs

Sommige typen bronnen mogelijk extra zorgvuldig te werk op de naamgeving en verdragen.

### <a name="virtual-machines"></a>Virtuele Machines

Met name in grotere topologieën stroomlijnt zorgvuldig naamgeving van virtuele machines de rol en het doel van elke computer te identificeren en meer voorspelbare scripting inschakelen.

> [AZURE.WARNING]Elke virtuele machine in Azure is een resourcenaam Azure en een host-naam van het besturingssysteem.  
> Als de resourcenaam en de host-naam verschillen, beheer van de VMs mogelijk uitdagende en moet worden vermeden.
> Bijvoorbeeld als een virtuele machine wordt gemaakt vanuit een .vhd dat al bevat een besturingssysteem geconfigureerd met een host-naam.

- [Naamgevingsconventies voor VMs voor Windows Server](https://support.microsoft.com/en-us/kb/188997)

<!-- TODO - recommendations on naming VMs. -->

### <a name="storage-accounts-and-storage-entities"></a>Accounts voor opslag en opslag entiteiten

Er zijn twee primaire use-cases voor opslag rekeningen - back-ups van schijven voor VMs en opslaan van gegevens in tabellen, wachtrijen en BLOB's.  Opslag-accounts die worden gebruikt voor VM-schijven moeten als volgt de naamgevingsregels van de bovenliggende naam VM koppelt (en met de mogelijke noodzaak voor meerdere accounts van opslag voor high-end VM SKU's zijn ook van toepassing een achtervoegsel).

> [AZURE.TIP]Opslag rekeningen - voor gegevens en schijven - dienen te volgen regels voor naamgeving waarmee meerdere accounts voor opslag te worden gebruikt (dat wil zeggen altijd met een numerieke achtervoegsel).

Het is mogelijk om een aangepaste domeinnaam configureren voor toegang tot blobgegevens in uw account voor de opslag van Azure.
Het standaardeindpunt voor de Blob-service is `https://mystorage.blob.core.windows.net`.

Maar als u een aangepast domein (bijvoorbeeld www.contoso.com) eindpunt van de blob voor uw opslag account toewijst, is ook toegankelijk blob-gegevens in uw account voor opslag met behulp van dat domein. Met een aangepaste domeinnaam, bijvoorbeeld `http://mystorage.blob.core.windows.net/mycontainer/myblob` kan worden benaderd als `http://www.contoso.com/mycontainer/myblob`.

Verwijzen naar [een aangepaste domeinnaam voor uw eindpunt Blob-opslag configureren](../storage/storage-custom-domain-name.md)voor meer informatie over het configureren van deze functie.

Voor meer informatie over de naamgeving van BLOB's, containers en tabellen:

- [Naamgeving en verwijzen naar de Containers, BLOB's en metagegevens](https://msdn.microsoft.com/library/dd135715.aspx)
- [Naamgeving van wachtrijen en metagegevens](https://msdn.microsoft.com/library/dd179349.aspx)
- [Namen van tabellen](https://msdn.microsoft.com/library/azure/dd179338.aspx)

Een blob-naam kan een willekeurige combinatie van tekens bevatten, maar de gereserveerde URL tekens correct moeten worden voorafgegaan. Voorkomen dat de blob-namen die eindigen met een punt (.), een slash (/), of een reeks of een combinatie van beide. De slash is door het Verdrag, het scheidingsteken voor de **virtuele** map. Gebruik niet een achterwaartse slash (\) in een blob-naam. De client API's kunnen staan, maar niet goed hash en komt niet overeen met de handtekeningen.

Het is niet mogelijk de naam van een opslag of container wijzigen nadat deze is gemaakt.
Als u wilt een nieuwe naam te gebruiken, moet u deze verwijderen en een nieuw maken.

> [AZURE.TIP] Wij raden aan dat u een naamgevingsschema voor alle accounts voor opslag en typen maakt voordat u de ontwikkeling van een nieuwe service of toepassing.

## <a name="example---deploying-an-n-tier-service"></a>Voorbeeld: een n-tier-service implementeren

In dit voorbeeld definiëren we de configuratie van een n-tier-service, bestaande uit front-IIS-servers (gehost in Windows Server VMs), met SQL Server (die zijn ondergebracht in twee Windows Server VMs), een ElasticSearch cluster (Perfmon.msc in 6 Linux VMs) en bijbehorende opslag, virtuele netwerken resources groeperen en de belasting voor documentconversies.

We beginnen met de contextuele overeenkomsten voor deze toepassing te definiëren:

| Entiteit | Verdrag | Beschrijving  |
| ------ | ---------- | ------------ |  
| Naam van de service | `profx` | De korte naam van de toepassing of service wordt ingezet |
| Omgeving | `prod` | Dit is voor de productie-implementatie (in plaats van QA, testen, enz.) |

Vanaf de basislijn die kunnen we wijst uit de verdragen voor elk van de resourcetypen:

| Resourcetype | Verdrag Base | Voorbeeld |
| ------------- | --------------- | ------- |
| Abonnement | `<Company> <Department (optional)> <Product Line (optional)> <Environment>` | `Contoso IT InternalApps Profx Production` |
| Resourcegroep | `servicename-rg` | `profx-rg` |
| Virtueel netwerk | `servicename-vnet` | `profx-vnet` |
| Subnet | `role-subnet` | `sql-vnet` |
| Taakverdeling | `servicename-lb` | `profx-lb` |
| Virtuele Machine | `servicename-role[number]` | `profx-sql0` |
| Opslag Account | `<vmnamenodashes>st<num>` | `profxsql0st0` |

Zoals in het volgende diagram:

![topologie systeemdiagram] (media/guidance-naming-conventions/guidance-naming-convention-example.png "Toepassingstopologie monster")

## <a name="sample---azure-cli-script-for-deploying-the-sample-above"></a>Voorbeeld - Azure CLI-script voor het implementeren van het bovenstaande voorbeeld

```bash
#!/bin/sh

#####################################################################
# Sample script using the Azure CLI to build out an application 
# demonstrating naming conventions.  
#
# Note; this script is not intended for production deployment, as it does 
# not create availability sets, configure network security rules, etc.
#####################################################################

# Set up variables to build out the naming conventions for deploying
# the cluster  
LOCATION=eastus2
APP_NAME=profx
ENVIRONMENT=prod
USERNAME=testuser
PASSWORD="testpass"

# Set up the tags to associate with items in the application
TAG_BILLTO="InternalApp-ProFX-12345"
TAGS="billTo=${TAG_BILLTO}"

# Explicitly set the subscription to avoid confusion as to which subscription
# is active/default
SUBSCRIPTION=3e9c25fc-55b3-4837-9bba-02b6eb204331

# Set up the names of things using recommended conventions
RESOURCE_GROUP="${APP_NAME}-${ENVIRONMENT}-rg"
VNET_NAME="${APP_NAME}-vnet"

# Set up the postfix variables attached to most CLI commands
POSTFIX="--resource-group ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}"

##########################################################################################
# Set up the VM conventions for Linux and Windows images

# For Windows, get the list of URN's via 
# azure vm image list ${LOCATION} MicrosoftWindowsServer WindowsServer 2012-R2-Datacenter
WINDOWS_BASE_IMAGE=MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20160126

# For Linux, get the list or URN's via 
# azure vm image list ${LOCATION} canonical ubuntuserver
LINUX_BASE_IMAGE=canonical:ubuntuserver:16.04.0-DAILY-LTS:16.04.201602130

#########################################################################################
## Define functions 
create_vm ()
{
    vm_name=$1
    vnet_name=$2
    subnet_name=$3
    os_type=$4
    vhd_path=$5
    vm_size=$6
    diagnostics_storage=$7

    # Create the network interface card for this VM
    azure network nic create --name "${vm_name}-0nic" --subnet-name ${subnet_name} --subnet-vnet-name ${vnet_name} \
        --tags="${TAGS}" ${POSTFIX}

    # Create the storage account for this vm's disks (premium locally redundant storage -> PLRS)
    # Note the ${var//-/} syntax to remove dashes from the vm name
    storage_account_name=${vm_name//-/}st01
    azure storage account create --type=PLRS --tags "${TAGS}" ${POSTFIX} "${storage_account_name}"

    # Map the name of the diagnostics storage account to a blob URI for boot diagnostics
    # This is (currently) required when deploying with a named premium storage account 
    diag_blob="https://${diagnostics_storage}.blob.core.windows.net/"

    # Create the VM
    azure vm create --name ${vm_name} --nic-name "${vm_name}-0nic" --os-type ${os_type} \
        --image-urn ${vhd_path} --vm-size ${vm_size} --vnet-name ${vnet_name} --vnet-subnet-name ${subnet_name} \
        --storage-account-name "${storage_account_name}" --storage-account-container-name vhds --os-disk-vhd "${vm_name}-osdisk.vhd" \
        --admin-username "${USERNAME}" --admin-password "${PASSWORD}" \
        --boot-diagnostics-storage-uri "${diag_blob}" \
        --tags="${TAGS}" ${POSTFIX} 
}

###################################################################################################
# Create resources

# Step 1 - create the enclosing resource group
azure group create --name "${RESOURCE_GROUP}" --location "${LOCATION}" --tags "${TAGS}" --subscription "${SUBSCRIPTION}"

# Step 2 - create the network security groups

# Step 3 - create the networks (VNet and subnets)
azure network vnet create --name "${VNET_NAME}" --address-prefixes="10.0.0.0/8" --tags "${TAGS}" ${POSTFIX}
# TODO - does subnet support tagging?
azure network vnet subnet create --name gateway-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.1.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name es-master-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.2.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name es-data-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.3.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}
azure network vnet subnet create --name sql-subnet --vnet-name "${VNET_NAME}" --address-prefix="10.0.4.0/24" --resource-group "${RESOURCE_GROUP}" --subscription ${SUBSCRIPTION}

# Step 4 - define the load balancer and network security rules
azure network lb create --name "${APP_NAME}-lb" ${POSTFIX}
# In a production deployment script, we'd create load balancer rules and 
# network security groups here

# Step 5 - create a diagnostics storage account
diagnostics_storage_account=${APP_NAME//-/}diag
azure storage account create --type=LRS --tags "${TAGS}" ${POSTFIX} "${diagnostics_storage_account}"

# Step 6.1 - Create the gateway VMs
for i in `seq 1 4`;
do
    create_vm "${APP_NAME}-gw${i}" "${APP_NAME}-vnet" "gateway-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}" 
done    

# Step 6.2 - Create the ElasticSearch master and data VMs
for i in `seq 1 3`;
do
    create_vm "${APP_NAME}-es-master${i}" "${APP_NAME}-vnet" "es-master-subnet" "Linux" "${LINUX_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
done
for i in `seq 1 3`;
do
    create_vm "${APP_NAME}-es-data${i}" "${APP_NAME}-vnet" "es-data-subnet" "Linux" "${LINUX_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
done

# Step 6.3 - Create the SQL VMs
create_vm "${APP_NAME}-sql0" "${APP_NAME}-vnet" "sql-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
create_vm "${APP_NAME}-sql1" "${APP_NAME}-vnet" "sql-subnet" "Windows" "${WINDOWS_BASE_IMAGE}" "Standard_DS1" "${diagnostics_storage_account}"
```
