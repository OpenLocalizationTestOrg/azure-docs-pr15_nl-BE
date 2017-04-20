<properties 
    pageTitle="Azure RemoteApp Veelgestelde vragen | Microsoft Azure" 
    description="Informatie over antwoorden op de meeste Veelgestelde vragen over Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="swadhwa" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="azure-remoteapp-faq"></a>Azure RemoteApp-Veelgestelde vragen

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

We hebben de volgende vragen over Azure RemoteApp gehoord. Anderen hebben? Ga naar de [forums van RemoteApp-](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureRemoteApp) en laat ons weten wat u moet weten of een opmerking onderstaande vervolgkeuzelijst.

## <a name="cant-find-what-youre-looking-for-have-a-question-we-didnt-answer"></a>Kan niet vinden wat u zoekt? Een vraag die we het antwoord niet hebben?
Als u niet de informatie vinden u nodig hebt of u een aanvullende vraag hebt waarop we bent die niet hier, Ga naar het [forum RemoteApp Azure](http://aka.ms/araforum) en er uw vraag stellen. We kunnen altijd meer antwoorden hier toevoegen.

## <a name="what-is-azure-remoteapp"></a>Wat is Azure RemoteApp? ##


- **Wat is Azure RemoteApp?** RemoteApp is dat een Azure service helpt u veilige, externe toegang bieden tot toepassingen van veel apparaten van andere gebruiker. Meer informatie over [RemoteApp Azure](remoteapp-whatis.md).
- **Wat zijn de opties voor distributie?** Er zijn twee soorten RemoteApp-collecties: cloud en hybride. Welke dat u nodig hebt, hangt af van een aantal factoren, zoals of u nodig hebt aan domein toevoegen. We praten over al deze besluiten [hier](remoteapp-collections.md).

## <a name="quick-tips-on-using-azure-remoteapp"></a>Snelle tips over het gebruik van Azure RemoteApp ##
- **Hoe lang totdat ik heb verbroken? Hoe lang ik inactief kan zijn voordat u Geef me de boot?** 4 uur. Als u of een van uw gebruikers niet actief is gedurende 4 uur, zult u automatisch worden aangemeld bij de Azure RemoteApp. Bekijk de andere standaardinstellingen in [Azure abonnement Service grenzen, quota's en beperkingen](../azure-subscription-service-limits.md).
- **Kan ik deze service gratis proberen?** Ja. Er is een gratis proefversie voor 30 dagen. Nadat de proefperiode is afgelopen, kunt u overstappen naar een betaalde account (die u kunt gebruiken in productie) of via de service stoppen. Start uw gratis proeflicentie door te gaan naar [portal.azure.com](http://portal.azure.com) - maakt een nieuw exemplaar van RemoteApp. Met de gratis proefversie kunt u 2 exemplaren van RemoteApp bouwen met 10 gebruikers per exemplaar. Houd er rekening mee dat deze evaluatieversie voor 30 dagen alleen woont.
## <a name="azure-remoteapp-subscription-details"></a>Azure RemoteApp-abonnement details ##

- **Wat zijn de beperkingen van de service?** U kunt informatie over de standaardinstellingen en de grenzen van de service van Azure RemoteApp in [Azure abonnement Service grenzen, quota's en beperkingen](../azure-subscription-service-limits.md). Laat ons weten als u nog meer vragen hebt.
- **Hoeveel gebruikers heb ik hebben?** Er is een minimum van 20 gebruikers. Ik wil herhalen dat super duidelijk - als u het MINIMUM aantal is 20. U wordt gefactureerd voor 20. 
- **Wat kost RemoteApp?** [Azure RemoteApp-prijzen Details ](https://azure.microsoft.com/pricing/details/remoteapp/)uitchecken.
- **Kost één type collectie meer dan een ander?** Ja, dit kan, afhankelijk van de vereisten van uw collectie. Een hybride-collectie vereist een verbinding met uw netwerk op ruimten Azure RemoteApp. Als u een bestaande VNET/Express-Route, is er geen extra kosten. Maar als u een nieuwe Azure-VNET en een gateway of een Route Express brengt voor de [VPN-gateway](https://azure.microsoft.com/pricing/details/vpn-gateway) of [Route Express](https://azure.microsoft.com/pricing/details/expressroute/). Deze kosten (Zie de koppelingen) is op uw maandelijkse Azure RemoteApp kosten.

## <a name="collections---whats-supported-which-should-you-use-and-others"></a>Incasso - wat wordt ondersteund, moet u gebruik maken van en anderen
- **Aangepaste line of business (LOB) toepassingen ondersteunt?** Ja. Als u een aangepaste toepassing in Azure RemoteApp, een [afbeelding van de aangepaste sjabloon](remoteapp-create-custom-image.md)maken en vervolgens uploaden naar de RemoteApp-collectie.
- **Mijn aangepaste LOB-toepassing werkt in Azure RemoteApp?** De beste manier om een afbeelding die uit te testen. De [Extern bureaublad -Sessiehost Compatibiliteitscentrum](http://www.rdcompatibility.com/compatibility/default.aspx)uitchecken.
- **Welke implementatiemethode (cloud of hybride) is het meest geschikt voor mijn organisatie?** Hybride collecties bieden de meest complete ervaring als u wilt dat de volledige integratie met eenmalige aanmelding (SSO) en veilig op gebouwen netwerkconnectiviteit. Wolk collecties bieden een flexibele en eenvoudige manier uw implementatie isoleren met meerdere verificatiemethoden. Meer informatie over de [Opties voor distributie](remoteapp-whatis.md).
- **We hebben SQL of een andere database in ruimten of in Azure. Welk type installatie moeten we gebruiken?** Dat is afhankelijk van waar uw SQL- of back-end-database is. Als de database zich in een particulier netwerk, gebruikt u de collectie hybride. Als de database is toegankelijk via het Internet en kan de client verbindingen tot stand te brengen, kunt u de cloud-collectie.
- **Wat over stationstoewijzing, USB en seriële poort, het gedeelde Klembord en printeromleiding?** Al deze functies worden ondersteund in Azure RemoteApp. Klembord delen en printeromleiding is standaard ingeschakeld. U kunt meer lezen over omleiding [hier](remoteapp-redirection.md). 


## <a name="template-images"></a>Sjabloon voor afbeeldingen
- **Kan ik gebruiken een wolk of een bestaande virtuele machine als de sjabloon voor Mijn verzameling RemoteApp?** Ja! U kunt een afbeelding op basis van een Azure VM maken, gebruikt u een van de afbeeldingen die deel uitmaken van uw abonnement of een aangepaste installatiekopie maken. Bekijk de [Opties voor RemoteApp-afbeeldingen](remoteapp-imageoptions.md).


## <a name="network-options"></a>Netwerkopties
- **De hybride-collectie is een VNET vereist. Kunnen we onze bestaande VNET gebruiken?** U kunt als de bestaande VNET een VNET met Azure is. Zie ' stap 1: instellen van het virtuele netwerk "in de [instructies van hybride-collectie](remoteapp-create-hybrid-deployment.md) voor meer informatie.
- **Kan ik een VNET met een cloud-collectie gebruiken** U kunt inderdaad. Bekijk [een verzameling cloud maken](remoteapp-create-cloud-deployment.md), met name stap 1 voor meer informatie.

## <a name="authentication-options"></a>Verificatie-opties



- **Hoe verificatie? Welke methoden worden ondersteund?** De cloud-collectie ondersteunt Microsoft-accounts en Azure Active Directory-accounts ook Office 365-accounts zijn. De hybride-collectie ondersteunt alleen Azure Active Directory-accounts gesynchroniseerd (met behulp van een hulpprogramma zoals [Azure Active Directory-synchronisatie](http://blogs.technet.com/b/ad/archive/2014/09/16/azure-active-directory-sync-is-now-ga.aspx)) van een Windows Server Active Directory-implementatie; met name een gesynchroniseerd met de optie Wachtwoordsynchronisatie of gesynchroniseerd met Active Directory Federation Services (AD FS) federation geconfigureerd. U kunt ook [Meerledige verificatie (MVR gesloten)](https://azure.microsoft.com/services/multi-factor-authentication/).

>[AZURE.NOTE]De Azure Active Directory-gebruikers moeten zijn van de huurder die is gekoppeld aan uw abonnement. (U kunt weergeven en wijzigen van uw abonnement op het tabblad **Instellingen** in de portal. Zie [de huurder Azure Active Directory wijzigen die wordt gebruikt door RemoteApp](remoteapp-changetenant.md) voor meer informatie.)

- **Waarom worden mijn Azure Active Directory-accounttoegang kan verlenen?** De Azure Active Directory-gebruikers moeten zich vanuit de map die is gekoppeld aan uw abonnement. U kunt weergeven of wijzigen van de map op het tabblad instellingen in de portal. Zie [de huurder Azure Active Directory wijzigen die wordt gebruikt door RemoteApp](remoteapp-changetenant.md) voor meer informatie.

## <a name="clients---what-device-can-i-use-to-access-azure-remoteapp"></a>Clients - welk apparaat kan ik gebruiken voor Azure RemoteApp?
Hier vindt u goede clientinformatie, met inbegrip van de stappen voor het installeren van de verschillende clients bij het [verkrijgen van toegang tot uw toepassingen in Azure RemoteApp](remoteapp-clients.md).

- **Welke apparaten en besturingssystemen het client-toepassingen ondersteunen?**
Eerst de computers en tablets: 
    - Windows 10 (client-voorbeeld)
    - Windows 8.1 en Windows 8
    - Windows 7 servicepack 1
    - Mac OS X
    - Windows RT
    - Android tablets
    - iPads en de telefoons:
    - iPhone
    - Android telefoon
    - Windows Phone
 
    [Download](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) nu een RemoteApp-client.
- **Ondersteunt Azure RemoteApp Thin Clients** Ja, de volgende Windows Embedded thin clients worden ondersteund:
    - Windows Embedded Standard 7
    - Windows Embedded Standard 8
    - Ingesloten Windows 8.1 industrie Pro
    - Windows 10 IoT Enterprise

- **Welke versie van Windows Server wordt ondersteund voor de Remote Desktop Session Host (RDSH)?** Windows Server 2012 R2.

## <a name="support-and-feedback"></a>Ondersteuning en feedback


- **Wat is het ondersteuningsplan voor RemoteApp?** Ondersteuning voor facturering en het abonnement is gratis beschikbaar. Technische ondersteuning is beschikbaar via de [Azure serviceplannen](https://azure.microsoft.com/support/plans/). Ook kunt u gratis Gemeenschap ondersteuning krijgen via onze [Azure discussieforum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp). 
- **Hoe ik feedback verzenden?** Ga naar [Feedbackforum](https://feedback.azure.com/forums/247748-azure-remoteapp/).
- **Wie kan ik meer informatie over RemoteApp Azure communiceren?** Naast onze [forum voor discussie](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp), dat een prima plek om te vragen is, kunt u deelnemen aan de wekelijkse [vragen de deskundigen webinar](https://azureinfo.microsoft.com/US-Azure-WBNR-FY15-11Nov-AzureRemoteAppAskTheExperts-Registration-Page.html), waar we over alle dingen RemoteApp praten.
- **Hoe zit het met RemoteApp-documentatie?** We zijn blij dat u gevraagd. Naast de help-informatie in de lade portal help (Klik op de **?** op elke pagina in de portal), de volgende artikelen zijn beschikbaar om u te leren alles over RemoteApp:
    - **Aan de slag:**
        - [Wat is RemoteApp?](remoteapp-whatis.md)
        - [Wat is in de sjabloon RemoteApp-afbeeldingen?](remoteapp-images.md)
        - [Hoe werkt licentieverlening samen?](remoteapp-licensing.md)
        - [Hoe RemoteApp- en Office werken samen?](remoteapp-o365.md)
        - [Hoe omleiding werkt in RemoteApp](remoteapp-redirection.md)?
    - **Implementeren:**
        - [De afbeelding van een aangepaste sjabloon maken](remoteapp-create-custom-image.md)
        - [Een collectie hybride maken](remoteapp-create-hybrid-deployment.md)
        - [Een collectie cloud maken](remoteapp-create-cloud-deployment.md)
        - [Azure Active Directory configureren voor RemoteApp](remoteapp-ad.md)
        - [Het publiceren van een app in RemoteApp](remoteapp-publish.md)
    - **Beheren:**
        - [Gebruikers toevoegen](remoteapp-user.md)
        - [Aanbevolen procedures voor het configureren en gebruiken van RemoteApp](remoteapp-bestpractices.md)  

    Video's! We hebben ook een aantal video's over RemoteApp. Sommige kunt Inleiding ([Inleiding tot RemoteApp Azure](https://azure.microsoft.com/documentation/videos/cloud-cover-ep-150-azure-remote-app-with-thomas-willingham-and-nihar-namjoshi/)) u anderen u doorlopen deployment ([implementatie van Cloud](https://www.youtube.com/watch?v=3NAv2iwZtGc&feature=youtu.be) en [hybride deployment](https://www.youtube.com/watch?v=GCIMxPUvg0c&feature=youtu.be)). Bekijk ze!

 
### <a name="help-us-help-you"></a>Help ons u te helpen 
Wist u dat naast de beoordeling van dit artikel en het toevoegen van opmerkingen naar beneden onder, kunt u wijzigingen aanbrengen in het artikel zelf? Iets ontbreekt? Iets mis? Schrijf iets dat is gewoon verwarrend? Omhoog schuiven en klik op **bewerken op GitHub** wijzigingen aan te brengen - die voor revisie naar ons komen, en vervolgens, wanneer we deze afmelden, ziet u de wijzigingen en verbeteringen hier.
