<properties
    pageTitle="Maak een Hello World Cloud Service voor Azure in Eclips"
    description="Informatie over het maken van een eenvoudige Hello World-toepassing met behulp van de Toolkit Azure voor Eclips."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690944.aspx -->

# <a name="create-a-hello-world-cloud-service-for-azure-in-eclipse"></a>Maak een Hello World Cloud Service voor Azure in Eclips #

De volgende stappen uit hoe u maken en implementeren van een toepassing basic JSP Azure met de Toolkit Azure voor Eclips. Ziet u een voorbeeld van JSP voor eenvoud, maar zeer vergelijkbare stappen voor een Java servlet, dienstig zou zijn wat betreft Azure-implementatie is.

De toepassing ziet er ongeveer als volgt:

![][ic600360]

## <a name="prerequisites"></a>Vereisten ##

* Een Java Developer Kit (JDK), v 1.7 of hoger.
* Voor ontwikkelaars van Java EE, IDE-Eclips Indigo of hoger. Dit kan worden gedownload van <http://www.eclipse.org/downloads/>.
* Een verdeling van een Java-webserver of een toepassing zoals Apache Tomcat, GlassFish, JBoss-toepassingsserver, Jetty of IBM® WebSphere® Application Server Liberty Core.
* Een Azure-abonnement, die kan worden verkregen uit <http://azure.microsoft.com/pricing/purchase-options/>.
* De Azure Toolkit voor Eclips. Zie [de Azure Toolkit voor Eclips installeren][]voor meer informatie.

## <a name="to-create-a-hello-world-application"></a>Voor het maken van een toepassing Hallo wereld ##

Eerst, we beginnen uit met het maken van een Java-project.

*  Eclips, start en klik op het menu **bestand**, klikt u op **Nieuw**en klik vervolgens op **Dynamische Web Project**. (Als er geen **Dynamische Web Project** weergegeven als een project beschikbaar na het klikken op **bestand**, **Nieuw**, vervolgens als volgt: klik op **bestand**, klikt u op **Nieuw**klikt u op **Project...**, vouw **Web**, klikt u op **Dynamic Web Project**en klik op **volgende**.)
*  Geef het project de **MyHelloWorld**voor de doeleinden van deze zelfstudie. (Zorg ervoor u deze naam gebruiken, de opeenvolgende stappen in deze zelfstudie verwacht het WAR-bestand de naam MyHelloWorld). Het scherm weergegeven met de volgende strekking: ![][ic589576]
* Klik op **Voltooien**.
* Vouw in de weergave Project Explorer Eclips van **MyHelloWorld**. **Webinhoud**met de rechtermuisknop op **Nieuw**en klik vervolgens op **JSP-bestand**.
* Naam in het dialoogvenster **Nieuwe JSP-bestand** , het bestand **index.jsp**. Houd de bovenliggende map als **MyHelloWorld/webinhoud**, zoals in het volgende:  ![][ic659262]
* Ten behoeve van deze zelfstudie **Nieuwe JSP-bestand (html)** selecteren in het dialoogvenster **Selecteer JSP-sjabloon** en klik op **Voltooien**.
* Als het bestand index.jsp wordt geopend in de Eclips, toevoegen in de tekst weer te geven dynamisch **Hello World!** binnen de bestaande `<body>` element. Uw bijgewerkte `<body>` inhoud moet worden weergegeven als de volgende:
```
    <body>
    <b><% out.println("Hello World!"); %></b>
    </body>
```
* Index.jsp opslaan.

## <a name="to-deploy-your-application-to-azure-the-quick-and-simple-way"></a>Uw toepassing Azure, de snelle en eenvoudige manier implementeren ##

Als er een Java webtoepassing gereed is om te testen, kunt u de volgende snelle methode uitproberen rechtstreeks op de Azure cloud.

1. Klik op **MyHelloWorld**in de Projectverkenner van de Eclips.
1. In de werkbalk Eclips op de vervolgkeuzeknop **publiceren** en klik op **Publiceren als Azure Cloud-Service**
    ![][publishDropdownButton]
1. Als u deze toepassing voor de eerste keer Azure publiceert en u een project Azure-implementatie voor deze toepassing voordat niet hebt gemaakt, worden een project Azure-implementatie u automatisch gemaakt. U ziet het volgende bericht, waarin ook de JDK-pakket en de toepassingsserver die automatisch wordt geïmplementeerd als uw toepassing wilt uitvoeren.
    ![][ic789598]

    Deze aanpak snelkoppeling kunt een snelle en eenvoudige manier om uw toepassing in Azure testen zonder een specifieke server of JDK die verschilt van de standaardinstellingen configureren. Als u tevreden met de standaardinstellingen bent, kunt u klikken op **OK** om door te gaan met de volgende stappen.
    Echter, als u wilt wijzigen van de JDK of toepassingsserver te gebruiken voor uw toepassing, kunt u dat later doen door het bewerken van het project Azure-implementatie die automatisch voor u is gemaakt, of kunt u nu op **Annuleren** klikken en lees het **gedeelte over Azure implementatie-projecten** van deze zelfstudie.
1. Klik in het dialoogvenster **publiceren op Azure** :
    1. Als er geen abonnementen te selecteren in de lijst met **abonnementen** nog, als volgt te werk als u wilt uw abonnementsgegevens importeren:
        1. Klik op **importeren uit bestand publicatie-instellingen**.
        1. Klik in het dialoogvenster **Gegevens importeren-abonnement** , **Publicatie-instellingen bestand downloaden**. Als u nog niet aangemeld bij uw account Azure, wordt u gevraagd aan te melden. Vervolgens wordt u gevraagd te publiceren instellingenbestand opslaan een Azure. Opslaan op uw lokale computer.
        1. **Nog steeds in het dialoogvenster **Gegevens importeren-abonnement** , klikt u op de knop **Bladeren** en selecteer het instellingenbestand publiceren dat u lokaal hebt opgeslagen in de vorige stap.** Het scherm ziet er ongeveer als volgt: ![][ic644267]
        1. Klik op **OK**.
    1. Voor **abonnement**, selecteert u het abonnement dat u gebruiken voor de implementatie wilt.
    1. Voor **rekening van de opslag**, de opslag rekening selecteren die u wilt gebruiken of klik op **Nieuw** om een nieuwe opslag-account te maken.
    1. Selecteer de cloud-service die u wilt gebruiken voor de **naam van de Service**, of klik op **Nieuw** om een nieuwe wolk service maken.
    1. Selecteer de versie van het besturingssysteem dat u wilt gebruiken voor de implementatie voor **Target OS**.
    1. Selecteer voor **doelomgeving**, ten behoeve van deze zelfstudie, **klaarzetten**. (Wanneer u klaar om te implementeren voor uw productiesite bent, wijzigt u dit naar **productie**.)
    1. Optioneel: Zorg ervoor dat **eerdere implementatie overschrijven** is ingeschakeld als u wilt dat uw nieuwe implementatie moeten worden overschreven eerdere implementatie. Als u deze optie inschakelt, wordt geen ervaring "409 conflict" problemen bij het publiceren naar dezelfde locatie.
        Opmerking het dialoogvenster **publiceren op Azure** bevat een sectie voor **Externe toegang**. Standaard externe toegang niet is ingeschakeld en we zullen het niet inschakelen voor dit voorbeeld. Als u RAS wilt inschakelen, voert u een gebruikersnaam en wachtwoord te gebruiken bij het aanmelden op afstand. Voor meer informatie over externe toegang Zie [Externe toegang inschakelen voor implementaties in Eclips Azure][].
        Het dialoogvenster **publiceren op Azure** wordt met de volgende strekking weergegeven: ![][ic719488]
1. Klik op **publiceren** om te publiceren naar de Staging-omgeving.
    Klik op **Ja**voor het uitvoeren van een volledige versie. Dit kan enkele minuten duren voordat de eerste versie.
    Een **Logboek met faxactiviteit Azure** weergegeven in de sectie Eclips browsen met tabbladen weergaven.
    ![][ic719489]
   Kunt u dit logboek als weergave van de **Console** voor een overzicht van de voortgang van uw implementatie. Een alternatief is het aanmelden bij de [Azure Management Portal][]en de sectie **Cloud-Services** gebruiken voor het controleren van de status.
1. Wanneer de installatie met succes is geïmplementeerd, wordt het **Logboek met faxactiviteit Azure** **gepubliceerde**status weergegeven. Klik op **gepubliceerd**, zoals in de volgende afbeelding en een exemplaar van de implementatie door de browser wordt geopend.
    ![][ic719490]

Omdat dit een implementatie van een testomgeving is, worden de DNS-naam van het formulier http://&lt;*guid*&gt;. cloudapp.net en de URL wordt de DNS-naam plus het achtervoegsel voor de toepassing bevatten. Bijvoorbeeld http://447564652c20426f6220526f636b7321.cloudapp.net/MyHelloWorld. (Het gedeelte van het **MyHelloWorld** is hoofdlettergevoelig.) Ook ziet u de DNS-naam als u klikt op de naam van de implementatie in de Portal Azure Platform Management (binnen het gedeelte Cloud Services de portal management).

Hoewel dit overzicht is een implementeren in de testomgeving, een productie-implementatie de dezelfde stappen volgt, behalve in het dialoogvenster **publiceren op Azure** , selecteert u de **productie** in plaats van een **Staging-** voor de **doelomgeving**. Een productie-implementatie resulteert in een URL op basis van de DNS-naam van uw keuze, in plaats van een GUID zoals gebruikt voor gefaseerde installatie.

>[AZURE.WARNING] Op dit punt hebt u uw Azure toepassing naar de cloud geïmplementeerd. Echter realiseren dat een gedistribueerde toepassing, zelfs als deze niet wordt uitgevoerd, blijft toenemen factureerbare tijd voor uw abonnement op voordat u verdergaat. Daarom is het zeer belangrijk dat u ongewenste implementaties uit uw abonnement Azure verwijderen.

## <a name="about-azure-deployment-projects"></a>Over Azure-implementatieprojecten ##

Een of meer Java-toepassingen naar Azure implementeren, is een Project Azure-implementatie vereist. Het speelt de rol van het "pakket" dat uw toepassingen worden verpakt moeten in om te worden gepubliceerd op Azure.

Naast de informatie over de toepassingen een Azure-implementatie-project bevat ook informatie over andere belangrijke onderdelen van uw distributie belangrijker is: de container application server uit te voeren in uw web app en de Java runtime op uitvoeren. Azure ondersteunt runtimes voor Java en Java-toepassingsservers die kunt u kiezen uit een grote selectie.

Hoewel het voorbeeld hier gebruikt voor educatieve doeleinden sterk vereenvoudigd, kan een project Azure-implementatie ook andere belangrijke configuratie-informatie die u kunt bijna willekeurige complexe, schaalbare, hoge beschikbaarheid, meerlaags cloud services maken met uw toepassingen bevatten. U kunt de **sessie affiniteit ("sticky sessies')**, **Snelle caching** **Foutopsporing op afstand**, **SSL-offloading**, **firewallpoort routering**, **externe toegang**en een aantal andere krachtige mogelijkheden inschakelen.

Als u de vorige sectie van deze zelfstudie ("aan uw toepassingen distribueren naar Azure, de snelle en eenvoudige manier") hebt uitgevoerd, kunt u ziet nu een nieuw project Azure-implementatie in de Project Explorer automatisch gegenereerd voor u en met de naam '**MyHelloWorld_onAzure**'.

U kan ook beginnen in deze zelfstudie op een lege Azure-implementatie-project zelf maken en toe te voegen de toepassingen aan. Het is een langere proces, maar geeft u meer controle over de eerste configuratie vanaf het begin.

Om een nieuw Azure-implementatieproject helemaal zelf maken, klikt u op **Nieuw Azure implementatieproject** ![][ic710876].

Ongeacht of u werkt met een reeds bestaand project voor Azure-implementatie, of maakt een geheel nieuw, u in staat bent te wijzigen van de configuratie-instellingen en -onderdelen, zoals de JDK of de toepassingsserver even gemakkelijk op elk gewenst moment.

Wijzigen van de JDK, of de application server of de lijst in een bestaand project met Azure-implementatie:

1. Vouw het knooppunt uit project (bv. **MyHelloWorld_onAzure**) in de Projectverkenner
2. Klik met de rechtermuisknop op **WorkerRole1**
3. Vouw het submenu **Azure** in het contextmenu
4. Klik op de **configuratie van de Server**

Ongeacht of u bent gestart serverconfiguratiestappen door een bestaand project met Azure-implementatie te bewerken zoals hierboven, of maakt een nieuw bestand maken, worden er dialoogvensters zodat u kunt uw JDK, server-en configureren van hetzelfde type. Meer informatie over het wijzigen van de instellingen in deze dialoogvensters, bijvoorbeeld voor het wijzigen van de JDK, de application server en toepassingen toevoegen of verwijderen in een implementatie, Zie het artikel van de [configuratie van servereigenschappen][] .

## <a name="windows-only-to-deploy-your-application-to-the-compute-emulator"></a>Alleen Windows: implementatie van de toepassing naar de compute-emulator ##

>[AZURE.NOTE] De Azure-emulator is alleen beschikbaar op Windows. Als u een besturingssysteem dan Windows gebruikt, kunt u deze sectie overslaan.

Als u de stappen eerder, dat wil zeggen impliciet een nieuw Azure-implementatie-project hebt gemaakt door het publiceren van uw toepassing met Azure, de JDK en toepassing zijn servers geconfigureerd voor de cloud, maar niet voor lokale emulatie. Ter voorbereiding van het project voor het testen in de lokale emulator, volg deze stappen:

1. Klik op **MyHelloWorld_onAzure**in de Projectverkenner van de Eclips.
1. Klik met de rechtermuisknop op de **WorkerRole1**.
1. Vouw het submenu **Azure** in het contextmenu.
1. Klik op de **configuratie van de Server**.
1. Controleer als de toolkit is een standaard vooraf geconfigureerd op het tabblad **JDK** lokale JDK voor u. Als niet, of als u wilt wijzigen van de standaardwaarden aangenomen, ervoor te zorgen dat het selectievakje **de JDK uit voor het testen van lokaal bestandspad gebruiken** is ingeschakeld en u de locatie van de JDK-installatie die u wilt gebruiken geeft. Als u deze wijzigen wilt, klikt u op de knop **Bladeren** en selecteer het besturingselement bladeren met de Active directory-locatie van de JDK te gebruiken.
1. Klik op het tabblad **Server** .
1. Voer in het tekstvak **lokaal pad** onderaan in het dialoogvenster het pad van een server lokaal zijn geïnstalleerd, die overeenkomt met het type en het primaire versienummer van de server aan de bovenkant van het dialoogvenster, bij het **implementeren van een server van dit type** checkbox is geselecteerd. Als u een ander type of primaire versie van de toepassingsserver gebruiken wilt, eerst de selectie onder die checkbox wijzigen.
1. Klik op **OK**.
1. In de werkbalk Eclips, klikt u op **uitvoeren in de Emulator Azure** , ![][ic710879]. Als de knop **uitvoeren in Azure-Emulator** is niet ingeschakeld, Controleer of **MyHelloWorld_onAzure** is geselecteerd in de Projectverkenner van de Eclips en ervoor te zorgen dat de Projectverkenner Eclips van focus als het huidige venster. Dit wordt eerst een volledige versie van het project te starten en start vervolgens uw webtoepassing van Java in de compute-emulator. (Opmerking dat afhankelijk van de kenmerken van de prestaties van uw computer, de eerste build enkele seconden tot enkele minuten duurt, maar latere bouwt krijgt sneller.) Nadat de eerste build stap is voltooid, wordt u door Windows Gebruikersaccountbeheer (UAC) gevraagd om deze opdracht om de computer te wijzigen. Klik op **Ja**.

>[AZURE.IMPORTANT] Als de UAC-prompt niet wordt weergegeven, controleert u de Windows-taakbalk op het pictogram UAC en klik eerst op. Soms de UAC prompt wordt niet weergegeven als bovenste venster, maar alleen als een pictogram op de taakbalk zichtbaar is.

1. Bekijk de uitvoer van de emulator compute UI om te bepalen als er problemen met uw project zijn. Afhankelijk van de inhoud van uw implementatie duurt een paar minuten voor de toepassing moet volledig worden gestart binnen de compute-emulator.
1. Start uw browser en gebruik de URL `http://localhost:8080/MyHelloWorld` als het adres (de `MyHelloWorld` deel van de URL is hoofdlettergevoelig). Ziet u uw toepassing MyHelloWorld (de uitvoer van index.jsp), zoals in de volgende afbeelding: ![][ic589579]

Wanneer u klaar bent om te stoppen met uw toepassing wordt uitgevoerd in de emulator berekenen op de werkbalk Eclips klikt u op de knop **Beginwaarden Azure Emulator** ![][ic710880].

## <a name="to-delete-your-deployment"></a>De installatie verwijderen ##

Als u wilt verwijderen van uw implementatie binnen de Azure Toolkit voor Eclips, ervoor te zorgen dat de **MyHelloWorld_onAzure** is geselecteerd in de Projectverkenner van de Eclips, controleer dan de Projectverkenner Eclips is het huidige venster gericht en klik vervolgens op de knop **publicatie ongedaan maken** ![][ic710883], op de werkbalk van de Eclips. (U kunt dezelfde bewerking kan doen door met de rechtermuisknop op de **MyHelloWorld_onAzure** in de Projectverkenner van de Eclips **Azure** klikken en vervolgens te klikken op de **Undeploy van Azure Cloud**.) Hierdoor wordt het dialoogvenster **Ongedaan Azure Project** weergegeven.

![][ic719491]

Selecteer de service-abonnement en een wolk met de implementatie, selecteert u de installatie die u wilt verwijderen en klik op **publicatie ongedaan maken**.

(Een alternatief voor het verwijderen van de installatie met de toolkit is het gebruik van de **Cloud diensten** sectie van de Azure Management Portal: Ga naar uw implementatie, selecteert u deze en klik vervolgens op de knop **verwijderen** . Dit zal stoppen en vervolgens te verwijderen, de implementatie. Als u alleen wilt stoppen met de distributie en niet wordt verwijderd, klikt u op de knop **stoppen** in plaats van de knop **verwijderen** , maar zoals hierboven vermeld, als u de implementatie niet verwijdert, blijven factureerbare kosten toenemen voor de implementatie, zelfs als deze is gestopt).

## <a name="see-also"></a>Zie ook ##

[Azure Toolkit voor Eclips][]

[Installatie van de Toolkit Azure voor Eclips][] 

[Wat is nieuw in de Azure Toolkit voor Eclips][]

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Role Properties]: http://go.microsoft.com/fwlink/?LinkID=699525
[Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkID=699529
[Externe toegang inschakelen voor Azure implementaties in Eclips]: http://go.microsoft.com/fwlink/?LinkID=699538
[Installatie van de Toolkit Azure voor Eclips]: http://go.microsoft.com/fwlink/?LinkId=699546
[Configuratie-servereigenschappen]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[Wat is nieuw in de Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic589576]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589576.png
[ic589579]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589579.png
[ic600360]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic600360.png
[ic644267]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic644267.png
[ic659262]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic659262.png
[ic710876]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710876.png
[ic710879]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710879.png
[ic710880]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710880.png
[ic710882]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710882.png
[ic710883]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710883.png
[ic719488]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719488.png
[ic719489]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719489.png
[ic719490]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719490.png
[ic719491]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719491.png
[ic789598]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic789598.png
[publishDropdownButton]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/publishDropdownButton.png
