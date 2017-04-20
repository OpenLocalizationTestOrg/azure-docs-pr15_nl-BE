<properties
    pageTitle="PHP web en werknemer rollen | Microsoft Azure"
    description="Een handleiding voor de PHP-web- en werknemer rollen in een Azure cloud-service maken en configureren van de PHP-runtime."
    services=""
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

#<a name="how-to-create-php-web-and-worker-roles"></a>PHP web en werknemer rollen maken

## <a name="overview"></a>Overzicht

Deze handleiding wordt beschreven hoe u PHP web of werknemer functies maken in een ontwikkelomgeving voor Windows, kiest u een specifieke versie van PHP in de 'ingebouwde' versies beschikbaar, wijzigen van de configuratie van PHP, -extensies inschakelen en tot slot implementeren op Azure. Het wordt ook beschreven hoe u een rol web of de werknemer voor het gebruik van een PHP-runtime (met aangepaste configuratie en uitbreidingen) die u opgeeft.

## <a name="what-are-php-web-and-worker-roles"></a>Wat zijn PHP web en werknemer rollen?

Azure biedt drie modellen voor het uitvoeren van toepassingen berekenen: Azure App Service, Azure virtuele Machines en Azure Cloud Services. Alle drie de modellen ondersteuning voor PHP. Cloud-Services, inclusief rollen web en werknemer, biedt een *platform als service (PaaS)*. In een cloud-service biedt een Webrol een specifieke Internet Information Services (IIS)-webserver front-end web host voor toepassingen. De rol van een werknemer asynchrone, langdurige of permanente taken onafhankelijk van de interactie van de gebruiker of de invoer kan worden uitgevoerd.

Voor meer informatie over deze opties Zie [Opties op Azure hosting berekenen](./cloud-services/cloud-services-choose-me.md).

## <a name="download-the-azure-sdk-for-php"></a>De Azure SDK voor PHP downloaden

De [SDK voor PHP Azure] bestaat uit verschillende onderdelen. In dit artikel worden twee van hen gebruikt: Azure PowerShell en de Azure emulators. Deze twee onderdelen kunnen worden geïnstalleerd via Microsoft Web Platform Installer. Zie voor meer informatie [het installeren en configureren van Azure PowerShell](powershell-install-configure.md).

## <a name="create-a-cloud-services-project"></a>Een Cloud Services-project maken

De eerste stap bij het maken van een PHP-web- of werknemer rol is een project Azure Service maken. een project Azure Service fungeert als een logische container voor web- en werknemer rollen en bevat de [definitie van service (.csdef)] en de [configuratie van de service (.cscfg)] bestanden van het project.

Maak een nieuw project voor Azure Service, Azure PowerShell uitvoeren als administrator en voer de volgende opdracht uit:

    PS C:\>New-AzureServiceProject myProject

Met deze opdracht maakt u een nieuwe map (`myProject`) u web- en werknemer rollen kunt toevoegen.

## <a name="add-php-web-or-worker-roles"></a>PHP web of werknemer rollen toevoegen

Om de functie van PHP web toevoegen aan een project, met de volgende opdracht uit in de hoofdmap van het project worden uitgevoerd:

    PS C:\myProject> Add-AzurePHPWebRole roleName

Gebruik deze opdracht voor een rol werknemer:

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [AZURE.NOTE] De `roleName` -parameter is optioneel. Als dit wordt weggelaten, wordt de naam van de functie automatisch gegenereerd. De Webrol van eerste gemaakt zijn `WebRole1`, is de tweede `WebRole2`, enzovoort. De eerste functie van werknemer gemaakt zijn `WorkerRole1`, is de tweede `WorkerRole2`, enzovoort.

## <a name="specify-the-built-in-php-version"></a>Geef de ingebouwde PHP versie

Wanneer u een PHP-web- of werknemer rol aan een project toevoegt, de configuratiebestanden van het project gewijzigd zodat PHP wordt geïnstalleerd op elk exemplaar van het web of een werknemer van uw toepassing als deze is geïmplementeerd. Overzicht van de versie van PHP die standaard wordt geïnstalleerd, moet u de volgende opdracht uitvoeren:

    PS C:\myProject> Get-AzureServiceProjectRoleRuntime

De uitvoer van de bovenstaande opdracht ziet er ongeveer als wat wordt hieronder weergegeven. In dit voorbeeld wordt de `IsDefault` vlag is ingesteld op `true` voor PHP 5.3.17, die aangeeft dat ze zal de standaard PHP versie is geïnstalleerd.

    Runtime Version     PackageUri                      IsDefault
    ------- -------     ----------                      ---------
    Node 0.6.17         http://nodertncu.blob.core...   False
    Node 0.6.20         http://nodertncu.blob.core...   True
    Node 0.8.4          http://nodertncu.blob.core...   False
    IISNode 0.1.21      http://nodertncu.blob.core...   True
    Cache 1.8.0         http://nodertncu.blob.core...   True
    PHP 5.3.17          http://nodertncu.blob.core...   True
    PHP 5.4.0           http://nodertncu.blob.core...   False

De PHP runtime-versie kunt u instellen op een van de PHP-versies die worden vermeld. Bijvoorbeeld voor het instellen van de PHP-versie (voor een rol met de naam `roleName`) naar 5.4.0, gebruikt u de volgende opdracht:

    PS C:\myProject> Set-AzureServiceProjectRole roleName php 5.4.0

> [AZURE.NOTE] Beschikbare versies van PHP kunnen in de toekomst veranderen.

## <a name="customize-the-built-in-php-runtime"></a>De ingebouwde PHP runtime aanpassen

U hebt volledige controle over de configuratie van de PHP runtime is geïnstalleerd wanneer u de stappen hierboven, met inbegrip van de wijziging van de `php.ini` -instellingen en -extensies inschakelen.

Als u wilt aanpassen op de ingebouwde PHP runtime, volg deze stappen:

1. Toevoegen van een nieuwe map met de naam `php`, naar de `bin` map van uw website. Voor de functie van een werknemer toe te voegen aan de hoofdmap van de rol.
2. In de `php` map, maakt u een andere map met de naam `ext`. Plaatsen van een `.dll` extensiebestanden (bijv. `php_mongo.dll`) die u wilt inschakelen in deze map.
3. Voeg toe een `php.ini` van het bestand in de `php` map. Eventuele aangepaste extensies inschakelen en instellen van een PHP-richtlijnen in dit bestand. Bijvoorbeeld, als u wilt schakelen `display_errors` op en de `php_mongo.dll` -extensie, de inhoud van uw `php.ini` bestand worden als volgt:

        display_errors=On
        extension=php_mongo.dll

> [AZURE.NOTE] Alle instellingen die u niet expliciet worden ingesteld in de `php.ini` bestand op te geven wordt automatisch ingesteld op de standaardwaarden. Houd er echter rekening mee dat u een complete toevoegen kunt `php.ini` bestand.

## <a name="use-your-own-php-runtime"></a>Gebruik uw eigen PHP-runtime
In sommige gevallen, in plaats van een ingebouwde PHP runtime te selecteren en te configureren zoals hierboven beschreven, kunt u uw eigen PHP-runtime bieden. U kunt bijvoorbeeld de PHP-runtime van dezelfde in een web of een werknemer de rol die u kunt in uw ontwikkelomgeving gebruiken. Dit maakt het gemakkelijker om ervoor te zorgen dat de toepassing het probleem in uw productieomgeving niet wijzigen.

### <a name="configure-a-web-role-to-use-your-own-php-runtime"></a>Configureren van een web-functie voor het gebruik van uw eigen PHP-runtime

Configureren van een web-functie voor het gebruik van een PHP runtime die u als volgt te werk:

1. Een project Azure Service maken en toevoegen van een PHP Webrol zoals eerder in dit onderwerp is beschreven.
2. Maak een `php` map in de `bin` map die zich in de hoofdmap van uw web-functie en vervolgens de PHP-runtime (alle binaire bestanden, configuratiebestanden, submappen, enzovoort) toevoegen aan de `php` map.
3. (OPTIONEEL) Als uw PHP runtime de [Microsoft-stuurprogramma's voor PHP voor SQL Server gebruikt][sqlsrv drivers], moet u voor het configureren van uw Webrol als u wilt installeren van [SQL Server Native Client-2012] [ sql native client] wanneer deze is ingericht. Hiertoe toevoegen de [installer sqlncli.msi x64] de `bin` map in de hoofdmap van uw web-functie. Het opstartscript dat wordt beschreven in de volgende stap wordt zonder het installatieprogramma uitgevoerd wanneer de rol wordt ingericht. Als uw PHP runtime geen Microsoft-Drivers voor PHP voor SQL Server gebruikt, kunt u de volgende regel verwijderen uit het script weergegeven in de volgende stap:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

4. Een taak is gestart voor het configureren van [Internet Information Services (IIS)] definieert[ iis.net] uw PHP runtime gebruiken voor het verwerken van aanvragen voor `.php` pagina's. Hiertoe opent u de `setup_web.cmd` bestand (in de `bin` -bestand van de hoofdmap van uw web-functie) in een teksteditor en de inhoud ervan wilt vervangen door het volgende script:

        @ECHO ON
        cd "%~dp0"

        if "%EMULATED%"=="true" exit /b 0

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

        SET PHP_FULL_PATH=%~dp0php\php-cgi.exe
        SET NEW_PATH=%PATH%;%RoleRoot%\base\x86

        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%',maxInstances='12',idleTimeout='60000',activityTimeout='3600',requestTimeout='60000',instanceMaxRequests='10000',protocol='NamedPipe',flushNamedPipe='False']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PATH',value='%NEW_PATH%']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PHP_FCGI_MAX_REQUESTS',value='10000']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/handlers /+"[name='PHP',path='*.php',verb='GET,HEAD,POST',modules='FastCgiModule',scriptProcessor='%PHP_FULL_PATH%',resourceType='Either',requireAccess='Script']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /"[fullPath='%PHP_FULL_PATH%'].queueLength:50000"

5. De toepassingsbestanden naar de hoofdmap van de rol van uw webpagina toevoegen. Dit is de hoofdmap van de webserver.

6. Uw toepassing zoals beschreven in de onderstaande sectie van [uw toepassing publiceren](#how-to-publish-your-application) publiceren.

> [AZURE.NOTE] De `download.ps1` script (in de `bin` map van de hoofdmap van de Webrol) kunnen worden verwijderd nadat u de stappen beschreven voor het gebruik van uw eigen PHP-runtime.

### <a name="configure-a-worker-role-to-use-your-own-php-runtime"></a>De rol van een werknemer voor het gebruik van uw eigen PHP runtime configureren

Configureren van de rol van een werknemer voor het gebruik van een PHP runtime die u als volgt te werk:

1. Een project Azure Service maken en toevoegen van een werknemer PHP functie zoals eerder in dit onderwerp is beschreven.
2. Maak een `php` map in de hoofdmap van de rol van de werknemer, en vervolgens de PHP-runtime (alle binaire bestanden, configuratiebestanden, submappen, enzovoort) toevoegen aan de `php` map.
3. (OPTIONEEL) Als uw PHP runtime wordt gebruikt voor [Microsoft-stuurprogramma's voor PHP voor SQL Server][sqlsrv drivers], moet u voor het configureren van uw rol werknemer om te installeren van [SQL Server Native Client-2012] [ sql native client] wanneer deze is ingericht. Voeg de [installer sqlncli.msi x64] naar de hoofdmap van de rol van de werknemer hiervoor. Het opstartscript dat wordt beschreven in de volgende stap wordt zonder het installatieprogramma uitgevoerd wanneer de rol wordt ingericht. Als uw PHP runtime geen Microsoft-Drivers voor PHP voor SQL Server gebruikt, kunt u de volgende regel verwijderen uit het script weergegeven in de volgende stap:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

4. Definieer een taak is gestart, die wordt toegevoegd uw `php.exe` uitvoerbaar aan de rol van de werknemer omgevingsvariabele PATH wanneer de rol wordt ingericht. Hiertoe opent u de `setup_worker.cmd` (in de hoofdmap van de rol van de werknemer)-bestand in een teksteditor en de inhoud ervan vervangen door het volgende script:

        @echo on

        cd "%~dp0"

        echo Granting permissions for Network Service to the web root directory...
        icacls ..\ /grant "Network Service":(OI)(CI)W
        if %ERRORLEVEL% neq 0 goto error
        echo OK

        if "%EMULATED%"=="true" exit /b 0

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

        setx Path "%PATH%;%~dp0php" /M

        if %ERRORLEVEL% neq 0 goto error

        echo SUCCESS
        exit /b 0

        :error

        echo FAILED
        exit /b -1

5. Voeg de toepassingsbestanden naar de hoofdmap van de rol van de werknemer.

6. Uw toepassing zoals beschreven in de onderstaande sectie van [uw toepassing publiceren](#how-to-publish-your-application) publiceren.

## <a name="run-your-application-in-the-compute-and-storage-emulators"></a>Uw toepassing uitvoeren in de opslag en rekenkracht emulators

De Azure emulators bieden een lokale omgeving waarin u uw Azure toepassing testen kunt voordat u deze naar de cloud distribueert. Er zijn enkele verschillen tussen de emulators en de Azure-omgeving. Om dit beter te begrijpen, Zie [Gebruik de emulator Azure opslag voor ontwikkelen en testen](./storage/storage-use-emulator.md).

Houd er rekening mee dat PHP lokaal voor het gebruik van de compute-emulator geïnstalleerd moet zijn. De compute-emulator gebruikt uw lokale installatie van PHP uw toepassing uit te voeren.

Uw project wordt uitgevoerd in de emulators, uitvoeren met de volgende opdracht vanaf de hoofdmap van uw project:

    PS C:\MyProject> Start-AzureEmulator

Ziet u uitvoer die vergelijkbaar is met dit:

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

U kunt uw toepassing in de emulator wordt uitgevoerd door een webbrowser te openen en te bladeren naar het lokale adres weergegeven in de uitvoer zien (`http://127.0.0.1:81` in het bovenstaande voorbeeld van uitvoer).

Als u wilt stoppen met de emulators, deze opdracht uitvoeren:

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a>Publiceer de toepassing

Voor het publiceren van uw toepassing, moet u eerst importeren uw publicatie-instellingen via de cmdlet [Import-en AzurePublishSettingsFile](https://msdn.microsoft.com/library/azure/dn790370.aspx) . Vervolgens kunt u uw toepassing met behulp van de cmdlet [Publiceren AzureServiceProject](https://msdn.microsoft.com/library/azure/dn495166.aspx) publiceren. Zie voor informatie over het aanmelden, [het installeren en configureren van Azure PowerShell](powershell-install-configure.md).

## <a name="next-steps"></a>Volgende stappen

Zie de [PHP Developer Center](/develop/php/)voor meer informatie.

[Azure SDK voor PHP]: /develop/php/common-tasks/download-php-sdk/
[install ps and emulators]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[definitie van service (.csdef)]: http://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[configuratie van de service (.cscfg)]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: http://www.iis.net/
[sql native client]: http://msdn.microsoft.com/sqlserver/aa937733.aspx
[sqlsrv drivers]: http://php.net/sqlsrv
[SQLNCLI.msi x64-installer]: http://go.microsoft.com/fwlink/?LinkID=239648
