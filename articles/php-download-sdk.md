<properties
    pageTitle="De Azure SDK voor PHP downloaden"
    description="Informatie over het downloaden en installeren van de SDK Azure voor PHP."
    documentationCenter="php"
    services="app-service\web"
    authors="allclark"
    manager="douge"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="06/01/2016"
    ms.author="allclark;yaqiyang"/>

#<a name="download-the-azure-sdk-for-php"></a>De Azure SDK voor PHP downloaden

## <a name="overview"></a>Overzicht

De SDK Azure voor PHP bevat onderdelen waarmee u kunt ontwikkelen, implementeren en beheren van toepassingen voor Azure PHP. De Azure SDK voor PHP bevat de volgende gegevens op:

* **PHP de client libraries voor Azure**. Deze klassenbibliotheken bieden een interface voor toegang tot Azure-functies, zoals het beheer van gegevens en cloud services.  
* **De Azure opdrachtregelinterface voor Mac, Linux en Windows (Azure CLI)**. Dit is een verzameling opdrachten voor het implementeren en beheren van Azure services, zoals Websites Azure en Azure virtuele Machines. Het werk van Azure CLI op elk platform, zoals Mac, Linux en Windows.
* **Azure PowerShell (alleen Windows)**. Dit is een set van PowerShell-cmdlets voor het implementeren en beheren van Azure Services, zoals Cloud Services en virtuele Machines.
* **De Azure-Emulators (alleen Windows)**. De opslag en rekenkracht emulators zijn lokale emulators van cloud en data management services kunnen u een toepassing lokaal te testen. De Azure-Emulators alleen onder Windows worden uitgevoerd.

In de volgende secties wordt beschreven hoe u downloaden en installeren van de bovengenoemde onderdelen.

De instructies in dit onderwerp wordt ervan uitgegaan dat er [PHP] [ install-php] geïnstalleerd.

> [AZURE.NOTE] U moet PHP 5.5 of hoger gebruikt de client PHP bibliotheken voor Azure hebben.

##<a name="php-client-libraries-for-azure"></a>PHP clientbibliotheken voor Azure

De PHP Client Libraries voor Azure biedt een interface voor toegang tot Azure-functies, zoals het beheer van gegevens en cloud services op een ander besturingssysteem. Deze bibliotheken kunnen worden geïnstalleerd via de componist.

Zie [de Blob-Service gebruiken]voor informatie over het gebruik van de PHP Client Libraries voor Azure,[blob-service], [het gebruik van de Service van de tabel] [ table-service] en [het gebruik van de Service van de wachtrij][queue-service].

###<a name="install-via-composer"></a>Installeren via de componist

1. [Git installeren][install-git].


    > [AZURE.NOTE] In Windows moet u ook de uitvoerbare Git toevoegen aan de omgevingsvariabele PATH.

2. Maak een bestand met de naam **composer.json** in de hoofdmap van het project en de volgende code aan toe te voegen:

        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }

3. Download ** [composer.phar] [ composer-phar] ** in de hoofdmap van het project.

4. Open een opdrachtprompt en dit uitvoeren in de hoofdmap van het project

        php composer.phar install

##<a name="azure-powershell-and-azure-emulators"></a>Azure PowerShell en Azure-emulatoren

Azure PowerShell is een set van PowerShell-cmdlets voor het implementeren en beheren van Azure Services (zoals Cloud Services en virtuele Machines). De Azure-Emulators zijn emulators van cloud en data management services kunnen u een toepassing lokaal testen. Deze onderdelen worden ondersteund alleen Windows.

De aanbevolen manier Azure PowerShell en de Azure-Emulators installeren met de [Microsoft Web Platform Installer]is[download-wpi]. Opmerking u kunt ook kiezen voor de installatie van andere onderdelen van de ontwikkeling, zoals PHP, SQL Server, de Microsoft-Drivers voor SQL Server voor PHP en WebMatrix.

Zie [How to Azure PowerShell gebruiken]voor informatie over het gebruik van Azure PowerShell[powershell-tools].

##<a name="azure-cli"></a>Azure CLI

De CLI Azure is een verzameling opdrachten voor het implementeren en beheren van Azure services, zoals Websites Azure en Azure virtuele Machines. Zie [de Azure CLI installeren](xplat-cli-install.md)voor meer informatie over het installeren van Azure CLI.

## <a name="next-steps"></a>Volgende stappen

Zie de [PHP Developer Center](/develop/php/)voor meer informatie.


[install-php]: http://www.php.net/manual/en/install.php
[composer-github]: https://github.com/composer/composer
[composer-phar]: http://getcomposer.org/composer.phar
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[download-wpi]: http://go.microsoft.com/fwlink/?LinkId=253447
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[blob-service]: http://go.microsoft.com/fwlink/?LinkId=252714
[table-service]: http://go.microsoft.com/fwlink/?LinkId=252715
[queue-service]: http://go.microsoft.com/fwlink/?LinkId=252716
[azure cli]: http://go.microsoft.com/fwlink/?LinkId=252717
[powershell-tools]: http://go.microsoft.com/fwlink/?LinkId=252718
[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
