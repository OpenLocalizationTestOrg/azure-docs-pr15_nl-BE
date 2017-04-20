<properties
    pageTitle="Windows-computers verbinding met Analytics logboek | Microsoft Azure"
    description="Dit artikel bevat de stappen op de Windows-computers in uw infrastructuur op ruimten rechtstreeks verbinden met OMS met behulp van een aangepaste versie van de Microsoft Monitoring Agent (MMA)."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>


# <a name="connect-windows-computers-to-log-analytics"></a>Windows-computers verbinding met Analytics logboek

Dit artikel bevat de stappen op de Windows-computers in uw infrastructuur op ruimten rechtstreeks verbinden met OMS werkruimten met behulp van een aangepaste versie van de Microsoft Monitoring Agent (MMA). U moet installeren en agenten voor alle computers die u wilt verbinden met on-board aan OMS om deze gegevens verzenden naar OMS te werken met gegevens in de OMS-portal. Elke agent kunt voor meerdere werkruimten melden.

U kunt installeren met behulp van Setup vanaf de opdrachtregel, agenten of met de gewenste status configuratie (DSC) in Azure automatisering.  

>[AZURE.NOTE] Voor virtuele machines uitgevoerd in Azure kan de installatie vereenvoudigen met behulp van de [uitbreiding van de virtuele machine](log-analytics-azure-vm-extension.md).

De agent gebruikt op computers met Internet-verbinding, de verbinding met het Internet om gegevens te verzenden naar OMS. Voor computers die geen internetverbinding hebt, kunt u een proxy of de doorstuurserver OMS logboek Analytics.

Uw Windows-computers verbinden met OMS is eenvoudig met behulp van 3 eenvoudige stappen:

1. Download het installatiebestand van agent
2. Installeren van de agent die met behulp van de methode die u kiest
3. De agent configureren of toevoegen van extra werkruimten, indien nodig

In het volgende diagram wordt de relatie tussen uw Windows-computers en OMS nadat u hebt geïnstalleerd en geconfigureerd agenten.

![OMS-direct-agent-diagram](./media/log-analytics-windows-agents/oms-direct-agent-diagram.png)


## <a name="system-requirements-and-required-configuration"></a>Systeemvereisten en vereiste configuratie
Voordat u installeert of agenten implementeert, bekijk de volgende zodat u voldoen aan de vereisten.

- U kunt de OMS MMA alleen installeren op computers met Windows Server 2008 SP 1 of hoger of Windows 7 SP1 of hoger.
- Je hebt een abonnement OMS.  Zie [aan de slag met Analytics logboek](log-analytics-get-started.md)voor meer informatie.
- Elke Windows-computer moet verbinding kunnen maken met het Internet via HTTPS. Deze verbinding kan worden direct via een proxyserver, of via de doorstuurserver OMS logboek Analytics.
- U kunt de OMS MMA installeren op zelfstandige computers, servers en virtuele machines. Als u verbinding Azure gehost op virtuele machines met OMS wilt, Zie [logboek Analytics voor Azure verbinding maken met virtuele machines](log-analytics-azure-vm-extension.md).
- De agent moet TCP-poort 443 gebruiken voor verschillende resources. Zie voor meer informatie, [proxy- en firewall instellingen configureren in het logboek voor Analytics](log-analytics-proxy-firewall.md).

## <a name="download-the-agent-setup-file-from-oms"></a>Download het installatiebestand van de agent uit het Kantoorbeheersysteem
1. Klik op de tegel **Instellingen** in de portal OMS op de pagina **Overzicht** .  Klik op het tabblad **Bronnen verbonden** aan de bovenkant.  
    ![Tabblad gekoppelde bronnen](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)
2. **Computers rechtstreeks koppelen**, klik op **Windows-Agent downloaden** van toepassing is op het processortype van uw computer te downloaden van het setup-bestand.
3. Aan de rechterkant van de **Werkruimte-ID**, klikt u op het pictogram kopiëren en plak de ID in Kladblok.
4. Aan de rechterkant van de **Primaire sleutel**, klik op het pictogram kopiëren en plak de sleutel in Kladblok.     
    ![Werkruimte-ID en een primaire sleutel kopiëren](./media/log-analytics-windows-agents/oms-direct-agent-primary-key.png)

## <a name="install-the-agent-using-setup"></a>De agent die met behulp van setup installeren
1. Voer Setup uit om de agent installeren op een computer die u wilt beheren.
2. Klik op de welkomstpagina op **volgende**.
3. Lees de licentie op de pagina voorwaarden van de gebruiksrechtovereenkomst en klik vervolgens op **ik ga akkoord**.
4. Op de pagina doelmap wijzigen of de standaardmap voor de installatie en klik vervolgens op **volgende**.
5. Op de pagina Setup-opties Agent kunt u verbinding maken met de agent naar Azure logboek Analytics (OMS), Operations Manager, of u kunt de opties leeg laten als u wilt dat de agent later configureren. Klik op **volgende**.   
    - Als u verbinding maken met Azure logboek Analytics (OMS), de **Werkruimte-ID** en **Sleutel werkruimte (primaire sleutel)** die u in de vorige procedure hebt gekopieerd naar het Kladblok plakken en klik op **volgende**.  
        ![Werkruimte-ID en het primaire sleutel](./media/log-analytics-windows-agents/connect-workspace.png)
    - Als u verbinding maakt met Operations Manager kiest, typt u het **Management groepsnaam**, de naam van **De Server Management** en **Management Server poort**en klik op **volgende**. Kies de account lokaal systeem of een lokale account op de pagina actie-Agent-Account en klik op **volgende**.  
        ![configuratie Management](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![agent actie-account](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)

6. Controleer uw keuzen op de pagina toepassing installeren, en klik vervolgens op **installeren**.
7. Pagina van de configuratie is voltooid, klikt u op **Voltooien**.
8. Als alles klaar is, verschijnt de **Microsoft Monitoring Agent** in **Het Configuratiescherm**. U kunt er uw configuratie bekijken en controleren of de agent is verbonden aan operationele inzichten (OMS). Wanneer verbonden met OMS, de agent wordt een bericht met de melding: **The Microsoft Monitoring Agent verbonden is met de service Microsoft Operations Management Suite.**

## <a name="install-the-agent-using-the-command-line"></a>Installeren van de agent via de opdrachtregel
- Wijzigen en vervolgens in het volgende voorbeeld te installeren van de agent via de opdrachtregel.

    >[AZURE.NOTE] Als u upgraden een agent wilt, moet u het logboek Analytics scripting API gebruiken. Zie de volgende sectie voor het upgraden van een agent.

    ```
    MMASetup-AMD64.exe /Q:A /R:N /C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1"
    ```

## <a name="upgrade-the-agent-and-add-a-workspace-using-a-script"></a>Upgrade van de agent en een werkruimte met behulp van een script toevoegen
U kunt een agent een upgrade en een werkruimte met behulp van het logboek Analytics API met het volgende voorbeeld van PowerShell scripts toevoegen.

```
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

>[AZURE.NOTE] Als u hebt gebruikt de opdracht of het script eerder installeren of configureren van de agent, `EnableAzureOperationalInsights` is vervangen door `AddCloudWorkspace`.

## <a name="install-the-agent-using-dsc-in-azure-automation"></a>Installeren van de agent met DSC in Azure automatisering

>[AZURE.NOTE] In dit voorbeeld van de procedure en het script wordt niet bijgewerkt voor een bestaande agent.

1. DSC-Module van de xPSDesiredStateConfiguration van [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) in Azure automatisering importeren.  
2.  Azure Automation-variabele elementen maken voor *OPSINSIGHTS_WS_ID* en *OPSINSIGHTS_WS_KEY*. *OPSINSIGHTS_WS_ID* aan uw werkruimte OMS logboek Analytics ID ingesteld en *OPSINSIGHTS_WS_KEY* op de primaire sleutel van de werkruimte.
3.  Gebruik het onderstaande script en sla het op als MMAgent.ps1
4.  Wijzigen en vervolgens in het volgende voorbeeld de DSC in Azure automatisering met agent installeren. MMAgent.ps1 importeren in Azure automatisering met behulp van de cmdlet of Azure Automation-interface.
5.  Een knooppunt toewijzen aan de configuratie. Binnen 15 minuten het knooppunt de configuratie wordt gecontroleerd en de MMA geduwd naar het knooppunt.

```
Configuration MMAgent
{
    $OIPackageLocalPath = "C:\MMASetup-AMD64.exe"
    $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
    $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"


    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    Node OMSnode {
        Service OIService
        {
            Name = "HealthService"
            State = "Running"
            DependsOn = "[Package]OI"
        }

        xRemoteFile OIPackage {
            Uri = "http://download.microsoft.com/download/0/C/0/0C072D6E-F418-4AD4-BCB2-A362624F400A/MMASetup-AMD64.exe"
            DestinationPath = $OIPackageLocalPath
        }

        Package OI {
            Ensure = "Present"
            Path  = $OIPackageLocalPath
            Name = "Microsoft Monitoring Agent"
            ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
            Arguments = '/C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
            DependsOn = "[xRemoteFile]OIPackage"
        }
    }
}  


```


## <a name="configure-an-agent-manually-or-add-additional-workspaces"></a>Een agent handmatig configureren of het toevoegen van extra werkruimten
Als u medewerkers hebt geïnstalleerd, maar heeft niet configureren of als u wilt dat de agent aan meerdere werkruimten, kunt u de volgende informatie een agent of configureert u deze opnieuw. Nadat u de agent hebt geconfigureerd, met de agent-service registreert en u benodigde configuratiegegevens en management packs die gegevens bevatten.

1. Nadat u Microsoft Monitoring Agent hebt geïnstalleerd, opent u **Het Configuratiescherm**.
2. Open **Microsoft Monitoring Agent** en klik vervolgens op het tabblad **Azure logboek Analytics (OMS)** .   
3. Klik op **toevoegen** om het **toevoegen van een logboek Analytics werkruimte** te openen.
4. Plak de **ID van de werkruimte** en de **Werkruimte sleutel (primaire sleutel)** die u hebt gekopieerd naar het Kladblok in een eerdere procedure voor de werkruimte die u wilt toevoegen en klik vervolgens op **OK**.  
    ![Operationele inzichten configureren](./media/log-analytics-windows-agents/add-workspace.png)

Nadat de gegevens worden verzameld van computers gecontroleerd door de agent, verschijnt het aantal computers gecontroleerd door OMS in de OMS-portal op het tabblad **Bronnen verbonden** in **Instellingen** als **Servers verbonden**.


## <a name="to-disable-an-agent"></a>Een agent uitschakelen
1. Open **Het Configuratiescherm**na de installatie van de agent.
2. Open Microsoft Monitoring Agent en klik vervolgens op het tabblad **Azure logboek Analytics (OMS)** .
3. Selecteer een werkruimte en klik vervolgens op **verwijderen**. Herhaal deze stap voor alle andere werkruimten.


## <a name="optionally-configure-agents-to-report-to-an-operations-manager-management-group"></a>Desgewenst te rapporteren aan een groep Operations Manager beheer configureren

Als u Operations Manager in uw IT-infrastructuur, kunt u ook de agent MMA als agent Operations Manager gebruiken.

### <a name="to-configure-mma-agents-to-report-to-an-operations-manager-management-group"></a>MMA agenten te rapporteren aan een groep Operations Manager beheer configureren
1.  Open **Het Configuratiescherm**op de computer waarop de agent is geïnstalleerd.
2.  Open **Microsoft Monitoring Agent** en klik vervolgens op het tabblad **Operations Manager** .
    ![Tabblad Microsoft Monitoring Agent Operations Manager](./media/log-analytics-windows-agents/om-mg01.png)
3.  Als uw servers Operations Manager integratie met Active Directory, klikt u op **automatisch bijwerken groepstoewijzingen beheer van AD DS**.
4.  Klik op **toevoegen** om het dialoogvenster **Management groep toevoegen** te openen.  
    ![Microsoft Monitoring Agent een Management-groep toevoegen](./media/log-analytics-windows-agents/oms-mma-om02.png)
5.  Typ de naam van uw management-groep in het vak **groepsnaam Management** .
6.  Typ in het vak **primaire management server** de naam van de primaire server.
7.  Typ het TCP-poortnummer in het vak **serverpoort Management** .
8.  Kies de account lokaal systeem of een lokale account onder **Agent actie-Account**.
9.  Klik op **OK** om te sluiten in het dialoogvenster **beheer groep toevoegen** en klik vervolgens op **OK** om te sluiten in het dialoogvenster **Eigenschappen van Microsoft Agent controleren** .

## <a name="optionally-configure-agents-to-use-the-oms-log-analytics-forwarder"></a>Agenten voor het gebruik van de doorstuurserver OMS logboek Analytics desgewenst configureren

Als u servers of clients die geen verbinding met het Internet hebt, kunt u nog steeds zijn gegevens verzenden naar OMS met behulp van de doorstuurserver OMS logboek Analytics.  Wanneer u de doorstuurserver, alle gegevens van agenten verzonden via een enkele server die toegang heeft tot het Internet. De doorstuurserver de gegevensoverdracht van de agents aan OMS rechtstreeks zonder het analyseren van de gegevens die worden overgebracht.

Zie [OMS logboek Analytics doorstuurservers](https://blogs.technet.microsoft.com/msoms/2016/03/17/oms-log-analytics-forwarder) voor meer informatie over de doorstuurserver, met inbegrip van de installatie en configuratie.

Voor meer informatie over het configureren van uw agenten voor het gebruik van een proxyserver, in dit geval de doorstuurserver OMS is, Zie [proxy- en firewall-instellingen in logboek Analytics configureren](log-analytics-proxy-firewall.md).

## <a name="optionally-configure-proxy-and-firewall-settings"></a>Proxy- en firewall-instellingen desgewenst configureren
Als er proxyservers en firewalls in uw omgeving met toegang tot het Internet beperkte, Zie [de proxy- en firewall instellingen configureren in logboek Analytics](log-analytics-proxy-firewall.md) uw agents te communiceren met de OMS-service inschakelen.

## <a name="next-steps"></a>Volgende stappen

- [Oplossingen voor logboek Analytics toevoegen uit de galerie met oplossingen](log-analytics-add-solutions.md) voor functionaliteit toevoegen en gegevens te verzamelen.
- [Proxy- en firewall instellingen configureren in logboek Analytics](log-analytics-proxy-firewall.md) als uw organisatie gebruikmaakt van een proxyserver of firewall zodat agenten met de logboek-Analytics-service communiceren kunnen.
