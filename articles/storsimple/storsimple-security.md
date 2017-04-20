<properties 
   pageTitle="StorSimple beveiliging | Microsoft Azure" 
   description="Beschrijving van de beveiliging en privacy-functies die voor uw StorSimple service, apparaat, en de gegevens in gebouwen en in de cloud." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="05/03/2016"
   ms.author="v-sharos"/>

# <a name="storsimple-security-and-data-protection"></a>StorSimple beveiliging en gegevensbescherming

## <a name="overview"></a>Overzicht

Beveiliging is van groot belang voor iedereen die vast te een nieuwe technologie, stellen is met name wanneer de technologie wordt gebruikt met vertrouwelijke gegevens. Als u verschillende technologieën beoordelen, moet u rekening houden met verhoogde risico's en kosten voor de bescherming van gegevens. Microsoft Azure StorSimple biedt een beveiligings- en privacy-oplossing voor de bescherming van gegevens, zodat: 

- **Vertrouwelijkheid** – alleen bevoegde entiteiten kunnen uw gegevens. 
- **Integriteit** : alleen geautoriseerde entiteiten kunt wijzigen of verwijderen van uw gegevens.

De oplossing met Microsoft Azure StorSimple bestaat uit vier hoofdonderdelen, die met elkaar samenwerken:

- **StorSimple Manager service gehost in Microsoft Azure** : de management-service waarmee u kunt configureren en instellen van het apparaat StorSimple.
- **StorSimple apparaat** – een fysiek apparaat geïnstalleerd in uw datacenter. Alle hosts en clients die gegevens genereren sluit aan op het apparaat StorSimple en het apparaat de gegevens beheert en verplaatst naar de Azure cloud indien nodig.
- **Clients/hosts die zijn verbonden met het apparaat** : de clients in uw infrastructuur die verbinding maken met het apparaat StorSimple en het genereren van gegevens die moeten worden beschermd.
- **Cloud opslag** : de locatie in de Azure cloud waarop gegevens worden opgeslagen.

De volgende secties worden de StorSimple beveiligingsfuncties die helpen voorkomen dat elk van deze onderdelen en de gegevens op deze. Het bevat ook een lijst met vragen die u over Microsoft Azure StorSimple beveiliging en de bijbehorende antwoorden hebt mogelijk.

## <a name="storsimple-manager-service-protection"></a>StorSimple Manager service bescherming

De StorSimple Manager-service is een service die wordt gehost in Microsoft Azure en gebruikt voor het beheren van alle StorSimple apparaten die uw organisatie is aangeschaft. U kunt de StorSimple Manager-service met behulp van uw organisatie-referenties aan te melden op de Azure klassieke portal via een webbrowser openen. 

Toegang tot de service Manager StorSimple, moet uw organisatie een Azure-abonnement met StorSimple. Uw abonnement regelt de functies die u in de klassieke Azure portal openen kunt. Als uw bedrijf beschikt niet over een Azure-abonnement en u wilt meer weten over deze, Zie [Aanmelden voor Azure als organisatie](../active-directory/sign-up-organization.md). 

Omdat de StorSimple Manager-service wordt gehost in Azure, wordt het beveiligd door de Azure beveiligingsfuncties. Voor meer informatie over de beveiligingsfuncties van Microsoft Azure, gaat u naar het [Vertrouwenscentrum van Microsoft Azure](https://azure.microsoft.com/support/trust-center/security/).

## <a name="storsimple-device-protection"></a>StorSimple apparaat bescherming

De StorSimple apparaat is een op ruimten hybride opslag met solid state harde schijven (SSD) en vaste-schijfstations (harde schijven), samen met automatische failover mogelijkheden en redundante controllers. De domeincontrollers beheren opslag trapsgewijs schakelen, het plaatsen van momenteel gebruikt (of warme) gegevens op lokale opslag (in de StorSimple apparaat of op gebouwen-servers), terwijl minder vaak gebruikte gegevens te verplaatsen naar de cloud.

Alleen geautoriseerde apparaten mogen deelnemen aan de StorSimple Manager-service die u hebt gemaakt in uw abonnement Azure StorSimple. Voor het machtigen van een apparaat, moet u deze registreren in de StorSimple Manager-service door middel van de sleutel voor de inschrijving. De sleutel voor de inschrijving is een 128-bits, willekeurige sleutel gegenereerd in de klassieke Azure portal. 

![Sleutel voor de inschrijving](./media/storsimple-security/ServiceRegistrationKey.png)

Voor meer informatie over hoe u een service registratie-sleutel, Ga naar [stap 2: de registratie sleutel ophalen](storsimple-deployment-walkthrough.md#step-2-get-the-service-registration-key).

De sleutel voor de inschrijving is een lange sleutel die 100 + tekens bevat. U kunt de sleutel kopiëren en opslaan in een tekstbestand op een veilige locatie, zodat u kunt u extra apparaten zo nodig toe te staan. Als de registratie sleutel verloren gegaan is nadat u het eerste apparaat hebt geregistreerd, kunt u een nieuwe sleutel genereren uit de StorSimple Manager-service. Dit heeft geen invloed op de werking van de bestaande apparaten. 

Nadat een apparaat is geregistreerd, worden deze tokens om te communiceren met Microsoft Azure. De sleutel voor de inschrijving wordt niet gebruikt na de registratie.

> [AZURE.NOTE] Wij raden aan dat u de registratie sleutel opnieuw na elk gebruik genereren.

## <a name="protect-your-storsimple-solution-via-passwords"></a>Uw oplossing StorSimple via wachtwoorden beveiligen

Wachtwoorden zijn een belangrijk aspect van de beveiliging van de computer en worden veel gebruikt in de StorSimple oplossing om ervoor te zorgen dat uw gegevens voor alleen geautoriseerde gebruikers toegankelijk is. StorSimple kunt u de volgende wachtwoorden configureren:

- StorSimple apparaat administrator-wachtwoord
- Challenge Handshake Authentication Protocol (CHAP) initiator- en doel-wachtwoorden
- StorSimple Snapshot Manager-wachtwoord

### <a name="windows-powershell-for-storsimple-and-the-storsimple-device-administrator-password"></a>Windows PowerShell voor StorSimple en de StorSimple apparaat administrator-wachtwoord

Windows PowerShell voor StorSimple is een opdrachtregelinterface waarmee u kunt het apparaat StorSimple beheren. Windows PowerShell voor StorSimple bevat functies waarmee u kunnen u uw apparaat te registreren, de netwerkinterface op het apparaat te configureren, bepaalde soorten updates installeren, problemen met uw apparaat via de support-sessie en de status van het apparaat wijzigen. U kunt Windows PowerShell openen voor StorSimple aansluiten op de seriële console op het apparaat of met Windows PowerShell remoting.

PowerShell remoting kan worden gedaan via HTTPS- of HTTP. Als u extern beheer via HTTPS is ingeschakeld, moet u het certificaat voor extern beheer van het apparaat te downloaden en installeren op de RAS-client. Voor meer informatie over PowerShell remoting, Ga naar [extern verbinding maken met uw apparaat StorSimple](storsimple-remote-connect.md).

Nadat u Windows PowerShell voor StorSimple gebruikt voor verbinding met het apparaat, moet u het wachtwoord van de beheerder apparaat aan te melden op het apparaat te geven.

![Apparaat administrator-wachtwoord](./media/storsimple-security/DeviceAdminPW.png)

Houd rekening met de volgende procedures:

- Extern beheer is standaard uitgeschakeld. U kunt de StorSimple Manager-service inschakelen. Beste beveiliging, externe toegang alleen tijdens de periode die daadwerkelijk nodig is moet worden ingeschakeld.
- Als u het wachtwoord wijzigt, moet u alle RAS-gebruikers waarschuwen dat ze zich een onverwachte connectiviteit verlies niet.
- De service Manager StorSimple bestaande wachtwoorden niet ophalen: kan deze alleen herstellen. Wij raden aan dat u alle wachtwoorden opslaan op een veilige plaats zodat er geen een wachtwoord opnieuw instellen als deze is vergeten. Als u een wachtwoord opnieuw instellen wilt, moet u alle gebruikers te waarschuwen voordat u dit opnieuw instellen. 

U kunt de Windows PowerShell-interface openen met behulp van een seriële verbinding met het apparaat. U kunt ook toegang tot het op afstand via HTTP of HTTPS, waarbij extra beveiliging. HTTPS biedt een hogere mate van beveiliging dan een serie- of HTTP-verbinding. Echter, voor het gebruik van HTTPS, moet u eerst een certificaat installeren op de client die toegang het apparaat tot. De RAS-certificaat kunt u downloaden van de configuratiepagina van apparaat in de StorSimple Manager-service. Als het certificaat voor externe toegang gegaan is, als u een nieuw certificaat te downloaden en deze doorgeven aan alle clients die gemachtigd zijn om extern beheer te gebruiken.

### <a name="challenge-handshake-authentication-protocol-chap-initiator-and-target-passwords"></a>Challenge Handshake Authentication Protocol (CHAP) initiator- en doel-wachtwoorden

CHAP is een verificatiemethode die wordt gebruikt door het apparaat StorSimple valideren de identiteit van de externe clients. De controle is gebaseerd op een gedeelde wachtwoorden. CHAP is eenrichtingsverkeer (Unidirectioneel) of onderlinge (bidirectioneel). Met CHAP unilaterale verifieert het doel (de StorSimple-apparaat) een initiator (host). Wederzijdse of omgekeerde CHAP is vereist dat het doel de initiator verifiëren en vervolgens de initiator het doel te verifiëren. De StorSimple kan worden geconfigureerd voor het gebruik van beide methoden.

Let op het volgende wanneer u CHAP configureren:

- De CHAP-gebruikersnaam moet minder dan 233 tekens bevatten.
- Het CHAP-wachtwoord moet tussen 12 en 16 tekens bevatten. Probeert te gebruiken een langere gebruikersnaam of wachtwoord leidt tot een verificatiefout op de Windows-host.
- U niet hetzelfde wachtwoord gebruiken voor zowel de CHAP initiator en het doel van CHAP.
- Nadat u het wachtwoord hebt ingesteld, kunnen worden gewijzigd maar kunnen niet worden opgehaald. Als het wachtwoord is gewijzigd, moet u alle RAS-gebruikers waarschuwen dat ze met succes verbinding met het apparaat StorSimple maken kunnen.

Ga voor meer informatie over CHAP en hoe u deze moet configureren voor uw oplossing StorSimple te [Configureren voor uw apparaat StorSimple CHAP](storsimple-configure-chap.md).

### <a name="storsimple-snapshot-manager-password"></a>StorSimple Snapshot Manager-wachtwoord

StorSimple Snapshot Manager is een Microsoft Management Console (MMC)-module die volume groepen en Windows Volume Shadow Copy-Service gebruikt voor het genereren van toepassing consistent back-ups. Bovendien kunt u StorSimple Snapshot Manager voor het maken van back-up schema's en klonen of volumes herstellen.

Wanneer u een apparaat gebruikt StorSimple Snapshot Manager configureert, moet u de StorSimple Snapshot Manager-wachtwoord op te geven. Dit wachtwoord wordt eerst ingesteld in Windows PowerShell voor StorSimple tijdens de registratie. Het wachtwoord kan ook worden ingesteld en gewijzigd van de StorSimple Manager-service. Dit wachtwoord wordt geverifieerd met StorSimple Snapshot Manager van het apparaat.

![StorSimple Snapshot Manager-wachtwoord](./media/storsimple-security/SnapshotMgrPassword.png)

Het wachtwoord StorSimple Snapshot Manager 14 tot 15 tekens en 3 of meer van een combinatie van hoofdletters, kleine letters, cijfers en speciale tekens moet bevatten. Nadat u de StorSimple Snapshot Manager-wachtwoord instelt, kunnen worden gewijzigd maar kunnen niet worden opgehaald. Als u het wachtwoord wijzigt, moet u alle externe gebruikers.

Ga voor meer informatie over StorSimple Snapshot Manager naar [Wat is StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md)

### <a name="password-best-practices"></a>Aanbevolen methoden voor wachtwoorden

Wij raden u aan gebruik te maken van de volgende richtlijnen om ervoor te zorgen dat StorSimple wachtwoorden sterk en goed beveiligd zijn:

- Wijzig uw wachtwoorden om de drie maanden. De wachtwoorden wijzigen wordt jaarlijks afgedwongen.
- Gebruik sterke wachtwoorden. Voor meer informatie gaat u naar de [sterkere wachtwoorden maken en ze te beschermen](http://blogs.microsoft.com/cybertrust/2014/08/25/create-stronger-passwords-and-protect-them/).
- Gebruik altijd verschillende wachtwoorden voor verschillende mechanismen; elk van de wachtwoorden die u opgeeft moet uniek zijn.
- Wachtwoorden niet delen met iedereen die niet is gemachtigd voor toegang tot het apparaat StorSimple.
- Niet over een wachtwoord voor anderen spreken of aanwijzing bij de indeling van een wachtwoord.
- Als u vermoedt dat een account of het wachtwoord is ingebroken, melden bij de beveiliging uw afdeling.
- Alle wachtwoorden worden als gevoelige en vertrouwelijke informatie. 

## <a name="storsimple-data-protection"></a>Gegevensbescherming StorSimple

Deze sectie beschrijft de StorSimple beveiligingsfuncties die in doorvoer en opgeslagen gegevens te beschermen.

Zoals beschreven in andere secties, worden wachtwoorden toe te staan en gebruikers worden geverifieerd voordat ze toegang hebben tot uw oplossing StorSimple gebruikt. Een andere rekening houden met is gegevens beschermen tegen onbevoegde gebruikers terwijl het wordt overgebracht tussen de systemen voor opslag en terwijl deze wordt opgeslagen. De volgende secties worden de functies voor gegevensbeveiliging StorSimple voorzien.

> [AZURE.NOTE] Deduplication biedt extra bescherming voor gegevens die zijn opgeslagen op het apparaat StorSimple en in de opslag van Microsoft Azure. Wanneer gegevens is deduplicated, de gegevensobjecten worden afzonderlijk opgeslagen van de metagegevens voor het toewijzen en ze: Er is geen beschikbare opslag niveau context om de gegevens op basis van volumestructuur, bestandssysteem of bestandsnaam te reconstrueren.

## <a name="protect-data-flowing-through-the-service"></a>Bescherming van gegevens die via de service

Het primaire doel van de StorSimple Manager-service is voor het beheren en configureren van het apparaat StorSimple. De StorSimple Manager-service wordt uitgevoerd in Microsoft Azure. De Azure klassieke portal te gebruiken apparaat configuratiegegevens in te voeren en vervolgens Microsoft Azure gebruikt de StorSimple Manager om de gegevens naar het apparaat sturen. Een systeem van asymmetrische sleutelparen StorSimple gebruikt om ervoor te zorgen dat een inbreuk op de Azure-service niet in een compromis van opgeslagen gegevens resulteren zal. 

![Codering van gegevens tijdens de vlucht](./media/storsimple-security/DataEncryption.png)

De asymmetrische sleutels system helpt bij het beveiligen van de gegevens die via de service als volgt:

1. Een certificaat voor codering van gegevens die gebruikmaakt van een asymmetrische openbare en persoonlijke sleutel paar op het apparaat wordt gegenereerd en wordt gebruikt om de gegevens te beschermen. De sleutels worden gegenereerd wanneer het eerste apparaat is geregistreerd. 
2. Het certificaatsleutels voor gegevenscodering worden geëxporteerd naar een bestand Personal Information Exchange (.pfx) dat wordt beveiligd door de coderingssleutel service gegevens, namelijk een sterke 128-bits sleutel dat willekeurig wordt gegenereerd door het eerste apparaat tijdens de registratie.
3. De openbare sleutel van het certificaat veilig beschikbaar wordt gemaakt voor de StorSimple Manager-service en de persoonlijke sleutel blijft met het apparaat.
4. Invoeren van de gegevens gecodeerd met behulp van het publiek en de gedecodeerd met behulp van de persoonlijke sleutel die is opgeslagen op het apparaat, ervoor te zorgen dat de Azure service de gegevens die aan het apparaat niet decoderen.

De sleutel data service wordt gegenereerd op alleen het eerste apparaat geregistreerd met de service. Alle volgende apparaten die zijn geregistreerd bij de service moeten dezelfde coderingssleutel voor service gegevens gebruiken. 

> [AZURE.IMPORTANT] 
> 
> Het is belangrijk dat u een kopie van de sleutel data service maken en opslaan in een beveiligde locatie. Een kopie van de sleutel data codering moet op zodanige wijze dat het kan worden benaderd door een bevoegd persoon en gemakkelijk kan worden doorgegeven aan de beheerder van het apparaat worden opgeslagen.
>
> Als de sleutel voor de codering gegevens verloren gaan, kunt een ondersteuningsmedewerker van Microsoft u ophalen op voorwaarde dat er ten minste één apparaat in de on line zijn. We raden u aan de sleutel data encryption wijzigen nadat deze is opgehaald. Ga naar [de sleutel data service wijzigen](storsimple-service-dashboard.md#change-the-service-data-encryption-key)voor instructies.

De sleutel data service en het bijbehorende certificaat voor codering kunt u wijzigen door de optie **coderingssleutel gegevens wijzigen** op de servicedashboard. Om ervoor te zorgen dat gegevens niet leiden tot beveiligingsproblemen, moet u een fysiek apparaat voor StorSimple te wijzigen van de sleutel data service. Wijzigen van de coderingssleutels vereist dat alle apparaten worden bijgewerkt met de nieuwe sleutel. Daarom raden we u wijzigt de sleutel wanneer alle apparaten on line zijn. Als er apparaten zijn off line is, kunnen de sleutels op een ander moment worden gewijzigd. De apparaten met verouderde sleutels nog wel back-ups uitvoeren, maar ze is niet mogelijk om gegevens te herstellen totdat u de sleutel wordt bijgewerkt. Ga naar [het servicedashboard StorSimple Manager-gebruiken](storsimple-service-dashboard.md)voor meer informatie.

De sleutel data service en het versleutelingscertificaat gegevens verlopen niet. We raden echter aan dat u de sleutel data service per jaar om te voorkomen dat inbreuk op sleutel wijzigen.

## <a name="protect-data-at-rest"></a>Bescherming van gegevens in rust

Gegevens van het apparaat StorSimple worden beheerd door op te slaan in lagen, lokaal en in de cloud, afhankelijk van de frequentie van gebruik. Alle host-computers die zijn aangesloten op het apparaat verzenden gegevens naar het apparaat dat gegevens vervolgens naar de cloud, indien nodig verplaatst. Gegevens worden overgebracht van het apparaat naar de cloud veilig via het Internet. Elk apparaat heeft een iSCSI-doel waarmee alle gedeelde volumes voor dat apparaat. Alle gegevens worden gecodeerd voordat deze wordt verzonden naar cloud opslag. 

![Coderingssleutel cloud-opslag](./media/storsimple-security/CloudStorageEncryption.png)

Om te helpen bij de beveiliging en integriteit van gegevens die zijn verplaatst naar de cloud, kunt StorSimple u coderingssleutels voor cloud opslag als volgt definiëren:

- De coderingssleutel cloud opslag wordt u bij het maken van een volume-container. De sleutel kan niet worden gewijzigd of later toegevoegd. 
- Alle volumes in een container volume delen dezelfde coderingssleutel. Als u een andere vorm van codering voor een bepaald volume, wordt aangeraden dat u een nieuwe container volume als dat volume host maakt.
- Als u de coderingssleutel cloud opslag in de StorSimple Manager-service, wordt de sleutel wordt gecodeerd met behulp van het openbare gedeelte van de sleutel data service en vervolgens naar het apparaat verzonden.
- De coderingssleutel cloud opslag wordt niet opgeslagen in de service en is alleen bekend is bij het apparaat.
- Een coderingssleutel cloud opslag geven is optioneel. U kunt gegevens die zijn gecodeerd op de host naar het apparaat sturen.

### <a name="additional-security-best-practices"></a>Aanbevolen procedures voor extra beveiliging

- Verkeer gesplitst: uw iSCSI SAN van gebruikersverkeer op een LAN-netwerk te isoleren door implementatie van een volledig gescheiden netwerk en het gebruik van VLAN's waarbij fysieke afscherming is niet een optie. Een exclusief netwerk voor iSCSI-opslag wordt gegarandeerd voor de veiligheid en de prestaties van uw bedrijfskritieke gegevens. Opslag en verkeer via een LAN-netwerk te mengen kunt wordt niet aanbevolen en verhogen latentieperiode en leiden tot netwerkstoringen.

- Gebruik voor de host-zijde netwerkbeveiliging, netwerkinterfaces die ondersteuning bieden voor TCP/IP Offload Engine (TOE). CPU-belasting beperkt TOE door de verwerking van TCP op de netwerkadapter.

## <a name="protect-data-via-storage-accounts"></a>Bescherming van gegevens via opslag rekeningen

Elk abonnement Microsoft Azure kan één of meer rekeningen van opslag maken. (Een account opslag biedt een unieke naamruimte voor het werken met gegevens die zijn opgeslagen in de cloud Azure). Toegang tot de account van een opslag wordt bepaald door het abonnement en toegang tot sleutels die zijn gekoppeld aan die account opslag. 

Wanneer u een opslag-account maakt, genereert Microsoft Azure twee opslag 512 bits toegangstoetsen, waarvan er één wordt gebruikt voor verificatie wanneer u het apparaat StorSimple toegang heeft tot de account van de opslag. Houd er rekening mee dat slechts een van deze sleutels gebruikt wordt. De andere sleutel wordt vastgehouden in de reserve, zodat u kunt de toetsen regelmatig draaien. Sleutels draaien, de secundaire sleutel te activeren en vervolgens de primaire sleutel verwijderen. Vervolgens maakt u een nieuwe sleutel voor gebruik tijdens de volgende rotatie. (Om veiligheidsredenen vereisen veel datacenters sleutel draaien.) 

We raden u aan deze best practices voor belangrijke rotatie:

- U moet draaien opslag account sleutels regelmatig om ervoor te zorgen dat uw opslag-account is niet toegankelijk door onbevoegde gebruikers.
- Regelmatig moet de Azure-beheerder wijzigen of de primaire of secundaire sleutel opnieuw genereren met behulp van de sectie voor de opslag van de klassieke Azure portal direct toegang tot de account van de opslag.


## <a name="protect-data-via-encryption"></a>Bescherming van gegevens via codering

StorSimple gebruikt de volgende coderingsalgoritmen ter bescherming van gegevens die zijn opgeslagen of tussen de componenten van uw oplossing StorSimple onderweg.

| Algoritme | Sleutellengte | Toepassingen-protocollen/opmerkingen |
| --------- | ---------- | ------------------------------- |
| RSA       | 2048       | RSA PKCS 1 v1.5 door de klassieke Azure portal configuratiegegevens gecodeerd die worden verzonden naar het apparaat wordt gebruikt: bijvoorbeeld opslag referenties, StorSimple configuratie, en cloud opslag coderingssleutels. |
| AES       | 256        | AES met CBC wordt gebruikt om het openbare gedeelte van de coderingssleutel service gegevens te coderen voordat het wordt verzonden naar de klassieke Azure portal van het apparaat StorSimple. Het wordt ook gebruikt door het apparaat StorSimple om gegevens te coderen voordat de gegevens worden verzonden naar de cloud opslag rekening. |


## <a name="storsimple-virtual-device-security"></a>StorSimple virtueel apparaat beveiliging

[AZURE.INCLUDE [storsimple virtual device security](../../includes/storsimple-virtual-device-security.md)]

## <a name="frequently-asked-questions-faq"></a>Veelgestelde vragen (FAQ)

Hier volgen enkele vragen en antwoorden over beveiliging en Microsoft Azure StorSimple.

**Q:** Mijn service is aangetast. Wat moet de volgende stappen zijn?

**A:** U moet onmiddellijk de sleutel data service en de opslag account sleutels voor de opslag-account die wordt gebruikt voor trapsgewijs schakelen gegevens wijzigen. Raadpleeg voor instructies: 

- [Wijzigen van de sleutel data service](storsimple-service-dashboard.md#change-the-service-data-encryption-key)
- [Key rotatie van opslag rekeningen](storsimple-manage-storage-accounts.md#key-rotation-of-storage-accounts)

**Q:** Ik heb een nieuw apparaat StorSimple die om de sleutel voor de inschrijving vraagt. Hoe krijg ik het terug?

**A:** Deze sleutel is gemaakt bij het maken van de StorSimple Manager-service. Wanneer u de StorSimple Manager-service gebruikt voor verbinding met het apparaat, kunt u de pagina snel starten weer te geven of de registratie sleutel opnieuw genereren. Genereren van een nieuwe service registratie heeft geen invloed op de bestaande geregistreerde apparaten. Raadpleeg voor instructies:

- [Weergeven of de registratie sleutel opnieuw genereren](storsimple-service-dashboard.md#view-or-regenerate-the-service-registration-key)

**Q:** Mijn coderingssleutel service gegevens verloren gegaan. Wat moet ik doen?

**A:** Neem contact op met Microsoft Support. Ze kunnen aanmelden bij een ondersteuningssessie op het apparaat en de Help-informatie ophalen van de sleutel (op voorwaarde dat ten minste één apparaat online is). Onmiddellijk nadat u de sleutel data service hebt verkregen, moet u om te zorgen dat de nieuwe sleutel alleen u kent. Raadpleeg voor instructies:

- [Wijzigen van de sleutel data service](storsimple-service-dashboard.md#change-the-service-data-encryption-key)

**Q:**  Ik een apparaat voor een service data encryption key wijziging toegestaan, maar het belangrijkste proces is niet gestart. Wat moet ik doen?

**A:** Als de time-outperiode is verstreken, moet u het apparaat voor service data encryption key wijziging autoriseren en het proces opnieuw starten.

**Q:**  Ik de sleutel data service gewijzigd, maar ik kon niet de andere apparaten binnen 4 uur werken. Heb ik opnieuw starten?

**A:** De periode van 4 uur is alleen voor het initiëren van de wijziging. Nadat u het updateproces op de geautoriseerde StorSimple apparaat gestart, is de vergunning is geldig totdat alle apparaten worden bijgewerkt.

**Q:** Onze StorSimple beheerder heeft het bedrijf verlaten. Wat moet ik doen?

**A:** Wijzigen en de wachtwoorden die toegang tot het apparaat StorSimple geven opnieuw instellen en wijzigen van de sleutel data service om ervoor te zorgen dat de nieuwe informatie niet bekend is niet-geautoriseerd personeel. Raadpleeg voor instructies:

- [Gebruik van de service Manager StorSimple storsimple wachtwoorden wijzigen](storsimple-change-passwords.md)
- [Wijzigen van de sleutel data service](storsimple-service-dashboard.md#change-the-service-data-encryption-key)
- [CHAP configureren voor uw apparaat StorSimple](storsimple-configure-chap.md)

**Q:** Ik wil het wachtwoord StorSimple Snapshot Manager met een host die is verbonden met het apparaat StorSimple opgeven, maar het wachtwoord is niet beschikbaar. Wat kan ik doen?

**A:** Als u het wachtwoord vergeten bent, moet u een nieuwe maken. Vervolgens moet u alle bestaande gebruikers op de hoogte dat het wachtwoord is gewijzigd en dat zij hun cliënten voor het gebruik van het nieuwe wachtwoord moeten worden bijgewerkt. Raadpleeg voor instructies:

- [De StorSimple Snapshot Manager-wachtwoord wijzigen](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password)
- [Een apparaat verifiëren](storsimple-snapshot-manager-manage-devices.md#authenticate-a-device)

**Q:** Het certificaat voor externe toegang tot het Windows PowerShell voor StorSimple is gewijzigd op het apparaat. Hoe werk ik mijn RAS-clients

**A:** U kunt downloaden van het nieuwe certificaat van de StorSimple Manager-service en geef worden geïnstalleerd in het certificaatarchief van uw RAS-clients. Raadpleeg voor instructies:

- [De cmdlet certificaat importeren](https://technet.microsoft.com/library/hh848630.aspx)

**Q:** Is mijn gegevens beveiligd als de StorSimple Manager service gevaar loopt?

**A:** Service-configuratiegegevens worden altijd gecodeerd met de openbare sleutel wanneer u de gegevens in een webbrowser weergeven. Omdat de service heeft geen toegang tot de persoonlijke sleutel, is de service niet mogelijk om alle gegevens te bekijken. Indien de StorSimple Manager-service is aangetast, is er geen effect, omdat er geen sleutels opgeslagen in de StorSimple Manager-service.

**Q:** Als u iemand toegang krijgt tot het versleutelingscertificaat gegevens, worden mijn gegevens worden aangetast?

**A:** Microsoft Azure van de klant-sleutel voor bestandscodering (.pfx-bestand) in een versleutelde indeling opgeslagen. Omdat het .pfx-bestand is gecodeerd en de StorSimple-service niet wordt de sleutel data service het .pfx-bestand te decoderen, wordt gewoon krijgen toegang tot het .pfx-bestand niet blootstellen geen geheimen.

**Q:** Wat gebeurt er als een overheidsinstelling Microsoft om mijn gegevens vraagt?

**A:** Omdat alle gegevens worden gecodeerd op de service en de persoonlijke sleutel wordt opgeslagen met het apparaat, moet de overheidsinstelling vraag van de klant voor de gegevens. 

## <a name="next-steps"></a>Volgende stappen

[Het apparaat StorSimple implementeren](storsimple-deployment-walkthrough.md).
 
