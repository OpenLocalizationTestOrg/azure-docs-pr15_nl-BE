<properties
   pageTitle="Met behulp van dynamische DNS host-namen registreren"
   description="Deze pagina krijgt informatie over het instellen van Dynamic DNS host-namen registreren in DNS-servers."
   services="dns"
   documentationCenter="na"
   authors="GarethBradshawMSFT"
   manager="carmonm"
   editor="" />
<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="sewhee" />

# <a name="using-dynamic-dns-to-register-hostnames-in-your-own-dns-server"></a>Met behulp van dynamische DNS host-namen registreren in de DNS-server

[Azure biedt omzetting van namen](virtual-networks-name-resolution-for-vms-and-role-instances.md) voor virtuele machines (VMs) en de rol van instanties. Wanneer de naamomzetting moet verder gaan dan die welke door de Azure, kunt u uw eigen DNS-servers bieden. Hiermee kunt u uw DNS-oplossing aanpassen aan uw eigen wensen aanpassen. U moet mogelijk toegang tot bronnen via uw Active Directory-domeincontroller op gebouwen.

Wanneer uw aangepaste DNS-servers worden gehost als Azure VMs kunt u hostnaam query's voor de vnet dezelfde doorsturen naar Azure hostnamen omzetten. Als u niet gebruiken, deze route wilt, kunt u uw VM-host-namen registreren in de DNS-server met behulp van dynamische DNS.  Azure heeft geen rechtstreeks om records te maken in uw DNS-servers zo nodig alternatieve regelingen zijn vaak de mogelijkheid (bv. referenties). Hier volgen enkele algemene scenario's met alternatieven.

## <a name="windows-clients"></a>Windows-clients

Windows-clients niet tot een domein behoren proberen onbeveiligde dynamische DNS (DDNS) updates wanneer ze opstart of wanneer hun IP-adres verandert. De DNS-naam is de naam van de host plus de primaire DNS-achtervoegsel. Azure blijft het primaire DNS-achtervoegsel leeg, maar u kunt dit instellen in de VM, via de [gebruikersinterface](https://technet.microsoft.com/library/cc794784.aspx) of [met behulp van automatisering](https://social.technet.microsoft.com/forums/windowsserver/3720415a-6a9a-4bca-aa2a-6df58a1a47d7/change-primary-dns-suffix).

Domein behoren Windows-clients registreren hun IP-adressen met de domeincontroller met behulp van beveiligde dynamische DNS. Het domeinlidmaatschap proces wordt het primaire DNS-achtervoegsel op de client en maakt en onderhoudt de vertrouwensrelatie.

## <a name="linux-clients"></a>Linux-clients

Linux-clients in het algemeen niet zich registreren bij de DNS-server bij het opstarten, ze wordt ervan uitgegaan dat de DHCP-server doet dit allemaal. Azure van DHCP-servers beschikt niet over de mogelijkheid of referenties om records te registreren in de DNS-server.  Een hulpprogramma, genaamd *nsupdate*, dat van de Bind-pakket uitmaakt deel, kunt u dynamische DNS-updates verzenden. Omdat het standaardiseren van het protocol voor dynamische DNS-kunt u *nsupdate* gebruiken, zelfs als u niet afhankelijk van de DNS-server gebruikt.

U kunt de haken die worden geleverd door de DHCP-client maakt en onderhoudt de hostname-vermelding in de DNS-server. Tijdens de cyclus DHCP voert de client de scripts in */etc/dhcp/dhclient-exit-hooks.d/*. Dit kan worden gebruikt voor het registreren van het nieuwe IP-adres met behulp van *nsupdate*. Bijvoorbeeld:

        #!/bin/sh
        requireddomain=mydomain.local

        # only execute on the primary nic
        if [ "$interface" != "eth0" ]
        then
            return
        fi

        # when we have a new IP, perform nsupdate
        if [ "$reason" = BOUND ] || [ "$reason" = RENEW ] ||
           [ "$reason" = REBIND ] || [ "$reason" = REBOOT ]
        then
            host=`hostname`
            nsupdatecmds=/var/tmp/nsupdatecmds
              echo "update delete $host.$requireddomain a" > $nsupdatecmds
              echo "update add $host.$requireddomain 3600 a $new_ip_address" >> $nsupdatecmds
              echo "send" >> $nsupdatecmds

              nsupdate $nsupdatecmds
        fi

        #done
        exit 0;

U kunt ook de opdracht *nsupdate* beveiligde dynamische DNS-updates uitvoeren. Bijvoorbeeld, als u een Bind DNS-server, is een openbare en persoonlijke sleutelpaar [gegenereerd](http://linux.yyz.us/nsupdate/).  De DNS-server is [geconfigureerd](http://linux.yyz.us/dns/ddns-server.html) met het openbare gedeelte van de sleutel, zodat deze kan controleren of de handtekening op de aanvraag. U moet de optie *-k* te geven dat het sleutelpaar te *nsupdate* voor dynamische DNS-update aanvraag moet worden ondertekend.

Wanneer u een Windows DNS-server gebruikt, kunt u Kerberos-verificatie kunt gebruiken met de parameter *-g* in *nsupdate* (niet beschikbaar in de Windows-versie van *nsupdate*). Gebruik hiervoor *kinit* laden de referenties (bijvoorbeeld via een [bestand keytab](http://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)). *Nsupdate g* komt vervolgens de referenties uit de cache ophalen.

Indien nodig, kunt u een zoekopdracht DNS-achtervoegsel toevoegen aan uw VMs. Het DNS-achtervoegsel is opgegeven in het bestand */etc/resolv.conf* . De meeste Linux-distros automatisch beheren van de inhoud van dit bestand, dus meestal niet worden bewerkt. U kunt echter het achtervoegsel overschrijven met behulp van de DHCP-client *vervangt* opdracht. Klik hiervoor in het */etc/dhcp/dhclient.conf*toevoegen:

        supersede domain-name <required-dns-suffix>;

