<properties
    pageTitle="Foutopsporing in Azure toepassingen in Eclips"
    description="Meer informatie over de Azure foutopsporing in toepassingen met behulp van de Toolkit Azure voor Eclips."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690949.aspx -->

# <a name="debugging-azure-applications-in-eclipse"></a>Foutopsporing in Azure toepassingen in Eclips #

Met de Toolkit Azure voor Eclips, kunt u fouten opsporen uw toepassingen of ze in Azure, of in de compute-emulator worden uitgevoerd als u Windows gebruikt als uw besturingssysteem. De volgende afbeelding ziet u de **Foutopsporing** eigenschappen dialoogvenster gebruikt voor foutopsporing op afstand inschakelen:

![][ic719504]

In deze zelfstudie wordt ervan uitgegaan dat er al een toepassing die is gemaakt en zijn bekend met de compute-emulator en distribueren naar Azure.

Gebruiken we de toepassing van de zelfstudie [met behulp van de Azure-runtimebibliotheek in JSP][] als uitgangspunt voor dit onderwerp. Voordat u doorgaat, maakt u die toepassing als u dat nog niet hebt gedaan.

## <a name="to-debug-your-application-while-running-in-azure"></a>Voor de toepassing tijdens het uitvoeren in Azure foutopsporing ##

>[AZURE.WARNING] De toolkit van de huidige ondersteuning voor externe foutopsporing in Java is voornamelijk bedoeld voor installaties die worden uitgevoerd in de emulator Azure berekenen. Omdat de foutopsporing verbinding niet beveiligd is, moet u niet inschakelen externe foutopsporing in de productie-implementaties. Als u fouten opsporen in een toepassing wordt uitgevoerd in de Azure cloud, gebruik van een staging-implementatie, maar realiseert zich dat onbevoegde toegang tot de debug-sessie mogelijk als iemand weet dat het IP-adres van uw cloud-implementatie, zelfs als het een staging-implementatie.

1. Het project voor het testen in de emulator bouwen: In Eclips Project Explorer met de rechtermuisknop op **MyAzureProject**, klik op **Eigenschappen**, op **Azure**, en stel **bouwen voor** op **implementatie cloud**.
1. Het project opnieuw: in het menu Eclips **Project**Klik **Alle bouwen**.
1. Implementeer de toepassing *klaarzetten* in Azure.
    >[AZURE.IMPORTANT] Zoals hierboven vermeld, is het raadzaam u foutopsporing in de compute-emulator in de meeste gevallen en vervolgens alleen als extra foutopsporing nodig is voor foutopsporing in de testomgeving. Het is raadzaam geen fouten opsporen in de productieomgeving.
1. Zodra de installatie klaar in Azure is, ophalen met de DNS-naam voor de implementatie van de [Azure Management Portal][]. Een gefaseerde installatie implementatie is een DNS-naam in de vorm van http://*&lt;guid&gt;*. cloudapp.net, waar * &lt;guid&gt; * is de waarde van een GUID toegewezen door Azure.
1. In de Projectverkenner van de Eclips, met de rechtermuisknop op **WorkerRole1**, klik op **Azure**en klik op **Foutopsporing**.
1. In het dialoogvenster **Eigenschappen voor het debuggen van WorkerRole1** :
    1. Controleer **Foutopsporing op afstand voor deze functie inschakelen.**
    1. Voor **Input eindpunt te gebruiken**, gebruikt u **Foutopsporing (openbare: 8090, privé: 8090)**.
    1. Controleer de **Dat JVM Start in de onderbroken modus, wacht op een verbinding voor foutopsporing** is uitgeschakeld.
        >[AZURE.IMPORTANT] De optie **JVM Start in de onderbroken modus, wacht op een verbinding debugger** is bedoeld voor geavanceerde scenario's in de compute-emulator alleen foutopsporing (niet voor cloud implementaties). Als de optie **JVM Start in de onderbroken modus, wacht op de verbinding van een foutopsporingsprogramma** wordt gebruikt, wordt het opstartproces van de server onderbreken totdat de debugger Eclips is verbonden met de JVM. Als u deze optie voor een sessie voor foutopsporing via de compute-emulator gebruiken, gebruik deze niet voor een sessie voor foutopsporing in een cloud-implementatie. De initialisatie van een server vindt plaats in een taak Azure opstarten en de Azure cloud maakt geen openbare eindpunten beschikbaar totdat de taak opstarten is voltooid. Daarom een opstartprocedure niet worden voltooid als deze optie is ingeschakeld in de implementatie van een wolk, omdat het niet kan ontvangen van een verbinding vanaf een externe client van de Eclips.
1. Klik op **Foutopsporing configuraties maken**.
1. Klik in het dialoogvenster **Configuratie van Azure Debug** :
    1. Selecteer voor **Java-project voor foutopsporing**, het **MyHelloWorld** -project.
    1. Raadpleeg **configureren voor foutopsporing**, **Azure cloud (tijdelijke)**.
    1. Controleer **dat Azure emulator berekenen** niet is ingeschakeld.
    1. Geef de DNS-naam van de gefaseerde implementatie, maar zonder de voorafgaande **http://**voor **Host**. Bijvoorbeeld (Gebruik de GUID in plaats van de GUID die hier worden weergegeven): **4e616d65-6f6e-6 d 65 6973 526f62657274.cloudapp.net**
1. Klik op **OK** om de **Configuratie van Azure Debug** dialoogvenster te sluiten.
1. Klik op **OK** om te sluiten van het dialoogvenster met **Eigenschappen voor het debuggen van WorkerRole1** .
1. Als er geen een onderbrekingspunt in index.jsp zijn ingesteld, stelt u deze:
    1. In de Projectverkenner van de Eclips Vouw van **MyHelloWorld**, **webinhoud**uit en dubbelklik op **index.jsp**.
    1. In index.jsp, met de rechtermuisknop op de blauwe balk aan de linkerkant van de Java-code en klik op de **Onderbrekingspunten in-of uitschakelen**, zoals in het volgende: ![][ic551537]
1. Binnen de Eclips menu, klikt u op **uitvoeren** en klik vervolgens op **Debug configuraties**.
1. In het dialoogvenster **Foutopsporing configuraties** , vouw **Externe Java-toepassing** in het linkerdeelvenster klikt u op de **Debug**en selecteer **Azure Cloud (WorkerRole1)**.
1. Uw browser, voeren de gefaseerde toepassing, **http://***&lt;guid&gt;***.cloudapp.net/MyHelloWorld**, vervangen door de GUID van de DNS-naam voor * &lt;guid&gt;*. Als u wordt gevraagd een dialoogvenster **Perspectief Switch bevestigen** , klikt u op **Ja**. Uw sessie voor foutopsporing moet nu worden uitgevoerd naar de coderegel waarin het onderbrekingspunt is ingesteld.

>[AZURE.NOTE] Als u probeert te starten van een externe worden verbinding aan een distributie met meerdere exemplaren van rol met, kunt u geen momenteel beheren welke instantie foutopsporing foutopsporing in eerste instantie verbonden met, zoals de Azure taakverdeling een exemplaar in willekeurige volgorde kiezen. Als u met die sessie verbonden bent, maar blijft u foutopsporing hetzelfde exemplaar. Opmerking ook als er een periode van inactiviteit van meer dan 4 minuten (bijvoorbeeld, als u bent gestopt bij een onderbrekingspunt te lang), Azure kan de verbinding niet sluiten.

## <a name="debugging-a-specific-role-instance-in-a-multi-instance-deployment"></a>Opsporen van fouten in een specifieke rol-instantie in een distributie met meerdere instanties ##

Wanneer de installatie wordt uitgevoerd in de cloud, zal u waarschijnlijk worden uitgevoerd het in compute, of rol, exemplaren. Hierdoor kunt u profiteren van de garanties voor de Azure 99.95% beschikbaarheid en voor het schalen van uw toepassing.

In zulke gevallen moet u foutopsporing op afstand uw Java-toepassing in het geval van een specifieke rol. Echter als u alleen een regelmatige invoer eindpunt voor foutopsporing inschakelt, wordt de Azure taakverdeling maken het vrijwel onmogelijk voor u de debugger verbinden met een specifieke rol-instantie. In plaats daarvan zal het u verbinding met een exemplaar van rol die deze in willekeurige volgorde opgehaald.

Dit is het soort scenario waar profiteren van invoer eindpunten exemplaar te vergemakkelijken kunt u fouten opsporen in een specifieke rol exemplaar.

Stel dat u van plan bent om maximaal 5 rol exemplaren van uw implementatie uitvoeren. Met behulp van de eigenschappenpagina van de **eindpunten** in het dialoogvenster met eigenschappen voor de rol, een instantie invoer eindpunt maken en hieraan een reeks openbare poorten in plaats van een getal met één poort. Geef bijvoorbeeld in het invoervak **openbare poort** **81-85**.

Nadat u uw toepassing met het eindpunt van deze instantie geïmplementeerd, wordt Azure een uniek poortnummer uit dit bereik toegewezen aan elk van de exemplaren van de rol. Klik, om te weten welke instantie welke poortnummers u hebt toegewezen, kunt u de omgevingsvariabele *InstanceEndpointName***_PUBLICPORT** (waarbij *InstanceEndpointName* de naam die u bij het maken van het eindpunt van de instantie toegewezen) automatisch geconfigureerd door de toolkit in uw implementatie (bijvoorbeeld door de waarde te retourneren in de voettekst van een webpagina, zodat het kan lezen wanneer u bezoekt).

Als u eenmaal weet welke openbare poortnummer is dat exemplaar toegewezen, u kunt ernaar verwijzen in uw configuratie foutopsporing in Eclips, door het aanbrengen van deze naar de hostnaam van uw service. Hiermee schakelt u de debugger Eclips verbinding maken met dat specifieke exemplaar en niet een van de andere exemplaren.

## <a name="windows-only-to-debug-your-application-while-running-in-the-compute-emulator"></a>Alleen Windows: voor foutopsporing van de toepassing tijdens het uitvoeren in de compute-emulator ##

>[AZURE.NOTE] De Azure-emulator is alleen beschikbaar op Windows. Als u een besturingssysteem dan Windows gebruikt, kunt u deze sectie overslaan. 

1. Het project voor het testen in de emulator bouwen: In Eclips Project Explorer met de rechtermuisknop op **MyAzureProject**, klikt u op **Eigenschappen**klikt u op **Azure**en instellen **voor opbouwen** om te **testen in de emulator**.
1. Het project opnieuw: in het menu Eclips **Project**Klik **Alle bouwen**.
1. In de Projectverkenner van de Eclips, met de rechtermuisknop op **WorkerRole1**, klik op **Azure**en klik op **Foutopsporing**.
1. In het dialoogvenster **Eigenschappen voor het debuggen van WorkerRole1** :
    1. Controleer **Foutopsporing op afstand voor deze functie inschakelen.**
    1. Voor **invoer eindpunt te gebruiken**, gebruiken de standaardeindpunt automatisch gegenereerd door de toolkit, vermeld als **Foutopsporing (openbare: 8090, privé: 8090)**.
    1. Controleer dat de optie **JVM Start in de onderbroken modus, wacht op een verbinding voor foutopsporing** is uitgeschakeld.
        >[AZURE.IMPORTANT] De optie **JVM Start in de onderbroken modus, wacht op een verbinding debugger** is bedoeld voor geavanceerde scenario's in de compute-emulator alleen foutopsporing (niet voor cloud implementaties). Als de optie **JVM Start in de onderbroken modus, wacht op de verbinding van een foutopsporingsprogramma** wordt gebruikt, wordt het opstartproces van de server onderbreken totdat de debugger Eclips is verbonden met de JVM. Als u deze optie voor een sessie voor foutopsporing via de compute-emulator gebruiken, gebruik deze niet voor een sessie voor foutopsporing in een cloud-implementatie. De initialisatie van een server vindt plaats in een taak Azure opstarten en de Azure cloud maakt geen openbare eindpunten beschikbaar totdat de taak opstarten is voltooid. Daarom een opstartprocedure niet worden voltooid als deze optie is ingeschakeld in de implementatie van een wolk, omdat het niet kan ontvangen van een verbinding vanaf een externe client van de Eclips.
1. Klik op **Foutopsporing configuraties maken**.
1. Klik in het dialoogvenster **Configuratie van Azure Debug** :
    1. Selecteer voor **Java-project voor foutopsporing**, het **MyHelloWorld** -project.
    1. Raadpleeg **configureren voor foutopsporing**, **Azure emulator berekenen**.
1. Klik op **OK** om de **Configuratie van Azure Debug** dialoogvenster te sluiten.
1. Klik op **OK** om te sluiten van het dialoogvenster met **Eigenschappen voor het debuggen van WorkerRole1** .
1. Hiermee stelt u een onderbrekingspunt in index.jsp:
    1. In de Projectverkenner van de Eclips Vouw van **MyHelloWorld**, **webinhoud**uit en dubbelklik op **index.jsp**.
    1. In index.jsp, met de rechtermuisknop op de blauwe balk aan de linkerkant van de Java-code en klik op de **Onderbrekingspunten in-of uitschakelen**, zoals in het volgende: ![][ic551537]

       Een onderbrekingspunt is ingesteld als er een pictogram van een onderbrekingspunt in de blauwe balk links van de code van Java.
1. Start de toepassing in de emulator berekenen door te klikken op de knop **uitvoeren in Azure Emulator** op de werkbalk Azure.
1. Binnen de Eclips menu, klikt u op **uitvoeren** en klik vervolgens op **Debug configuraties**.
1. In het dialoogvenster **Foutopsporing configuraties** , vouw **Externe Java-toepassing** in het linkerdeelvenster en selecteer **Azure Emulator (WorkerRole1)** **Zie Debug**.
1. Nadat de compute-emulator geeft aan dat de toepassing wordt uitgevoerd, in uw browser, **http://localhost:8080/MyHelloWorld**worden uitgevoerd.
    Als u wordt gevraagd een dialoogvenster **Perspectief Switch bevestigen** , klikt u op **Ja**.
    Uw sessie voor foutopsporing moet nu worden uitgevoerd naar de coderegel waarin het onderbrekingspunt is ingesteld.

Hierdoor hebt u geleerd hoe u foutopsporing in de compute-emulator de volgende sectie wordt beschreven hoe u fouten opsporen in een toepassing naar Azure geïmplementeerd.

## <a name="debugging-notes"></a>Foutopsporing in notities ##

* Na de foutopsporing, kunt u het perspectief van **Foutopsporing** op **Java** schakelen via te klikken op de Eclips menu door te klikken op **venster** **Open perspectief**, en selecteert het perspectief dat u wilt gebruiken.
* Als u wilt inschakelen voor externe foutopsporing in GlassFish, gebruik geen externe van de Configuratiefunctie voor foutopsporing van de Toolkit Azure voor Eclips. In plaats daarvan GlassFish handmatig configureren. Door de manier waarop die glassfish Java opties voor vooraf gedefinieerde in de omgevingsvariabelen worden behandeld, werkt de toolkit van externe configuratie van de functie voor foutopsporing niet goed met GlassFish. Als de toolkit van externe configuratie van de functie voor foutopsporing is ingeschakeld, kan het GlassFish dat wordt gestart.

## <a name="see-also"></a>Zie ook ##

[Azure Toolkit voor Eclips][]

[Een toepassing Hallo wereld maakt voor Azure in Eclips][]

[Installatie van de Toolkit Azure voor Eclips][] 

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkID=699529
[Een toepassing Hallo wereld maakt voor Azure in Eclips]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installatie van de Toolkit Azure voor Eclips]: http://go.microsoft.com/fwlink/?LinkId=699546
[Met behulp van de Service Azure Runtime Library in JSP]: http://go.microsoft.com/fwlink/?LinkID=699551

<!-- IMG List -->

[ic719504]: ./media/azure-toolkit-for-eclipse-debugging-azure-applications/ic719504.png
[ic551537]: ./media/azure-toolkit-for-eclipse-debugging-azure-applications/ic551537.png
