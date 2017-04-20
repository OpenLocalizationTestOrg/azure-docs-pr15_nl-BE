<properties
    pageTitle="Problemen met status Azure verkeer Manager verslechterd"
    description="Het verkeer profielen oplossen als deze wordt weergegeven als de status verslechterd."
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

# <a name="troubleshooting-degraded-state-on-azure-traffic-manager"></a>Problemen met Azure verkeer Manager staat verslechterd

In dit artikel wordt beschreven hoe een Azure verkeer Verbindingsbeheer-profiel dat wordt weergegeven een status gedegradeerde oplossen. In dit scenario kunt u een verkeer serviceprofiel aan te wijzen aan sommige van uw cloudapp.net gehoste services is geconfigureerd. Wanneer u de status van uw manager verkeer controleert, ziet u dat de Status is verslechterd.

![slechtere staat](./media/traffic-manager-troubleshooting-degraded/traffic-manager-degraded.png)

Als u naar het tabblad eindpunten van dat profiel gaat, ziet u een of meer van de eindpunten met een off line status:

![off line](./media/traffic-manager-troubleshooting-degraded/traffic-manager-offline.png)

## <a name="understanding-traffic-manager-probes"></a>Wat verkeer Manager sondes

- Beheer van netwerkverkeer beschouwt een eindpunt zijn ONLINE alleen wanneer de sonde een HTTP 200-antwoord terugkrijgt van de sonde pad. Een ander niet-200 antwoord is een fout opgetreden.
- Een omleiding 30 x mislukt, zelfs als de omgeleide URL geeft als een 200 resultaat.
- Certificaatfouten worden voor HTTPs-sondes genegeerd.
- De inhoud van het pad van de sonde maakt niet uit als een 200 wordt geretourneerd. Zoeken naar een URL naar een statische inhoud zoals "/ favicon.ico" is een veelvoorkomende techniek. Dynamische inhoud, zoals de ASP-pagina's kan altijd retourneert geen 200, zelfs als de toepassing in orde is.
- Aanbevolen wordt het pad van de sonde naar iets dat voldoende logica om te bepalen heeft of de site zich omhoog of omlaag. In het vorige voorbeeld door het pad in te stellen op ' / favicon.ico ', alleen tests die w3wp.exe reageert. Deze sonde mogelijk niet uw webtoepassing in orde is. Een betere optie zou zijn, zoals een pad instellen naar een iets "/ Probe.aspx" die de logica voor het bepalen van de gezondheid van de site heeft. U kunt bijvoorbeeld gebruik van CPU-gebruik van prestatiemeteritems of meet het aantal mislukte aanvragen. Of u kan proberen toegang te krijgen tot de bronnen van de database of de sessiestatus om ervoor te zorgen dat de webtoepassing werkt.
- Als alle eindpunten in een profiel zijn verslechterd, behandelt verkeer Manager alle eindpunten als gezonde en routes verkeer naar alle eindpunten. Dit probleem zorgt ervoor dat problemen met het Zoek mechanisme niet in een volledig onderhoud van uw service resulteren.

## <a name="troubleshooting"></a>Het oplossen van problemen

Voor het oplossen van een storing van de sonde moet u een hulpprogramma waarmee de HTTP-statuscode retourneren uit de URL van de sonde. Er zijn veel hulpmiddelen beschikbaar waarmee u de onbewerkte HTTP-antwoord geven.

* [Fiddler](http://www.telerik.com/fiddler)
* [krul](https://curl.haxx.se/)
* [wget](http://gnuwin32.sourceforge.net/packages/wget.htm)

Ook kunt u het tabblad netwerk van de F12 foutopsporingsprogramma's in Internet Explorer de HTTP-antwoorden bekijken.

In dit voorbeeld willen we de reactie van onze URL sonde Zie: http://watestsdp2008r2.cloudapp.net:80/sonde. De volgende PowerShell voorbeeld illustreert het probleem.

```powershell
    Invoke-WebRequest 'http://watestsdp2008r2.cloudapp.net/Probe' -MaximumRedirection 0 -ErrorAction SilentlyContinue | Select-Object StatusCode,StatusDescription
```

Voorbeeld van de uitvoer:

```text
    StatusCode StatusDescription
    ---------- -----------------
            301 Moved Permanently
```

U ziet dat we een omleiding antwoord ontvangen. Zoals al eerder aangegeven, een StatusCode dan 200, wordt beschouwd als een fout. Beheer van verkeer verandert de Eindpuntstatus op Offline. Controleer de configuratie van de website om ervoor te zorgen dat de juiste StatusCode kan worden geretourneerd uit het pad van de sonde het probleem op te lossen. De configuratie van de sonde verkeer Manager om te verwijzen naar een pad geeft als resultaat een 200.

Als de sonde het HTTPS-protocol wordt gebruikt, moet u wellicht uitschakelen certificaat controleren van SSL/TLS om fouten te voorkomen tijdens de test. De volgende instructies met PowerShell certificaatverificatie voor de huidige PowerShell-sessie uit te schakelen:

```powershell
    add-type @"
    using System.Net;
    using System.Security.Cryptography.X509Certificates;
    public class TrustAllCertsPolicy : ICertificatePolicy {
        public bool CheckValidationResult(
        ServicePoint srvPoint, X509Certificate certificate,
        WebRequest request, int certificateProblem) {
        return true;
        }
    }
    "@
    [System.Net.ServicePointManager]::CertificatePolicy = New-Object TrustAllCertsPolicy
```

## <a name="next-steps"></a>Volgende stappen

[Over het beheer van verkeer verkeer routeringsmethoden](traffic-manager-routing-methods.md)

[Wat is verkeer Manager](traffic-manager-overview.md)

[Cloud Services](http://go.microsoft.com/fwlink/?LinkId=314074)

[Azure Web Apps](https://azure.microsoft.com/documentation/services/app-service/web/)

[Bewerkingen op verkeer Manager (REST API Reference)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure verkeer Manager-Cmdlets][1]

[1]: https://msdn.microsoft.com/library/mt125941(v=azure.200).aspx
