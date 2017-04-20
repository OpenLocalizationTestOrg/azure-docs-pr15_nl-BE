<properties
    pageTitle="Azure beveiligingshandleiding voor opslag | Microsoft Azure"
    description="Details van de vele methoden van de Azure opslag, met inbegrip van maar niet beperkt tot RBAC, Storage Service codering codering van Client-side, SMB 3.0 en Azure schijfversleuteling."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="robinsh"/>

#<a name="azure-storage-security-guide"></a>Azure beveiligingshandleiding voor opslag

##<a name="overview"></a>Overzicht

Azure opslag biedt een uitgebreide reeks beveiligingsmogelijkheden die samen kunnen ontwikkelaars toepassingen bouwen. De opslag account zelf kan worden beveiligd met behulp van toegangsbeheer op basis van rollen en Azure Active Directory. Gegevens kunnen worden beveiligd tijdens de overdracht tussen een toepassing en Azure met behulp van [Client-Side codering](storage-client-side-encryption.md), HTTPS of SMB 3.0. Gegevens kan worden ingesteld op automatisch worden gecodeerd als geschreven naar Azure opslag [Opslag Service codering (SSE)](storage-service-encryption.md)te gebruiken. OS en Data schijven die worden gebruikt door virtuele machines kunnen worden versleuteld met [Azure schijf codering](../security/azure-security-disk-encryption.md)worden ingesteld. Gedelegeerde toegang tot de objecten data in Azure opslag kan worden verleend met [Gedeelde toegang handtekeningen](storage-dotnet-shared-access-signature-part-1.md).

Dit artikel biedt een overzicht van elk van deze beveiligingsfuncties die met Azure opslag kunnen worden gebruikt. Koppelingen zijn naar artikelen die de details van elke functie in dat geval kunt u gemakkelijk doen nader onderzoek over elk onderwerp.

Hier vindt u de onderwerpen in dit artikel:

-   [Beveiliging van veranderingenbeheer vlak](#management-plane-security) – uw opslag-Account beveiligen

    Het beheervlak bestaat uit de bronnen die worden gebruikt voor het beheren van uw account voor opslag. In dit gedeelte bespreken we het implementatiemodel Azure Resource Manager en het gebruik van op rollen gebaseerde toegangscontrole RBAC () toegang tot uw opslag rekeningen. We zullen ook spreken over het beheren van uw opslag account sleutels en het opnieuw genereren.

-   [Gegevens vlak beveiliging](#data-plane-security) : toegang tot uw gegevens beveiligen

    In dit gedeelte bekijken we toestaan van toegang tot de objecten van de werkelijke gegevens in uw account voor opslag, als BLOB's, bestanden, wachtrijen en tabellen, met behulp van gedeelde toegang tot handtekeningen en -beleid opgeslagen. Wordt ingegaan op het serviceniveau SAS zowel SAS niveau. We ziet ook hoe u de toegang beperken tot een specifiek IP-adres (of een bereik van IP-adressen), het protocol dat wordt gebruikt om HTTPS te beperken en een gedeelde Access-handtekening intrekken zonder te wachten totdat deze verlopen.

-   [Codering in Transit](#encryption-in-transit)

    In deze sectie wordt beschreven hoe u gegevens kunt beveiligen wanneer u deze van of naar Azure opslag overbrengt. Bespreken we het aanbevolen gebruik van HTTPS en de versleuteling van SMB 3.0 voor bestandsshares Azure. We zullen ook Kijk eens naar de Client-side codering waarmee u de gegevens versleutelen voordat deze op te slaan in een clienttoepassing wordt overgebracht en decoderen van de gegevens na het overbrengen van opslag.

-   [Codering in rust](#encryption-at-rest)

    We zullen hebben over Storage Service-codering (SSE) en hoe deze inschakelen voor een rekening voor opslag, waardoor de blobs blok, BLOB's pagina, en worden automatisch gecodeerd als geschreven naar opslag Azure BLOB's toevoegen. We zullen ook bekijken hoe Azure schijfversleuteling gebruiken en de belangrijkste verschillen en gevallen van schijfversleuteling versus SSE versus Client Side Encryption verkennen. Laten we zien een korte FIPS-naleving voor de computers van de Amerikaanse overheid.

-   Met behulp van [Analytics opslag](#storage-analytics) toegang van Azure opslag controleren

    In deze sectie wordt beschreven hoe u informatie zoeken in de opslag analytics zich aanmeldt voor een aanvraag. We eens echte opslag analytics logboekgegevens en Zie hoe te onderscheiden of een aanvraag wordt ingediend met de sleutel opslag rekening met een handtekening toegang tot gedeelde of anoniem en of het is geslaagd of mislukt.

-   [Op basis van Browser-Clients met behulp van CORS inschakelen](#Cross-Origin-Resource-Sharing-CORS)

    In deze sectie gesprekken voert over het toestaan van verschillende oorsprong resource sharing (CORS). Bespreken we domeintoegang en hoe het met de CORS mogelijkheden ingebouwd in Azure opslag.

##<a name="management-plane-security"></a>Beveiliging van veranderingenbeheer vlak

Het beheervlak bestaat uit bewerkingen die betrekking hebben op de opslag account zelf. Bijvoorbeeld kunt u maken of een opslag account verwijderen, krijgt een lijst met accounts voor opslag in een abonnement, ophalen van de sleutels opslag account of de opslag account sleutels te genereren.

Wanneer u een nieuwe opslag-account maakt, selecteert u een implementatiemodel van klassieke of Resource Manager. Het klassieke model voor het maken van bronnen in Azure kan alleen ofwel volledig, ofwel de toegang tot het abonnement, en op zijn beurt de opslag-account.

Deze handleiding is gericht op het model van Resource Manager is de aanbevolen manier voor het maken van accounts voor opslag. Met de Resource Manager opslag rekeningen, in plaats van die toegang tot het hele abonnement, u kunt de toegang op een meer beperkte niveau aan het management met behulp van op rollen gebaseerde toegangscontrole RBAC ().

###<a name="how-to-secure-your-storage-account-with-role-based-access-control-rbac"></a>Het beveiligen van uw account opslag met op rollen gebaseerde toegangscontrole RBAC)

We bespreken wat RBAC is en hoe u deze kunt gebruiken. Elk abonnement Azure heeft een Azure Active Directory. Gebruikers, groepen en toepassingen van die directory kunnen toegang worden verleend voor het beheer van bronnen in het abonnement Azure het implementatiemodel Resource Manager gebruiken. Dit is op rollen gebaseerde toegangscontrole RBAC () genoemd. Om deze toegang te beheren, kunt u de [Azure portal](https://portal.azure.com/), [Extra CLI Azure](../xplat-cli-install.md), [PowerShell](../powershell-install-configure.md)of de [Azure Storage Resource Provider REST API](https://msdn.microsoft.com/library/azure/mt163683.aspx).

Met het model van de Resource Manager plaatst u de account voor de opslag in een resource en de controle toegang op het beheervlak van die specifieke opslag account met Azure Active Directory. Bijvoorbeeld, kunt u geven bepaalde gebruikers toegang tot de opslag-account te drukken, terwijl andere gebruikers informatie over de account van de opslag bekijken kunnen, maar hebben geen toegang de sleutels opslag account tot.

####<a name="granting-access"></a>Het verlenen van toegang

Toegang wordt verleend door de juiste RBAC-rol toewijzen aan gebruikers, groepen en -toepassingen, op het juiste bereik. U wijst een functie op het niveau van het abonnement, om toegang tot het hele abonnement. U kunt toegang verlenen tot alle bronnen in een groep door machtigingen te verlenen aan de resourcegroep zelf. U kunt ook specifieke rollen toewijzen aan specifieke resources, zoals opslag.

Hier volgen de belangrijkste punten die u weten moet over het gebruik van RBAC toegang te krijgen tot de beheertaken uit te voeren van een account Azure opslag:

-   Wanneer u toegang wilt geven, toewijzen u in feite een rol aan de account die u wilt toegang hebben. U kunt de toegang aan de bewerkingen voor het beheren van opslag account, maar niet op de objecten met de account. Zo kunt u machtigen voor het ophalen van de eigenschappen van de opslag-account (zoals redundantie) te installeren, maar niet aan een container of gegevens in een container in de Blob-opslag.

-   Voor iemand gemachtigd voor toegang tot de objecten in de opslag-account kunt u machtigen hen de sleutels opslag account lezen en die gebruiker kunt die toetsen de BLOB's, wachtrijen, tabellen en bestanden.

-   Rollen kunnen worden toegewezen aan een specifieke gebruikersaccount, een groep gebruikers of voor een specifieke toepassing.

-   Elke functie heeft een lijst van acties en geen acties. De rol van inzender voor virtuele Machine heeft bijvoorbeeld een actie van "listKeys" waarmee de opslag account sleutels moeten worden gelezen. De medewerker heeft 'geen ' acties zoals het bijwerken van de toegang voor gebruikers in Active Directory.

-   Rollen voor opslag omvatten (maar zijn niet beperkt tot) het volgende:

    -   Eigenaar: kunnen ze beheren alles, met inbegrip van access.

    -   Inzender: ze kunnen doen alles wat de eigenaar kan behalve toegang toewijzen. Iemand met deze rol kan bekijken en de opslag account sleutels te genereren. Met de sleutels van de account opslag zij toegang tot de data objects.

    -   Lezer – ze kunnen informatie weergeven over de opslag-account, met uitzondering van geheimen. Bijvoorbeeld als u een rol met reader-machtigingen voor de account van de opslag aan iemand anders toewijst, kunnen ze de eigenschappen van de account opslag bekijken, maar ze niet wijzigingen aanbrengt in de eigenschappen of de toetsen opslag account bekijken.

    -   Opslag Account Inzender: kunnen ze de opslag account beheren – de bronnengroepen en bronnen van het abonnement kan lezen en maken en beheren van abonnement resource groep implementaties. Ze kunnen ook toegang tot de opslag-account te drukken, wat op zijn beurt betekent dat ze toegang tot de gegevens vlak.

    -   Beheerder toegang: kunnen ze toegang tot de account opslag beheren. Ze kunnen bijvoorbeeld lezerstoegang verlenen aan een specifieke gebruiker.

    -   Virtuele Machine Inzender – ze kunnen beheren virtuele machines, maar niet de opslag account waarmee ze zijn verbonden. Deze rol kunt aanbieden, de opslag-account te drukken, wat betekent dat de gebruiker aan wie u deze rol wilt toewijzen op het vlak van de gegevens kunt bijwerken.

        Zij hebben om een gebruiker een virtuele machine maken, kunnen de bijbehorende VHD-bestand maakt in een opslag-account. Hiervoor moeten ze kunnen de opslag account sleutel opgehaald en doorgegeven aan de API voor het maken van de VM. Ze hebben daarom deze machtiging zodat de opslag account sleutels kunnen worden weergegeven.

- De mogelijkheid voor het definiëren van aangepaste rollen is een functie waarmee u voor het opstellen van een reeks acties in een lijst met beschikbare acties die kunnen worden uitgevoerd op Azure bronnen.

- De gebruiker moet worden ingesteld in uw Azure Active Directory voordat u een rol aan hen toewijzen kunt.

- U kunt een rapport van die verleend/ingetrokken wat voor soort toegang of naar wie en op welk bereik met PowerShell of de CLI Azure.

####<a name="resources"></a>Bronnen

-   [Azure Active Directory toegang op basis van rollen beheren](../active-directory/role-based-access-control-configure.md)

    In dit artikel wordt uitgelegd en werking van het toegangsbeheer op basis van Azure Active Directory-rol.

-   [RBAC: Ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md)

    In dit artikel een gedetailleerd overzicht van alle van de ingebouwde functies beschikbaar in RBAC.

-   [Informatie over implementatie resourcemanager en klassieke implementatie](../resource-manager-deployment-model.md)

    In dit artikel worden de bronnenbeheerder implementatie en klassieke implementatiemodellen en uitleg over de voordelen van het gebruik van de Resource Manager en resource-groepen

-   [Azure Compute-, netwerk- en Storage Providers onder de Azure Resource Manager](../virtual-machines/virtual-machines-windows-compare-deployment-models.md)

    In dit artikel wordt uitgelegd hoe de Azure berekenen, netwerk- en Storage Providers onder het model Resource Manager werken.

-   [Toegangsbeheer met de REST API op basis van rollen beheren](../active-directory/role-based-access-control-manage-access-rest.md)

    In dit artikel ziet u hoe de REST-API gebruiken voor het beheren van RBAC.

-   [Azure opslag Resource Provider REST API: naslag](https://msdn.microsoft.com/library/azure/mt163683.aspx)

    Dit is de referentie voor API's kunt u uw account opslag via programmacode beheren.

-   [Handleiding voor verificatie met Azure Resource Manager API voor ontwikkelaars](http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/)

    In dit artikel ziet u hoe om te verifiëren met behulp van de API's voor bronbeheer.

-   [Toegangsbeheer op basis van rollen voor Microsoft Azure van Ignite](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

    Dit is een koppeling naar een video op Channel 9 van de Conferentie 2015 MS Ignite. In deze sessie, ze praten over toegang tot de beheer- en rapportagefuncties in Azure en ontdek de beste praktijken om de toegang tot Azure abonnementen met Azure Active Directory beveiligen.

###<a name="managing-your-storage-account-keys"></a>Uw Account opslag sleutels beheren

Opslag account sleutels zijn tekenreeksen van 512 bits Azure die samen met de rekeningnaam opslag kunnen worden gebruikt voor toegang tot de gegevensobjecten opgeslagen in de opslag-account, bijvoorbeeld BLOB's, entiteiten in een tabel, berichten en bestanden op een share Azure bestanden gemaakt. Toegang tot de opslag account sleutels controleert de toegang tot het vlak van de gegevens voor die account opslag beheren.

Elke account opslag heeft twee sleutels 'Key 1' en 'belangrijke 2"in de [portal Azure](http://portal.azure.com/) en in de PowerShell cmdlets genoemd. Deze kunnen worden hersteld handmatig met behulp van een van verschillende methoden, met inbegrip van maar niet beperkt tot het gebruik van de [portal Azure](https://portal.azure.com/), PowerShell, de CLI Azure of via programmacode met behulp van de .NET opslag Client Library of Azure Storage Services REST API.

Er zijn een aantal redenen om uw account opslag sleutels opnieuw genereren.

-   U kunt opnieuw genereren, op regelmatige basis om veiligheidsredenen.

-   U zou uw account opslag sleutels opnieuw genereren als iemand beheerd inbreken in een toepassing om de sleutel ophalen die is hardcoded of opgeslagen in een configuratiebestand opslaan, zodat ze volledige toegang tot uw account opslag.

-   Een ander geval voor sessiesleutels is uw team met behulp van een toepassing Opslagverkenner waarin de sleutel opslag account behouden blijven als een van de leden van het team verlaat. De toepassing zou blijven werken, dat zij toegang krijgen tot uw account opslag nadat ze verdwenen zijn. Dit is eigenlijk de belangrijkste reden dat deze account niveau gedeelde toegang handtekeningen gemaakt: u kunt een SAS niveau in plaats van de toegangstoetsen in een configuratiebestand opslaan.

####<a name="key-regeneration-plan"></a>Plan van sessiesleutels

U wilt niet alleen opnieuw genereren van de sleutel die u zonder een planning gebruikt. Als u dat doet, kan u alle toegang aan deze account opslag, die leiden verstoring van de belangrijkste tot kan afgekapt. Daarom zijn er twee sleutels. U moet één sleutel opnieuw genereren tegelijk.

Voordat u uw sleutels opnieuw genereren, dient u een lijst met alle toepassingen die afhankelijk zijn van de account van de opslag, alsmede eventuele andere services die u gebruikt in Azure. Bijvoorbeeld, als u Azure Media Services die afhankelijk van uw account voor opslag zijn gebruikt, moet u opnieuw synchroniseren de toegangstoetsen met uw media-service nadat u de sleutel opnieuw genereren. Als u toepassingen zoals een Opslagverkenner, moet u de nieuwe sleutels voor deze toepassingen ook. Houd er rekening mee dat als VMs waarvan VHD-bestanden worden opgeslagen in de opslag-account hebt, kunt u ze zal niet worden beïnvloed door de opslag account sleutels opnieuw genereren.

U kunt uw sleutels in de portal Azure opnieuw genereren. Nadat de sleutels zijn gegenereerd kunnen maximaal 10 minuten worden gesynchroniseerd via Storage-Services nemen.

Wanneer u klaar bent, is hier het algemeen met informatie over hoe u uw sleutel te wijzigen. In dit geval is de veronderstelling dat u sleutel 1 gebruikt en u alles gaat te gebruiken sleutel 2 te wijzigen.

1.  Opnieuw genereren toets 2 om ervoor te zorgen dat het veilig is. U kunt dit doen in de portal Azure.

2.  Wijzig de opslag voor het gebruik van de nieuwe waarde van de sleutel 2 in alle toepassingen waar de opslag-sleutel is opgeslagen. Testen en publiceren van de toepassing.

3.  Nadat alle toepassingen en diensten alsmede de sleutel 1 met succes wordt uitgevoerd, opnieuw te genereren. Dit zorgt ervoor dat iedereen aan wie u niet uitdrukkelijk de nieuwe sleutel hebben niet langer toegang tot de opslag-account hebben wordt.

Als u met sleutel 2 werkt, kunt u hetzelfde proces gebruiken, maar de namen van sleutels reverse.

U kunt migreren via een paar dagen, elke toepassing voor het gebruik van de nieuwe sleutel wijzigen en publiceren. Nadat deze zijn gedaan, moet u teruggaan en de oude sleutel opnieuw genereren, zodat dit niet meer werkt.

Een andere mogelijkheid is de sleutel opslag account in een [Azure sleutel kluis](https://azure.microsoft.com/services/key-vault/) als een geheim en de toepassingen van de sleutel ophalen vanaf daar. Wanneer u de sleutel opnieuw genereren en bijwerken van de sleutel Azure kluis, moet de toepassingen niet worden ingezet omdat ze gewoon door de nieuwe sleutel van de kluis Azure sleutel automatisch. Houd er rekening mee dat u kunt de toepassing telkens wanneer die u deze nodig hebt voor de sleutel lezen, of kunt u deze in de cache in het geheugen en is mislukt bij het gebruik, de sleutel ophalen opnieuw vanuit de Azure sleutel kluis.

Met behulp van Azure sleutel kluis voegt ook een ander beveiligingsniveau voor uw opslag-sleutels. Als u deze methode gebruikt, hebt u nooit de belangrijkste hardcoded opslag in een configuratiebestand dat avenue van iemand krijgen toegang tot de sleutels zonder de uitdrukkelijke toestemming.

Een ander voordeel van Azure sleutel kluis is dat u ook toegang tot uw sleutels met Azure Active Directory kunt beheren. Dit betekent dat u kunt het aantal toepassingen dat moet ophalen van de sleutels van Azure sleutel kluis en weet dat andere toepassingen geen toegang kunnen krijgen tot de sleutels zonder ze machtiging expliciet toegang verlenen.

Opmerking: het verdient aanbeveling slechts één van de toetsen gebruiken in al uw toepassingen tegelijkertijd. Als u sleutel 1 op sommige plaatsen en sleutel 2 van anderen, is het niet mogelijk uw sleutels draaien zonder een toepassing toegang meer hebt.

####<a name="resources"></a>Bronnen

-   [Over Azure opslag Accounts](storage-create-storage-account.md#regenerate-storage-access-keys)

    Dit artikel geeft een overzicht van opslag rekeningen en bekijken, kopiëren en opnieuw genereren van de toegangstoetsen voor opslag.

-   [Azure opslag Resource Provider REST API: naslag](https://msdn.microsoft.com/library/mt163683.aspx)

    Dit artikel bevat koppelingen naar specifieke artikelen over het ophalen van de sleutels van de account opslag en het opnieuw genereren van sleutels opslag account voor een Azure rekening met de REST API. Opmerking: Dit is voor de Resource Manager opslag rekeningen.

-   [Bewerkingen op de rekeningen van de opslag](https://msdn.microsoft.com/library/ee460790.aspx)

    In dit artikel in de Storage Service Manager REST API Reference bevat koppelingen naar specifieke artikelen op te halen en opnieuw genereren van de opslag account sleutels met de REST API. Opmerking: Dit is voor de klassieke opslag rekeningen.

-   [Nooit meer fotorolletjes om key management – toegang tot gegevens in Azure opslag met Azure AD beheren](http://www.dushyantgill.com/blog/2015/04/26/say-goodbye-to-key-management-manage-access-to-azure-storage-data-using-azure-ad/)

    Dit artikel wordt beschreven hoe u met Active Directory toegang tot uw opslag Azure sleutels in Azure sleutel kluis. Ook ziet u hoe u met een project Azure automatisering genereren de sleutels op uurbasis.

##<a name="data-plane-security"></a>Beveiliging van gegevens vlak

Beveiliging van gegevens vlak verwijst naar de methoden die worden gebruikt voor het beveiligen van de DAO-objecten opgeslagen in Azure opslag – de BLOB's, wachtrijen, tabellen en bestanden. We methoden voor het coderen van de gegevens en de beveiliging tijdens de overdracht van de gegevens hebt bekeken, maar hoe doet u dit over het toestaan van toegang tot de objecten?

Er zijn in principe twee methoden voor het beheren van toegang tot de objecten zelf. De eerste is door de toegang tot de sleutels opslag account beheren en de tweede gedeelde toegang tot handtekeningen toegang te verlenen tot specifieke objecten voor een specifieke hoeveelheid tijd wordt gebruikt.

Een geldt Opmerking u toegang kunt toestaan dat de BLOB's door het toegangsniveau voor de container met de BLOB's dienovereenkomstig. Als u toegang hebt ingesteld voor een container voor het Blob of Container, is het toegestaan openbare leestoegang voor de BLOB's in die container. Dit betekent dat iedereen met een URL die verwijst naar een blob in die container kunt openen in een browser zonder gebruik van een gedeelde Access-handtekening of de toetsen opslag account hebben.

###<a name="storage-account-keys"></a>Toetsen voor opslag Account

Opslag account sleutels zijn tekenreeksen van 512 bits Azure die samen met de rekeningnaam opslag kan worden gebruikt voor toegang tot de gegevensobjecten opgeslagen in de opslag-account gemaakt.

U kunt bijvoorbeeld BLOB's lezen, schrijven naar wachtrijen, tabellen maken en wijzigen. Veel van deze acties kunnen worden uitgevoerd via de Azure portal of met behulp van een van de vele toepassingen van Opslagverkenner. U kunt ook code schrijven om te de REST API of een van de opslag Client Libraries gebruiken voor het uitvoeren van deze bewerkingen.

Zoals besproken in de sectie op de [Beveiliging van vlak](#management-plane-security), kan toegang tot de sleutels van de opslag voor een klassieke opslag worden verleend door middel van een volledige toegang tot de Azure abonnement. Toegang tot de sleutels van de opslag voor een opslag met behulp van het model Azure Resource Manager kan worden beheerd via op rollen gebaseerde toegangscontrole RBAC ().

###<a name="how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies"></a>Het overdragen van toegang tot objecten in uw account met behulp van gedeelde toegang tot handtekeningen en -beleid opgeslagen

Een gedeelde Access-handtekening is een tekenreeks met een beveiligingstoken dat kan worden gekoppeld aan een URI waarmee u gemachtigdentoegang tot opslagobjecten en beperkingen zoals de machtigingen en het bereik van de datum en tijd van access instellen.

U kunt toegang verlenen tot BLOB's, containers, berichten, bestanden en tabellen. Met tabellen, kunt u daadwerkelijk machtiging voor toegang tot een scala van diensten in de tabel door te geven van de partitie en rij belangrijke bereiken waarnaar de gebruiker toegang te verlenen. Bijvoorbeeld, als u gegevens opgeslagen met een partitiesleutel van geografische staat hebt, kan geeft u iemand toegang tot alleen de gegevens voor Californië.

In een ander voorbeeld zou u geven een webtoepassing een SAS-token waarmee gegevens schrijven naar een wachtrij en geef een werknemer rol toepassing berichten uit de wachtrij ophalen en verwerken deze token SAS. Of u kunt een klant een token SAS ze gebruiken om afbeeldingen te uploaden naar een container in de Blob-opslag en een web toepassing machtigen voor het lezen van deze foto's. In beide gevallen is er een scheiding van de bezwaren – elke toepassing kan alleen de toegang die zij nodig hebben om hun taak worden gegeven. Dit is mogelijk door het gebruik van gedeelde toegang handtekeningen.

####<a name="why-you-want-to-use-shared-access-signatures"></a>Waarom wilt u gedeelde toegang handtekeningen gebruiken

Waarom zou u wilt een SAS in plaats van alleen uw opslag account sleutel veel gemakkelijker is te gebruiken? Geven van uw sleutel opslag account lijkt op de sleutels van het Koninkrijk opslagruimte delen. Het verleent volledige toegang. Iemand kan de gebruiken en hun volledige muziekbibliotheek uploaden naar je account opslag. Zij kunnen ook uw bestanden vervangen door versies van geïnfecteerde of uw gegevens te stelen. Weggeven, onbeperkte toegang tot uw account opslag is iets dat niet moet worden gedacht.

Met gedeelde toegang handtekeningen, kunt u een client geven alleen de machtigingen die vereist zijn voor een beperkte tijdsduur. Bijvoorbeeld, als iemand een blob is uploaden naar uw account, kunt u ze schrijven toegang verlenen voor net genoeg tijd voor het uploaden van de blob (afhankelijk van de grootte van de blob, natuurlijk). En als u van gedachten verandert, kunt u die toegang intrekken.

Bovendien kunt u opgeven dat aanvragen die zijn gemaakt met behulp van een SAS beperkt tot een bepaald IP-adres of een IP-adresbereik buiten Azure zijn. U kunt ook vereisen dat aanvragen worden gedaan met een bepaald protocol (HTTPS of HTTP/HTTPS). Dit betekent dat als u alleen wilt toestaan van HTTPS-verkeer, u het vereiste protocol alleen op HTTPS instellen kunt en HTTP-verkeer wordt geblokkeerd.

####<a name="definition-of-a-shared-access-signature"></a>Definitie van een gedeelde Access-handtekening

Een gedeelde Access-handtekening is een verzameling queryparameters toegevoegd aan de URL die verwijst naar de bron

dat geeft informatie over de toegang is toegestaan en de periode waarvoor de toegang is toegestaan. Hier volgt een voorbeeld; Deze URI voorziet leestoegang tot een blob in vijf minuten. Opmerking SAS queryparameters moeten gecodeerde URL, zoals % 3A voor de dubbele punt (:) of 20% voor een spatie.

    http://mystorage.blob.core.windows.net/mycontainer/myblob.txt (URL to the blob)
    ?sv=2015-04-05 (storage service version)
    &st=2015-12-10T22%3A18%3A26Z (start time, in UTC time and URL encoded)
    &se=2015-12-10T22%3A23%3A26Z (end time, in UTC time and URL encoded)
    &sr=b (resource is a blob)
    &sp=r (read access)
    &sip=168.1.5.60-168.1.5.70 (requests can only come from this range of IP addresses)
    &spr=https (only allow HTTPS requests)
    &sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D (signature used for the authentication of the SAS)

####<a name="how-the-shared-access-signature-is-authenticated-by-the-azure-storage-service"></a>Hoe de toegang tot gedeelde handtekening is geverifieerd door de Azure Storage-Service

Wanneer de storage-service het verzoek ontvangt, de invoer voor de query parameters en maakt u een handtekening met behulp van dezelfde methode als het aanroepende programma. Vervolgens vergelijkt de twee handtekeningen. Als deze ermee instemt, kunt vervolgens de storage-service controleren de storage service-versie om te controleren of dat het geldig is, controleert u of de huidige datum en tijd in het opgegeven venster zijn, controleert u of de gevraagde toegang komt overeen met de aanvraag is ingediend, enz.

Bijvoorbeeld met onze URL zou als de URL is verwijzen naar een bestand in plaats van een blob, dit verzoek mislukken omdat deze geeft aan dat de toegang tot gedeelde handtekening voor een blob. Als de opdracht REST wordt aangeroepen voor het bijwerken van een blob, zou het mislukt, omdat de toegang tot gedeelde handtekening geeft aan dat alleen leestoegang is toegestaan.

####<a name="types-of-shared-access-signatures"></a>Typen gedeelde toegang handtekeningen

-   Een SAS service-niveau kan worden gebruikt voor toegang tot bepaalde bronnen in een opslag-account. Enkele voorbeelden hiervan zijn een aantal BLOB's in een container, een blob downloaden, bijwerken van een entiteit in een tabel, berichten toe te voegen aan een wachtrij of een bestand uploadt naar een bestandsshare ophalen.

-   Een SAS niveau kan worden gebruikt voor toegang tot alles wat een SAS service-niveau kan worden gebruikt voor. Bovendien geeft deze opties voor bronnen die niet zijn toegestaan met een SAS service-niveau, zoals de mogelijkheid containers, tabellen, wachtrijen en bestandsshares maken. U kunt ook toegang tot meerdere diensten in één keer opgeven. Bijvoorbeeld, misschien geeft u iemand toegang tot BLOB's en bestanden in uw account voor opslag.

####<a name="creating-an-sas-uri"></a>Maken van een SAS-URI

1.  Kunt u een ad-hoc-URI op verzoek, alle queryparameters elke keer te definiëren.

    Dit is heel flexibel, maar als er een logische verzameling parameters die vergelijkbaar elke keer zijn, met behulp van een opgeslagen-beleid is een beter idee.

2.  U kunt een opgeslagen-beleid voor een hele container, een bestandsshare, een tabel of een wachtrij maken. Vervolgens kunt u deze als basis voor de SAS URI's die u maakt. Machtigingen op basis van opgeslagen-beleid kunnen gemakkelijk worden ingetrokken. Er kunnen maximaal 5 beleid gedefinieerd op elke container, wachtrij, tabel of bestandsshare.

    Als u zijn veel mensen de BLOB's in een bepaalde container lezen, kan u bijvoorbeeld een opgeslagen-beleid die zegt "leestoegang geven" en andere instellingen die niet hetzelfde elke keer maken. Vervolgens maakt u een URI SAS met behulp van de instellingen van het toegangsbeleid opgeslagen en de vervaldatum van de datum en tijd op te geven. Het voordeel hiervan is dat u niet aan alle van de queryparameters opgeven die elke keer.

####<a name="revocation"></a>Intrekking

Stel dat uw SAS is aangetast, of u wilt wijzigen in zakelijke beveiliging of regelgeving. Hoe u toegang tot een bron die SAS met intrekken? Dit is afhankelijk van hoe u de URI SAS gemaakt.

Als u een ad-hoc-URI gebruikt, hebt u drie opties. U kunt probleem SAS tokens met korte verloopbeleid en gewoon wachten tot de SA's verloopt. U kunt wijzigen of verwijderen van de bron (ervan uitgaande dat het token is binnen het bereik van een object). U kunt de toetsen opslag account wijzigen. Deze laatste optie kan een grote invloed, afhankelijk van hoeveel services die account opslag gebruikt en is waarschijnlijk niet iets dat die u uitvoeren wilt zonder een planning.

Als u een SAS afgeleid van een opgeslagen-beleid gebruikt, kunt u access verwijderen door het intrekken van het toegangsbeleid opgeslagen – kunt u alleen wijzigen zodat het al is verlopen of kunt u deze helemaal verwijderen. Dit wordt onmiddellijk van kracht en wordt elke SAS gemaakt met behulp van Access opgeslagen beleid ongeldig. Het bijwerken of verwijderen van het toegangsbeleid opgeslagen kan invloed mensen toegang hebben tot die specifieke container, bestandsshare, tabel of wachtrij via SAS, maar als de clients worden geschreven, zodat ze een nieuwe SA's vragen wanneer de oude versie ongeldig wordt, dit werkt prima.

Omdat u met behulp van een SAS afgeleid van een opgeslagen-beleid biedt u de mogelijkheid dat SAS onmiddellijk intrekken, is het aanbevolen best practice-beleid opgeslagen indien mogelijk altijd wilt gebruiken.

####<a name="resources"></a>Bronnen

Raadpleeg de volgende artikelen voor meer gedetailleerde informatie over het gebruik van gedeelde toegang tot handtekeningen en opgeslagen-beleid, compleet met voorbeelden:

-   Dit zijn de artikelen van de referentie.

    -   [SAS service](https://msdn.microsoft.com/library/dn140256.aspx)

        Dit artikel bevat voorbeelden van het gebruik van een SAS service level met BLOB's, berichten, tabel bereiken en bestanden.

    -   [Een SAS-service maken](https://msdn.microsoft.com/library/dn140255.aspx)

    -   [Een SAS-account maken](https://msdn.microsoft.com/library/mt584140.aspx)

-   Dit zijn de cursussen voor gedeelde toegang handtekeningen en opgeslagen-beleid maken met de .NET client library.

    -   [Gebruik van gedeelde toegang handtekeningen (SAS)](storage-dotnet-shared-access-signature-part-1.md)

    -   [Gedeelde toegang handtekeningen, deel 2: Maken en gebruiken van een SAS met de Blob-Service](storage-dotnet-shared-access-signature-part-2.md)

        Dit artikel bevat een uitleg van het model SAS, voorbeelden van gedeelde toegang tot handtekeningen en aanbevelingen voor beste praktijken van SAS gebruiken. Ook besproken, is de intrekking van de machtiging is verleend.

-   Toegang beperken op basis van IP-adres (IP-ACL's)

    -   [Wat is een eindpunt toegangsbeheerlijst (ACL's)?](../virtual-network/virtual-networks-acl.md)

    -   [Een SAS-Service maken](https://msdn.microsoft.com/library/azure/dn140255.aspx)

        Dit is de verwijzing naar artikel voor SAS service niveau; het bevat een voorbeeld van een IP-ACLing.

    -   [Een SAS-Account maken](https://msdn.microsoft.com/library/azure/mt584140.aspx)

        Dit is de verwijzing naar artikel voor SAS niveau; het bevat een voorbeeld van een IP-ACLing.

-   Verificatie

    -    [Verificatie voor de van Azure opslagservices](https://msdn.microsoft.com/library/azure/dd179428.aspx)

-   Gedeelde toegang handtekeningen aan de slag zelfstudie

    -   [Aan de slag zelfstudie SAS](https://github.com/Azure-Samples/storage-dotnet-sas-getting-started)

##<a name="encryption-in-transit"></a>Codering in Transit

###<a name="transport-level-encryption--using-https"></a>Transportniveau codering – via HTTPS

Een andere stap die u uitvoeren moet om de beveiliging van uw gegevens in de opslag van Azure is het coderen van de gegevens tussen de client en de opslag van Azure. De eerste aanbeveling is altijd met het [HTTPS](https://en.wikipedia.org/wiki/HTTPS) -protocol dat zorgt voor beveiligde communicatie via het openbare Internet.

Gebruik altijd HTTPS tijdens het aanroepen van de overige API's of toegang tot in de opslag objecten. **Gedeelde toegang handtekeningen**, die kan worden gebruikt voor het delegeren van toegang tot objecten voor gegevensopslag in Azure, is ook een optie om aan te geven dat alleen het HTTPS-protocol kan worden gebruikt bij het gebruik van gedeelde toegang handtekeningen, ervoor te zorgen dat iedereen verzenden van koppelingen met SAS-tokens het juiste protocol gebruikt.

####<a name="resources"></a>Bronnen

-   [HTTPS voor een app in Azure App-Service inschakelen](../app-service-web/web-sites-configure-ssl-certificate.md)

    In dit artikel wordt beschreven hoe u HTTPS inschakelen voor een Web App van Azure.

###<a name="using-encryption-during-transit-with-azure-file-shares"></a>Met behulp van codering tijdens de overdracht met Azure bestandsshares

Azure bestandsopslag HTTPS biedt ondersteuning bij het gebruik van de REST API, maar meer vaak gebruikt als een bestand SMB-share op een VM is aangesloten. SMB-2.1 biedt geen ondersteuning voor codering, zodat de verbindingen zijn alleen toegestaan in hetzelfde gebied in Azure. Echter SMB 3.0 ondersteunt codering en kan worden gebruikt met Windows Server 2012 R2, Windows 8, Windows 8.1 en Windows 10, waardoor cross-regio toegang krijgen tot en zelfs toegang op het bureaublad.

Houd er rekening mee dat terwijl bestandsshares Azure kan worden gebruikt met Unix, de Linux SMB-client nog geen codering, ondersteunt zodat toegang is alleen toegestaan binnen een gebied met Azure. Ondersteuning voor codering voor Linux is op de routekaart van Linux-ontwikkelaars die verantwoordelijk is voor SMB-functionaliteit. Bij het toevoegen van versleuteling, hebt u dezelfde mogelijkheid voor toegang tot een bestandsshare Azure op Linux als voor Windows.

####<a name="resources"></a>Bronnen

-   [Azure bestandsopslag gebruiken met Linux](storage-how-to-use-files-linux.md)

    In dit artikel ziet u hoe een Azure bestandsshare op een Linux-systeem koppelen en uploaden/downloaden van bestanden.

-   [Aan de slag met Azure opslaan van bestanden in Windows](storage-dotnet-how-to-use-files.md)

    Dit artikel geeft een overzicht van Azure bestandsshares en het koppelen en deze gebruiken met behulp van PowerShell en .NET.

-   [Inside Azure bestandsopslag](https://azure.microsoft.com/blog/inside-azure-file-storage/)

    In dit artikel kondigt de algemene beschikbaarheid van Azure bestandsopslag en technische informatie over de codering SMB 3.0 biedt.

###<a name="using-client-side-encryption-to-secure-data-that-you-send-to-storage"></a>Client-side codering gebruiken voor het beveiligen van gegevens die u naar de opslag verzendt

Een andere optie die u ervoor te zorgen helpt dat uw gegevens veilig worden overgedragen tussen een clienttoepassing en opslag tijdens het Client-side-codering is. De gegevens worden gecodeerd voordat deze worden overgebracht in Azure opslag. Bij het ophalen van de gegevens uit de opslag in Azure, is de gegevens gedecodeerd nadat het op de client is ontvangen. Zelfs als de gegevens worden versleuteld via de kabel gaat, wordt aangeraden te gebruiken HTTPS, omdat deze gegevens integriteitscontroles ingebouwd die helpen verlichten van de integriteit van de gegevens op het gebied van netwerkfouten bevat.

Client-side-codering is ook een methode voor het coderen van uw gegevens in rust, terwijl de gegevens in de gecodeerde vorm worden opgeslagen. Bespreken we dit in meer detail in de sectie over [codering in rust](#encryption-at-rest).

##<a name="encryption-at-rest"></a>Codering in rust

Er zijn drie Azure functies voor codering in rust. Azure schijfversleuteling wordt gebruikt voor het coderen van de gegevens en OS schijven IaaS virtuele Machines. De andere twee – Client-side coderings- en SSE – zijn beide gebruikt voor het coderen van gegevens in de opslag van Azure. We kijken op elk van deze, een vergelijking doen en Zie bij elkaar kan worden gebruikt.

U kunt Client-side codering gebruiken voor het coderen van de gegevens in transit (dit wordt ook opgeslagen in de gecodeerde vorm in opslag), kunt u desgewenst gewoon HTTPS gebruiken tijdens het overbrengen en hebt een manier zijn waarop de gegevens worden automatisch gecodeerd wanneer het wordt opgeslagen. Er zijn twee manieren doen: Azure schijf coderings- en SSE. Een wordt gebruikt voor het coderen van de gegevens op gegevens en OS schijven die worden gebruikt door VMs rechtstreeks en de andere wordt gebruikt voor het coderen van gegevens die worden geschreven naar Azure Blob-opslag.

###<a name="storage-service-encryption-sse"></a>Opslag Service codering (SSE)

SSE kunt u vragen dat de storage-service automatisch coderen van de gegevens bij het schrijven naar Azure opslag. Als u de gegevens uit de opslag van Azure leest, wordt gedecodeerd door de storage-service voordat het wordt geretourneerd. Hiermee kunt u uw gegevens te beveiligen zonder de code te wijzigen of code toevoegen aan toepassingen.

Dit is een instelling die voor de volledige account geldt. U kunt inschakelen en deze functie uitschakelen door de waarde van de instelling te wijzigen. Hiervoor kunt u de portal Azure, PowerShell, de CLI Azure, de opslag Resource Provider REST API of de .NET opslag Client Library. SSE is standaard uitgeschakeld.

Op dit moment zijn de sleutels die worden gebruikt voor de codering worden beheerd door Microsoft. We oorspronkelijk de sleutels genereren en beheren van de veilige opslag van de sleutels als de normale rotatie zoals gedefinieerd door het interne beleid van Microsoft. U wordt in de toekomst krijg de mogelijkheid om uw eigen sleutels beheren en bieden een migratiepad naar klant beheerde sleutels uit Microsoft beheerd sleutels.

Deze functie is beschikbaar voor Standard- en Premium-opslagruimte accounts die zijn gemaakt met behulp van het implementatiemodel Resource Manager. SSE is alleen van toepassing op BLOB's, pagina BLOB's, blokkeren en BLOB's toevoegen. De andere typen gegevens, zoals tabellen, wachtrijen en -bestanden, worden niet gecodeerd.

Gegevens worden alleen gecodeerd als SSE is ingeschakeld en de gegevens worden weggeschreven naar een Blob-opslag. In- of uitschakelen van SSE heeft geen gevolgen voor bestaande gegevens. Met andere woorden, wanneer u deze codering inschakelt, zal het niet terug en coderen van gegevens die al bestaat. ook worden de gegevens die al aanwezig is wanneer u SSE uitschakelen gedecodeerd.

Als u deze functie wilt gebruiken met een klassieke opslag account wilt, kunt u een nieuwe Resource Manager opslag account maken en AzCopy gebruiken om de gegevens te kopiëren naar de nieuwe account. 

###<a name="client-side-encryption"></a>Client-side-codering

Client-side codering wordt gezegd bij het bespreken van de codering van de gegevens tijdens de overdracht. Met deze functie kunt u via programmacode gegevens in een clienttoepassing voordat het wordt verzonden via het netwerk naar opslag Azure worden geschreven en decoderen via programmacode uw gegevens na het uit de opslag van Azure coderen.

Deze codering in transit biedt, maar biedt ook de functie van de codering in rust. Houd er rekening mee dat hoewel de gegevens worden gecodeerd tijdens de overdracht, nog steeds raadzaam gebruik te maken van de ingebouwde data integriteitscontroles toegangsbeheermodel netwerkfouten gevolgen heeft voor de integriteit van de gegevens met behulp van HTTPS.

Een voorbeeld van waar u deze kunt gebruiken is als u een webtoepassing die BLOB's opgeslagen en opgehaald van BLOB's hebt en u wilt dat de toepassing en de gegevens zo veilig mogelijk worden. In dat geval zou u client-side-codering gebruiken. De gecodeerde bron van het verkeer tussen de client en de Azure Blob-Service bevat en niemand kan interpreteren van de gegevens tijdens de overdracht en het opnieuw samenstellen in uw persoonlijke BLOB's.

Client-side-codering is ingebouwd in de Java en .NET opslag clientbibliotheken, die op zijn beurt de Azure sleutel kluis API's, waardoor het heel eenvoudig kunt implementeren. Het proces van coderen en decoderen van de gegevens maakt gebruik van de techniek van de envelop en metagegevens die worden gebruikt door de codering in elke opslagobject wordt opgeslagen. Bijvoorbeeld voor BLOB's, deze opgeslagen in de blob-metagegevens voor wachtrijen, deze toegevoegd aan elk bericht wachtrij.

U kunt genereren en beheren van uw eigen coderingssleutels voor de codering zelf. Bovendien kunt u sleutels die zijn gegenereerd door de Client-bibliotheek van Azure opslag of hebt u de Azure sleutel kluis de sleutels te genereren. Kunt u uw coderingssleutels in opslag op ruimten sleutel opslaan of kunt u deze opslaan in een kluis Azure-sleutel. Azure sleutel kluis kunt u toegang verlenen tot de geheimen in Azure sleutel kluis aan specifieke gebruikers met Azure Active Directory. Dit betekent dat niet alleen iedereen kan de Azure sleutel kluis lezen en ophalen van de sleutels die u voor client-side-codering gebruikt.

####<a name="resources"></a>Bronnen

-   [Coderen en decoderen van BLOB's in Microsoft Azure opslag met Azure sleutel kluis](storage-encrypt-decrypt-blobs-key-vault.md)

    In dit artikel ziet u hoe client-side-codering gebruikt met Azure sleutel kluis, met inbegrip van de KEK maken en deze opslaan in de kluis met PowerShell.

-   [Client-Side codering en Azure sleutel kluis voor Microsoft Azure opslag](storage-client-side-encryption.md)

    Dit artikel geeft een uitleg van client-side codering en voorbeelden van client-bibliotheek van de opslag te coderen en decoderen van de bronnen van de vier storage-services. Het is ook gesprekken voert over Azure sleutel kluis.

###<a name="using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines"></a>Azure schijf codering gebruiken voor het coderen van schijven die worden gebruikt door uw virtuele machines

Azure schijfversleuteling is een nieuwe functie die zich in het voorbeeld. Met deze functie kunt u de OS-schijven en schijven van gegevens door een IaaS virtuele Machine gebruikt te coderen. De stations worden gecodeerd met behulp van industriestandaard BitLocker-versleuteling technologie voor Windows. De schijven zijn gecodeerd met de technologie van DM-Crypt voor Linux. Dit is geïntegreerd met Azure sleutel kluis kunt u controleren en beheren van coderingssleutels van de schijf.

De Azure schijfversleuteling-oplossing ondersteunt de volgende drie klant codering scenario's:

-   Schakel codering in voor nieuwe IaaS VMs gemaakt van de VHD-bestanden gecodeerd met een klant en klant coderingssleutels, die zijn opgeslagen in Azure sleutel kluis.

-   Schakel codering in voor nieuwe IaaS VMs gemaakt op basis van de markt Azure.

-   Schakel codering op bestaande IaaS VMs in Azure is gebeurd.

>[AZURE.NOTE] Voor Linux VMs in Azure is gebeurd, of nieuwe Linux VMs gemaakt op basis van afbeeldingen op Marketplace Azure, wordt codering van de OS-schijf momenteel niet ondersteund. Het coderen van het Volume met het besturingssysteem voor Linux VMs wordt alleen ondersteund voor VMs die zijn gecodeerd op gebouwen en geüpload naar Azure. Deze beperking is alleen van toepassing op de OS-schijf. codering van hoeveelheden gegevens voor een Linux VM wordt ondersteund.

De oplossing ondersteunt de volgende voor VMs IaaS voor openbare preview-versie wanneer ingeschakeld in Microsoft Azure:

-   Integratie met Azure sleutel kluis

-   Standaard- [A, D en G-serie IaaS VMs](https://azure.microsoft.com/pricing/details/virtual-machines/)

-   IaaS VMs gemaakt met behulp van [Bronbeheer Azure](../azure-resource-manager/resource-group-overview.md) model-codering inschakelen

-   Alle Azure openbare [gebieden](https://azure.microsoft.com/regions/)

Deze functie zorgt ervoor dat alle gegevens op de schijven van de virtuele machine is gecodeerd in rust in Azure opslag.

####<a name="resources"></a>Bronnen

-   [Azure schijf codering voor Windows en Linux IaaS virtuele Machines](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

    In dit artikel worden besproken de preview-versie van Azure schijfversleuteling en bevat een koppeling voor het downloaden van het witboek.

###<a name="comparison-of-azure-disk-encryption-sse-and-client-side-encryption"></a>Vergelijking van Azure schijfversleuteling, SSE en Client-Side-codering

####<a name="iaas-vms-and-their-vhd-files"></a>IaaS VMs en de VHD-bestanden

Voor schijven die worden gebruikt door IaaS VMs, beste Azure schijf codering gebruiken. SSE voor het coderen van de VHD-bestanden die worden gebruikt voor die schijven in Azure opslag back te schakelen, maar dat alleen onlangs geschreven gegevens worden gecodeerd. Dit betekent dat als u een VM maken en vervolgens SSE inschakelen op de opslag rekening met het VHD-bestand, worden alleen de wijzigingen worden gecodeerd, niet het oorspronkelijke VHD-bestand.

Als u een VM maken met behulp van een afbeelding uit de markt Azure Azure voert een [oppervlakkige kopie](https://en.wikipedia.org/wiki/Object_copying) van de afbeelding aan uw account voor opslag in Azure opslag en niet zelfs als u ingeschakeld SSE hebt is gecodeerd. Nadat de VM wordt gemaakt en het bijwerken van de afbeelding begint, start SSE coderen van de gegevens. Om deze reden is het beste Azure schijf codering gebruikt voor VMs gemaakt op basis van afbeeldingen op Marketplace Azure als u wilt dat ze volledig zijn gecodeerd.

Als u een vooraf gecodeerde VM in Azure op ruimten, wordt u mogelijk de coderingssleutels uploaden naar Azure sleutel kluis en gaat u verder met de codering voor die VM dat u op het bedrijf. Azure schijf codering is ingeschakeld voor het verwerken van dit scenario.

Als u niet-gecodeerde VHD in ruimten, kunt u uploaden naar de galerie als een aangepaste afbeelding en een VM uit het inrichten. Als u dit met de Resource Manager-sjablonen doet, kunt u deze in te schakelen wanneer het wordt opgestart om de VM Azure schijfversleuteling vragen.

Wanneer u een gegevensschijf toevoegen en op de VM te koppelen, kunt u op Azure schijf codering op die gegevensschijf. Eerst op de gegevensschijf lokaal zal worden versleuteld en vervolgens de service management laag doet een fikse schrijven tegen opslag zodat de inhoud van de opslag is gecodeerd.

####<a name="client-side-encryption"></a>Client-side-codering####

Client-side-versleuteling is de veiligste methode van het coderen van uw gegevens, omdat het codeert het voor communautair douanevervoer en de gegevens in rust codeert. Het is echter vereist dat u code toevoegt aan uw toepassingen gebruikmaken van opslag die u niet wilt doen. In dat geval kunt u HTTPs voor uw gegevens in transit en SSE voor het coderen van de gegevens in rust.

U kunt tabel entiteiten, berichten en BLOB's coderen met client-side-codering. U kunt alleen BLOB's met SSE coderen. Als u tabel- en gegevens moeten worden gecodeerd, moet u client-side-codering gebruiken.

Client-side codering wordt volledig beheerd door de toepassing. Dit is de veiligste methode, maar moet u via programmacode wijzigingen aanbrengt in uw toepassing en de belangrijkste processen die. Wanneer u de extra beveiligen tijdens de overdracht wilt en de opgeslagen gegevens worden versleuteld, zou u dit gebruiken.

Client-side-codering is meer belasting op de client en er rekening mee houden in de plannen voor schaalbaarheid, vooral als u zijn gecodeerd en een grote hoeveelheid gegevens.

####<a name="storage-service-encryption-sse"></a>Opslag Service codering (SSE)

SSE wordt door Azure opslag beheerd. Met behulp van SSE voorziet niet in voor de beveiliging van de gegevens tijdens de overdracht, maar het coderen van de gegevens zoals deze wordt weggeschreven naar Azure opslag. Er is geen invloed op de prestaties bij het gebruik van deze functie.

U kunt alleen blok BLOB's coderen BLOB's toevoegen en pagina-BLOB's met SSE. Als u gegevens in een tabel of de wachtrijgegevens coderen, moet u rekening houden met client-side-codering.

Hebt u een archief of bibliotheek VHD-bestanden die u als basis gebruiken voor het maken van nieuwe virtuele machines, kunt u een nieuwe account voor de opslag, SSE inschakelen en vervolgens de VHD-bestanden uploaden naar dat account. De VHD-bestanden worden gecodeerd door Azure opslag.

Hebt u Azure schijf codering is ingeschakeld voor de schijven in een VM en SSE ingeschakeld op de opslag rekening houden de VHD-bestanden, werkt dit prima; Dit resulteert in een zojuist geschreven gegevens twee keer worden gecodeerd.

##<a name="storage-analytics"></a>Analytics voor opslag

###<a name="using-storage-analytics-to-monitor-authorization-type"></a>Met behulp van Analytics opslag type vergunning controleren

U kunt voor elke rekening opslag Azure opslag Analytics registratie uitvoeren en metrische gegevens op te slaan inschakelen. Dit is een geweldig hulpmiddel om te gebruiken als u wilt controleren de prestatiegegevens van een opslag of problemen met een opslag account omdat er problemen met de prestaties.

Een ander gedeelte van de gegevens die u in de opslag analytics logboeken bekijken kunt wordt de verificatiemethode die door iemand wordt gebruikt wanneer zij toegang hebben tot opslag. Bijvoorbeeld met een Blob-opslag ziet u als ze een gedeelde Access-handtekening of de opslag-account gebruikt, of als de blob toegankelijk openbaar is.

Dit is heel handig als u toegang tot opslag strak zijn beveiligen. Bijvoorbeeld in de Blob-opslag kunt u stelt u alle van de containers op persoonlijke en het gebruik van een SAS service implementeren in uw toepassingen. Vervolgens kunt u de logboeken regelmatig om te zien als de BLOB's worden geopend met behulp van de sleutels van de account opslag, die op een schending van de beveiliging duiden kunnen, of als de BLOB's openbaar zijn, maar ze hoeft niet controleren.

####<a name="what-do-the-logs-look-like"></a>Hoe zien de logboeken eruit?

Nadat u het maatstelsel opslag account inschakelen en registreren via de portal Azure, analytics-gegevens snel worden gestart. Is de registratie en metrics voor elke service afzonderlijk; de logboekregistratie is opgesteld als er activiteit in die account opslag terwijl elke minuut, elk uur of dagelijks, afhankelijk van hoe u deze configureert de parameters wordt geregistreerd.

De logboekbestanden worden opgeslagen in de BLOB's blokkeren in een container met de naam $logs in de opslag. Deze container wordt automatisch gemaakt wanneer opslag Analytics is ingeschakeld. Zodra deze container wordt gemaakt, verwijderen u deze niet, maar u kunt de inhoud ervan verwijderen.

Er is een map voor elke dienst onder de container $logs en submappen er voor het jaar/maand/dag/uur te zijn. Onder het uur, worden de logboeken te genummerd. Dit is de directory-structuur bekijken:

![Weergave van logboekbestanden](./media/storage-security-guide/image1.png)

Elke aanvraag van Azure opslag is geregistreerd. Hier is een momentopname van een logboekbestand met de eerste paar velden.

![Momentopname van een logboekbestand](./media/storage-security-guide/image2.png)

Ziet u dat kunt u de logboekbestanden voor het bijhouden van elke soort oproepen naar een opslag.

####<a name="what-are-all-of-those-fields-for"></a>Wat zijn alle van deze velden voor?

Er is een artikel dat wordt vermeld in de onderstaande die bronnen vindt u de lijst met veel velden in de logboeken en waarvoor ze gebruikt. Hier is de lijst met velden in de volgorde:

![Momentopname van velden in een logboekbestand](./media/storage-security-guide/image3.png)

We bent geïnteresseerd in de posten voor GetBlob en hoe ze worden geverifieerd, dus we moeten controleren-status van de aanvraag (4<sup>th</sup> kolom) en de autorisatie-type (<sup>de</sup> kolom 8) en zoek naar vermeldingen met het type bewerking 'Get '-Blob.

Bijvoorbeeld in de eerste paar rijen in de bovenstaande lijst de status van de aanvraag is "Geslaagd" en de vergunning van het type 'geverifieerd'. Dit betekent dat de aanvraag werd gevalideerd met behulp van de sleutel opslag account.

####<a name="how-are-my-blobs-being-authenticated"></a>Hoe worden mijn blobs geverifieerd?

Wij hebben drie gevallen waarin we geïnteresseerd bent.

1.  De blob is openbaar en het is toegankelijk via een URL zonder een gedeelde Access-handtekening. In dit geval de status van de aanvraag is "AnonymousSuccess" en de vergunning van het type 'anoniem' is.

    1.0; 2015-11-17T02:01:29.0488963Z; GetBlob; **AnonymousSuccess**; 200; 124; 37; **anonieme**; mystorage...

2.  De blob is persoonlijk en met een gedeelde Access-handtekening is gebruikt. In dit geval de status van de aanvraag is "SASSuccess" en de vergunning van het type 'sas' is.

    1.0; 2015-11-16T18:30:05.6556115Z; GetBlob; **SASSuccess**; 200; 416; 64; **sas**; mystorage...

3.  De blob is persoonlijk en de opslag sleutel werd gebruikt om het te openen. In dit geval-de status van de "**succes**" is en de autorisatie-type is '**geverifieerd**'.

    1.0; 2015-11-16T18:32:24.3174537Z; GetBlob; **Succes**; 206; 59; 22; **geverifieerd**; mystorage...

U kunt het bericht Microsoft Analyzer weergeven en analyseren van deze logboeken. Dit omvat mogelijkheden voor zoeken en filteren. Bijvoorbeeld, misschien wilt u zoeken naar exemplaren van de GetBlob om te zien of het gebruik van wat u verwacht, dat wil zeggen om ervoor te zorgen dat iemand geen toegang heeft tot uw account opslag wel.

####<a name="resources"></a>Bronnen

-   [Analytics voor opslag](storage-analytics.md)

    Dit artikel is een overzicht van opslag analytics en hoe u deze inschakelen.

-   [Opslag Analytics logboekbestandsindeling](https://msdn.microsoft.com/library/azure/hh343259.aspx)

    In dit artikel ziet u de indeling voor logboekbestanden Analytics opslag en een gedetailleerd overzicht van de beschikbare velden, met inbegrip van verificatie van het type, die welk type verificatie dat wordt gebruikt voor de aanvraag aangeeft.

-   [Een Account voor opslag in de portal Azure controleren](storage-monitor-storage-account.md)

    In dit artikel ziet u hoe u parameters prestatiecontrole en -registratie voor een account opslag configureren.

-   [End-to-End probleemoplossing met Azure opslag maatstaven en logboekregistratie, AzCopy en bericht Analyzer](storage-e2e-troubleshooting.md)

    In dit artikel gesprekken voert over het oplossen van problemen met de Analytics opslag en laat zien hoe met de Microsoft Message Analyzer.

-   [Microsoft Message Analyzer operationele handleiding](https://technet.microsoft.com/library/jj649776.aspx)

    In dit artikel wordt de verwijzing naar de Microsoft Message Analyzer en bevat koppelingen naar een zelfstudie, het snel starten en de functie overzicht.

##<a name="cross-origin-resource-sharing-cors"></a>Cross-oorsprong Resource Sharing (CORS)

###<a name="cross-domain-access-of-resources"></a>Domeintoegang van resources

Wanneer een webbrowser in één domein met een HTTP-aanvraag voor een resource uit een ander domein, heet dit een cross-oorsprong HTTP-verzoek. Een HTML-pagina van contoso.com bediend maakt bijvoorbeeld een aanvraag voor een JPEG-bestand gehost op fabrikam.blob.core.windows.net. Cross-oorsprong HTTP-aanvragen die afkomstig zijn van in scripts, zoals JavaScript, om veiligheidsredenen browsers beperken. Dit betekent dat wanneer sommige JavaScript-code van een webpagina op contoso.com die jpeg op fabrikam.blob.core.windows.net aanvraagt, de browser, kan de aanvraag.

Wat heeft dit te maken met Azure opslag Ook als u statische elementen zoals JSON- of XML-gegevens opslaat in Blob-opslag met een opslag account Fabrikam genoemd, het domein voor de activa worden fabrikam.blob.core.windows.net en worden ze toegang krijgen tot de webtoepassing contoso.com niet met JavaScript, omdat de domeinen anders zijn. Dit is ook het geval als u probeert te bellen op een van de opslag Azure Services – zoals tabelopslag – die gegevens worden verwerkt door de client JavaScript JSON retourneren.

####<a name="possible-solutions"></a>Mogelijke oplossingen

Een manier om dit probleem oplossen is een aangepast domein zoals "storage.contoso.com" toewijzen aan fabrikam.blob.core.windows.net. Het probleem is dat u alleen aangepaste domein aan account een opslag toewijzen kunt. Wat gebeurt er als de activa worden opgeslagen in meerdere accounts voor opslag?

Een andere manier voor het oplossen van dit is dat de webtoepassing die als proxy voor het aanroepen van de opslag. Dit betekent dat als u een bestand naar een Blob-opslag uploadt, de webtoepassing zou het lokaal te schrijven en vervolgens te kopiëren naar een Blob-opslag of het zou alles in het geheugen gelezen en vervolgens op te schrijven naar een Blob-opslag. U kunt ook een specifieke web-toepassing (zoals een Web-API) dat de bestanden lokaal en schrijft deze naar een Blob-opslag schrijven. In beide gevallen hebt u voor die functie account nodig om de schaalbaarheid te bepalen.

####<a name="how-can-cors-help"></a>Hoe kunt CORS

Azure opslag kunt u met CORS – Cross-oorsprong resources delen. Voor elke rekening opslag kunt u domeinen die toegang heeft tot de bronnen in die account opslag. We kunnen in ons geval die hierboven worden beschreven, bijvoorbeeld CORS op de account fabrikam.blob.core.windows.net opslag inschakelen en configureren voor het toestaan van toegang tot contoso.com. Vervolgens de web application contoso.com rechtstreeks toegang tot de bronnen in de fabrikam.blob.core.windows.net.

Één ding om te onthouden is dat CORS toegang toestaat, maar biedt geen verificatie vereist voor alle niet-toegang van het publiek van opslagbronnen is. Dit betekent dat u kunt alleen toegang tot BLOB's als zij openbaar zijn of u moet een gedeelde Access handtekening worden voorzien zodat u de juiste machtiging beschikt. Tabellen, wachtrijen en bestanden hebben geen toegang van het publiek en vereisen een SAS.

CORS is standaard uitgeschakeld op alle services. CORS kunt u met behulp van de REST API of de clientbibliotheek opslag aan te roepen van een van de methoden om de beleidsregels van de service. Als u dat doet, kunt u een regel CORS, die XML-opnemen. Hier volgt een voorbeeld van een regel die is ingesteld in de werking van de Service-eigenschappen instellen voor de Blob-Service voor een opslagruimte voor CORS. U kunt met behulp van de client-bibliotheek van opslag of de REST API's voor opslag van Azure bewerking uitvoeren.

    <Cors>    
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
            <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
            <MaxAgeInSeconds>200</MaxAgeInSeconds>
        </CorsRule>
    <Cors>

Dit is wat elke rij betekent:

-   **AllowedOrigins** Dit geeft aan welke domeinen niet-overeenkomende kunnen aanvragen en ontvangen van gegevens van de storage-service. Dit zegt dat contoso.com en fabrikam.com gegevens uit de Blob-opslag voor een specifieke opslag opvragen kunnen. U kunt dit ook instellen op een jokerteken (\*) zodat alle domeinen toegang aanvragen.

-   **AllowedMethods** Dit is de lijst met methoden (verbs http-request) die kunnen worden gebruikt bij het maken van de aanvraag. In dit voorbeeld wordt zijn alleen de PUT en GET toegestaan. U kunt dit instellen op een jokerteken (\*) zodat alle methoden worden gebruikt.

-   **AllowedHeaders** Dit is de aanvraagheaders die het oorspronkelijke domein opgeven kunt wanneer de aanvraag. In dit voorbeeld wordt zijn alle metagegevens headers beginnend met x-ms-metagegevens, x-ms-meta-doel en x-ms-meta-abc toegestaan. Het jokerteken (\*) geeft aan dat een kop die begint met het opgegeven voorvoegsel is toegestaan.

-   **ExposedHeaders** Dit geeft aan welke antwoordheaders worden blootgesteld door de browser naar de uitgever van de aanvraag. In dit voorbeeld wordt elke kop beginnen met "x-ms - meta-" zullen worden blootgesteld.

-   **MaxAgeInSeconds** Dit is de maximale tijdsduur dat een browser de Preflight-opties verzoek cache. (Controleer het eerste artikel hieronder voor meer informatie over de Preflight-aanvraag).

####<a name="resources"></a>Bronnen

Check deze bronnen voor meer informatie over CORS en over te schakelen.

-   [Cross-oorsprong Resource Sharing ondersteuning voor de van Azure opslagservices op Azure.com (CORS)](storage-cors-support.md)

    Dit artikel bevat een overzicht van CORS en het instellen van de regels voor de van andere opslagservices.

-   [Cross-oorsprong Resource Sharing ondersteuning voor de van Azure opslagservices op MSDN (CORS)](https://msdn.microsoft.com/library/azure/dn535601.aspx)

    Dit is de documentatie voor ondersteuning voor de opslag Azure Services CORS. Deze bevat koppelingen naar artikelen voor elke opslagservice en toont een voorbeeld en elk element in het bestand CORS wordt uitgelegd.

-   [Microsoft Azure opslag: Kennismaking CORS](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/02/03/windows-azure-storage-introducing-cors.aspx)

    Dit is een koppeling naar het eerste blog artikel CORS aangekondigd en waarin wordt beschreven hoe u deze kunt gebruiken.

##<a name="frequently-asked-questions-about-azure-storage-security"></a>Veelgestelde vragen over de beveiliging van Azure opslag

1.  **Hoe kan ik verifiëren van de integriteit van de BLOB's die ik ben als ik het HTTPS-protocol niet gebruiken in- of Azure opslag overbrengen?**

    Als voor een of andere reden moet u HTTP gebruikt in plaats van HTTPS en u werkt met een BLOB's blokkeren, kunt u MD5 controleren om te controleren of de integriteit van de BLOB's worden overgebracht. Dit helpt de beveiliging van het netwerk/transport layer fouten, maar niet noodzakelijkerwijs met tussenliggende aanvallen.

    Als HTTPS transportbeveiliging biedt, kunt u is vervolgens met behulp van MD5 controle overbodig en onnodig.
    
    Check de [Azure Blob MD5-overzicht](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/02/18/windows-azure-blob-md5-overview.aspx)voor meer informatie.

2.  **Hoe zit het met FIPS-naleving van de Amerikaanse regering?**

    De Verenigde Staten Federal Information Processing Standard (FIPS) definieert de cryptografische algoritmen die zijn goedgekeurd voor gebruik door de Amerikaanse federale overheidssystemen voor de bescherming van gevoelige gegevens. Inschakelen van FIPS vertelt-modus op een Windows-server of desktop de OS dat cryptografische algoritmen alleen FIPS gevalideerd moeten worden gebruikt. Als een toepassing niet-compatibele algoritmen gebruikt, worden de toepassingen worden verbroken. Versies With.NET Framework 4.5.2 of hoger, de toepassing automatisch wordt geschakeld tussen de cryptografische algoritmen om FIPS-compatibele algoritmen gebruiken wanneer de computer in de FIPS-modus.

    Microsoft blijft tot elke klant om te bepalen of de FIPS-modus inschakelen. Wij denken dat er geen dwingende redenen zijn voor klanten die niet onderworpen aan de wettelijke voorschriften zijn met de FIPS-modus standaard ingeschakeld is.

    **Bronnen**

-   [Waarom We nu niet aanbevelen 'FIPS-modus' niet meer](http://blogs.technet.com/b/secguide/archive/2014/04/07/why-we-re-not-recommending-fips-mode-anymore.aspx)

    Dit blog-artikel geeft een overzicht van FIPS en wordt uitgelegd waarom zij de FIPS-modus standaard niet ingeschakeld.

-   [FIPS 140 validatie](https://technet.microsoft.com/library/cc750357.aspx)

    Dit artikel bevat informatie over hoe Microsoft-producten en cryptografische modules in overeenstemming zijn met de FIPS-standaard voor de Amerikaanse federale overheid.

-   [' Systeemcryptographie: gebruik FIPS compliant algorithmes voor codering, hashing en ondertekening ' beveiligingsrisico van instellingen in Windows XP en nieuwere versies van Windows](https://support.microsoft.com/kb/811833)

    In dit artikel gesprekken voert over het gebruik van de FIPS-modus in oudere Windows-computers.
