<properties
    pageTitle="Testen van verkeer Manager | Microsoft Azure"
    description="Dit artikel helpt u verkeer Manager instellingen testen"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="test-your-traffic-manager-settings"></a>Test de instellingen voor beheer van netwerkverkeer

Uw Manager verkeer als instellingen wilt testen, moet u meerdere clients op verschillende locaties, van waaruit u uw tests kunt uitvoeren. Brengt de eindpunten in uw profiel beheer van verkeer langs een tegelijk.

* De DNS-TTL-waarde laag ingesteld zodat wijzigingen snel (bijvoorbeeld 30 seconden doorgeven).
* Weet dat de IP-adressen van uw Azure cloud services en websites in het profiel dat u wilt testen.
* Gebruik de hulpprogramma's waarmee u een DNS-naam omzetten naar een IP-adres en dit adres weergeven.

U controleert om te zien dat de DNS-namen omzetten in IP-adressen van de eindpunten in uw profiel. De namen moeten in samenhang met verkeer methode die is gedefinieerd in het profiel van het beheer van verkeer oplossen. U kunt de hulpprogramma's zoals **nslookup** of **graven** DNS-namen om te zetten.

De volgende voorbeelden kunnen u test uw serviceprofiel verkeer.

### <a name="check-traffic-manager-profile-using-nslookup-and-ipconfig-in-windows"></a>Verkeer Manager profiel met nslookup en ipconfig in Windows controleren

1. Een opdracht of een Windows PowerShell-prompt openen als beheerder.
2. Type `ipconfig /flushdns` naar de DNS-omzettingscache leegmaken.
3. Type `nslookup <your Traffic Manager domain name>`. De volgende opdracht controleert bijvoorbeeld de naam van het domein met het voorvoegsel *myapp.contoso*

        nslookup myapp.contoso.trafficmanager.net

    Een gangbaar resultaat bevat de volgende informatie:

    * De DNS-naam en IP-adres van de DNS-server die u kunt oplossen door deze domeinnaam verkeer Manager wordt geopend.
    * De domeinnaam Manager verkeer u hebt opgegeven op de opdrachtregel na 'nslookup' en het IP-adres dat het verkeer Manager-domein wordt opgelost. Het tweede IP-adres is het belangrijk om te controleren. Deze moet overeenkomen met een openbare virtueel IP (VIP)-adres voor een van de cloud-services of websites die in het verkeer Manager profiel dat u wilt testen.

## <a name="how-to-test-the-failover-traffic-routing-method"></a>Het testen van de routeringsmethode voor failover-verkeer

1. Laat alle eindpunten van.
2. Met behulp van één enkele client, DNS-omzetting voor de domeinnaam van uw bedrijf met nslookup of een vergelijkbaar hulpprogramma aanvragen.
3. Zorg ervoor dat het omgezette IP-adres komt overeen met het primaire eindpunt.
4. Buiten het primaire eindpunt of verwijder het bestand controleren zodat deze Manager verkeer denkt de toepassing niet dat actief is.
5. Wacht totdat de DNS-Time-to-Live (TTL) van het verkeer Manager profiel plus twee minuten extra. Bijvoorbeeld, als uw DNS-TTL is 300 seconden (5 minuten), moet u wachten tot zeven minuten.
6. De DNS-client cache en een aanvraag voor DNS-omzetting met nslookup leegmaken. In Windows kunt u met de opdracht ipconfig/flushdns de DNS-cache leegmaken.
7. Zorg ervoor dat het omgezette IP-adres overeenkomt met uw secundaire eindpunt.
8. Herhaal dit proces, waardoor u elk eindpunt op zijn beurt. Controleer of de DNS-server het IP-adres van het eindpunt van de volgende in de lijst retourneert. Wanneer alle eindpunten niet beschikbaar zijn, moet u het IP-adres van de primaire eindpunt opnieuw.

## <a name="how-to-test-the-weighted-traffic-routing-method"></a>Het testen van de routeringsmethode gewogen verkeer

1. Laat alle eindpunten van.
2. Met behulp van één enkele client, DNS-omzetting voor de domeinnaam van uw bedrijf met nslookup of een vergelijkbaar hulpprogramma aanvragen.
3. Zorg ervoor dat het omgezette IP-adres overeenkomt met een van de eindpunten.
4. De DNS-client leegmaken en Herhaal stap 2 en 3 voor elk eindpunt. Hier ziet u verschillende IP-adressen die voor elk van de eindpunten wordt geretourneerd.

## <a name="how-to-test-the-performance-traffic-routing-method"></a>Het testen van de prestaties routeringsmethode voor verkeer

Effectief een prestaties verkeer omleiden als methode wilt testen, moet u clients die zich bevinden in verschillende delen van de wereld hebben. U kunt clients maken in verschillende Azure gebieden die kunnen worden gebruikt voor het testen van uw diensten. Als er een wereldwijd netwerk, kunt u op afstand aanmelden bij clients in andere delen van de wereld en de tests van daaruit uitvoeren.

Ook zijn er gratis DNS-zoekopdracht op het web en graven van services die beschikbaar zijn. Sommige van deze hulpprogramma's kunt u controleren van DNS-naamomzetting vanuit verschillende locaties over de hele wereld. Doe een zoekopdracht op 'DNS-lookup' voor voorbeelden. Services van derden zoals Gomez of thematoespraak kunnen worden gebruikt om te bevestigen dat uw profielen verkeer distribueren zoals verwacht.

## <a name="next-steps"></a>Volgende stappen

* [Over het beheer van verkeer verkeer routeringsmethoden](traffic-manager-routing-methods.md)
* [Overwegingen bij prestaties van verkeer Manager](traffic-manager-performance-considerations.md)
* [Probleemoplossing verkeer Manager verslechterd staat](traffic-manager-troubleshooting-degraded.md)




