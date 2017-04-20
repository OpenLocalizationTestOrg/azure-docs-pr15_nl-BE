<properties
    pageTitle="Azure AD verbinden: Writeback apparaat inschakelen | Microsoft Azure"
    description="Het inschakelen van apparaat writeback Azure AD verbinding maken met een gedetailleerd overzicht van dit document"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-enabling-device-writeback"></a>Azure AD verbinden: Writeback apparaat inschakelen

>[AZURE.NOTE] Een abonnement op Azure AD Premium is vereist voor het terugschrijven naar apparaat.

De volgende documentatie bevat informatie over het inschakelen van de functie apparaat Write-back in Azure AD verbinding maken. Apparaat Write-back wordt gebruikt in de volgende scenario's:

- Voorwaardelijke toegang op basis van AD FS-apparaten (2012 R2 of hoger) beveiligde toepassingen (gebruikmakende partij vertrouwensrelaties).

Dit biedt extra beveiliging en zekerheid dat de toegang tot toepassingen wordt slechts verleend aan vertrouwde apparaten. Zie voor meer informatie over voorwaardelijke toegang, [Risico's beheren met voorwaardelijke toegang](active-directory-conditional-access.md) en het [instellen van On-premises voorwaardelijke toegang met behulp van Azure Active Directory de registratie](https://msdn.microsoft.com/library/azure/dn788908.aspx).

>[AZURE.IMPORTANT]
<li>Apparaten moeten zich bevinden in hetzelfde forest als de gebruikers. Aangezien apparaten moeten worden geschreven aan één forest, ondersteunt deze functie niet momenteel een distributie met meerdere forests van de gebruiker.</li>
<li>Registratie-configuratie-object slechts één apparaat kan worden toegevoegd aan het Active Directory-forest op gebouwen. Deze functie is niet compatibel met een topologie waarin Active Directory op ruimten tot meerdere directory's van Azure AD wordt gesynchroniseerd.</li>

## <a name="part-1-install-azure-ad-connect"></a>Deel 1: Installeer Azure AD verbinding
1. Azure AD verbinden via een aangepaste installatie of Express instellingen. Microsoft raadt beginnen met alle gebruikers en groepen met elkaar gesynchroniseerd voordat u terugschrijven apparaat inschakelen.

## <a name="part-2-prepare-active-directory"></a>Deel 2: Active Directory voorbereiden
Gebruik de volgende stappen uit om voor te bereiden voor het gebruik van apparaat Write-back.

1.  Starten van de computer waarop de Azure AD verbinden is geïnstalleerd, PowerShell in verhoogde modus.

2.  Als de module Active Directory PowerShell niet is geïnstalleerd, installeert u deze met de volgende opdracht:

    `Install-WindowsFeature –Name AD-Domain-Services –IncludeManagementTools`

3. Als de module Azure Active Directory PowerShell niet is geïnstalleerd, downloaden en installeren vanaf [Azure Active Directory Module voor Windows PowerShell (64-bits versie)](http://go.microsoft.com/fwlink/p/?linkid=236297). Dit onderdeel wordt afhankelijk is van de assistent-in, die wordt geïnstalleerd met Azure AD verbinden.

4.  Voer de volgende opdrachten met enterprise Administrator-referenties, en vervolgens af te sluiten PowerShell.

    `Import-Module 'C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1'`

    `Initialize-ADSyncDeviceWriteback {Optional:–DomainName [name] Optional:-AdConnectorAccount [account]}`

Enterprise Administrator-referenties zijn vereist omdat wijzigingen in de naamruimte configuratie nodig zijn. Een domeinadministrator hebben niet voldoende rechten.

![PowerShell voor het terugschrijven apparaat inschakelen](./media/active-directory-aadconnect-feature-device-writeback/powershell.png)

Beschrijving:

- Als ze niet al bestaan, maakt en configureert u de nieuwe containers en objecten onder CN = configuratie van registratie, CN = Services, CN = Configuration, [forest-dn].
- Als ze niet al bestaan, maakt en configureert u de nieuwe containers en objecten onder CN = RegisteredDevices, [domein-dn]. Apparaatobjecten wordt in deze container gemaakt.
- Vereiste machtigingen ingesteld op de rekening voor het beheren van apparaten in uw Active Directory AD-Connector Azure.
- Alleen moet worden uitgevoerd op één forest, zelfs als de Azure AD verbinding maken met meerdere forests wordt geïnstalleerd.

Parameters:

- Domeinnaam: Active Directory-domein waar de apparaatobjecten worden gemaakt. Opmerking: Alle apparaten voor een bepaalde Active Directory-forest wordt gemaakt in één domein.
- AdConnectorAccount: Active Directory-account die wordt gebruikt door Azure AD verbinden voor het beheren van objecten in de directory. Dit is de account die verbinding maken met het AD Azure AD verbinden sync gebruikt. Als u met express-instellingen hebt geïnstalleerd, is de account die wordt voorafgegaan door een MSOL_.

## <a name="part-3-enable-device-writeback-in-azure-ad-connect"></a>Deel 3: Enable apparaat terugschrijven in Azure AD-verbinding maken
Gebruik de volgende procedure apparaat Write-back in Azure AD verbinding maken inschakelen.

1.  De installatiewizard opnieuw uitvoeren. Selecteer **synchronisatie-opties aanpassen** vanuit de pagina meer taken en klik op **volgende**.
![Aangepaste installatie Synchronisatieopties aanpassen](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback2.png)
2.  Optionele functies op de pagina apparaat Write-back niet meer grijs weergegeven. Neem wordt opmerking dat als de verbinding van de advertentie Azure prep stappen zijn niet voltooide apparaat writeback uit op de pagina optionele functies beschikbaar. Schakel het selectievakje in voor apparaat Write-back en klik op **volgende**. Als het nog steeds het selectievakje is uitgeschakeld, raadpleegt u de [sectie troubleshooting](#the-writeback-checkbox-is-still-disabled).
![Aangepast apparaat Writeback optionele functies installeren](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback3.png)
3.  Op de pagina Write-back ziet u het opgegeven domein als het forest standaard apparaat Write-back.
![Aangepaste installatie apparaat terugschrijven naar doel-forest](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback4.png)
4.  Voltooi de installatie van de Wizard zonder aanvullende configuratie wijzigingen. Raadpleeg desgewenst [aangepaste installatie van Azure AD verbinden.](./connect/active-directory-aadconnect-get-started-custom.md)

## <a name="enable-conditional-access"></a>Voorwaardelijke toegang
Gedetailleerde instructies voor het inschakelen van dit scenario zijn beschikbaar in de [On-premises voorwaardelijke toegang met behulp van Azure Active Directory de registratie instellen](https://msdn.microsoft.com/library/azure/dn788908.aspx).

## <a name="verify-devices-are-synchronized-to-active-directory"></a>Controleer of de dat apparaten worden gesynchroniseerd met Active Directory
Apparaat Write-back moet nu correct werken. Let erop dat het apparaatobjecten worden geschreven terug naar AD tot 3 uur kan duren.  Om te controleren dat uw apparaten zijn correct wordt gesynchroniseerd, volgt nadat de synchronisatie regels:

1.  Start Active Directory Administrative Center.
2.  Vouw de RegisteredDevices binnen het domein dat federated is.
![Active Directory-beheercentrum geregistreerde apparaten](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback5.png)
3.  Huidige geregistreerde apparaten worden daar weergegeven.
![Active Directory-beheercentrum apparaatlijst geregistreerd](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback6.png)

## <a name="troubleshooting"></a>Het oplossen van problemen

### <a name="the-writeback-checkbox-is-still-disabled"></a>Het selectievakje Write-back is nog steeds uitgeschakeld.
Als het selectievakje voor een apparaat Write-back is niet ingeschakeld, zelfs als u de bovenstaande stappen hebt gevolgd, volgt helpt u wat de installatiewizard te controleren voordat het beschikbaar wordt.

Eerste dingen eerste:

- Zorg ervoor dat ten minste één forest Windows Server 2012R2. Het apparaattype moet aanwezig zijn.
- Als de installatiewizard wordt al uitgevoerd, vervolgens de wijzigingen niet gedetecteerd. In dit geval de installatiewizard voltooien en start opnieuw.
- Controleer of de account die u in het initialisatie script opgeeft daadwerkelijk de juiste gebruiker door de Active Directory Connector gebruikt. U kunt dit controleren door de volgende stappen uit:
    - Open de **Tijdsynchronisatie-Service**uit het startmenu.
    - Open het tabblad **verbindingslijnen** .
    - De verbindingslijn met het type Active Directory Domain Services te zoeken en te selecteren.
    - Selecteer onder **Acties**, **Eigenschappen**.
    - Ga naar het **verbinding maken met Active Directory-Forest**. Controleer of de naam van de domein- en gebruikersnaam opgegeven op dit scherm overeenkomen met de account die aan het script.
![Connector-account in Sync Service Manager](./media/active-directory-aadconnect-feature-device-writeback/connectoraccount.png)

Controleer of de configuratie in Active Directory:
- Controleren of de registratieservice via het apparaat is gevonden in de onderstaande locatie (CN = DeviceRegistrationService, CN apparaat registratie Services, CN = apparaat registratie Configuration, CN = Services, CN = configuratie =) onder de configuratienaamgevingscontext.

![Problemen met DeviceRegistrationService in de naamruimte van de configuratie](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot1.png)

- Controleer of er slechts één configuratieobject door te zoeken in de naamruimte van de configuratie. Als er meer dan één, verwijder het duplicaat.

![Problemen oplossen, dubbele objecten zoeken](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot2.png)

- Op het apparaat registratie-object, moet het kenmerk msDS-DeviceLocation aanwezig is en een waarde. Zoeken deze locatie en zorg ervoor dat het met het objectType msDS-DeviceContainer aanwezig is.

![Problemen met msDS-DeviceLocation](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot3.png)

![Problemen met de klasse object RegisteredDevices](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot4.png)

- Controleer of de account die wordt gebruikt door Active Directory Connector heeft vereiste machtigingen voor de container geregistreerde apparaten door de vorige stap. Dit is de verwachte machtigingen voor deze container:

![Problemen oplossen, Controleer of de machtigingen voor de container](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot5.png)

- Controleer of de Active Directory-account heeft machtigingen op de GN apparaat registratie Configuration, CN = Services, CN = =-configuratie-object.

![Problemen oplossen, Controleer de machtigingen op registratie apparaatconfiguratie](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot6.png)

## <a name="additional-information"></a>Als u meer informatie
- [Risico met voorwaardelijke toegang beheren](active-directory-conditional-access.md)
- [On-premises voorwaardelijke toegang met behulp van Azure Active Directory de registratie instellen](https://msdn.microsoft.com/library/azure/dn788908.aspx)

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md).
