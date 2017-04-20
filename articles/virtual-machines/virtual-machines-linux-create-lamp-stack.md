<properties
    pageTitle="LAMP installeren op een virtuele machine voor Linux | Microsoft Azure"
    description="Informatie over het installeren van de LAMP stack op een Linux VM"
    services="virtual-machines-linux"
    documentationCenter="virtual-machines"
    authors="jluk"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="NA"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="juluk"/>

# <a name="deploy-lamp-stack-on-azure"></a>LAMP Stack op Azure implementeren
Dit artikel helpt u bij het implementeren van een Apache webserver, MySQL en PHP (de LAMP stack) op Azure. U moet een Azure-Account ([gratis proberen](https://azure.microsoft.com/pricing/free-trial/)) en de [CLI Azure](../xplat-cli-install.md) die is [aangesloten op uw account Azure](../xplat-cli-connect.md).

Er zijn twee methoden voor het installeren van licht die in dit artikel:

## <a name="quick-command-summary"></a>Overzicht snelle opdracht

1) LICHT op nieuwe VM implementeren

```
# One command to create a resource group holding a VM with LAMP already on it
$ azure group create -n uniqueResourceGroup -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
```

2) LICHT op een bestaande VM implementeren

```
# Two commands: one updates packages, the other installs Apache, MySQL, and PHP
user@ubuntu$ sudo apt-get update
user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a>LICHT op nieuwe VM-scenario te implementeren

U kunt starten met het maken van een nieuwe [resourcegroep](../azure-resource-manager/resource-group-overview.md) maken met de VM:

    $ azure group create uniqueResourceGroup westus
    info:    Executing command group create
    info:    Getting resource group uniqueResourceGroup
    info:    Creating resource group uniqueResourceGroup
    info:    Created resource group uniqueResourceGroup
    data:    Id:                  /subscriptions/########-####-####-####-############/resourceGroups/uniqueResourceGroup
    data:    Name:                uniqueResourceGroup
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

Om de VM zelf maakt, kunt u een reeds geschreven Azure Resource Manager sjabloon vinden [hier op GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).

    $ azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json uniqueResourceGroup uniqueLampName

Een antwoord te vragen sommige meer ingangen worden weergegeven:

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    storageAccountNamePrefix: lampprefix
    location: westus
    adminUsername: someUsername
    adminPassword: somePassword
    mySqlPassword: somePassword
    dnsLabelPrefix: azlamptest
    info:    Initializing template configurations and parameters
    info:    Creating a deployment
    info:    Created template deployment "uniqueLampName"
    info:    Waiting for deployment to complete
    data:    DeploymentName     : uniqueLampName
    data:    ResourceGroupName  : uniqueResourceGroup
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          :
    data:    Mode               : Incremental
    data:    CorrelationId      : d51bbf3c-88f1-4cf3-a8b3-942c6925f381
    data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
    data:    ContentVersion     : 1.0.0.0
    data:    DeploymentParameters :
    data:    Name                      Type          Value
    data:    ------------------------  ------------  -----------
    data:    storageAccountNamePrefix  String        lampprefix
    data:    location                  String        westus
    data:    adminUsername             String        someUsername
    data:    adminPassword             SecureString  undefined
    data:    mySqlPassword             SecureString  undefined
    data:    dnsLabelPrefix            String        azlamptest
    data:    ubuntuOSVersion           String        14.04.2-LTS
    info:    group deployment create command OK

U hebt nu een Linux VM gemaakt met licht al is geïnstalleerd. Als u wilt, kunt u de installatie controleren door eroverheen te springen naar [controleren of LAMP geïnstalleerd].

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a>LICHT op een bestaande VM-scenario te implementeren

Als u hulp bij het maken van een Linux VM head u [hier voor meer informatie over het maken van een Linux VM] (. / virtual-machines-linux-quick-create-cli.md). Vervolgens moet u aan SSH in Linux VM. Als u hulp nodig hebt bij het maken van een SSH-sleutel head u [hier voor meer informatie over het maken van een SSH-sleutel op Linux/Mac] (. / virtual-machines-linux-mac-create-ssh-keys.md).
Als u al een SSH-sleutel hebt, gaat u vooruit en SSH in uw Linux VM met `ssh username@uniqueDNS`.

Nu dat u binnen uw Linux VM werkt, doorlopen we de LAMP stack installeren op Debian gebaseerde distributies. De exacte opdrachten, hangt af van andere Linux distros.

#### <a name="installing-on-debianubuntu"></a>Installeren op Debian/Ubuntu

U moet de volgende pakketten geïnstalleerd: `apache2`, `mysql-server`, `php5`, en `php5-mysql`. U kunt deze rechtstreeks worden met Tasksel of toepassen van deze pakketten te installeren. Hieronder vindt u instructies voor beide opties.
Voordat u moet downloaden en bijwerken van lijsten van pakket.

    user@ubuntu$ sudo apt-get update
    
##### <a name="individual-packages"></a>Afzonderlijke pakketten
Met behulp van apt-get:

    user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql

##### <a name="using-tasksel"></a>Met behulp van Tasksel
U kunt ook downloaden Tasksel, een hulpprogramma voor Debian/Ubuntu meerdere gerelateerde pakketten als een gecoördineerde 'taak' op uw systeem installeert.

    user@ubuntu$ sudo apt-get install tasksel
    user@ubuntu$ sudo tasksel install lamp-server

Na het uitvoeren van een van de bovenstaande opties wordt u gevraagd om deze pakketten en een aantal andere afhankelijkheden te installeren. Druk op 'y' en vervolgens op 'Enter' om te gaan en volg eventuele andere vragen om een beheerderswachtwoord instellen voor MySQL. Hiermee installeert u de minimaal vereiste PHP uitbreidingen die nodig zijn voor het gebruik van PHP met MySQL. 

![][1]

Voer de volgende opdracht om te zien andere PHP-uitbreidingen die beschikbaar zijn als pakketten:

    user@ubuntu$ apt-cache search php5


#### <a name="create-infophp-document"></a>Info.php document maken

U moet nu mogelijk om te controleren welke versie van Apache, MySQL en PHP hebt u via de opdrachtregel door `apache2 -v`, `mysql -v`, of `php -v`.

Als u dat wilt om verder te testen, kunt u een snelle PHP info-pagina te bekijken in een browser. Maak een nieuw bestand met Nano teksteditor met deze opdracht:

    user@ubuntu$ sudo nano /var/www/html/info.php

Binnen het GNU Nano teksteditor, voeg de volgende regels:

    <?php
    phpinfo();
    ?>

Vervolgens opslaan en sluit de teksteditor af.

Apache starten met deze opdracht, zodat alle nieuwe installaties van kracht.

    user@ubuntu$ sudo service apache2 restart

## <a name="verify-lamp-successfully-installed"></a>Controleer of de LAMP is geïnstalleerd

Nu u de PHP info-pagina die u zojuist hebt gemaakt in uw browser door naar http://youruniqueDNS/info.php te gaan controleren kunt, moet het er ongeveer zo.

![][2]

De Apache-installatie kunt u controleren door Apache2 Ubuntu Default Page door u http://youruniqueDNS/ wilt weergeven. U ziet er ongeveer als volgt.

![][3]

Gefeliciteerd, u hebt setup alleen een LAMP stack op de Azure VM!

## <a name="next-steps"></a>Volgende stappen

Bekijk de documentatie Ubuntu op de LAMP stack:

- [https://Help.Ubuntu.com/community/ApacheMySQLPHP](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/virtual-machines-linux-deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/virtual-machines-linux-deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/virtual-machines-linux-deploy-lamp-stack/apachesuccesspage.png
