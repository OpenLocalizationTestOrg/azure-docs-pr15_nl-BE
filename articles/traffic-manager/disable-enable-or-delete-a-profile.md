<properties
   pageTitle="Uitschakelen, inschakelen of verwijderen van een serviceprofiel verkeer | Microsoft Azure"
   description="In dit artikel kunt u werken met de profielen van uw Manager verkeer."
   services="traffic-manager"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="disable-enable-or-delete-a-profile"></a>Uitschakelen, inschakelen of verwijderen van een profiel


Een bestaand profiel voor verkeer Manager kunt u uitschakelen zodat het niet aanvragen van gebruikers naar de geconfigureerde eindpunten verwijst. Wanneer u een serviceprofiel verkeer, het profiel zelf en de gegevens in het profiel uitschakelen blijft intact en kunnen worden bewerkt in de interface van de Manager van verkeer. Als u het profiel opnieuw in te schakelen, hoeft u dus in de Azure portal en verwijzingen wordt hervat. Wanneer u een serviceprofiel verkeer in Azure portal maakt, wordt deze automatisch ingeschakeld.

## <a name="to-disable-a-profile"></a>Een profiel uitschakelen

1. Wijzig de DNS-bronrecord op de Internet-DNS-server te gebruiken van het betreffende recordtype en een pointer naar een andere naam of het IP-adres van een specifieke locatie op het Internet. Met andere woorden, de DNS-bronrecord op de Internet-DNS-server zodanig wijzigen dat een CNAME-bronrecord die naar de domeinnaam van uw serviceprofiel verkeer verwijst niet meer gebruikt.
1. Verkeer niet meer worden gericht op de eindpunten via de instellingen van het beheer van verkeer.
1. Selecteer het profiel dat u wilt uitschakelen. Selecteer het profiel, klik op de pagina beheer van verkeer, het profiel te markeren door te klikken op de kolom naast de naam van het profiel. Klik niet op de naam van het profiel of de pijl naast de naam van de als u naar de instellingenpagina voor het profiel gaat.
1. Selecteer het profiel en klik op uitschakelen onder aan de pagina.

## <a name="to-enable-a-profile"></a>Om een profiel

1. Selecteer het profiel dat u wilt inschakelen. Selecteer het profiel, klik op de pagina beheer van verkeer, het profiel te markeren door te klikken op de kolom naast de naam van het profiel. Klik niet op de naam van het profiel of de pijl naast de naam van de als u naar de instellingenpagina voor het profiel gaat.
1. Na het selecteren van het profiel, klikt u op inschakelen op de onderkant van de pagina.
1. De DNS-bronrecord op de Internet-DNS-server gebruikt de CNAME-record, die de domeinnaam van uw bedrijf wordt toegewezen aan de domeinnaam van uw verkeer Manager profiel wijzigen. Zie [punt een bedrijf internetdomein een domein verkeer Manager](traffic-manager-point-internet-domain.md)voor meer informatie.
1. Verkeer wordt gestart opnieuw wordt gericht op de eindpunten.

## <a name="delete-a-profile"></a>Een profiel verwijderen


1. Zorg ervoor dat de DNS-bronrecord op de Internet-DNS-server geen gebruik maakt van een CNAME-bronrecord die naar de domeinnaam van uw serviceprofiel verkeer verwijst.
1. Selecteer het profiel dat u wilt verwijderen. Selecteer het profiel, klik op de pagina beheer van verkeer markeren het standaardprofiel
1. Als u op de kolom naast het profiel. Klik niet op de naam van het profiel of de pijl naast de naam van de als u naar de instellingenpagina voor het profiel gaat.
1. Na het selecteren van het profiel, klik op verwijderen onder aan de pagina.

## <a name="next-steps"></a>Volgende stappen

[Manager - verkeer uitschakelen of inschakelen van een eindpunt](disable-or-enable-an-endpoint.md)

[Failover-methode configureren](traffic-manager-configure-failover-routing-method.md)

[Methode round robin configureren](traffic-manager-configure-round-robin-routing-method.md)

[Prestaties routeringsmethode configureren](traffic-manager-configure-performance-routing-method.md)

[Probleemoplossing verkeer Manager verslechterd staat](traffic-manager-troubleshooting-degraded.md)

