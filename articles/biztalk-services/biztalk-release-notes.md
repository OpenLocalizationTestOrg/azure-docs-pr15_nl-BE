<properties
    pageTitle="Release-opmerkingen voor BizTalk Azure Services | Azure Microsoft BizTalk-Services"
    description="Worden de bekende problemen voor Azure BizTalk-Services" 
    services="biztalk-services"
    documentationCenter=""
    authors="msftman"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="deonhe"/>

# <a name="release-notes-for-azure-biztalk-services"></a>Release-opmerkingen voor Azure BizTalk-Services

Release-opmerkingen voor de BizTalk-Microsoft Azure Services bevatten de bekende problemen in deze release.

## <a name="whats-new-in-the-november-update-of-biztalk-services"></a>Wat is er nieuw in de update November van BizTalk-Services
* Codering in rust kan worden ingeschakeld in de Portal BizTalk-Services. Zie de [Rest in BizTalk-serviceportal-codering inschakelen](https://msdn.microsoft.com/library/azure/dn874052.aspx).

## <a name="update-history"></a>Updategeschiedenis

### <a name="october-update"></a>Update van oktober

* Organisatie-accounts worden ondersteund:  
 * **Scenario**: U een BizTalk-Service implementatie met behulp van een Microsoft-account geregistreerd (zoals user@live.com). In dit scenario kunnen alleen gebruikers van Microsoft-Account de BizTalk-Service via de portal BizTalk-Services beheren. Een organisatie-account kan niet worden gebruikt.  
 * **Scenario**: U een BizTalk-Service implementatie met een organisatie account in een Azure Active Directory geregistreerd (zoals user@fabrikam.com of user@contoso.com). In dit scenario kunnen alleen Azure Active Directory: gebruikers binnen dezelfde organisatie de BizTalk-Service via de portal BizTalk-Services beheren. Een Microsoft-account kan niet worden gebruikt.  
* Wanneer u een BizTalk-Service in de klassieke Azure portal maakt, wordt u automatisch geregistreerd in de Portal BizTalk-Services.
 * **Scenario**: U inloggen op de klassieke Azure portal, een BizTalk-Service maken en **beheren** voor de eerste keer selecteert. Als de portal BizTalk-Services wordt geopend, wordt de BizTalk-Service automatisch registreert en gereed is voor de distributie.  
 Zie [registreren en bijwerken van de implementatie van een BizTalk-Service op de Portal van BizTalk-Services](https://msdn.microsoft.com/library/azure/hh689837.aspx).  

### <a name="august-14-update"></a>14 augustus Update
* Overeenkomst en bridge ontkoppeling – Trading partner overeenkomsten en bruggen zijn nu ontkoppeld in de Portal BizTalk-Services. Nu u overeenkomsten en bruggen afzonderlijk en in runtime bruggen omzetten in een overeenkomst op basis van de waarden in het EDI-bericht. Zie [Overeenkomsten in Azure BizTalk-Services maken](https://msdn.microsoft.com/library/azure/hh689908.aspx), [een EDI-brug met BizTalk-Services Portal maken](https://msdn.microsoft.com/library/azure/dn793986.aspx), [maken een AS2 bridge met BizTalk-Services Portal](https://msdn.microsoft.com/library/azure/dn793993.aspx), en [hoe bruggen los overeenkomsten tijdens runtime?](https://msdn.microsoft.com/library/azure/dn794001.aspx)  
* De optie voor het maken van sjablonen voor overeenkomsten wordt stopgezet.  
* Voor de overeenkomst verzenden kant kunt u sets van verschillende scheidingstekens voor elk schema. Deze configuratie is opgegeven onder de instellingen van de overeenkomst van verzenden aan de zijkant. Zie voor meer informatie [maken een X12 overeenkomst in Azure BizTalk-Services](https://msdn.microsoft.com/library/azure/hh689847.aspx) en [maken een overeenkomst EDIFACT in Azure BizTalk-Services](https://msdn.microsoft.com/library/azure/dn606267.aspx). Twee nieuwe entiteiten worden ook toegevoegd aan de TPM OM API voor hetzelfde doel. Zie [X12DelimiterOverrides](https://msdn.microsoft.com/library/azure/dn798749.aspx) en [EDIFACTDelimiterOverride](https://msdn.microsoft.com/library/azure/dn798748.aspx).  
* Standaard XSD-constructs, met inbegrip van afgeleide typen worden nu ondersteund. [Gebruik standaard XSD-constructs in uw toewijzingen](https://msdn.microsoft.com/library/azure/dn793987.aspx) en [Gebruik afgeleide Types in de toewijzing van scenario's en voorbeelden](https://msdn.microsoft.com/library/azure/dn793997.aspx)zien.  
* AS2 ondersteunt nieuwe MIC voor het ondertekenen van berichten en nieuwe versleutelingsalgoritmen. Zie [een AS2-overeenkomst in Azure BizTalk-Services maken](https://msdn.microsoft.com/library/azure/hh689890.aspx).  
## <a name="know-issues"></a>Bekende problemen

### <a name="connectivity-issues-after-biztalk-services-portal-update"></a>Verbindingsproblemen nadat het BizTalk-Services bijwerken Portal

  Als u de BizTalk-Services Portal opent terwijl BizTalk-Services is bijgewerkt om de wijzigingen doorvoeren voor de service, kunt u verbindingsproblemen met de BizTalk-Services Portal wordt geconfronteerd.  
  Als tijdelijke oplossing kan u de browser opnieuw starten, cache van de browser te verwijderen of de portal in de particuliere modus start.  

### <a name="visual-studio-ide-cannot-locate-the-artifact-if-you-click-an-error-or-warning-in-a-biztalk-services-project"></a>Visual Studio IDE kan het onderdeel niet vinden als u klikt op een fout of waarschuwing in een BizTalk-Services-project
Installeer Visual Studio 2012 Update 3 RC 1 u kunt het probleem oplossen.  

### <a name="custom-binding-project-reference"></a>Aangepaste binding projectverwijzing
Houd rekening met de volgende situaties met een BizTalk-Services-project in Visual Studio-oplossing:  
* Dezelfde Visual Studio-oplossing is een BizTalk-Services-project en een aangepaste binding-project. Het project BizTalk-Service heeft een verwijzing naar dit projectbestand aangepaste binding.
* Het project BizTalk-Service heeft een verwijzing naar een aangepaste/bindingsgedrag dll-bestand.

U Build' ' de oplossing in Visual Studio met succes. Vervolgens 'Opnieuw' of 'Schoon' de oplossing. Na dat, wanneer u opnieuw maken of opnieuw schoon het volgende foutbericht weergegeven:  
  Kan bestand niet kopiëren <Path to DLL> op 'bin\Debug\FileName.dll'. Het proces heeft geen toegang tot het bestand 'bin\Debug\FileName.dll' omdat het door een ander proces wordt gebruikt.  

#### <a name="workaround"></a>Tijdelijke oplossing
* Als u [Visual Studio 2012 Update 3](https://www.microsoft.com/download/details.aspx?id=39305) is geïnstalleerd, hebt u twee mogelijkheden:

  * Start Visual Studio, of

  * De oplossing opnieuw. Voer alleen een Build van de oplossing.  

* Als [Visual Studio 2012 Update 3](https://www.microsoft.com/download/details.aspx?id=39305) is niet geïnstalleerd, Taakbeheer openen, klikt u op het tabblad processen klikt u op het proces MSBuild.exe en klik vervolgens op de knop proces beëindigen.  

### <a name="routing-to-basichttprelay-endpoints-is-not-supported-from-bridges-and-biztalk-services-portal-if-non-printable-characters-are-promoted-as-http-headers"></a>Routering naar de eindpunten van de BasicHttpRelay wordt niet ondersteund van bruggen en BizTalk-Services Portal als niet-afdrukbare tekens worden gepromoveerd tot HTTP-headers

Als u niet-afdrukbare tekens als onderdeel van gepromoveerde eigenschappen voor berichten gebruikt, kunnen niet die berichten worden gerouteerd naar relay bestemmingen die gebruikmaken van de BasicHttpRelay-binding. Ook gepromoveerde eigenschappen die beschikbaar zijn als onderdeel van het bijhouden van URL-gecodeerde voor BLOB's en niet gecodeerd voor bestemmingen.  

### <a name="mdn-is-sent-asynchronously-even-if-the-send-asynchronous-mdn-option-is-unchecked"></a>MDN wordt asynchroon verzonden, zelfs als de optie verzenden asynchrone MDN uitgeschakeld is  
Bekijk dit scenario – als u het selectievakje voor het **verzenden van asynchrone MDN** en geef een URL op voor het verzenden van de asynchrone MDN, en schakel het selectievakje voor het **verzenden van asynchrone MDN** opnieuw de MDN is nog steeds verzonden naar de opgegeven URL, hoewel de optie voor het verzenden van asynchrone MDNs niet is geselecteerd.  
Als tijdelijke oplossing moet u de opgegeven URL wissen voordat u het selectievakje voor het **verzenden van asynchrone MDN** uitschakelt en vervolgens implementeert de AS2-overeenkomst.  

### <a name="whitespace-characters-beyond-a-valid-interchange-cause-an-empty-message-to-be-sent-to-the-suspend-endpoint"></a>Spaties na een geldige interchange ertoe leiden dat een leeg bericht moet worden verzonden naar het eindpunt van de stand-bymodus  
Er zijn whitespaces dan een segment van het IEA, de disassembler wordt dit beschouwd als einde van het huidige knooppunt als wordt gekeken naar de volgende reeks whitespaces als een volgend bericht. Omdat dit geen geldige interchange, kan u zien dat een succesvolle bericht wordt verzonden naar de bestemming van de route en een leeg het eindpunt van de stand-bymodus bericht.  
### <a name="tracking-in-biztalk-services-portal"></a>Volgen in BizTalk-serviceportal  
Bijhouden van gebeurtenissen worden vastgelegd op de verwerking van de EDI-bericht en een correlatie. Als een bericht niet buiten het werkgebied van het Protocol, wordt tracering succesvol weergegeven. Raadpleeg de sectie LOG onder de kolom **Details** in **bijhouden** voor details van fouten in deze situatie.
De X12 instellingen voor verzenden en ontvangen ([maken een X12 overeenkomst in Azure BizTalk-Services](https://msdn.microsoft.com/library/azure/hh689847.aspx)) bieden informatie over het werkgebied Protocol.  

### <a name="update-agreement"></a>Update overeenkomst  
De Portal van BizTalk-Services kunt u de kwalificatie van een identiteit wijzigen wanneer een overeenkomst is geconfigureerd. Dit kan resulteren in het dialoogvenster Eigenschappen is inconsistent. Er is bijvoorbeeld een overeenkomst met ZZ:1234567 en ZZ:7654321 de kwalificatie. In de profielinstellingen van BizTalk-Services Portal kunt u ZZ:1234567 om 01:ChangedValue te wijzigen. U opent de overeenkomst en de 01:ChangedValue wordt weergegeven in plaats van ZZ:1234567.
Wijzigen van de kwalificatie van een identiteit, de overeenkomst verwijderen, **identiteiten** in het Partnerprofiel bijwerken en vervolgens opnieuw maken de overeenkomst.  
> AZURE. Waarschuwing van dit probleem heeft gevolgen voor X12 en AS2.  

### <a name="as2-attachments"></a>AS2-bijlagen  
Berichten worden niet ondersteund in AS2 bijlagen verzenden of ontvangen. Met name bijlagen worden genegeerd en wordt de hoofdtekst van het bericht wordt verwerkt als een regelmatige AS2-bericht.  
### <a name="resources-remembering-path"></a>Bronnen: Pad te onthouden  
Als u **Resources**toevoegt, kan het dialoogvenster het pad dat eerder is gebruikt voor het toevoegen van een bron niet onthouden. De eerder gebruikte pad onthoudt, proberen de BizTalk-Services Portal-website toe te voegen aan de **Vertrouwde websites** in Internet Explorer.  
### <a name="if-you-rename-the-entity-name-of-a-bridge-and-close-the-project-without-saving-changes-opening-the-entity-again-results-in-an-error"></a>Als u de naam van de entiteit van een brug en het project sluit zonder wijzigingen worden opgeslagen, resulteert opnieuw openen van de entiteit in een fout
Houd rekening met een scenario in de volgende volgorde:  
* Een brug (bijvoorbeeld een XML-One-Way brug) toevoegen aan een BizTalk-Service-project  

* Wijzig de naam van de brug door een waarde opgeeft voor de eigenschap naam van de entiteit. Hiermee wijzigt u de bijbehorende .bridgeconfig-bestand met de opgegeven naam.  

* Het bestand .bcs (op het tabblad in Visual Studio sluiten) sluit zonder de wijzigingen worden opgeslagen.  

* Open het bestand .bcs opnieuw in de Solution Explorer.  
U zult merken dat terwijl het bestand gekoppelde .bridgeconfig de nieuwe naam die u hebt opgegeven heeft, de naam van de entiteit in de ontwerpweergave nog steeds de oude naam is. Als u de configuratie Bridge openen probeert door te dubbelklikken op het onderdeel bridge, wordt het volgende foutbericht weergegeven:  
  '<old name>"Entiteit hoort bestand"<old name>.bridgeconfig' bestaat niet  
Als u wilt kunt u voorkomen dat in dit scenario, moet dat u de wijzigingen opslaan nadat u de naam van de entiteiten in een project BizTalk-Service.  
### <a name="biztalk-service-project-builds-successfully-even-if-an-artifact-has-been-excluded-from-a-visual-studio-project"></a>BizTalk-Service automatisch samengesteld is zelfs als een artefact is uitgesloten van een Visual Studio-project
Rekening houden met een scenario waarin u een artefact (bijvoorbeeld een XSD-bestand) toevoegen aan een project BizTalk-Service, die artefact opnemen in de configuratie van de brug (bijvoorbeeld door te geven dat als een berichttype verzoek) en vervolgens uitsluiten van de Visual Studio-project. In dat geval geeft het project bouwen een fout als de verwijderde artefact is beschikbaar op de schijf op dezelfde locatie waar is opgenomen in de Visual Studio-project.
### <a name="the-biztalk-service-project-does-not-check-for-schema-availability-while-configuring-the-bridges"></a>Het project BizTalk-Service niet meer of schema beschikbaarheid tijdens het configureren van de bruggen
In een project BizTalk-Service als u een ander schema Hiermee importeert u een schema dat wordt toegevoegd aan het project wordt het project BizTalk-Service niet gecontroleerd of het geïmporteerde schema wordt toegevoegd aan het project. Als u probeert te maken van een dergelijk project, er eventuele fouten maken.
### <a name="the-response-message-for-a-xml-request-reply-bridge-is-always-of-charset-utf-8"></a>Het antwoordbericht voor een verzoek-antwoord XML-brug is altijd van charset UTF-8
Voor deze release, is de tekenset van het response-bericht van een XML-Request-Reply Bridge altijd ingesteld op UTF-8.
### <a name="user-defined-datatypes"></a>Aangepaste gegevenstypen
Door de gebruiker gedefinieerde gegevenstypen voor adapter bewerkingen kunnen gebruikmaken van de adapters Pack voor BizTalk-Adapter in de functie van BizTalk-Adapter.
Wanneer u door de gebruiker gedefinieerde gegevenstypen, de bestanden (.dll) kopiëren naar station: \Program Files\Microsoft Service\BAServiceRuntime\bin\ van BizTalk-Adapter of op de Cache GAC (Global Assembly) op de server die als host fungeert voor de BizTalk-Adapter-service. Anders wordt worden het volgende foutbericht weergegeven op de client:  
```<s:Fault xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <faultcode>s:Client</faultcode>
  <faultstring xml:lang="en-US">The UDT with FullName "File, FileUDT, Version=Value, Culture=Value, PublicKeyToken=Value" could not be loaded. Try placing the assembly containing the UDT definition in the Global Assembly Cache.</faultstring>
  <detail>
    <AFConnectRuntimeFault xmlns="http://Microsoft.ApplicationServer.Integration.AFConnect/2011" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <ExceptionCode>ERROR_IN_SENDING_MESSAGE</ExceptionCode>
    </AFConnectRuntimeFault>
  </detail>
</s:Fault> ```  
> [AZURE.IMPORTANT] Het verdient GACUtil.exe gebruiken om een bestand in de globale Assembly-Cache installeren. GACUtil.exe documenten voor het gebruik van dit programma en de opdrachtregelopties van Visual Studio.  

### <a name="restarting-the-biztalk-adapter-service-web-site"></a>De website van BizTalk-Adapter-Service opnieuw starten
Installeren van de **BizTalk-Adapter Service Runtime** * maakt de * *Service van BizTalk-Adapter* * website in IIS met de * *BAService* * toepassing.* *BAService** toepassing gebruikt intern relay binding uit te breiden het bereik van lokale service-eindpunt naar de cloud. Voor een gehoste service op-bedrijf, het bijbehorende relay eindpunt geregistreerd op de Bus Service alleen als de service op locatie wordt gestart.  

Als u stopt en een toepassing start, wordt de configuratie voor het automatisch starten van een toepassing niet gehonoreerd. Dus als **BAService** is gestopt, moet u altijd opnieuw de **BizTalk-Adapter Service** website in plaats daarvan. Start of stopt de toepassing **BAService** niet.
### <a name="special-characters-should-not-be-used-for-address-and-entity-names-of-lob-components"></a>Speciale tekens mogen niet worden gebruikt voor adres- en entiteit van LOB-onderdelen
Gebruik speciale tekens niet voor adres- en entiteit van LOB-onderdelen. Als u dit doet, ontvangt u een fout tijdens het implementeren van het project BizTalk-Service. Voor bepaalde tekens zoals '%', de website van BizTalk-Adapter kan gaan in een staat gestopt en moet u deze handmatig starten.
### <a name="test-map-with-get-context-property"></a>Kaart met de eigenschap Context Get testen
Als een Transform een bewerking **Get contexteigenschap** toewijzen bevat, mislukt **Toewijzing Test** . Als tijdelijke oplossing, de **Eigenschap Context ophalen** kaart bewerking worden vervangen door een tekenreeks aaneenschakelen kaart bewerking met POP-gegevens. Dit vullen het doelschema en kunt dat u een andere transformatie functionaliteit testen.
### <a name="test-map-property-does-not-display"></a>De eigenschap toewijzen test wordt niet weergegeven.
De eigenschappen van de **Test Map** weergeven niet in Visual Studio. Dit kan gebeuren als het venster **Eigenschappen** en het venster **Solution Explorer** niet gelijktijdig worden gekoppeld. U kunt dit oplossen door verankeren de **Eigenschappen** en de **Solution Explorer** -vensters.  
### <a name="datetime-reformat-drop-down-is-grayed-out"></a>Vervolgkeuzelijst DateTime formatteren wordt grijs weergegeven
Wanneer een bewerking opnieuw formatteren kaart voor datum/tijd wordt naar het ontwerpvlak toegevoegd en geconfigureerd, de vervolgkeuzelijst indeling mogelijk grijs weergegeven. Dit kan gebeuren als de computer beeldscherm is ingesteld, **Normaal-125%** of **groter – 150%**. Om op te lossen, stelt u de weergave op **kleiner – 100% (standaard)** met behulp van de volgende stappen uit:  
1. Open het **Configuratiescherm** en klik op **Vormgeving aan persoonlijke voorkeur aanpassen**.
2. Klik op **weergeven**.
3. **Laag-100% (standaard)** en klik op **toepassen**.

De vervolgkeuzelijst **indeling** moet nu werken zoals verwacht.
### <a name="duplicate-agreements-in-the-biztalk-services-portal"></a>Dubbele overeenkomsten in de BizTalk-Services Portal
Neem het volgende scenario:
1. Maak een overeenkomst met de Trading Partner beheer OM API.
2. De overeenkomst in de BizTalk-Services Portal in twee verschillende tabbladen openen.
3. Implementatie van de overeenkomst van de beide tabbladen.
4. De beide overeenkomsten krijgen hierdoor geïmplementeerd waardoor dubbele vermeldingen in de BizTalk-Services Portal

**Tijdelijke oplossing**. Open een van de dubbele overeenkomsten in de BizTalk-Services Portal en geïmplementeerd.  

### <a name="bridges-do-not-use-updated-certificate-even-after-a-certificate-has-been-updated-in-the-artifact-store"></a>Bruggen bijgewerkte certificaat niet gebruiken zelfs nadat u een certificaat in het archief artefact is bijgewerkt
Houd rekening met de volgende scenario's:  

**Scenario 1: Vingerafdruk-certificaten gebruiken voor het beveiligen van bericht overdracht van een brug naar een service-eindpunt**  
Neem een scenario waarin u certificaten op basis van een vingerafdruk in uw project BizTalk-Service gebruiken. U het certificaat in de BizTalk-Services Portal bijwerken met dezelfde naam maar een andere vingerafdruk, maar het project BizTalk-Service niet dienovereenkomstig bijwerken. In een scenario blijven de brug kan de berichten niet verwerken omdat de oudere certificaatgegevens mogelijk toch in de cache van het kanaal. Daarna verwerking mislukt.  

**Oplossing**: het certificaat in de BizTalk-Service project bijwerken en implementeren van het project.  

**Scenario 2: Met behulp van gedrag op basis van een naam voor het identificeren van certificaten voor het beveiligen van bericht overdracht van een brug naar een service-eindpunt**

Neem een scenario waarin met gedrag op basis van de naam kunt u certificaten in uw project BizTalk-Service. Het certificaat in de BizTalk-Services Portal werken, maar het project BizTalk-Service niet dienovereenkomstig bijwerken. In een scenario blijven de brug kan de berichten niet verwerken omdat de oudere certificaatgegevens mogelijk toch in de cache van het kanaal. Daarna verwerking mislukt.  

**Oplossing**: het certificaat in de BizTalk-Service project bijwerken en implementeren van het project.  

### <a name="bridges-continue-to-process-messages-even-when-the-sql-database-is-offline"></a>Bruggen blijven berichten verwerken, zelfs als de SQL-database off line is.
De bruggen BizTalk-Services blijven voor het verwerken van berichten voor een tijdje, zelfs als de Microsoft Azure SQL-Database (die de actieve informatie zoals geïmplementeerde artefacten en pijpleidingen wordt opgeslagen), off line is. Dit komt doordat de BizTalk-Services maakt gebruik van de cache artefacten en de brugconfiguratie.
Als u niet dat de bruggen alle berichten te verwerken wilt als de SQL-Database off line is, kunt u de BizTalk-Services PowerShell-cmdlets stoppen of onderbreken van de BizTalk-Service. Zie [Azure BizTalk-Service Management monster](http://go.microsoft.com/fwlink/p/?LinkID=329019) voor de Windows PowerShell-cmdlets voor het beheren van bewerkingen.  
### <a name="reading-the-xml-message-within-a-bridges-custom-code-component-includes-an-extra-bom-character"></a>Lezen van het XML-bericht in een brug van aangepaste code-component bevat een extra teken voor stuklijst
Neem een scenario waar u een XML-bericht van de bridge aangepaste code. Als u de .NET API System.Text.Encoding.UTF8.GetString(bytes) een extra teken voor de stuklijst wordt opgenomen in de uitvoer aan het begin van het bericht. Dus als u niet dat de uitvoer wilt naar de extra BOM-teken bevatten, moet u ```System.IO.StreamReader().ReadToEnd()```.
### <a name="sending-messages-to-a-bridge-using-wcf-does-not-scale"></a>Verzenden van berichten naar een brug met WCF schalen niet
Berichten worden verzonden naar een brug met WCF niet kan worden geschaald. In plaats daarvan moet u het HttpWebRequest gebruiken als u wilt dat een schaalbare client.
### <a name="upgrade-token-provider-error-after-upgrading-from-biztalk-services-preview-to-general-availability-ga"></a>UPGRADE: Token providerfout na een upgrade van de voorvertoning van BizTalk-Services voor algemene beschikbaarheid (GA)
Er is een EDI of AS2 overeenkomst met actieve batches. Als de BizTalk-Service wordt bijgewerkt vanuit voorbeeld naar GA, treedt het volgende:
* Fout: De token provider is niet voorzien van een beveiligingstoken. Token provider bericht geretourneerd: de externe naam kan niet worden omgezet.

* Batchtaken worden geannuleerd.

**Oplossing**: nadat de BizTalk-Service wordt bijgewerkt naar algemene beschikbaarheid (GA), implementeren van de overeenkomst.  

### <a name="upgrade-toolbox-shows-the-old-bridge-icons-after-upgrading-the-biztalk-services-sdk"></a>UPGRADE: Werkset bevat de pictogrammen van de oude brug na de upgrade van de BizTalk-Services SDK
Nadat u een upgrade uitvoert van een eerdere versie van de BizTalk-Services SDK had oude pictogrammen die de bruggen, blijft de werkset de oude pictogrammen voor de bruggen weer te geven. Als u een brug aan de BizTalk-Service project designer oppervlak toevoegt, geeft het oppervlak het nieuwe pictogram.  

**Tijdelijke oplossing**. U kunt dit probleem omzeilen door het verwijderen van de bestanden .tbd onder <system drive>: \Users\<gebruiker > \AppData\Local\Microsoft\VisualStudio\11.0.  

### <a name="upgrade-biztalk-portal-update-from-preview-to-ga-might-show-an-error-indicating-that-the-edi-capability-is-not-available"></a>UPGRADE: Portal BizTalk-update uit het voorbeeld van NH mogelijk weer een foutbericht dat aangeeft dat de mogelijkheid van EDI niet beschikbaar is
Als u bent aangemeld bij de BizTalk-Services Portal terwijl de BizTalk-Services van de voorvertoning wordt bijgewerkt naar NH, kunt u op de portal wordt het volgende foutbericht:  

Deze functionaliteit is niet beschikbaar als onderdeel van deze versie van Microsoft Azure BizTalk-Services. Voor het gebruik van overschakelen deze mogelijkheden naar de juiste versie.  

**Resolutie**: uitloggen uit de portal, sluiten en openen van de browser en meld u vervolgens in de portal.  
### <a name="upgrade-new-tracking-data-does-not-show-up-after-biztalk-services-is-upgraded-to-ga"></a>UPGRADE: Bijhouden van nieuwe gegevens wordt niet weergegeven nadat het BizTalk-Services is bijgewerkt naar NH
Wordt uitgegaan van een scenario waar u een XML-bridge geïmplementeerd op de BizTalk-Services Preview-abonnement hebt. Verzenden van berichten aan de brug en de bijbehorende gegevens bijhouden is beschikbaar op de Portal van BizTalk-Services. Nu, als de runtime-bits BizTalk-Services Portal en BizTalk-Services worden geüpgraded naar NH en u een bericht naar hetzelfde bridge eindpunt eerder ingezet verzenden, het bijhouden van gegevens wordt niet weergegeven voor berichten die worden verzonden na de upgrade.  

### <a name="pipelines-vs-bridges"></a>V/s bruggen van pijpleidingen
In het gehele document, worden de term 'pijpleidingen' en 'bruggen' door elkaar gebruikt. Zowel in wezen betekenis dezelfde, dat is, een bericht processing unit geïmplementeerd op de BizTalk-Services.  

### <a name="concepts"></a>Concepten  

[BizTalk-Services](https://msdn.microsoft.com/library/azure/hh689864.aspx)   
