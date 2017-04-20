<properties
    pageTitle="Staging-omgevingen voor web apps in Azure App-Service instellen"
    description="Informatie over het gefaseerde publiceren gebruiken voor web apps in Azure App-Service."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    writer="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/09/2016"
    ms.author="cephalin"/>

# <a name="set-up-staging-environments-for-web-apps-in-azure-app-service"></a>Staging-omgevingen voor web apps in Azure App-Service instellen
<a name="Overview"></a>

Wanneer u uw web app met [App-Service](http://go.microsoft.com/fwlink/?LinkId=529714)implementeert, kunt u implementeren op een aparte implementatie sleuf in plaats van de standaard productie-sleuf in de **Standard** of **Premium** App Service plan-modus. Implementatie sleuven zijn daadwerkelijk live web apps met hun eigen host-namen. Inhoud en configuraties elementen van web app kunnen tussen twee sleuven van distributie, met inbegrip van de productie-sleuf worden verwisseld. Implementatie van een sleuf voor de implementatie van de toepassing heeft de volgende voordelen:

- U kunt web app wijzigingen in een gefaseerde installatie sleuf voor de implementatie valideren alvorens deze te verwisselen met de sleuf voor de productie.

- Eerst implementeren van een web app in een sleuf en wisselen zij in productie zorgt ervoor dat alle exemplaren van de sleuf worden opgewarmd voordat de productie wordt geruild. Hierdoor wordt de uitvaltijd bij het implementeren van uw web app. De omleiding van verkeer is naadloos en er geen aanvragen worden verbroken als gevolg van swap-bewerkingen. Deze hele workflow worden geautomatiseerd door te [Wisselen automatisch](#configure-auto-swap-for-your-web-app) configureren als validatie swap vooraf is niet nodig.

- Na een swap heeft de sleuf met eerder gefaseerde web app nu de vorige productie web app. Als de wijzigingen die zijn omgezet in de sleuf van de productie zijn niet zoals u verwacht, kunt u uitvoeren met de dezelfde swap onmiddellijk als u de 'laatste bekende goede site' terug.

Elke App Service plan modus ondersteunt een verschillend aantal sleuven voor implementatie. Als u wilt weten van het aantal sleuven van uw web app modus ondersteunt, kunt u [Serviceprijzen App](/pricing/details/app-service/).

- Als uw web app meerdere sleuven heeft, kunt u de modus niet wijzigen.

- Schaal is niet beschikbaar voor niet-productieve sleuven.

- Gekoppelde bronbeheer wordt niet ondersteund voor "slots" niet voor productie. U kunt in de [Portal Azure](http://go.microsoft.com/fwlink/?LinkId=529715) alleen deze potentiële gevolgen voor een productie-sleuf voorkomen door tijdelijk de sleuf niet-productieve verplaatsen in een andere App plan modus. Houd er rekening mee dat de niet-productieve sleuf opnieuw dezelfde modus met de sleuf productie delen moet voordat u de twee sleuven kunt omwisselen.


[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

<a name="Add"></a>
## <a name="add-a-deployment-slot-to-a-web-app"></a>Een sleuf implementatie toevoegen aan een web app. ##

De web app moet worden uitgevoerd in de modus **standaard** of **Premium** u om meerdere distributie-sleuven.

1. Open in de [Azure Portal](https://portal.azure.com/)van uw web app blade.
2. Klik op **Instellingen**en klik vervolgens op **Deployment sleuven**. In het blad **implementatie sleuven** Klik **Sleuf toevoegen**.

    ![Toevoegen van een nieuwe distributie-sleuf][QGAddNewDeploymentSlot]

    > [AZURE.NOTE]
    > Als de web app nog niet in de modus **standaard** of **Premium** , ontvangt u een bericht met de ondersteunde modi voor het inschakelen van gefaseerde publiceren. U hebt nu de optie **Upgrade** selecteren en Ga naar het tabblad **schaal** van uw web app voordat u verdergaat.

2. In het blad **toevoegen een sleuf** de sleuf een naam geven en selecteer of u wilt klonen web app-configuratie van een andere bestaande implementatie-sleuf. Klik op het vinkje om door te gaan.

    ![Configuratiebron][ConfigurationSource1]

    De eerste keer dat u een sleuf toevoegen, hebt u slechts twee mogelijkheden: configuratie van de kloon van het standaard slot in productie of helemaal niet.

    Nadat u meerdere sleuven hebt gemaakt, is het mogelijk klonen van de configuratie van een slot dan de productie:

    ![Configuratie van bronnen][MultipleConfigurationSources]

5. Klik op de sleuf implementatie een blade voor de sleuf, met een reeks cijfers en configuratie, net als elke andere web app geopend in de blade **-sleuven voor implementatie** . **Your-Web-App-name-Deployment-slot-name** wordt weergegeven aan de bovenkant van de blade om eraan te herinneren dat u de sleuf voor de implementatie bekijkt.

    ![Titel van implementatie-sleuf][StagingTitle]

5. Klik op de URL van de app in de sleuf van blade. Sleuf voor de implementatie heeft een eigen host-naam en het is ook een live app. Als u wilt beperken de toegang van het publiek in de sleuf voor de implementatie, Zie [App Service Web App – webpagina toegang blokkeren om "slots" niet-productie-implementatie](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).

Er is geen inhoud na implementatie sleuf maken. U kunt implementeren op de sleuf van een andere opslagplaats filiaal of een geheel andere opslagplaats. Ook kunt u de configuratie van de sleuf. Gebruik de profiel- of referenties van publiceren die zijn gekoppeld aan de implementatie sleuf voor updates van inhoud.  U kunt bijvoorbeeld [op deze sleuf met git publiceren](app-service-deploy-local-git.md).

<a name="AboutConfiguration"></a>
## <a name="configuration-for-deployment-slots"></a>Configuratie voor implementatie-sleuven ##
Als u een configuratie vanaf een andere distributie sleuf kloon, is de gekloonde configuratie kan worden bewerkt. Sommige configuratie-elementen zal bovendien de inhoud volgen via wisselbestand (geen sleuf voor specifieke) terwijl andere configuratie-elementen staan in dezelfde sleuf na een swap (specifieke sleuf blijft). Het volgende overzicht ziet de configuratie wordt gewijzigd wanneer u sleuven verwisselt.

**Instellingen die worden verwisseld**:

- Algemene instellingen - zoals framework-versie, 32/64-bits Web sockets
- App instellingen (kan worden geconfigureerd om te blijven vasthouden aan een sleuf)
- Verbindingsreeksen (kan worden geconfigureerd om te blijven vasthouden aan een sleuf)
- Handlertoewijzingen
- Instellingen voor bewaking en diagnose
- Inhoud WebJobs

**Instellingen die niet worden geruild**:

- Publishing eindpunten
- Aangepaste domeinnamen
- SSL-certificaten en bindingen
- Schaalinstellingen voor
- WebJobs planners

Als u wilt configureren een app instellen of de verbindingstekenreeks aan een sleuf (niet verwisseld), toegang tot de **Instellingen van de toepassing** -blade voor een bepaalde sleuf en selecteer vervolgens het vak **Instelling van de sleuf** voor de configuratie-elementen die de sleuf moet houden. Houd er rekening mee dat markering configuratie-element als specifieke sleuf heeft effect tot stand brengen van dat element als niet swappable over alle distributie-sleuven die is gekoppeld aan de web app.

![Instellingen van de sleuf][SlotSettings]

<a name="Swap"></a>
## <a name="to-swap-deployment-slots"></a>Implementatie van "slots" verwisselen ##

>[AZURE.IMPORTANT] Voordat u een web app in een sleuf voor de implementatie in productie verwisselt, ervoor zorgen dat alle niet-sleuf voor specifieke instellingen zijn geconfigureerd, precies zoals u wilt dat dit in het doel wisselen.

1. Om te wisselen implementatie sleuven, klikt u op de knop **wisselen** in de werkbalk van het web app of in de opdrachtbalk van een sleuf voor de implementatie. Zorg ervoor dat de swap-bron- en swap juist zijn ingesteld. Meestal is het doel wisselen de sleuf voor de productie.  

    ![Wisselen, knop][SwapButtonBar]

3. Klik op **OK** om de bewerking te voltooien. Wanneer de bewerking is voltooid, zijn de implementatie-sleuven zijn verwisseld.

## <a name="configure-auto-swap-for-your-web-app"></a>Wisselen automatisch configureren voor uw web app ##

Automatische Swap stroomlijnt DevOps scenario's waar u uw web app met nul koude start en nul uitvaltijd voortdurend implementeren voor eind-klanten van het web app. Wanneer een sleuf voor de implementatie is geconfigureerd voor automatische wisselen in productie, elke keer dat u de update van push-in deze sleuf, zal App-Service automatisch de web app wisselen in productie na al is opgewarmd in de sleuf.

>[AZURE.IMPORTANT] Wanneer u automatische wisselen voor een slot inschakelt, moet dat de configuratie van de sleuf is precies de configuratie die bestemd zijn voor de doel-sleuf (meestal de sleuf van de productie).

Configureren van Auto verwisselen voor een sleuf is eenvoudig. Voer de volgende stappen uit:

1. Selecteer een niet-productieve sleuf in de blade **-Sleuven voor implementatie** , klikt u op **Alle instellingen** voor die sleuf van blade.  

    ![][Autoswap1]

2. Klik op **instellingen van de toepassing**. Voor het **Automatische wisselen** **op** selecteren en selecteert u het gewenste doel sleuf in **Auto verwisselen sleuf**op de opdrachtbalk klikt u op **Opslaan** . Controleer of de configuratie voor de sleuf is precies de configuratie die bestemd zijn voor de doel-sleuf.

    Het tabblad **meldingen** knippert groen **voltooid** zodra de bewerking voltooid is.

    ![][Autoswap2]

    >[AZURE.NOTE] Test Auto verwisselen voor uw web app, kunt u eerst een niet-productieve doel sleuf in **Auto verwisselen sleuf** vertrouwd wilt raken met de functie selecteren.  

3. Een drukknop code in die sleuf implementatie uitvoeren. Automatische wisselen gebeurt na korte tijd en de update automatisch worden doorgevoerd in de sleuf van de doel-URL.

<a name="Multi-Phase"></a>
## <a name="use-multi-phase-swap-for-your-web-app"></a>Meerdere fasen swap gebruiken voor uw web app ##

Meerdere fasen swap is beschikbaar voor validatie in de context van de configuratie-elementen aan een sleuf zoals verbindingsreeksen ontworpen vereenvoudigen. In deze gevallen is het mogelijk handig deze configuratie-elementen van het doel wisselen van toepassing op de swap-bron en valideren voordat swap daadwerkelijk van kracht wordt. Zodra de swap doel-configuratie-elementen worden toegepast op de swap-bron de acties die beschikbaar zijn voor het voltooien van de swap of terugkeren naar de oorspronkelijke configuratie voor de swap-bron heeft ook het effect van het annuleren van de swap. De monsters voor de Azure PowerShell cmdlets die beschikbaar zijn voor het wisselbestand in meerdere fasen worden opgenomen in de Azure PowerShell-cmdlets voor implementatie van "slots" sectie.

<a name="Rollback"></a>
## <a name="to-rollback-a-production-app-after-swap"></a>Ongedaan maken van een app productie na wisselen ##

Als er fouten worden geïdentificeerd in productie na een sleuf swap, herstellen de sleuven hun lidstaten wisselen vooraf door de dezelfde twee sleuven direct te verwisselen.

<a name="Warm-up"></a>
## <a name="custom-warm-up-before-swap"></a>Aangepaste opgewarmd voordat swap ##

Bepaalde toepassingen mogelijk opgewarmd aangepaste acties. De applicationInitialization, configuratie-element in web.config kunt u aangepaste initialisatie acties opgeven die moeten worden uitgevoerd voordat een aanvraag wordt ontvangen. De swap-bewerking wacht deze aangepaste opgewarmd te voltooien. Hier is een fragment monster web.config.

    <applicationInitialization>
        <add initializationPage="/" hostName="[web app hostname]" />
        <add initializationPage="/Home/About" hostname="[web app hostname]" />
    </applicationInitialization>

<a name="Delete"></a>
## <a name="to-delete-a-deployment-slot"></a>Verwijderen van een sleuf voor de implementatie##

In het blad voor een sleuf voor de implementatie, klikt u op **verwijderen** op de opdrachtbalk.  

![Verwijderen van een sleuf voor de implementatie][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>
##Azure PowerShell-cmdlets voor implementatie-sleuven

Azure PowerShell is een module waarmee u de cmdlets voor het beheren van Azure via Windows PowerShell, inclusief ondersteuning voor het beheren van web app implementatie sleuven in Azure App-Service.

- Zie voor meer informatie over het installeren en configureren van Azure PowerShell en Azure PowerShell verifiëren met uw abonnement Azure, [het installeren en configureren van Microsoft Azure PowerShell](../powershell-install-configure.md).  

----------

### <a name="create-web-app"></a>Web app maken

```
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [web app name] -Location [location] -AppServicePlan [app service plan name]
```

----------

### <a name="create-a-deployment-slot-for-a-web-app"></a>Maak een implementatie-sleuf voor een web app.

```
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [web app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

----------

### <a name="initiate-multi-phase-swap-and-apply-target-slot-configuration-to-source-slot"></a>Meerdere fasen swap initiëren en doel-sleuf configuratie toepassen op de bron-sleuf

```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

----------

### <a name="revert-the-first-phase-of-multi-phase-swap-and-restore-source-slot-configuration"></a>Herstellen van de eerste fase van het wisselbestand in meerdere fasen en bron sleuf-configuratie herstellen

```
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

----------

### <a name="swap-deployment-slots"></a>Implementatie van "slots" swap

```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

----------

### <a name="delete-deployment-slot"></a>Implementatie sleuf verwijderen

```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [web app name]/[slot name] -ApiVersion 2015-07-01
```

----------

<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>
##Azure opdrachtregelinterface (CLI Azure) opdrachten voor implementatie-sleuven

De CLI Azure biedt cross-platform-opdrachten voor het werken met Azure, inclusief ondersteuning voor de implementatie van "slots" Web App beheren.

- Zie voor instructies voor het installeren en configureren van de CLI Azure, waaronder informatie over Azure CLI aansluiten op uw abonnement Azure, [installeren en configureren van de CLI Azure](../xplat-cli-install.md).

-  Als u de beschikbare opdrachten voor Azure App Service in de CLI Azure, call `azure site -h`.

----------
### <a name="azure-site-list"></a>Azure sitelijst
Bel voor informatie over het web apps van het huidige abonnement, **azure, sitelijst**, zoals in het volgende voorbeeld.

`azure site list webappslotstest`

----------
### <a name="azure-site-create"></a>Azure-site maken
Als u een sleuf voor de implementatie, call **azure site maken** en geeft u de naam van een bestaand web app en de naam van de sleuf te maken, zoals in het volgende voorbeeld.

`azure site create webappslotstest --slot staging`

Als u wilt dat het besturingselement voor de nieuwe slot, gebruikt u de optie **--git** , zoals in het volgende voorbeeld.

`azure site create --git webappslotstest --slot staging`

----------
### <a name="azure-site-swap"></a>Azure site swap
Als u de bijgewerkte implementatie sleuf van de productie-app, gebruikt u de opdracht **omwisselen azure site** een swap-bewerking, zoals in het volgende voorbeeld uit te voeren. De app productie zal geen uitvaltijd ondervinden, noch zullen ondergaan een koude start.

`azure site swap webappslotstest`

----------
### <a name="azure-site-delete"></a>Azure-site verwijderen
Als u wilt verwijderen een sleuf implementatie die niet langer nodig is, gebruik van de opdracht **azure-site verwijderen** , zoals in het volgende voorbeeld.

`azure site delete webappslotstest --slot staging`

----------

>[AZURE.NOTE] Als u wilt aan de slag met Azure App-Service voordat u aanmelden voor een account met Azure, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een starter tijdelijk web app in de App-Service maken kunt. Geen creditcard vereist; geen verplichtingen.

## <a name="next-steps"></a>Volgende stappen ##
[Azure App Service Web App – webpagina toegang blokkeren om "slots" niet-productie-implementatie](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)

[Gratis proefversie van Microsoft Azure](/pricing/free-trial/)

## <a name="whats-changed"></a>Wat er veranderd
* Zie voor een handleiding voor het wijzigen van Websites met App-Service: [Azure App Service en de Impact op de bestaande Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[QGAddNewDeploymentSlot]:  ./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png
[AddNewDeploymentSlotDialog]: ./media/web-sites-staged-publishing/AddNewDeploymentSlotDialog.png
[ConfigurationSource1]: ./media/web-sites-staged-publishing/ConfigurationSource1.png
[MultipleConfigurationSources]: ./media/web-sites-staged-publishing/MultipleConfigurationSources.png
[SiteListWithStagedSite]: ./media/web-sites-staged-publishing/SiteListWithStagedSite.png
[StagingTitle]: ./media/web-sites-staged-publishing/StagingTitle.png
[SwapButtonBar]: ./media/web-sites-staged-publishing/SwapButtonBar.png
[SwapConfirmationDialog]:  ./media/web-sites-staged-publishing/SwapConfirmationDialog.png
[DeleteStagingSiteButton]: ./media/web-sites-staged-publishing/DeleteStagingSiteButton.png
[SwapDeploymentsDialog]: ./media/web-sites-staged-publishing/SwapDeploymentsDialog.png
[Autoswap1]: ./media/web-sites-staged-publishing/AutoSwap01.png
[Autoswap2]: ./media/web-sites-staged-publishing/AutoSwap02.png
[SlotSettings]: ./media/web-sites-staged-publishing/SlotSetting.png
 
