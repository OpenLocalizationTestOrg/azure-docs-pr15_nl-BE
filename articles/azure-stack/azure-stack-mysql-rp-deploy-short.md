<properties
    pageTitle="MySQL-databases gebruiken als PaaS op Azure Stack | Microsoft Azure"
    description="De snelle stappen voor het implementeren van de MySQL bron provider en MySQL bieden als een service op de Stack Azure begrijpen."
    services="azure-stack"
    documentationCenter=""
    authors="Dumagar"
    manager="bradleyb"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="dumagar"/>

# <a name="use-mysql-databases-as-paas-on-azure-stack"></a>MySQL-databases gebruiken als PaaS op Azure Stack

> [AZURE.NOTE] De volgende informatie geldt alleen voor Azure Stack TP1-implementaties.

U kunt een MySQL resource provider op Azure Stack implementeren. Nadat u de resource-provider hebt geïmplementeerd, kunt u MySQL-servers en databases via Azure Resource Manager implementatie sjablonen maken en MySQL-databases bieden als een service. MySQL-databases kunnen gebruikt op websites worden, ondersteunen veel website-platforms. Nadat u de resource-provider hebt geïmplementeerd, kunt u WordPress websites van het Web Apps van Azure-platform als een invoegtoepassing voor de service (PaaS) voor Azure Stack.

## <a name="quick-steps-to-deploy-the-resource-provider"></a>Snelle stappen voor de implementatie van de resource-provider
Volg deze procedure als u al bekend met Azure Stack bent. Als u meer informatie wilt, volgt u de koppelingen in elke sectie of Ga direct naar [de MySQL Database Resource Provider Adapter op Azure Stack Haalbaarheidstest implementeren](azure-stack-mysql-rp-deploy-long.md).

1.  Zorg ervoor dat u [alle instellen stappen voltooien](azure-stack-mysql-rp-deploy-long.md#set-up-steps-before-you-deploy) voordat u de resource-provider:

    - .NET 3.5 framework is al ingesteld in de basisinstallatiekopie van Windows Server. (Als u de bits Azure Stack gedownload na februari 23, 2016, kunt u deze stap overslaan.)
    - [Een uitgave van Azure PowerShell die compatibel is met Azure-Stack is geïnstalleerd](http://aka.ms/azStackPsh).
    - In Internet Explorer de beveiligingsinstellingen op de ClientVM, [Internet Explorer Verbeterde beveiliging is uitgeschakeld en cookies zijn ingeschakeld](azure-stack-mysql-rp-deploy-long.md#Turn-off-IE-enhanced-security-and-enable-cookies).

2. [Downloaden van de MySQL-provider binaire bronbestand](http://aka.ms/masmysqlrp) en pak het bestand op de ClientVM in uw bewijs van haalbaarheidstest Azure-Stack.

3. [Bootstrap.cmd en de scripts worden uitgevoerd](azure-stack-mysql-rp-deploy-long.md#Bootstrap-the-resource-provider-deployment-PowerShell-and-Prepare-for-deployment).

    Een set scripts die is gegroepeerd op twee belangrijke tabbladen wordt geopend in de PowerShell Integrated Scripting Environment (ISE). De geladen scripts uitvoeren in volgorde van links naar rechts in elk tabblad.

    1. Een script uitvoeren op het tabblad **voorbereiden** van links naar rechts voor:

        - Maak een wildcard certificaat om beveiligde communicatie tussen de bronnen en Azure Resource Manager.
        - Accepteer de voorwaarden van de overeenkomst voor MySQL en download de binaire bestanden van MySQL.
        - De certificaten en alle andere onderdelen uploaden naar een storage Stack Azure account.
        - Galerie pakketten publiceren zodat u MySQL-bronnen door de galerie kunt implementeren.

        > [AZURE.IMPORTANT] Als een van de scripts zonder aanwijsbare reden vastloopt nadat u uw huurder Azure Active Directory indienen, blokkeert door uw beveiligingsinstellingen een DLL-bestand dat is vereist voor de implementatie uit te voeren. Dit probleem op te lossen voor de Microsoft.AzureStack.Deployment.Telemetry.Dll in de map resource provider zoeken, op de rechtermuisknop klikt u op **Eigenschappen**en Controleer op het tabblad **Algemeen** op **blokkering opheffen** .

    2. Een script uitvoeren op het tabblad **implementeren** van links naar rechts aan:

        - [Een virtuele machine (VM) implementeren](azure-stack-mysql-rp-deploy-long.md#Deploy-the-MySQLResource-Provider-VM) die host is van zowel de resource provider, MySQL servers en databases die u zal instantiëren. Dit script verwijst naar een JSON parameter-bestand dat u nodig hebt om te werken met bepaalde waarden voordat u het script uitvoert.
        - [Een lokale DNS-record registreren](azure-stack-mysql-rp-deploy-long.md#Update-the-local-DNS) die worden toegewezen aan de resource provider VM.
        - [Registreer uw provider resource](azure-stack-mysql-rp-deploy-long.md#Register-the-MySQL-RP-Resource-Provider) met de lokale Azure Resource Manager.

        > [AZURE.IMPORTANT] Alle scripts wordt ervan uitgegaan dat de installatiekopie van het besturingssysteem voldoet aan de vereisten (.NET 3.5 geïnstalleerd, JavaScript en cookies zijn ingeschakeld op de ClientVM en de meest recente versie van Azure PowerShell is geïnstalleerd). Als u foutmeldingen ontvangt wanneer u de scripts uitvoert, controleert u nogmaals dat u de voorwaarden hebt voldaan.

5. [Test uw nieuwe MySQL resource provider](/azure-stack-MySql-rp-deploy-long.md#create-your-first-mysql-database-to=test-your-deployment), een MySQL-database vanaf de portal Azure Stack te implementeren. **Klik op** &gt; **aangepaste** &gt; **MySQL-Server en de Database**.

Dit moet krijgen uw provider MySQL bron van ongeveer 25 minuten.
