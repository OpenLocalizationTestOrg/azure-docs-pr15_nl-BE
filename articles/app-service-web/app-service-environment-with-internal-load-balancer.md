<properties
    pageTitle="Maken en gebruiken van een interne taakverdeling met een App-omgeving | Microsoft Azure"
    description="Maken en gebruiken van een ASE met een ILB"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="ccompy"/>

# <a name="using-an-internal-load-balancer-with-an-app-service-environment"></a>Met behulp van een interne taakverdeling met een App-omgeving #

De functie Environments(ASE) van App-Service is een Premium serviceoptie van Azure App-Service biedt een verbeterde functionaliteit die niet beschikbaar is in de huurder met meerdere stempels.  De functie ASE implementeert in feite de Azure App-Service in uw Azure virtuele Network(VNet).  Krijgen een beter inzicht in de mogelijkheden aangeboden door App serviceomgevingen gelezen het, [Wat is een App-omgeving] [ WhatisASE] documentatie.  Als u niet de voordelen weet van het werken in een VNet lezen de [Azure virtuele netwerk Veelgestelde vragen over][virtualnetwork].  


## <a name="overview"></a>Overzicht ##


Een ASE kan worden geïmplementeerd met een internet toegankelijk eindpunt of een IP-adres in uw VNet.  U moet het IP-adres ingesteld op een VNet-adres de ASE met een interne laden Balancer(ILB) implementeren.  Wanneer uw ASE is geconfigureerd met een ILB moet u het volgende opgeven:

- uw eigen domein of subdomein.  Zodat u gemakkelijk in dit document wordt ervan uitgegaan dat subdomein, maar kunt u deze configureren in beide gevallen.  
- het certificaat voor HTTPS
- DNS-beheer voor het subdomein.  


In ruil kunt u doen, zoals:

- intranet hosttoepassingen, zoals bedrijfstoepassingen veilig in de cloud die u via een Site naar Site of ExpressRoute VPN toegang tot
- host-apps in de cloud die niet worden vermeld in openbare DNS-servers
- maken internet alleenstaande backend apps die veilig uw front-end-toepassingen kunnen integreren


#### <a name="disabled-functionality"></a>Uitgeschakelde functies ####

Er zijn enkele dingen die u niet bij het gebruik van een ILB ASE.  Die dingen zijn:

- met behulp van IPSSL
- IP-adressen toewijzen aan specifieke apps
- kopen en het gebruik van een certificaat met een app via de portal.  U kunt natuurlijk nog steeds rechtstreeks een certificeringsinstantie certificaten verkrijgen en gebruiken met uw toepassingen, niet alleen via de portal Azure.


## <a name="creating-an-ilb-ase"></a>Maken van een ILB ASE ##

Maken van een ASE ILB verschilt niet veel van een ASE normaal maken.  Voor een diepere discussie over het maken van een ASE Lees [het maken van een App-omgeving][HowtoCreateASE].  Het proces voor het maken van een ASE ILB is hetzelfde tussen het maken van een VNet tijdens het maken van ASE of een bestaande VNet te selecteren.  Een ASE ILB maken: 

1.  Selecteer **Nieuw -> Web + Mobile -> App-omgeving** in Azure portal
2.  Selecteer uw abonnement
3.  Een resourcegroep maken of selecteren
4.  Selecteer of maak een VNet
5.  Een subnet maken als een VNet selecteren
6.  Selecteer **virtuele netwerklocatie-> VNet-configuratie** en het VIP Type ingesteld op Internal
7.  (Dit is het subdomein gebruikt voor toepassingen die zijn gemaakt in deze ASE) subdomeinnaam bieden
8.  Klik op Ok en maak


![][1]


Er is een VNet configuratie-optie binnen het virtuele netwerk blade.  Deze kunt u kiezen tussen een VIP externe of interne VIP.  De standaardwaarde is extern.  Als u ingesteld op extern hebt gebruik uw ASE een toegankelijke VIP internet.  Als u interne, worden de ASE geconfigureerd met een ILB op een IP-adres binnen de VNet.  


Als u interne, de mogelijkheid meer IP-adressen toevoegen aan uw ASE is verwijderd en in plaats daarvan moet u het subdomein van de ASE.  De naam van de ASE wordt in een ASE met een externe VIP voor toepassingen die zijn gemaakt in deze ASE gebruikt in het subdomein.  
De ASE heette ***contosotest*** en uw app ASE heette dat ***mytest*** vervolgens het subdomein van de indeling ***contosotest.p.azurewebsites.net*** zou zijn als de URL voor dit app zou worden ***mytest.contosotest.p.azurewebsites.net***.  
Als u het VIP Type ingesteld op Internal, wordt uw naam ASE niet gebruikt in het subdomein voor de ASE.  U opgeven het subdomein expliciet.  Een subdomein is ***contoso.corp.net*** als u een app in dat ASE met de naam ***timereporting*** gemaakt zou de URL voor de app die ***timereporting.contoso.corp.net***zijn.


## <a name="apps-in-an-ilb-ase"></a>Apps in een ASE ILB ##

Maken van een app in een ASE ILB is hetzelfde als het maken van een app in een ASE normaal.  

1. Selecteer **Nieuw -> Web + Mobile -> Web** of **mobiele** of **API App** in Azure portal
2. Voer de naam van app
2. Selecteer abonnement
3. Resourcegroep maken of selecteren
4. Selecteer of maak App Service Plan(ASP).  Als het maken van een nieuwe ASP de ASE selecteren als de locatie en selecteer vervolgens de werknemer toepassingen die u wilt dat uw ASP worden gemaakt.  Wanneer u de ASP-code maakt u uw ASE als de locatie en de toepassingen van de werknemer.  Als u de naam van de app ziet u het subdomein onder de naam van uw app wordt vervangen door het subdomein voor de ASE.   
5. Selecteer maken.  Als u wilt dat de app weergeven op het dashboard moet u het selectievakje **pincode om dashboard te** selecteren.  

![][2]


Naam van het subdomein wordt onder de naam van de app bijgewerkt zodat het subdomein van uw ASE.  


## <a name="post-ilb-ase-creation-validation"></a>Validatie van post ILB ASE maken ##

Een ASE ILB is iets anders dan de niet - ILB-ASE.  Zoals reeds opgemerkt moet u uw eigen DNS beheren en u moet ook uw eigen certificaat voor HTTPS-verbindingen bieden.  


Nadat u uw ASE maakt zult u zien dat het subdomein het subdomein wordt opgegeven en er een nieuw item in het menu van **instelling** **ILB certificaat**genoemd.  De ASE is gemaakt met een zelf-ondertekend certificaat, waardoor het makkelijker wordt voor het testen van HTTPS.  Het portaal laat u weten dat u nodig hebt om uw eigen certificaat voor HTTPS, maar dit is om u te voorzien van een certificaat met een subdomein.  

![][3]


Als u probeert gewoon dingen uit en het maken van een certificaat niet weet, kunt u de IIS MMC-consoletoepassing voor het maken van een zelf-ondertekend certificaat.  U kunt nadat deze is gemaakt als een .pfx-bestand exporteren en upload deze in de gebruikersinterface van de ILB-certificaat. Wanneer u een site die is beveiligd met een zelf-ondertekend certificaat, uw browser, krijgt u een waarschuwing dat de site die u wilt openen is niet veilig als gevolg van het feit dat het certificaat te valideren.  Als u wilt voorkomen dat deze waarschuwing moet u een correct ondertekende certificaat dat overeenkomt met een subdomein en heeft een vertrouwensketen die door uw browser wordt herkend.

![][6]

Als u wilt proberen de stroom met een eigen certificaat en het testen van HTTP- en HTTPS-toegang tot uw ASE:

1.  Ga naar de UI ASE nadat ASE is gemaakt **ASE -> Instellingen -> ILB certificaten**
2.  ILB certificaat ingesteld door het PFX-certificaatbestand selecteren en wachtwoord op te geven.  Gaat even verwerken en wordt het bericht dat een schaal bewerking uitgevoerd wordt weergegeven.
3.  Het ILB adres krijgen voor uw ASE (**ASE -> Eigenschappen -> virtuele IP-adres**)
4.  Een web app in een ASE maken na het maken van 
5.  Een VM maken als er geen één van die VNET (niet in hetzelfde subnet als de pauze ASE of dingen)
6.  DNS instellen voor het subdomein.  U kunt jokertekens gebruiken met een subdomein in DNS of als u wilt enkele eenvoudige tests uitvoeren, bewerk het hostsbestand op uw VM web app naam VIP-IP-adres instellen.  Als uw ASE had de subdomeinnaam. ilbase.com en u het web app mytestapp aangebracht zodat zou op mytestapp.ilbase.com gericht wordt ingesteld dat in uw hosts-bestand.  (In Windows het hosts-bestand is tegen C:\Windows\System32\drivers\etc\)
7.  Gebruik een browser op dat VM en Ga naar http://mytestapp.ilbase.com (of wat de naam van uw web app met een subdomein is)
8.  Gebruik een browser op dat VM en Ga naar de https://mytestapp.ilbase.com hebt u het gebrek aan beveiliging accepteren als een zelf-ondertekend certificaat.  


Het IP-adres van het ILB wordt weergegeven in de eigenschappen als het virtuele IP-adres

![][4]


## <a name="using-an-ilb-ase"></a>Met behulp van een ILB ASE ##

#### <a name="network-security-groups"></a>Netwerk-beveiligingsgroepen ####

Een ASE ILB kunnen netwerkisolatie voor uw toepassingen als de apps niet toegankelijk of zelfs bekend zijn met het internet zijn.  Dit is uitstekend voor intranetsites zoals zakelijke toepassingen die als host fungeert.  Als u nodig hebt om toegang te beperken zelfs kunt verder u Network Security Groups(NSGs) toegang op het netwerkniveau van het. 


Als u gebruik wenst te NSGs verder om toegang te beperken moet u controleren of beschadigt u de mededeling dat de ASE nodig heeft om te kunnen functioneren.  Hoewel de HTTP/HTTPS-toegang beperken tot het ILB gebruikt door de ASE steeds de ASE nog gebruik van resource buiten de VNet.  Om te zien welke toegang tot het netwerk is nog steeds vereist Kijk op de gegevens in het document op [Binnenkomend verkeer naar een App-omgeving beheren] [ ControlInbound] en het document op de [Gegevens in de netwerkconfiguratie App Service in omgevingen met ExpressRoute][ExpressRoute].  


Om uw NSGs te configureren, moet u het IP-adres dat door Azure wordt gebruikt voor het beheren van uw ASE.  Dat IP-adres is ook de uitgaande IP-adres van uw ASE als internet-verzoeken.  Om te zoeken naar dit IP adres gaat u naar **Instellingen -> Eigenschappen** en de **Uitgaande IP-adres**vinden.  

![][5]


#### <a name="general-ilb-ase-management"></a>Management algemeen ILB ASE ####

Het beheren van een ASE ILB is grotendeels hetzelfde als normaal gesproken een ASE beheren.  U moet voor het schalen van uw werknemer toepassingen meer exemplaren van de ASP-en schalen van uw front-end-servers om grotere hoeveelheden van het HTTP/HTTPS-verkeer verwerken.  Voor algemene informatie over het beheren van de configuratie van een ASE, lees het document over het [configureren van een App-omgeving][ASEConfig].  


De artikelen extra management zijn Certificaatbeheer en DNS-beheer.  U moet downloaden en uploaden van het certificaat na het maken van de ILB-ASE voor HTTPS en vervangen voordat deze verloopt.  Omdat Azure eigenaar is van het domein basis bieden we certificaten voor ASEs met een externe VIP.  Aangezien het subdomein gebruikt door een ASE ILB van alles zijn kan, moet u uw eigen certificaat voor HTTPS. 


#### <a name="dns-configuration"></a>DNS-configuratie ####

Wanneer u een externe VIP de DNS-server wordt beheerd door Azure.  Elke toepassing die is gemaakt in de ASE wordt automatisch toegevoegd aan de Azure DNS is een openbare DNS-server.  U hebt uw eigen DNS beheren in een ASE ILB.  U moet voor een bepaald subdomein, zoals contoso.corp.net A DNS-records die wijzen naar uw adres ILB voor maken:

    * 
    ftp *.SCM publiceren 


## <a name="getting-started"></a>Aan de slag
Alle artikelen en hoe-aan de App serviceomgevingen zijn beschikbaar in het [Leesmij-bestand voor Application Service-omgevingen](../app-service/app-service-app-service-environments-readme.md).

Zie [Inleiding App Service-omgevingen] aan de slag met App Service-omgevingen[WhatisASE]

Zie voor meer informatie over het platform Azure App Service [Azure App Service][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createilbase.png
[2]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createapp.png
[3]: ./media/app-service-environment-with-internal-load-balancer/ilbase-newase.png
[4]: ./media/app-service-environment-with-internal-load-balancer/ilbase-vip.png
[5]: ./media/app-service-environment-with-internal-load-balancer/ilbase-externalvip.png
[6]: ./media/app-service-environment-with-internal-load-balancer/ilbase-ilbcertificate.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[vnetnsgs]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
