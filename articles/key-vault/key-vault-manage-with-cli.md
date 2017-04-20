<properties
    pageTitle="Sleutel kluis met CLI beheren | Microsoft Azure"
    description="Deze zelfstudie gebruiken voor het automatiseren van algemene taken in een kluis met sleutel met behulp van de CLI"
    services="key-vault"
    documentationCenter=""
    authors="BrucePerlerMS"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/26/2016"
    ms.author="bruceper"/>

# <a name="manage-key-vault-using-cli"></a>Sleutel kluis met CLI beheren #
Azure sleutel kluis is beschikbaar in de meeste regio's. Zie de [sleutel kluis prijzen pagina](https://azure.microsoft.com/pricing/details/key-vault/)voor meer informatie.

## <a name="introduction"></a>Inleiding  
Met deze zelfstudie kunt u aan de slag met Azure sleutel kluis voor het maken van een harde container (kluis) in Azure, opslaan en beheren van cryptografische sleutels en geheimen in Azure. Het helpt u bij het proces van het gebruik van de opdrachtregelinterface voor Azure-Cross-Platform voor het maken van een kluis met een sleutel of het wachtwoord dat u een toepassing Azure kunt. Deze vervolgens ziet u hoe een toepassing kunt die sleutel of wachtwoord.

**Geschatte tijd:** 20 minuten

>[AZURE.NOTE]  Deze zelfstudie bevat geen instructies over het schrijven van de Azure toepassing dat een van de stappen omvat, die laat zien hoe een toepassing voor het gebruik van een sleutel of een geheim in de sleutel kluis toe te staan.
>
>U kunt op dit moment Azure sleutel kluis in Azure portal niet configureren. Gebruik deze instructies opdrachtregelinterface Cross-Platform. Of Zie [deze zelfstudie equivalent](key-vault-get-started.md)voor Azure PowerShell instructies.

Zie voor informatie over Azure sleutel kluis, [Wat is Azure sleutel kluis?](key-vault-whatis.md)

## <a name="prerequisites"></a>Vereisten
Deze zelfstudie hebt u het volgende:

- Een abonnement op Microsoft Azure. Als u dit niet hebt nog, kunt u zich aanmelden voor een [gratis proefperiode](https://azure.microsoft.com/pricing/free-trial).
- Interface met opdrachtregel versie 0.9.1 of hoger. Zie de meest recente versie installeren en aansluiten op uw abonnement Azure, [installeren en configureren van de opdrachtregelinterface van Azure Cross-Platform](../xplat-cli-install.md).
- Een toepassing die wordt geconfigureerd voor het gebruik van de sleutel of wachtwoord die u in deze zelfstudie maakt. Een voorbeeldtoepassing is beschikbaar via het [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343). Zie de bijbehorende Leesmij-bestand voor instructies.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Help-informatie met Azure platformonafhankelijke Interface met opdrachtregel

In deze zelfstudie wordt ervan uitgegaan dat u bekend met de opdrachtregelinterface (Bash, Terminal, MS-DOS-prompt bent)

De--help of h - parameter kan worden gebruikt voor het weergeven van help voor specifieke opdrachten. U kunt ook de azure help [opdracht] [opties] indeling kan ook worden gebruikt om terug te keren dezelfde informatie. Bijvoorbeeld de volgende opdrachten alle hetzelfde resultaat geven:

    azure account set --help

    azure account set -h

    azure help account set

Raadpleeg bij twijfel over de parameters die nodig zijn voor een opdracht om te helpen met behulp van--help, -h of azure help [opdracht].

U kunt ook de volgende zelfstudies om vertrouwd te raken met Azure Resource Manager in Azure platformonafhankelijke Interface met opdrachtregel lezen:

- [Het installeren en configureren van Azure platforms Command Line Interface](../xplat-cli-install.md)
- [Met behulp van Azure opdrachtregelinterface van Cross-Platform met Azure Resource Manager](../xplat-cli-azure-resource-manager.md)


## <a name="connect-to-your-subscriptions"></a>Verbinding maken met uw abonnementen

Gebruik de volgende opdracht te melden met een account van de organisatie:

    azure login -u username -p password

of als u wilt aanmelden, typt u interactief

    azure login

>[AZURE.NOTE]  De methode aanmelding werkt alleen met organisatie-account. Een organisatie-account is een gebruiker die wordt beheerd door uw organisatie en gedefinieerd in Azure Active Directory-huurder van uw organisatie.


Als u hebt niet de account van een organisatie- en van een Microsoft-account gebruikmaakt aan te melden uw abonnement Azure, kunt u gemakkelijk maken met behulp van de volgende stappen uit.

1.  Aanmelden bij de aanmelding op de [Azure Management Portal](https://manage.windowsazure.com/)en klik op Active Directory.
2.  Als er geen map bestaat, selecteert u de map maken en geef de gevraagde gegevens.
3.  Selecteer de map en een nieuwe gebruiker toevoegen. Deze nieuwe gebruiker is een organisatie-account. Tijdens het maken van de gebruiker die u hebt opgegeven met zowel een e-mailadres van de gebruiker en een tijdelijk wachtwoord. Bewaar deze gegevens zoals het wordt gebruikt in een andere stap.
4.  Selecteer instellingen en selecteer beheerders van de portal. Selecteer de optie toevoegen en de nieuwe gebruiker toevoegen als beheerder van een collega. Hierdoor kan de organisatie-account voor het beheer van uw abonnement op Azure.
5.  Ten slotte, zijn afgemeld bij de Azure portal en meld u vervolgens weer aan met de nieuwe organisatie-account. Als dit de eerste keer aanmelden met deze account, wordt u gevraagd het wachtwoord te wijzigen.

Zie [aanmelden bij Microsoft Azure als organisatie](../active-directory/sign-up-organization.md)voor meer informatie over het gebruik van een organisatie-account met Microsoft Azure.

Meerdere abonnementen als een specifieke opgeven voor Azure sleutel kluis, typt u het volgende overzicht van de abonnementen voor uw account:

    azure account list

Als u het abonnement wilt gebruiken, typt u vervolgens:

    azure account set <subscription name>

Zie voor meer informatie over het configureren van de opdrachtregelinterface van Azure Cross-Platform [installeren en configureren van Azure platformonafhankelijke Interface met opdrachtregel](../xplat-cli-install.md).


## <a name="switch-to-using-azure-resource-manager"></a>Azure Resource Manager gebruiken

De sleutel kluis Azure Resource Manager vereist, dus typt u de volgende overschakelen naar de modus Azure Resource Manager:

    azure config mode arm

## <a name="create-a-new-resource-group"></a>Een nieuwe resourcegroep maken

Wanneer u Azure Resource Manager, worden alle verwante bronnen binnen een groep gemaakt. We gaan een nieuwe resourcegroep 'ContosoResourceGroup' maken voor deze zelfstudie.

    azure group create 'ContosoResourceGroup' 'East Asia'

De eerste parameter is de naam van de resource en de tweede parameter is de locatie. Gebruik de opdracht voor de locatie, `azure location list` om te bepalen hoe u een alternatieve locatie naar de in dit voorbeeld. Als u meer informatie nodig hebt, typt u:`azure help location`

## <a name="register-the-key-vault-resource-provider"></a>De sleutel kluis resource provider registreren
Zorg ervoor dat die sleutel kluis resource provider is geregistreerd in uw abonnement:

`azure provider register Microsoft.KeyVault`

Dit moet alleen eenmaal per abonnement worden gedaan.


## <a name="create-a-key-vault"></a>Maak een sleutel kluis

Gebruik de `azure keyvault create` de opdracht voor het maken van een sleutel kluis. Dit script heeft drie verplichte parameters: de naam van een resourcegroep, de naam van een sleutel kluis en de geografische locatie.

Bijvoorbeeld als u de naam van de kluis van ContosoKeyVault, de naam van de resource van ContosoResourceGroup en de locatie van Oost-Azië, typt u:

    azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'

De uitvoer van deze opdracht worden de eigenschappen van de sleutel kluis die u net hebt gemaakt. De twee belangrijkste eigenschappen zijn:

- **Naam**: In het voorbeeld is dit ContosoKeyVault. U gebruikt deze naam voor een andere sleutel kluis cmdlets.
- **vaultUri**: In het voorbeeld is dit https://contosokeyvault.vault.azure.net. Toepassingen die gebruikmaken van de kluis via de REST API, moeten deze URI gebruiken.

Uw Azure account mag zich nu alle bewerkingen uitvoeren op deze sleutel kluis. Nog is niemand anders.


## <a name="add-a-key-or-secret-to-the-key-vault"></a>Een sleutel of een geheim om de sleutel toevoegen

Als u Azure sleutel kluis een sleutel software beschermd voor u te maken, gebruikt u de `azure key create` opdracht en typt u het volgende:

    azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software

Echter, als u een bestaande sleutel in een .pem bestand als lokaal bestand opgeslagen in een bestand met de naam softkey.pem die u wilt uploaden naar Azure sleutel kluis, typt u het volgende voor het importeren van de sleutel uit het. PEM-bestand die de sleutel worden beveiligd door de software in de sleutel kluis-service:

    azure keyvault key import --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' --password 'PaSSWORD' --destination software

Nu kunt u verwijzen naar de sleutel die u hebt gemaakt of geüpload naar Azure sleutel kluis, met behulp van de URI. Altijd beschikt over de huidige versie met **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** en **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** gebruiken om deze specifieke versie.

Een geheim aan de kluis, die een wachtwoord met de naam SQLPassword en die de waarde van Pa$ $w0rd naar Azure sleutel kluis, typt u het volgende:

    azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'

Nu kunt u verwijzen naar dit wachtwoord die u hebt toegevoegd aan de Azure sleutel kluis, met behulp van de URI. Altijd beschikt over de huidige versie met **https://ContosoVault.vault.azure.net/secrets/SQLPassword** en **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** gebruiken om deze specifieke versie.

We bekijken de sleutel of een geheim dat u zojuist hebt gemaakt:

- Als u wilt weergeven in uw sleutel, typt u:`azure keyvault key list --vault-name 'ContosoKeyVault'`
- Als u wilt uw geheime weergeven, typt u:`azure keyvault secret list --vault-name 'ContosoKeyVault'`


## <a name="register-an-application-with-azure-active-directory"></a>Een toepassing met Azure Active Directory registreren

Deze stap zou worden meestal gebruikt door een ontwikkelaar, op een afzonderlijke computer. Het is niet specifiek voor Azure sleutel kluis, maar is hier opgenomen voor de volledigheid.


>[AZURE.IMPORTANT] Om de zelfstudie, uw account de kluis en de toepassing die u in deze stap wordt registreert moet alle in Azure Active directory.

Toepassingen die gebruikmaken van een sleutel kluis moeten worden geverifieerd met behulp van een token uit Azure Active Directory. Hiervoor moet de eigenaar van de toepassing eerst de toepassing in Azure Active Directory registreren. Aan het einde van de registratie wordt de eigenaar van de toepassing de volgende waarden:


- Een **Toepassings-ID** (ook wel een Client-ID) en de **verificatiesleutel** (ook bekend als het gedeelde geheim). De toepassing moet van deze waarden bieden voor Azure Active Directory, een token ophalen. Hoe de toepassing hiervoor is geconfigureerd, is afhankelijk van de toepassing. Voor de voorbeeldtoepassing sleutel kluis stelt de eigenaar van de toepassing deze waarden in het bestand app.config.



De toepassing in Azure Active Directory registreren:

1. Log in om de Azure portal.
2. Klik op **Active Directory**aan de linkerkant en selecteer vervolgens de directory waarin u uw toepassing wordt geregistreerd. <br> <br> Opmerking: Selecteer de directory waarin de Azure abonnement waarmee u uw sleutel kluis gemaakt op. Als u niet welke map dit weet is, klikt u op **Instellingen**en noteer de naam van de map weergegeven in de laatste kolom herkennen het abonnement waarmee u uw sleutel kluis gemaakt.

3. Klik op **toepassingen**. Als er geen apps zijn toegevoegd aan de map, wordt deze pagina alleen de koppeling **toevoegen** op een App weergegeven. Klik op de koppeling of als alternatief kunt u het **toevoegen** aan de opdrachtbalk.
4.  In de wizard **Toepassing toevoegen** op de **Wat wilt u doen?** pagina, klikt u op **het ontwikkelen van mijn organisatie toepassing toevoegen**.
5.  Geef een naam op voor uw toepassing en **WEB toepassing en/of WEB-API** (de standaardinstelling) selecteren op de pagina **Vertel ons over uw aanvraag** . Klik op het volgende pictogram.
6.  Geef de **SIGN-ON-URL** en **APP-ID-URI** voor uw webtoepassing op de pagina **Eigenschappen van App** . Als uw toepassing niet over deze waarden, kunt u ze voor deze stap (bijvoorbeeld, kon u http://test1.contoso.com voor beide vakken). Het maakt niet uit als deze sites bestaan; Wat is het belangrijk is dat de app URI-ID voor elke toepassing verschillend voor elke toepassing in de map. De map wordt deze tekenreeks gebruikt voor het identificeren van uw app.
7.  Klik op het pictogram volledige slaat u de wijzigingen in de wizard.
8.  Op de pagina Quick Start, klikt u op **configureren**.
9.  Schuif naar de sectie **sleutels** , selecteert u de duur en klik vervolgens op **Opslaan**. De pagina wordt vernieuwd en bevat nu de waarde van een sleutel. U moet uw toepassing configureren met de waarde van deze sleutel en de waarde van de **CLIENT-ID** . (Instructies voor deze configuratie zijn toepassingsspecifiek.)
10. Kopieer de waarde van de client-ID op deze pagina wordt u in de volgende stap machtigingen in te stellen op de kluis.




## <a name="authorize-the-application-to-use-the-key-or-secret"></a>De toepassing voor het gebruik van de sleutel of een geheim toestaan

Gebruik voor het machtigen van de toepassing toegang tot de sleutel of een geheim in de kluis, de `azure keyvault set-policy` opdracht.

Bijvoorbeeld als de naam van uw kluis ContosoKeyVault is en de toepassing die u wilt toestaan dat een client-ID van de 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed heeft en u toestaan dat de toepassing wilt voor het decoderen en ondertekenen met sleutels in uw kluis, voert u het volgende:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-keys '["decrypt","sign"]'

>[AZURE.NOTE] Als u op de opdrachtprompt van Windows wordt uitgevoerd, moet u enkele aanhalingstekens vervangen door dubbele aanhalingstekens en ook de interne dubbele aanhalingstekens ontsnappen. Bijvoorbeeld: ' [\"decoderen\",\"teken\"] ".

Als u toestaan dat dezelfde toepassing wilt lezen geheimen in de kluis, het volgende uitvoeren:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-secrets '["get"]'

## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Als u wilt gebruiken een hardwarebeveiligingsmodule (HSM) ##

U kunt importeren of het genereren van sleutels in hardware security modules (HSM's) die nooit de HSM-grens laat voor toegevoegde assurance. De HSM's zijn FIPS 140-2 niveau 2 gevalideerd. Als deze eis niet voor u geldt, kunt u deze sectie overslaan en Ga naar [de sleutel kluis te verwijderen en het bijbehorende sleutels en geheimen](#delete-the-key-vault-and-associated-keys-and-secrets).

Om deze sleutels beveiligd met een HSM maakt, hebt u een abonnement kluis die beveiligd met een HSM-sleutels ondersteunt.

Wanneer u de keyvault maakt, moet u de parameter 'sku' toevoegen:

    azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'

U kunt software beschermd (zoals eerder) en beveiligd met een HSM sleutels toevoegen aan deze kluis. Als u een sleutel beveiligd met een HSM, stelt de parameter Destination naar 'HSM':

    azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'

De volgende opdracht kunt u een sleutel wilt importeren uit een bestand .pem op uw computer. Met deze opdracht importeert de sleutel in HSM's in de sleutel kluis-service:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' --password 'PaSSWORD'

De volgende opdracht importeert u een "doen om uw eigen sleutel' pakket (BYOK). Hiermee kunt u uw sleutel in uw lokale HSM genereren en overbrengen naar HSM's in de sleutel kluis-service zonder het verlaten van de rand van de HSM-sleutel:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'

Zie voor gedetailleerde instructies over het genereren van dit pakket BYOK [HSM-Protected sleutels met Azure sleutel kluis gebruiken](key-vault-hsm-protected-keys.md).


## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>Verwijder de sleutel kluis en de bijbehorende sleutels en geheimen

Als u niet langer de sleutel kluis en de sleutel of een geheim dat het bevat, kunt u de sleutel kluis verwijderen met de opdracht azure keyvault verwijderen:

    azure keyvault delete --vault-name 'ContosoKeyVault'

Of u kunt een hele Azure resourcegroep, waaronder de sleutel kluis en andere bronnen die u hebt opgenomen in de groep verwijderen:

    azure group delete --name 'ContosoResourceGroup'


## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Andere Azure platforms opdrachtregelinterface opdrachten

Andere opdrachten die u wellicht nuttig voor het beheer van Azure sleutel kluis.

Met deze opdracht wordt een tabelvorm weergave van alle sleutels en geselecteerde eigenschappen:

    azure keyvault key list --vault-name 'ContosoKeyVault'

Deze opdracht geeft u een volledige lijst van eigenschappen voor de opgegeven sleutel:

    azure keyvault key show --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Met deze opdracht wordt een tabelvorm weergave van alle geselecteerde eigenschappen en geheime namen:

    azure keyvault secret list --vault-name 'ContosoKeyVault'

Hier volgt een voorbeeld van het verwijderen van een specifieke sleutel:

    azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Hier volgt een voorbeeld van het verwijderen van een specifieke geheim:

    azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'


## <a name="next-steps"></a>Volgende stappen

Zie [de Azure sleutel kluis developer's guide](key-vault-developers-guide.md)voor het programmeren van verwijzingen.
