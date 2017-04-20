<properties 
    pageTitle="Integratie tussen Azure meerledige verificatie en Active Directory"
    description="Dit is de pagina Azure meerledige verificatie die wordt beschreven hoe u de Azure meerledige verificatieserver integreren met Active Directory, zodat u de mappen kunt synchroniseren."
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

# <a name="directory-integration-between-azure-mfa-server-and-active-directory"></a>Integratie tussen Azure MVR gesloten Server en Active Directory

De Directory-integratie sectie kunt u de server configureren om te integreren met Active Directory of andere LDAP-directory.  U kunt kenmerken overeenkomen met het directory-schema en het instellen van automatische synchronisatie van gebruikers configureren.

## <a name="settings"></a>Instellingen
De Server Azure meerledige verificatie is standaard geconfigureerd om te importeren of gebruikers uit Active Directory synchroniseren.  Het tabblad kunt u het standaardgedrag overschrijven en binden aan een andere LDAP-directory, een ADAM-map of specifieke Active Directory-domeincontroller.  Het biedt ook voor het gebruik van LDAP-verificatie naar LDAP-proxy of LDAP Bind als een doel voor RADIUS-verificatie vooraf voor IIS-verificatie of primaire verificatie voor User Portal.  De volgende tabel worden de afzonderlijke instellingen.

![Instellingen](./media/multi-factor-authentication-get-started-server-dirint/dirint.png)

| Functie | Beschrijving |
| ------- | ----------- |
| Active Directory gebruiken | Selecteer de optie Active Directory gebruikt Active Directory gebruiken voor het importeren en synchronisatie.  Dit is de standaardinstelling. <br>Opmerking: De computer moet lid zijn van een domein en u moet zich hebben aangemeld met een domeinaccount voor integratie met Active Directory goed te laten werken. |
| Vertrouwde domeinen bevatten | Selectievakje het selectievakje vertrouwde domeinen bevatten om de agent poging tot verbinding met domeinen vertrouwd door het huidige domein, een ander domein in het forest of domeinen die betrokken zijn bij een forest-vertrouwensrelatie.  Wanneer niet importeren of de synchronisatie van gebruikers uit vertrouwde domeinen, schakel het selectievakje uit om prestaties te verbeteren.  De standaard is ingeschakeld. |
| Gebruik specifieke LDAP-configuratie | Selecteer de optie Gebruik LDAP gebruiken de LDAP-instellingen opgegeven voor het importeren en synchronisatie. Opmerking: Als Gebruik LDAP is geselecteerd, verandert de gebruikersinterface verwijzingen uit Active Directory met LDAP. |
| Knop bewerken | De knop bewerken kunt de huidige LDAP configuratie-instellingen gewijzigd. |
| Kenmerk scope query's gebruiken | Hiermee wordt aangegeven of het kenmerk scope query's moeten worden gebruikt.  Kenmerk scope query's toestaan voor zoekopdrachten efficiënte directory records op basis van de posten in het kenmerk van een andere record in aanmerking komt.  De Azure meerledige verificatieserver gebruikt attribuut scope query's efficiënt query uitvoeren op de gebruikers die lid zijn van een beveiligingsgroep.   <br>Opmerking: Er zijn bepaalde gevallen waar kenmerk scope query's worden ondersteund, maar niet moeten worden gebruikt.  Active Directory kan bijvoorbeeld problemen met het kenmerk scope query's hebben als een groep leden uit meer dan één domein bevat.  In dit geval het selectievakje worden uitgeschakeld. |

De volgende tabel beschrijft de LDAP-configuratie-instellingen.

| Functie | Beschrijving |
| ------- | ----------- |
| Server | Voer de hostnaam of het IP-adres van de server met de LDAP-directory.  Een back-upserver kan ook worden opgegeven, gescheiden door een puntkomma. <br>Opmerking: Als Type binden SSL is een volledig gekwalificeerde hostnaam is meestal nodig. |
| Base DN | Geef de DN-naam van de basismap van het object waaruit alle directory-query's wordt gestart.  Bijvoorbeeld, dc = abc, dc = com. |
| Type - query's koppelen | Selecteer het juiste binding voor gebruik bij het binden van de LDAP-adreslijst zoeken.  Dit wordt gebruikt voor invoer, synchronisatie en resolutie van de gebruikersnaam. <br><br>  Anoniem - een anonieme binding wordt uitgevoerd.  BIND DN-naam en wachtwoord binden wordt niet gebruikt.  Dit werkt alleen als de LDAP-directory kan anonieme binding en machtigingen toestaan de query de gewenste records en kenmerken.  <br><br> Eenvoudig - Bind DN-naam en wachtwoord binden wordt doorgegeven als tekst zonder opmaak binding met de LDAP-directory.  Dit moet alleen worden gebruikt voor testdoeleinden gebruikt om te controleren of dat de server kan worden bereikt en dat de account afhankelijk van de juiste toegang heeft.  Het verdient aanbeveling dat SSL moet worden gebruikt in plaats daarvan nadat het juiste certificaat is geïnstalleerd.  <br><br> SSL - binding DN-naam en wachtwoord binden wordt gecodeerd met SSL binding met de LDAP-directory.  Hiervoor is vereist dat een certificaat lokaal worden geïnstalleerd dat de LDAP-directory-vertrouwensrelaties.  <br><br> Windows - Bind gebruikersnaam en wachtwoord binden wordt veilig verbinding maken met een Active Directory-domeincontroller of de ADAM-map gebruikt.  Als binden gebruikersnaam leeg is, wordt de aangemelde gebruikersaccount worden gebruikt voor binding. |
| Type - verificaties binden | Selecteer het juiste binding voor gebruik bij het uitvoeren van de LDAP bind-verificatie.  Zie de beschrijvingen van de Bind-type - query's typt bind.  Dit kan bijvoorbeeld voor anonieme binding voor query's worden gebruikt, terwijl SSL-binding wordt gebruikt voor beveiligde LDAP bind verificaties. |
| BIND DN-naam of gebruikersnaam Bind | Geef de DN-naam van de gebruikersrecord voor de account moet worden gebruikt wanneer de binding met de LDAP-directory.<br><br>De DN-naam van bind wordt alleen gebruikt als binden eenvoudig of SSL is.  <br><br>Voer de gebruikersnaam van de Windows-account moet worden gebruikt bij het binden wordt het als Windows-binding met de LDAP-directory.  Als het argument leeg is, wordt de aangemelde gebruikersaccount worden gebruikt voor binding. |
| Wachtwoord binden | Voer het wachtwoord van de binding voor het binden DN-naam of gebruikersnaam worden gebruikt voor binding aan de LDAP-directory.  Voor meer informatie over het configureren van het wachtwoord voor de Service meerledige verificatie Server AdSync synchronisatie moet worden ingeschakeld en de service moet worden uitgevoerd op de lokale computer.  Het wachtwoord wordt opgeslagen in de Windows opgeslagen gebruikersnamen en wachtwoorden met de account die de AdSync meerledige verificatie Server-Service wordt uitgevoerd als.  Onder de account die als de gebruikersinterface meerledige verificatie Server wordt uitgevoerd en de account die het meerledige verificatie Server-Service wordt uitgevoerd als worden ook het wachtwoord opgeslagen.  <br><br> Opmerking: Aangezien het wachtwoord alleen in de lokale server Windows opgeslagen gebruikersnamen en wachtwoorden opgeslagen wordt, deze stap moet worden uitgevoerd op elke meerledige verificatie Server die toegang tot het wachtwoord nodig heeft. |
| Limiet voor query | Hiermee geeft u de limiet voor het maximum aantal gebruikers dat een directory-zoekactie retourneert.  Deze limiet moet overeenkomen met de configuratie op de LDAP-directory.  Voor grote zoekopdrachten waarbij paginering wordt niet ondersteund, probeert importeren en synchronisatie op te halen van gebruikers in batches.  Als de limiet voor de opgegeven hier groter is dan de limiet die is geconfigureerd op de LDAP-adreslijst, kunnen sommige gebruikers worden overgeslagen. |
| Knop testen | Klik op de knop testen om te testen, binding met de LDAP-server.  <br><br> Opmerking: De optie Gebruik LDAP niet hoeft te worden geselecteerd om te testen van de binding.  Hierdoor kan de binding te testen voordat u de configuratie van LDAP. |

## <a name="filters"></a>Filters
Met filters kunt u criteria records in aanmerking bij het uitvoeren van een zoekopdracht voor de map instellen.  Door het instellen van het filter kunt u het bereik van de objecten die u wilt synchroniseren.  

![Filters](./media/multi-factor-authentication-get-started-server-dirint/dirint2.png)

Azure meerledige verificatie heeft de volgende 3 opties.

- **Container-filter** - filtercriteria gebruikt om records van de container te kwalificeren bij het uitvoeren van een zoekopdracht voor de map opgeven.  Voor Active Directory en ADAM, (| () objectClass=organizationalUnit)(objectClass=container)) wordt in het algemeen gebruikt.  Voor andere LDAP-directory's moeten de criteria die in aanmerking komt elk type container-object afhankelijk van de directory-schema worden gebruikt.  <br>Opmerking: Als u leeg laten ((objectClass=organizationalUnit)(objectClass=container)) wordt standaard gebruikt.

- **Groep beveiligingsfilter** - Geef de filtercriteria security groepsrecords in aanmerking bij het uitvoeren van een zoekopdracht voor de map.  Voor Active Directory en ADAM, (&(objectCategory=group) (groupType:1.2.840.113556.1.4.804:=-2147483648)) over het algemeen wordt gebruikt.  Voor andere LDAP-directory's moeten de criteria die in aanmerking komt elk type security group-object afhankelijk van de directory-schema worden gebruikt.  <br>Opmerking: Als u leeg laten (&(objectCategory=group) (groupType:1.2.840.113556.1.4.804:=-2147483648)) wordt standaard gebruikt.

- **Gebruikersfilter** - de filtercriteria gebruikt om gebruikersrecords te kwalificeren bij het uitvoeren van een zoekopdracht voor de map opgeven.  Voor Active Directory en ADAM, (& (objectClass=user)(objectCategory=person)) in het algemeen wordt gebruikt.  Voor andere LDAP-directory's (objectClass = inetOrgPerson) of iets dergelijks afhankelijk van het directory-schema moet worden gebruikt. <br>Opmerking: Als leeg, (& (objectCategory=person)(objectClass=user)) wordt standaard gebruikt.

## <a name="attributes"></a>Kenmerken
Kenmerken kunnen worden aangepast als dat nodig is voor een specifieke map.  Hiermee kunt u aangepaste kenmerken toevoegen en afstellen van de synchronisatie worden alleen de kenmerken die u nodig hebt.  De waarde voor elk kenmerkveld moet de naam van het kenmerk, zoals gedefinieerd in het directory-schema.  Gebruik de onderstaande tabel voor meer informatie.

![Kenmerken](./media/multi-factor-authentication-get-started-server-dirint/dirint3.png)

| Functie | Beschrijving |
| ------- | ----------- |
| De unieke id | Voer de naam van het kenmerk van het kenmerk dat als de unieke id van de container, groep en gebruikersrecords fungeert.  In Active Directory is dit meestal objectGUID.  In andere implementaties van LDAP mogelijk entryUUID of iets dergelijks.  De standaardwaarde is objectGUID. |
| ... Knoppen (kenmerk selecteren) | Elk kenmerkveld heeft een knop '...' ernaast die in het dialoogvenster kenmerk selecteren is zodat een kenmerk te selecteren in een lijst worden weergegeven. <br><br>Dialoogvenster kenmerk selecteren.<br><br>Opmerking: Kenmerken handmatig worden ingevoerd en hoeven niet overeenkomen met een kenmerk in de lijst met kenmerken. |
| De unieke id-type | Selecteer het type van de unieke id-kenmerk.  In Active Directory is de kenmerken objectGUID kenmerk van het type GUID.  In andere implementaties LDAP kan zijn van het type Byte-matrix ASCII- of tekenreeks.  De standaardwaarde is de GUID. <br><br>Opmerking: Het is belangrijk om dit goed ingesteld omdat Items voor synchronisatie door de unieke id wordt verwezen en de unieke id wordt gebruikt om het object direct zoeken in de map.  Als deze tekenreeks wanneer de map daadwerkelijk wordt de waarde opgeslagen als een matrix van bytes van ASCII-tekens voorkomen de synchronisatie dat wordt niet goed functioneren. |
| DN-naam | Voer de naam van het kenmerk van het kenmerk DN-naam voor elke record bevat.  In Active Directory is dit meestal distinguishedName.  In andere implementaties van LDAP mogelijk entryDN of iets dergelijks.  De standaardwaarde is distinguishedName. <br><br>Opmerking: Als een kenmerk dat alleen de DN-naam niet bestaat, het adspath kenmerk kan worden gebruikt.  De "LDAP: / /<server>/" gedeelte van het pad automatisch uitschakelen zodat alleen de DN-naam van het object worden verwijderd. |
| Naam van container | Voer de naam van het kenmerk van het kenmerk de naam van een record van de container bevat.  De waarde van dit kenmerk wordt weergegeven in de hiërarchie van de Container bij het importeren vanuit Active Directory of het toevoegen van items voor synchronisatie.  De standaardwaarde is de naam. <br><br>Opmerking: Als verschillende containers met verschillende kenmerken voor hun namen, meerdere container naam kenmerken kunnen worden opgegeven gescheiden door puntkomma's.  De eerste container kenmerk name gevonden op een container-object wordt gebruikt om de naam weer te geven. |
| Naam beveiligingsgroep | Voer de naam van het kenmerk van het kenmerk met de naam in een record voor beveiliging.  De waarde van dit kenmerk wordt weergegeven in de lijst groep bij het importeren vanuit Active Directory of het toevoegen van items voor synchronisatie.  De standaardwaarde is de naam. |
| Gebruikers | De volgende kenmerken worden gebruikt voor het zoeken, weergeven, importeren en synchroniseren van gebruikersgegevens uit de map. |
| Gebruikersnaam | Voer de naam van het kenmerk van het kenmerk met de gebruikersnaam in een gebruikersrecord.  De waarde van dit kenmerk wordt gebruikt als de gebruikersnaam meerledige verificatie Server.  Een tweede kenmerk kan worden opgegeven als een back-up naar de eerste.  Het tweede kenmerk wordt alleen gebruikt als het eerste kenmerk een waarde voor de gebruiker niet bevat.  De standaardwaarden zijn userPrincipalName en sAMAccountName. |
| Voornaam | Voer de naam van het kenmerk van het kenmerk de naam van de eerste in een gebruikersrecord bevat.  De standaardwaarde is givenName. |
| Achternaam | Voer de naam van het kenmerk van het kenmerk de naam van de laatste in een gebruikersrecord bevat.  De standaardwaarde is sn. |
| E-mailadres | Voer de naam van het kenmerk van het kenmerk het e-mailadres in een gebruikersrecord bevat.  E-mailadres wordt gebruikt om te verzenden, Welkom en e-mails voor de gebruiker.  De standaardwaarde is e. |
| Gebruikersgroep | Voer de naam van het kenmerk van het kenmerk met de gebruikersgroep in een gebruikersrecord.  Gebruikersgroep kan worden gebruikt om te bepalen of gebruikers in de agent en rapporten in de Portal meerledige verificatie Server Management. |
| Beschrijving | Voer de naam van het kenmerk van het kenmerk met de beschrijving in een gebruikersrecord.  Beschrijving wordt alleen gebruikt voor het zoeken.  De standaardwaarde is de beschrijving. |
| Voice call taal | Voer de naam van het kenmerk van het kenmerk met de korte naam van de taal die u wilt gebruiken voor gesprekken voor de gebruiker. |
| De taal van de SMS-tekst | Voer de naam van het kenmerk van het kenmerk met de korte naam van de taal die u wilt gebruiken voor SMS-berichten voor de gebruiker. |
| Telefoon app taal | Voer de naam van het kenmerk van het kenmerk met de korte naam van de taal voor de telefoon app berichten voor de gebruiker. |
| EDE token taal | Voer de naam van het kenmerk van het kenmerk met de korte naam van de gewenste taal voor berichten van EDE token-tekst voor de gebruiker. |
| Telefoons | De volgende kenmerken worden gebruikt om te importeren of te synchroniseren van telefoonnummers van de gebruiker.  Als de naam van een kenmerk niet is opgegeven voor het telefoontype, het telefoontype is alleen beschikbaar wanneer importeren vanuit Active Directory of het toevoegen van items voor synchronisatie. |
| Business | Voer de naam van het kenmerk van het kenmerk dat het zakelijke telefoonnummer in een gebruikersrecord bevat.  De standaardwaarde is telephoneNumber. |
| Home | Voer de naam van het kenmerk van het kenmerk met de privé-telefoonnummer in een gebruikersrecord.  De standaardwaarde is TelefoonPrivé. |
| Pager | Voer de naam van het kenmerk van het kenmerk het semafoonnummer in een gebruikersrecord bevat.  De standaardwaarde is de pager. |
| Mobile | Voer de naam van het kenmerk van het kenmerk het nummer van de mobiele telefoon in een gebruikersrecord bevat.  De standaardwaarde is mobiel. |
| Fax | Voer de naam van het kenmerk van het kenmerk het faxnummer in een gebruikersrecord bevat.  De standaardwaarde is facsimileTelephoneNumber. |
| IP-telefoon | Voer de naam van het kenmerk van het kenmerk het IP-telefoonnummer in een gebruikersrecord bevat.  De standaardwaarde is ipPhone. |
| Aangepaste | Voer de naam van het kenmerk van het kenmerk bevat een aangepaste telefoonnummer in |
|  | een gebruikersrecord.  De standaardwaarde is leeg. |
| Extensie | Voer de naam van het kenmerk van het kenmerk het toestelnummer van een gebruikersrecord bevat.  De waarde van het Uitbreidingsveld wordt gebruikt als de extensie voor het primaire telefoonnummer alleen.  De standaardwaarde is leeg. <br><br>Opmerking: Als de extensie-kenmerk niet wordt opgegeven, extensies kunnen worden opgenomen als onderdeel van het kenmerk van de telefoon.  De extensie moet worden voorafgegaan door een "x" zodat deze kan worden geparseerd.  Bijvoorbeeld 555-123-4567 x890 zou leiden tot 555-123-4567 als het telefoonnummer en 890 de uitbreiding. |
| Knop Standaardinstellingen herstellen | Klik op de knop Beginwaarden om terug te keren alle kenmerken terug naar de standaardwaarde.  De standaardwaarden moeten goed met de normale Active Directory- of ADAM-schema. |

Als u kenmerken wilt bewerken, klikt u op de knop bewerken op het tabblad kenmerken.  U krijgt een windows waarmee u de kenmerken te bewerken.

![Kenmerken bewerken](./media/multi-factor-authentication-get-started-server-dirint/dirint4.png)

## <a name="synchronization"></a>Synchronisatie
Synchronisatie zorgt ervoor dat de Azure meerledige database gesynchroniseerd met de gebruikers in Active Directory of een andere Lightweight Directory Access Protocol (LDAP) Lightweight Directory Access Protocol-directory.  Het proces is vergelijkbaar met het importeren van gebruikers handmatig uit Active Directory, maar regelmatig controleert of Active Directory-gebruiker en groep wijzigingen te verwerken.  Ook vindt u hier bij het uitschakelen of verwijderen van gebruikers verwijderd uit een container- of groep gebruikers en te verwijderen uit Active Directory verwijderd.

De meerledige verificatie ADSync service is een Windows-service waarmee het periodiek opvragen van Active Directory wordt uitgevoerd.  Dit is niet te verwarren met Azure AD Sync of Azure AD verbinden.  de ADSync meerledige verificatie, is gebaseerd op een vergelijkbare codebasis specifiek voor de Server Azure meerledige verificatie.  Het is geïnstalleerd in een staat gestopt en wordt gestart door de service meerledige verificatie Server geconfigureerd om uit te voeren.  Als u een configuratie met meerdere servers meerledige verificatie Server hebt, kan de ADSync meerledige verificatie alleen worden uitgevoerd op één server.

De service meerledige verificatie ADSync gebruikt de extensie DirSync LDAP server geleverd door Microsoft worden efficiënt wordt gecontroleerd op wijzigingen.  Dit besturingselement DirSync beller moet 'directory wijzigingen ophalen' rechts en Active Directory-replicatie-Get-wijzigingen uitgebreid rechts toegang beheren.  Deze rechten worden standaard toegewezen aan de accounts Administrator en LocalSystem op domeincontrollers.  De service meerledige verificatie AdSync standaard uitgevoerd als LokaalSysteem.  Daarom is het eenvoudigste om de service op een domeincontroller wordt uitgevoerd.  De service kan worden uitgevoerd als een account met minder machtigingen als u deze altijd een volledige synchronisatie uitvoeren als u wilt configureren.  Dit is minder efficiënt, maar minder machtigingen vereist.

Als geconfigureerd voor gebruik van het besturingselement DirSync biedt ondersteuning voor LDAP en de LDAP-adreslijst, vervolgens voor de gebruiker en groep beveiligingswijzigingen polling werken hetzelfde als met Active Directory.  Als het besturingselement DirSync biedt geen ondersteuning voor de LDAP-adreslijst, wordt een volledige synchronisatie tijdens elke cyclus worden uitgevoerd.

![Synchronisatie](./media/multi-factor-authentication-get-started-server-dirint/dirint5.png)

Gebruik de onderstaande tabel voor meer informatie over elk van de afzonderlijke instellingen op het tabblad synchronisatie.

| Functie | Beschrijving |
| ------- | ----------- |
| Synchronisatie met Active Directory inschakelen | Wanneer het selectievakje is ingeschakeld, wordt de meerledige verificatie Server-service gestart worden periodiek gecontroleerd Active Directory op wijzigingen. <br><br>Opmerking: ten minste één synchronisatie-Item moet worden toegevoegd en een nu synchroniseren moet worden uitgevoerd voordat de meerledige verificatie Server-service wordt gestart met het verwerken van wijzigingen. |
| Synchroniseren elke | Geef het interval dat tussen polling en wijzigingen verwerken de meerledige verificatie Server-service wacht. <br><br> Opmerking: Het opgegeven interval is de tijd tussen het begin van elke cyclus.  Als de tijd wijzigingen verwerken het interval wordt overschreden, wordt de service direct opnieuw opvragen. |
| Gebruikers niet meer verwijderen uit Active Directory | Wanneer ingeschakeld, wordt de serverservice meerledige verificatie Active Directory verwijderde gebruiker tombstones verwerken en de gebruiker meerledige verificatie Server verwijderen. |
| Altijd een volledige synchronisatie uitvoeren | Wanneer het selectievakje is ingeschakeld, wordt de serverservice meerledige verificatie altijd een volledige synchronisatie uitvoeren.  Wanneer dit selectievakje is uitgeschakeld, wordt de serverservice meerledige verificatie een incrementele synchronisatie uitvoeren door de query alleen gebruikers die zijn gewijzigd.  De standaardinstelling is uitgeschakeld. <br><br> Opmerking: Wanneer dit selectievakje is uitgeschakeld, een incrementele synchronisatie kan alleen worden uitgevoerd als de map het besturingselement DirSync ondersteunt en de account die wordt gebruikt om te binden aan de directory heeft de juiste machtigingen DirSync incrementele query's kan uitvoeren.  Als de account niet de juiste machtigingen heeft of meerdere domeinen zijn betrokken bij de synchronisatie, voert u een volledige synchronisatie wordt aanbevolen. |
| Vereist goedkeuring beheerder wanneer meer dan X-gebruikers worden uitgeschakeld of verwijderd | Items voor synchronisatie kunnen worden geconfigureerd als u wilt uitschakelen of verwijderen van gebruikers die niet langer lid van de beveiligingsgroep of container van het item.  Als beveiliging is goedkeuring van een beheerder vereist als het aantal gebruikers wilt uitschakelen of verwijderen van een drempelwaarde overschrijdt.  Bij de controle, zal goedkeuring vereist zijn voor de opgegeven drempelwaarde.  De standaardwaarde is 5 en het bereik is 1 tot 999. <br><br> Goedkeuring wordt vergemakkelijkt door eerst een e-mailbericht verzenden naar beheerders. Het e-mailbericht bevat instructies voor het controleren en goedkeuren van het uitschakelen en verwijderen van gebruikers.  Wanneer de gebruikersinterface meerledige verificatie Server wordt gestart, wordt u gevraagd voor goedkeuring. |

De knop **Nu synchroniseren** , kunt u een volledige synchronisatie voor de opgegeven items voor synchronisatie.  Een volledige synchronisatie is vereist wanneer synchronisatie items zijn toegevoegd, gewijzigd, verwijderd of opnieuw gerangschikt.  Het is ook nodig voordat de service meerledige verificatie AdSync operationeel sinds het startpunt van waaruit de service wordt uitgevoerd voor incrementele wijzigingen wordt ingesteld.  Als wijzigingen zijn aangebracht in items voor synchronisatie en een volledige synchronisatie niet is uitgevoerd, wordt u gevraagd op Nu synchroniseren bij het navigeren naar een andere sectie of bij het sluiten van de gebruikersinterface.

De knop **verwijderen** kan de beheerder een of meer items voor synchronisatie in de lijst meerledige verificatie Server synchronisatie item verwijderen.

>[AZURE.WARNING]Als u een record met de synchronisatie is verwijderd, kan niet worden hersteld. U moet de record met de synchronisatie opnieuw toevoegen als u per ongeluk verwijderd.

De synchronisatie-item of de items voor synchronisatie zijn van meerdere factoren Auth Server verwijderd.  De serverservice meerledige verificatie worden niet meer verwerkt door de items voor synchronisatie.

De knoppen omhoog en omlaag kunnen de beheerder om de volgorde van de items voor synchronisatie.  De volgorde is belangrijk omdat de gebruiker lid van meer dan één item synchronisatie (bijv. een container en een beveiligingsgroep zijn kan).  De instellingen die zijn toegepast op de gebruiker tijdens de synchronisatie is afkomstig van de eerste synchronisatie-item in de lijst die gekoppeld aan de gebruiker is.  De items voor synchronisatie moeten daarom worden geplaatst in volgorde van prioriteit.

>[AZURE.TIP]Een volledige synchronisatie moet worden uitgevoerd na het verwijderen van items voor synchronisatie.  Een volledige synchronisatie worden uitgevoerd na het bestellen van artikelen voor synchronisatie.  Klik op de knop Nu synchroniseren om een volledige synchronisatie uitvoeren.

## <a name="multi-factor-auth-servers"></a>Meerledige verificatie Servers
Extra meerledige verificatie Servers kunnen worden ingesteld om te fungeren als een back-RADIUS-proxy, proxy, LDAP, of voor de IIS-verificatie. De configuratie van de synchronisatie worden gedeeld door alle van de agenten. Slechts één van deze agenten hebben echter het meerledige verificatie Server-service actief is. Op dit tabblad kunt u de meerledige verificatie Server selecteren die moet worden ingeschakeld voor synchronisatie.

![Meerdere Factor Auth-servers](./media/multi-factor-authentication-get-started-server-dirint/dirint6.png)
