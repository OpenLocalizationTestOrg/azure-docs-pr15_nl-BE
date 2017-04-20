<properties
    pageTitle="Azure MVR gesloten Server gebruiken met AD FS 2.0 | Microsoft Azure"
    description="Dit is de pagina Azure meerledige verificatie die wordt beschreven hoe u aan de slag met Azure MVR gesloten en AD FS 2.0."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>

# <a name="secure-cloud-and-on-premises-resources-using-azure-multi-factor-authentication-server-with-ad-fs-20"></a>Cloud en op ruimten bronnen met Azure meerledige verificatieserver met AD FS 2.0 beveiligen

Dit artikel is voor organisaties die met Azure Active Directory zijn federatieve en wilt beveiligen van bronnen die zich in ruimten of in de cloud. Bescherm uw resources door de Server Azure meerledige verificatie en het werken met AD FS, zodat verificatie in twee stappen wordt geactiveerd voor hoge waarde eindpunten configureren.

Deze documentatie wordt beschreven met behulp van de Server Azure meerledige verificatie met AD FS 2.0.  Meer informatie krijgen over [beveiligen wolk en op ruimten bronnen met Azure meerledige verificatieserver met Windows Server 2012 R2 AD FS](multi-factor-authentication-get-started-adfs-w2k12.md).


## <a name="secure-ad-fs-20-with-a-proxy"></a>AD FS 2.0 beveiligen met een proxy
Beveiliging van AD FS-2.0 met een proxy, Azure meerledige verificatieserver installeren op ADFS-proxyserver en de Server configureren.

### <a name="configure-iis-authentication"></a>IIS-verificatie configureren

1. Binnen de Azure meerledige verificatieserver, klikt u op het pictogram van de **IIS-verificatie** in het linkermenu.
2. Klik op het tabblad voor het **Formulier is gebaseerd** .
3. Klik op de **toevoegen...** de knop.
<center>![Setup](./media/multi-factor-authentication-get-started-adfs-adfs2/setup1.png)</center>
4. Gebruikersnaam, wachtwoord en domein variabelen niet automatisch herkent, voert u de aanmeldings-URL (zoals https://sso.contoso.com/adfs/ls) in het dialoogvenster Auto-Configure Form-Based Website en klik op OK.
5. Schakel het selectievakje **gebruiker Azure meerledige verificatie vereisen overeenkomt met** als alle gebruikers zijn of worden geïmporteerd naar de Server en na verificatie in twee stappen. Als u een groot aantal gebruikers nog niet zijn geïmporteerd in de Server en/of zullen worden vrijgesteld van verificatie in twee stappen, laat u het vak uitgeschakeld. Zie het help-bestand voor meer informatie over deze functie.
6. Als de pagina variabelen kunnen niet automatisch worden gedetecteerd, klikt u op de **Opgegeven handmatig...** knop in het dialoogvenster Website Auto-Configure Form-Based.
7. Voer de URL naar de aanmeldingspagina van AD FS in het veld URL verzenden (zoals https://sso.contoso.com/adfs/ls) en voert u de naam van een toepassing (optioneel) in het dialoogvenster Website Add Form-Based. Naam van de toepassing wordt weergegeven in rapporten Azure meerledige verificatie en kan worden weergegeven in SMS of Mobile App verificatieberichten. Zie het help-bestand voor meer informatie over de URL verzenden.
8. Instellen van de indeling van het verzoek aan "Boeken of GET."
9. Voer de gebruikersnaam (ctl00 ContentPlaceHolder1$ $UsernameTextBox) en wachtwoord variabele (ctl00 ContentPlaceHolder1$ $PasswordTextBox). Als uw formulier gebaseerd aanmeldingspagina domein textbox wordt weergegeven, voert u de domein-variabele. Wellicht moet u Ga naar de login pagina in een webbrowser, klik met de rechtermuisknop op de pagina en selecteer **Bron weergeven** om te zoeken naar de namen van de invoervakken binnen de aanmeldingspagina.
10. Schakel het selectievakje **gebruiker Azure meerledige verificatie vereisen overeenkomt met** als alle gebruikers zijn of worden geïmporteerd naar de Server en na verificatie in twee stappen. Als u een groot aantal gebruikers nog niet zijn geïmporteerd in de Server en/of zullen worden vrijgesteld van verificatie in twee stappen, laat u het vak uitgeschakeld.
<center>![Setup](./media/multi-factor-authentication-get-started-adfs-adfs2/manual.png)</center>
11. Klik op de **Geavanceerde...** de knop Geavanceerde instellingen te controleren. Kunt u instellingen met inbegrip van de mogelijkheid om een denial-of aangepaste pagina-bestand, selecteer cache succesvolle verificaties met de website gebruik van cookies en om te selecteren hoe de primaire referenties verifiëren.
12. Aangezien de ADFS-proxyserver niet kunnen worden toegevoegd aan het domein, kunt u LDAP verbinding maken met de domeincontroller voor het importeren van de gebruiker en vooraf-verificatie. Klik op het tabblad **Verificatie primaire** en **LDAP Bind** te selecteren voor het type verificatie vooraf-verificatie in het dialoogvenster Website Advanced Form-Based.
13. Als alles klaar is, klikt u op de knop **OK** om terug te keren naar het dialoogvenster Website Add Form-Based. Raadpleeg de help voor meer informatie over geavanceerde instellingen.
14. Klik op de knop **OK** om het dialoogvenster te sluiten.
15. Zodra de pagina en de URL-variabelen zijn ontdekt of ingevoerd, is de websitegegevens worden weergegeven in het deelvenster op basis van een formulier.
16. Klik op het tabblad **Systeemeigen Module** en selecteer de server, de website die de AD FS-proxy wordt uitgevoerd onder (zoals "Standaardwebsite") of het ADFS proxy-toepassing (zoals "ls" onder 'adfs') om de invoegtoepassing IIS op het gewenste niveau.
17. Klik in het vak **verificatie van IIS inschakelen** op de bovenkant van het scherm.
18. De IIS-verificatie is nu ingeschakeld.

### <a name="configure-directory-integration"></a>Directory-integratie configureren

U IIS-verificatie ingeschakeld, maar voor het uitvoeren van de vooraf op uw Active Directory (AD) via LDAP-verificatie moet u de LDAP-verbinding met de domeincontroller.

1. Klik op het pictogram van de **Directory-integratie** .
2. Selecteer het keuzerondje **specifiek LDAP-configuratie gebruiken** op het tabblad instellingen.
<center>![Setup](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap1.png)</center>
3. Klik op de **bewerken...** de knop.
4. Klik in het dialoogvenster configuratie van LDAP bewerken de velden worden gevuld met de informatie die is vereist voor verbinding met de domeincontroller AD. In de onderstaande tabel vindt u beschrijvingen van de velden. Deze informatie is ook opgenomen in de Azure meerledige verificatieserver help.
5. De LDAP-verbinding testen door te klikken op de knop **testen** .
<center>![Setup](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap2.png)</center>
6. Als de LDAP-verbindingstest geslaagd is, klikt u op de knop **OK** .

### <a name="configure-company-settings"></a>Bedrijfsinstellingen configureren

1. Vervolgens klikt u op het pictogram voor de **Instellingen van het bedrijf** en selecteer het tabblad **Resolutie gebruikersnaam** .
2. Selecteer het keuzerondje **Gebruik LDAP-kenmerk van de unieke id voor de overeenkomende gebruikersnamen** .
3. Als gebruikers hun gebruikersnaam in de notatie 'domein\gebruikersnaam' invoert, moet de Server kunnen het domein uit de gebruikersnaam verwijderen wanneer de LDAP-query wordt gemaakt. Dat kan worden gedaan door een registerinstelling.
4. Open de Register-editor en Ga naar HKEY_LOCAL_MACHINE/SOFTWARE/Wow6432Node/positief netwerken/PhoneFactor op een 64-bits-server. Als op een 32-bits-server "Wow6432Node" uit het pad te nemen. Maak een DWORD-registersleutel genaamd 'UsernameCxz_stripPrefixDomain' en de waarde ingesteld op 1. Azure meerledige verificatie is nu de beveiliging van de AD FS-proxy.

Zorg ervoor dat gebruikers zijn geïmporteerd uit Active Directory op de Server. Zie de [sectie vertrouwde IP -adressen](#trusted-ips) als u wilt het interne IP-adressen "witte" lijst zodat verificatie in twee stappen niet vereist is wanneer het aanmelden bij de website van deze locaties.

<center>![Setup](./media/multi-factor-authentication-get-started-adfs-adfs2/reg.png)</center>

## <a name="ad-fs-20-direct-without-a-proxy"></a>AD FS 2.0 rechtstreeks zonder proxy

AD FS als de AD FS-proxy wordt gebruikt, kunt u beveiligen. Azure meerledige verificatieserver installeren op de AD FS-server en configureer de Server per de volgende stappen uit:

1. Binnen de Azure meerledige verificatieserver, klikt u op het pictogram van de **IIS-verificatie** in het linkermenu.
2. Klik op het tabblad **HTTP** .
3. Klik op de **toevoegen...** de knop.
4. Voer de URL voor de website van ADFS waar HTTP-verificatie wordt uitgevoerd (zoals https://sso.domain.com/adfs/ls/auth/integrated) in het veld voor de basis-URL in het dialoogvenster van de basis-URL toevoegen. Voer vervolgens de naam van een toepassing (optioneel). Naam van de toepassing wordt weergegeven in rapporten Azure meerledige verificatie en kan worden weergegeven in SMS of Mobile App verificatieberichten.
5. Indien gewenst, aanpassen van de time-out voor inactiviteit en de maximale sessieduur.
6. Schakel het selectievakje **gebruiker Azure meerledige verificatie vereisen overeenkomt met** als alle gebruikers zijn of worden geïmporteerd naar de Server en na verificatie in twee stappen. Als u een groot aantal gebruikers nog niet zijn geïmporteerd in de Server en/of zullen worden vrijgesteld van verificatie in twee stappen, laat u het vak uitgeschakeld. Zie het help-bestand voor meer informatie over deze functie.
7. Het selectievakje cookie cache indien gewenst.
<center>![Setup](./media/multi-factor-authentication-get-started-adfs-adfs2/noproxy.png)</center>
8. Klik op de knop **OK** .
9. Klik op het tabblad **Systeemeigen Module** en selecteer de server, de website (zoals "Standaardwebsite") of het AD FS-toepassing (zoals "ls" onder "adfs") voor het inschakelen van de invoegtoepassing IIS op het gewenste niveau.
10. Klik in het vak **verificatie van IIS inschakelen** op de bovenkant van het scherm. AD FS is nu de beveiliging van Azure meerledige verificatie.

Zorg ervoor dat gebruikers zijn geïmporteerd uit Active Directory op de Server. Zie de sectie vertrouwde IP-adressen als u wilt het interne IP-adressen "witte" lijst zodat verificatie in twee stappen niet vereist is wanneer het aanmelden bij de website van deze locaties.


## <a name="trusted-ips"></a>Vertrouwde IP-adressen
Vertrouwde IP-adressen kunnen gebruikers Azure meerledige verificatie overslaan voor website-aanvragen die afkomstig zijn van bepaalde IP-adressen of subnetten. Bijvoorbeeld, kunt u voor gebruikers met vrijstelling van verificatie in twee stappen wanneer zij zich bij het Bureau aanmelden. Zou u het subnet office opgeven als een vertrouwde IP-adressen.

### <a name="to-configure-trusted-ips"></a>Vertrouwde IP-adressen configureren


1. Klik op het tabblad **Vertrouwde IP -adressen** in de sectie IIS-verificatie.
1. Klik op de **toevoegen...** de knop.
1. Wanneer het dialoogvenster vertrouwde IPs toevoegen wordt weergegeven, selecteert u een van de keuzerondjes **Één IP**of **bereik IP-** **Subnet** .
1. Voer het IP-adres, bereik van IP-adressen of subnet die whitelisted moet worden. Als een subnet opgeven, selecteert u de juiste Netmask en klik op de knop **OK** . De vertrouwde IP is nu toegevoegd.


<center>![Setup](./media/multi-factor-authentication-get-started-adfs-adfs2/trusted.png)</center>
