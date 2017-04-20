<properties 
    pageTitle="IIS-verificatie en Azure meerledige verificatieserver"
    description="Dit is de pagina Azure meerledige verificatie die helpt bij het implementeren van IIS-verificatie en Azure meerledige verificatieserver."
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

# <a name="iis-authentication"></a>IIS-verificatie

De sectie IIS-verificatie van de Server Azure meerledige verificatie kunt u inschakelen en configureren van IIS-verificatie voor integratie met Microsoft IIS-webtoepassingen. De Azure meerledige verificatieserver installeert een invoegtoepassing die wordt gemaakt met de IIS-webserver toevoegen Azure meerledige verificatie aanvragen kunt filteren. De invoegtoepassing IIS biedt ondersteuning voor formulieren gebaseerde verificatie en geïntegreerde Windows-verificatie voor HTTP. Vertrouwde dat IP-adressen kan ook worden geconfigureerd voor interne IP-adressen worden vrijgesteld van verificatie met twee factoren.


![IIS-verificatie](./media/multi-factor-authentication-get-started-server-iis/iis.png)


## <a name="using-form-based-iis-authentication-with-azure-multi-factor-authentication-server"></a>Formulieren gebaseerde IIS-verificatie gebruiken met Azure meerledige verificatieserver

Een IIS-webtoepassing die gebruikmaakt van verificatie op basis van een formulier beveiligde, Azure meerledige verificatieserver installeren op de IIS-webserver en configureer de Server per in de volgende procedure.

1. Binnen de Azure meerledige verificatieserver klikt u op het pictogram van de IIS-verificatie in het linkermenu.
2. Klik op het tabblad voor het formulier is gebaseerd.
3. Klik op toevoegen... de knop.
4. Detecteren gebruikersnaam, wachtwoord en domein variabelen automatisch, de aanmeldings-URL (bijv. https://localhost/contoso/auth/login.aspx) invoeren in het dialoogvenster Auto-Configure Form-Based Website en klik op OK.
5. Schakel het selectievakje meerledige verificatie vereisen gebruiker overeenkomst in als alle gebruikers zijn of worden geïmporteerd in de Server en meerledige verificatie. Als u een groot aantal gebruikers nog niet zijn geïmporteerd in de Server en/of zullen worden vrijgesteld van meerledige verificatie, laat u het vak uitgeschakeld.
6. Als de pagina variabelen kunnen niet automatisch worden gedetecteerd, klikt u op de opgegeven handmatig... knop in het dialoogvenster Website Auto-Configure Form-Based.
7. Voer de URL naar de aanmeldingspagina in het veld URL verzenden en voert u de naam van een toepassing (optioneel) in het dialoogvenster Website Add Form-Based. Naam van de toepassing wordt weergegeven in rapporten Azure meerledige verificatie en kan worden weergegeven in SMS of Mobile App verificatieberichten. Zie het help-bestand voor meer informatie over de URL verzenden.
8. Selecteer de juiste indeling. Dit is ingesteld op "POST of GET" voor de meeste webtoepassingen.
9. Geef de variabele Username, wachtwoord en domein variabele (als deze op de aanmeldingspagina wordt weergegeven). Wellicht moet u Ga naar de login pagina in een webbrowser, klik met de rechtermuisknop op de pagina en selecteer 'Bron weergeven' om te zoeken naar de namen van de invoervakken binnen de pagina.
10. Schakel het selectievakje Azure meerledige verificatie vereisen gebruiker overeenkomst in als alle gebruikers zijn of worden geïmporteerd in de Server en meerledige verificatie. Als u een groot aantal gebruikers nog niet zijn geïmporteerd in de Server en/of zullen worden vrijgesteld van meerledige verificatie, laat u het vak uitgeschakeld. Raadpleeg de help voor meer informatie over deze functie.
11.  Klik op de geavanceerde... de knop Geavanceerde instellingen, met inbegrip van de mogelijkheid om een denial-of aangepaste pagina-bestand, selecteer succesvolle verificaties met de website in de cache voor een periode met behulp van cookies en selecteren of verificatie de primaire referenties op basis van het Windows-domein, een LDAP-adreslijst of een RADIUS-server te controleren. Klik op OK om terug te keren naar het dialoogvenster Website Add Form-Based als voltooid. Raadpleeg de help voor meer informatie over geavanceerde instellingen.
12. Klik op OK.
13. Zodra de pagina en de URL-variabelen zijn ontdekt of ingevoerd, weergegeven de websitegegevens in het deelvenster op basis van het formulier.
14. Zie de IIS inschakelen Plug-ins voor Azure meerledige verificatieserver hieronder rechtstreeks voor het voltooien van de configuratie van IIS-verificatie.

## <a name="using-integrated-windows-authentication-with-azure-multi-factor-authentication-server"></a>Met behulp van geïntegreerde Windows-verificatie met Azure meerledige verificatieserver

Azure meerledige verificatieserver installeren op de IIS-webserver te beveiligen van een IIS-webtoepassing die geïntegreerde Windows HTTP-verificatie gebruikt, en configureer de Server per in de volgende procedure.

1. Binnen de Azure meerledige verificatieserver klikt u op het pictogram van de IIS-verificatie in het linkermenu.
2. Klik op het tabblad HTTP. Klik op het tabblad voor het formulier is gebaseerd.
3. Klik op toevoegen... de knop.
4. Voer de URL voor de website waarop HTTP-verificatie wordt uitgevoerd (bijvoorbeeld http://localhost/owa) in het veld voor de basis-URL in het dialoogvenster Base URL toevoegen en voert een naam (optioneel). Naam van de toepassing wordt weergegeven in rapporten Azure meerledige verificatie en kan worden weergegeven in SMS of Mobile App verificatieberichten.
5. Pas de time-out voor inactiviteit en maximale sessieduur als de standaard niet voldoende is.
6. Schakel het selectievakje meerledige verificatie vereisen gebruiker overeenkomst in als alle gebruikers zijn of worden geïmporteerd in de Server en meerledige verificatie. Als u een groot aantal gebruikers nog niet zijn geïmporteerd in de Server en/of zullen worden vrijgesteld van meerledige verificatie, laat u het vak uitgeschakeld. Raadpleeg de help voor meer informatie over deze functie.
7. Het selectievakje cookie cache indien gewenst.
8. Klik op OK.
9. Zie de sectie [IIS inschakelen Plug-ins voor Azure meerledige verificatieserver](#enable-iis-plug-ins-for-azure-multi-factor-authentication-server) direct onder de configuratie van IIS-verificatie te voltooien.


## <a name="enable-iis-plug-ins-for-azure-multi-factor-authentication-server"></a>IIS-Plug-ins inschakelen voor Azure meerledige verificatieserver

Wanneer u het formulier op basis van hebt geconfigureerd of HTTP-verificatie-URL's en instellingen, moet u de locaties waar de Azure meerledige verificatie IIS plug-ins moet worden geladen en ingeschakeld in IIS. Gebruik de volgende procedure:

1. Als in IIS 6 wordt uitgevoerd, klikt u op het tabblad ISAPI- en selecteert u de website waarop de webtoepassing wordt onder (bijvoorbeeld standaardwebsite) als de Azure meerledige verificatie-ISAPI-filter die voor die site invoegtoepassing wilt inschakelen.
2. Als u in IIS 7 of hoger, klikt u op het tabblad systeemeigen Module en selecteer de server, website (s) of toepassingen inschakelen van de invoegtoepassing IIS op het gewenste coderingsniveau.
3. Klik op de Schakel IIS-verificatie aan de bovenkant van het scherm. Azure meerledige verificatie is nu de beveiliging van de geselecteerde IIS-toepassing. Zorg ervoor dat gebruikers in de Server zijn geïmporteerd. Zie het gedeelte vertrouwde IP-adressen als u wilt "witte" lijst die interne IP-adressen die verificatie met twee factoren en is dus niet vereist bij het aanmelden bij de website van deze locaties.


## <a name="trusted-ips"></a>Vertrouwde IP-adressen

De vertrouwde IP-adressen kunnen gebruikers Azure meerledige verificatie overslaan voor website-aanvragen die afkomstig zijn van bepaalde IP-adressen of subnetten. Bijvoorbeeld, kunt u vrijgesteld gebruikers van Azure meerledige verificatie bij het aanmelden van het kantoor. Voor deze, zou u het subnet office als vertrouwde IP-adressen opgeven. Vertrouwde IP-adressen gebruik configureren de volgende procedure:

1. Klik op het tabblad vertrouwde IP-adressen in de sectie IIS-verificatie.
2. Klik op toevoegen... de knop.
3. Wanneer het dialoogvenster vertrouwde IPs toevoegen wordt weergegeven, selecteer de één IP-, bereik van IP-Subnet keuzerondje.
4. Geef het IP-adres, bereik van IP-adressen of subnet die whitelisted moet worden. Als een subnet opgeven, selecteert u de juiste Netmask en klik op OK. De "witte" lijst is nu toegevoegd.
