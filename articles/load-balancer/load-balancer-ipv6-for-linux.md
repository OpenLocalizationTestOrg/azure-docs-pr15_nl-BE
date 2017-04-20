<properties
    pageTitle="DHCPv6 configureren voor Linux VMs | Microsoft Azure"
    description="Het DHCPv6 configureren voor Linux VMs."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    keywords="IPv6, azure-taakverdeling, dual-stack, openbaar IP-, systeemeigen ipv6, mobiele, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="configuring-dhcpv6-for-linux-vms"></a>DHCPv6 configureren voor Linux VMs

Sommige afbeeldingen op Marketplace Azure Linux virtuele machine geen DHCPv6 standaard geconfigureerd. Ter ondersteuning van IPv6 moet DHCPv6 worden geconfigureerd in binnen de distributie van Linux-besturingssysteem dat u gebruikt. Andere Linux distributies hebben verschillende manieren van DHCPv6 configureren omdat ze verschillende pakketten gebruiken.

>[AZURE.NOTE] Recente afbeeldingen van SUSE Linux en CoreOS in de markt Azure zijn vooraf geconfigureerd met DHCPv6. Geen extra wijzigingen zijn vereist bij het gebruik van deze afbeeldingen.

Dit document wordt beschreven hoe u DHCPv6 inschakelen zodat uw Linux virtuele machine een IPv6-adres verkrijgt.

>[AZURE.WARNING] Onjuist bewerken van configuratiebestanden van netwerk kan ertoe leiden dat netwerktoegang tot uw VM verliezen. Het is raadzaam dat u uw wijzigingen in de configuratie op niet-productieve systemen testen. De instructies in dit artikel is getest op de nieuwste versies van de Linux-afbeeldingen op Marketplace Azure. Raadpleeg de documentatie voor uw specifieke versie van Linux voor gedetailleerde instructies.

## <a name="ubuntu"></a>Ubuntu

1. Bewerk het bestand `/etc/dhcp/dhclient6.conf` en voeg de volgende regel:

        timeout 10;

2. Wijzig de netwerkconfiguratie voor de eth0 interface met de volgende configuratie:

    * Bewerk het bestand op **Ubuntu 12.04 en 14.04**`/etc/network/interfaces.d/eth0.cfg`
    * Bewerk het bestand op **Ubuntu, 16.04**,`/etc/network/interfaces.d/50-cloud-init.cfg`

    ```bash
    iface eth0 inet6 auto
        up sleep 5
        up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true
    ```

3. IPv6-adres vernieuwen:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a>Debian

1. Bewerk het bestand `/etc/dhcp/dhclient6.conf` en voeg de volgende regel:

        timeout 10;

2. Bewerk het bestand `/etc/network/interfaces` en voeg de volgende configuratie:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. IPv6-adres vernieuwen:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a>RHEL / CentOS / Oracle Linux

1. Bewerk het bestand `/etc/sysconfig/network` en voeg de volgende parameter:

        NETWORKING_IPV6=yes

2. Bewerk het bestand `/etc/sysconfig/network-scripts/ifcfg-eth0` en voeg de volgende twee parameters:

        IPV6INIT=yes
        DHCPV6C=yes

3. IPv6-adres vernieuwen:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a>SLES 11 & openSUSE 13

Recente SLES en openSUSE afbeeldingen in Azure is vooraf geconfigureerd met DHCPv6. Geen extra wijzigingen zijn vereist bij het gebruik van deze afbeeldingen. Hebt u een VM op basis van een ouder of aangepaste SUSE-afbeelding, gebruikt u de volgende stappen uit:

1. Installeer de `dhcp-client` pakket, als dat nodig is:

    ```bash
    # sudo zypper install dhcp-client
    ```

2. Bewerk het bestand `/etc/sysconfig/network/ifcfg-eth0` en voeg de volgende parameter:

        DHCLIENT6_MODE='managed'

3. Het IPv6-adres vernieuwen:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>12 SLES en openSUSE sprong

Recente SLES en openSUSE afbeeldingen in Azure is vooraf geconfigureerd met DHCPv6. Geen extra wijzigingen zijn vereist bij het gebruik van deze afbeeldingen. Hebt u een VM op basis van een ouder of aangepaste SUSE-afbeelding, gebruikt u de volgende stappen uit:

1. Bewerk het bestand `/etc/sysconfig/network/ifcfg-eth0` en vervangt u deze parameter

        #BOOTPROTO='dhcp4'

    met de volgende waarde:

        BOOTPROTO='dhcp'

2. Voeg de volgende parameter toe `/etc/sysconfig/network/ifcfg-eth0`:

        DHCLIENT6_MODE='managed'

3. Het IPv6-adres vernieuwen:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Recente CoreOS afbeeldingen in Azure is vooraf geconfigureerd met DHCPv6. Geen extra wijzigingen zijn vereist bij het gebruik van deze afbeeldingen. Als u een VM op basis van een ouder of een aangepaste afbeelding voor CoreOS hebt, gebruikt u de volgende stappen uit:

1. Bewerk het bestand`/etc/systemd/network/10_dhcp.network`

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. Het IPv6-adres vernieuwen:

    ```bash
    # sudo systemctl restart systemd-networkd
    ```
