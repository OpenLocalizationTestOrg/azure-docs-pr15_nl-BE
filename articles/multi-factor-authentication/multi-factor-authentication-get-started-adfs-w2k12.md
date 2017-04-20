<properties
    pageTitle="Server met Windows Server 2012 R2 AD FS MVR gesloten | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe aan de slag met Azure meerledige verificatie en AD FS in Windows Server 2012 R2."
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


# <a name="secure-your-cloud-and-on-premises-resources-using-azure-multi-factor-authentication-server-with-ad-fs-in-windows-server-2012-r2"></a>Beveiligen van uw wolk en op gebouwen bronnen met Azure meerledige verificatieserver met AD FS in Windows Server 2012 R2

Als u Active Directory Federation Services (AD FS) en beveiligde cloud of op locatie resources wilt, kunt u configureren Azure meerledige verificatieserver werkt met AD FS. Deze configuratie wordt gegenereerd uit twee stappen bestaande controle voor hoogwaardige eindpunten.

In dit artikel wordt besproken met Azure meerledige verificatieserver met AD FS in Windows Server 2012 R2. Meer informatie over het [veilige cloud en op ruimten bronnen met behulp van Azure meerledige verificatieserver met AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md)voor meer informatie.

## <a name="secure-windows-server-2012-r2-ad-fs-with-azure-multi-factor-authentication-server"></a>Windows Server 2012 R2 AD FS met Azure meerledige verificatieserver beveiligen

Wanneer u Azure meerledige verificatieserver installeert, hebt u de volgende opties:

- Azure meerledige verificatieserver lokaal op dezelfde server als AD FS installeren
- Lokaal de Azure meerledige verificatie-adapter installeren op de AD FS-server en installeer vervolgens meerledige verificatieserver op een andere computer

Voordat u begint, worden op de hoogte van de volgende informatie:

- U bent niet verplicht Azure meerledige verificatieserver installeren op de AD FS-server. U moet echter de meerledige verificatie-adapter installeren voor AD FS op een Windows Server 2012 R2 met AD FS. U kunt de server installeren op een andere computer als deze versie ondersteund wordt en u afzonderlijk de AD FS-adapter op de AD FS-federation-server installeren. Zie de volgende procedures om te leren hoe u de adapter niet afzonderlijk te installeren.

- Wanneer de Server van de MVR gesloten AD FS-adapter is ontworpen, is het verwachte dat AD FS de naam van de gebruikmakende partij zouden kunnen aan de adapter doorgeven. De partijnaam van de gebruikmakende kan vervolgens worden gebruikt als de naam van een toepassing. Maar dit bleek niet te gelden. Als uw organisatie van tekstbericht of mobiele app verificatiemethoden te gebruiken gebruikmaakt, bevatten de tekenreeksen die zijn gedefinieerd in de instellingen van het bedrijf een tijdelijke aanduiding < $$*Naam_toepassing*>. Deze aanduiding wordt niet automatisch vervangen wanneer u de AD FS-adapter. Wij raden u aan de tijdelijke aanduiding van de juiste tekenreeksen te verwijderen wanneer u AD FS beveiligde.

- De account waarmee u zich kunt aanmelden moet gebruikersrechten aan beveiligingsgroepen in uw Active Directory-service te maken hebben.

- De installatiewizard meerledige verificatie AD FS-adapter maakt een beveiligingsgroep met de naam PhoneFactor Admins in uw exemplaar van Active Directory. De AD FS-account van de federation-service wordt vervolgens toegevoegd aan deze groep. Raadzaam te controleren op de domeincontroller die de groep PhoneFactor Admins inderdaad is gemaakt en dat de AD FS-serviceaccount een lid van deze groep is. Indien nodig, moet u handmatig de AD FS-account toevoegen aan de groep PhoneFactor Admins op uw domeincontroller.

- Lees voor meer informatie over het installeren van de SDK Web Service aan de gebruiker portal over [de gebruiker portal voor Azure meerledige verificatieserver implementeren.](multi-factor-authentication-get-started-portal.md)


### <a name="install-azure-multi-factor-authentication-server-locally-on-the-ad-fs-server"></a>Azure meerledige verificatieserver lokaal installeren op de AD FS-server

1. Download en installeer Azure meerledige verificatieserver van uw AD FS-server. Lees over [aan de slag met Azure meerledige verificatieserver](multi-factor-authentication-get-started-server.md)voor informatie over de installatie.
2. Klik op het pictogram van **AD FS** in Azure meerledige verificatieserver management console en selecteert u de opties **toestaan gebruiker inschrijving** en **gebruikers toestaan om methode te selecteren**.
3. Selecteer eventueel andere opties die u wilt opgeven voor uw organisatie.
4. Klik op de **AD FS-Adapter installeert**.
<center>![Wolk](./media/multi-factor-authentication-get-started-adfs-w2k12/server.png)</center>
5. Als het Active Directory-venster wordt weergegeven, betekent dit dat twee dingen. Uw computer deel uitmaakt van een domein en de configuratie van Active Directory voor het beveiligen van communicatie tussen de AD FS-adapter en de service meerledige verificatie is onvolledig. Klik op **volgende** om automatisch voltooien van deze configuratie, of Selecteer het **overslaan van automatische configuratie van Active Directory en het configureren van instellingen handmatig** en klik vervolgens op **volgende**.
6. Als de lokale groep wordt weergegeven, betekent dit dat twee dingen. De computer is geen lid van een domein en de configuratie van de lokale groep voor het beveiligen van communicatie tussen de AD FS-adapter en de service meerledige verificatie is onvolledig. Klik op **volgende** om automatisch voltooien van deze configuratie, of Selecteer het **overslaan van automatische configuratie van de lokale groep en het configureren van instellingen handmatig** in en klik vervolgens op **volgende**.
7. Klik op **volgende**in de wizard installeren. Azure meerledige verificatieserver maakt de groep PhoneFactor Admins en de AD FS-account toegevoegd aan de groep PhoneFactor Admins.
<center>![Wolk](./media/multi-factor-authentication-get-started-adfs-w2k12/adapter.png)</center>
8. Klik op **volgende**op de pagina **Installatieprogramma starten** .
9. Klik op **volgende**in het installatieprogramma van de adapter meerledige verificatie in AD FS.
10. Klik op **sluiten** wanneer de installatie is voltooid.
11. Als de adapter is geïnstalleerd, moet u deze registreren met AD FS. Open Windows PowerShell en voer de volgende opdracht uit:<br>
    `C:\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`
   <center>![Wolk](./media/multi-factor-authentication-get-started-adfs-w2k12/pshell.png)</center>
12. De nieuw ingeschreven om adapter te gebruiken, het bewerken globale verificatie in AD FS. Ga naar het knooppunt **Beleid voor verificatie** in de AD FS-beheerconsole. Klik op de koppeling **bewerken** naast de sectie **Algemene instellingen** in de sectie **Meerledige verificatie** . **Meerledige verificatie** als een andere verificatiemethode te selecteren in het venster **Bewerken globaal beleid voor verificatie** en klik op **OK**. De adapter is geregistreerd als WindowsAzureMultiFactorAuthentication. Start de service van AD FS voor de registratie te activeren.

<center>![Wolk](./media/multi-factor-authentication-get-started-adfs-w2k12/global.png)</center>

Op dit moment is meerledige verificatieserver ingesteld als een aanvullende voorziening voor gebruik met AD FS.

## <a name="install-a-standalone-instance-of-the-ad-fs-adapter-by-using-the-web-service-sdk"></a>Een standalone-exemplaar van de AD FS-adapter installeren met behulp van de Web Service SDK
1. De Web Service SDK installeren op de server waarop meerledige verificatieserver wordt uitgevoerd.
2. Kopieer de volgende bestanden van de \Program Files\Multi-Factor Authentication Server directory op de server waarop u van plan bent om de AD FS-adapter te installeren:
  - MultiFactorAuthenticationAdfsAdapterSetup64.msi
  - Register MultiFactorAuthenticationAdfsAdapter.ps1
  - De registratie van MultiFactorAuthenticationAdfsAdapter.ps1
  - MultiFactorAuthenticationAdfsAdapter.config
3. Voer het bestand MultiFactorAuthenticationAdfsAdapterSetup64.msi-installatie.
4. In het installatieprogramma meerledige verificatie AD FS-adapter, klikt u op **volgende** om de installatie te starten.
5. Klik op **sluiten** wanneer de installatie is voltooid.

## <a name="edit-the-multifactorauthenticationadfsadapterconfig-file"></a>Bewerk het bestand MultiFactorAuthenticationAdfsAdapter.config

Ga als volgt te werk om het bestand MultiFactorAuthenticationAdfsAdapter.config te bewerken:

1. Het knooppunt **UseWebServiceSdk** ingesteld op **true**.  
2. Stel de waarde voor **WebServiceSdkUrl** naar de URL van de SDK meerledige verificatie Web Service. Bijvoorbeeld: *https://contoso.com/&lt;certificatename&gt;/MultiFactorAuthWebServicesSdk/PfWsSdk.asmx*, waarbij certificatename de naam van uw certificaat.  
3. Bewerk het Register MultiFactorAuthenticationAdfsAdapter.ps1-script door toe te voegen *- ConfigurationFilePath &lt;pad&gt; * aan het einde van de `Register-AdfsAuthenticationProvider` opdracht, waar * &lt;pad&gt; * is het volledige pad naar het bestand MultiFactorAuthenticationAdfsAdapter.config.

### <a name="configure-the-web-service-sdk-with-a-username-and-password"></a>De SDK Web Service configureren met een gebruikersnaam en wachtwoord

Er zijn twee opties voor het configureren van de SDK Web Service. De eerste is met een gebruikersnaam en wachtwoord, de tweede met een clientcertificaat. Volg deze stappen voor de eerste optie of voor het tweede overslaan.  

1. Stel de waarde voor **WebServiceSdkUsername** op een account die lid is van de beveiligingsgroep PhoneFactor Admins. Gebruik de &lt;domein&gt;& #92; &lt;gebruikersnaam&gt; indeling.  
2. Stel de waarde voor **WebServiceSdkPassword** op het juiste wachtwoord.

### <a name="configure-the-web-service-sdk-with-a-client-certificate"></a>De SDK Web Service configureren met een clientcertificaat.

Als u niet dat een gebruikersnaam en wachtwoord te gebruiken wilt, volgt u de SDK Web Service configureren met een clientcertificaat.

1. Een clientcertificaat aanvragen bij een certificate authority voor de server waarop de Web Service SDK. Meer informatie over het [verkrijgen van clientcertificaten](https://technet.microsoft.com/library/cc770328.aspx).  
2. Dit certificaat importeren in het persoonlijke certificaatarchief lokale computer van de server waarop de Web Service SDK. Controleer of de certificeringsinstantie een certificaat met openbare archief met certificaten van vertrouwde basiscertificeringsinstanties.  
3. De openbare en persoonlijke sleutels van het clientcertificaat exporteren naar een .pfx-bestand.  
4. De openbare sleutel in Base64-indeling exporteren naar een cer-bestand.  
5. In Serverbeheer of de functie voor verificatie van clientcertificaattoewijzingen \Web Server\Security\IIS Web Server (IIS) is geïnstalleerd. Als niet is geïnstalleerd, selecteert u **functies toevoegen en onderdelen die** deze functie toe te voegen.  
6. In IIS-beheer, dubbelklik op **Systeemconfiguratie-Editor** in de website die de virtuele map Web Service SDK bevat. Het is belangrijk om dit te doen op het niveau van de website en niet op het niveau van de virtuele map.  
7. Ga naar de sectie **system.webServer/security/authentication/iisClientCertificateMappingAuthentication** .  
8. Instellen op **true**worden ingeschakeld.  
9. OneToOneCertificateMappingsEnabled ingesteld op **true**.  
10. Klik op de knop **...** naast oneToOneMappings en klik op de koppeling **toevoegen** .  
11. Open het Base64 cer-bestand die u eerder hebt geëxporteerd. *---Beginnen certificaat---* *---END CERTIFICATE---*en alle regeleinden verwijderd. Kopieer de resulterende tekenreeks.  
12. Certificaat aan de tekenreeks die in de voorgaande stap hebt gekopieerd.  
13. Instellen op **true**worden ingeschakeld.  
14. Een account die lid is van de beveiligingsgroep PhoneFactor Admins gebruikersnaam ingesteld. Gebruik de &lt;domein&gt;& #92; &lt;gebruikersnaam&gt; indeling.  
15. Het juiste wachtwoord-wachtwoord instellen en sluit de systeemconfiguratie-Editor.  
16. Klik op **toepassen** .  
17. Dubbelklik in de virtuele map Web Service SDK op **verificatie**.  
18. Controleer of u ASP.NET-imitatie en basisverificatie worden ingesteld op **ingeschakeld**en dat alle andere items zijn ingesteld op **uitgeschakeld**.  
19. Dubbelklik in de virtuele map Web Service SDK op **SSL-instellingen**.  
20. Instellen van clientcertificaten op **accepteren**en klik vervolgens op **toepassen**.  
21. Kopieer het pfx-bestand dat u eerder hebt geëxporteerd naar de server waarop de AD FS-adapter.  
22. Het .pfx-bestand importeren in het persoonlijke certificaatarchief van de lokale computer.  
23. Met de rechtermuisknop op **Persoonlijke sleutels beheren**, en selecteer vervolgens alleen toegang verlenen tot het account waarmee u zich kunt aanmelden bij de service van AD FS.  
24. Open het clientcertificaat en kopieer de vingerafdruk van het tabblad **Details** .  
25. Stel in het bestand MultiFactorAuthenticationAdfsAdapter.config **WebServiceSdkCertificateThumbprint** aan de tekenreeks die in de vorige stap hebt gekopieerd.  


Ten slotte de adapter te, voeren de \Program Files\Multi-Factor verificatie-Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1-script in PowerShell. De adapter is geregistreerd als WindowsAzureMultiFactorAuthentication. Start de service van AD FS voor de registratie te activeren.

## <a name="related-topics"></a>Verwante onderwerpen

Zie de [Veelgestelde vragen Azure meerledige verificatie](multi-factor-authentication-faq.md) voor het oplossen van problemen
