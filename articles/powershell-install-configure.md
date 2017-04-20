<properties
    pageTitle="Het installeren en configureren van Azure PowerShell"
    description="Informatie over het installeren en configureren van Azure PowerShell."
    editor="tysonn"
    manager="dongill"
    documentationCenter=""
    services=""
    authors="coreyp-at-msft"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="powershell"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="coreyp"/>

# <a name="how-to-install-and-configure-azure-powershell"></a>Het installeren en configureren van Azure PowerShell

<div class="dev-center-tutorial-selector sublanding"><a href="/manage/install-and-configure-windows-powershell/" title="PowerShell" class="current">PowerShell</a><a href="/manage/install-and-configure-cli/" title="Azure CLI">Azure CLI</a></div>

##<a name="what-is-azure-powershell"></a>Wat is Azure PowerShell?
Azure PowerShell is een set van modules die cmdlets voor het beheren van Azure met Windows PowerShell. U kunt de cmdlets gebruiken om te maken, testen, implementeren en beheren oplossingen en services die worden geleverd via de Azure-platform. In de meeste gevallen kan de cmdlets worden gebruikt voor de taken van de Portal Azure, zoals het maken en configureren van cloud-services, virtuele machines, virtuele netwerken en web apps.

## <a name="how-versioning-works"></a>Hoe versiebeheer werkt

Azure PowerShell gebruikt semantische versiebeheer, wat dat als betekent versie A > B versie dan versie A is de meest recente API's. Het betekent ook dat wijzigingen in de primaire versie een recente in een of meer cmdlets wijzigen.  Dus bijvoorbeeld is versie 1.7.0 een hotfix voor een recente wijziging in de versies 1.x van Azure PowerShell.

Zie voor meer informatie over procedures in Azure PowerShell semantische Versioning, de semantische Versioning Specification op: http://semver.org
 
Als u de meest recente API's, moet u versie 2.x. Maar als u scripts die zijn geschreven met versie 1.x en u niet wilt dat voor het absorberen van de recente wijzigingen in versie 2.x wordt beschreven in de 2.x [Release-opmerkingen](https://github.com/Azure/azure-powershell/blob/dev/documentation/release-notes/migration-guide.2.0.0.md), en vervolgens moet u 1.7.0 installeren.

Een versieconflict kan tot gevolg hebben als een eerdere versie van een module die afhankelijk van het is verladen en de meest recente versie van de profiel-module is geïnstalleerd. De eenvoudigste manier om dit probleem oplossen is voor de installatie van het meest recente .msi. Het .msi ruimt oudere versies van de modules.
 
###<a name="installing-module-versions-side-by-side"></a>Versies naast elkaar module installeren

Versie 2.1.0 (versie 1.2.6 voor AzureStack) de eerste module versies zijn ontworpen om te worden geïnstalleerd en gebruikt naast elkaar. Omdat Azure PowerShell binaire modules gebruikt, moet u een nieuw venster met PowerShell openen en **Import-Module** gebruiken voor het importeren van een specifieke versie van de AzureRM-cmdlets:

    Import-Module AzureRM -RequiredVersion 2.1.0**

Versies voordat 2.1.0 (met uitzondering van 1.2.6) niet werken goed naast elkaar met andere versies van de module Azure PowerShell. Bij het laden van een eerdere versie van de Azure PowerShell-modules met behulp van een opdracht, zoals de afbeelding hierboven, niet-compatibele versies van de module **AzureRM.Profile** wordt geladen, waardoor de cmdlets gevraagd u aan te melden wanneer u een cmdlet, uitvoeren ook nadat u zich hebt aangemeld.

De eenvoudigste manier om het oplossen van dat dit is de meest recente Azure PowerShell installeren vanaf het WebPI-kanaal of .msi – dit verwijdert eerdere versies van de modules die zijn geïnstalleerd in de galerie. 

Houd er rekening mee dat Azure-en AzureRM afhankelijkheden met elkaar gemeen hebben, dus als u beide modules, bij het bijwerken van een gebruiken, moet u beide bijwerken. Eerdere versies van de module Azure hebben hetzelfde probleem met de side-by-side module laden van eerdere versies van de module AzureRM hebben.

<a id="Install"></a>
## <a name="step-1-install"></a>Stap 1: installeren

Hieronder staan de twee methoden waarmee u Azure PowerShell kunt installeren. U kunt het installeren van de galerie met PowerShell of de WebPI.

###<a name="installing-azure-powershell-from-the-powershell-gallery"></a>Azure PowerShell installeren vanaf de PowerShell-galerie

De beste manier is PowerShell galerie te gebruiken. U moet de module PowerShellGet de PowerShell-galerie te gebruiken. Dit is hier beschikbaar: [PowerShellGallery.com](https://www.powershellgallery.com/)

Azure PowerShell 1.3.0 installeren of hoger van de PowerShell-galerie met behulp van een verhoogde opdrachtprompt Windows PowerShell of PowerShell Integrated Scripting Environment (ISE) met de volgende opdrachten:

    # Install the Azure Resource Manager modules from the PowerShell Gallery
    Install-Module AzureRM

    # Install the Azure Service Management module from the PowerShell Gallery
    Install-Module Azure

####<a name="more-about-these-commands"></a>Meer informatie over deze opdrachten

- **AzureRM installatie-Module** installeert een updatepakket module voor de Azure Resource Manager-cmdlets. Afhankelijk van de module AzureRM 
- een bepaalde versiebereik voor elke module Azure Resource Manager. Het bereik van de meegeleverde versie zorgt ervoor dat er geen module recente wijzigingen opgenomen worden kunnen bij het installeren van AzureRM modules met dezelfde primaire versie. Tijdens de installatie van de module AzureRM wordt elke bronnenbeheerder Azure-module die eerder niet geïnstalleerd worden gedownload en geïnstalleerd in de galerie met PowerShell. Zie [semver.org](http://semver.org)voor meer informatie over de semantische versioning gebruikt door Azure PowerShell-modules. 
- **Installeer Module Azure** installeert de Azure module. In deze module wordt de module Servicebeheer van Azure PowerShell 0.9.x. Dit heeft geen grote wijzigingen en van de vorige versie van de module Azure uitwisselbaar.

###<a name="installing-azure-powershell-from-webpi"></a>Azure PowerShell installeren vanaf WebPI

Azure PowerShell 1.0 en hoger installeren vanaf WebPI is hetzelfde als het was voor 0.9.x. [Azure PowerShell](http://aka.ms/webpi-azps) downloaden en de installatie te starten. Als u beschikt over Azure PowerShell 0.9.x geïnstalleerd, versie 0.9.x als onderdeel van de upgrade wordt verwijderd. Als u uit galerie PowerShell modules voor Azure PowerShell hebt geïnstalleerd, verwijdert het installatieprogramma automatisch de modules voor de installatie op een consistente omgeving voor Azure PowerShell.

> [AZURE.NOTE] Als u in de galerie met PowerShell modules voor Azure eerder hebt geïnstalleerd, kan het installatieprogramma voor deze wordt automatisch verwijderd. Dit voorkomt verwarring over welke moduleversies die u hebt geïnstalleerd en waar ze zich bevinden. Galerie met PowerShell modules installeert gewoonlijk in **%ProgramFiles%\WindowsPowerShell\Modules**. Daarentegen WebPI wordt geïnstalleerd de Azure modules in * *% ProgramFiles (x86) %\Microsoft SDKs\Azure\PowerShell\**. Als een fout tijdens de installatie optreedt, kunt u handmatig de Azure verwijderen* mappen in de map **%ProgramFiles%\WindowsPowerShell\Modules** en probeer de installatie opnieuw.

Nadat de installatie is voltooid, de ```$env:PSModulePath``` instelling moet bevatten van de lijsten met de Azure PowerShell-cmdlets.

> [AZURE.NOTE] Er is een bekend probleem met PowerShell **$env: PSModulePath** die zich voordoen bij het installeren van WebPI. Als de computer opnieuw worden gestart door het systeemupdates of andere installaties moet, kan dit leiden tot updates **$env: PSModulePath** op het pad waar Azure PowerShell is geïnstalleerd. Als dit gebeurt, ziet u mogelijk een bericht 'cmdlet niet herkend' wanneer u probeert te Azure PowerShell-cmdlets gebruiken na de installatie of upgrade. Start het systeem moet in dat geval het probleem opgelost.

Als u een als het volgende bericht wanneer u probeert te laden of uitvoeren van cmdlets:

```
    PS C:\> Get-AzureRmResource
    Get-AzureRmResource : The term 'Get-AzureRmResource' is not recognized as the name of a cmdlet, function,
    script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is
    correct and try again.
    At line:1 char:1
    + Get-AzureRmResource
    + ~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : ObjectNotFound: (get-azurermresourcefork:String) [], CommandNotFoundException
        + FullyQualifiedErrorId : CommandNotFoundException
```

Dit kan worden gecorrigeerd door de computer opnieuw te starten of de cmdlets importeren uit C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\ als volgt (waarbij XXXX staat voor de versie van PowerShell is geïnstalleerd:

```
Import-Module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\azure.psd1"
Import-Module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\expressroute\expressroute.psd1"
```

## <a name="step-2-start"></a>Stap 2: Start
U kunt de cmdlets kunt uitvoeren vanuit de console van de standaard Windows PowerShell of van PowerShell Integrated Scripting Environment (ISE).
De methode die u gebruikt voor het openen van de console is afhankelijk van de versie van Windows u uitvoert:

- Op een computer met minimaal Windows 8 of Windows Server 2012, kunt u de ingebouwde zoeken. Beginnen met typen voeding vanuit **het startscherm** . Hiermee wordt een bereik lijst van toepassingen met Windows PowerShell. Als de console wilt openen, klikt u op beide app. (Als u wilt de app op **het startscherm** vastmaken, met de rechtermuisknop op het pictogram.)

- Gebruik het **menu Start**op een computer met een versie ouder dan Windows 8 of Windows Server 2012. In het menu **Start** op **Alle programma's**, **Bureau-accessoires**klikt u op de map **Windows PowerShell** en klik vervolgens op **Windows PowerShell**.

U kunt ook de **Windows PowerShell ISE** gebruik van sneltoetsen en menu-items voor het uitvoeren van veel van de taken die u in de Windows PowerShell-console uitvoeren wilt uitvoeren. De ISE, in de Windows PowerShell-console Cmd.exe, of in het vak **uitvoeren** , typt, **powershell_ise.exe**.

###<a name="commands-to-help-you-get-started"></a>Opdrachten voor het aan de slag

    # To make sure the Azure PowerShell module is available after you install
    Get-Module –ListAvailable 
    
    # To log in to Azure Resource Manager
    Login-AzureRmAccount

    # You can also use a specific Tenant if you would like a faster log in experience
    # Login-AzureRmAccount -TenantId xxxx

    # To view all subscriptions for your account
    Get-AzureRmSubscription

    # To select a default subscription for your current session
    Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

    # View your current Azure PowerShell session context
    # This session state is only applicable to the current session and will not affect other sessions
    Get-AzureRmContext

    # To select the default storage context for your current session
    Set-AzureRmCurrentStorageAccount –ResourceGroupName “your resource group” –StorageAccountName “your storage account name”

    # View your current Azure PowerShell session context
    # Note: the CurrentStorageAccount is now set in your session context
    Get-AzureRmContext

    # To list all of the blobs in all of your containers in all of your accounts
    Get-AzureRmStorageAccount | Get-AzureStorageContainer | Get-AzureStorageBlob


## <a name="step-3-connect"></a>Stap 3: verbinding maken
De cmdlets moet uw abonnement zodat ze uw services kunnen beheren. Als u nog niet hebt, kunt u een Azure-abonnement aanschaffen. Zie voor instructies de [Azure aanschaffen](http://go.microsoft.com/fwlink/p/?LinkId=320795).

1. Type **Aanmelding AzureRmAccount**

2. Typ het e-mailadres en wachtwoord aan uw account gekoppeld. Azure verifieert de referentie-informatie wordt opgeslagen en vervolgens wordt het venster gesloten.

--EN--

Inloggen op uw werk of school account:

    $cred = Get-Credential
    Login-AzureRmAccount -Credential $cred
> [AZURE.NOTE] Als u meer dan één huurder die is gekoppeld aan uw organisatie-account hebt, moet u de parameter TenantId opgeven:

    $loadersubscription = Get-AzureRmSubscription -SubscriptionName $YourSubscriptionName -TenantId $YourAssociatedSubscriptionTenantId


> [AZURE.NOTE] Deze niet-interactief aanmelden methode werkt alleen met een account voor werk of school. Een werk of school-account is een gebruiker die wordt beheerd door uw werk of op school en gedefinieerd in de Azure Active Directory-instantie voor uw werk of op school. Als u niet de account van een werk of school hebt en van een Microsoft-account gebruikmaakt aan te melden uw abonnement Azure, kunt u gemakkelijk maken met behulp van de volgende stappen uit.

> 1. Log in op de [Azure klassieke portal](https://manage.windowsazure.com)en klik op **Active Directory**.

> 2. Als de map niet bestaat, selecteert u **de map maken** en geef de gevraagde gegevens.

> 3. Selecteer de map en een nieuwe gebruiker toevoegen. Deze nieuwe gebruiker kunt aanmelden met een account voor werk of school. Tijdens het maken van de gebruiker die u hebt opgegeven met zowel een e-mailadres van de gebruiker en een tijdelijk wachtwoord. Deze informatie opslaan, zoals het wordt gebruikt in stap 5 hieronder.

> 4. Selecteer **Instellingen** en selecteer **beheerders**uit de Azure klassieke portal. Selecteer de optie **toevoegen**en de nieuwe gebruiker toevoegen als beheerder van een collega. Hiermee kunt het werk of school account voor het beheer van uw abonnement op Azure.

> 5. Ten slotte zijn afgemeld bij de klassieke Azure portal en meld u vervolgens weer aan met behulp van het werk of school account. Als dit de eerste keer aanmelden met deze account, wordt u gevraagd het wachtwoord te wijzigen.

> Zie [aanmelden bij Microsoft Azure als organisatie](./active-directory/sign-up-organization.md)voor meer informatie over het ondertekenen van Microsoft Azure met een account voor werk of school.

> Zie voor meer informatie over verificatie en abonnement management in Azure, [Accounts beheren, abonnementen, en administratieve rollen](http://go.microsoft.com/fwlink/?LinkId=324796).

### <a name="view-account-and-subscription-details"></a>Details van account en abonnement

U kunt meerdere rekeningen en beschikbaar voor gebruik door Azure PowerShell abonnementen hebben. U kunt meerdere accounts toevoegen door meer dan één keer **Toevoegen AzureRmAccount** uit te voeren.

Als u de beschikbare accounts Azure, typt u de **Get-AzureAccount**.

Als u uw abonnementen Azure, typt u de **Get-AzureRmSubscription**.

##<a id="Help"></a>Help-informatie opvragen##

Deze resources biedt ondersteuning voor specifieke cmdlets:


-   Vanuit de console kunt u de ingebouwde Help-systeem. De cmdlet **Get-Help** biedt toegang tot dit systeem. 

- Probeer deze populaire forums voor hulp van de Gemeenschap:

 - [Azure-forum op MSDN]( http://go.microsoft.com/fwlink/p/?LinkId=320212)
 - [Stackoverflow](http://go.microsoft.com/fwlink/?LinkId=320213)

##<a name="learn-more"></a>Meer informatie


Zie de volgende bronnen voor meer informatie over de cmdlets gebruiken:

Zie [Windows PowerShell gebruiken](http://go.microsoft.com/fwlink/p/?LinkId=321939)voor elementaire instructies over het gebruik van Windows PowerShell.

Zie voor informatie over de cmdlets [Azure Cmdlet referentie](https://msdn.microsoft.com/library/windowsazure/jj554330.aspx).

Zie voor voorbeelden van scripts en instructies waarmee u leren Azure beheren met scripts het [Script Center](http://go.microsoft.com/fwlink/p/?LinkId=321940).

