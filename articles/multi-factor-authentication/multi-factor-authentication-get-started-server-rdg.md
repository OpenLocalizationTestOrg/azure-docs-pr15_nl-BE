<properties 
    pageTitle="Extern bureaublad-Gateway en Azure meerledige verificatieserver RADIUS gebruiken"
    description="Dit is de pagina Azure meerledige verificatie die helpt bij het implementeren van extern bureaublad (RD) Gateway en Azure meerledige verificatieserver RADIUS gebruiken."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="kgremban"/>

# <a name="remote-desktop-gateway-and-azure-multi-factor-authentication-server-using-radius"></a>Extern bureaublad-Gateway en Azure meerledige verificatieserver RADIUS gebruiken

Extern bureaublad-Gateway wordt in veel gevallen de lokale NPS gebruikt om gebruikers te verifiëren. Dit document wordt beschreven hoe u RADIUS-aanvraag uit van de extern bureaublad-Gateway (via de lokale NPS) naar de Server meerledige verificatie.

Het meerledige verificatie-Server moet worden geïnstalleerd op een afzonderlijke server, die vervolgens proxy de RADIUS-aanvraag naar de NPS op de extern bureaublad-gatewayserver. Nadat u NPS evalueert de gebruikersnaam en het wachtwoord, wordt een antwoord naar de Server van meerledige verificatie waarmee de tweede factor verificatie voordat een resultaat wordt geretourneerd naar de gateway geretourneerd.





## <a name="configure-the-rd-gateway"></a>De RD-Gateway configureren

De RD-Gateway moet worden geconfigureerd voor het verzenden van RADIUS-verificatie met een Server Azure meerledige verificatie. Zodra de RD-Gateway is geïnstalleerd, geconfigureerd en werkt, gaat u naar de eigenschappen van de RD-Gateway. Ga naar het tabblad RD CAP-archief en wijzigen voor het gebruik van een centrale server waarop NPS wordt uitgevoerd in plaats van de lokale server met NPS. Voeg een of meer Azure meerledige verificatieservers toe als RADIUS-servers en een gedeeld geheim voor elke server opgeven.





## <a name="configure-nps"></a>NPS configureren

De RD-Gateway gebruikt NPS RADIUS-verzoek verzenden naar Azure meerledige verificatie. Er is een time-out moet worden gewijzigd om te voorkomen dat de RD-Gateway van een time-out opgetreden voordat een meerledige verificatie heeft voltooid. Gebruik de volgende procedure voor het configureren van NPS.

1. Vouw het RADIUS-Clients en Server-menu in de linkerkolom en klik op externe RADIUS-servergroepen in NPS. Ga naar de eigenschappen van de groep TS-GATEWAY-SERVER. Bewerk de RADIUS-server (s) weergegeven en Ga naar het tabblad taakverdeling. Wijzigen van de "aantal seconden zonder reactie voordat de aanvraag wordt beschouwd als verwijderd" en het "aantal seconden tussen verzoeken als de server wordt geïdentificeerd als niet beschikbaar" 30-60 seconden. Klik op het tabblad verificatie/rekening en zorg ervoor dat de RADIUS-poorten die overeenkomen met de poorten waarop de Server meerledige verificatie wordt geluisterd.
2. NPS moet ook worden geconfigureerd voor het ontvangen van RADIUS-verificatie van de Server Azure meerledige verificatie. Klik op RADIUS-Clients in het linkermenu. De Azure meerledige verificatieserver toevoegen als een RADIUS-client. Kies een beschrijvende naam en een gedeeld geheim opgeven.
3. Vouw de beleid-sectie in de navigatiebalk aan de linkerkant en klik op beleid voor verbindingsaanvragen. Een verbinding aanvragen beleid voor TS GATEWAY-VERIFICATIEBELEID dat is gemaakt tijdens de configuratie van RD-Gateway moet bevatten. Dit beleid stuurt de RADIUS-aanvragen naar de Server meerledige verificatie.
4. Dit beleid wilt maken van een nieuwe kopiëren. Voeg een voorwaarde die overeenkomt met de beschrijvende naam van de Client met de beschrijvende naam voor de Azure meerledige verificatie Server RADIUS-client in stap 2 hierboven instellen in het nieuwe beleid. De verificatieprovider op de lokale Computer wijzigen. Dit beleid zorgt ervoor dat als een RADIUS-aanvraag wordt ontvangen van de Server Azure meerledige verificatie, de verificatie plaatsvindt lokaal in plaats van het verzenden van een RADIUS-aanvraag naar de Server Azure meerledige verificatie die in een lusvoorwaarde resulteert. Als u wilt voorkomen dat de lusvoorwaarde, moet dit nieuwe beleid boven het oorspronkelijke beleid dat wordt doorgestuurd naar de Server meerledige verificatie worden besteld.

## <a name="configure-azure-multi-factor-authentication"></a>Azure meerledige verificatie configureren


--------------------------------------------------------------------------------



De Azure meerledige verificatie-Server is geconfigureerd als een RADIUS-proxy tussen RD-Gateway en NPS.  Het moet worden geïnstalleerd op een server deel uitmaakt van een domein, dat losstaat van de RD-gatewayserver. Gebruik de volgende procedure voor het configureren van de Server Azure meerledige verificatie.

1. Open de Server Azure meerledige verificatie en klik op het pictogram voor RADIUS-verificatie. Schakel het selectievakje inschakelen RADIUS-verificatie.
2. Zorg ervoor de poorten overeenkomen met wat in NPS is geconfigureerd op het tabblad Clients en klik op toevoegen... de knop. Voeg het IP-adres van de RD-Gateway-server, de naam van de toepassing (optioneel) en een gedeeld geheim. Het gedeelde geheim moet hetzelfde zijn op de Azure meerledige verificatieserver en de RD-Gateway.
3. Klik op het tabblad doel en kiest u het keuzerondje van de RADIUS-server (s).
4. Klik op toevoegen... de knop. Voer het IP-adres, het gedeelde geheim en poorten van de NPS-server. Tenzij een centrale NPS gebruikt, is de RADIUS-client en de RADIUS-doel hetzelfde. Het gedeelde geheim moet overeenkomen met het bestand dat in het gedeelte van de RADIUS-client van de NPS-server setup.

![RADIUS-verificatie](./media/multi-factor-authentication-get-started-server-rdg/radius.png)
