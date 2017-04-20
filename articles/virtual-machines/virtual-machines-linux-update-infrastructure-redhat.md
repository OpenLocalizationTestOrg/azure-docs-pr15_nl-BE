<properties
   pageTitle="Infrastructuur van Red Hat (RHUI) | Microsoft Azure"
   description="Meer informatie over Red Hat Update infrastructuur (RHUI) voor Red Hat Enterprise Linux-instanties op verzoek in Microsoft Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="BorisB2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="borisb"/>

# <a name="red-hat-update-infrastructure-rhui-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Infrastructuur van Red Hat (RHUI) voor on-demand Red Hat Enterprise Linux VMs in Azure

Virtuele machines die zijn gemaakt op basis van de bellen op Red Hat Enterprise Linux (RHEL) afbeeldingen beschikbaar in Azure markt zijn geregistreerd voor toegang tot de Red Hat Update infrastructuur (RHUI) in Azure geïmplementeerd.  De exemplaren van de RHEL op verzoek hebben toegang tot een regionale yum repository en incrementele updates meer ontvangen.

De yum repository lijst, die wordt beheerd door RHUI, is geconfigureerd in uw exemplaar RHEL tijdens het inrichten. U hoeft te doen extra configuratie - uitvoeren `yum update` nadat de RHEL-instantie gereed is voor de nieuwste updates ophalen.

> [AZURE.NOTE] Azure RHUI infrastructuur is onlangs bijgewerkt (September 2016) en wijzigingen in de configuratie van uw bestaande RHEL exemplaren voor ononderbroken toegang tot de Azure RHUI vereist. Raadpleeg de sectie RHUI Azure infrastructuur-Update voor meer informatie.


## <a name="rhui-azure-infrastructure-update"></a>RHUI Azure infrastructuur-Update
Azure is vanaf September 2016, een nieuwe reeks servers Red Hat Update infrastructuur (RHUI). Deze servers zijn gedistribueerd met [Azure verkeer Manager]( https://azure.microsoft.com/services/traffic-manager/) zodat een enkelvoudig eindpunt (rhui 1.micrsoft.com) kan worden gebruikt door alle VM ongeacht de regio. Ze gebruiken ook een SSL-certificaat dat wordt geschakeld naar een bekende Certificate Authority (hoofd-Baltimore). Deze update automatisch te maken zou zijn gevaarlijk voor sommige klanten met ACL's of aangepaste routeringstabellen voor de RHUI update-servers, zodat deze update is "opt-in." Handmatige stappen voor onboarding voor deze nieuwe servers zijn beschikbaar op deze pagina en een volledige script voor onboarding op geautomatiseerde wijze (na controle van de afzonderlijke stappen). De nieuwe RHEL PAYG afbeeldingen op Marketplace Azure (gedateerde September 2016 of latere versies) automatisch verwijst naar de nieuwe servers van Azure RHUI en geen extra maatregelen vereist.

### <a name="the-new-azure-rhui-infrastructure-onboarding-timeline"></a>De nieuwe RHUI Azure infrastructuur onboarding tijdlijn

| Datum | Opmerking |
| --- | --- |
|22 september 2016 | RHUI-servers en beschikbaar zijn voor installatie-instructies gebruiken. Met behulp van de nieuwe (September 2016 gedateerd) geïmplementeerd VMs RHEL PAYG marketplace afbeeldingen worden automatisch gebruikt voor de nieuwe RHUI-servers, maar bestaande VMs zijn "opt-in"
|1 november 2016 | Oude RHEL PAYG VM-installatiekopieën (waarin de oude Azure RHUI servers) worden verwijderd uit de galerie Azure Marketplace
|16 januari 2017 | Wordt de oude RHUI Azure-servers buiten gebruik worden gesteld. Alle van de betrokken PAYG RHEL VMs bijwerken door deze tijd toegang tot Azure RHUI onderhouden

### <a name="the-ips-for-the-new-rhui-content-delivery-servers-are"></a>De IP-adressen voor de nieuwe RHUI content delivery servers zijn

```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213

# Azure US Government
13.72.186.193
```

### <a name="manual-update-procedure-to-use-the-new-azure-rhui-servers"></a>Handmatige updateprocedure voor het gebruik van de nieuwe Azure RHUI servers

Downloaden (via krul) van de openbare sleutel handtekening

```
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 
```

Controleer of de gedownloade sleutel

```
gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
```

De uitvoer, controleert u of `keyid` en `user ID packet`:

```
Version: GnuPG v1.4.7 (GNU/Linux)
:public key packet:
        version 4, algo 1, created 1446074508, expires 0
        pkey[0]: [2048 bits]
        pkey[1]: [17 bits]
        keyid: EB3E94ADBE1229CF
:user ID packet: "Microsoft (Release signing) <gpgsecurity@microsoft.com>"
:signature packet: algo 1, keyid EB3E94ADBE1229CF
        version 4, created 1446074508, md5len 0, sigclass 0x13
        digest algo 2, begin of digest 1a 9b
        hashed subpkt 2 len 4 (sig created 2015-10-28)
        hashed subpkt 27 len 1 (key flags: 03)
        hashed subpkt 11 len 5 (pref-sym-algos: 9 8 7 3 2)
        hashed subpkt 21 len 3 (pref-hash-algos: 2 8 3)
        hashed subpkt 22 len 2 (pref-zip-algos: 2 1)
        hashed subpkt 30 len 1 (features: 01)
        hashed subpkt 23 len 1 (key server preferences: 80)
        subpkt 16 len 8 (issuer key ID EB3E94ADBE1229CF)
        data: [2047 bits]
```

De openbare sleutel installeren

```
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
```

Downloaden, te controleren en RPM-Client installeren

Download: Voor RHEL 6

```
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.0-2.noarch.rpm 
```

Voor RHEL 7

```
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.0-2.noarch.rpm  
```

Controleer of:

```
rpm -Kv azureclient.rpm
```

Controleer in de uitvoer die handtekening van het pakket is OK

```
azureclient.rpm:
    Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
    Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
    V3 RSA/SHA256 Signature, key ID be1229cf: OK
    MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
```

De RPM installeren

```
sudo rpm -U azureclient.rpm
```

Na voltooiing, Controleer of u toegang hebt tot formulier RHUI Azure VM

### <a name="all-in-one-script-for-automating-the-above-task"></a>Script voor het automatiseren van de voorgaande taak in-één
Met het volgende script naar wens kunt automatiseren waarin dit probleem optreedt VMs bijwerken naar de nieuwe Azure RHUI servers.

```
# Download key
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 

# Validate key
if ! gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release | grep -q "keyid: EB3E94ADBE1229CF"; then
    echo "Keyfile azure.asc NOT valid. Exiting."
    exit 1
fi

# Install Key
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release

# Download RPM package
if grep -q "release 7" /etc/redhat-release; then
    ver=7
elif  grep -q "release 6" /etc/redhat-release; then
    ver=6
else
    echo "Version not supported, exiting"
    exit 1
fi

url=https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel$ver/rhui-azure-rhel$ver-2.0-2.noarch.rpm
curl -o azureclient.rpm "$url"

# Verify package
if ! rpm -Kv azureclient.rpm | grep -q "key ID be1229cf: OK"; then
    echo "RPM failed validation ($url)"
    exit 1
fi

# Install package
sudo rpm -U azureclient.rpm
```

## <a name="rhui-overview"></a>Overzicht RHUI
[Infrastructuur van Red Hat](https://access.redhat.com/products/red-hat-update-infrastructure) biedt een zeer schaalbare oplossing om de inhoud van de yum repository voor Red Hat Enterprise Linux cloud-exemplaren die worden gehost door Red Hat-gecertificeerde cloud providers beheren. Op basis van de upstream project van Pulp, kunt RHUI cloud providers lokaal mirror content repository gehost van Red Hat, aangepaste opslagplaatsen met hun eigen inhoud te maken en deze opslagplaatsen beschikbaar te maken voor een grote groep eindgebruikers via een systeem content delivery verdeeld.

## <a name="regions-where-rhui-is-available"></a>Regio's waar de RHUI beschikbaar is
RHUI is beschikbaar in alle regio's waar RHEL op verzoek afbeeldingen beschikbaar zijn. Momenteel omvat alle openbare gebieden op de pagina [status Azure dashboard](https://azure.microsoft.com/status/) en Azure Amerikaanse regering regio's vermeld. RHUI-toegang voor VMs ingericht van RHEL bellen op afbeeldingen is opgenomen in de prijs. Als we uitbreiding RHEL op aanvraag beschikbaar in de toekomst wordt meer regionale/nationale wolk beschikbaarheid bijgewerkt.

> [AZURE.NOTE] Toegang tot RHUI Azure gehost is beperkt tot het VMs binnen [Microsoft Azure Datacenter IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653).

## <a name="get-updates-from-another-update-repository"></a>Updates ophalen van een andere update opslagplaats

Als u nodig hebt om updates te downloaden uit de bibliotheek van een andere update (in plaats van Azure gehost RHUI) moet u de registratie van uw sessies van RHUI en opnieuw wilt registreren met de gewenste update-infrastructuur (bijvoorbeeld Red Hat satelliet of Red Hat Customer Portal CDN). Moet u passende Red Hat abonnementen voor deze registratie en services voor [Toegang tot de Cloud Red Hat in Azure](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure).

De registratie van RHUI en opnieuw te registreren om uw infrastructuur update volgen de onderstaande stappen te volgen.

1.  /Etc/yum.repos.d/rh-cloud.repo bewerken en wijzigt u alle `enabled=1` naar `enabled=0`. Bijvoorbeeld:

        # sed -i 's/enabled=1/enabled=0/g' /etc/yum.repos.d/rh-cloud.repo

2.  /Etc/yum/pluginconf.d/rhnplugin.conf bewerken en wijzigt u `enabled=0` naar `enabled=1`.
3.  Klik met de gewenste infrastructuur, zoals Red Hat Customer Portal registreren. Volg de gids voor Red Hat-oplossing voor [het registreren en abonneren een systeem aan de Portal voor klanten van Red Hat](https://access.redhat.com/solutions/253273).

> [AZURE.NOTE] Toegang tot de Azure gehost RHUI is inbegrepen in de prijs van de afbeelding RHEL Pay-As-You-Go (PAYG). Afmelden van een Omslagstelsel RHEL VM uit de RHUI Azure gehost wordt niet geconverteerd de virtuele machine in plaatsen-uw-eigen-licentie (BYOL), type VM en dus u kan worden telefoontikken dubbele als u dezelfde VM met een andere bron van updates registreert. 
> 
> Als u voortdurend wilt gebruiken een infrastructuur van andere dan Azure gehost RHUI kunt u maken en implementeren van uw eigen afbeeldingen (type BYOL) zoals beschreven in artikel [maken en uploaden van Red Hat virtuele machine voor Azure](virtual-machines-linux-redhat-create-upload-vhd.md) .

## <a name="next-steps"></a>Volgende stappen
Voor het maken van een Red Hat Enterprise Linux VM van Azure Marketplace Pay-As-You-Go-afbeelding en de hefboomwerking Azure gehost RHUI Ga naar [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/). Is het mogelijk gebruik van `yum update` in uw exemplaar RHEL zonder eventuele aanvullende instellingen.