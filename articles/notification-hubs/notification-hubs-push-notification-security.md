<properties
    pageTitle="Beveiliging voor kennisgeving Hubs"
    description="In dit onderwerp wordt uitgelegd voor Azure melding hubs."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="security"></a>Beveiliging

##<a name="overview"></a>Overzicht

Dit onderwerp beschrijft het beveiligingsmodel van Azure melding Hubs. Omdat een entiteit Bus Service melding Hubs, implementeren zij hetzelfde beveiligingsmodel als Service Bus. Zie de onderwerpen [Service Bus-verificatie](https://msdn.microsoft.com/library/azure/dn155925.aspx) voor meer informatie.

##<a name="shared-access-signature-security-sas"></a>Beveiliging van gedeelde Access-handtekening (SAS) 

Melding Hubs implementeert een schema voor beveiliging op gebruikersniveau entiteit SAS (gedeelde toegang handtekening) genoemd. Deze regeling kan berichten entiteiten te verklaren tot 12 verificatieregels in hun beschrijving die het verlenen van rechten voor die entiteit.

Elke regel bevat een naam, een waarde (gedeelde geheime) sleutel en een set van rechten, zoals wordt beschreven in de sectie "Beveiliging vorderingen." Bij het maken van een Hub melding twee regels worden gemaakt: één met Listen rechten (met de clienttoepassing) en één met alle rechten (met de back-end app).

Bij het uitvoeren van registratie van beheer van clienttoepassingen, als de gegevens worden verzonden meldingen is niet gevoelig (bijvoorbeeld weer updates), een gebruikelijke manier toegang te krijgen tot een kennisgeving Hub is de waarde van de sleutel van de regel Listen-only toegang geven tot de clienttoepassing en de waarde van de sleutel van de regel volledige toegang geven tot de backend app.

Het is niet raadzaam dat u de waarde van de sleutel in de Windows Store-clienttoepassingen insluiten. Een manier om te voorkomen dat de waarde van de sleutel te embedding is dat het ophalen bij het opstarten van de app-end-clienttoepassing.

Het is belangrijk te begrijpen dat de sleutel Listen toegang kan een client app te registreren voor een label. Als uw app moet registraties beperken tot specifieke codes aan specifieke clients (bijvoorbeeld als labels geven de gebruikers-id's), moet uw backend app de registraties uitvoeren. Zie registratie beheer voor meer informatie. Houd er rekening mee dat op deze manier kan de clienttoepassing geen directe toegang tot kennisgeving Hubs.

##<a name="security-claims"></a>Beveiliging aanvragen

Net als bij andere entiteiten, melding Hub bewerkingen zijn toegestaan voor de drie argumenten van beveiliging: luisteren, versturen en beheren.

| Claim | Beschrijving | Toegestane bewerkingen |
|-------|-------------|--------------------|
| Luisteren | Maken of bijwerken, lezen en verwijderen van enkele registraties | Registratie maken of bijwerken<br><br>Lees de registratie<br><br>Alle registraties voor een ingang gelezen<br><br>Registratie verwijderen |
| Verzenden | Berichten verzenden naar de hub melding | Bericht verzenden |
| Beheren | CRUDs op de kennisgeving Hubs (inclusief PNS referenties en beveiligingssleutels bijwerken) en lees registraties op basis van tags | Hubs melding maken, bijwerken of lezen/verwijderen<br><br>Registraties door code te lezen |


Melding Hubs verleend door Microsoft Azure Access Control tokens en handtekening tokens gegenereerd met gedeelde sleutels die zijn geconfigureerd op de Hub melding rechtstreeks vorderingen te accepteren.