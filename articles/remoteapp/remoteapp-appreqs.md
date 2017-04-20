
<properties
    pageTitle="Vereisten voor Azure RemoteApp-App | Microsoft Azure"
    description="Meer informatie over de vereisten voor toepassingen die u wilt gebruiken in Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="app-requirements"></a>App-vereisten

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Azure RemoteApp ondersteunt streaming 32-bits of 64-bits Windows-toepassingen vanaf een installatiekopie van Windows Server 2012 R2. De meeste bestaande 32-bits of 64-bits Windows-toepassingen worden uitgevoerd "as is" in Azure RemoteApp (extern bureaublad-Services of voorheen bekend als Terminal Services) omgeving. Echter, er is een verschil tussen uitgevoerd en goed - sommige toepassingen goed werken en uitvoeren, terwijl andere niet. De volgende informatie bevat voor het ontwikkelen van toepassingen in een omgeving voor extern bureaublad-Services en compatibiliteit testen.

Tip: We ook werken enkele praktijkvoorbeelden van apps voor u maken. Hier ziet u nieuwe onderwerpen bespreken met behulp van Microsoft Access, QuickBooks en App-V in RemoteApp.

## <a name="requirements"></a>Vereisten
Deze drie vereisten kunnen als gevolgd, u uw toepassing uitvoeren in RemoteApp-en:

1.  Volledige compatibiliteit met RemoteApp-hebt toepassingen die voldoen aan alle [Certificaatvereisten voor Windows desktop apps](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) en voldoen aan de [richtlijnen voor programmeren van extern bureaublad-Services](https://msdn.microsoft.com/library/aa383490.aspx) .
2.  Toepassingen moeten nooit gegevens lokaal opslaan op de afbeelding of het RemoteApp-exemplaren die verloren kunnen gaan.  Nadat u een RemoteApp-collectie gemaakt, de exemplaren worden gekloond en stateless zijn en mag alleen toepassingen bevatten. Gegevens opslaan in een externe gegevensbron of in het profiel van de gebruiker.
3.  Aangepaste afbeeldingen mag nooit bevatten gegevens die verloren kunnen gaan.  

## <a name="testing-your-apps"></a>Uw apps testen
Voer deze stappen uit voor het testen van toepassingen:

1.  Windows Server 2012 R2 en de toepassing installeren
2.  Extern bureaublad inschakelen
3.  Twee gebruikersaccounts maken, GebruikerA en verleent, beide gebruikersaccounts toe te voegen aan de groep Extern bureaublad.
4.  Multisessie-compatibiliteit controleren door de vaststelling van twee gelijktijdige RDS-sessies op de computer bij het starten van de toepassing.
5.  App gedrag valideren

## <a name="application-development-guidelines"></a>Ontwikkelingsondersteuning voor toepassingen
Gebruik de volgende richtlijnen voor het ontwikkelen van toepassingen voor RemoteApp.

### <a name="multiple-users"></a>Meerdere gebruikers

- Installatie van een [toepassing voor één gebruiker ](https://msdn.microsoft.com/library/aa380661.aspx)kan problemen veroorzaken in een omgeving voor meerdere gebruikers.
- Toepassingen moeten [slaan gebruikersspecifieke informatie](https://msdn.microsoft.com/library/aa383452.aspx) gebruiker-specifieke locaties, onafhankelijk van de algemene informatie die voor alle gebruikers geldt.
- RemoteApp wordt gebruikt voor meerdere [naamruimten voor kernel-objecten](https://msdn.microsoft.com/library/aa382954.aspx); een globale naamruimte wordt hoofdzakelijk gebruikt door services in client/server-toepassingen.
- Het is niet veilig om aan te nemen dat de computernaam of het [IP-adres](https://msdn.microsoft.com/library/aa382942.aspx) toegewezen aan de computer zijn gekoppeld aan een enkele gebruiker omdat meerdere gebruikers kunnen gelijktijdig worden aangemeld met een Remote Desktop Session Host (RD Session Host)-server.

### <a name="performance"></a>Prestaties
- [Grafische effecten](https://msdn.microsoft.com/library/aa380822.aspx) uitschakelen voordat u uw app voor RemoteApp toevoegen.
- Maximale CPU-beschikbaarheid voor alle gebruikers, [achtergrondtaken](https://msdn.microsoft.com/library/aa380665.aspx) uitschakelen of maak efficiënte achtergrondtaken die geen systeembronnen.
- U moet afstemmen en saldo [thread gebruik](https://msdn.microsoft.com/library/aa383520.aspx) van toepassingen in een omgeving met meerdere gebruikers, met meerdere processors.
- Om prestaties te optimaliseren, is het verstandig om toepassingen te [detecteren](https://msdn.microsoft.com/library/aa380798.aspx) of ze worden uitgevoerd in een clientsessie.
