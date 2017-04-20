<properties
   pageTitle="Diagnostische gegevens configureren voor Azure Cloud Services en virtuele Machines | Microsoft Azure"
   description="Beschrijving van het configureren van diagnostische informatie voor foutopsporingsdoeleinden cloude Azure services en virtuele machines (VMs) in Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configuring-diagnostics-for-azure-cloud-services-and-virtual-machines"></a>Diagnostische gegevens voor Azure Cloud Services en virtuele Machines configureren

Wanneer u problemen met een Azure cloud service of Azure virtuele machine, kunt u diagnostische Azure gemakkelijker met behulp van Visual Studio. Azure diagnostische gegevens vastleggen systeem en logboekregistratie van gegevens op de virtuele machines en de virtuele machine exemplaren die uw cloud-service wordt uitgevoerd en die gegevens overgebracht naar een account voor opslag van uw keuze. Zie [Diagnostische gegevens vastleggen voor web apps in Azure App-Service inschakelen](./app-service-web/web-sites-enable-diagnostic-log.md) voor meer informatie over diagnostische gegevens vastleggen in Azure.

In dit onderwerp wordt beschreven hoe u inschakelen en configureren van Azure diagnostics in Visual Studio, zowel vóór als na de implementatie, maar ook in Azure virtuele machines. Dit ziet u ook het selecteren van de diagnostische gegevens te verzamelen en hoe u de informatie bekijken nadat deze zijn verzameld.

Azure diagnostische gegevens kunt u op de volgende manieren configureren:

- U kunt diagnostische gegevens van het configuratie-instellingen in het dialoogvenster **Configuratie van diagnostische** in Visual Studio. De instellingen worden opgeslagen in een bestand met de naam diagnostics.wadcfgx (diagnostics.wadcfg in Azure SDK 2.4 of eerder). U kunt het configuratiebestand ook rechtstreeks wijzigen. Als u het bestand handmatig bijwerken, wijzigingen in de configuratie worden pas van kracht de volgende keer implementeren van de wolk naar Azure service of de service wordt uitgevoerd in de emulator.

- **Cloud Explorer** of **Server Explorer** gebruiken in Visual Studio de diagnostische instellingen wijzigen voor een virtuele machine of actieve cloud-service.

## <a name="azure-26-diagnostics-changes"></a>Azure 2.6 diagnostics wijzigingen

Azure SDK 2.6 projecten in Visual Studio, zijn de volgende wijzigingen aangebracht. (Deze wijzigingen ook van toepassing op nieuwere versies van Azure SDK.)

- De lokale emulator biedt nu ondersteuning voor diagnostische gegevens. Dit betekent dat u kunt diagnostische gegevens te verzamelen en dat uw toepassing de juiste traces maakt tijdens het ontwikkelen en testen in Visual Studio. De verbindingsreeks `UseDevelopmentStorage=true` kunt u het verzamelen van diagnostische gegevens terwijl u uw cloud service-project in Visual Studio werkt met behulp van de emulator Azure opslag. Alle diagnostische gegevens worden verzameld in de opslag (opslag ontwikkeling).

- Diagnostische gegevens opslag account verbindingsreeks (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) opnieuw opgeslagen in het configuratiebestand van service (.cscfg). In Azure SDK 2.5 is de diagnostische gegevens opslag rekening opgegeven in het bestand diagnostics.wadcfgx.

Er zijn enkele belangrijke verschillen tussen hoe de verbindingsreeks in Azure SDK 2.4 en eerder heeft gewerkt en hoe het werkt in Azure SDK 2.6 en hoger.

- In Azure SDK 2.4 en eerder is de verbindingsreeks gebruikt als een runtime door de invoegtoepassing diagnostische gegevens om de gegevens van de opslag voor het overbrengen van diagnostische logboeken.

- In Azure SDK 2.6 en hoger wordt de verbindingsreeks van de diagnostische gegevens van de uitbreiding van diagnostische gegevens configureren met de gegevens van de juiste opslag tijdens publicatie door Visual Studio gebruikt. De verbindingsreeks, kunt u verschillende opslag instellen voor verschillende configuraties die Visual Studio wordt gebruikt bij het publiceren. Omdat de invoegtoepassing diagnostics (na Azure SDK 2.5) niet langer beschikbaar is, inschakelen niet het bestand .cscfg op zichzelf echter de extensie Diagnostics. U moet de extensie via hulpprogramma's zoals Visual Studio of PowerShell afzonderlijk inschakelen.

- De uitvoer van het pakket van Visual Studio bevat ook de openbare configuratie XML voor de uitbreiding van de diagnostische gegevens voor elke rol ter vereenvoudiging van het proces van de uitbreiding van diagnostische gegevens configureren met PowerShell. Visual Studio gebruikt de verbindingsreeks diagnostische gegevens aanwezig zijn in de configuratie van openbare gegevens van de opslag te vullen. De openbare configuratiebestanden worden gemaakt in de map extensies en PaaSDiagnostics van het patroon volgen. &lt;Rolnaam >. PubConfig.xml. Alle implementaties op basis van PowerShell kunnen dit patroon gebruiken om elke configuratie toewijzen aan een rol.

- De verbindingsreeks in het bestand .cscfg wordt ook gebruikt door de [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) toegang krijgen tot de diagnostische gegevens, zodat het kan worden weergegeven op het tabblad **controle** . De verbindingsreeks is nodig voor het configureren van de service kunt u uitgebreide controle gegevens weergeven in de portal.

## <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Migratie projecten op Azure SDK 2.6 en hoger

Bij het migreren van Azure SDK 2.5 naar Azure SDK 2.6 of hoger, als er een diagnostische gegevens opslag account die is opgegeven in het bestand .wadcfgx, vervolgens blijft er. Als u wilt profiteren van de flexibiliteit van opslag van verschillende rekeningen gebruiken voor van verschillende opslagconfiguraties, moet u handmatig de verbindingstekenreeks aan uw project toevoegen. Als u een project van Azure SDK 2.4 en eerder naar Azure SDK 2.6 migreert bent, worden tekenreeksen voor de diagnostische gegevens behouden. Echter, Let op de wijzigingen in hoe tekenreeksen worden behandeld in Azure SDK 2.6 zoals opgegeven in de vorige sectie.

### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Hoe Visual Studio bepaalt de diagnostische gegevens opslag account

- Als u een verbindingsreeks diagnostics is opgegeven in het bestand .cscfg, Visual Studio wordt gebruikt voor het configureren van de uitbreiding van de diagnostische gegevens publiceren als bij het genereren van de openbare XML-configuratiebestanden tijdens de verpakking.

- Als u geen verbindingsreeks diagnostics is opgegeven in het bestand .cscfg, vervolgens valt Visual Studio terug op de uitbreiding van de diagnostische gegevens publiceren en de openbare XML-configuratiebestanden worden gegenereerd wanneer de verpakking te configureren met behulp van de opslag-account die is opgegeven in het bestand .wadcfgx.

- De verbindingsreeks diagnostische gegevens in het bestand .cscfg heeft voorrang op de rekening van de opslag in het bestand .wadcfgx. Als een verbindingsreeks diagnostics is opgegeven in het bestand .cscfg, Visual Studio die wordt gebruikt en negeert de rekening opslag in .wadcfgx.

### <a name="what-does-the-update-development-storage-connection-strings-checkbox-do"></a>De betekenis van de "Update ontwikkeling opslag verbindingsreeksen..." CheckBox doen?

Het selectievakje voor de **Update ontwikkeling opslag verbindingsreeksen voor diagnostiek en Caching met Microsoft Azure opslag referenties wanneer u publiceert naar Microsoft Azure** biedt u een handige manier om eventuele verbindingstekenreeksen ontwikkeling opslag account bijwerken met de account Azure opslag tijdens publicatie.

Stel bijvoorbeeld dat u dit selectievakje inschakelt en de verbindingsreeks diagnostische gegevens `UseDevelopmentStorage=true`. Wanneer u het project naar Azure publiceren, worden Visual Studio de verbindingsreeks diagnostische gegevens automatisch bijgewerkt met de opslag-account die u hebt opgegeven in de wizard publiceren. Echter als een echte opslag-account als de diagnostische gegevens verbindingsreeks is opgegeven, wordt dat account gebruikt in plaats daarvan.

## <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Diagnostische functies verschillen tussen Azure SDK 2.4 en eerder en Azure SDK 2.5 en hoger

Als u uw project vanuit Azure SDK 2.4 Azure SDK 2.5 of hoger overschakelt, moet u voorzien in het achterhoofd de volgende diagnostische functies verschillen.

- **Configuratie-API's zijn afgeschaft** – programma configuratie van diagnostische gegevens beschikbaar in Azure SDK 2.4 of eerdere versies, maar is afgeschaft in Azure SDK 2.5 en hoger. Als u de configuratie van de diagnostische gegevens die momenteel in code is gedefinieerd, moet u het opnieuw configureren van deze instellingen geheel in het gemigreerde project om diagnostische gegevens blijven werken. Het configuratiebestand van de diagnostische gegevens voor Azure SDK 2.4 is diagnostics.wadcfg en diagnostics.wadcfgx voor Azure SDK 2.5 en hoger.

- **Diagnostische toepassingen voor cloud-service kan alleen worden geconfigureerd op het niveau van de rol niet op het niveau van het exemplaar.**

- **Telkens wanneer u uw app, implementeert de diagnostische configuratie wordt bijgewerkt** : dit kan pariteit problemen veroorzaken als u de configuratie van de diagnostische gegevens van Server Explorer wijzigt en vervolgens implementeren van uw app.

- **Azure SDK 2.5 en hoger, loopt vast dumpen in het configuratiebestand van de diagnostische gegevens niet in de code zijn geconfigureerd** : als u crashdumps geconfigureerd in de code hebt, hebt u de configuratie van code handmatig overbrengen naar het configuratiebestand, omdat de crashdumps worden niet tijdens de migratie naar Azure SDK 2.6 overgebracht.

## <a name="enable-diagnostics-in-cloud-service-projects-before-deploying-them"></a>Diagnostische gegevens in de cloud serviceprojecten inschakelen voordat u ze implementeert

In Visual Studio kunt u voor het verzamelen van diagnostische gegevens voor de rollen die worden uitgevoerd in Azure, wanneer de service in de emulator wordt uitgevoerd voordat u deze implementeert. Alle wijzigingen in de instellingen van de diagnostische gegevens in Visual Studio zijn opgeslagen in het configuratiebestand diagnostics.wadcfgx. Deze configuratie-instellingen opgeven voor de opslag account waar diagnostische gegevens wordt opgeslagen wanneer u uw cloud-service implementeert.

### <a name="to-enable-diagnostics-in-visual-studio-before-deployment"></a>Diagnostiek in Visual Studio vóór implementatie inschakelen

1. Kies **Eigenschappen**in het snelmenu voor de rol die u interesseert, en kies het tabblad **configuratie** in het venster **Eigenschappen** van de rol.

1. Zorg dat het selectievakje **Diagnostics inschakelen** is geselecteerd in de sectie **Diagnostische gegevens** .

    ![Toegang tot de optie diagnose inschakelen](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796660.png)

1. Kies de knop met het weglatingsteken (...) op te geven van de opslag account waar u de diagnostische gegevens moeten worden opgeslagen. De opslag-account die u kiest is de locatie waar de diagnostische gegevens wordt opgeslagen.

    ![Geef de rekening op opslag gebruiken](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796661.png)

1. Opgeven of u wilt verbinden met de Azure opslag Emulator, een Azure-abonnement of handmatig referenties hebt ingevoerd in het dialoogvenster **Verbindingsreeks opslag maken** .

    ![Opslag van het dialoogvenster account](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796662.png)

  - Als u de Microsoft Azure opslag Emulator optie, de verbindingsreeks is ingesteld op UseDevelopmentStorage = true.

  - Als u ervoor kiest de uw abonnement kunt u de Azure abonnement dat u wilt gebruiken en de accountnaam. U kunt de knop Accounts beheren voor het beheren van uw abonnementen op Azure.

  - Als u de optie handmatig ingevoerde referenties, wordt u gevraagd om de naam en de sleutel van de Azure account die u wilt gebruiken.

1. Klik op de knop **configureren** om het dialoogvenster **configuratie van diagnostische** . Elk tabblad (met uitzondering van de **algemene** en **Logboek mappen**) vertegenwoordigt een diagnostische gegevensbron die u kunt verzamelen. Het standaardtabblad **Algemeen**biedt de volgende diagnostische gegevens verzameling opties: **alleen fouten**, **alle gegevens**en **aangepaste plan**. De standaardoptie, **alleen fouten**, heeft de minste hoeveelheid opslagruimte omdat deze waarschuwingen of berichten voor tracering niet overgebracht. De optie alle informatie brengt de meeste informatie en is daarom de duurste optie voor opslag.

    ![Azure diagnostische gegevens en configuratie inschakelen](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)

1. In dit voorbeeld de optie **aangepast schema** zodat u de verzamelde gegevens kan aanpassen.

1. Het **Quota in MB schijf** geeft aan hoeveel ruimte u in uw account opslag voor diagnostische gegevens wilt toewijzen. Als u wilt, kunt u de standaardwaarde wijzigen.

1. Selecteer op elk tabblad Diagnostische gegevens u wilt verzamelen, de **inschakelen overbrengen van <log type> ** selectievakje. Als u verzamelen toepassingslogboeken wilt, selecteert u het selectievakje **overdracht van de toepassingslogboeken inschakelen** op het tabblad **Toepassingen** in. Ook andere informatie die vereist is voor elk type diagnostische gegevens opgeven. Zie de sectie **bronnen diagnostische gegevens configureren** verderop in dit onderwerp voor informatie over de configuratie op elk tabblad.

1. Nadat u de collectie van alle diagnostische gegevens die u hebt ingeschakeld, klik op de knop **OK** .

1. Zoals gebruikelijk het project Azure cloud-service uitvoeren in Visual Studio. Als u uw toepassing gebruikt, worden de logboekgegevens die u hebt ingeschakeld wordt opgeslagen in de Azure opslag account die u hebt opgegeven.

## <a name="enable-diagnostics-in-azure-virtual-machines"></a>Diagnostische gegevens in Azure virtual machines schakelen

In Visual Studio kunt u voor het verzamelen van diagnostische gegevens voor Azure virtuele machines.

### <a name="to-enable-diagnostics-in-azure-virtual-machines"></a>Diagnostische gegevens in Azure virtuele machines inschakelen

1. Kies het knooppunt Azure in **Server Explorer**en maak verbinding met uw Azure abonnement als u nog niet bent verbonden.

1. Vouw het knooppunt **virtuele Machines** . U kunt een nieuwe virtuele machine maken of dat al aanwezig is.

1. Kies in het snelmenu voor de virtuele machine die u interesseert, **configureren**. Dit geeft het dialoogvenster configuratie van virtuele machines.

    ![Een Azure virtuele machine configureren](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796663.png)

1. Als dit nog niet is geïnstalleerd, de Microsoft Monitoring Agent Diagnostics-extensie toevoegen. Deze extensie kunt u het verzamelen van diagnostische gegevens voor de Azure virtual machine. In de lijst extensies geïnstalleerd, kiest u een vervolgkeuzelijst met beschikbare uitbreiding het selecteren en kies vervolgens Microsoft Monitoring Agent diagnostische gegevens.

    ![Een uitbreiding van Azure virtual machine installeren](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766024.png)

    >[AZURE.NOTE] Andere extensies diagnostische gegevens zijn beschikbaar voor uw virtuele machines. Zie voor meer informatie, functies en uitbreidingen voor Azure VM.

1. Kies de knop **toevoegen** om de extensie toevoegen en weergeven in het dialoogvenster **configuratie van diagnostische** .

1. Kies de knop **configureren** naar een opslag account opgeven en klik vervolgens op **OK** .

    Elk tabblad (met uitzondering van de **algemene** en **Logboek mappen**) vertegenwoordigt een diagnostische gegevensbron die u kunt verzamelen.

    ![Azure diagnostische gegevens en configuratie inschakelen](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)

    Het standaardtabblad **Algemeen**biedt de volgende diagnostische gegevens verzameling opties: **alleen fouten**, **alle gegevens**en **aangepaste plan**. De standaardoptie, **alleen fouten**, heeft de minste hoeveelheid opslagruimte omdat deze waarschuwingen of berichten voor tracering niet overgebracht. De optie **alle informatie** brengt de meeste informatie en is daarom de duurste optie voor opslag.

1. In dit voorbeeld de optie **aangepast schema** zodat u de verzamelde gegevens kan aanpassen.

1. Het **Quota in MB schijf** geeft aan hoeveel ruimte u in uw account opslag voor diagnostische gegevens wilt toewijzen. Als u wilt, kunt u de standaardwaarde wijzigen.

1. Selecteer op elk tabblad Diagnostische gegevens u wilt verzamelen, de **inschakelen overbrengen van <log type> ** selectievakje.

    Als u verzamelen toepassingslogboeken wilt, selecteert u het selectievakje **overdracht van de toepassingslogboeken inschakelen** op het tabblad **Toepassingen** in. Ook andere informatie die vereist is voor elk type diagnostische gegevens opgeven. Zie de sectie **bronnen diagnostische gegevens configureren** verderop in dit onderwerp voor informatie over de configuratie op elk tabblad.

1. Nadat u de collectie van alle diagnostische gegevens die u hebt ingeschakeld, klik op de knop **OK** .

1. Sla het bijgewerkte project.

    Hier ziet u een bericht in het venster **Microsoft Azure activiteitenlogboek** bijgewerkt van de virtuele machine.

## <a name="configure-diagnostics-data-sources"></a>Gegevensbronnen van diagnostische gegevens configureren

Nadat u het verzamelen van diagnostische gegevens hebt ingeschakeld, kunt u precies welke gegevensbronnen u wilt verzamelen en welke gegevens worden verzameld. Hier volgt een lijst van de tabbladen in het dialoogvenster **configuratie van diagnostische** en betekent dat elke configuratieoptie.

### <a name="application-logs"></a>Toepassingslogboeken

**Toepassingslogboeken** bevatten diagnostische informatie geproduceerd door een webtoepassing. Als u vastleggen toepassingslogboeken wilt, selecteert u het selectievakje **overdracht van de toepassingslogboeken inschakelen** . U kunt vergroten of verkleinen van het aantal minuten wanneer de toepassingslogboeken worden overgebracht naar uw rekening opslag door de waarde **Transfer periode (min)** . U kunt ook de hoeveelheid informatie vastgelegd in het logboek door het logboek niveau waarde wijzigen. U kunt bijvoorbeeld **uitgebreid** naar meer informatie of kies **kritiek** alleen kritieke fouten vastleggen. Als u een specifieke diagnoses-provider die u toepassingslogboeken genereert, kunt u deze vastleggen door de GUID van de provider toe te voegen aan het vak van de **GUID van de Provider** .

  ![Toepassingslogboeken](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758145.png)

  Zie [Diagnostische gegevens vastleggen voor web apps in Azure App-Service inschakelen](./app-service-web/web-sites-enable-diagnostic-log.md) voor meer informatie over de toepassingslogboeken.

### <a name="windows-event-logs"></a>Windows-gebeurtenislogboeken

Als u wilt dat Windows-gebeurtenislogboeken vastleggen, selecteert u het selectievakje **van de Windows-gebeurtenislogboeken voor overdracht inschakelen** . U kunt vergroten of verkleinen van het aantal minuten wanneer de gebeurtenislogboeken worden overgebracht naar uw rekening opslag door de waarde **Transfer periode (min)** . Schakel de selectievakjes in voor de typen gebeurtenissen die u wilt bijhouden.

  ![Gebeurtenislogboeken](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796664.png)

Als u Azure SDK 2.6 of hoger gebruikt en u een aangepaste gegevensbron opgeven, voert u deze in de **<Data source name>** tekst in het vak en kies vervolgens de knop **toevoegen** naast het. De gegevensbron wordt toegevoegd aan het bestand diagnostics.cfcfg.

Als u Azure SDK 2.5 en geef een aangepaste gegevensbron wilt, kunt u het toevoegen aan de `WindowsEventLog` sectie van de diagnostics.wadcfgx-bestand, zoals in het volgende voorbeeld.

```
<WindowsEventLog scheduledTransferPeriod="PT1M">
   <DataSource name="Application!*" />
   <DataSource name="CustomDataSource!*" />
</WindowsEventLog>
```
### <a name="performance-counters"></a>Prestatiemeteritems

Gegevens van prestatiemeteritems kunt u Zoek systeemknelpunten en afstemmen van de systeemprestaties en de toepassingen. [Maken en het gebruik van prestatiemeteritems in een toepassing Azure](https://msdn.microsoft.com/library/azure/hh411542.aspx) Zie voor meer informatie. Als u wilt voor het vastleggen van prestatiemeteritems, selecteert u het selectievakje **Enable overdracht van prestatiemeteritems** . U kunt vergroten of verkleinen van het aantal minuten wanneer de gebeurtenislogboeken worden overgebracht naar uw rekening opslag door de waarde **Transfer periode (min)** . Schakel de selectievakjes voor de items die u wilt bijhouden.

  ![Prestatiemeteritems](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758147.png)

Voor het bijhouden van een Prestatiemeter die niet wordt vermeld, voert u deze met behulp van de syntaxis van de voorgestelde en kies vervolgens de knop **toevoegen** . Het besturingssysteem op de virtuele machine wordt bepaald welke prestatiemeteritems die u kunt bijhouden. Zie voor meer informatie over de syntaxis van [u een itempad opgeeft](https://msdn.microsoft.com/library/windows/desktop/aa373193.aspx).

### <a name="infrastructure-logs"></a>Infrastructuur-Logboeken

Als u opnemen zich aanmeldt infrastructuur, die informatie over de Azure diagnostische infrastructuur, de Remote Access-module en de RemoteForwarder-module wilt bevatten, selecteer het selectievakje **overdracht van infrastructuur Logboeken inschakelen** . U kunt vergroten of verkleinen van het aantal minuten wanneer de logboeken worden overgebracht naar uw rekening opslag door de waarde Transfer periode (min).

  ![Diagnostische infrastructuur Logboeken](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758148.png)

  Zie [Logboekregistratie gegevens verzamelen door met behulp van Azure diagnostische gegevens](https://msdn.microsoft.com/library/azure/gg433048.aspx) voor meer informatie.

### <a name="log-directories"></a>Logboek voor mappen

Als u wilt vastleggen logboek mappen met gegevens die worden verzameld uit mappen logboek voor aanvragen voor Internet Information Services (IIS), mislukte aanvragen of mappen die u kiest, schakel het selectievakje **overdracht van mappen in logboek inschakelen** . U kunt vergroten of verkleinen van het aantal minuten wanneer de logboeken worden overgebracht naar uw rekening opslag door de waarde **Transfer periode (min)** .

Kunt u de vakken van de logboeken die u verzamelen wilt, zoals **IIS-logboeken** en logboeken voor **Aanvraag is mislukt** . Standaardnamen voor opslag container worden geleverd, maar u kunt de namen wijzigen als u wilt.

U kunt ook Logboeken vanuit een willekeurige map vastleggen. Alleen het pad opgeven in de sectie **Log uit Absolute Directory** en kies vervolgens de knop **Map toevoegen** . De logbestanden wordt vastgelegd op de opgegeven containers.

  ![Logboek voor mappen](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796665.png)

### <a name="etw-logs"></a>ETW-Logboeken

Als u gebruik [Event Tracing voor Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803(v=vs.85).aspx) (ETW) wilt vastleggen ETW Logboeken, selecteert het selectievakje **overdracht van ETW-Logboeken inschakelen** . U kunt vergroten of verkleinen van het aantal minuten wanneer de logboeken worden overgebracht naar uw rekening opslag door de waarde **Transfer periode (min)** .

De gebeurtenissen worden vastgelegd in bronnen van gebeurtenissen en gebeurtenis manifesten die u opgeeft. Als u een gebeurtenisbron, voer een naam in de sectie **Bronnen van gebeurtenissen** en kies vervolgens de knop **Bron toevoegen** . U kunt ook een manifest gebeurtenis opgeven in de sectie **Gebeurtenis zich voordoet** en kies vervolgens de knop **Gebeurtenis Manifest toevoegen** .

  ![ETW-Logboeken](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766025.png)

  Het framework ETW wordt in ASP.NET ondersteund door de klassen in [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110)-naamruimte. De Microsoft.WindowsAzure.Diagnostics naamruimte overneemt en uitgebreid standaard [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110)-klassen, kunt u gebruikmaken van [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110) als een kader vastleggen in de Azure-omgeving. Zie voor meer informatie, [het nemen van logboekregistratie en tracering in Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) en [Diagnose inschakelen in Azure Cloud Services en virtuele Machines](./cloud-services/cloud-services-dotnet-diagnostics.md).

### <a name="crash-dumps"></a>Crashdumps

Als u wilt voor het vastleggen van informatie over wanneer een instantie rol loopt, selecteert u het selectievakje **Enable overdracht van Crash dumpen** . (Omdat ASP.NET de meeste uitzonderingen verwerkt, dit is meestal alleen nuttig voor werknemer rollen.) U kunt vergroten of verkleinen van de hoeveelheid opslagruimte die gewijd is aan de crashdumps door het wijzigen van de waarde van de **Quota van de map (%)** . U kunt de opslag container waarin de crashdumps worden opgeslagen en kunt u aangeven of u wilt vastleggen van een **volledige** of een **Mini** -dump.

De processen die momenteel worden bijgehouden, worden weergegeven. Schakel de selectievakjes voor de processen die u wilt vastleggen. Voer de procesnaam een ander proces toevoegen aan de lijst, en klik op de knop **Toevoegen** .

  ![Crashdumps](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766026.png)

  Zie [het nemen van logboekregistratie en tracering in Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) en [Microsoft Azure Diagnostics-Part 4: Custom Logging onderdelen en Azure Diagnostics 1.3 wijzigingen](http://justazure.com/microsoft-azure-diagnostics-part-4-custom-logging-components-azure-diagnostics-1-3-changes/) voor meer informatie.

## <a name="view-the-diagnostics-data"></a>De diagnostische gegevens weergeven

Als u de diagnostische gegevens van een service cloud of een virtuele machine hebt verzameld, kunt u deze bekijken.

### <a name="to-view-cloud-service-diagnostics-data"></a>Cloud service diagnostische gegevens weergeven

1. Uw cloud-service als de gebruikelijke implementeren en uitvoeren.

1. U kunt de diagnostische gegevens weergeven in een rapport dat Visual Studio is gegenereerd of tabellen in uw account voor opslag. De gegevens in een rapport weergeven, **Cloud Explorer** of de **Server Explorer**openen, opent u het snelmenu van het knooppunt voor de rol die u interesseert en klik op **Diagnostische gegevens weergeven**.

    ![Diagnostische gegevens](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748912.png)

    Een rapport van de beschikbare gegevens wordt weergegeven.

    ![Microsoft Azure diagnostisch rapport in Visual Studio](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796666.png)

    Als u niet de meest recente gegevens is het mogelijk om te wachten op de transfer periode moet verstrijken.

    Kies de koppeling **vernieuwen** de gegevens direct bijwerken of kiezen uit een interval in de vervolgkeuzelijst voor **Automatisch vernieuwen** de gegevens automatisch bijgewerkt. De om foutgegevens te exporteren, klik op de knop **exporteren naar CSV** voor het maken van een CSV-bestand die u in een werkblad openen kunt.

    **Cloud Explorer** of **Server Explorer**, open het opslag-account die is gekoppeld aan de implementatie.

1. De diagnostische gegevens van tabellen in de tabel viewer openen en bekijk de gegevens die u verzameld. U kunt een container blob openen voor IIS-logboeken en logboeken voor aangepaste. Aan de hand van de volgende tabel vindt u de tabel of blob-container die de gegevens bevat die u interesseert. Naast de gegevens voor dit logboekbestand bevatten items in de tabel EventTickCount, DeploymentId, rol- en RoleInstance om te identificeren welke virtuele machine en de rol van de gegevens gegenereerd en wanneer. 

  	|Diagnostische gegevens|Beschrijving|Locatie|
  	|---|---|---|
  	|Toepassingslogboeken|De logboeken die uw code wordt gegenereerd door het aanroepen van methoden van de klasse System.Diagnostics.Trace.|WADLogsTable|
  	|Gebeurtenislogboeken|Deze gegevens zijn van de Windows-gebeurtenislogboeken op de virtuele machines. Windows slaat informatie in deze logboeken, maar toepassingen en services ook gebruiken om fouten te melden of logboekgegevens.|WADWindowsEventLogsTable|
  	|Prestatiemeteritems|U kunt gegevens verzamelen van de prestatiemeteritems die beschikbaar is op de virtuele machine. Het besturingssysteem bevat prestatiemeteritems, waaronder veel statistieken, zoals geheugen en de processor tijd.|WADPerformanceCountersTable|
  	|Infrastructuur-Logboeken|Deze logboeken worden gegenereerd op basis van de diagnostische infrastructuur zelf.|WADDiagnosticInfrastructureLogsTable|
  	|IIS-logboeken|Deze logboeken vastleggen webverzoeken. Als uw service cloud een aanzienlijke hoeveelheid verkeer wordt, zijn deze logboeken heel lang u moet verzamelen en opslaan van deze gegevens alleen wanneer u deze nodig hebt.|Kan geen verzoek geregistreerd in de blob bij af-iis-failedreqlogs onder een pad dat inzetten en de rol en de instantie te vinden. Hier vindt u complete logs onder af-iis-logboekbestanden. Vermeldingen voor elk bestand worden aangebracht in de tabel WADDirectories.|
  	|Crashdumps|Deze informatie bevat binaire installatiekopieën van de cloud-service-proces (meestal een rol werknemer).|af-crush-dumps blob-container|
  	|Aangepaste logboekbestanden|De logboeken van de gegevens die u vooraf gedefinieerd.|Kunt u in de code de locatie van aangepaste logboekbestanden in uw account voor opslag. Zo kunt u een aangepaste blob-container.|

1. Als gegevens van elk type is afgebroken, kunt u proberen te verhogen van de buffer voor gegevens type of het verkorten van het interval tussen de overdrachten van de virtuele machine op uw account voor de opslag van gegevens.

1. (optioneel) Gegevens verwijderen uit de opslag rekening af en toe aan de totale opslagkosten verminderen.

1. Wanneer u een volledige implementatie, het bestand diagnostics.cscfg (.wadcfgx voor Azure SDK 2.5) wordt bijgewerkt in Azure en wijzigingen in de configuratie van de diagnostische gegevens van uw cloud-service hervat. Als u, in plaats daarvan een bestaande implementatie bijwerken, wordt het bestand .cscfg niet bijgewerkt in Azure. U nog steeds kunt Diagnostische instellingen wijzigen, maar door de stappen in de volgende sectie. Zie voor meer informatie over het uitvoeren van een volledige implementatie en het bijwerken van een bestaande installatie [Azure Application Wizard publiceren](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-view-virtual-machine-diagnostics-data"></a>Virtuele machine diagnostische gegevens weergeven

1. Kies in het snelmenu voor de virtuele machine **-Diagnostische gegevens weergeven**.

    ![Diagnostische gegevens weergeven in Azure virtuele machine](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766027.png)

    Hiermee opent u het venster **diagnostiek samenvatting** .

    ![Azure VM diagnostics-overzicht](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796667.png)  

    Als u niet de meest recente gegevens is het mogelijk om te wachten op de transfer periode moet verstrijken.

    Kies de koppeling **vernieuwen** de gegevens direct bijwerken of kiezen uit een interval in de vervolgkeuzelijst voor **Automatisch vernieuwen** de gegevens automatisch bijgewerkt. De om foutgegevens te exporteren, klik op de knop **exporteren naar CSV** voor het maken van een CSV-bestand die u in een werkblad openen kunt.

## <a name="configure-cloud-service-diagnostics-after-deployment"></a>Cloud service diagnostische gegevens configureren na implementatie

Als u bij het onderzoeken van een probleem met een cloud service die al wordt uitgevoerd, kunt u het verzamelen van gegevens die u hebt opgegeven voordat u de rol oorspronkelijk geïmplementeerd. In dit geval kunt u beginnen met het verzamelen van gegevens met behulp van de instellingen in Server Explorer. U kunt diagnostische gegevens voor een enkele instantie of alle instanties in een rol, afhankelijk van of u in het dialoogvenster configuratie van diagnostische vanuit het snelmenu voor de instantie of de rol opent. Als u het knooppunt rol is geconfigureerd, worden eventuele wijzigingen gelden voor alle exemplaren. Als u het exemplaar van knooppunt is geconfigureerd, worden eventuele wijzigingen gelden voor alleen dat exemplaar.

### <a name="to-configure-diagnostics-for-a-running-cloud-service"></a>Diagnostische gegevens voor een actieve cloud-service configureren

1. Vouw het knooppunt **Services Cloud** in Server Explorer, en vouw de knooppunten om te zoeken naar de rol of de instantie die u wilt onderzoeken of beide.

    ![Diagnostische gegevens configureren](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748913.png)

1. In het snelmenu voor een exemplaar van knooppunt of een knooppunt rol **Update diagnostische instellingen**kiezen en kies vervolgens de diagnostische instellingen die u wilt verzamelen.

    Zie **Diagnostische gegevens-bronnen configureren** in dit onderwerp voor meer informatie over de configuratie-instellingen. Zie de **weergave van de diagnostische gegevens** in dit onderwerp voor meer informatie over het weergeven van de diagnostische gegevens.

    Als u het verzamelen van gegevens in **Server Explorer**wijzigt, blijven deze wijzigingen van kracht totdat u uw cloud-service volledig opnieuw implementeren. Als u de standaardwaarde gebruiken in publicatie-instellingen, de wijzigingen niet worden overschreven, omdat de standaard publiceren wordt de bestaande implementatie bijwerken in plaats van een volledig nieuwe installatie te doen. Ga naar het tabblad **Geavanceerde instellingen** in de wizard Publiceren om te controleren of dat de instellingen bij de implementatie wissen en schakel het selectievakje **bijwerken van implementatie** . Wanneer u opnieuw met deze checkbox is uitgeschakeld, terug de instellingen die in het bestand .wadcfgx (of .wadcfg) zoals ingesteld via de Properties-editor voor de rol. Als u uw implementatie bijwerken, houdt Azure de oude instellingen.

## <a name="troubleshoot-azure-cloud-service-issues"></a>Problemen met Azure cloud-service

Als u ondervindt problemen met uw cloud service-projecten, zoals een rol die niet verder komt dan de status 'bezet', herhaaldelijk wordt gerecycled of een interne serverfout genereert, zijn er hulpprogramma's en technieken waarmee u kunt vaststellen en oplossen van deze problemen. Zie voor specifieke voorbeelden van veelvoorkomende problemen en oplossingen, alsmede een overzicht van de concepten en hulpprogramma's voor het opsporen en oplossen van dergelijke fouten, [Azure PaaS berekenen diagnostische gegevens](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

## <a name="q--a"></a>Q & A

**Wat is de grootte van de buffer, en hoe groot moet het zijn?**

Van elk exemplaar van de virtuele machine beperken quota hoeveel diagnostische gegevens kan worden opgeslagen op het lokale bestandssysteem. Bovendien geven u een buffergrootte voor elk type van de diagnostische gegevens die beschikbaar is. De grootte van deze buffer fungeert als een quotum voor dat type gegevens. Onder aan het dialoogvenster te controleren, kunt u bepalen het totale quotum en de hoeveelheid geheugen dat aanwezig blijft. Als u grotere buffers of meerdere typen gegevens opgeeft, kunt u het totale quotum benadert. Doordat het configuratiebestand diagnostics.wadcfg/.wadcfgx kunt u het totale quotum. De diagnostische gegevens worden opgeslagen op het hetzelfde bestandssysteem als de gegevens van uw toepassing, dus als uw toepassing gebruikmaakt van een grote hoeveelheid schijfruimte, kunt u het algemene diagnostische gegevens van het contingent niet moet verhogen.

**Wat is de periode voor de overdracht en hoe lang moet het zijn?**

De transfer periode is de hoeveelheid tijd die is verstreken tussen de gegevens worden vastgelegd. Na elke periode overdracht gegevens verplaatst van het lokale bestandssysteem op een virtuele machine aan tabellen in uw account voor de opslag. Als de hoeveelheid gegevens die worden verzameld het quotum voor het einde van een periode van overdracht overschrijdt, oudere gegevens verwijderd. U kunt de transfer periode verkleinen als u wilt dat gegevens verloren gaan omdat de gegevens groter is dan de buffergrootte of het totale quotum.

**Welke tijdzone zijn de tijdstempels in?**

De tijdstempels worden in de lokale tijdzone van het datacenter die fungeert als host voor de service cloud. De volgende drie timestamp-kolommen in de tabellen van het logboek worden gebruikt.

  - **PreciseTimeStamp** is de ETW-tijdstempel van de gebeurtenis. Dat wil zeggen, de tijd van de client wordt de gebeurtenis vastgelegd.

  - **Tijdstempel** wordt naar beneden afgerond op de grens van uploaden frequentie PreciseTimeStamp. Dus, als uw upload frequentie 5 minuten en de gebeurtenis tijd 00:17:12, tijdstempel ook 00:15:00 uur.

  - **Tijdstempel** wordt het tijdstip waarop de entiteit is gemaakt in de tabel Azure.

**Hoe beheer ik kosten bij het verzamelen van gegevens?**

De standaardinstellingen (**logboekniveau** ingesteld op **fout** - en **Transfer periode** instellen op **1 minuut**) zijn ontworpen om de kosten te minimaliseren. Als u meer diagnostische gegevens te verzamelen of de transfer periode verlaagt verhoogt uw kosten berekenen. Niet meer gegevens dan u nodig hebt en u niet vergeten om het verzamelen van gegevens uit te schakelen wanneer u niet langer verzamelen. Altijd kunt u dit opnieuw, zelfs tijdens runtime, zoals in de vorige sectie.

**Hoe ik mislukt verzoek logboeken van IIS verzamelen?**

Standaard verzamelen geen IIS-logboeken kan geen verzoek. U kunt IIS om ze te verzamelen als u het bestand web.config voor de rol van uw webpagina bewerken.

**Ik krijg geen trace-informatie van RoleEntryPoint methoden zoals OnStart. Wat is er aan de hand?**

De methoden van RoleEntryPoint worden in het kader van WAIISHost.exe, niet door IIS genoemd. Daarom de configuratie-informatie in web.config die normaal gesproken schakelt tracering niet van toepassing. Een .config-bestand toevoegen aan uw webproject rol dit probleem op te lossen en de naam van het bestand overeenkomt met de uitvoer assembly met de code RoleEntryPoint. De naam van het .config-bestand wordt in het standaard rol webproject WAIISHost.exe.config. Voeg de volgende regels toe aan dit bestand:

```
<system.diagnostics>
  <trace>
      <listeners>
          <add name “AzureDiagnostics” type=”Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener”>
              <filter type=”” />
          </add>
      </listeners>
  </trace>
</system.diagnostics>
```

Nu in het venster **Eigenschappen van** de **kopie naar de uitvoermap** eigenschap instellen op **altijd kopiëren**.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over diagnostische gegevens vastleggen in Azure, [Diagnostics inschakelen in Azure Cloud Services en virtuele Machines](./cloud-services/cloud-services-dotnet-diagnostics.md) en [Diagnostische logboekregistratie voor web apps in Azure App-Service in te schakelen](./app-service-web/web-sites-enable-diagnostic-log.md).
