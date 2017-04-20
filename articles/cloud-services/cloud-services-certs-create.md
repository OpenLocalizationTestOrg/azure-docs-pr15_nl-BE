<properties 
    pageTitle="Cloud Services en het beheer van certificaten | Microsoft Azure" 
    description="Meer informatie over het maken en gebruiken van certificaten met Microsoft Azure" 
    services="cloud-services" 
    documentationCenter=".net" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016"
    ms.author="adegeo"/>

# <a name="certificates-overview-for-azure-cloud-services"></a>Certificaten-overzicht voor Azure Cloud Services
Certificaten worden gebruikt in Azure, cloud services ([service-certificaten](#what-are-service-certificates)) en voor het verifiëren van de beheer-API ([beheer van certificaten](#what-are-management-certificates) bij het gebruik van de Azure klassieke portal en niet-ARM). Dit onderwerp geeft een algemeen overzicht van beide typen certificaten [maken](#create) en [implementeren van](#deploy) hen naar Azure.

Certificaten die worden gebruikt in Azure x.509 v3-certificaten zijn en kunnen worden ondertekend door een ander vertrouwd certificaat of niet-geverifieerd kunnen worden. Een zelfondertekend certificaat wordt ondertekend door de maker van de eigen en om deze reden, worden standaard niet vertrouwd. De meeste browsers kunnen dit negeren. Zelfondertekende certificaten mag alleen worden gebruikt door uzelf bij het ontwikkelen en testen van uw cloud-services. 

Certificaten die worden gebruikt door Azure kunnen een particulier of een openbare sleutel bevatten. Certificaten hebben een blauwdruk die kan worden op eenduidige wijze identificeren. Deze vingerafdruk wordt in de Azure [configuratiebestand](cloud-services-configure-ssl-certificate.md) gebruikt om te bepalen welk certificaat een cloud-service moet gebruiken. 

## <a name="what-are-service-certificates"></a>Wat zijn certificaten service?
Service-certificaten zijn gekoppeld aan cloud services en veilige communicatie van en naar de service in te schakelen. Als u een Webrol hebt geïmplementeerd, kunt u leveren een certificaat dat een blootgestelde HTTPS-eindpunt kan verifiëren. Service-certificaten, gedefinieerd in de definitie van de service worden automatisch geïmplementeerd op de virtuele machine waarop een exemplaar van uw rol. 

Certificaten service kunt u uploaden naar Azure klassieke portal met behulp van de klassieke Azure portal of met behulp van de API-Service Management. Certificaten service die is gekoppeld aan een specifieke cloud service en toegewezen aan een distributie in het definitiebestand.

Certificaten service afzonderlijk kunnen worden beheerd via de services en door verschillende personen kunnen worden beheerd. Een ontwikkelaar kan bijvoorbeeld een servicepakket dat verwijst naar een certificaat dat een IT-manager is eerder geüpload naar Azure uploaden. IT-manager kunt beheren en wijzigen van de configuratie van de service voor het uploaden van een nieuw servicepakket zonder certificaat vernieuwen. Dit is mogelijk omdat de logische naam op voor het certificaat en de naam en de locatie zijn opgegeven in het definitiebestand service terwijl de vingerafdruk van het certificaat is opgegeven in het bestand van de configuratie. Als u wilt bijwerken in het certificaat, is alleen nodig voor het uploaden van een nieuw certificaat en wijzig de waarde in het configuratiebestand van de service.

## <a name="what-are-management-certificates"></a>Wat zijn certificaten beheren?
Beheer van certificaten kunnen u verifiëren bij de Service Management-API die door klassieke Azure. Veel programma's en hulpprogramma's (zoals Visual Studio of de SDK Azure) gebruikt deze certificaten voor het automatiseren van de configuratie en implementatie van verschillende Azure diensten. Deze zijn niet echt gerelateerd aan cloud services. 

>[AZURE.WARNING] Wees voorzichtig! Deze typen certificaten dat alle gebruikers worden geverifieerd met hen voor het beheren van het abonnement dat ze zijn gekoppeld. 

### <a name="limitations"></a>Beperkingen
Er is een limiet van 100 management certificaten per abonnement. Er is ook een limiet van 100 beheer van certificaten voor alle abonnementen onder een specifieke service-beheerder gebruikers-ID. Als de gebruikers-ID voor de account administrator al 100 management certificaten toevoegen gebruikt en er behoefte aan meer certificaten is is, kunt u de beheerder van een collega om toe te voegen extra certificaten toevoegen. 

Zie voordat u meer dan 100 certificaten toe te voegen, als u een bestaand certificaat opnieuw kunt gebruiken. Mogelijk onnodige complexiteit met behulp van CO-beheerders worden toegevoegd aan uw certificaat management proces.


<a name="create"></a>
## <a name="create-a-new-self-signed-certificate"></a>Een nieuw, zelfondertekend certificaat maken
U kunt een hulpprogramma voor het maken van een zelfondertekend certificaat, zolang zij zich aan deze instellingen houden beschikbaar:

* Een x.509-certificaat.
* Bevat een persoonlijke sleutel.
* Voor sleuteluitwisseling (.pfx-bestand) gemaakt.
* De naam van certificaathouder moet overeenkomen met het domein dat wordt gebruikt voor toegang tot de service cloud. 
    > U kan niet aan te schaffen een SSL-certificaat voor de cloudapp.net (of voor alle gerelateerde Azure) domein. de onderwerpnaam van het certificaat moet overeenkomen met de naam van het aangepaste domein gebruikt voor toegang tot de toepassing. Bijvoorbeeld **contoso.net**, niet **contoso.cloudapp.net**.
* Minimum van 2048-bits codering.
* **Alleen service certificaat**: Client-side certificaat moet zich bevinden in het *persoonlijke* certificaatarchief.

Er zijn twee eenvoudige manieren voor het maken van een certificaat op Windows, met de `makecert.exe` -up- of IIS.

### <a name="makecertexe"></a>MakeCert.exe

Dit hulpprogramma is afgeschaft en wordt hier niet beschreven. Zie [dit MSDN-artikel](https://msdn.microsoft.com/library/windows/desktop/aa386968) voor meer informatie.

### <a name="powershell"></a>PowerShell

```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```

>[AZURE.NOTE] Als u gebruikmaken van het certificaat met een IP-adres in plaats van een domein wilt, gebruikt u het IP-adres in de parameter - DNS-naam.


Als u wilt met dit [certificaat met de portal beheren](../azure-api-management-certs.md), exporteren naar een **.cer** -bestand:

```powershell
Export-Certificate -Type CERT -Cert $cert -FilePath .\my-cert-file.cer
```

### <a name="internet-information-services-iis"></a>Internet informatieservices (IIS)

Er zijn veel pagina's op het internet die betrekking hebben op hoe u dit doet met IIS. [Hier](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) is een geweldig ik heb gevonden, ik denk dat het goed wordt uitgelegd. 

### <a name="java"></a>Java
U kunt Java gebruiken om een certificaat te [maken](../app-service-web/java-create-azure-website-using-java-sdk.md#create-a-certificate).

### <a name="linux"></a>Linux
[In dit](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) artikel wordt beschreven hoe certificaten maken met SSH.

## <a name="next-steps"></a>Volgende stappen

[Upload uw servicecertificaat aan de klassieke Azure portal](cloud-services-configure-ssl-certificate.md) (of de [Azure portal](cloud-services-configure-ssl-certificate-portal.md)).

Een [management API certificaat](../azure-api-management-certs.md) uploaden naar de klassieke Azure portal.

>[AZURE.NOTE] De Azure portal maakt geen gebruik van certificaten beheren voor toegang tot de API, maar in plaats daarvan gebruikt gebruikersaccounts.
