<properties
   pageTitle="Foutafhandeling Azure automatisering | Microsoft Azure"
   description="Dit artikel bevat elementaire fout afhandeling stappen voor het oplossen van algemene fouten van Azure automatisering."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="stevenka"
   editor="tysonn"
   tags="top-support-issue"
   keywords="automatiseringsfout foutafhandeling"/>
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/06/2016"
   ms.author="sngun; v-reagie"/>

# <a name="error-handling-tips-for-common-azure-automation-errors"></a>Tips voor algemene Azure Automatiseringsfouten afhandelen

Dit artikel worden enkele algemene Azure automatisering fouten u zich kan voordoen en mogelijke stappen voor foutafhandeling voorgesteld.

## <a name="troubleshoot-authentication-errors-when-working-with-azure-automation-runbooks"></a>Verificatie fouten bij het werken met Azure automatisering runbooks  

### <a name="scenario-sign-in-to-azure-account-failed"></a>Scenario: Aanmelden bij Azure Account is mislukt

**Fout:** Het foutbericht "Unknown_user_type: onbekende gebruikerstype" bij het werken met de cmdlets Add-AzureAccount of aanmelding AzureRmAccount.

**De reden van de fout:** Deze fout treedt op als de referentie Activumnaam ongeldig is of als de gebruikersnaam en het wachtwoord dat u gebruikt voor het instellen van de automatisering referentie activa niet geldig zijn.

**Tips voor probleemoplossing:** Om te bepalen wat er mis is, moet u de volgende stappen uitvoeren:  

1. Zorg ervoor dat er geen speciale tekens, met inbegrip van de **@** tekens in de naam van automatisering referentie actief waarmee u verbinding maakt met Azure.  

2. Controleer of u de gebruikersnaam en het wachtwoord die zijn opgeslagen in de referentie voor automatisering van Azure in PowerShell ISE editor kunt gebruiken. U kunt dit doen door de volgende cmdlets uit te voeren in de PowerShell ISE:  

        $Cred = Get-Credential  
        #Using Azure Service Management   
        Add-AzureAccount –Credential $Cred  
        #Using Azure Resource Manager  
        Login-AzureRmAccount –Credential $Cred

3. Als de verificatie lokaal mislukt, betekent dit dat u nog niet uw referenties Azure Active Directory correct ingesteld. Verwijzen naar het blogbericht [Authenticating naar Azure Azure Active Directory wordt gebruikt](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) om de Azure Active Directory-account correct is ingesteld.  


### <a name="scenario-unable-to-find-the-azure-subscription"></a>Scenario: Kan het abonnement Azure vinden

**Fout:** Het foutbericht ' het abonnement met de naam ``<subscription name>`` kan niet worden gevonden ' wanneer u werkt met de cmdlets selecteren AzureSubscription of selecteer AzureRmSubscription.

**De reden van de fout:** Deze fout treedt op als de naam van het abonnement is niet geldig of als de Azure Active Directory-gebruiker die probeert om de details van het abonnement niet is geconfigureerd als een beheerder van het abonnement.

**Tips voor probleemoplossing:** Om te bepalen of u naar Azure hebt geverifieerd en toegang hebben tot het abonnement dat u wilt selecteren, moet u de volgende stappen uitvoeren:  

1. Zorg ervoor dat de **Add-AzureAccount** uit te voeren voordat u de cmdlet **Selecteren AzureSubscription** .  

2. Als u dit foutbericht nog steeds weergegeven, uw code wijzigen door de cmdlet **Get-AzureSubscription** is na de cmdlet **Add-AzureAccount** toe te voegen en vervolgens de code uitvoert.  Controleer nu of als de uitvoer van de Get-AzureSubscription de details van uw abonnement bevat.  
    * Als u alle details van het abonnement in de uitvoer niet ziet, betekent dit dat het abonnement nog niet is geïnitialiseerd.  
    * Als u de details van het abonnement in de uitvoer ziet, ervoor zorgen dat u de juiste abonnementsnaam of ID met de cmdlet **Selecteren AzureSubscription** .   


### <a name="scenario-authentication-to-azure-failed-because-multi-factor-authentication-is-enabled"></a>Scenario: Azure-verificatie is mislukt omdat het meerledige verificatie is ingeschakeld.

**Fout:** Het foutbericht ' Add-AzureAccount: AADSTS50079: sterke verificatie inschrijving (bewijs-up) is vereist ' bij het verifiëren van Azure met uw Azure gebruikersnaam en wachtwoord.

**De reden van de fout:** Als u meerledige verificatie op uw Azure-account hebt, kunt u Azure Active Directory-gebruiker niet gebruiken voor het verifiëren van Azure.  In plaats daarvan moet u een certificaat of een service principal gebruiken voor verificatie bij Azure.

**Tips voor probleemoplossing:** Als u wilt een certificaat met de cmdlets Azure Service Management gebruikt, raadpleegt u [maken en het toevoegen van een certificaat voor het beheren van services op Azure.](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx) Zie [service principal met Azure portal maken](./resource-group-create-service-principal-portal.md) voor informatie over het gebruik van een service principal met Azure Resource Manager-cmdlets en [verificatie van een service principal met Azure bronnenbeheerder.](./resource-group-authenticate-service-principal.md)


## <a name="troubleshoot-common-errors-when-working-with-runbooks"></a>Het oplossen van veelvoorkomende fouten bij het werken met runbooks

### <a name="scenario-runbook-fails-because-of-deserialized-object"></a>Scenario: Runbook is mislukt vanwege een gedeserialiseerde object

**Fout:** De runbook mislukt met de fout ' kan niet binden parameter ``<ParameterName>``. Kan niet worden geconverteerd de ``<ParameterType>`` de waarde van het type Deserialized ``<ParameterType>`` typt ``<ParameterType>``".

**De reden van de fout:** Als uw runbook een werkstroom PowerShell is, opgeslagen complexe objecten in een gedeserialiseerde indeling om de status van uw runbook worden gehandhaafd als de werkstroom wordt onderbroken.  

**Tips voor probleemoplossing:**  
Een van de volgende drie oplossingen het probleem opgelost:

1. Als u complexe objecten uit een cmdlet naar de andere zijn sluizen, Wikkel deze cmdlets in een InlineScript.  
2. De naam of de waarde die u nodig hebt uit de complexe object in plaats van het gehele object doorgeven.  

3. Een runbook PowerShell gebruiken in plaats van een werkstroom PowerShell runbook.  


### <a name="scenario-runbook-job-failed-because-the-allocated-quota-exceeded"></a>Scenario: Runbook taak is mislukt omdat de toegewezen quota overschreden

**Fout:** Uw taak runbook mislukt met de fout "het quotum voor de maandelijkse totale project runtime is bereikt voor dit abonnement".

**De reden van de fout:** Deze fout treedt op wanneer de taakuitvoering groter is dan de 500 minuten gratis quota voor uw account. Dit quotum is van toepassing op alle soorten taken uitvoeren zoals het testen van een taak, een taak starten vanuit de portal, een taak wordt uitgevoerd met behulp van webhooks en planning van een taak uit te voeren met behulp van de Azure portal of in uw datacenter. Zie meer informatie over prijzen voor automatisering [automatisering prijzen](https://azure.microsoft.com/pricing/details/automation/).

**Tips voor probleemoplossing:** Als u met meer dan 500 minuten behandeling per maand moet u uw abonnement te wijzigen van de vrij laag naar de Basic laag. U kunt upgraden naar de Basic laag door de volgende stappen:  

1. Aanmelden bij uw abonnement Azure  
2. Selecteer de account automatisering die u wilt bijwerken  
3. Klik op **Instellingen** > **prijzen laag en het gebruik van** > **prijzen laag**  
4. Selecteer op het blad **kiezen de prijzen laag** **Basic**    


### <a name="scenario-cmdlet-not-recognized-when-executing-a-runbook"></a>Scenario: Cmdlet niet herkend bij het uitvoeren van een runbook

**Fout:** De functie runbook mislukt met de fout '``<cmdlet name>``: de term ``<cmdlet name>`` wordt niet herkend als de naam van een cmdlet, functie, script-bestand of programma. "

**De reden van de fout:** Deze fout wordt veroorzaakt wanneer de PowerShell-engine kan de cmdlet die u in uw runbook gebruikt niet vinden.  Het is mogelijk dat de module met de cmdlet in de account ontbreekt, er een naamconflict met een runbook naam wordt, of de cmdlet ook in een andere module bestaat en automatisering kan de naam niet omzetten.

**Tips voor probleemoplossing:** Een van de volgende oplossingen wordt het probleem opgelost:  

- Controleer de naam van de cmdlet juist ingevoerd.  

- Controleer of de cmdlet bestaat in uw account automatisering en er zijn geen conflicten. Om te controleren of de cmdlet aanwezig is, opent u een runbook in de bewerkingsmodus en zoek naar de cmdlet die u wilt zoeken in de bibliotheek of run **opdracht Get ``<CommandName>`` **.  Zodra u hebt die de cmdlet beschikbaar voor de account is gevalideerd en dat er geen naamconflicten optreden met andere cmdlets of runbooks, toe te voegen aan het canvas en controleert u of u een geldige parameter instellen in uw runbook.  

- Als er een naamconflict en de cmdlet beschikbaar in twee verschillende modules is, kunt u dit oplossen met behulp van de volledig gekwalificeerde naam voor de cmdlet. U kunt bijvoorbeeld **ModuleName\CmdletName**.  

- Als u de runbook van gebouwen in een hybride-groep voor werkprocessen worden uitgevoerd, moet u ervoor zorgen dat de module/cmdlet geïnstalleerd op de computer waarop de werknemer hybride.


### <a name="scenario-a-long-running-runbook-consistently-fails-with-the-exception-the-job-cannot-continue-running-because-it-was-repeatedly-evicted-from-the-same-checkpoint"></a>Scenario: Een langdurige runbook consequent niet met de uitzondering: "de taak kan niet worden voortgezet omdat het herhaaldelijk is verwijderd uit de dezelfde controlepunt wordt uitgevoerd".

**De reden van de fout:** Dit is de ontwerp-probleem als gevolg van de "Billijk aandeel" controle van processen binnen Azure automatisering die automatisch een runbook onderbreekt als deze wordt uitgevoerd langer dan 3 uur. Echter het foutbericht geretourneerd biedt geen "volgende stap" opties. Een runbook kan worden geschorst om een aantal redenen. Hiermee onderbreekt u gebeurt voornamelijk te wijten aan fouten. Bijvoorbeeld een niet-afgevangen uitzondering in een runbook, een netwerkfout of een crash op de werknemer van de Runbook met de runbook, al zal de runbook worden opgeschort en starten vanaf de laatste checkpoint als hervat.

**Tips voor probleemoplossing:** De beschreven oplossing om te voorkomen dat dit probleem is met controlepunten in een werkstroom.  Voor meer informatie over dat meer verwijzen naar [Leren PowerShell werkstromen](automation-powershell-workflow.md#Checkpoints).  Een grondiger beschrijving van "Billijk aandeel" en Checkpoint vindt u in dit blog-artikel [Met behulp van controlepunten in Runbooks](https://azure.microsoft.com/en-us/blog/azure-automation-reliable-fault-tolerant-runbook-execution-using-checkpoints/).


## <a name="troubleshoot-common-errors-when-importing-modules"></a>Het oplossen van veelvoorkomende fouten bij het importeren van modules

### <a name="scenario-module-fails-to-import-or-cmdlets-cant-be-executed-after-importing"></a>Scenario: Module niet kan worden geïmporteerd of cmdlets na het importeren kan niet worden uitgevoerd

**Fout:** Een module niet kan worden geïmporteerd of worden geïmporteerd met succes, maar geen cmdlets worden uitgepakt.

**De reden van de fout:** Er zijn enkele algemene oorzaken van een module kan niet met succes geïmporteerd naar Azure automatisering:  

- De structuur komt niet overeen met de structuur die automatisering moet in.  

- De module is afhankelijk van een andere module die aan uw account voor automatisering niet is geïmplementeerd.  

- De module ontbreken de afhankelijkheden in de map.  

- De cmdlet **New-AzureRmAutomationModule** wordt gebruikt voor het uploaden van de module en u niet het volledige opslag pad hebben of de module niet geladen met behulp van een openbaar toegankelijke URL.  

**Tips voor probleemoplossing:**  
Een van de volgende oplossingen wordt het probleem opgelost:  

- Zorg ervoor dat de module de volgende notatie volgt:  
ModuleName.Zip **->** modulenaam of het versienummer **->** (ModuleName.psm1, ModuleName.psd1)

- Open het bestand .psd1 en controleert u of de module afhankelijkheden heeft.  Als dit het geval is, upload deze modules op de rekening van de automatisering.  

- Controleer of de DLL waarnaar wordt verwezen in de map module.  


## <a name="troubleshoot-common-errors-when-working-with-desired-state-configuration-dsc"></a>Het oplossen van veelvoorkomende fouten bij het werken met de gewenste status configuratie (DSC)  

### <a name="scenario-node-is-in-failed-status-with-a-not-found-error"></a>Scenario: Knooppunt is in de status mislukt met een fout 'Niet gevonden'

**Fout:** Het knooppunt is een rapport met de status **mislukt** en met de fout ' de poging om de actie van https:// server``<url>``//accounts/``<account-id>``Nodes(AgentId=``<agent-id>``)/GetDscAction is mislukt omdat een geldige configuratie ``<guid>`` kan niet worden gevonden. "

**De reden van de fout:** Deze fout treedt gewoonlijk op wanneer het knooppunt is toegewezen aan de naam van een configuratie (bijvoorbeeld ABC) in plaats van een knooppuntnaam configuratie (bijvoorbeeld ABC. WebServer).  

**Tips voor probleemoplossing:**  

- Zorg ervoor dat u bij het toewijzen van het knooppunt met het ' knooppunt configuratie de naam' en niet de ' configuratie '.  

- U kunt de configuratie van een knooppunt op een knooppunt met Azure portal of met een PowerShell-cmdlet.
    - Een configuratie van knooppunten aan een knooppunt met Azure portal toewijzen, opent de **DSC-knooppunten** bladeservers, en vervolgens selecteert u een knooppunt en klik op **knooppuntconfiguratie toewijzen** .  
    - Een configuratie van knooppunten aan een knooppunt met PowerShell-cmdlet toewijzen, gebruikt u de cmdlet **Set-AzureRmAutomationDscNode**


### <a name="scenario--no-node-configurations-mof-files-were-produced-when-a-configuration-is-compiled"></a>Scenario: Geen knooppunt configuraties (MOF-bestanden) zijn gemaakt tijdens het compileren van een configuratie

**Fout:** Uw taak DSC compilatie wordt uitgesteld met de fout: "de compilatie met succes is voltooid, maar geen knooppunt configuratie .mofs zijn gegenereerd".

**De reden van de fout:** Wanneer de volgende expressie het sleutelwoord **knooppunt** in de configuratie van DSC evalueert $null vervolgens geen knooppunt configuraties worden gemaakt.    

**Tips voor probleemoplossing:**  
Een van de volgende oplossingen wordt het probleem opgelost:  

- Zorg ervoor dat de expressie naast het sleutelwoord **knooppunt** in de definitie van de configuratie niet wordt beoordeeld op $null.  
- Als u ConfigurationData bij het compileren van de configuratie doorgeeft, moet u doorgeven van de verwachte waarden waarvoor de configuratie van [ConfigurationData](automation-dsc-compile.md#configurationdata).


### <a name="scenario--the-dsc-node-report-becomes-stuck-in-progress-state"></a>Scenario: Het knooppunt DSC rapport Usbhub.sys vast staat "in uitvoering"

**Fout:** DSC-Agent wordt de uitvoer 'Geen exemplaar gevonden met de opgegeven eigenschapswaarden '.

**De reden van de fout:** U de WMF-versie hebt bijgewerkt en WMI hebt beschadigd.  

**Tips voor probleemoplossing:** Volg de instructies in het weblogbericht [DSC bekende problemen en beperkingen](https://msdn.microsoft.com/powershell/wmf/limitation_dsc) om het probleem te verhelpen.


### <a name="scenario--unable-to-use-a-credential-in-a-dsc-configuration"></a>Scenario: Kan een referentie gebruiken in een configuratie met DSC

**Fout:** De compilatie DSC taak is onderbroken met de fout: ' System.InvalidOperationException fout bij het verwerken van de eigenschap 'Referenties' van het type ``<some resource name>``: converteren en opslaan van een gecodeerd wachtwoord als leesbare tekst is alleen toegestaan als PSDscAllowPlainTextPassword is ingesteld op true ".

**De reden van de fout:** Een referentie in een configuratie hebt gebruikt, maar niet bieden goede **ConfigurationData** om de **PSDscAllowPlainTextPassword** ingesteld op true voor elke knooppuntconfiguratie.  

**Tips voor probleemoplossing:**  
- Zorg dat u doorgeeft in de juiste **ConfigurationData** op **PSDscAllowPlainTextPassword** ingesteld op true voor elke knooppuntconfiguratie genoemd in de configuratie. Raadpleeg voor meer informatie [activa in Azure automatisering DSC](automation-dsc-compile.md#assets).


## <a name="next-steps"></a>Volgende stappen

Als u de bovenstaande stappen hebt gevolgd en aanvullende hulp op elk punt in dit artikel, kunt u:

- Hulp van experts Azure. Geef uw probleem op de [forums van MSDN Azure of Stack Overflow.](https://azure.microsoft.com/support/forums/)

- Een incident voor ondersteuning van Azure-bestand. Ga naar de [site ondersteuning van Azure](https://azure.microsoft.com/support/options/) en **ondersteuning** op ondersteuning van **technische en facturering**.

- Een aanvraag voor een Script in [Script Center](https://azure.microsoft.com/documentation/scripts/) als u een oplossing voor automatisering Azure runbook of een integratiemodule zoekt boeken.

- Feedback verzamelen of aanvragen voor Azure automatisering op [Gebruiker gesproken](https://feedback.azure.com/forums/34192--general-feedback)boeken.
