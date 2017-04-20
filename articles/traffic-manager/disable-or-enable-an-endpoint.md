<properties
   pageTitle="Een eindpunt van het beheer van verkeer in- of uitschakelen | Microsoft Azure"
   description="Dit artikel helpt uw verkeer Manager profiel eindpunten in- of uitschakelen."
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

# <a name="disable-or-enable-a-traffic-manager-endpoint"></a>Een eindpunt van het beheer van verkeer in- of uitschakelen

U kunt ook afzonderlijke eindpunten die deel van een serviceprofiel verkeer uitmaken uitschakelen. Eindpunten zijn zowel cloud services en websites. Het uitschakelen van een eindpunt verlaat als onderdeel van het profiel maar het profiel alsof het eindpunt is niet opgenomen in het. Deze actie is erg handig voor het tijdelijk verwijderen van een eindpunt dat zich in de onderhoudsmodus of opnieuw worden geïmplementeerd. Zodra het eindpunt opnieuw actief is, kan worden ingeschakeld

>[AZURE.NOTE] **Het uitschakelen van een eindpunt heeft niets te maken met de status van de implementatie in Azure. Een gezonde eindpunt blijven omhoog en ontvangen verkeer terwijl in beheer van netwerkverkeer uitgeschakeld. Bovendien een eindpunt in één profiel uitschakelen heeft geen invloed op de status in een ander profiel.**

## <a name="to-disable-an-endpoint"></a>Een eindpunt uitschakelen

1. In het deelvenster Beheer van verkeer in de klassieke Azure portal naar het profiel dat verkeer Manager met de endpoint-instellingen die u wilt wijzigen en klik vervolgens op de pijl rechts van de naam van het profiel. Hiermee opent u de pagina-instellingen voor het profiel.
1. Aan de bovenkant van de pagina, klikt u op de **eindpunten** om de eindpunten die zijn opgenomen in de configuratie weer te geven.
1. Klik op het eindpunt dat u wilt uitschakelen en klik vervolgens op **uitschakelen** onder aan de pagina.
1. Verkeer wordt gestopt die doorloopt tot aan het eindpunt op basis van de DNS-Time-to-Live (TTL) geconfigureerd voor de domeinnaam van het beheer van verkeer. Kunt u de TTL-waarde van de pagina configuratie van het profiel van het beheer van verkeer.

## <a name="to-enable-an-endpoint"></a>Een eindpunt inschakelen


1. In het deelvenster Beheer van verkeer in de klassieke Azure portal naar het profiel dat verkeer Manager met de endpoint-instellingen die u wilt wijzigen en klik vervolgens op de pijl rechts van de naam van het profiel. Hiermee opent u de pagina-instellingen voor het profiel.
1. Aan de bovenkant van de pagina, klikt u op de **eindpunten** om de eindpunten die zijn opgenomen in de configuratie weer te geven.
1. Klik op het eindpunt dat u wilt inschakelen en klik vervolgens op **inschakelen** onder aan de pagina.
1. Verkeer wordt gestart die aan de service opnieuw als gedicteerde door het profiel.

## <a name="next-steps"></a>Volgende stappen

[Manager - Disable, enable verkeer of een profiel verwijderen](disable-enable-or-delete-a-profile.md)

[Probleemoplossing verkeer Manager verslechterd staat](traffic-manager-troubleshooting-degraded.md)

[Overwegingen bij prestaties van verkeer Manager](traffic-manager-performance-considerations.md)