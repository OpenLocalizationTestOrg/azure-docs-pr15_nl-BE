<properties
   pageTitle="Azure AD verbinden: Concepten ontwerpen | Microsoft Azure"
   description="In dit onderwerp worden bepaalde gebieden implementatie-ontwerp"
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.custom = "azure-ad-connect"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Identity"
   ms.date="09/13/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-design-concepts"></a>Azure AD verbinden: Ontwerpconcepten
Het doel van dit onderwerp is voor het beschrijven van de gebieden die bij het implementatieontwerp van Azure AD verbinden via moeten worden beschouwd. In dit onderwerp wordt een grondige kennismaking op bepaalde gebieden en deze concepten worden kort beschreven in andere onderwerpen ook.

## <a name="sourceanchor"></a>sourceAnchor
Het kenmerk sourceAnchor wordt gedefinieerd als *een kenmerk onveranderbare gedurende de levensduur van een object*. Deze is uniek voor een object dat u hetzelfde object op-pand en in Azure AD. Het kenmerk wordt ook **immutableId** genoemd en de twee namen onderling verwisselbaar worden gebruikt.

Het woord onveranderbaar zijn, dat is "kan niet worden gewijzigd", is het belangrijk dat dit onderwerp. Aangezien de waarde van dit kenmerk kan niet worden gewijzigd nadat deze is ingesteld, is het belangrijk om te kiezen van een ontwerp dat uw scenario ondersteunt.

Het kenmerk wordt gebruikt voor de volgende scenario's:

- Wanneer een nieuwe synchronisatie-engine-server wordt gemaakt of opnieuw opgebouwd na het herstellen van fouten, koppelt u dit kenmerk bestaande objecten in Azure AD met objecten op ruimten.
- Als u een identiteit alleen cloud model gesynchroniseerde identiteit, kan dit kenmerk 'harde overeenkomst' bestaande objecten objecten in Azure AD met objecten voor gebouwen.
- Als u federation gebruikt, is wordt dit kenmerk met de **userPrincipalName** gebruikt in de claim als unieke identificatie voor een gebruiker.

In dit onderwerp alleen gesprekken voert over sourceAnchor met betrekking tot gebruikers. Dezelfde regels van toepassing op alle objecttypen, maar is alleen voor gebruikers van dat dit probleem is meestal een probleem.

### <a name="selecting-a-good-sourceanchor-attribute"></a>Het kenmerk van een goede sourceAnchor te selecteren
Waarde van het kenmerk moet voldoen aan de volgende regels:

- Minder dan 60 tekens lang
    - Tekens die niet worden a-z, A-Z of 0-9 zijn gecodeerd en als 3 tekens geteld
- Geen speciale tekens bevatten: & #92;! # $ % & * + / = ? ^ & #96; { } | ~ haken () "; : , [ ] " @ _
- Globaal uniek zijn
- Moet een tekenreeks, getal of binair
- Moet niet worden gebaseerd op de naam van de gebruiker, deze wijzigingen
- Moet niet hoofdlettergevoelig en moet voorkomen dat waarden die per geval verschillen kunnen
- Moet worden toegewezen wanneer het object is gemaakt.

Als het geselecteerde sourceAnchor is niet van het type string, vervolgens Azure AD verbinding Base64Encode de waarde van het kenmerk om ervoor te zorgen geen speciale tekens weergegeven. Als u de federation-server van een andere dan de AD FS gebruikt, moet de server kan ook Base64Encode het kenmerk.

Het kenmerk sourceAnchor is hoofdlettergevoelig. Een waarde van "JanSmit" is niet hetzelfde als "JanSmit". Maar u moet niet twee verschillende objecten met alleen een verschil in kwestie.

Als u één forest is op lokalen, vervolgens het kenmerk moet u **objectGUID**. Dit is ook het kenmerk gebruikt wanneer u express-instellingen in Azure AD verbinden en ook het kenmerk dat wordt gebruikt door DirSync.

Als u meerdere forests en gebruikers niet tussen forests en domeinen verplaatsen, is **objectGUID** te gebruiken in dit geval zelfs een goede kenmerk.

Als u gebruikers tussen forests en domeinen verplaatsen, moet vervolgens u zoeken een kenmerk dat niet verandert, of kan worden verplaatst met de gebruikers tijdens de verplaatsing. Een benadering aanbevolen is een synthetische kenmerk introduceren. Een kenmerk dat iets kan bevatten die eruit als een GUID ziet zouden geschikt zijn. Een nieuwe GUID wordt gemaakt en gestempeld op de gebruiker tijdens het maken van een object. Een aangepaste synchronisatie-regel kan worden gemaakt in de synchronisatie-engine server deze waarde op basis van de **kenmerken objectGUID** maken en bijwerken van het geselecteerde kenmerk in ADDS. Wanneer u het object verplaatst, moet u ook de inhoud van deze waarde gekopieerd.

Een andere oplossing is het kiezen van een bestaand kenmerk dat u wijzigt niet kent. Algemeen gebruikte kenmerken omvatten de **werknemer-id**. Als u rekening houden met een kenmerk dat letters bevat, zorg ervoor dat er dat geen kans op het geval (hoofdletters en kleine letters) voor de waarde van het kenmerk kunt wijzigen. Ongeldige kenmerken die niet worden gebruikt zijn die kenmerken door de naam van de gebruiker. In een huwelijk of echtscheiding, de naam verwacht te wijzigen, hetgeen niet is toegestaan voor dit kenmerk. Dit is ook een reden waarom kenmerken zoals **userPrincipalName**, **e-mail**en **targetAddress** zijn niet zelfs mogelijk om te selecteren in de installatiewizard Azure AD verbinden. Deze kenmerken bevatten ook de @-character, die niet is toegestaan in de sourceAnchor.

### <a name="changing-the-sourceanchor-attribute"></a>Het kenmerk sourceAnchor te wijzigen
De waarde van het kenmerk sourceAnchor kan niet worden gewijzigd nadat het object is gemaakt in Azure AD en de identiteit wordt gesynchroniseerd.

Daarom gelden de volgende beperkingen op Azure AD Connect:

- Het kenmerk sourceAnchor kan alleen worden ingesteld tijdens de eerste installatie. Als u de installatiewizard opnieuw uitvoeren, is deze optie alleen-lezen. Als u deze instelling wijzigt, moet u verwijderen en opnieuw installeren.
- Als u een andere Azure AD Connect-server hebt geïnstalleerd, selecteert u hetzelfde sourceAnchor kenmerk zoals al eerder is gebruikt. Als u eerder hebt gebruikt als DirSync en Azure AD verbinding te verplaatsen, moet u **objectGUID** gebruiken omdat dit het kenmerk dat wordt gebruikt door DirSync.
- Als de waarde voor sourceAnchor is gewijzigd nadat is het object geëxporteerd naar Azure AD, vervolgens Azure AD verbinden synchronisatie een fout genereert en kan geen wijzigingen meer op dat object voordat het probleem is opgelost en de sourceAnchor wordt gewijzigd in de bronmap.

## <a name="azure-ad-sign-in"></a>Azure AD aanmelden
Terwijl de map op locatie integratie met Azure Active Directory, is het belangrijk dat u begrijpt hoe de synchronisatie-instellingen invloed hebben op de manier waarop gebruikers worden geverifieerd. Azure AD wordt userPrincipalName (UPN) gebruikt om de gebruiker te verifiëren. Bij het synchroniseren van uw gebruikers, moet u het kenmerk moet worden gebruikt voor de waarde van userPrincipalName zorgvuldig kiezen.

### <a name="choosing-the-attribute-for-userprincipalname"></a>Het kenmerk voor userPrincipalName kiezen
Wanneer u selecteert het kenmerk voor het verstrekken van zorg de waarde van een UPN moet worden gebruikt in Azure een

- Waarden van het kenmerk voldoet aan de UPN-syntaxis (RFC 822), moet de indeling van hetusername@domain
- Het achtervoegsel in de waarden komt overeen met een van de gecontroleerde aangepaste domeinen in Azure AD

In de express-instellingen is de veronderstelde keuze voor het kenmerk userPrincipalName. Als het kenmerk userPrincipalName niet de waarde bevat u wilt dat gebruikers zich aanmelden bij Azure, dan moet u **Aangepaste installatie**.

### <a name="custom-domain-state-and-upn"></a>Aangepaste domein staat en UPN
Het is belangrijk om ervoor te zorgen dat er een geverifieerde domein voor het UPN-achtervoegsel is.

John is een gebruiker in contoso.com. U wilt Jan de UPN in gebouwen te gebruiken john@contoso.com aan te melden bij Azure nadat u gebruikers hebt gesynchroniseerd met uw directory Azure AD contoso.onmicrosoft.com. Hiervoor moet u toevoegen en contoso.com controleren als een aangepast domein in Azure AD voordat u kunt synchroniseren van de gebruikers. Als het UPN-achtervoegsel van Jan, bijvoorbeeld contoso.com, komt niet overeen met een gecontroleerde domein in Azure AD, vervolgens vervangen Azure AD door het UPN-achtervoegsel contoso.onmicrosoft.com.

### <a name="non-routable-on-premises-domains-and-upn-for-azure-ad"></a>Niet-routeerbaar op ruimten domeinen en UPN voor Azure advertentie
Sommige organisaties hebben een niet-routeerbaar domeinen, zoals domeinen zoals contoso eenvoudig één etiket of contoso.local. U bent niet kunnen verifiëren of een niet-routeerbaar domein in Azure AD. Azure AD Connect kunt synchroniseren met alleen een geverifieerde domein in Azure AD. Wanneer u een map Azure AD maakt, wordt een routeerbaar-domein standaarddomein voor uw Azure AD bijvoorbeeld contoso.onmicrosoft.com wordt gemaakt. Daarom is het noodzakelijk geworden om te controleren of elk ander domein in een dergelijke situatie geschikt in het geval u niet wilt synchroniseren met het standaarddomein van onmicrosoft.com.

Lees [uw aangepaste domeinnaam naar Azure Active Directory toevoegen](active-directory-add-domain.md) voor meer informatie over het toevoegen van en het verifiëren van domeinen.

Azure AD verbinding gedetecteerd als u in een niet-routeerbaar domeinomgeving en op de juiste wijze waarschuwen zou uit wilt doorgaan met de express-instellingen. Als u in een niet-routeerbaar domein werkt, is het waarschijnlijk dat de UPN van de gebruikers niet-routeerbaar achtervoegsels te hebben. Bijvoorbeeld als u onder contoso.local uitvoert, stelt Azure AD verbinding maken u de aangepaste instellingen in plaats van met behulp van express-instellingen gebruiken. Met aangepaste instellingen, kan u het kenmerk dat moet worden gebruikt als UPN aan te melden bij Azure nadat de gebruikers zijn gesynchroniseerd met Azure AD opgeven.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md).
