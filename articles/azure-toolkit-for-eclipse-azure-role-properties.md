<properties
    pageTitle="Eigenschappen van Azure"
    description="Leren werken met de Toolkit Azure voor Eclips Azure functie-instellingen configureren."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690945.aspx -->

# <a name="azure-role-properties"></a>Eigenschappen van Azure #

Verschillende configuratie-instellingen voor uw Azure rol kunnen worden ingesteld binnen de Azure Toolkit voor Eclips.

## <a name="configuring-azure-role-properties"></a>Azure eigenschappen configureren ##

Configureren van de eigenschappen van Azure wordt bereikt met de eigenschap dialogs voor uw rol werknemer. Open het contextmenu voor de rol in de Eclips Project Explorer-venster en selecteer het vervolgmenu **Azure** . (Als u de rol in de Projectverkenner Eclips niet ziet, uw Azure project uitvouwen in de Projectverkenner.)

![][ic789599]

In dit onderwerp worden de verschillende eigenschappen die kunnen worden ingesteld in de dialoogvensters **Eigenschappen** beschreven. Houd er rekening mee dat veel eigenschappen worden automatisch ingevuld wanneer u een nieuw project voor Azure-implementatie maakt.

De volgende eigenschappenvensters zijn beschikbaar voor rollen Azure.

* [Eigenschappen van de virtuele machine](#virtual_machine_properties)
* [Eigenschappen in cache opslaan](#caching_properties)
* [Eigenschappen van certificaten](#certificates_properties)
* [Eigenschappen voor onderdelen](#components_properties)
* [Eigenschappen voor foutopsporing](#debugging_properties)
* [Eigenschappen van eindpunten](#endpoints_properties)
* [Omgeving variabelen eigenschappen](#environment_variables_properties)
* [Taakverdeling / eigenschappen van session affiniteit (a.k.a 'sticky sessies')](#session_affinity_properties)
* [Eigenschappen van lokale opslag](#local_storage_properties)
* [Configuratie-servereigenschappen](#server_configuration_properties)
* [SSL-offloading eigenschappen](#ssl_offloading_properties)
    
<a name="virtual_machine_properties"></a>
### <a name="virtual-machine-properties"></a>Eigenschappen van de virtuele machine ###

Open het contextmenu voor de rol in het venster Project Explorer Eclips van **Azure**op en klik vervolgens op **Eigenschappen**, en u hebt de mogelijkheid om te wijzigen van de grootte van de virtuele machine, en ook het aantal exemplaren wijzigen zoals in de volgende afbeelding.

![][ic719499]

>[AZURE.NOTE] Alleen Windows: als u het aantal exemplaren op een waarde groter dan 1 en u ook een toepassingsserver configureren, de toolkit mogen slechts 1 exemplaar van rol in de emulator, ongeacht deze instelling uit te voeren. Dit is de poort binding om conflicten te voorkomen tussen de instanties van de andere server (bijvoorbeeld alle probeert te binden aan poort 8080) wanneer ze worden uitgevoerd op dezelfde computer. De instelling voor de gewenste instantie aantal blijft behouden, maar wordt ingevoerd wanneer u naar de cloud implementeren.

<a name="caching_properties"></a> 
### <a name="caching-properties"></a>Eigenschappen in cache opslaan ###

Open het contextmenu voor de rol in de Eclips Project Explorer-venster, klikt u op **Azure**en klik op **opslaan in cache**. In dit dialoogvenster kunt u benoemde reserveren memcache-compatibele caches, zodat u kunt versnellen uw webtoepassingen.

![][ic719483]

In de eigenschappenpagina **Caching** kunt u algemene instellingen voor het volgende:

* of reserveren caching is ingeschakeld.
* de grootte van de cache als een percentage van het geheugen.
* de accountnaam van de opslagruimte voor het opslaan van de cache staat wanneer de toepassing wordt uitgevoerd als een service cloud of geen als u niet wilt dat de status van de cache opslaan. (De naam van de opslag wordt niet gebruikt wanneer u uw toepassing in de emulator compute uitvoert.) Als u de naam van de opslag op **(auto)** (dit is de standaardinstelling), worden de configuratie opslaan in cache automatisch dezelfde opslag account gebruikt als die u in het dialoogvenster **publiceren op Azure selecteert** .

>[AZURE.NOTE] De instelling van de **(auto)** heeft het gewenste effect alleen als u uw implementatie van de toolkit Eclips met publiceren wizard publiceren. Als u in plaats daarvan het publiceren van het .cspkg-bestand handmatig met behulp van een extern mechanisme, zoals de [Azure Management Portal][]de installatie niet naar behoren.

Het volgende dialoogvenster ziet u de eigenschappen van een cache.

![][ic719501]

* **Naam:** De naam van de cache reserveren.
* **Poort nummer:** Het poortnummer dat u wilt gebruiken voor de cache.
* **Verloopbeleid:** Een van de volgende waarden die aangeeft wanneer u een sleutel in de cache is verlopen.
    * **Absolute:** De sleutel verloopt als de tijd die is opgegeven door **minuten live** is bereikt.
    * **NeverExpires:** De sleutel beschikt niet over een verlooptijd.
    * **SlidingWindow:** De sleutel verloopt als niet is geopend voor de tijd die is opgegeven in **minuten live**; elke keer dat deze wordt geopend, de klok van de vervaldatum opnieuw wordt ingesteld.
* **Minuten live:** Maximum aantal minuten voor een sleutel memcached met live aan het verloopbeleid.
* **Hoge beschikbaarheid met gerepliceerde back-ups op andere rol exemplaren:** Indien ingeschakeld, zorgt voor hoge beschikbaarheid met behulp van back-ups op een andere rol exemplaren gerepliceerd. Zorg ervoor dat ten minste twee exemplaren van de rol van kracht voor de implementatie van deze functie kan werken.

Een nieuwe cache, klikt u op de knop **toevoegen** in het eigenschappenvenster van de **cache** , en een dialoogvenster **Met de naam Cache configureren** wordt geopend. Waarden opgeven voor de eigenschappen die hierboven zijn beschreven.

Een benoemde cache wijzigen, selecteert u de cache en klik op de knop **bewerken** in het eigenschappenvenster van de **cache** . Een dialoogvenster wordt geopend waarin u de cache-eigenschappen wijzigen. Druk op **OK** om de cachewaarden opslaat.

Een cache verwijderen, selecteert u de cache en klikt u op de knop **verwijderen** in het eigenschappenvenster van de **cache** en klikt u op **Ja** om de verwijdering te bevestigen.

Zie voor meer informatie over het gebruik van caching [Co-located Caching gebruiken][].

<a name="certificates_properties"></a> 
### <a name="certificates-properties"></a>Eigenschappen van certificaten ###

Open het contextmenu voor de rol in de Project Explorer-venster van de Eclips **Azure**en klik vervolgens op **certificaten**.

![][ic710964]

In dit dialoogvenster kunt u toevoegen of verwijderen waarnaar wordt verwezen door uw project Eclips certificaten. Opmerking de hier vermelde certificaten niet automatisch worden opgeslagen in een willekeurige Java keystore en zijn daarom niet automatisch beschikbaar voor gebruik binnen een Java-toepassing uit. Ze worden alleen geregistreerd met Azure, zodat ze kunnen vooraf worden geladen in de Windows-certificaat opslaan op de virtuele machines met de implementatie en vervolgens worden gebruikt in andere Windows-software. De enige functie van de toolkit die gebruikmaakt van de verklaringen waarnaar wordt verwezen op deze manier in het dialoogvenster **certificaten** is momenteel [SSL-Offloading][], omdat het gebruikmaakt van Internet Information Services (IIS) en toepassing aanvragen routering (ARR), waarvoor het juiste certificaat op deze manier beschikbaar.

Wanneer u uw project op Azure met behulp van de wizard Publiceren implementeren, wordt u gevraagd om te wijzen op de uitwisseling van persoonlijke gegevens (PFX) bestanden overeenkomt met deze certificaten, samen met hun wachtwoorden, om automatisch te uploaden naar de Azure service, maar alleen als ze niet zijn geüpload er eerder.

<a name="components_properties"></a> 
### <a name="components-properties"></a>Eigenschappen voor onderdelen ###

Open het contextmenu voor de rol in de Project Explorer-venster van de Eclips **Azure**en klik vervolgens op **onderdelen**. In dit dialoogvenster hebt u de mogelijkheid tot het toevoegen, wijzigen of verwijderen van de onderdelen van uw rol, alsmede wijzigen van de volgorde waarin ze worden verwerkt.

![][ic719502]

De functie onderdelen kunt u afhankelijkheden toevoegen aan uw project Azure-implementatie, zoals Java-toepassing projecten, speciale bestanden en opdrachtregel uitvoerbare instructies die nodig zijn voor de implementatie.

Voor elk onderdeel, kunt u het volgende opgeven:

* De stap te nemen bij het importeren van het onderdeel in uw project Azure-implementatie als deze is ingebouwd.
* De stap die moeten worden genomen bij de implementatie van dat onderdeel in de Azure cloud.

>[AZURE.NOTE] Bij het onderdeelbestanden of opdrachtregels opgeven, houd er rekening mee dat uw implementatie worden gepubliceerd in een virtuele Windows-computer, zodat u uw aangepaste werk moet geldig zijn voor een Windows-besturingssysteem. 

Onderdelen hebben de volgende eigenschappen:

* **Importeren:** Deze methode geeft aan hoe het onderdeel wordt geïmporteerd in het project als het project is gebouwd. Dit kan een van de volgende waarden zijn:
    * **kopiëren:** Het onderdeel wordt gekopieerd uit het lokale pad dat is opgegeven met de eigenschap **uit** in de rol van **approot** directory.
    * **Wissen:** Het onderdeel is een Java enterprise archief (EAR) geïmporteerd vanuit een Enterprise Application-Project in het lokale pad dat is opgegeven met de eigenschap **uit** . (Dit wordt automatisch gedetecteerd door de toolkit op basis van de aard van het project op die locatie).
    * **JAR:** Het onderdeel is een Java-archief (JAR) en wordt geïmporteerd uit een Java-project in het lokale pad dat is opgegeven met de eigenschap **uit** . (Dit wordt automatisch gedetecteerd door de toolkit op basis van de aard van het project op die locatie).
    * **geen:** Geen actie wordt ondernomen voor het importeren van het onderdeel. Dit is van toepassing wanneer de component wordt aangenomen dat al aanwezig in de rol van **approot** directory of als de component is slechts een uitvoerbare opdrachtregel-instructie, zoals opgegeven in de eigenschap **als** wanneer **de methode** **exec**.
    * **WAR:** Het onderdeel is een Java web application archive (WAR) en wordt geïmporteerd uit een dynamische Web-Project in het lokale pad dat is opgegeven met de eigenschap **uit** . (Dit wordt automatisch gedetecteerd door de toolkit op basis van de aard van het project op die locatie).
    * **Postcode:** Het onderdeel is een zip-bestand en wordt geïmporteerd door comprimeren voor de map of het bestand dat is opgegeven door de eigenschap **uit** .
* **Van:** Bronpad op uw lokale computer naar de map of het bestand waarmee de items te importeren in uw implementatie. Omgevingsvariabelen van Windows kunnen worden gebruikt in deze eigenschap. Alle importeerbare onderdelen worden geïmporteerd in de rol van **approot** directory wanneer het project wordt gebouwd.
    
    Houd er rekening mee dat er de mogelijkheid voor de implementatie van een onderdeel van een download bij de implementatie van de wolk (niet de compute-emulator). Zie onderstaande verwante informatie over het toevoegen van een onderdeel.    
    
* **As:** Naam waaronder het onderdeel wordt geïmporteerd in de rol van **approot** directory en uiteindelijk in de cloud Azure geïmplementeerd. Niets op deze eigenschap om de naam hetzelfde als bij de lokale computer. (Voor componenten die zijn **methode waarvan** is ingesteld op de **Raad**, dit is een willekeurige Windows-opdrachtregel-instructie.)

    >[AZURE.IMPORTANT] Als u tekens voor deze waarde, zullen ze worden verwerkt afhankelijk van de methode. Als de methode **exec**, worden spaties geïnterpreteerd als scheidingstekens voor argumenten voor opdrachtregel en niet als onderdeel van de bestandsnaam. Implementeren voor alle andere methoden, spaties worden geïnterpreteerd als onderdeel van de bestandsnaam.
    
* **Implementeren:** Deze methode de actie die wordt toegepast op het onderdeel geeft, wanneer de installatie wordt gestart. Dit kan een van de volgende waarden zijn:
    * **kopiëren:** Het onderdeel wordt gekopieerd naar het doelpad opgegeven door de eigenschap **aan** .
    * **exec:** Het onderdeel is een uitvoerbaar Windows-opdrachtregel instructie uitgevoerd in de context van het pad dat is opgegeven door **de eigenschap** op het moment dat de installatie wordt gestart.
    * **geen:** Geen actie wordt toegepast op het onderdeel, wanneer de installatie wordt gestart.
    * **Postcode:** Het onderdeel is uitgepakt naar de bestemmingspad opgegeven door de eigenschap **aan** . Deze methode is alleen beschikbaar wanneer de eigenschap **Import** **zip wordt**.
* **To:** Het doelpad op de virtuele machine waarin het onderdeel wordt geïmplementeerd. Omgevingsvariabelen van Windows kunnen worden gebruikt in deze eigenschap en de paden zijn ten opzichte van **approot**.
    
Om een nieuw onderdeel wilt toevoegen, klikt u op de knop **toevoegen** in het eigenschappenvenster van de **onderdelen** en een dialoogvenster met een **Onderdeel van Azure rol** wordt geopend. Waarden opgeven voor de eigenschappen die hierboven zijn beschreven. 

Hieronder ziet u een voorbeeld voor het toevoegen van een nieuw onderdeel in de WAR.

![][ic719503]

Bij de distributie in de cloud (niet de compute-emulator), als u wilt dat voor de implementatie van het onderdeel van een download, zorgen **in de cloud, in plaats van in het pakket, met inbegrip van de implementatie van** is ingeschakeld. Als u downloaden van uw account Azure opslag, de opslag van de **opslag account** keuzelijst wilt (kunt u op de koppeling van de **Accounts** om te wijzigen wat er in de lijst), account die wordt gedeeltelijk invullen in het veld **URL** en vult u vervolgens het resterende deel van de URL selecteren. Als u niet Azure opslag gebruiken wilt, selecteert u **(geen)** in de vervolgkeuzelijst **opslag account** en voer de URL voor de component in het veld **URL** . Een van de volgende manieren opgeven:

* **kopiëren:** Het onderdeel downloaden wordt gekopieerd naar het doelpad aangegeven door het pad **Naar de Directory** .
* **hetzelfde:** Dezelfde methode gebruikt voor het **implementeren van downloaden** voor het **distribueren van het pakket**.
* **Postcode:** Het onderdeel downloaden is uitgepakt naar door het pad **Naar de map** opgegeven bestemmingspad.

Als u wilt een onderdeel wijzigen, selecteert u het onderdeel en klikt u op de knop **bewerken** in het eigenschappenvenster van de **onderdelen** . Een dialoogvenster wordt geopend zodat u de onderdeeleigenschappen wijzigen. Druk op **OK** om de onderdeelwaarden opslaan.

Als u een onderdeel wilt verwijderen, selecteert u het onderdeel en klikt u op de knop **verwijderen** in het eigenschappenvenster van de **onderdelen** en klikt u op **Ja** om de verwijdering te bevestigen.

Onderdelen worden in de aangegeven volgorde verwerkt. Gebruik de knoppen **Omhoog** en **Omlaag** om de volgorde te bepalen.

>[AZURE.NOTE] Onderdelen ook is afhankelijk van de functie server configuratie. Deze onderdelen kunnen worden verwijderd of gewijzigd zonder de bijbehorende configuratie van de server verwijderd. U wordt gevraagd over die bij een poging om deze onderdelen te wijzigen.

<a name="debugging_properties"></a> 
### <a name="debugging-properties"></a>Eigenschappen voor foutopsporing ###

Open het contextmenu voor de rol in de Eclips Project Explorer-venster, klikt u op **Azure**en klik op **Foutopsporing**. In dit dialoogvenster, u kunt in- of uitschakelen van foutopsporing op afstand, alsmede debug-configuraties maken zoals in de volgende afbeelding.

![][ic719504]

Zie voor meer informatie over foutopsporing [Foutopsporing Azure toepassingen in Eclips][].

<a name="endpoints_properties"></a> 
### <a name="endpoints-properties"></a>Eigenschappen van eindpunten ###

Open het contextmenu voor de rol in de Eclips Project Explorer-venster en op **Azure**, **eindpunten**. In dit dialoogvenster hebt u de mogelijkheid om een eindpunt, maken en bewerken of verwijderen van een eindpunt, zoals in de volgende afbeelding.

![][ic719505]

Een eindpunt, klik op de knop **toevoegen** in het eigenschappenvenster van de **eindpunten** , en een dialoogvenster met een **Eindpunt toevoegen** wordt geopend.

![][ic710897]

Voer een naam voor het eindpunt, selecteer het type ( **Input**, **intern**, of **InstanceInput**) en de openbare en particuliere poort opgeven. Druk op **OK** om de nieuwe waarden voor eindpunt.

Afhankelijk van het type eindpunt, mag u poortbereiken als volgt:

* Voor het eindpunt van een exemplaar van invoer, kan de openbare poort een bereik van poorten (bijvoorbeeld **2000-2010**) en de particuliere poort is een vaste waarde.
* Voor een interne eindpunt kan openbare poort niet wordt gebruikt, en de private poort een bereik, of leeg of is ingesteld op een sterretje om aan te geven dat deze automatisch wordt ingesteld door Azure.
* Voor invoer van eindpunten, de openbare poort kan alleen worden een vaste waarde en particuliere poort kan een vaste waarde of leeg of ingesteld als een sterretje om aan te geven dat wordt automatisch ingesteld op Azure.

Als u een enkele poortnummer gebruiken in plaats van een bereik wilt, laat u het tekstvak voor het einde van het bereik geen.

Voor de poorten die zijn ingesteld op automatisch, als u nodig hebt om te bepalen welke poort wordt daadwerkelijk gebruikt tijdens runtime, kunnen toepassingen gebruikmaken van de Azure Runtime API Service, die wordt beschreven in het [com.microsoft.windowsazure.serviceruntime pakket samenvatting][].

Hoe invoer eindpunten exemplaar kunnen worden gebruikt om te helpen bij het opsporen van fouten in een distributie met meerdere instanties Zie [Foutopsporing in een specifieke rol exemplaar in een distributie met meerdere exemplaren][].

Als u wilt wijzigen op een eindpunt, selecteer het eindpunt en klik op de knop **bewerken** in het eigenschappenvenster van de **eindpunten** . Een dialoogvenster wordt geopend waarin u de naam, type en openbare en particuliere poorten wijzigen. Druk op **OK** om het eindpunt van gewijzigde waarden opslaan.

Een eindpunt verwijderen, selecteert u het eindpunt en klikt u op de knop **verwijderen** in het eigenschappenvenster van de **eindpunten** en klikt u op **Ja** om de verwijdering te bevestigen.

De toolkit kan alleen goed configureren sommige functies (bijvoorbeeld Caching, foutopsporing op afstand, sessie affiniteit of SSL-offloading) ingeschakeld door de gebruiker op een rol, speciale eindpunten die samen met door de gebruiker gedefinieerde eindpunten worden weergegeven automatisch configureren. De toolkit wordt voorkomen dat de gebruiker bewerken of eindpunten verwijderen die automatisch worden gegenereerd als de bijbehorende functie is ingeschakeld.

<a name="environment_variables_properties"></a> 
### <a name="environment-variables-properties"></a>Omgeving variabelen eigenschappen ###

Open het contextmenu voor de rol in de Project Explorer-venster van de Eclips **Azure**en klik vervolgens op **Omgevingsvariabelen**. In dit dialoogvenster hebt u de mogelijkheid om een omgevingsvariabele maakt ook wijzigen of verwijderen van een omgevingsvariabele zoals in de volgende afbeelding.

![][ic719506]

Omgevingsvariabelen zijn beschikbaar voor het script gestart bij het starten van de rol.

>[AZURE.NOTE] Als u omgevingsvariabelen opgeeft, houd er rekening mee dat uw implementatie worden gepubliceerd in een virtuele Windows-computer, zodat de omgevingsvariabelen geldig zijn voor een Windows-besturingssysteem moet.

Als een voorbeeld van een omgevingsvariabele wordt beschikbaar als de functie wordt gestart, maakt u een nieuwe omgevingsvariabele door te klikken op de knop **toevoegen** . Hieronder ziet u een omgevingsvariabele met de naam **MyRoleVersion** wordt gemaakt en de waarde **1.0**toegewezen.

![][ic659268]

U kunt uw jsp-code weergeven de waarde met behulp van de `System.getenv` methode:

    <body>
      <b> Hello World!</b>
      <p>Running role version: <%= System.getenv("MyRoleVersion") %></p>
    </body>

Waardoor deze uitvoer wanneer uw toepassing wordt uitgevoerd:

![][ic552233]

Om een omgevingsvariabele wijzigen, selecteert u de variabele en klikt u op de knop **bewerken** in het eigenschappenvenster voor **Omgevingsvariabelen** . Een dialoogvenster wordt geopend zodat u de omgeving variabele eigenschappen wijzigen. Druk op **OK** om de omgeving variabele waarden opslaan.

Om een omgevingsvariabele verwijderen, selecteert u de variabele en klikt u op de knop **verwijderen** in het eigenschappenvenster van de **Omgevingsvariabelen** en klikt u op **Ja** om de verwijdering te bevestigen.

De toolkit kan alleen goed configureren sommige functies (zoals serverconfiguratie foutopsporing op afstand of de lokale opslag) ingeschakeld door de gebruiker op een rol, speciale omgevingsvariabelen die u samen met de door de gebruiker gedefinieerde omgevingsvariabelen automatisch configureren. De toolkit wordt voorkomen dat de gebruiker bewerken of verwijderen die automatisch omgevingsvariabelen gegenereerd als de bijbehorende functie is ingeschakeld.

<a name="session_affinity_properties"></a> 
### <a name="load-balancing--session-affinity-aka-sticky-sessions-properties"></a>Taakverdeling / eigenschappen van session affiniteit (a.k.a 'sticky sessies') ###

Open het contextmenu voor de rol in de Project Explorer-venster van de Eclips **Azure**en klik vervolgens op **Load Balancing**. In dit dialoogvenster hebt u de mogelijkheid tot in- of uitschakelen van affiniteit sessie, zoals in de volgende afbeelding.

![][ic719492]

Zie [Sessie affiniteit][]voor verwante informatie. Let ook op de werking van deze functie in het kader van het SSL-offloading, zoals wordt beschreven op [SSL-Offloading][].

<a name="local_storage_properties"></a> 
### <a name="local-storage-properties"></a>Eigenschappen van lokale opslag ###

Open het contextmenu voor de rol in de Project Explorer-venster van de Eclips **Azure**en klik vervolgens op **Lokale opslag**. In dit dialoogvenster hebt u de mogelijkheid om te maken, wijzigen of verwijderen van tijdelijke lokale opslag voor de virtuele machine waarop de toepassing wordt uitgevoerd. Bepaalde waarden kunnen worden ingesteld voor de grootte van de lokale opslag, en of de inhoud behouden blijven wanneer de rol wordt gerecycled, zoals in de volgende afbeelding.

![][ic719508]

U kunt desgewenst ook een omgevingsvariabele die overeenkomt met de lokale opslag opgeven.

Standaard is alles wat u in Azure implementeren geplaatst (en zijn uitgepakt) in de map **approot** van het exemplaar van de rol. Terwijl de meest eenvoudige implementatie zal is passen er ook nadat ritsen, de toegewezen ruimte voor de directory **approot** beperkt en is niet goed gedefinieerd (minder dan 1 GB is een redelijke vuistregel). Daarom dat Azure voldoende schijfruimte wordt toegewezen voor grotere implementaties die niet in de map **approot** passen mogelijk, zodat u moet een lokale opslag resource instellen met behulp van het dialoogvenster **Lokale opslag** . Zie [Grote implementaties implementeren][]voor een eenvoudige manier om dit te doen.

Eenvoudig kunt u verwijzen naar de resource opslag van opstartscripts (bijvoorbeeld uw **startup.cmd**) met de omgevingsvariabele automatisch gekoppeld met de toolkit Eclips de bron, zoals wordt weergegeven in het dialoogvenster **Lokale opslag** . Deze omgevingsvariabele bevat het volledige pad van de lokale resource die u hebt geconfigureerd op het moment dat het opstartscript wordt uitgevoerd. 

Als u wilt wijzigen in een bron van lokale opslag, selecteert u de resource lokale opslag en klik op de knop **bewerken** in het eigenschappenvenster van de **Lokale opslag** . Een dialoogvenster wordt geopend waarin u de eigenschappen van lokale opslag resource wijzigen. Druk op **OK** om de lokale opslag resource waarden opslaan.

Om een lokale opslagbron verwijdert, selecteert u de resource lokale opslag en klikt u op de knop **verwijderen** in het eigenschappenvenster voor **Lokale opslag** en klikt u op **Ja** om de verwijdering te bevestigen.

<a name="server_configuration_properties"></a> 
### <a name="server-configuration-properties"></a>Configuratie-servereigenschappen ###

Open het contextmenu voor de rol in de Project Explorer-venster van de Eclips **Azure**en klik vervolgens op **Configuratie van de Server**. In dit dialoogvenster kan toevoegen, verwijderen en wijzigen van de JDK en Java application server gebruikt door uw implementatie u ook toevoegen of verwijderen van de toepassingen (zoals WAR, POTJES of EAR-bestanden) die worden gebruikt door de implementatie.

### <a name="jdk-configuration"></a>JDK-configuratie ###

In dit dialoogvenster kunt u het pakket JDK te gebruiken voor de implementatie. Als u een Eclips in Windows gebruikt, kunt u het pakket JDK lokaal gebruiken als u in de emulator Azure en hebt u de mogelijkheid om te implementeren die lokale installatie op Azure. De emulator JDK-instelling is niet van toepassing op niet-Windows-besturingssystemen, en u kunt de lokaal geïnstalleerde JDK niet implementeren omdat deze niet compatibel met Windows. Echter, ongeacht het besturingssysteem dat u gebruikt, u kunt altijd tussen de 3de partij JDK pakketten te implementeren op Azure of op uw eigen Windows-compatibele JDK-pakket vanaf een alternatieve downloadlocatie aan te wijzen.

Hier volgt een voorbeeld van hoe u in Windows een JDK kunt opgeven:

![][ic780647]

Als u een Eclips in Windows gebruikt, kunt u een JDK te gebruiken met de compute-emulator hiervoor zorgen **dat de JDK uit voor het testen van lokaal bestandspad gebruiken** is ingeschakeld in het gedeelte **Emulator implementeren** . Geef vervolgens het lokale pad naar de JDK; u kunt bladeren naar andere JDK als degene die u wilt gebruiken niet automatisch wordt geselecteerd. U hebt ook de mogelijkheid uw JDK implementeren op uw Azure cloud service; hiervoor de optie **Mijn lokale JDK (auto-uploaden naar cloud opslag) implementeren** in het gedeelte **Cloud-implementatie** .

Opmerking: Op niet-Windows-besturingssystemen, de **Emulator implementatie** -instellingen en de optie **Mijn lokale JDK implementeren** zijn niet beschikbaar. In het volgende voorbeeld ziet u een JDK opgeven op een Mac of andere ondersteunde niet-Windows-besturingssysteem:

![][ic789643]

Ongeacht het besturingssysteem dat u op bent, hebt u de volgende twee **wolk implementatie** -opties voor de bron en het type van de JDK-pakket:

* **Een 3e partij JDK-pakket beschikbaar op Azure** 
* **Implementeren van een aangepaste downloaden** 

Als u de optie **een 3de partij van Azure JDK-pakket implementeren** :

1. Schakel het selectievakje met de naam **een 3de partij van Azure JDK-pakket implementeren**.
1. Selecteer uit de vervolgkeuzelijst het 3e partij JDK-pakket is beschikbaar op Azure.
1. Het tabblad **JDK** ziet er ongeveer als de volgende opties in Windows:  ![][ic780648] 
    en het ziet er ongeveer als volgt op de Mac OS of andere niet-Windows-besturingssystemen ondersteund: ![][ic789643]
1. Klik op **OK** om uw wijzigingen te slaan.
1. Wanneer u wordt gevraagd om de gebruiksrechtovereenkomst van de 3de partij JDK Pakketprovider, kunt u de licentievoorwaarden bekijken. Ervan uitgaande dat u de voorwaarden accepteert, klikt u op **Ja** om de **gebruiksrechtovereenkomst (EULA) accepteren** -dialoogvenster te sluiten.
    Houd er rekening mee dat de onderliggende logica waarop items worden weergegeven in de vervolgkeuzelijst voor de optie **een 3de partij van Azure JDK-pakket implementeren** kan worden aangepast. Klik op de link **aanpassen** om de items in het dialoogvenster **JDK** aanpassen. Hiermee sluit u het eigenschappenvenster van de **JDK** en open het bestand **componentsets.xml** in de Eclips, die u kunt vervolgens zo nodig wijzigen. Documentatie voor **componentsets.xml** is opgenomen in het **componentsets.xml** -bestand zelf.

Als u de optie **implementeren een JDK van een aangepaste downloaden** :

1. Maak een ZIP van de installatiemap van JDK om te garanderen dat de directory knooppunt zelf het kind van de ZIP-structuur en niet de inhoud ervan. Let op de naam van de map, als u het later nodig hebt en houd er rekening mee dat deze installatie JDK worden geïmplementeerd op een virtuele Windows-computer.
1. De POSTCODE in uw rekening Azure opslag uploaden als een blob. U doet dit met behulp van een hulpprogramma voor extern beschikbaar voor uploaden BLOB's naar Azure opslag. Het verdient aanbeveling een blob voor particulier gebruik. Let op de blob-URL van de inhoud van de ZIP.
1. Schakel het selectievakje met de naam **implementeren een JDK van een aangepaste downloaden**.
    Als u downloaden van uw account Azure opslag, de opslag van de **opslag account** keuzelijst wilt (kunt u op de koppeling van de **Accounts** om te wijzigen wat er in de lijst), account die wordt gedeeltelijk invullen in het veld **URL** en vult u vervolgens het resterende deel van de URL selecteren. Als u niet Azure opslag gebruiken wilt, selecteert u **(geen)** in de vervolgkeuzelijst **opslag account** en voer de URL voor het downloaden van de JDK in het veld **URL** . Opslag Azure moet blob namen in de URL in kleine letters.
1. Zorg ervoor dat het tekstvak **JAVA_HOME** naar de juiste mapnaam verwijst. Standaard wordt het verwijzen naar dezelfde JDK directorynaam als de waarde die u hebt gekozen voor lokaal gebruik. Maar als de map in de ZIP heeft een andere naam (bijvoorbeeld door met een andere versie), de naam van de map in het tekstvak **JAVA_HOME** dienovereenkomstig bijwerken, aangezien deze instelling wordt gebruikt in de cloud (niet in de compute-emulator).
1. Klik op **OK** om uw wijzigingen te slaan.

Dat is. Nu, wanneer u voor de cloud, zult u de grootte van het pakket is veel kleiner, het bouwproces moet normaal gesproken minder tijd in beslag en de implementatie zelf wanneer u naar de cloud publiceert moet ook minder tijd in beslag. Houd er rekening mee dat de opties voor **Mijn lokale JDK (auto-uploaden naar cloud opslag) distribueren** of **distribueren een JDK van een aangepaste downloaden** gelden alleen wanneer uw toepassing is geïmplementeerd in de cloud. Ze hebben geen invloed op uw compute emulator ervaring; de lokale versie van de onderdelen wordt nog steeds gebruikt bij het implementeren van de compute-emulator. 

### <a name="server-configuration"></a>Configuratie van de server ###

Hier volgt een voorbeeld van hoe u een toepassingsserver kunt opgeven.

![][ic796926]

Controleer of het selectievakje voor de **implementatie van een server van dit type** is geselecteerd en kies vervolgens het type van de toepassingsserver die u wilt gebruiken.

Voor het opgeven van een server te gebruiken voor de implementatie van de wolk, kunt u profiteren van de volgende opties:

1. **Implementeren is een 3de partij server beschikbaar in Azure** - dit is vooral van toepassing in dev/test scenario's waarbij implementatie en eenvoud een prioriteit is en de server hoeft niet met een aangepaste configuratie. Of wanneer u wilt een van deze servers als uitgangspunt gebruiken, maar u aanpassingen van de juiste server stappen in uw implementatie van opstarten logica.
1. **Implementeren van een aangepaste download** - dit is vooral van toepassing in productie scenario's wanneer u een speciaal voorbereid en geconfigureerde server die u wilt gebruiken in de cloud.
1. **De installatie van de lokale server implementeren** - dit is vooral van toepassing in als de lokale server-installatie al aangepast zijn geconfigureerd voor gebruik is. Als u deze optie kiest, moet u ook het pad naar de lokale server in het tekstvak **lokaal pad** opgeven.

Als u de optie **een 3de partij beschikbaar op Azure server implementeren** :

1. Schakel het selectievakje met de naam **een 3de partij beschikbaar op Azure server distribueren**.
1. In de vervolgkeuzelijst, selecteer de gewenste server-software voor gebruik met de implementatie in de cloud. Opmerking Als u al een soort server eerder hebt opgegeven, u zal worden beperkt tot een wolk-server die in de familie van dat servertype kiezen. Maar als u een servertype niet hebt, kunt u kiezen uit een van de servers die momenteel beschikbaar op Azure zijn en het servertype wordt automatisch voor u geselecteerd.
1. Klik op **OK** om uw wijzigingen te slaan.

Als de optie voor **het implementeren van een aangepaste downloaden** :

1. Zorg ervoor dat u een servertype volgens de voorgaande stappen al hebt geselecteerd. Zo weet de invoegtoepassing voor het implementeren van de server van uw aangepaste downloaden als het afkomstig uit dezelfde familie als de geselecteerde server zijn moet.
1. Schakel het selectievakje met de naam **implementeren van een aangepaste downloaden**.
    Als u downloaden van uw account Azure opslag wilt, download de opslag account uit de **opslag account** keuzelijst (kunt u op de koppeling van de **Accounts** om te wijzigen wat er in de lijst), die gedeeltelijk wordt ingevuld in het veld **URL** en vult u vervolgens het resterende deel van de URL naar uw server selecteren ZIP (bij gebruik van Azure opslag, blob namen in de URL in kleine letters moet). Als u niet Azure opslag gebruiken wilt, selecteert u **(geen)** in de vervolgkeuzelijst **opslag account** en geef de URL op de server download ZIP in het veld **URL** . De ZIP bevat een onderliggende map van de application server-installatiemap. Als u een zip voor zou Apache Tomcat, 7.0.35, binnen de postcode de onderliggende map van de installatiemap, bijvoorbeeld **apache-tomcat-7.0.35**zijn. 
1. De waarde voor de omgevingsvariabele basismap opgeven. Wordt standaard de waarde gebruikt voor de lokale toepassingsserver, indien van toepassing, maar u kunt een andere waarde opgeven als uw toepassingsserver wolk van uw lokale toepassingsserver verschilt. Echter, moet u om ervoor te zorgen dat uw toepassingsserver wolk van dezelfde familie als de server is eerder geselecteerde type.
    Als u in de toekomst uw postcode cloud application server bijwerkt, kunt u handmatig de instelling van de basismap wijzigen of deze weer overeenkomen met uw lokale instelling (als u uw lokale toepassingsserver te wijzigen).
1. Klik op **OK** om uw wijzigingen te slaan.

De onderliggende logica waarop items worden weergegeven op het tabblad **Server** van het eigenschappenvenster voor de **Configuratie van de Server** kan worden aangepast. Dit is een geavanceerde functie die u mogelijk nodig als uw behoeften verder reiken dan de standaardwaarden of als u wilt toevoegen andere servers. Klik op de link **aanpassen** de logica, in het dialoogvenster **Server** instellen. Hiermee sluit u de pagina **Serverconfiguratie** -eigenschap en open het bestand **componentsets.xml** in de Eclips, die u vervolgens wijzigen kunt wanneer dat nodig is om uit te breiden de configuratie van server-sjabloon. Documentatie voor **componentsets.xml** is opgenomen in het **componentsets.xml** -bestand zelf.

Als u de optie **Mijn lokale server (auto-uploaden naar cloud opslag) implementeren** :

1. Schakel het selectievakje met de naam **Mijn lokale server (auto-uploaden naar cloud opslag) implementeren**.
1. Selecteer in de vervolgkeuzelijst **opslag account** **(auto)**. Als u hier **(auto)** , de toolkit Eclips gebruikt dezelfde account opslag voor uw server geselecteerd voor de implementatie in het dialoogvenster **publiceren op Azure** .
1. Klik op **OK** om uw wijzigingen te slaan.

Selecteer in het tekstvak **pad naar de lokale server** een server installatiepad op uw computer als een van de volgende voorwaarden voldaan wordt:

* Wilt u uw implementatie testen in de emulator (alleen van toepassing op Windows).
* Wilt u de lokaal geïnstalleerde server implementeren naar de cloud.
* Wilt u een aangepaste server download in de cloud, waar de zaak gebruiken, worden bovendien dat de optie **Mijn lokale server (auto-uploaden naar cloud opslag) distribueren** hierboven is geselecteerd.

Als geen van de bovenstaande opties van toepassing zijn op uw situatie, is de instelling van de lokale server is optioneel.

### <a name="applications-configuration"></a>Configuratie van toepassingen ###

Hier volgt een voorbeeld van hoe u een toepassing kunt opgeven.

![][ic719512]

Klik op **toevoegen** aan een andere toepassing toevoegen of **verwijderen** om een toepassing te verwijderen. Voor doeleinden van efficiëntie, gebruiken als u downloaden voor de bron van een toepassing gebruiken wilt bij de implementatie van de wolk, de [Eigenschappen van onderdelen](#components_properties) opgeven van een URL, de rekening voor opslag, enz. 

Beginnen met de release van April 2014, worden uw toepassingen automatisch geüpload in dezelfde account opslag (onder de container **eclipsedeploy** ) als de geselecteerd voor de implementatie. De logica van het opstarten van uw distributie bevat een stap die eerst downloads die toepassingen van de opslag account. Dit betekent dat mogelijk upgrade uw toepassingen in uw implementatie zonder bouwen en implementeren van het hele pakket door nieuwere versies van de toepassing rechtstreeks in opslag (bijvoorbeeld de Azure portal met) account handmatig uploaden, oorspronkelijk er vervangen de WAR-bestanden worden geüpload door de toolkit. Vervolgens net start het recyclen van deze rol overal met behulp van Azure management portal opnieuw of via opdrachtregelprogramma's. (Rol rechtstreeks van recycling in de toolkit Eclips activeren wordt momenteel niet ondersteund.)

### <a name="notes-about-server-configuration"></a>Opmerkingen over de configuratie van de server ###

Via de pagina **Serverconfiguratie** -eigenschap wijzigingen worden doorgevoerd in de `<component>` elementen van het bestand Package.xml heeft.

Als u het **automatisch uploaden...** gebruiken of **distribueren van downloaden...** opties voor de JDK of application server en u voor de wolk (niet de compute-emulator maakt) en u met het netwerk verbonden bent, ziet u berichten in de Console-uitvoer de volgende bouwen zoals de Ant builder wordt gecontroleerd of de beschikbaarheid van de download:

`[windowsazurepackage] Verifying blob availability (https://example.blob.core.windows.net/temp/tomcat6.zip)...` 

Als u de optie voor **het implementeren van downloaden...** hebt geselecteerd, kan de volgende waarschuwing weergegeven, maar blijft de build:

`[windowsazurepackage] warning: Failed to confirm blob availability! Make sure the URL and/or the access key is correct (https://example.blob.core.windows.net/temp/tomcat6.zip).` 

Deze waarschuwing is de enige aanwijzing dat de beschikbaarheid van de download niet is gecontroleerd. Dus als een installatie niet in de cloud voor een of andere reden, controleren als u deze waarschuwing hebt ontvangen.

Als u wilt dat de controle van de download (bijvoorbeeld, als u denkt dat deze onnodig wordt vertraagd de build) uitschakelen, stelt u de `verifydownloads` om het kenmerk `false` in de `<windowsazurepackage>` element van Package.xml heeft: 

`<windowsazurepackage verifydownloads="false" ...>` 

Als u de optie **automatisch uploaden...** Klik in het consolevenster worden geselecteerd build berichten rapportage van de voortgang van het uploaden van elke 5 seconden wanneer een upload nodig is.

<a name="ssl_offloading_properties"></a> 
### <a name="ssl-offloading-properties"></a>SSL-offloading eigenschappen ###

Open het contextmenu voor de rol in de Project Explorer-venster van de Eclips **Azure**en klik vervolgens op **SSL-Offloading**. 

![][ic719481]

In dit dialoogvenster kunt u SSL-offloading, zodat u gemakkelijk in uw implementatie van Java op Azure, Hypertext Transfer Protocol Secure (HTTPS)-ondersteuning inschakelt zonder dat u SSL configureren in Java-toepassingsserver. Zie voor meer informatie [SSL-Offloading][] en [hoe gebruik SSL-Offloading][].

## <a name="see-also"></a>Zie ook ##

[Azure Toolkit voor Eclips][]

[Installatie van de Toolkit Azure voor Eclips][]

[Een toepassing Hallo wereld maakt voor Azure in Eclips][]

[Azure Projecteigenschappen][]

[Lijst met Azure opslag][]

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Projecteigenschappen]: http://go.microsoft.com/fwlink/?LinkID=699524
[Lijst met Azure opslag]: http://go.microsoft.com/fwlink/?LinkID=699528
[com.Microsoft.windowsazure.serviceruntime pakket overzicht]: http://azure.github.io/azure-sdk-for-java/com/microsoft/windowsazure/serviceruntime/package-summary.html
[Een toepassing Hallo wereld maakt voor Azure in Eclips]: http://go.microsoft.com/fwlink/?LinkID=699533
[Opsporen van fouten in een specifieke rol-instantie in een distributie met meerdere instanties]: http://go.microsoft.com/fwlink/?LinkID=699535#debugging_specific_role_instance
[Foutopsporing in Azure toepassingen in Eclips]: http://go.microsoft.com/fwlink/?LinkID=699535
[Implementatie van grote implementaties]: http://go.microsoft.com/fwlink/?LinkID=699536
[Het gebruik van reserveren in de cache opslaan]: http://go.microsoft.com/fwlink/?LinkID=699542
[Het gebruik van SSL-Offloading]: http://go.microsoft.com/fwlink/?LinkID=699545
[Installatie van de Toolkit Azure voor Eclips]: http://go.microsoft.com/fwlink/?LinkId=699546
[Sessie-affiniteit]: http://go.microsoft.com/fwlink/?LinkID=699548
[SSL-Offloading]: http://go.microsoft.com/fwlink/?LinkID=699549

<!-- IMG List -->

[ic789599]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789599.png
[ic719499]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719499.png
[ic719483]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719483.png
[ic719501]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719501.png
[ic710964]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic710964.png
[ic719502]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719502.png
[ic719503]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719503.png
[ic719504]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719504.png
[ic719505]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719505.png
[ic710897]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic710897.png
[ic719506]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719506.png
[ic659268]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic659268.png
[ic552233]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic552233.png
[ic719492]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719492.png
[ic719508]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719508.png
[ic780647]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic780647.png
[ic789643]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789643.png
[ic780648]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic780648.png
[ic789643]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789643.png
[ic796926]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic796926.png
[ic719512]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719512.png
[ic719481]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719481.png
