<properties
    pageTitle="Het gebruik van de service management API (Python) - Functieoverzicht"
    description="Informatie over het uitvoeren van algemene beheertaken voor service via programmering van Python."
    services="cloud-services"
    documentationCenter="python"
    authors="lmazuel"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="lmazuel"/>

# <a name="how-to-use-service-management-from-python"></a>Het gebruik van beheer van Python Service

> [AZURE.NOTE] Service Management API wordt vervangen door de nieuwe Resource Management API, die momenteel beschikbaar zijn in een preview-versie.  Zie de [documentatie Azure Resource Management](http://azure-sdk-for-python.readthedocs.org/) voor meer informatie over het gebruik van de nieuwe Resource Management API van Python.

Deze handleiding wordt beschreven hoe u algemene beheertaken voor service van Python programmatisch worden uitgevoerd. De klasse **ServiceManagementService** in de [SDK voor Python Azure](https://github.com/Azure/azure-sdk-for-python) biedt ondersteuning voor programmatische toegang tot veel van de leidinggevende functie die beschikbaar is in de [Azure klassieke portal] [ management-portal] (zoals het **maken, bijwerken en verwijderen van cloud-services, implementaties, beheer van gegevens, en virtuele machines**). Deze functie kan handig zijn bij het bouwen van toepassingen die programmatische toegang tot de service management moeten worden.

## <a name="WhatIs"> </a>Wat is Service Management
De Service Management API biedt programmatische toegang tot veel van de service management functionaliteit beschikbaar via de [portal voor Azure klassieke][management-portal]. De Azure SDK for Python, kunt u uw cloud services en opslag accounts beheren.

Voor het gebruik van de Service Management API, moet u [een Azure account maken](https://azure.microsoft.com/pricing/free-trial/).

## <a name="Concepts"> </a>Concepten
De SDK Azure voor Python loopt de [Azure Service Management API][svc-mgmt-rest-api], die is een REST API. Alle API-bewerkingen worden uitgevoerd via SSL en wederzijds geverifieerd met X.509 v3-certificaten. De management-service kan worden geopend vanuit binnen een service die in Azure, of rechtstreeks via het Internet worden uitgevoerd vanuit elke toepassing die een HTTPS-aanvraag verzenden en ontvangen van een HTTPS-antwoord.

## <a name="Installation"> </a>Installatie

Alle in dit artikel beschreven functies zijn beschikbaar in de `azure-servicemanagement-legacy` -pakket, die u kunt installeren met behulp van pip. Voor meer informatie over de installatie (bijvoorbeeld, als u bekend bent met Python), raadpleegt u dit artikel: [installeren van Python en de SDK Azure](../python-how-to-install.md)

## <a name="Connect"> </a>Procedure: verbinding maken met de service management
Verbinding van het eindpunt van het beheer van Service, moet u uw Azure abonnements-ID en een geldig certificaat. U vindt uw abonnements-ID via de [portal voor Azure klassieke][management-portal].

> [AZURE.NOTE] Sinds Azure SDK for Python v0.8.0 is het nu mogelijk te gebruiken certificaten die met OpenSSL als met Windows.  Python punt 2.7.4 is vereist of hoger. Het is raadzaam gebruikers OpenSSL gebruiken in plaats van .pfx, aangezien de ondersteuning voor .pfx certificaten worden in de toekomst waarschijnlijk verwijderd.

### <a name="management-certificates-on-windowsmaclinux-openssl"></a>Beheer van certificaten op Windows/Mac/Linux (OpenSSL)
U kunt [OpenSSL](http://www.openssl.org/) management certificaat maken.  Eigenlijk moet u twee certificaten, één voor de server te maken (een `.cer` bestand) en één voor de client (een `.pem` bestand). Voor het maken van de `.pem` bestand, uitvoeren:

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Voor het maken van de `.cer` certificaat, het uitvoeren van:

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

Voor meer informatie over Azure certificaten Zie [Certificaten-overzicht voor Azure Cloud Services](./cloud-services-certs-create.md). Zie de documentatie op [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html)voor een volledige beschrijving van de parameters van OpenSSL.

Nadat u deze bestanden hebt gemaakt, moet u voor het uploaden van de `.cer` bestand naar Azure via de actie 'Uploaden' van het tabblad 'Instellingen' van de [Azure klassieke portal][management-portal], en noteer waar u opgeslagen moet de `.pem` bestand.

Nadat u uw abonnement-ID hebt ontvangen, een certificaat heeft gemaakt en geüpload de `.cer` bestand naar Azure, kunt u het eindpunt van het beheer van Azure door de abonnement-id en het pad naar de `.pem` te **ServiceManagementService**bestand:

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

In het voorgaande voorbeeld `sms` is een **ServiceManagementService** -object. De klasse **ServiceManagementService** is de primaire klasse Azure services beheren.

### <a name="management-certificates-on-windows-makecert"></a>Beheer van certificaten op Windows (MakeCert)

U kunt een zelfondertekend certificaat maken op uw computer met behulp van `makecert.exe`.  Open een **opdrachtprompt van Visual Studio** als **beheerder** en gebruik de volgende opdracht, *AzureCertificate* vervangen door de naam van het certificaat wilt gebruiken.

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

De opdracht maakt u de `.cer` -bestand en deze worden geïnstalleerd in het **persoonlijke** certificaatarchief. Zie [Certificaten-overzicht voor Azure Cloud Services](./cloud-services-certs-create.md)voor meer informatie.

Nadat u het certificaat hebt gemaakt, moet u voor het uploaden van de `.cer` bestand naar Azure via de actie 'Uploaden' van het tabblad 'Instellingen' van de [Azure klassieke portal][management-portal].

Nadat u uw abonnement-ID hebt ontvangen, een certificaat heeft gemaakt en geüpload de `.cer` bestand naar Azure, kunt u het eindpunt van het beheer van Azure door de abonnement-id en de locatie van het certificaat in het **persoonlijke** certificaatarchief naar **ServiceManagementService** (opnieuw, vervangt u *AzureCertificate* door de naam van het certificaat):

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

In het voorgaande voorbeeld `sms` is een **ServiceManagementService** -object. De klasse **ServiceManagementService** is de primaire klasse Azure services beheren.

## <a name="ListAvailableLocations"> </a>Hoe: lijst van beschikbare locaties

U kunt de locaties die beschikbaar zijn voor het hosten van services gebruiken de **lijst\_locaties** methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

Wanneer u een cloud service of storage-service moet u een geldige locatie. De **lijst\_locaties** methode retourneert altijd een actuele lijst van de momenteel beschikbare locaties. Schrijven van dit zijn de beschikbare locaties:

- West-Europa
- Noord-Europa
- Zuidoost-Azië
- Oost-Azië
- Centrale VS
- Centrale Noord-Amerikaanse
- Zuid-centraal-Verenigde Staten
- West-Verenigde Staten
- Oost-Verenigde Staten
- East Japan
- West Japan
- Brazilië-Zuid
- Australië, Oost
- Zuidoost Australië

## <a name="CreateCloudService"> </a>Hoe: een cloud-service maken

Wanneer u een toepassing maken en uitvoeren in Azure, worden de code en een configuratie samen genoemd een Azure [cloud-service][] (ook wel een *gehoste service* in oudere versies van Azure). De **maken\_gehost\_service** methode kunt u een nieuwe gehoste service door middel van de servicenaam van een gehoste (die uniek zijn in Azure), een label (automatisch gecodeerd met base64), een beschrijving en een locatie maken.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

Je kunt aanbieden het gehoste services voor uw abonnement met de **lijst\_gehost\_services** methode:

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

Als u wilt kunt u informatie over een bepaalde gehoste service, kunt u doen door de naam van de gehoste service aan de **get\_gehost\_service\_eigenschappen** methode:

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

Nadat u een cloud-service hebt gemaakt, kunt u uw code implementeren op de service met de **Maak\_implementatie** methode.

## <a name="DeleteCloudService"> </a>Hoe: een cloud-service verwijderen

U kunt een cloud-service verwijderen door de naam van de service aan de **verwijderen\_gehost\_service** methode:

    sms.delete_hosted_service('myhostedservice')

Voordat u een service verwijderen kunt, moeten alle implementaties voor de service worden verwijderd. (Zie [hoe: verwijderen van een implementatie van](#DeleteDeployment) voor meer informatie.)

## <a name="DeleteDeployment"> </a>Hoe: verwijderen van een implementatie

U een distributie te verwijderen met de **verwijderen\_implementatie** methode. In het volgende voorbeeld ziet u hoe te verwijderen van een implementatie met de naam `v1`.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <a name="CreateStorageService"> </a>Hoe: storage-service maken

Een [storage-service](../storage/storage-create-storage-account.md) hebt u toegang tot Azure [BLOB's](../storage/storage-python-how-to-use-blob-storage.md), [tabellen](../storage/storage-python-how-to-use-table-storage.md)en [wachtrijen](../storage/storage-python-how-to-use-queue-storage.md). U maakt een storage-service, moet u een naam voor de service (tussen 3 en 24 tekens in kleine letters en uniek in Azure), een beschrijving, een label (maximaal 100 tekens bevatten, automatisch gecodeerd met base64) en een locatie. In het volgende voorbeeld ziet u hoe een storage-service door een locatie maken.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mystorageaccount'
    label = 'mystorageaccount'
    location = 'West US'
    desc = 'My storage account description.'

    result = sms.create_storage_account(name, desc, label, location=location)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Opmerking in het voorgaande voorbeeld die de status van de **maken\_opslag\_account** bewerking kan worden opgehaald door het resultaat van **maken\_opslag\_account** naar de **ophalen\_bewerking\_status** methode.  

Overzicht van uw opslag rekeningen en hun eigenschappen met de **lijst\_opslag\_rekeningen** methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <a name="DeleteStorageService"> </a>Hoe: storage-service verwijderen

Kunt u een storage-service verwijderen door de servicenaam van de opslag aan de **verwijderen\_opslag\_account** methode. Storage-service verwijdert, worden alle gegevens die zijn opgeslagen in de service (BLOB's, tabellen en wachtrijen).

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <a name="ListOperatingSystems"> </a>Hoe: lijst met beschikbare besturingssystemen

U kunt de besturingssystemen die beschikbaar zijn voor het hosten van services gebruiken de **lijst\_die\_systemen** methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

Ook kunt u de **lijst\_werkzaam\_system\_families** methode, die de besturingssystemen door familie groepen:

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <a name="CreateVMImage"> </a>Hoe: Maak een installatiekopie van het besturingssysteem

Om de installatiekopie van een besturingssysteem toevoegen aan de bibliotheek afbeeldingen, gebruikt u de **toevoegen\_os\_afbeelding** methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mycentos'
    label = 'mycentos'
    os = 'Linux' # Linux or Windows
    media_link = 'url_to_storage_blob_for_source_image_vhd'

    result = sms.add_os_image(label, media_link, name, os)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

U kunt de installatiekopieën van besturingssystemen die beschikbaar zijn, gebruiken de **lijst\_os\_afbeeldingen** methode. Het bevat alle platform-afbeeldingen en gebruikersafbeeldingen:

    result = sms.list_os_images()

    for image in result:
        print('Name: ' + image.name)
        print('Label: ' + image.label)
        print('OS: ' + image.os)
        print('Category: ' + image.category)
        print('Description: ' + image.description)
        print('Location: ' + image.location)
        print('Media link: ' + image.media_link)
        print('')

## <a name="DeleteVMImage"> </a>Hoe: verwijderen van een besturingssysteemkopie

Om de afbeelding van een gebruiker verwijderen, gebruikt u de **verwijderen\_os\_afbeelding** methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <a name="CreateVM"> </a>Procedure: een virtuele machine maken

U maakt een virtuele machine, moet u eerst een [cloud-service](#CreateCloudService)maken.  Maak vervolgens de virtuele machine implementeren met behulp van de **maken\_virtual\_machine\_implementatie** methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where the VM disk
    # will be created
    media_link = 'url_to_target_storage_blob_for_vm_hd'

    # Linux VM configuration, you can use WindowsConfigurationSet
    # for a Windows VM instead
    linux_config = LinuxConfigurationSet('myhostname', 'myuser', 'mypassword', True)

    os_hd = OSVirtualHardDisk(image_name, media_link)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=os_hd,
        role_size='Small')

## <a name="DeleteVM"> </a>Hoe: verwijderen van een virtuele machine

Als u wilt verwijderen van een virtuele machine, u eerst verwijderen de implementatie met behulp van de **verwijderen\_implementatie** methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

De cloud-service kan vervolgens worden verwijderd met behulp van de **verwijderen\_gehost\_service** methode:

    sms.delete_hosted_service(service_name='myvm')

##<a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a>Procedure: Een virtuele Machine maken van een installatiekopie vastgelegde virtuele Machine

Voor het vastleggen van een afbeelding VM belt u eerst de **vastleggen\_vm\_afbeelding** methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace the below three parameters with actual values
    hosted_service_name = 'hs1'
    deployment_name = 'dep1'
    vm_name = 'vm1'

    image_name = vm_name + 'image'
    image = CaptureRoleAsVMImage    ('Specialized',
        image_name,
        image_name + 'label',
        image_name + 'description',
        'english',
        'mygroup')

    result = sms.capture_vm_image(
            hosted_service_name,
            deployment_name,
            vm_name,
            image
        )

Zorg ervoor dat u met succes de installatiekopie hebt vastgelegd, met de volgende de **lijst\_vm\_afbeeldingen** api, en zorg ervoor dat uw afbeelding wordt weergegeven in de resultaten:

    images = sms.list_vm_images()

U kunt de virtuele machine met behulp van de vastgelegde afbeelding ten slotte maken het **maken\_virtual\_machine\_implementatie** , zoals voorheen, maar deze keer door in de vm_image_name

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=None,
        role_size='Small',
        vm_image_name = image_name)

Zie voor meer informatie over het vastleggen van een virtuele Machine van Linux, [een Linux virtuele Machine vastleggen.](../virtual-machines/virtual-machines-linux-classic-capture-image.md)

Zie voor meer informatie over het vastleggen van een virtuele Windows-computer, [een virtuele Windows-computer vastleggen.](../virtual-machines/virtual-machines-windows-classic-capture-image.md)

## <a name="What's Next"> </a>Volgende stappen

Nu u de basisbeginselen van het servicebeheer hebt geleerd, kunt u toegang krijgen tot de [volledige API-documentatie voor de Python Azure SDK](http://azure-sdk-for-python.readthedocs.org/) en complexe taken eenvoudig uw python om toepassing te beheren.

Zie de [Python Developer Center](/develop/python/)voor meer informatie.

[What is Service Management]: #WhatIs
[Concepts]: #Concepts
[How to: Connect to service management]: #Connect
[How to: List available locations]: #ListAvailableLocations
[How to: Create a cloud service]: #CreateCloudService
[How to: Delete a cloud service]: #DeleteCloudService
[How to: Create a deployment]: #CreateDeployment
[How to: Update a deployment]: #UpdateDeployment
[How to: Move deployments between staging and production]: #MoveDeployments
[How to: Delete a deployment]: #DeleteDeployment
[How to: Create a storage service]: #CreateStorageService
[How to: Delete a storage service]: #DeleteStorageService
[How to: List available operating systems]: #ListOperatingSystems
[How to: Create an operating system image]: #CreateVMImage
[How to: Delete an operating system image]: #DeleteVMImage
[How to: Create a virtual machine]: #CreateVM
[How to: Delete a virtual machine]: #DeleteVM
[Next Steps]: #NextSteps
[management-portal]: https://manage.windowsazure.com/
[svc-mgmt-rest-api]: http://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[cloud-service]:/services/cloud-services/

