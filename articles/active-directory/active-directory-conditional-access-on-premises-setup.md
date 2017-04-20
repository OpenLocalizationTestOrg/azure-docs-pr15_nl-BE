<properties
    pageTitle="Instellen van de voorwaardelijke toegang voor ruimten met Azure Active Directory de registratie | Microsoft Azure"
    description="Een stapsgewijze handleiding voor het inschakelen van voorwaardelijke toegang tot voor bedrijfsruimten toepassingen met behulp van Active Directory Federation Services (AD FS) in Windows Server 2012 R2."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>


# <a name="setting-up-on-premises-conditional-access-using-azure-active-directory-device-registration"></a>Instellen van de voorwaardelijke toegang voor ruimten met Azure Active Directory de registratie

Persoonlijk kunnen eigendom apparaten van uw gebruikers gemarkeerd bekend dat uw organisatie door te vereisen dat de gebruikers in plaats van werk join hun apparaten naar de registratieservice Azure Active Directory apparaat. Hieronder vindt u een stapsgewijze handleiding voor het inschakelen van voorwaardelijke toegang tot voor bedrijfsruimten toepassingen met behulp van Active Directory Federation Services (AD FS) in Windows Server 2012 R2.

> [AZURE.NOTE]
> Office 365-licentie of Azure AD Premium is vereist bij het gebruik van apparaten geregistreerd in Azure Active Directory de registratie service voorwaardelijke-beleid. Dit omvat beleid van Active Directory Federation Services (AD FS) bronnen voor gebouwen.

Voor meer informatie over de voorwaardelijke toegang scenario's voor op de gebouwen, Zie [lid worden van de werkplek vanaf elk apparaat voor eenmalige aanmelding en naadloze tweede Factor verificatie via bedrijfstoepassingen](https://technet.microsoft.com/library/dn280945.aspx).

Deze mogelijkheden zijn beschikbaar voor klanten die een Azure Active Directory Premium licentie aanschaffen.

<a name="supported-devices"></a>Ondersteunde apparaten
-------------------------------------------------------------------------
* Apparaten voor Windows 7 domein toegevoegd.
* Windows 8.1 pc en het domein verbonden apparaten.
* iOS 6 en hoger, Safari-browser
* Android 4.0 of hoger, Samsung GS3 of telefoons, de Samsung Note2 of boven de tabletten.


<a name="scenario-prerequisites"></a>Scenario-vereisten
------------------------------------------------------------------------
* Abonnement op Office 365 of Premium Azure Active Directory
* Een huurder Azure Active Directory
* Windows Server Active Directory (Windows Server 2008 of hoger)
* Bijgewerkte schema van Windows Server 2012 R2
* Licentie voor Premium Azure Active Directory
* Windows Server 2012 R2 Federation Services geconfigureerd voor eenmalige aanmelding naar Azure AD
* Windows Server 2012 R2 Web Application Proxy Microsoft Azure Active Directory-verbinding (verbinding maken Azure AD). [Azure downloaden AD hier verbinden](http://www.microsoft.com/en-us/download/details.aspx?id=47594).
* Het domein is geverifieerd.

<a name="known-issues-in-this-release"></a>Bekende problemen in deze versie
-------------------------------------------------------------------------------
* Op basis van voorwaardelijke toegang apparaatbeleid verlangen dat apparaat object terugschrijven naar Active Directory Azure Active Directory. Maximaal uur 3 op apparaatobjecten die kunnen worden geschreven terug naar Active Directory
* iOS 7 apparaten wordt altijd de gebruiker gevraagd om een certificaat te selecteren tijdens de verificatie van clientcertificaten.
* Sommige versies van iOS8, voordat iOS 8.3 niet werkt.

## <a name="scenario-assumptions"></a>Scenario veronderstellingen
In dit scenario wordt ervan uitgegaan dat u beschikt over een hybride omgeving die bestaat uit een huurder Azure AD en een Active Directory op gebouwen. Deze huurder moeten worden verbonden met Azure AD verbinden met een domein gecontroleerd en met AD FS voor eenmalige aanmelding. Onderstaande controlelijst helpt u bij het configureren van uw omgeving naar het werkgebied hierboven beschreven.

<a name="checklist-prerequisites-for-conditional-access-scenario"></a>Controlelijst: Voorwaarden voor voorwaardelijke toegang scenario
--------------------------------------------------------------
De huurder Azure AD verbinden met uw Active Directory op gebouwen.

## <a name="configure-azure-active-directory-device-registration-service"></a>Registratieservice Azure Active Directory-apparaat configureren
Gebruik deze handleiding voor het implementeren en Azure Active Directory apparaat registratie-Service configureren voor uw organisatie.

Deze handleiding wordt ervan uitgegaan dat u Windows Server Active Directory hebt geconfigureerd en u bent geabonneerd op Microsoft Azure Active Directory. Zie de bovenstaande voorwaarden.

Azure Active Directory apparaat registratieservice implementeren met uw huurder Azure Active Directory, kunt u de taken in de volgende controlelijst in volgorde uitvoeren. Wanneer een koppeling u naar een conceptueel onderwerp gaat, terug naar deze controlelijst na het lezen van het conceptuele onderwerp zodat u de resterende taken in deze controlelijst kunt uitvoeren. Sommige taken bevat een scenario validatiestap waarmee u kunt bepalen of dat de stap is voltooid.

## <a name="part-1-enable-azure-active-directory-device-registration"></a>Deel 1: Registratie Azure Active Directory inschakelen

Volg onderstaande inschakelen en configureren van de Azure Active Directory apparaat registratieservice controlelijst.

| Taak                                                                                                                                                                                                                                                                                                                                                                                             | Referentie                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| De registratie inschakelen in uw huurder Azure Active Directory om apparaten te nemen aan de werkplek. Meerledige verificatie is standaard niet ingeschakeld voor de service. Meerledige verificatie wordt echter aangeraden bij het registreren van een apparaat. Voordat maakt meerledige verificatie in ADRS, ervoor te zorgen dat AD FS is geconfigureerd voor een meerledige verificatie. | [Registratie Azure Active Directory inschakelen](active-directory-conditional-access-device-registration-overview.md)               |
| Apparaten ontdekt uw Azure Active Directory apparaat registratieservice door te zoeken naar bekende DNS-records. U moet uw bedrijf DNS configureren zodat apparaten uw Azure Active Directory apparaat registratieservice ontdekken kunnen.                                                                                                                                                   | [Discovery Azure Active Directory de registratie configureren.](active-directory-conditional-access-device-registration-overview.md) |

##<a name="part-2-deploy-and-configure-windows-server-2012-r2-active-directory-federation-services-and-set-up-a-federation-relationship-with-azure-ad"></a>Deel 2: Implementeren en configureren van Windows Server 2012 R2 Active Directory Federation Services en een federation-relatie met Azure Active Directory instellen

| Taak                                                                                                                                                                                                                                                                                                                                                                                             | Referentie                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Implementeren van Active Directory Domain Services-domein met de schema-uitbreidingen voor Windows Server 2012 R2. U hoeft niet een van de domeincontrollers upgraden naar Windows Server 2012 R2. De schema-upgrade is de enige vereiste. | [De Active Directory Domain Services-Schema bijwerken](#upgrade-your-active-directory-domain-services-schema)               |
| Apparaten ontdekt uw Azure Active Directory apparaat registratieservice door te zoeken naar bekende DNS-records. U moet uw bedrijf DNS configureren zodat apparaten uw Azure Active Directory apparaat registratieservice ontdekken kunnen.                                                                                                                                                   | [Voorbereiden van uw Active Directory-ondersteuning voor apparaten](#prepare-your-active-directory-to-support-devices) |


##<a name="part-3-enable-device-writeback-in-azure-ad"></a>Deel 3: Enable apparaat terugschrijven in Azure AD

| Taak                                                                                                                                                                                                                                                                                                                                                                                             | Referentie                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Deel 2 van inschakelen apparaat Write-back in Azure AD verbinding voltooid. Na voltooiing, retourneren deze in deze handleiding. | [Apparaat Write-back in Azure AD-verbinding inschakelen](#upgrade-your-active-directory-domain-services-schema)               |


##<a name="optional-part-4-enable-multi-factor-authentication"></a>[Optioneel] Deel 4: Enable meerledige verificatie

Het wordt sterk aanbevolen dat u een van de verschillende opties voor het meerledige verificatie configureren. Als u dat MVR gesloten wilt, Zie [kiezen de meerledige beveiligingsoplossing voor u](../multi-factor-authentication/multi-factor-authentication-get-started.md). Het bevat een beschrijving van elke oplossing en koppelingen naar informatie waarmee u bij het configureren van de oplossing van uw keuze.

## <a name="part-5-verification"></a>Deel 5: verificatie

De implementatie is voltooid. U kunt nu bepaalde scenario's uitproberen. Volg de onderstaande links om te experimenteren met de service en u vertrouwd bent met de functies


| Taak                                                                                                                                                                                                                         | Referentie                                                                       |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Sommige apparaten toevoegen aan uw werk via Azure Active Directory de registratie. U kunt deelnemen aan iOS, Windows en Android-apparaten                                                                                         | [Apparaten toevoegen aan uw werk via Azure Active Directory de registratie](#join-devices-to-your-workplace-using-azure-active-directory-device-registration) |
| U kunt bekijken en uitschakelen geregistreerde apparaten met behulp van de Portal-beheerder. In deze taak zal u sommige geregistreerde apparaten met behulp van de Portal-beheerder weergeven.                                                        | [Azure Active Directory apparaat registratie-overzicht](active-directory-conditional-access-device-registration-overview.md)                             |
| Controleer of dat apparaatobjecten worden opgeslagen in van Azure Active Directory Windows Server Active Directory.                                                                                                                  | [Controleer of de geregistreerde apparaten zijn geschreven terug naar Active Directory](#verify-registered-devices-are-written-back-to-active-directory)                  |
| Nu dat gebruikers hun apparaten te registreren kunnen, kunt u de toepassing maken-beleid in AD FS waardoor alleen geregistreerde apparaten. In deze taak u maakt toegang een toegangsregel toepassing en een aangepast bericht geweigerd | [Maak een toepassingsbeleid toegang en aangepaste toegang geweigerd](#create-an-application-access-policy-and-custom-access-denied-message)            |



## <a name="integrate-azure-active-directory-with-on-premises-active-directory"></a>Azure Active Directory integreren met Active Directory on-premises
Hierdoor kunt u integreren in uw active directory op lokalen, Azure AD verbinding maken met uw huurder Azure AD. Hoewel de stappen in de klassieke Azure portal beschikbaar zijn, noteer eventuele speciale instructies in deze sectie worden vermeld.

1.  Meld u aan met een account die is een globale beheerder in Azure AD Azure klassieke portal.
2.  Selecteer **Active Directory**in het linkerdeelvenster.
3.  Selecteer op het tabblad **map** de map.
4.  Selecteer het tabblad **Directory-integratie** .
5.  Volg de stappen 1 tot en met 3 Azure Active Directory integreren in uw directory op de ruimten onder de sectie **implementeren en beheren** .
  1.    Domeinen toevoegen.
  2.    Installeren en uitvoeren van Azure AD verbinden: Azure installeren AD verbinding maken met behulp van de volgende instructies, [aangepaste installatie van Azure AD verbinden](./connect/active-directory-aadconnect-get-started-custom.md).
  3. Te controleren en beheren van de directory-synchronisatie. Instructies voor single sign-on zijn beschikbaar in deze stap.

  > [AZURE.NOTE]
  > Federatie configureren met AD FS als vermeld in het document gekoppeld hierboven. U hoeft niet te configureren op een van de functies van de voorbeeldweergave.


## <a name="upgrade-your-active-directory-domain-services-schema"></a>De Active Directory Domain Services-schema bijwerken

> [AZURE.NOTE]
> Upgraden van uw Active Directory-schema kan niet ongedaan worden gemaakt. Het is raadzaam dat u eerst in een testomgeving uitvoeren.

1. Log in op uw domeincontroller met een account met ondernemingsadministrator-en Schema-Administrator.
2. De **\support\adprep [media]** map en submappen kopiëren naar een van uw Active Directory-domeincontrollers.
3. Waarbij [media] is het pad naar de installatiemedia van Windows Server 2012 R2.
4. Vanaf een opdrachtprompt, Ga naar de map adprep en uitvoeren: **adprep.exe/forestprep**. Volg de instructies op het scherm om de schema-upgrade te voltooien.

## <a name="prepare-your-active-directory-to-support-devices"></a>Voorbereiden van uw Active Directory voor de ondersteuning van apparaten

>[AZURE.NOTE] Dit is een eenmalige bewerking die u uitvoeren moet om het voorbereiden van uw Active Directory-forest apparaten worden ondersteund. U moet zijn aangemeld met beheerdersmachtigingen onderneming en uw Active Directory-forest moet het schema Windows Server 2012 R2 om deze procedure te voltooien.


##<a name="prepare-your-active-directory-forest-to-support-devices"></a>Apparaten worden ondersteund uw Active Directory-forest voorbereiden

> [AZURE.NOTE]
>Dit is een eenmalige bewerking die u uitvoeren moet om het voorbereiden van uw Active Directory-forest apparaten worden ondersteund. U moet zijn aangemeld met beheerdersmachtigingen onderneming en uw Active Directory-forest moet het schema Windows Server 2012 R2 om deze procedure te voltooien.

### <a name="prepare-your-active-directory-forest"></a>Het Active Directory-forest voorbereiden

1.  Open een Windows PowerShell-opdrachtvenster en typ op de federation-server: Initialize-ADDeviceRegistration
2.  Wanneer **ServiceAccountName**wordt gevraagd, voert u de naam van de serviceaccount die u hebt geselecteerd als de serviceaccount voor AD FS. Als het een gMSA-account, voer de rekening in de notatie **domain\accountname$** . Voor een domeinaccount gebruikt u de notatie **domain\accountname**.



### <a name="enable-device-authentication-in-ad-fs"></a>Verificatie van AD FS apparaat inschakelen

1. Open de console van het beheer van AD FS op de federatieserver en Ga naar **AD FS** > **Beleid voor verificatie**.
2. Selecteer **globale primaire verificatie bewerken** in het deelvenster **Acties** .
3. Controleer **apparaat-verificatie inschakelen** en klik vervolgens op**OK**.
4. Standaard worden AD FS regelmatig ongebruikte apparaten verwijderd uit Active Directory. Bij het gebruik van Azure Active Directory de registratie zodat apparaten kunnen worden beheerd in Azure, moet u deze taak uitschakelen.


### <a name="disable-unused-device-cleanup"></a>Opruimen van ongebruikte apparaat uitschakelen
1. Open een Windows PowerShell-opdrachtvenster en typ op de federation-server: Set AdfsDeviceRegistration - MaximumInactiveDays 0

### <a name="prepare-azure-ad-connect-for-device-writeback"></a>Voorbereiden op Azure AD verbinden apparaat Write-back

1.  Voer deel 1: Azure AD voorbereiden verbinding.


## <a name="join-devices-to-your-workplace-using-azure-active-directory-device-registration"></a>Apparaten toevoegen aan uw werk via Azure Active Directory de registratie

### <a name="join-an-ios-device-using-azure-active-directory-device-registration"></a>Lid worden van een iOS-apparaat met behulp van Azure Active Directory de registratie

Azure Active Directory de registratie wordt het inschrijvingsproces Over-the-Air profiel gebruikt voor iOS-apparaten. Dit proces begint met de gebruiker verbinding maken met het profiel voor inschrijving URL met behulp van de webbrowser Safari. De URL-notatie is als volgt:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/"yourdomainname"

Waarbij `yourdomainname` is de domeinnaam die u hebt geconfigureerd met Azure Active Directory. Bijvoorbeeld als de naam van het domein contoso.com, is de URL:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com

Er zijn veel verschillende manieren communiceren deze URL voor uw gebruikers. Het is raadzaam deze URL publiceren in een aangepaste toepassing toegang geweigerd weergegeven in AD FS. Hierover vindt u in de sectie aanstaande: [toegang tot het toepassingenbeleid maken en aangepaste toegang geweigerd](#create-an-application-access-policy-and-custom-access-denied-message).

###<a name="join-a-windows-81-device-using-azure-active-directory-device-registration"></a>Lid worden van een Windows 8.1 apparaat met Azure Active Directory de registratie

1. Ga naar **PC-instellingen**op uw apparaat met Windows 8.1 > **netwerk** > **Werkplek**.
2. Voer uw gebruikersnaam in de UPN-indeling. Bijvoorbeeld dan@contoso.com..
3. Selecteer **koppelen**.
4. Wanneer dat wordt gevraagd, aanmelden met uw referenties. Het apparaat is nu gekoppeld.

###<a name="join-a-windows-7-device-using-azure-active-directory-device-registration"></a>Deelnemen aan een Windows 7-apparaat met behulp van Azure Active Directory de registratie
Als het domein Windows 7 registreren verbonden apparaten moet u het apparaat registratie software pakket. Het softwarepakket werkplek Join voor Windows 7 wordt genoemd en is beschikbaar op de [website van Microsoft Connect](https://connect.microsoft.com/site1164). Instructies die het gebruik van het pakket zijn beschikbaar op de [registratie van automatische inrichting configureren voor Windows 7 domein verbonden apparaten](active-directory-conditional-access-automatic-device-registration-windows7.md).

### <a name="join-an-android-device-using-azure-active-directory-device-registration"></a>Deelnemen aan een Android-apparaat met behulp van Azure Active Directory de registratie

De [Verificator Azure voor Android onderwerp](active-directory-conditional-access-azure-authenticator-app.md) heeft instructies op Azure authenticator app installeren op je Android-apparaat en een werk toevoegen. Wanneer een account werken op een Android apparaat is gemaakt, is dat apparaat werkplek deel uitmaakt van de organisatie.

## <a name="verify-registered-devices-are-written-back-to-active-directory"></a>Controleer of de geregistreerde apparaten zijn geschreven terug naar Active Directory
U kunt bekijken en controleren of dat uw apparaatobjecten zijn geschreven terug naar de Active Directory met LDP.exe of met ADSI bewerken. Beide zijn beschikbaar bij de hulpprogramma's voor Active Directory-beheerder.

Standaard wordt in hetzelfde domein bevinden als de AD FS-farm apparaatobjecten die geschreven terug uit Azure Active Directory zijn geplaatst.

    CN=RegisteredDevices,defaultNamingContext

## <a name="create-an-application-access-policy-and-custom-access-denied-message"></a>Maak een toepassingsbeleid toegang en aangepaste toegang geweigerd
Neem het volgende scenario: U een Relying partij vertrouwen in AD FS-toepassing maken en configureren van een verificatieregel uitgifte waarmee alleen geregistreerde apparaten. Nu mogen alleen apparaten die zijn geregistreerd voor toegang tot de toepassing. Als u wilt dat gebruikers toegang krijgen tot de toepassing te vereenvoudigen kunt u een aangepaste toegang geweigerd bericht met instructies over het toevoegen van hun apparaat configureren. Uw gebruikers hebben nu een naadloze manier om hun apparaten te registreren voor toegang tot een toepassing.

De volgende stappen wordt beschreven hoe u voor het implementeren van dit scenario.

>[AZURE.NOTE]
In dit gedeelte wordt ervan uitgegaan dat u al hebt geconfigureerd een vertrouwen partij vertrouwen voor de toepassing in AD FS.

1. Open de AD FS-MMC-hulpprogramma en Ga naar AD FS > vertrouwensrelaties > vertrouwen partij vertrouwt.
2. Zoek de toepassing waarvoor deze nieuwe access-regel van toepassing. Klik met de rechtermuisknop op de toepassing en selecteer Claim regels bewerken...
3. Selecteer het tabblad **Uitgifte verificatieregels** en selecteer vervolgens een **Regel toevoegen**
4. De **Claim regel** sjabloon vervolgkeuzelijst, selecteer **toestaan of weigeren gebruikers op basis van een binnenkomende Claim**. Selecteer **volgende**.
5. In naam van de Claim regel: veld type: **toegang van geregistreerde apparaten**
6. Van de binnenkomende claimtype: vervolgkeuzelijst, selecteer **Geregistreerd gebruiker Is**.
7. In de binnenkomende claim waarde: veld: **true**
8. Selecteer het keuzerondje **toegang voor gebruikers met deze binnenkomende claim** .
9. Selecteer **Voltooien** en selecteer **toepassen**.
10. Verwijder alle regels die zijn meer strikte dan de regel die u zojuist hebt gemaakt. Verwijder bijvoorbeeld de standaardregel **Toegang toestaan voor alle gebruikers** .

De toepassing is nu geconfigureerd voor het toestaan van toegang alleen wanneer de gebruiker is afkomstig van een apparaat dat ze geregistreerd en gekoppeld aan de werkplek. Voor meer geavanceerde-beleid, Zie [Beheren risico met toegangsbeheer op meerdere factoren](https://technet.microsoft.com/library/dn280949.aspx).

Vervolgens configureert u een aangepast foutbericht voor uw toepassing. Het foutbericht zal laten weten dat zij hun apparaat aan de werkplek toevoegen moeten voordat ze toegang tot de toepassing krijgen. U kunt een aangepaste toepassing toegang geweigerd met behulp van aangepaste HTML- en Windows PowerShell.

Open een Windows PowerShell-opdrachtvenster en typ de volgende opdracht op de federatieserver. Delen van de opdracht vervangen door items die specifiek voor uw systeem zijn:

    Set-AdfsRelyingPartyWebContent -Name "relying party trust name" -ErrorPageAuthorizationErrorMessage
U moet uw apparaat registreren voordat u toegang hebt tot deze toepassing.

**Als u werkt met een iOS-apparaat, selecteert u deze koppeling deel te nemen aan het apparaat**:

    a href='https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/yourdomain.com

Deelnemen aan deze i/o-apparaat op uw werkplek.


**Als u een apparaat met Windows 8.1 gebruikt**, kunt u uw apparaat toevoegen door te gaan naar **PC-instellingen ** >  **netwerk** > **Werkplek**.


Waarbij '**partijnaam vertrouwensrelatie vertrouwen**' is de naam van het object van de partij vertrouwensrelatie vertrouwen toepassingen in AD FS.
Waarbij **yourdomain.com** is de domeinnaam die u hebt geconfigureerd met Azure Active Directory. Bijvoorbeeld contoso.com.
Zorg ervoor dat alle regeleinden (indien aanwezig) in de HTML-inhoud die u aan de cmdlet **Set-AdfsRelyingPartyWebContent doorgeeft** verwijderen.


Wanneer gebruikers toegang krijgen de toepassing van een apparaat dat niet met Azure Active apparaat registratie van Active Directory is geregistreerd tot, ontvangen zij nu een pagina die op de onderstaande schermafdruk lijkt.

![Screeshot van een fout wanneer gebruikers hun apparaat met Azure Active Directory nog niet geregistreerd](./media/active-directory-conditional-access/error-azureDRS-device-not-registered.gif)

##<a name="related-articles"></a>Verwante artikelen

- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
