<properties
   pageTitle="Aan de slag met Azure Batch CLI | Microsoft Azure"
   description="Een korte inleiding tot de opdrachten Batch in Azure CLI ophalen voor het beheren van bronnen van Azure Batch-service"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="09/30/2016"
   ms.author="marsma"/>

# <a name="get-started-with-azure-batch-cli"></a>Aan de slag met Azure Batch CLI

De cross-platform Azure opdrachtregelinterface (CLI Azure) kunt u uw Batchaccounts en bronnen, zoals toepassingen, projecten en taken in Linux, Mac en Windows opdrachtshells te beheren. U kunt met de CLI Azure uitvoeren en scripts van veel van de taken die u met de Batch-API's, Azure portal en Batch PowerShell-cmdlets uitvoert.

Dit artikel is gebaseerd op Azure CLI versie 0.10.5.

## <a name="prerequisites"></a>Vereisten

* [De Azure CLI installeren](../xplat-cli-install.md)

* [Verbinding maken met de CLI Azure Azure abonnement](../xplat-cli-connect.md)

* Overschakelen naar de **modus Resource Manager**:`azure config mode arm`

>[AZURE.TIP] Wij raden aan dat u uw Azure CLI installatie bijwerken vaak om te profiteren van service-updates en verbeteringen.

## <a name="command-help"></a>Opdracht help

U kunt voor elke opdracht help-tekst weergeven in de CLI Azure door toe te voegen `-h` als de enige optie bij de opdracht. Bijvoorbeeld:

* Hulp voor de `azure` , kunt invoeren:`azure -h`
* Als u een lijst met alle opdrachten voor Batch in de CLI, gebruikt u:`azure batch -h`
* Als u Help-informatie over het maken van een Batch-account, typ:`azure batch account create -h`

Bij twijfel, gebruik de `-h` de opdrachtregeloptie voor help bij een opdracht Azure CLI.

## <a name="create-a-batch-account"></a>Een Batch-account maken

Syntaxis:

    azure batch account create [options] <name>

Voorbeeld:

    azure batch account create --location "West US"  --resource-group "resgroup001" "batchaccount001"

Een nieuwe Batch-account gemaakt met de opgegeven parameters. U moet ten minste een locatie, de resourcegroep en de accountnaam opgeven. Als u nog niet een resourcegroep, maken door het uitvoeren van `azure group create`, en geeft u een van de Azure regio's (zoals 'West ons') voor de `--location` optie. Bijvoorbeeld:

    azure group create --name "resgroup001" --location "West US"

> [AZURE.NOTE] De naam van de account moet uniek zijn binnen de Azure regio die de account wordt gemaakt. Het kan alleen kleine letters alfanumerieke tekens bevatten en moet 3-24 tekens lang zijn. U kunt speciale tekens zoals gebruiken `-` of `_` in rekening namen.

### <a name="linked-storage-account-autostorage"></a>Opslag van gekoppelde account (autostorage)

(Optioneel) kunt u een **algemene** opslag-account koppelen aan uw Batch-account wanneer u deze maakt. De functie voor [toepassingspakketten](batch-application-packages.md) batch gebruikt blob-opslag in een gekoppelde algemene opslag-account de [Batch bestand verdragen .NET](batch-task-output.md) library. Deze optionele functies helpt u bij het persistent maken van de gegevens die zij produceren en distribueren van de toepassingen die de batchverwerking taken uitgevoerd.

Als een bestaande account voor de opslag van Azure koppelen aan een nieuwe Batch-account wanneer u deze maakt, geeft de `--autostorage-account-id` optie. Deze optie moet de volledige bron-ID van de account van de opslag.

Details van uw opslag account eerst weergeven:

    azure storage account show --resource-group "resgroup001" "storageaccount001"

Gebruik vervolgens de **Url** -waarde voor de `--autostorage-account-id` optie. De waarde van de Url begint met "/ abonnementen /" en uw abonnement-ID en bron pad bevat naar de rekening voor de opslag:

    azure batch account create --location "West US"  --resource-group "resgroup001" --autostorage-account-id "/subscriptions/8ffffff8-4444-4444-bfbf-8ffffff84444/resourceGroups/resgroup001/providers/Microsoft.Storage/storageAccounts/storageaccount001" "batchaccount001"

## <a name="delete-a-batch-account"></a>Een Batch-account verwijderen

Syntaxis:

    azure batch account delete [options] <name>

Voorbeeld:

    azure batch account delete --resource-group "resgroup001" "batchaccount001"

Hiermee verwijdert u de opgegeven account voor de Batch. Wanneer dat wordt gevraagd, bevestig verwijderen van de account (account verwijderen kan enige tijd duren).

## <a name="manage-account-access-keys"></a>Toegangstoetsen account beheren

Moet u een toegangstoets [maken](#create-and-modify-batch-resources) en wijzigen van de resources in uw account voor de Batch.

### <a name="list-access-keys"></a>Toegangstoetsen lijst

Syntaxis:

    azure batch account keys list [options] <name>

Voorbeeld:

    azure batch account keys list --resource-group "resgroup001" "batchaccount001"

Hier de account toetsen voor de opgegeven account voor de Batch.

### <a name="generate-a-new-access-key"></a>Een nieuwe access-sleutel genereren

Syntaxis:

    azure batch account keys renew [options] --<primary|secondary> <name>

Voorbeeld:

    azure batch account keys renew --resource-group "resgroup001" --primary "batchaccount001"

Worden opnieuw gegenereerd door de sleutel opgegeven account voor de opgegeven account voor de Batch.

## <a name="create-and-modify-batch-resources"></a>Maken en wijzigen van Batch-bronnen

U kunt de CLI Azure maken, lezen, bijwerken, en bronnen zoals pools (CRUD) Batch verwijderen, knooppunten, projecten en taken te berekenen. Deze bewerkingen CRUD moeten uw account de naam, de toegangstoets en het eindpunt. Kunt u deze met de `-a`, `-k`, en `-u` -opties of set [omgevingsvariabelen](#credential-environment-variables) die gebruikmaakt van de CLI automatisch (indien gevuld).

### <a name="credential-environment-variables"></a>Omgevingsvariabelen referentie

U kunt instellen `AZURE_BATCH_ACCOUNT`, `AZURE_BATCH_ACCESS_KEY`, en `AZURE_BATCH_ENDPOINT` omgevingsvariabelen in plaats van `-a`, `-k`, en `-u` opties op de opdrachtregel voor elke opdracht die u wilt uitvoeren. Deze variabelen worden gebruikt in de Batch-CLI (indien ingesteld) zodat u kunt weglaten de `-a`, `-k`, en `-u` opties. De rest van dit artikel wordt ervan uitgegaan dat het gebruik van deze omgevingsvariabelen.

>[AZURE.TIP] Lijst van de sleutels met `azure batch account keys list`, en het eindpunt van de account met weer `azure batch account show`.

### <a name="json-files"></a>JSON bestanden

Wanneer u een Batch-bronnen zoals toepassingen en taken maakt, kunt u een JSON-bestand met de nieuwe resource-configuratie in plaats van de parameters doorgeven als parameters. Bijvoorbeeld:

`azure batch pool create my_batch_pool.json`

Terwijl u veel bewerkingen van resources maken met alleen de opdrachtregelopties uitvoeren kunt, moeten sommige functies een opgemaakte JSON bestand met details over de bron. U moet bijvoorbeeld een JSON-bestand gebruiken als u wilt opgeven van de bronbestanden voor een starttaak.

De JSON vereist voor het maken van een bron, Zie de [Batch REST API reference] [ rest_api] documentatie op MSDN. Voorbeeld JSON bevat elk onderwerp *'Resourcetype*toevoegen' voor het maken van de bron die u als sjablonen voor JSON bestanden gebruiken kunt. Bijvoorbeeld JSON voor het maken van toepassingen kan worden gevonden in [een groep aan een account toevoegen][rest_add_pool].

>[AZURE.NOTE] Als u een JSON-bestand opgeven als u een resource maakt, worden alle andere parameters die u op de opdrachtregel voor die resource opgeeft genegeerd.

## <a name="create-a-pool"></a>Een groep maken

Syntaxis:

    azure batch pool create [options] [json-file]

Voorbeeld (virtuele Machine Configuration):

    azure batch pool create --id "pool001" --target-dedicated 1 --vm-size "STANDARD_A1" --image-publisher "Canonical" --image-offer "UbuntuServer" --image-sku "14.04.2-LTS" --node-agent-id "batch.node.ubuntu 14.04"

Voorbeeld (Cloud Services Configuration):

    azure batch pool create --id "pool002" --target-dedicated 1 --vm-size "small" --os-family "4"

Een groep met computerknooppunten maakt in de Batch-service.

Zoals vermeld in het [overzicht van de functie Batch](batch-api-basics.md#pool), hebben twee opties wanneer u een besturingssysteem voor de knooppunten in uw groep: **Virtuele machineconfiguratie** en de **Configuratie van de Cloud-Services**. Gebruik de `--image-*` opties voor het maken van virtuele machineconfiguratie van toepassingen en `--os-family` Cloud Services configuratie van toepassingen maken. U kunt niet beide opgeven `--os-family` en `--image-*` opties.

U kunt toepassingen [toepassingspakketten](batch-application-packages.md) en de opdrachtregel voor een [taak starten](batch-api-basics.md#start-task). Als u de bronbestanden voor de starttaak, maar moet u gebruiken een [JSON-bestand](#json-files).

Hiermee verwijdert u een groep met:

    azure batch pool delete [pool-id]

>[AZURE.TIP] Controleer de [lijst met afbeeldingen van de virtuele machine](batch-linux-nodes.md#list-of-virtual-machine-images) voor waarden die geschikt zijn voor de `--image-*` opties.

## <a name="create-a-job"></a>Een taak maken

Syntaxis:

    azure batch job create [options] [json-file]

Voorbeeld:

    azure batch job create --id "job001" --pool-id "pool001"

Wordt een taak aan de Batch-account en de groep waarop de taken worden uitgevoerd.

Een taak met verwijderen:

    azure batch job delete [job-id]

## <a name="list-pools-jobs-tasks-and-other-resources"></a>Lijst van toepassingen, projecten, taken en andere resources

Elke Batch brontype ondersteunt een `list` opdracht die uw Batch-account opvragen en lijsten bronnen van dat type. U kunt bijvoorbeeld de toepassingen aanbieden in uw account en de taken in een project:

    azure batch pool list
    azure batch task list --job-id "job001"

### <a name="listing-resources-efficiently"></a>Bronnen aanbieding efficiënt

Voor snellere query's, kunt u **selecteren**, **filter**en **Vouw** component opties voor `list` bewerkingen. Gebruik deze opties om de hoeveelheid gegevens die worden geretourneerd door de Batch-service beperken. Omdat alle filtering server-side plaatsvindt, wordt alleen de gegevens die u geïnteresseerd bent in de draad gesneden. Met deze componenten kunt u bandbreedte besparen (en dus tijd) wanneer u een lijst met bewerkingen uitvoert.

Hiermee herstelt u bijvoorbeeld alleen de groepen waarvan de id met 'renderTask begint':

    azure batch task list --job-id "job001" --filter-clause "startswith(id, 'renderTask')"

De Batch CLI ondersteunt alle drie componenten worden ondersteund door de service Batch:

* `--select-clause [select-clause]`Een subset van eigenschappen voor elke entiteit retourneren
* `--filter-clause [filter-clause]`Alleen entiteiten die overeenkomen met de opgegeven expressie van OData retourneren
* `--expand-clause [expand-clause]`De entiteit informatie in één onderliggende REST aanroep ophalen. De component uitvouwen ondersteunt alleen de `stats` eigenschap op dit moment.

Zie [de Batch Azure service efficiënt Query](batch-efficient-list-queries.md)voor meer informatie over de drie componenten en presterende lijst met query's met hen.

## <a name="application-package-management"></a>Pakket Toepassingsbeheer

Toepassingspakketten biedt een vereenvoudigde manier voor de implementatie van toepassingen aan de compute nodes in uw toepassingen. Met de CLI Azure kunt u uploaden toepassingspakketten, pakket versies beheren en verwijderen van pakketten.

Een nieuwe toepassing maken en toevoegen van een Pakketversie:

**Maken** van een toepassing:

    azure batch application create "resgroup001" "batchaccount001" "MyTaskApplication"

**Toevoegen** van een toepassingspakket:

    azure batch application package create "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" package001.zip

**Activeren** van het pakket:

    azure batch application package activate "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" zip

De **standaardversie** voor de toepassing instellen:

    azure batch application set "resgroup001" "batchaccount001" "MyTaskApplication" --default-version "1.10-beta3"

### <a name="deploy-an-application-package"></a>Een pakket van toepassing

Wanneer u een nieuwe groep maakt, kunt u een of meer toepassingspakketten voor de implementatie opgeven. Wanneer u een pakket bij het maken van toepassingen opgeeft, wordt deze geïnstalleerd op elk knooppunt als de joins knooppunt toepassingen. Pakketten worden ook geïmplementeerd als een knooppunt wordt opnieuw opgestart of reimaged.

Geef de `--app-package-ref` bij het maken van een een toepassingspakket op knooppunten van de toepassingen installeren als zij lid worden van de groep van toepassingen de optie. De `--app-package-ref` optie accepteert een door puntkomma's gescheiden lijst van application-id's voor de implementatie van de compute nodes.

    azure batch pool create --pool-id "pool001" --target-dedicated 1 --vm-size "small" --os-family "4" --app-package-ref "MyTaskApplication"

Als u een groep maakt met behulp van opties op de opdrachtregel, kunt u niet op dit moment *welke* versie van het pakket-toepassing implementeren op de compute nodes, bijvoorbeeld "1.10-Bèta3" opgeven. Daarom moet u eerst een standaardversie van de toepassing met opgeven `azure batch application set [options] --default-version <version-id>` voordat u de groep maakt (Zie de vorige sectie). Als u een [JSON-bestand](#json-files) in plaats van opdrachtregelopties gebruiken wanneer u de groep maakt, kunt u echter een Pakketversie voor de groep opgeven.

U vindt meer informatie over toepassingspakketten in de [implementatie van toepassingen met Azure Batch toepassingspakketten](batch-application-packages.md).

>[AZURE.IMPORTANT] Moet u [een opslag Azure-account koppelen](#linked-storage-account-autostorage) aan uw account voor Batch toepassingspakketten gebruiken.

### <a name="update-a-pools-application-packages"></a>Updatepakketten van een groep van toepassingen

Uitgeven om te werken de toepassingen toegewezen aan een bestaande groep van toepassingen, de `azure batch pool set` opdracht met de `--app-package-ref` optie:

    azure batch pool set --pool-id "pool001" --app-package-ref "MyTaskApplication2"

Voor de implementatie van de nieuwe toepassingspakket computerknooppunten al in een bestaande groep, moet u opnieuw opstart of reimage die knooppunten:

    azure batch node reboot --pool-id "pool001" --node-id "tvm-3105992504_1-20160930t164509z"

>[AZURE.TIP] U kunt een lijst van de knooppunten in een groep, samen met de knooppunt-id's ophalen met `azure batch node list`.

Houd er rekening mee dat u moet al de toepassing hebt geconfigureerd met een standaardversie voor installatie (`azure batch application set [options] --default-version <version-id>`).

## <a name="troubleshooting-tips"></a>Tips voor probleemoplossing

Deze sectie is bedoeld om bronnen te gebruiken bij het oplossen van problemen met Azure CLI. Deze won't noodzakelijkerwijs oplossing voor alle problemen, maar kunt u beperken de oorzaak en wijst u de hulpbronnen.

* Gebruik `-h` **help-tekst** opvragen voor een opdracht CLI

* Gebruik `-v` en `-vv` om weer te geven van **uitgebreide** uitvoer van de opdracht; `-vv` "extra" uitgebreid en wordt de werkelijke REST-aanvragen en -antwoorden. Deze schakelopties zijn handig voor het weergeven van uitvoer volledig fout.

* U kunt weergeven **als JSON uitvoer van een opdracht** met de `--json` optie. Bijvoorbeeld: `azure batch pool show "pool001" --json` pool001 van eigenschappen in JSON-indeling weergegeven. U kunt vervolgens kopiëren en wijzigen van deze uitvoer te gebruiken in een `--json-file` ( [JSON-bestanden](#json-files) Zie eerder in dit artikel).

* De [Batch-forum op MSDN] -[ batch_forum] is een bron van nuttige informatie en nauwkeurig wordt gecontroleerd door teamleden Batch. Zorg ervoor dat er vragen posten als u problemen ondervindt of hulp nodig hebt met een specifieke bewerking.

* Niet elke resource batchbewerking wordt momenteel ondersteund door de CLI Azure. Bijvoorbeeld kan niet, u op dit moment opgeven een pakket van toepassing *versie* voor een groep, met alleen de pakket-ID. In dergelijke gevallen moet u mogelijk opgeven een `--json-file` voor de opdracht in plaats van het gebruik van opdrachtregelopties. Zorg ervoor dat de hoogte blijven van de meest recente versie van de CLI picken van toekomstige uitbreidingen.

## <a name="next-steps"></a>Volgende stappen

*  Zie de [implementatie van toepassingen met Azure Batch toepassingspakketten](batch-application-packages.md) om erachter te komen hoe u deze functie wilt gebruiken voor het beheren en distribueren van de toepassingen die u op de Batch-computerknooppunten uitvoert.

* Zie [efficiënt opvragen van de Batch-service](batch-efficient-list-queries.md) voor meer informatie over vermindering van het aantal artikelen en het soort informatie dat wordt geretourneerd voor query's naar de Batch.

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx