<properties
    pageTitle="Het genereren en beveiligd met een HSM sleutels voor Azure sleutel kluis transfer | Microsoft Azure"
    description="In dit artikel gebruiken om te plannen, te genereren en brengt u uw eigen beveiligd met een HSM toetsen die met Azure sleutel kluis."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="cabailey"/>
#<a name="how-to-generate-and-transfer-hsm-protected-keys-for-azure-key-vault"></a>Het genereren en beveiligd met een HSM transfer toetsen voor Azure sleutel kluis

##<a name="introduction"></a>Inleiding

Voor extra zekerheid, wanneer u Azure sleutel kluis, kunt u importeren of het genereren van sleutels in hardware security modules (HSM's) die nooit de HSM-grens laat. In dit scenario wordt vaak *een eigen sleutel*, of BYOK genoemd. De HSM's zijn FIPS 140-2 niveau 2 gevalideerd. Thales nShield-familie HSM's Azure sleutel kluis gebruikt ter bescherming van uw sleutels.

Gebruik de informatie in dit onderwerp kunt u plannen, genereren en brengt u uw eigen beveiligd met een HSM toetsen die met Azure sleutel kluis.

Deze functionaliteit is niet beschikbaar voor China Azure. 

>[AZURE.NOTE] Zie voor meer informatie over Azure sleutel kluis [Wat is Azure sleutel kluis?](key-vault-whatis.md)  
>
>Zie voor een ophalen begonnen zelfstudie, waaronder een sleutel kluis voor beveiligd met een HSM-sleutels maken, [aan de slag met Azure sleutel kluis](key-vault-get-started.md).

Meer informatie over het genereren van een sleutel beveiligd met een HSM overbrengen via het Internet:

- U kunt de sleutel genereren vanaf een werkstation off line, waardoor de kwetsbaarheid.

- De sleutel wordt gecodeerd met een sleutel Exchange Key (KEK), dat gecodeerd blijft voordat ze worden overgebracht naar de Azure sleutel kluis HSM's. Alleen de gecodeerde versie van uw sleutel, blijft het oorspronkelijke werkstation.

- De toolset stelt de eigenschappen van de sleutel van uw huurder die uw sleutel aan de wereld Azure sleutel kluis beveiliging bindt. Dus kunnen de Azure sleutel kluis HSM's ontvangt en uw sleutel decoderen, deze HSM's gebruiken. Uw sleutel kan niet worden geëxporteerd. Deze binding wordt afgedwongen door de Thales HSM's.

- De sleutel Exchange Key (KEK) die wordt gebruikt voor het coderen van uw sleutel in de HSM Azure sleutel kluis's wordt gegenereerd en kan niet worden geëxporteerd. De HSM's afdwingen dat er geen leesbare versie van de KEK buiten de HSM's kunnen worden. Bovendien bevat de toolset verklaring van Thales dat de KEK kan niet worden geëxporteerd en dat is gegenereerd in een legitieme HSM gefabriceerd werd door Thales.

- De werkset bevat een attest uit dat de wereld Azure sleutel kluis beveiliging ook is gegenereerd op een legitieme HSM vervaardigd door Thales Thales. Deze verklaring bewijst u dat Microsoft genuine hardware wordt gebruikt.

- Microsoft gebruikt afzonderlijke KEKs en afzonderlijke werelden in elke geografische regio beveiliging. Deze scheiding zorgt ervoor dat uw sleutel kan alleen worden gebruikt in datacenters in de regio waarin u deze hebt gecodeerd. Bijvoorbeeld kan niet een sleutel van een Europese klant worden gebruikt in datacenters in Noord-Amerika of Azië.

##<a name="more-information-about-thales-hsms-and-microsoft-services"></a>Meer informatie over Thales HSM's en Microsoft-services

Thales e-beveiliging is een toonaangevende wereldwijde leverancier van gegevenscodering en cyber security-oplossingen voor de financiële diensten, technologie voor hoge productie, overheid en sectoren van de technologie. Met een 40 jaar track record van het beschermen van bedrijven en de overheid informatie worden Thales oplossingen gebruikt door vier van de vijf grootste bedrijven voor energie- en ruimtevaart. Hun oplossingen worden ook gebruikt door 22 NAVO-landen en meer dan 80 procent van wereldwijde transacties te beveiligen.

Microsoft heeft samengewerkt met Thales voor het verbeteren van de status van de illustraties voor HSM's. Deze verbeteringen kunnen u de gebruikelijke voordelen van gehoste services zonder prijsgeeft controle over uw sleutels ophalen. Deze verbeteringen kunnen met name Microsoft de HSM's beheren, zodat u niet te hoeft. Als een service cloud schaalt Azure sleutel kluis op korte termijn te voldoen aan uw organisatie gebruik pieken. Op hetzelfde moment, de sleutel is beveiligd in de HSM's van Microsoft: behoudt u controle over de levenscyclus van belangrijke omdat u de sleutel te genereren en naar HSM's van Microsoft overbrengen.

##<a name="implementing-bring-your-own-key-byok-for-azure-key-vault"></a>Implementatie van uw eigen sleutel (BYOK) van Azure sleutel kluis te brengen

Gebruik de volgende informatie en procedures als u uw eigen sleutel beveiligd met een HSM genereren en vervolgens naar Azure sleutel kluis overbrengen, het eigen scenario sleutel (BYOK).


##<a name="prerequisites-for-byok"></a>Vereisten voor BYOK

Zie de volgende tabel voor een overzicht van de vereisten voor uw eigen sleutel (BYOK) van Azure sleutel kluis te brengen.

|Vereiste|Meer informatie|
|---|---|
|Een abonnement op Azure|Een Azure sleutel kluis maken, moet u een abonnement op Azure: [Meld u aan voor de gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)|
|De laag Azure sleutel kluis Premium service voor de ondersteuning van sleutels beveiligd met een HSM|Zie de website [Azure sleutel kluis prijzen](https://azure.microsoft.com/pricing/details/key-vault/) voor meer informatie over de niveaus van de service en de mogelijkheden voor Azure sleutel kluis.|
|Thales HSM, smartcards en voor ondersteuningssoftware|U moet toegang hebben tot een hardwarebeveiligingsmodule Thales en fundamentele operationele kennis van Thales HSM's. Zie [Thales Hardware Security Module](https://www.thales-esecurity.com/msrms/buy) voor de lijst van compatibele modellen of een HSM aanschaffen als u niet hebt.|
|De volgende hardware en software:<ol><li>Een off line x64 workstation met een minimale Windows besturingssysteem van Windows 7 en Thales nShield software die minimaal versie 11.50.<br/><br/>Als dit werkstation Windows 7 wordt uitgevoerd, moet u de [installatie van Microsoft .NET Framework 4.5](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</li><li>Een werkstation dat is aangesloten op het Internet en heeft een minimale Windows-besturingssysteem van Windows 7.</li><li>Een USB-station of andere draagbare opslagapparaat dat ten minste 16 MB vrije ruimte heeft.</li></ol>|Uit veiligheidsoverwegingen is het raadzaam dat het eerste werkstation niet is verbonden met een netwerk. Deze aanbeveling is echter niet programmatisch afgedwongen.<br/><br/>Houd er rekening mee dat in de instructies die volgen, dit werkstation is aangeduid als het werkstation verbinding is verbroken.</p></blockquote><br/>Ook als de huurder sleutel voor een productienetwerk, wordt aangeraden gebruik te maken van een tweede, afzonderlijk werkstation de toolset downloaden en uploaden van de sleutel van de huurder. Maar voor testdoeleinden kunt u hetzelfde werkstation als het eerste.<br/><br/>Houd er rekening mee dat in de instructies die volgen, dit tweede werkstation is aangeduid als het werkstation met internetverbinding.</p></blockquote><br/>|

##<a name="generate-and-transfer-your-key-to-azure-key-vault-hsm"></a>Genereren en uw sleutel overbrengen naar Azure sleutel kluis HSM

U kunt de volgende vijf stappen genereren en uw sleutel tot een HSM Azure sleutel kluis te brengen:

- [Stap 1: Bereid uw werkstation met internetverbinding](#step-1-prepare-your-internet-connected-workstation)
- [Stap 2: Bereid uw werkstation verbinding is verbroken](#step-2-prepare-your-disconnected-workstation)
- [Stap 3: Uw sleutel genereren](#step-3-generate-your-key)
- [Stap 4: De sleutel voor de overdracht voorbereiden](#step-4-prepare-your-key-for-transfer)
- [Stap 5: Uw sleutel overbrengen naar Azure sleutel kluis](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1-prepare-your-internet-connected-workstation"></a>Stap 1: Bereid uw werkstation met internetverbinding
Voer de volgende procedures op het werkstation dat is aangesloten op het Internet voor deze eerste stap.


###<a name="step-11-install-azure-powershell"></a>Stap 1.1: Azure PowerShell installeren

Vanaf het werkstation internetverbinding, download en installeer de Azure PowerShell-module met de cmdlets voor het beheren van Azure sleutel kluis. Dit vereist een minimale versie van 0.8.13.

Zie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md)voor installatie-instructies.

###<a name="step-12-get-your-azure-subscription-id"></a>Stap 1.2: Uw Azure abonnements-ID ophalen

Een Azure PowerShell-sessie start en zich aanmelden bij uw Azure rekening met de volgende opdracht:

        Add-AzureAccount
Voer uw Azure gebruikersnaam en wachtwoord in het pop-browservenster. Gebruik vervolgens de opdracht [Get-AzureSubscription](https://msdn.microsoft.com/library/azure/dn790366.aspx) :

        Get-AzureSubscription
Zoek vanaf de uitvoer, de ID voor het abonnement dat u voor Azure sleutel kluis gebruiken wilt. U moet later deze abonnement-ID.

Sluit het venster Azure PowerShell niet.

###<a name="step-13-download-the-byok-toolset-for-azure-key-vault"></a>Stap 1.3: De BYOK-toolset voor Azure sleutel kluis downloaden

Ga naar het Microsoft Download Center en [downloadt de toolset Azure sleutel kluis BYOK](http://www.microsoft.com/download/details.aspx?id=45345) voor uw geografisch gebied of een exemplaar van Azure. Gebruik de volgende gegevens ter identificatie van de pakketnaam te downloaden en de overeenkomstige SHA-256 pakket hash:

---

**Noord-Amerika:**

KeyVault-BYOK-extra-Verenigd States.zip

305F44A78FEB750D1D478F6A0C345B097CD5551003302FA465C73D9497AB4A03

---

**Europa:**

KeyVault, BYOK, hulpmiddelen, Europe.zip

C73BB0628B91471CA7F9ADFCE247561C6016A5103EF1A315D49C3EA23AFC0B9C

---

**Azië:**

KeyVault, BYOK, hulpmiddelen, AsiaPacific.zip

BE9A84B6C76661929F9FDAD627005D892B3B8F9F19F351220BB4F9C356694192

---

**Latijns-Amerika:**

KeyVault, BYOK, hulpmiddelen, LatinAmerica.zip
    
9E8EE11972DECE8F05CD898AF64C070C375B387CED716FDCB788544AE27D3D23

---

**Japan:**

KeyVault, BYOK, hulpmiddelen, Japan.zip

E6B88C111D972A02ABA3325F8969C4E36FD7565C467E9D7107635E3DDA11A8B2

---

**Australië:**

KeyVault, BYOK, hulpmiddelen, Australia.zip

7660D7A675506737857B14F527232BE51DC269746590A4E5AB7D50EDD220675D

---

[**Azure overheid:**](https://azure.microsoft.com/features/gov/)

KeyVault, BYOK, hulpmiddelen, USGovCloud.zip

53801A3043B0F8B4A50E8DC01A935C2BFE61F94EE027445B65C52C1ACC2B5E80

---

**Canada:**

KeyVault, BYOK, hulpmiddelen, Canada.zip

A42D9407B490E97693F8A5FA6B60DC1B06B1D1516EDAE7C9A71AA13E12CF1345

---

**Duitsland:**

KeyVault, BYOK, hulpmiddelen, Germany.zip

4795DA855E027B2CA8A2FF1E7AE6F03F772836C7255AFC68E576410BDD28B48E

---
**India:**

KeyVault, BYOK, hulpmiddelen, India.zip

26853511EB767A33CF6CD880E78588E9BBE04E619B17FBC77A6B00A5111E800C

---

Gebruik de cmdlet [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) voor het valideren van de integriteit van uw gedownloade BYOK toolset van Azure PowerShell-sessie.

    Get-FileHash KeyVault-BYOK-Tools-*.zip

De werkset bevat het volgende:

- Een pakket met Key Exchange Key (KEK) waarvan de naam begint met **BYOK-KEK - pak-.**
- Een World Security pakket waarvan de naam begint met **BYOK-SecurityWorld - pak-.**
- Een python-script met de naam v**erifykeypackage.py.**
- Een opdrachtregelprogramma uitvoerbaar bestand met de naam **KeyTransferRemote.exe** en bijbehorende DLL's.
- Een distributiepakket Visual C++, met de naam **vcredist_x64.exe.**

Het pakket kopiëren naar een USB-station of andere draagbare opslag.

##<a name="step-2-prepare-your-disconnected-workstation"></a>Stap 2: Bereid uw werkstation verbinding is verbroken

Voer de volgende procedures op het werkstation dat niet is verbonden met een netwerk (het Internet of uw interne netwerk) voor deze tweede stap.


###<a name="step-21-prepare-the-disconnected-workstation-with-thales-hsm"></a>Stap 2.1: Het werkstation verbroken met Thales HSM voorbereiden

De nCipher (Thales) ondersteuning voor software installeren op een computer met Windows, en voeg vervolgens een HSM Thales op die computer.

Zorg ervoor dat de Thales-hulpprogramma's in het pad (**%nfast_home%\bin** en **%nfast_home%\python\bin**). Typ bijvoorbeeld het volgende:

        set PATH=%PATH%;”%nfast_home%\bin”;”%nfast_home%\python\bin”

Zie voor meer informatie de handleiding met de HSM Thales opgenomen.

###<a name="step-22-install-the-byok-toolset-on-the-disconnected-workstation"></a>2.2 stap: De toolset BYOK geïnstalleerd op het werkstation verbroken

Het pakket BYOK-toolset van het USB-station of andere draagbare opslagmedia kopiëren en voer de volgende handelingen uit:

1. Pak de bestanden uit het gedownloade pakket in een willekeurige map.
2. Voer vcredist_x64.exe uit die map.
3. Volg de instructies voor de installatie de Visual C++ runtime-onderdelen voor Visual Studio 2013.

##<a name="step-3-generate-your-key"></a>Stap 3: Uw sleutel genereren

Voor deze derde stap doen in de volgende procedures op het werkstation verbinding is verbroken.

###<a name="step-31-create-a-security-world"></a>Stap 3.1: Een wereld voor beveiliging maken

Start een opdrachtprompt en voer het programma Thales nieuwe wereld.

    new-world.exe --initialize --cipher-suite=DLf1024s160mRijndael --module=1 --acs-quorum=2/3

Dit programma maakt een bestand **World Security** op NFAST_KMDATA%\local\world, die overeenkomt met de map Beheer-Settings\User C:\ProgramData\nCipher\Key %. In ons voorbeeld verschijnt er drie blanco kaarten en pennen voor elke invoeren, maar kunt u verschillende waarden voor het quorum. Vervolgens geven elke twee kaarten volledige toegang tot de wereld van de beveiliging. Deze kaarten worden de **Beheerder-kaart instellen** voor de nieuwe wereld van de beveiliging.

Doe het volgende:

- Back-up van het bestand ter wereld. Veilig en bescherming van het bestand ter wereld, de beheerder kaarten en hun pin en zorg dat er geen enkele persoon heeft toegang tot meer dan één kaart.

###<a name="step-32-validate-the-downloaded-package"></a>Stap 3.2: Het gedownloade pakket valideren

Deze stap is optioneel maar aanbevolen zodat u kunt het volgende valideren:

- De Key Exchange die is opgenomen in de werkset is gegenereerd vanuit een legitieme Thales HSM.
- De hash van de wereld van de beveiliging die is opgenomen in de werkset is gegenereerd in een legitieme Thales HSM.
- De Key Exchange is niet exporteerbaar.

>[AZURE.NOTE]Voor het valideren van het gedownloade pakket met moet de HSM moet worden aangesloten, ingeschakeld, en een wereld van de beveiliging op het (zoals die u zojuist hebt gemaakt).

Het gedownloade pakket valideren:

1.  Het script verifykeypackage.py uitvoeren door een van de volgende, afhankelijk van uw geografische regio of een exemplaar van Azure koppelverkoop:
    - Voor Noord-Amerika:

            python verifykeypackage.py -k BYOK-KEK-pkg-NA-1 -w BYOK-SecurityWorld-pkg-NA-1
    - Voor Europa:

            python verifykeypackage.py -k BYOK-KEK-pkg-EU-1 -w BYOK-SecurityWorld-pkg-EU-1
    - Voor Azië:

            python verifykeypackage.py -k BYOK-KEK-pkg-AP-1 -w BYOK-SecurityWorld-pkg-AP-1
    - Voor Latijns-Amerika:

            python verifykeypackage.py -k BYOK-KEK-pkg-LATAM-1 -w BYOK-SecurityWorld-pkg-LATAM-1
    - Voor Japan:

            python verifykeypackage.py -k BYOK-KEK-pkg-JPN-1 -w BYOK-SecurityWorld-pkg-JPN-1
    - Voor Australië:

            python verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
    - Voor [Azure regering](https://azure.microsoft.com/features/gov/), die het exemplaar van de Amerikaanse regering van Azure gebruikt:

            python verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
    - Voor Canada:

            python verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
    - Voor Duitsland:

            python verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
    - Voor India:

            python verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
    >[AZURE.TIP]De Thales-software bevat python op %NFAST_HOME%\python\bin

2.  Bevestig dat u ziet het volgende succesvolle validatie geeft: **resultaat: SUCCES**

Dit script controleert de ondertekenaar keten tot aan de hoofdsleutel van Thales. De hash van dit hoofdsleutel is ingesloten in het script en de waarde ervan moet **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**. U kunt deze waarde ook afzonderlijk controleren door de [Thales-website](http://www.thalesesec.com/)te bezoeken.

U kunt nu een nieuwe sleutel maken.

###<a name="step-33-create-a-new-key"></a>Stap 3.3: Maak een nieuwe sleutel

Een sleutel genereren met behulp van het programma Thales **generatekey** .

Voer de volgende opdracht om de sleutel te genereren:

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

Wanneer u deze opdracht uitvoert, gebruikt u deze instructies:

- De parameter *bescherming* moeten worden ingesteld op de waarde **module**, zoals. Hiermee maakt u een sleutel beveiligd module. De toolset BYOK ondersteunt geen sleutels OCS beschermd.

- De waarde van *contosokey* voor de **ident** en **plainname** vervangen door een tekenreekswaarde. Administratieve overheadkosten te minimaliseren en het risico van fouten, is het raadzaam gebruik te maken van dezelfde waarde voor beide. De waarde **ident** moet bevatten alleen cijfers, streepjes en kleine letters.

- De pubexp in dit voorbeeld blijft leeg (standaard), maar kunt u specifieke waarden. Zie voor meer informatie de documentatie van Thales.

Deze opdracht maakt u een ge? exeerd sleutel-bestand met een naam die begint met **key_simple_**, gevolgd door de **ident** die is opgegeven in de opdracht in de map %NFAST_KMDATA%\local. Bijvoorbeeld: **key_simple_contosokey**. Dit bestand bevat een gecodeerde sleutel.

Back-up van dit bestand wordt ge? exeerd sleutel op een veilige plaats.

>[AZURE.IMPORTANT] Wanneer u later uw sleutel naar Azure sleutel kluis overbrengt, exporteren niet Microsoft deze sleutel terug naar zodat het zeer belangrijk dat u back-up van uw wereld sleutel en beveiliging veilig wordt. Neem contact op met Thales voor richtlijnen en aanbevolen procedures voor back-ups van uw sleutel.

U bent nu gereed voor het overbrengen van uw sleutel naar Azure sleutel kluis.

##<a name="step-4-prepare-your-key-for-transfer"></a>Stap 4: De sleutel voor de overdracht voorbereiden

Voor deze vierde stap doen in de volgende procedures op het werkstation verbinding is verbroken.

###<a name="step-41-create-a-copy-of-your-key-with-reduced-permissions"></a>Stap 4.1: Een kopie van de sleutel met beperkte machtigingen maken

Om de machtigingen voor de sleutel vanaf een opdrachtprompt, een van de volgende, afhankelijk van uw geografische regio of een exemplaar van Azure worden uitgevoerd:

- Voor Noord-Amerika:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1
- Voor Europa:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1
- Voor Azië:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1
- Voor Latijns-Amerika:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1
- Voor Japan:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1
- Voor Australië:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
- Voor [Azure regering](https://azure.microsoft.com/features/gov/), die het exemplaar van de Amerikaanse regering van Azure gebruikt:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
- Voor Canada:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
- Voor Duitsland:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
- Voor India:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1


Wanneer u deze opdracht uitvoert, vervangt u *contosokey* door dezelfde waarde die u hebt opgegeven in **stap 3.3: Maak een nieuwe sleutel** uit de stap [uw sleutel genereren](#step-3-generate-your-key) .

U wordt gevraagd uw beveiliging world admin kaarten aansluiten.

Wanneer de opdracht is voltooid, ziet u **resultaat: SUCCES** en de kopie van de sleutel met minder machtigingen in het bestand met de naam key_xferacId_<contosokey>.

###<a name="step-42-inspect-the-new-copy-of-the-key"></a>Stap 4.2: De nieuwe kopie van de sleutel controleren

U kunt desgewenst de Thales's uitvoeren om de minimale machtigingen voor de nieuwe sleutel te bevestigen:

- aclprint.PY:

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
- kmfile-dump.exe:

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
Als u deze opdrachten uitvoert, vervangt u contosokey door dezelfde waarde die u hebt opgegeven in **stap 3.3: Maak een nieuwe sleutel** uit de stap [uw sleutel genereren](#step-3-generate-your-key) .

###<a name="step-43-encrypt-your-key-by-using-microsofts-key-exchange-key"></a>4.3 stap: Uw sleutel coderen met behulp van Microsoft Key Exchange-sleutel

Voer een van de volgende opdrachten, afhankelijk van uw geografische regio of een exemplaar van Azure:

- Voor Noord-Amerika:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Voor Europa:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Voor Azië:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Voor Latijns-Amerika:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Voor Japan:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Voor Australië:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Voor [Azure regering](https://azure.microsoft.com/features/gov/), die het exemplaar van de Amerikaanse regering van Azure gebruikt:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Voor Canada:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Voor Duitsland:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Voor India:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey


Wanneer u deze opdracht uitvoert, gebruikt u deze instructies:

- Vervang *contosokey* door de id die u gebruikt voor het genereren van de sleutel in **stap 3.3: Maak een nieuwe sleutel** uit de stap [uw sleutel genereren](#step-3-generate-your-key) .

- Vervang *SubscriptionID* door de ID van de Azure abonnement met uw sleutel kluis. U deze waarde opgehaald, in de **stap 1.2: uw Azure abonnements-ID ophalen** uit de stap van het [voorbereiden van uw workstation met Internet is verbonden](#step-1-prepare-your-internet-connected-workstation) .

- *ContosoFirstHSMKey* vervangen door een label dat wordt gebruikt voor de naam van het uitvoerbestand.

Wanneer dit is voltooid, wordt **resultaat: SUCCES** en is er een nieuw bestand in de huidige map met de volgende naam: TransferPackage -*ContosoFirstHSMkey*.byok

###<a name="step-44-copy-your-key-transfer-package-to-the-internet-connected-workstation"></a>Stap 4.4: Uw pakket sleutel overdracht kopiëren naar het werkstation met internetverbinding

Gebruik een USB-station of andere draagbare opslag het uitvoerbestand uit de vorige stap (KeyTransferPackage ContosoFirstHSMkey.byok) kopiëren naar uw werkstation met internetverbinding.

##<a name="step-5-transfer-your-key-to-azure-key-vault"></a>Stap 5: Uw sleutel overbrengen naar Azure sleutel kluis

Gebruik de cmdlet [Add-AzureKeyVaultKey](https://msdn.microsoft.com/library/azure/dn868048\(v=azure.300\).aspx) voor het uploaden van de key transfer pakket dat u hebt gekopieerd vanaf het werkstation van de verbinding is verbroken op de HSM Azure sleutel kluis op het werkstation met internetverbinding voor deze laatste stap:

    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\TransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'

Als de upload voltooid is, ziet u de eigenschappen van de sleutel die u zojuist hebt toegevoegd weergegeven.


##<a name="next-steps"></a>Volgende stappen

Nu kunt u deze sleutel beveiligd met een HSM in uw sleutel kluis. Zie voor meer informatie de sectie **Als u wilt een hardwarebeveiligingsmodule (HSM) gebruiken** in de zelfstudie [aan de slag met Azure sleutel kluis](key-vault-get-started.md) .
