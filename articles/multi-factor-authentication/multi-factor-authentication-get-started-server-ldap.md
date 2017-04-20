<properties 
    pageTitle="LDAP-verificatie en Azure meerledige verificatieserver"
    description="Dit is de pagina Azure meerledige verificatie die helpt bij het implementeren van LDAP-verificatie en Azure meerledige verificatieserver."
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
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="ldap-authentication-and-azure-multi-factor-authentication-server"></a>LDAP-verificatie en Azure meerledige verificatieserver


De Server Azure meerledige verificatie is standaard geconfigureerd om te importeren of gebruikers uit Active Directory synchroniseren. Het kan echter worden geconfigureerd om te binden aan een andere LDAP-directory's, zoals een ADAM-directory, of een bepaalde Active Directory-domeincontroller. Wanneer geconfigureerd voor verbinding met een map via LDAP, kan de Azure meerledige verificatieserver worden geconfigureerd om te fungeren als een LDAP-proxy verificaties uitvoeren. Daarnaast kunt u voor het gebruik van LDAP-binding een target RADIUS voor verificatie van gebruikers bij het gebruik van IIS-verificatie vooraf of primaire verificatie in de Azure meerledige verificatie User Portal.

Azure meerledige verificatie gebruikt als een LDAP-proxy, de Azure meerledige verificatieserver ingevoegd tussen de LDAP-client (bijvoorbeeld VPN-toestel, toepassing) en de LDAP-adreslijstserver toevoegen meerledige verificatie. Voor Azure meerledige verificatie van functie, moet de Server Azure meerledige verificatie om te communiceren met de LDAP-directory en de servers van de client worden geconfigureerd. In deze configuratie de Azure meerledige verificatieserver LDAP-aanvragen van clients, servers en toepassingen accepteert en stuurt deze door naar de doel LDAP directory server de primaire referenties te valideren. Als het antwoord van de LDAP-adreslijst wordt weergegeven dat primaire referenties geldig zijn, worden de Azure meerledige verificatie tweede verificatiestap uitvoert en stuurt een antwoord terug naar de LDAP-client. De volledige verificatie slaagt alleen als zowel de verificatie van de LDAP-server en de meerledige verificatie is gelukt.





## <a name="ldap-authentication-configuration"></a>Configuratie van LDAP-verificatie


Configureren van LDAP-verificatie de Azure meerledige verificatieserver te installeren op een Windows server. Gebruik de volgende procedure:

1. Binnen de Azure meerledige verificatieserver klikt u op het pictogram van de LDAP-verificatie in het linkermenu.
2. Schakel het selectievakje LDAP-verificatie inschakelen.![LDAP-verificatie](./media/multi-factor-authentication-get-started-server-ldap/ldap2.png)
3. Wijzig de TCP-poort en de SSL-poort als de Azure meerledige verificatie LDAP-service aan niet-standaard poorten binden te om te luisteren naar de LDAP-aanvragen van clients die worden geconfigureerd op het tabblad Clients.
4. Als u LDAPS van de client naar de Server Azure meerledige verificatie gebruiken, moet een SSL-certificaat worden geïnstalleerd op de server die op de Server wordt uitgevoerd. Klik op de bladeren... naast het vak SSL-certificaat en selecteer het geïnstalleerde certificaat dat wordt gebruikt voor de beveiligde verbinding.
5. Klik op toevoegen... de knop.
6. Voer in het dialoogvenster LDAP-Client toevoegen het IP-adres van het toestel, server of toepassing die wordt geverifieerd op de Server en de naam van een toepassing (optioneel). Naam van de toepassing wordt weergegeven in rapporten Azure meerledige verificatie en kan worden weergegeven in SMS of Mobile App verificatieberichten.
7. Schakel het selectievakje Azure meerledige verificatie vereisen gebruiker overeenkomst in als alle gebruikers zijn of worden geïmporteerd in de Server en mutli-factor verificatie. Als u een groot aantal gebruikers nog niet zijn geïmporteerd in de Server en/of zullen worden vrijgesteld van verificatie van mutli-factor, laat u het vak uitgeschakeld. Raadpleeg de help voor meer informatie over deze functie.
8. U mag Herhaal stap 5 tot en met 7 om toe te voegen extra LDAP-clients.
9. Wanneer de Azure meerledige verificatie is geconfigureerd voor het ontvangen van LDAP-verificatie, moet deze proxy verificatie die door de LDAP-directory. Het tabblad doel geeft alleen een enkel, is daarom grijs van optie voor het gebruik van een LDAP-doel. De LDAP-directory-verbinding configureren, klikt u op het pictogram van de Directory-integratie.
10. Selecteer op het tabblad instellingen het gebruik specifieke configuratie keuzerondje LDAP.
11. Klik op bewerken... de knop.
12. In het dialoogvenster configuratie van LDAP bewerken de velden worden gevuld met de informatie die is vereist voor verbinding met de LDAP-directory. In de onderstaande tabel vindt u beschrijvingen van de velden. Opmerking: Deze informatie is ook opgenomen in de Azure meerledige verificatieserver help.![Directory-integratie](./media/multi-factor-authentication-get-started-server-ldap/ldap.png)
13. De LDAP-verbinding testen door te klikken op de knop testen.
14. Als de LDAP-verbindingstest geslaagd is, klikt u op de knop OK.
15. Klik op het tabblad Filters. De Server is vooraf geconfigureerd voor het laden van containers, beveiligingsgroepen en gebruikers vanuit Active Directory. Als de binding met een andere LDAP-directory, moet u waarschijnlijk de weergegeven filters bewerken. Klik op de koppeling Help voor meer informatie over filters.
16. Klik op de tab kenmerken. De Server is vooraf geconfigureerd voor het toewijzen van kenmerken van Active Directory.
17. Als de binding met een andere LDAP-directory of de kenmerktoewijzingen vooraf geconfigureerde te wijzigen, klikt u op bewerken... de knop.
18. Klik in het dialoogvenster kenmerken bewerken de toewijzingen van de LDAP-kenmerk voor de map te wijzigen. Namen kunnen worden getypt of geselecteerd door te klikken op de... naast elk veld.
19. Klik op de koppeling Help voor meer informatie over kenmerken.
20. Klik op OK.
21. Klik op het pictogram voor de instellingen van het bedrijf en selecteer het tabblad resolutie gebruikersnaam.
22. Als verbinding met Active Directory op een server deel uitmaakt van een domein, moet u kunnen verlaten de gebruik Windows beveiligings-id's (SID's) voor overeenkomende gebruikersnamen keuzerondje geselecteerd zijn. Selecteer anders de Gebruik LDAP-kenmerk van de unieke id voor overeenkomende gebruikersnamen keuzerondje. Wanneer ingeschakeld, probeert de Server Azure meerledige verificatie om te zetten in elke gebruikersnaam een unieke id in de LDAP-directory. Een LDAP-zoekopdracht wordt uitgevoerd op de gebruikersnaam kenmerken die in de Directory-integratie -> tabblad kenmerken. Wanneer een gebruiker wordt geverifieerd, de unieke id in de LDAP-directory de gebruikersnaam wordt opgelost en de unieke id wordt gebruikt voor het afstemmen van de gebruiker in het bestand Azure meerledige verificatie. Hiermee hoofdlettergevoelige vergelijkingen als goed als lange en korte gebruikersnaam opmaak dit voltooit de serverconfiguratie Azure meerledige verificatie. De Server luistert op de geconfigureerde poorten voor LDAP-aanvragen van de clients geconfigureerd en ingesteld op de proxy die verzoeken tot de LDAP-adreslijst voor de verificatie.


## <a name="ldap-client-configuration"></a>Configuratie van LDAP-Client

Gebruik de richtlijnen om de LDAP-client configureren:

- Configureer uw toestel, server of toepassing worden geverifieerd via LDAP naar de Azure meerledige verificatieserver alsof de LDAP-adreslijst. U dezelfde instellingen die u gewoonlijk rechtstreeks verbinden met uw LDAP-lijst, met uitzondering van de servernaam of IP-adres van de Server Azure meerledige verificatie worden gebruikt.
- 30-60 seconden de time-out voor de LDAP zodanig configureren dat er tijd is om te valideren van de referenties van de gebruiker met de LDAP-directory, het tweede factor verificatie uitvoeren, hun reactie ontvangen en vervolgens reageren op de LDAP-toegang aanvragen.
- Als u LDAPS, moet het toestel of de server de LDAP-query's zodat het SSL-certificaat geïnstalleerd op de Server Azure meerledige verificatie vertrouwt.
