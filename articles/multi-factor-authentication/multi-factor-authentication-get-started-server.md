<properties 
    pageTitle="Aan de slag met de Azure meerledige verificatieserver"
    description="Dit is de pagina Azure meerledige verificatie die wordt beschreven hoe u aan de slag met Azure MVR gesloten Server."
    services="multi-factor-authentication"
    keywords="verificatie, azure multi factor authentication app activeringspagina server authentication server downloaden"
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

# <a name="getting-started-with-the-azure-multi-factor-authentication-server"></a>Aan de slag met de Azure meerledige verificatieserver




<center>![Wolk](./media/multi-factor-authentication-get-started-server/server2.png)</center>

Nu dat we hebben of te gebruiken op-premises meerledige verificatie bepaald, krijgen we te gaan. Deze pagina heeft betrekking op een nieuwe installatie van de server en het verkrijgen van setup met Active Directory voor het bedrijf. Als u al de PhoneFactor-server geïnstalleerd en zijn op zoek te upgraden Zie [upgraden naar de Server Azure meerledige](multi-factor-authentication-get-started-server-upgrade.md) of zoekt u naar informatie over het installeren van de web-service Zie [Azure meerledige verificatie Server Mobile App Web Service](multi-factor-authentication-get-started-server-webservice.md).


## <a name="download-the-azure-multi-factor-authentication-server"></a>Download de Azure meerledige verificatieserver



Er zijn twee verschillende manieren dat u de Azure meerledige verificatieserver kunt downloaden. Beide worden gedaan via de portal Azure. De eerste is door de Provider meerledige verificatie rechtstreeks beheren. De tweede is via de service-instellingen. De tweede optie is een meerledige verificatie-Provider of een Azure MVR gesloten, Azure AD Premium of Enterprise Mobility Suite-licentie vereist.


### <a name="to-download-the-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Downloaden van de server Azure meerledige verificatie van de Azure portal
--------------------------------------------------------------------------------

1. De Azure Portal als beheerder aanmelden.
2. Selecteer Active Directory aan de linkerkant.
3. Klik op de pagina Active Directory boven **Meerledige verificatie Providers**
4. Klik onder **beheren**
5. Hiermee opent u een nieuwe pagina.  Klik op **Downloads.** 
 ![Downloaden](./media/multi-factor-authentication-sdk/download.png)
6. Boven **Genereren activering referenties**op **downloaden.** 
 ![Downloaden](./media/multi-factor-authentication-get-started-server/download4.png)
7. Sla de download.



### <a name="to-download-the-azure-multi-factor-authentication-server-via-the-service-settings"></a>Downloaden van de Server Azure meerledige verificatie via de service-instellingen


1. De Azure Portal als beheerder aanmelden.
2. Selecteer Active Directory aan de linkerkant.
3. Dubbelklik vervolgens op uw exemplaar van AD Azure.
4. Klik op **configureren**boven
![downloaden](./media/multi-factor-authentication-sdk/download2.png)
5. Selecteer onder meerledige verificatie **service-instellingen beheren**
6. Op de instellingenpagina van services onder aan het scherm klikt u op **de portal**.
![Downloaden](./media/multi-factor-authentication-get-started-server/servicesettings.png)
7. Hiermee opent u een nieuwe pagina. Klik op **Downloads.**
8. Boven **Genereren activering referenties**op **downloaden.**
9. Sla de download.




## <a name="install-and-configure-the-azure-multi-factor-authentication-server"></a>Installeer en configureer de Server Azure meerledige verificatie
U kunt nu dat u de server hebt gedownload installeren en configureren.  Zorg ervoor dat de server die u wilt installeren op de volgende vereisten voldoet:



Serververeisten meerledige verificatie mogelijk Azure|Beschrijving|
:------------- | :------------- |
Hardware|<li>200 MB ruimte op vaste schijf</li><li>kan x32 of x64-processor</li><li>1 GB of meer RAM-geheugen</li>
Software|<li>Windows Server 2008 of hoger als de host een besturingssysteem van de server is</li><li>Windows 7 of hoger als de host een client-OS is</li><li>4.0 van Microsoft .NET Framework</li><li>SDK van IIS 7.0 of hoger als de user portal of web-service installeren</li>

### <a name="azure-multi-factor-authentication-server-firewall-requirements"></a>Azure meerledige verificatie firewall serververeisten
--------------------------------------------------------------------------------
Elke server MVR gesloten moet kunnen communiceren op poort 443 uitgaande is het volgende:

- https://PFD.phonefactor.NET
- https://pfd2.phonefactor.NET
- https://CSS.phonefactor.NET

Als uitgaande firewalls op poort 443 beperkt zijn, moeten de volgende IP-adresbereiken worden geopend:

IP-Subnet|Netmask|IP-bereik
:------------- | :------------- | :------------- |
134.170.116.0/25|255.255.255.128|134.170.116.1 – 134.170.116.126
134.170.165.0/25|255.255.255.128|134.170.165.1 – 134.170.165.126
70.37.154.128/25|255.255.255.128|70.37.154.129 – 70.37.154.254

Als u geen van functies voor Azure meerledige verificatie gebeurtenis bevestiging gebruikmaakt en als gebruikers niet kunnen verifiëren met de mobiele apps meerledige verificatie van apparaten in het bedrijfsnetwerk van de IP bereiken worden verkleind met het volgende:


IP-Subnet|Netmask|IP-bereik
:------------- | :------------- | :------------- |
134.170.116.72/29|255.255.255.248|134.170.116.72 – 134.170.116.79
134.170.165.72/29|255.255.255.248|134.170.165.72 – 134.170.165.79
70.37.154.200/29|255.255.255.248|70.37.154.201 – 70.37.154.206


### <a name="to-install-and-configure-the-azure-multi-factor-authentication-server"></a>Installeer en configureer de server Azure meerledige verificatie
--------------------------------------------------------------------------------


1. Dubbelklik op het uitvoerbare bestand. Dit zal de installatie te starten.
2. Zorg ervoor dat de map juist is op het scherm installatiemap selecteren en klik op volgende.
3. Zodra de installatie is voltooid, klikt u op voltooien.  Hierdoor wordt de configuratiewizard gestart.
4. De configuratiewizard Welkom-scherm, schakel dit selectievakje in **overslaan met behulp van de Wizard Configuratie van verificatie** en klik op **volgende**.  Hiermee sluit u de wizard en start de server.
    ![Wolk](./media/multi-factor-authentication-get-started-server/skip2.png)
5. Klik op de knop **Genereren activering referenties** terug op de pagina die we gedownload van de server uit.  Deze gegevens kopiëren naar de Server van de MVR gesloten Azure in de daartoe bestemde vakken en klik op **activeren**.


De bovenstaande stappen weergeven een snelle installatie met de configuratiewizard.  U kunt de verificatiewizard opnieuw uitvoeren door deze te selecteren in het menu Extra op de server.



##<a name="import-users-from-active-directory"></a>Gebruikers importeren vanuit Active Directory

Nu dat de server is geïnstalleerd en geconfigureerd, kunt u snel gebruikers importeren in de MVR gesloten Azure Server.

### <a name="to-import-users-from-active-directory"></a>Om gebruikers te importeren vanuit Active Directory
--------------------------------------------------------------------------------


1. Selecteer **gebruikers**in Azure MVR gesloten Server, aan de linkerkant.
2. Selecteer onder **importeren vanuit Active Directory**.
3. Nu kunt u zoeken naar afzonderlijke gebruikers of de directory AD voor organisatie-eenheden zoeken met andere gebruikers in deze.  In dit geval zullen we de gebruikers van de organisatie-eenheid opgeven.
4. Alle gebruikers die aan de rechterkant en klik op **importeren**.  Er wordt een pop-upvenster waarin u succesvol was.  Sluit het venster importeren.

![Wolk](./media/multi-factor-authentication-get-started-server/import2.png)

## <a name="send-users-an-email"></a>Gebruikers een e-mailbericht verzenden
Nu dat u uw gebruikers hebt geïmporteerd in de server Azure meerledige verificatie, wordt het aanbevolen dat u uw gebruikers een e-mail sturen die meedeelt dat zij zijn ingeschreven bij een meerledige verificatie.

Er zijn verschillende manieren om uw gebruikers configureren voor het gebruik van meerledige verificatie met de Server Azure meerledige verificatie.  Bijvoorbeeld, als u weet dat de gebruikers telefoonnummers of konden de telefoonnummers in de Server Azure meerledige verificatie uit van hun bedrijf directory importeren, kunt het e-mailbericht gebruikers laten weten dat ze zijn geconfigureerd voor Azure meerledige verificatie gebruiken, bieden sommige instructies over het gebruik van Azure meerledige verificatie en de gebruiker van het telefoonnummer krijgen hun verificaties op de hoogte.  

De inhoud van het e-mailbericht is afhankelijk van de methode van verificatie die is ingesteld voor de gebruiker (zoals telefoongesprek, SMS, mobiele app).  Bijvoorbeeld als de gebruiker een PINCODE gebruiken moet wanneer ze worden geverifieerd, ziet het e-mailbericht ze wat hun eerste PIN is ingesteld op.  Gebruikers zijn meestal hun PINCODE wijzigen tijdens hun eerste verificatie vereist.

Als de gebruikers telefoonnummers niet geconfigureerd of geïmporteerd in de Server Azure meerledige verificatie of gebruikers vooraf geconfigureerd zijn met de mobiele app gebruiken voor verificatie, kunt u ze een e-mailbericht waarmee ze weten dat ze zijn geconfigureerd voor het gebruik van Azure meerledige verificatie en hen leidt voor het voltooien van hun inschrijving rekening via de Portal Azure meerledige verificatie gebruiker verzenden.  Een hyperlink worden opgenomen dat de gebruiker toegang te krijgen tot de User Portal op klikt. Wanneer de gebruiker op de hyperlink klikt, wordt de webbrowser openen en duren van hun bedrijf Azure meerledige verificatie User Portal.   


### <a name="configuring-email-and-email-templates"></a>E-mailadres en e-mailsjablonen configureren

U kunt de instellingen voor het verzenden van deze e-mailberichten instellen door te klikken op het pictogram e-mail aan de linkerkant.  Dit is waar kunt u de SMTP-gegevens van uw e-mailserver en u kunt een raamcontract breed per e-mail door een cheque te verzenden mails naar gebruikers verzenden.

![E-mailinstellingen](./media/multi-factor-authentication-get-started-server/email1.png)

Op het tabblad e-inhoud ziet u alle van de verschillende e-mailsjablonen die beschikbaar zijn om uit te kiezen.  Dus afhankelijk van hoe u uw gebruikers gebruik meerledige verificatie hebt geconfigureerd, kunt u de sjabloon die het beste past bij u.

![E-mailsjablonen](./media/multi-factor-authentication-get-started-server/email2.png)

## <a name="how-the-azure-multi-factor-authentication-server-handles-user-data"></a>Hoe de Azure meerledige verificatieserver omgaat met gegevens van de gebruiker

Wanneer u de Server (MVR gesloten meerledige verificatie) van gebouwen, worden gegevens van een gebruiker wordt opgeslagen in de servers op gebouwen. Geen permanente gebruikersgegevens worden opgeslagen in de cloud. Wanneer de gebruiker een verificatie met twee factoren, verzendt de MVR gesloten Server gegevens naar de MVR gesloten Azure cloud-service de verificatie uitvoert. Wanneer deze verificatie-aanvragen worden verzonden naar de cloud-service, worden de volgende velden in de aanvraag en logboeken verzonden zodat ze beschikbaar in rapporten over verificatie/gebruik van de klant zijn. Sommige velden zijn optioneel kan worden ingeschakeld of uitgeschakeld in de Server meerledige verificatie. De mededeling van de MVR gesloten Server aan de MVR gesloten cloud-service maakt gebruik van SSL/TLS via poort 443 uitgaande. Deze velden zijn:

- Unieke ID - gebruikersnaam of ID van interne MVR gesloten
- Eerste en laatste naam - optioneel
- E-mailadres - optioneel
- Telefoonnummer - bij het uitvoeren van een telefoongesprek of een SMS-verificatie
- Apparaat token - bij het uitvoeren van verificatie van de mobiele app
- Verificatiemodus
- Verificatieresultaat
- De naam van de MVR gesloten
- MVR gesloten Server IP
- Client IP-indien beschikbaar



Naast de velden is de verificatieresultaat (succes/DOS) en de reden voor eventuele weigeringen ook met de verificatiegegevens opgeslagen en beschikbaar zijn via de verificatie/gebruiksrapporten.


## <a name="advanced-azure-multi-factor-authentication-server-configurations"></a>Geavanceerde configuraties van Azure meerledige verificatie Server
Gebruik de onderstaande tabel voor meer informatie over geavanceerde setup en configuratie-informatie.

Methode|Beschrijving
:------------- | :------------- |
[User Portal](multi-factor-authentication-get-started-portal.md)|  Informatie over het instellen en configureren van de User portal met inbegrip van de implementatie en de gebruiker zelf.
[Active Directory Federation-Service](multi-factor-authentication-get-started-adfs.md)|Informatie over het instellen van Azure meerledige verificatie met AD FS.
[RADIUS-verificatie](multi-factor-authentication-get-started-server-radius.md)|  Informatie over het instellen en configureren van de Server van de MVR gesloten Azure met straal.
[IIS-verificatie](multi-factor-authentication-get-started-server-iis.md)|Informatie over het instellen en configureren van de MVR gesloten Azure Server met IIS.
[Windows-verificatie](multi-factor-authentication-get-started-server-windows.md)|  Informatie over het instellen en configureren van de MVR gesloten Azure Server met Windows-verificatie.
[LDAP-verificatie](multi-factor-authentication-get-started-server-ldap.md)|Informatie over het instellen en configureren van de Server van de MVR gesloten Azure met LDAP-verificatie.
[Extern bureaublad-Gateway en Azure meerledige verificatieserver RADIUS gebruiken](multi-factor-authentication-get-started-server-rdg.md)|  Informatie over het instellen en configureren van de Server van de MVR gesloten Azure met extern bureaublad-Gateway RADIUS gebruiken.
[Synchronisatie met Windows Server Active Directory](multi-factor-authentication-get-started-server-dirint.md)|Informatie over het instellen en configureren van synchronisatie tussen Active Directory en de Server van de MVR gesloten Azure.
[De Azure meerledige verificatie Server mobiele App Web Service implementeren](multi-factor-authentication-get-started-server-webservice.md)|Informatie over het instellen en configureren van de MVR gesloten Azure server webservice.
