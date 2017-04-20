<properties
   pageTitle="Zelfstudie: Verwerken EDIFACT facturen met Azure BizTalk-Services | Azure Microsoft BizTalk-Services"
   description="Maken en het vak Connector of API app configureren en gebruiken in een logica in Azure App Service app"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="msftman"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/31/2016"
   ms.author="deonhe"/>

# <a name="tutorial-process-edifact-invoices-using-azure-biztalk-services"></a>Zelfstudie: Proces EDIFACT facturen met Azure BizTalk-Services
U kunt de Portal BizTalk-Services configureren en implementeren van X12 en EDIFACT overeenkomsten. In deze zelfstudie kijken we het maken van een EDIFACT-overeenkomst voor de uitwisseling van facturen tussen handelspartners. Deze zelfstudie is geschreven om een end-to-end oplossing waarbij twee handelspartners, Northwind en Contoso die EDIFACT berichten uitwisselen.  

## <a name="sample-based-on-this-tutorial"></a>Voorbeeld op basis van deze zelfstudie
Deze zelfstudie is geschreven rond een steekproef, het **Verzenden van EDIFACT facturen BizTalk-Services gebruiken**, die beschikbaar is voor downloaden vanaf de [MSDN Code Gallery](http://go.microsoft.com/fwlink/?LinkId=401005). U kan het monster en via deze zelfstudie om te begrijpen hoe het monster is gebouwd. Of u kunt deze zelfstudie voor het maken van uw eigen oplossing a t/m. Deze zelfstudie is gericht op de tweede aanpak, zodat u begrijpt hoe deze oplossing is gebouwd. Ook zo veel mogelijk de zelfstudie is consistent met de sample en gebruikt dezelfde namen voor artefacten (bijvoorbeeld, schema's, transformaties) als in het monster.  

>[AZURE.NOTE] Omdat deze oplossing een bericht verzenden vanuit een EAI bridge naar een EDI-brug omvat, er wordt gebruikgemaakt van het monster [BizTalk-Services Bridge chaining monster](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104) .  

## <a name="what-does-the-solution-do"></a>Wat is de oplossing?

In deze oplossing ontvangt Noordenwind EDIFACT facturen van Contoso. Deze facturen zijn niet in een standaard EDI-indeling. Dus voordat de factuur wordt verzonden naar Noordenwind, moet het worden omgezet naar een document EDIFACT factuur (INVOIC wordt ook genoemd). Na ontvangst moet Noordenwind verwerken de factuur EDIFACT en een besturingsbericht (ook wel CONTRL) terug naar Contoso.

![][1]  

Voor dit bedrijfsscenario, Contoso gebruikmaakt van functies met Microsoft Azure BizTalk-Services.

*   EAI bruggen Contoso gebruikt om te zetten van de oorspronkelijke factuur INVOIC EDIFACT.

*   De brug EAI vervolgens bericht het naar een EDI verzenden brug is geïmplementeerd als onderdeel van een overeenkomst in de BizTalk-Services Portal worden geconfigureerd.

*   De brug EDI verzenden de INVOIC EDIFACT verwerkt en doorgestuurd naar Noordenwind.

*   Na ontvangst van de factuur ontvangen Noordenwind geeft een CONTRL bericht op de EDI bridge geïmplementeerd als onderdeel van de overeenkomst.  

> [AZURE.NOTE] Deze oplossing wordt eventueel ook batchen gebruiken voor het verzenden van facturen in batches in plaats van elke factuur afzonderlijk te verzenden.  

Als u het scenario wilt gebruiken we Service Bus wachtrijen te verzenden factuur van Contoso Noordenwind of bevestiging ontvangt van Noordenwind. Deze wachtrijen worden gemaakt met behulp van een clienttoepassing die is gedownload en wordt opgenomen in het monster pakket dat beschikbaar is als onderdeel van deze zelfstudie.  

## <a name="prerequisites"></a>Vereisten

*   Hebt u een naamruimte Service Bus. Zie voor meer informatie over het maken van een naamruimte [How To: maken of wijzigen van een Service Bus Service Namespace](https://msdn.microsoft.com/library/azure/hh674478.aspx). Laat het ons wordt ervan uitgegaan dat er al een naamruimte Service Bus is ingericht, **edifactbts**genoemd.

*   Hebt u een abonnement BizTalk-Services. Zie [een BizTalk-Service maken met behulp van Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=302280)voor instructies. Voor deze zelfstudie laat ons u uitgaan van een BizTalk-abonnement, **contosowabs**genoemd.

*   Registreer uw abonnement BizTalk-Services op de Portal van BizTalk-Services. Zie voor instructies [de implementatie van een BizTalk-Service op de Portal van BizTalk-Services registreren](https://msdn.microsoft.com/library/hh689837.aspx)

*   Visual Studio is geïnstalleerd, moet u hebben.

*   U hebt de BizTalk-Services SDK is geïnstalleerd. U kunt de SDK downloaden van [http://go.microsoft.com/fwlink/?LinkId=235057](http://go.microsoft.com/fwlink/?LinkId=235057)  

## <a name="step-1-create-the-service-bus-queues"></a>Stap 1: De Service Bus wachtrijen maken  
Deze oplossing wordt Service Bus wachtrijen gebruikt voor het uitwisselen van berichten tussen handelspartners. Contoso en Noordenwind verzenden berichten naar de wachtrijen van waar de bruggen EAI en/of EDI ze verbruiken. Voor deze oplossing moet u drie Service Bus wachtrijen:

*   **northwindreceive** – Noordenwind ontvangt de factuur van Contoso via deze wachtrij.

*   **contosoreceive** : Contoso de bevestiging ontvangt van Noordenwind op deze wachtrij.

*   **geschorst** – geschorst alle berichten worden doorgestuurd naar deze wachtrij. Berichten worden geschorst indien ze niet tijdens de verwerking.

Deze Service Bus wachtrijen kunt u maken met behulp van een clienttoepassing die is opgenomen in het pakket monster.  

1.  Open in de locatie waar u het voorbeeld hebt gedownload, **Zelfstudie verzenden van facturen met behulp van BizTalk-Services EDI Bridges.sln**.

2.  Druk op **F5** en start de toepassing **Client voor zelfstudie** .

3.  Voer de Service Bus ACS-naamruimte, uitgever naam en sleutel van de uitgevende instelling in het scherm.

    ![][2]  
4.  Een bericht waarin drie wachtrijen gemaakt in de naamruimte Service Bus. Klik op **OK**.

5.  Laat de zelfstudie Client wordt uitgevoerd. Open het, klik op **Service Bus** > **_uw naamruimte Service Bus_** > **wachtrijen**, en controleer of de drie wachtrijen worden gemaakt.  

## <a name="step-2-create-and-deploy-trading-partner-agreement"></a>Stap 2: Maken en implementeren van trading partnerovereenkomst
Maak een trading partnerovereenkomst tussen Contoso en Noordenwind. Een trading partnerovereenkomst definieert een contract voor handel tussen de twee zakelijke partners, zoals welke het schema moet worden gebruikt, welke messaging protocol te gebruiken, enz. Een trading partnerovereenkomst bevat twee EDI bruggen, één om berichten te verzenden naar handelspartners (de **brug EDI verzenden**genoemd) en één om berichten te ontvangen van handelspartners (de **brug EDI wordt**genoemd).

In het kader van deze oplossing is de brug EDI verzenden komt overeen met de verzenden-kant van de overeenkomst en wordt gebruikt voor het verzenden van de factuur EDIFACT van Contoso naar Noordenwind. Ook de brug EDI ontvangen komt overeen met de ontvangen-kant van de overeenkomst en wordt gebruikt om ontvangen bevestigingen van Noordenwind.  

### <a name="create-the-trading-partners"></a>Maak de handelspartners

Maak eerst handelspartners voor Contoso en Noordenwind.  

1.  Klik op **toevoegen**in het Portal BizTalk-Services op het tabblad **Partners** .

2.  **Contoso** opgeven als de Partnernaam van een in de nieuwe partner pagina en klik op **Opslaan**.

3.  Herhaal de stap maken van de tweede partner **Noordenwind**.  

### <a name="create-the-agreement"></a>De overeenkomst maken
Trading partner overeenkomsten worden gemaakt tussen zakelijke profielen van de handelspartners. Deze oplossing gebruikt de standaardprofielen partner die automatisch worden gemaakt wanneer we de partners hebben gemaakt.  

1.  Klik in de Portal diensten van BizTalk- **overeenkomsten** > **toevoegen**.

2.  In de nieuwe overeenkomst pagina **Algemene instellingen** opgeven, zoals in de onderstaande afbeelding en klik op **Doorgaan**.

    ![][3]  

    Nadat u op **Doorgaan**klikt, worden twee tabbladen, **Instellingen voor ontvangen** en **Verzenden** beschikbaar.

3.  De overeenkomst verzenden tussen Contoso en Noordenwind maken. Deze overeenkomst bepaalt hoe Contoso de EDIFACT factuur stuurt naar Noordenwind.

    1.  Klik op **verzenden**.

    2.  Behouden de standaardwaarden op de **Inkomende URL**, **Transformeren**en **Batching** tabbladen.

    3.  Upload het schema van de **EFACT_D93A_INVOIC.xsd** op het tabblad **Protocol** onder de sectie **schema's** . Dit schema is beschikbaar bij het monster pakket.

        ![][4]  
    4.  Geef de details voor de wachtrijen Bus Service op het tabblad **Transport** . We gebruiken de **northwindreceive** wachtrij voor het verzenden van de factuur EDIFACT naar Noordenwind en de wachtrij **onderbroken** voor het routeren van berichten die tijdens de verwerking is mislukt en worden geschorst voor de overeenkomst verzenden kant. U hebt gemaakt van deze wachtrijen in **stap 1: de Service Bus wachtrijen maken** (in dit onderwerp).

        ![][5]  

        Onder **Transport instellingen > Transport type** en **schorsing instellingen > type Transport**, selecteer Azure Service Bus en waarden zoals in de afbeelding.

4.  De overeenkomst ontvangen tussen Contoso en Noordenwind maken. Deze overeenkomst bepaalt hoe Contoso de bevestiging ontvangt van Noordenwind.

    1.  Klik op **Instellingen voor ontvangen**.

    2.  Behouden de standaardwaarden op de tabbladen **Transport** en **transformatie** .

    3.  Upload het schema van de **EFACT_4.1_CONTRL.xsd** op het tabblad **Protocol** onder de sectie **schema's** . Dit schema is beschikbaar bij het monster pakket.

    4.  Maak een filter om ervoor te zorgen dat alleen de bevestigingen van Noordenwind worden gerouteerd naar Contoso op het tabblad **Route** . Klik op **toevoegen** om de routering filter te maken onder **Route instellingen**.

        ![][6]  
        1.  Zoals in de afbeelding wordt weergegeven, moet u waarden opgeven voor de **Naam voor de regel**, **regel Route**en **bestemming van de Route** .

        2.  Klik op **Opslaan**.

    5.  Klik op het tabblad **Route** opnieuw opgeven waar de geschorste bevestigingen (bevestigingen tijdens de verwerking mislukt) worden doorgestuurd naar. Het transporttype ingesteld op Azure Service Bus, type bestemming route **wachtrij**verificatietype **Gedeelde Access-handtekening** (SAS), de verbindingsreeks SAS leveren voor de naamruimte Service Bus en voer vervolgens de naam van de wachtrij als **geschorst**.

5.  Klik ten slotte op **implementeren** voor de implementatie van de overeenkomst. Let op de eindpunten waarbij het verzenden en ontvangen van overeenkomsten krijgen geïmplementeerd.

    *   Let op het tabblad **Instellingen voor verzenden** onder **Inkomende URL**, het eindpunt. Voor het verzenden van een bericht van Contoso naar Noordenwind met behulp van de bridge EDI verzenden, moet u een bericht verzenden naar dit eindpunt.

    *   Let op het tabblad **Instellingen ontvangen** bij **vervoer**, het eindpunt. Een bericht verzenden vanuit Noordenwind naar Contoso bridge, met behulp van de EDI ontvangen moet u een bericht verzenden naar dit eindpunt.  

## <a name="step-3-create-and-deploy-the-biztalk-services-project"></a>Stap 3: Maak en het project BizTalk-Services implementeren

In de vorige stap geïmplementeerd de EDI-verzenden en ontvangen van overeenkomsten voor het verwerken van facturen EDIFACT en bevestigingen. Deze overeenkomsten kunnen alleen berichten die aan de standaard EDIFACT berichtschema voldoen. Echter per het scenario voor deze oplossing stuurt Contoso een factuur naar Noordenwind.mdb in een intern eigen schema. Dus voordat het bericht wordt verzonden naar de brug EDI verzenden, moet deze worden omgezet in het interne schema aan het schema standaard EDIFACT factuur. De BizTalk-Services EAI-project doet dat.

Het project BizTalk-Services, **InvoiceProcessingBridge**, waarmee het bericht omgezet is ook deel uit van het monster die u hebt gedownload. Het project omvat de volgende onderdelen:

*   **INHOUSEINVOICE. XSD** – Schema van de interne factuur die wordt verzonden naar Noordenwind.

*   **EFACT_D93A_INVOIC. XSD** – Schema van de standaard EDIFACT factuur.

*   **EFACT_4.1_CONTRL. XSD** – Schema van de ontvangstbevestiging EDIFACT die Noordenwind naar Contoso verzendt.

*   **INHOUSEINVOICE_to_D93AINVOIC. TRFM** – de transformatie die de interne factuur schema aan het schema standaard EDIFACT factuur.  

### <a name="create-the-biztalk-services-project"></a>De BizTalk-Services-project maken
1.  Vouw het InvoiceProcessingBridge-project in de Visual Studio-oplossing en open vervolgens het bestand **MessageFlowItinerary.bcs** .

2.  Klik op het canvas en de **URL van BizTalk-Service** instellen in het vak van de eigenschap geeft de naam van uw abonnement BizTalk-Services. Bijvoorbeeld `https://contosowabs.biztalk.windows.net`.

    ![][7]  
3.  Vanuit de werkset een **XML-One-Way Bridge** naar het canvas te slepen. Stel de **Naam van de entiteit** en het **Relatieve adres** eigenschappen van de brug op **ProcessInvoiceBridge**. Dubbelklik op **ProcessInvoiceBridge** om het oppervlak van de configuratie van bridge openen.

4.  Klik op het plusteken in het vak **Berichttypen** (**+**) om het schema van het binnenkomende bericht opgeven. Omdat het binnenkomende bericht voor de brug EAI altijd de factuur zelf, kunt u deze naar de **INHOUSEINVOICE**instellen.

    ![][8]  
5.  Klik op de vorm van **XML-transformatie** en klik op de knop met het weglatingsteken (**...**) in het vak voor de eigenschap **kaarten** . Klik in het dialoogvenster **Selectie kaarten** het transformatiebestand **INHOUSEINVOICE_to_D93AINVOIC** selecteren en klik op **OK**.

    ![][9]  
6.  Ga terug naar **MessageFlowItinerary.bcs**en van de werkset, sleept u een **Externe Service-eindpunt Two-Way** rechts van de **ProcessInvoiceBridge**. De eigenschap **Naam van entiteit** instellen op **EDIBridge**.

7.  Vouw het **MessageFlowItinerary.bcs** in de Solution Explorer en dubbelklik op het bestand **EDIBridge.config** . De inhoud van de **EDIBridge.config** vervangen door het volgende.

    > [AZURE.NOTE] Waarom moet ik het .config-bestand bewerken De externe service-eindpunt dat is toegevoegd aan de brug designer canvas vertegenwoordigt de EDI-bruggen die we eerder geïmplementeerd. EDI bruggen zijn bilaterale bruggen, met een verzenden en ontvangen aan de zijkant. De EAI brug die we toegevoegd aan de ontwerper van de brug is een brug in één richting. Dus voor het verwerken van de berichten exchange patronen van de twee bruggen, we gebruiken een aangepaste bridge gedrag door de configuratie in het .config-bestand. Bovendien zorgt aangepaste gedrag ook voor de verificatie van het eindpunt van EDI verzenden bridge. Dit gedrag is beschikbaar als een afzonderlijke monster bij [BizTalk-Services Bridge chaining monster - EAI naar EDI](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104). Deze oplossing wordt gebruikgemaakt van het monster.  
    
    ```
<?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <extensions>
          <behaviorExtensions>
            <add name="BridgeAuthentication" 
                  type="Microsoft.BizTalk.Bridge.Behaviour.BridgeBehaviorElement, Microsoft.BizTalk.Bridge.Behaviour, Version=1.0.0.0, Culture=neutral, PublicKeyToken=ae58f69b69495c05" />
          </behaviorExtensions>
        </extensions>
        <behaviors>
          <endpointBehaviors>
            <behavior name="BridgeAuthenticationConfiguration">
              <!-- Enter the ACS namespace, issuer name and issuer secret of the BizTalk Services deployment -->
              <BridgeAuthentication acsnamespace="[YOUR ACS NAMESPACE]" 
                                    issuername="owner" 
                                    issuersecret="[YOUR ACS SECRET]" />
              <webHttp />
            </behavior>
          </endpointBehaviors>
        </behaviors>
        <bindings>
          <webHttpBinding>
            <binding name="BridgeBindingConfiguration">
              <security mode="Transport" />
            </binding>
          </webHttpBinding>
        </bindings>
        <client>
          <clear />
          <!--
            Go BizTalk Portal > Agreement > Send Settings > Inbound URL
            Copy the Endpoint URL and paste it in the below address field
          -->
          <endpoint name="TwoWayExternalServiceEndpointReference1" 
                    address="[YOUR EDI BRIDGE SEND URI]" 
                    behaviorConfiguration="BridgeAuthenticationConfiguration" 
                    binding="webHttpBinding" 
                    bindingConfiguration="BridgeBindingConfiguration" 
                    contract="System.ServiceModel.Routing.IRequestReplyRouter" />
        </client>
      </system.serviceModel>
    </configuration>

    ```
8.  Update het bestand EDIBridge.config op te nemen gegevens in de netwerkconfiguratie

    *   Onder _<behaviors>_, de ACS-naamruimte en de sleutel die is gekoppeld aan het abonnement BizTalk-Services.

    *   Onder _<client>_, biedt het eindpunt waarin de overeenkomst EDI verzenden is geïmplementeerd.

    Wijzigingen opslaan en sluiten van het configuratiebestand.

9.  Klik op de **verbindingslijn** en deelnemen aan de onderdelen **ProcessInvoiceBridge** en **EDIBridge** vanuit de werkset. Selecteert u de verbindingslijn en in eigenschappen voor Stel **Filter** op **Alles afstemmen**. Dit zorgt ervoor dat alle berichten die door de brug EAI verwerkt worden gerouteerd naar de EDI-brug.

    ![][10]  
10.  Wijzigingen opslaan in de oplossing.  

### <a name="deploy-the-project"></a>Implementatie van het project

1.  Op de computer waarop u de BizTalk-Services-project hebt gemaakt, download en installeer het SSL-certificaat voor uw abonnement BizTalk-Services. Uit, klik op **Dashboard**onder BizTalk-Services en klik op **SSL-certificaat downloaden**. Dubbelklik op het certificaat en volg de prompt om de installatie te voltooien. Zorg ervoor dat u installeert het certificaat in het certificaatarchief **Vertrouwde basiscertificeringsinstanties** .

2.  In Visual Studio Solution Explorer met de rechtermuisknop op het **InvoiceProcessingBridge** -project en klik op **Deploy**.

3.  De waarden bevatten, zoals in de afbeelding en klik op **Deploy**. U kunt de referenties van de ACS voor BizTalk-Services ophalen door te klikken op **De verbindingsgegevens** uit het dashboard BizTalk-Services.

    ![][11]  

    Kopiëren vanuit het deelvenster Uitvoer het eindpunt waar de brug EAI is geïmplementeerd, bijvoorbeeld `https://contosowabs.biztalk.windows.net/default/ProcessInvoiceBridge`. U moet dit eindpunt-URL later.  

## <a name="step-4-test-the-solution"></a>Stap 4: Test de oplossing


In dit onderwerp bekijken we hoe de oplossing testen met de **Zelfstudie** -clienttoepassing die deel uitmaken van het monster.  

1.  Druk op F5 om de **Zelfstudie Client**starten in Visual Studio.

2.  Het scherm moet hebben de waarden vooraf ingevuld in de stap waar we de Service Bus wachtrijen gemaakt. Klik op **volgende**.

3.  In het volgende venster ACS-referenties voor de BizTalk-abonnement en de eindpunten opgeven waar EAI en EDI (ontvangen) bruggen zijn geïmplementeerd.

    U hebt het eindpunt van de brug EAI gekopieerd in de vorige stap. Voor EDI bridge-eindpunt in de Portal BizTalk-Services ontvangt, gaat u naar de overeenkomst > Instellingen ontvangen > Transport > eindpunt.

    ![][12]  
4.  In het volgende venster klikt u op **De interne factuur** sturen klikt, Contoso. In het bestand in het dialoogvenster openen, opent u het bestand INHOUSEINVOICE.txt. De inhoud van het bestand en klik vervolgens op **OK** om de factuur te sturen.

    ![][13]  
5.  In een paar seconden wordt de factuur ontvangen voor Noordenwind. Klik op de koppeling **Bericht weergeven** als u wilt zien van de factuur ontvangen door Noordenwind. U ziet hoe de factuur ontvangen door Noordenwind in schema standaard EDIFACT terwijl een verzonden door Contoso een eigen schema is.

    ![][14]  
6.  Selecteer de factuur en klik op **Bevestiging sturen**. In het dialoogvenster dat wordt weergegeven, ziet dat de knooppunt-ID dezelfde in de ontvangen factuur en de bevestiging wordt verzonden. Klik op OK in het dialoogvenster voor **Bevestiging sturen** .

    ![][15]  
7.  In een paar seconden is de bevestiging ontvangen bij Contoso.

    ![][16]  

## <a name="step-5-optional-send-edifact-invoice-in-batches"></a>Stap 5 (optioneel): de factuur verzenden EDIFACT in batches 
BizTalk-Services EDI bruggen ondersteunt ook batchen van uitgaande berichten. Deze functie is handig voor het ontvangen van partners die een reeks berichten (voldoen aan bepaalde criteria) in plaats van afzonderlijke berichten ontvangt.

Het belangrijkste aspect bij het werken met batches is de werkelijke release van de partij, ook wel de criteria voor de release. De release criteria kunnen worden gebaseerd op de manier waarop de ontvangende partner wil ontvangen van berichten. Als de batchverwerking is ingeschakeld, verzendt de brug EDI geen het uitgaande bericht aan de ontvangende partner totdat is voldaan aan de criteria voor de release. Bijvoorbeeld een batchen criteria op basis van bericht grootte verzendingen een batch alleen als "n" berichten zijn batch verwerkt. Een batchcriteria zijn op basis van tijd, zodat een batch wordt verzonden op een vaste tijd elke dag. In deze oplossing probeert u de grootte van het bericht gebaseerde criteria.

1.  Klik op de overeenkomst die u eerder hebt gemaakt in het Portal voor BizTalk-Services. Klik op verzendinstellingen > batchen > Batch toevoegen.

2.  **InvoiceBatch**invoeren voor batchnaam, een beschrijving en klik op **volgende**.

3.  Een batchcriteria opgeven, waarmee wordt gedefinieerd welke berichten aan een batch worden geplaatst. In deze oplossing batch we alle berichten. Zo schakelt u de geavanceerde optie definities gebruiken en voer **1 = 1**. Dit is een voorwaarde die altijd true en daarmee alle berichten batch wordt verwerkt. Klik op **volgende**.

    ![][17]  
4.  Een batch release criteria opgeven. Selecteer **MessageCountBased**uit de keuzelijst en geef voor **aantal**, **3**. Dit betekent dat een partij van de drie berichten worden verzonden naar Noordenwind. Klik op **volgende**.

    ![][18]  
5.  Bekijk het overzicht en klik op **Opslaan**. Klik op **Deploy** om het implementeren van de overeenkomst.

6.  Ga terug naar de **Client zelfstudie** **Interne factuur verzenden**en volg de aanwijzingen voor het verzenden van de factuur. U ziet dat de factuur niet is ontvangen op Noordenwind omdat niet is voldaan aan de omvang van de partij. Herhaal deze stap twee keer, zodat u beschikt over drie factuur worden verzonden naar Noordenwind. Dit voldoet aan de criteria voor het vrijgeven van batch 3 berichten en u ziet nu een factuur op Noordenwind.


<!--Image references-->
[1]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-1.PNG  
[2]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-2.PNG  
[3]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-3.PNG
[4]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-4.PNG  
[5]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-5.PNG  
[6]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-6.PNG  
[7]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-7.PNG  
[8]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-8.PNG
[9]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-9.PNG  
[10]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-10.PNG  
[11]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-11.PNG  
[12]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-12.PNG  
[13]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-13.PNG
[14]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-14.PNG  
[15]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-15.PNG  
[16]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-16.PNG  
[17]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-17.PNG  
[18]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-18.PNG

