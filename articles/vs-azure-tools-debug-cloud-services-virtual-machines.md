<properties 
    pageTitle="Opsporen van fouten in een service Azure cloud of een virtuele machine in Visual Studio | Microsoft Azure"
    description="Opsporen van fouten in een Cloud-Service of een virtuele Machine in Visual Studio"
    services="visual-studio-online"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />
<tags 
    ms.service="visual-studio-online"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/15/2016"
    ms.author="tarcher" />

# <a name="debugging-an-azure-cloud-service-or-virtual-machine-in-visual-studio"></a>Opsporen van fouten in een service Azure cloud of een virtuele machine in Visual Studio

Visual Studio biedt u verschillende opties voor foutopsporing in Azure, cloud services en virtuele machines.



## <a name="debug-your-cloud-service-on-your-local-computer"></a>Fouten opsporen in uw cloud-service op de lokale computer

Bespaart u tijd en geld met behulp van de Azure berekenen emulator voor foutopsporing van de cloud-service op een lokale computer. Door foutopsporing service lokaal voordat u het implementeert, kunt u zonder te betalen voor de tijd van de compute betrouwbaarheid en prestaties verbeteren. Enkele fouten optreden, alleen bij het uitvoeren van een cloud-service in Azure zelf. Als u foutopsporing op afstand inschakelen wanneer u de service publiceert en vervolgens de debugger aan een instantie van de rol koppelt, kunt u deze fouten opsporen.

De emulator simuleert de service Azure berekenen en wordt uitgevoerd in uw lokale omgeving, zodat u kunt testen en fouten opsporen in uw cloud-service voordat u het implementeert. De emulator handelt de levenscyclus van de exemplaren van uw rol en biedt toegang tot gesimuleerde bronnen, zoals lokale opslag. Wanneer u foutopsporing of uw service vanuit Visual Studio uitvoeren, de emulator wordt automatisch gestart als een toepassing van de achtergrond en vervolgens implementeert u uw service naar de emulator. U kunt de emulator uw service weergeven wanneer deze wordt uitgevoerd in de lokale omgeving. U kunt de volledige versie of de express-versie van de emulator uitvoeren. (Vanaf Azure 2.3 is de express-versie van de emulator is de standaardinstelling.) Zie [met behulp van Emulator Express uit te voeren en fouten opsporen in een Cloud-Service lokaal](https://msdn.microsoft.com/library/dn339018.aspx).

### <a name="to-debug-your-cloud-service-on-your-local-computer"></a>Voor foutopsporing van de cloud-service op de lokale computer

1. Kies in het menu **Debug**, **Foutopsporing Start** uw project Azure cloud-service wordt uitgevoerd. Als alternatief, drukt u op F5. Hier ziet u een bericht dat de Emulator berekenen wordt gestart. Wanneer de emulator wordt gestart, wordt het pictogram op de taakbalk bevestigt het.

    ![Azure-emulator in het systeemvak](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC783828.png)

1. De gebruikersinterface voor de emulator berekenen via het snelmenu voor het Azure-pictogram in het systeemvak weergeven en selecteer vervolgens **Berekenen Emulator gebruikersinterface weergeven**.

    Het linkerdeelvenster van de gebruikersinterface ziet u de services die momenteel worden geïmplementeerd op de compute-emulator en de rol van instanties die elke service wordt uitgevoerd. U kunt de service of rollen lifecycle, logboekregistratie en diagnostische gegevens weergeven in het rechterdeelvenster. Als u de focus in de bovenmarge van een venster opgenomen, het wordt vergroot en vult u het rechterdeelvenster.

1. Stap voor stap de toepassing door onderbrekingspunten instellen in uw code te selecteren in het menu **Foutopsporing** . Tijdens het doorlopen van de toepassing in de debugger het bijgewerkt met de huidige status van de toepassing. Als u stopt met het opsporen van fouten, wordt de installatie van toepassingen verwijderd. Als uw toepassing een web-functie bevat en u de opstarteigenschap actie start de webbrowser hebt ingesteld, wordt uw webtoepassing in Visual Studio gestart in de browser. Als u het aantal exemplaren van een rol in de configuratie van de service wijzigt, moet u uw cloud-service stoppen en opnieuw starten zodat u kunt deze nieuwe exemplaren van de functie Foutopsporing foutopsporing.

    **Opmerking:** Als u uitvoeren of het opsporen van fouten in de service stopt, worden niet de lokale compute-emulator en opslag emulator gestopt. U moet stoppen ze expliciet uit het systeemvak.


## <a name="debug-a-cloud-service-in-azure"></a>Fouten opsporen in een cloud-service in Azure

Voor foutopsporing op een cloud-service van een externe computer, moet u inschakelen dat de functionaliteit expliciet wanneer u uw cloud-service implementeert, zodat de vereiste services (bijvoorbeeld msvsmon.exe) zijn geïnstalleerd op de virtuele machines die uw rol exemplaren worden uitgevoerd. Als u geen foutopsporing op afstand inschakelen wanneer u de service hebt gepubliceerd, hebt u de service te publiceren met externe foutopsporing is ingeschakeld.

Als u foutopsporing op afstand voor een cloud-service inschakelt, wordt niet het slechtere prestaties levert of extra kosten wordt geconfronteerd. Niet moet u foutopsporing op afstand op een productieservice, omdat clients die gebruikmaken van de service kunnen worden aangetast.

>[AZURE.NOTE] Als u een cloud-service vanuit Visual Studio publiceert, kunt u **IntelliTrace** voor alle rollen die gericht zijn op het .NET Framework 4 of het .NET Framework 4.5 in die service. Met behulp van **IntelliTrace**, kunt u de gebeurtenissen die hebben plaatsgevonden in het geval van een rol in het verleden te onderzoeken en reproduceren van de context van die tijd. Zie [Foutopsporing in een gepubliceerde cloud service met IntelliTrace en Visual Studio](http://go.microsoft.com/fwlink/?LinkID=623016) en [Met behulp van IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx).

### <a name="to-enable-remote-debugging-for-a-cloud-service"></a>Foutopsporing op afstand voor een cloud-service inschakelen

1. Open het snelmenu voor het project Azure en selecteer vervolgens **publiceren**.

1. Selecteer de **Staging** -omgeving en de **Debug** -configuratie.

    Dit is slechts een richtlijn. U kunt ervoor kiezen om uw testomgeving uitvoeren in een productieomgeving. U mag gebruikers echter nadelig beïnvloeden als foutopsporing op afstand op de productie-omgeving. U kunt de configuratie van de Release, maar de Debug-configuratie maakt foutopsporing gemakkelijker.

    ![Kies de Debug-configuratie](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746717.gif)

1. De gebruikelijke stappen uit, maar schakel het selectievakje **Externe foutopsporing inschakelen voor alle rollen** op het tabblad **Geavanceerde instellingen** .

    ![Configuratie van Debug](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746718.gif)

### <a name="to-attach-the-debugger-to-a-cloud-service-in-azure"></a>De debugger koppelen aan een cloud service in Azure

1. Vouw het knooppunt voor uw service cloud in Server Explorer.

1. Open het snelmenu voor de functie of rol-exemplaar dat u wilt koppelen en selecteer **Debugger koppelen**.

    Bij het debuggen van een rol, wordt de Visual Studio debugger koppelt aan elk exemplaar van die rol. De foutopsporing wordt onderbroken bij een onderbrekingspunt voor het eerste exemplaar voor de rol die wordt uitgevoerd die regel en voldoet aan alle voorwaarden van deze onderbrekingspunt. Als u een exemplaar, de debugger wordt alleen dat exemplaar en-einden op een onderbrekingspunt wanneer dat specifieke exemplaar wordt uitgevoerd die regel en voldoet aan de voorwaarden van het onderbrekingspunt voor foutopsporing.

    ![Debugger koppelen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746719.gif)

1. Nadat de debugger aan een instantie koppelt, zoals gebruikelijk foutopsporing. De debugger worden automatisch gekoppeld aan de bijbehorende host-proces voor uw rol. Afhankelijk van wat de rol is, wordt de debugger koppelt aan w3wp.exe, WaWorkerHost.exe of WaIISHost.exe. Als u wilt controleren of het proces waarmee de debugger wordt gekoppeld, vouw exemplaar in Server Explorer. Zie [Azure rol Architecture](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) voor meer informatie over Azure processen.

    ![Selecteer in het dialoogvenster code-type](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Open het dialoogvenster processen door op de menubalk, foutopsporing, Windows, processen te kiezen ter identificatie van de processen waarmee de debugger wordt gekoppeld. (Toetsenbord: Ctrl + Alt + Z) Als u wilt loskoppelen van een bepaald proces, opent u het snelmenu en selecteer **Proces loskoppelen**. Of, Ga naar het exemplaar van knooppunt in Server Explorer, zoek het proces opent het snelmenu en selecteer **Proces loskoppelen**.

    ![Processen voor foutopsporing](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC690787.gif)

>[AZURE.WARNING] Vermijd lange stilstand op onderbrekingspunten wanneer externe foutopsporing. Azure behandelt een proces dat langer dan een paar minuten reageert gestopt en stopt met het verzenden van verkeer naar dat exemplaar. Als u voor te lang msvsmon.exe losgekoppeld van het proces te stoppen.

Als u wilt loskoppelen van het foutopsporingsprogramma van alle processen in uw exemplaar of rol, opent u het snelmenu voor de rol of het exemplaar dat u fouten opspoort en selecteer **Debugger loskoppelen**.

## <a name="limitations-of-remote-debugging-in-azure"></a>Beperkingen voor externe foutopsporing in Azure

Van Azure SDK 2.3 heeft foutopsporing op afstand de volgende beperkingen.

- Wanneer externe foutopsporing is ingeschakeld, kunt u een cloud-service die een rol meer dan 25 exemplaren heeft niet publiceren.

- Poorten 30400-30424, 31400-31424 en 32400-32424 maakt gebruik van de debugger. Als u een van deze poorten gebruiken probeert, niet mogelijk voor het publiceren van uw service en een van de volgende foutberichten wordt weergegeven in het logboek voor faxactiviteit voor Azure: 

    - Fout bij het valideren van het bestand .cscfg van het bestand .csdef. 
    De gereserveerde poort bereik 'range' voor eindpunt Microsoft.WindowsAzure.Plugins.RemoteDebugger.Connector van rol 'rol' overlap met een reeds gedefinieerde poort of een bereik.
    - Toewijzing is mislukt. Probeer het later opnieuw, Verminder de VM-grootte of het aantal exemplaren van de rol of probeer het implementeren van een andere regio.


## <a name="debugging-azure-virtual-machines"></a>Foutopsporing in Azure virtuele machines

U kunt fouten opsporen in programma's die in Azure virtuele machines door in Visual Studio Server Explorer. Wanneer u foutopsporing op afstand op een Azure virtuele machine, Azure de externe foutopsporing extensie geïnstalleerd op de virtuele machine. Vervolgens kunt u koppelen aan processen die op de virtuele machine en foutopsporing zoals u gewend bent.

>[AZURE.NOTE] Virtuele machines die zijn gemaakt via de stack Azure resource manager kunnen op afstand fouten opgespoord met behulp van Cloud Explorer in Visual Studio 2015. Zie voor meer informatie [Azure bronnen beheren met Cloud Explorer](http://go.microsoft.com/fwlink/?LinkId=623031).

### <a name="to-debug-an-azure-virtual-machine"></a>Voor foutopsporing van een Azure virtuele machine

1. Vouw het knooppunt virtuele Machines in Server Explorer en selecteer het knooppunt van de virtuele machine die u fouten wilt opsporen.

1. Open het contextmenu en selecteer **Foutopsporing inschakelen**. Selecteer **Ja**als u zeker weet of u wilt u foutopsporing inschakelt op de virtuele machine wordt gevraagd.

    Azure installeert de uitbreiding voor externe foutopsporing op de virtuele machine-foutopsporing inschakelen.

    ![Virtuele machine opdracht Foutopsporing inschakelen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Azure activiteitenlogboek](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)

1. Nadat de externe foutopsporing extensie de installatie heeft voltooid, open het contextmenu van de virtuele machine en selecteert u **Foutopsporing koppelen**

    Azure haalt een lijst met processen op de virtuele machine en geeft ze in de bijlage aan het proces van het dialoogvenster.

    ![De foutopsporingsopdracht koppelen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)

1. Selecteer **selecteren** om te beperken van de lijst met resultaten wilt weergeven van de soorten code die u fouten wilt opsporen in het dialoogvenster **koppelen aan een proces** . U kunt 32 - of 64-bits beheerde code of native code opsporen.

    ![Selecteer in het dialoogvenster code-type](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Selecteer de processen die u wilt debuggen op de virtuele machine en selecteer **toevoegen**. U kunt bijvoorbeeld het proces w3wp.exe kiezen als u wilt een web app op de virtuele machine debug. Zie [fouten opsporen in een of meer processen in Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) en [Azure rol-architectuur](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) voor meer informatie.

## <a name="create-a-web-project-and-a-virtual-machine-for-debugging"></a>Een webproject en een virtuele machine voor het opsporen van fouten maken

Voordat u uw Azure project publiceert, vindt u het misschien handig om deze te testen in een ingesloten omgeving met ondersteuning voor foutopsporing en het testen van scenario's en waar u het testen en controleren of programma's kunt installeren. Eén manier om dit te doen is voor foutopsporing op afstand van uw app op een virtuele machine.

Visual Studio, ASP.NET-projecten bieden een optie voor het maken van een handige virtuele machine die u voor het testen van app gebruiken kunt. De virtuele machine bevat meestal nodig eindpunten als PowerShell, extern bureaublad en WebDeploy.

### <a name="to-create-a-web-project-and-a-virtual-machine-for-debugging"></a>Voor het maken van een webproject en een virtuele machine voor het opsporen van fouten

1. Maak een nieuwe ASP.NET-webtoepassing in Visual Studio.

1. Kies in het dialoogvenster Nieuw Project voor ASP.NET in de sectie Azure **virtuele Machine** in de vervolgkeuzelijst. Laat het selectievakje **externe bronnen maken** is geselecteerd. Klik op **OK** om verder te gaan.

    De **virtuele machine maken op Azure** -dialoogvenster wordt weergegeven.


    ![ASP.NET web project het dialoogvenster maken](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746723.png)

    **Opmerking:** Wordt u gevraagd aan te melden bij uw Azure-account als u nog niet bent aangemeld.

1. Selecteer de verschillende instellingen voor de virtuele machine en klik vervolgens op **OK**. Zie [virtuele Machines]( http://go.microsoft.com/fwlink/?LinkId=623033) voor meer informatie.

    De naam die u voor de DNS-naam invoert is de naam van de virtuele machine. 

    ![Virtuele machine maken in het dialoogvenster Azure](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746724.png)

    Azure maakt u de virtuele machine en bepalingen en configureert u de eindpunten, zoals Extern bureaublad en implementeren van Web



1. Nadat u de virtuele machine is geconfigureerd, selecteert u het knooppunt van de virtuele machine in Server Explorer.

1. Open het contextmenu en selecteer **Foutopsporing inschakelen**. Selecteer **Ja**als u zeker weet of u wilt u foutopsporing inschakelt op de virtuele machine wordt gevraagd. 

    Azure installeert de uitbreiding voor externe foutopsporing in de virtuele machine-foutopsporing inschakelen.

    ![Virtuele machine opdracht Foutopsporing inschakelen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Azure activiteitenlogboek](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)

1. Publiceer uw project, zoals wordt beschreven in [hoe: implementatie van een Web Project met één klik publiceren in Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx). Selecteer **Foutopsporing** als de configuratie omdat u fouten wilt opsporen in de virtuele machine, op de pagina **Instellingen** van de wizard **Web publiceren** . Dit zorgt ervoor dat code symbolen zijn beschikbaar bij het opsporen van fouten.

    ![Publicatie-instellingen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718349.png)

1. De **Opties voor het publiceren**, selecteer **verwijderen van extra bestanden op bestemming** als het project is al geïmplementeerd op een eerder moment.

1. Nadat het project in het contextmenu van de virtuele machine in Server Explorer publiceert, selecteert u **Foutopsporing koppelen**

    Azure haalt een lijst met processen op de virtuele machine en geeft ze in de bijlage aan het proces van het dialoogvenster.

    ![De foutopsporingsopdracht koppelen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)

1. Selecteer **selecteren** om te beperken van de lijst met resultaten wilt weergeven van de soorten code die u fouten wilt opsporen in het dialoogvenster **koppelen aan een proces** . U kunt 32 - of 64-bits beheerde code of native code opsporen.

    ![Selecteer in het dialoogvenster code-type](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Selecteer de processen die u wilt debuggen op de virtuele machine en selecteer **toevoegen**. U kunt bijvoorbeeld het proces w3wp.exe kiezen als u wilt een web app op de virtuele machine debug. Zie [fouten opsporen in een of meer processen in Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Gebruik **Intellitrace** voor het verzamelen van een logboek van gesprekken en gebeurtenissen van een release-server. Zie [Foutopsporing in een gepubliceerde Cloud Service met IntelliTrace en Visual Studio](http://go.microsoft.com/fwlink/?LinkID=623016).
- **Diagnostische gegevens van Azure** gedetailleerde informatie van de code die wordt uitgevoerd binnen de rollen, aanmelden of de rollen worden uitgevoerd in de ontwikkelomgeving of in Azure gebruiken. Zie [verzamelen gegevens met behulp van Azure diagnostische gegevens vastleggen](http://go.microsoft.com/fwlink/p/?LinkId=400450).
