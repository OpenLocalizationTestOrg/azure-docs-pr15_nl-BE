<properties 
    pageTitle="RADIUS-verificatie en Azure meerledige verificatieserver"
    description="Dit is de pagina Azure meerledige verificatie die helpt bij de implementatie van RADIUS-verificatie en Azure meerledige verificatieserver."
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



# <a name="radius-authentication-and-azure-multi-factor-authentication-server"></a>RADIUS-verificatie en Azure meerledige verificatieserver

Het gedeelte van de RADIUS-verificatie kunt u inschakelen en configureren van RADIUS-verificatie voor de Server Azure meerledige verificatie. RADIUS is een standaardprotocol om verificatieaanvragen te accepteren en om de aanvragen te verwerken. De Azure meerledige verificatieserver fungeert als een RADIUS-server en wordt ingevoegd tussen de RADIUS-client (bijvoorbeeld VPN-toestel) en het verificatiedoel-, die kan Active Directory (AD), een LDAP-adreslijst of een andere RADIUS-server toe te voegen Azure meerledige verificatie. Voor Azure meerledige verificatie van functie, moet u de Server Azure meerledige verificatie configureren zodat het met de servers van de client en het verificatiedoel communiceren kan. De Azure meerledige verificatieserver accepteert verzoeken van een RADIUS-client, verifieert referenties ten opzichte van het verificatiedoel wordt toegevoegd Azure meerledige verificatie en stuurt een antwoord terug naar de RADIUS-client. De volledige verificatie alleen slagen als het mislukt de verificatie van de primaire en de Azure meerledige verificatie.

>[AZURE.NOTE]
>De MVR gesloten Server biedt alleen ondersteuning voor PAP (wachtwoord authentication protocol) en MSCHAPv2 (Microsoft Challenge Handshake Authentication Protocol) RADIUS protocollen als fungeert als een RADIUS-server.  Andere protocollen, zoals EAP (extensible authentication protocol) kunnen worden gebruikt wanneer de MVR gesloten server fungeert als een RADIUS-proxy naar een andere RADIUS-server die dit protocol zoals Microsoft NPS ondersteunt.
></br>
>Als u andere protocollen gebruikt in deze configuratie, werkt unilaterale SMS en EED tokens niet omdat de MVR gesloten-Server kan niet tot stand brengen van een geslaagde RADIUS-vraag/antwoord dat protocol gebruiken.


![RADIUS-verificatie](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## <a name="radius-authentication-configuration"></a>Configuratie van RADIUS-verificatie

RADIUS-verificatie configureren, de Azure meerledige verificatieserver te installeren op een Windows server. Als er een Active Directory-omgeving, moet de server worden toegevoegd aan het domein in het netwerk. Gebruik de volgende procedure voor het configureren van de Server Azure meerledige verificatie:

1. Binnen de Azure meerledige verificatieserver klikt u op het pictogram van de RADIUS-verificatie in het linkermenu.
2. Schakel het selectievakje inschakelen RADIUS-verificatie.
3. Wijzig de poorten voor verificatie en Accounting poort(en) als de Azure meerledige verificatie, RADIUS-service aan niet-standaard poorten binden te om te luisteren naar de RADIUS-aanvragen van clients die worden geconfigureerd op het tabblad Clients.
4. Klik op toevoegen... de knop.
5. Voer in het dialoogvenster RADIUS-Client toevoegen het IP-adres van het toestel/server die verifieert de Azure meerledige verificatieserver, de naam van een toepassing (optioneel) en een gedeeld geheim. Het gedeelde geheim moet hetzelfde zijn op de Server van Azure meerledige verificatie en toestel/server. Naam van de toepassing wordt weergegeven in rapporten Azure meerledige verificatie en kan worden weergegeven in SMS of Mobile App verificatieberichten.
6. Schakel het selectievakje meerledige verificatie vereisen gebruiker overeenkomst in als alle gebruikers zijn of worden geïmporteerd in de Server en meerledige verificatie. Als u een groot aantal gebruikers nog niet zijn geïmporteerd in de Server en/of zullen worden vrijgesteld van meerledige verificatie, laat u het vak uitgeschakeld. Raadpleeg de help voor meer informatie over deze functie.
7. Het selectievakje Enable fallback EDE token als de mobiele app Azure meerledige verificatie worden gebruikt en u wilt passcodes EED als een terugval naar de out-of-band telefoon bellen, SMS of push notification-verificatie gebruiken.
8. Klik op OK.
9. U kunt Herhaal stap 4 tot en met 8 om extra RADIUS-clients toevoegen.
10. Klik op het tabblad doel.
11. Als de Azure meerledige verificatieserver is geïnstalleerd op een server deel uitmaakt van een domein in een Active Directory-omgeving, selecteert u Windows-domein.
12. Als gebruikers moeten worden geverifieerd ten opzichte van een LDAP-adreslijst, selecteert u de LDAP-binding. Als u LDAP-binding gebruikt, moet u op het pictogram Directory-integratie en de LDAP-configuratie op het tabblad Instellingen bewerken zodat de Server aan de directory binden kan. In de configuratie van LDAP-Proxy handleiding vindt u instructies voor het configureren van LDAP.
13. Als gebruikers moeten worden geverifieerd ten opzichte van een andere RADIUS-server, selecteert u RADIUS-server (s).
14. Configureer de server dat de Server wordt proxy de RADIUS-aanvragen naar door te klikken op toevoegen... de knop.
15. Voer in het dialoogvenster RADIUS-Server toevoegen het IP-adres van de RADIUS-server en een gedeeld geheim. Het gedeelde geheim moet hetzelfde zijn op de Server van Azure meerledige verificatie en de RADIUS-server. De poort voor verificatie en accountingpoort wijzigen als u andere poorten worden gebruikt door de RADIUS-server.
16. Klik op OK.
17. U moet de Azure meerledige verificatieserver als een RADIUS-client op de RADIUS-server toevoegen zodat afkomstig van de Server Azure meerledige verificatie aanvragen worden verwerkt. U moet hetzelfde gedeelde geheim op de Azure meerledige verificatieserver geconfigureerd.
18. Deze stap voor aanvullende RADIUS-servers toevoegen en configureren van de volgorde waarin de Server deze met de knoppen omhoog en omlaag aanroept moet kunt u herhalen. Dit is de serverconfiguratie Azure meerledige verificatie voltooid. De Server is nu beschikbaar op de geconfigureerde poorten voor RADIUS-aanvragen van de geconfigureerde clients.   


## <a name="radius-client-configuration"></a>RADIUS-clientconfiguratie

Gebruik de richtlijnen om de RADIUS-client configureren:

- Uw toestel/server worden geverifieerd via de RADIUS naar Azure meerledige verificatie van de Server IP-adres, dat als de RADIUS-server fungeren zal configureren.
- Gebruik hetzelfde gedeelde geheim dat hierboven is geconfigureerd.
- De RADIUS-time-outwaarde van 30-60 seconden zo configureren dat er tijd is om te valideren van de referenties van de gebruiker, de meerledige verificatie uitvoeren, hun reactie ontvangen en vervolgens reageren op de RADIUS-access-request.
