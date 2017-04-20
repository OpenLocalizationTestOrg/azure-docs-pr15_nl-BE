<properties
    pageTitle="Aan de slag met logboek Analytics | Microsoft Azure"
    description="Kunt u actief met Analytics logboek in Microsoft Operations Management Suite (OMS) in minuten."
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
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="get-started-with-log-analytics"></a>Aan de slag met Analytics logboek

Kunt u actief met Analytics logboek in Microsoft Operations Management Suite (OMS) in minuten. Bij het kiezen van het maken van een werkruimte OMS die vergelijkbaar is met een account hebt u twee mogelijkheden:

- Website Microsoft Operations Management Suite
- Abonnement op Microsoft Azure

U kunt een gratis OMS-werkruimte met behulp van de OMS-website maken. Of u kunt een abonnement op Microsoft Azure een OMS-werkruimte maken. Beide werkruimten zijn functioneel equivalent, met dien verstande dat een vrije OMS-werkruimte alleen 500 MB aan gegevens dagelijks naar de OMS-service verzenden kan. Als u een abonnement op Azure, kun je abonnement toegang tot andere Azure diensten. Ongeacht de methode die u gebruikt om de werkruimte te maken, maakt u de werkruimte met een Microsoft-account of een organisatie-account.

Hier is een blik op het proces:

![Onboarding diagram](./media/log-analytics-get-started/oms-onboard-diagram.png)

## <a name="log-analytics-prerequisites-and-deployment-considerations"></a>Logboek Analytics vereisten en overwegingen voor de implementatie

- U moet een betaalde licentie van Microsoft Azure volledig logboek Analytics gebruiken. Als u niet een Azure-abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) waarmee u toegang tot alle Azure service maken. Of u kunt een gratis OMS-account maken op de website van [Operations Management Suite](http://microsoft.com/oms) en **Probeer gratis**.
- Een werkruimte OMS
- Elke Windows-computer die u wilt verzamelen gegevens van Windows Server 2008 SP1 moet uitvoeren of hoger
- Webadressen [firewall](log-analytics-proxy-firewall.md) toegang tot het Kantoorbeheersysteem van service
- Een [Doorstuurserver voor OMS logboek Analytics](https://blogs.technet.microsoft.com/msoms/2016/03/17/oms-log-analytics-forwarder) (Gateway)-server verkeer moet worden doorgestuurd van servers met OMS, als toegang tot het Internet niet beschikbaar op computers is
- Als u Operations Manager, Analytics logboek ondersteunt Operations Manager 2012 SP1 UR6 en boven en Operations Manager 2012 R2 UR2 en hoger. Proxy-ondersteuning in een Operations Manager 2012 SP1 UR7 en Operations Manager 2012 R2 UR3 toegevoegd. Bepalen hoe deze worden geïntegreerd met OMS.
- Bepalen of uw computers direct toegang tot het Internet hebt. Als dat niet het geval is, moeten ze een gateway-server toegang tot de service van OMS websites. Alle toegang is via HTTPS.
- Bepalen welke technologieën en servers gegevens naar OMS verzenden. Bijvoorbeeld domeincontroller, SQL Server, enzovoort.
- Andere gebruikers machtigen in OMS en Azure.
- Als u zich zorgen over het gebruik van gegevens maakt, afzonderlijk elke oplossing implementeren en testen van de invloed op de prestaties voor het toevoegen van extra oplossingen.
- Bekijk uw gebruik van de gegevens en de prestaties als u oplossingen en functies aan logboek Analytics toevoegen. Dit geldt ook voor gebeurtenis-collectie, verzamelen, verzameling van prestatiegegevens, enz. Het is beter beginnen met minimale gegevens gebruik-collectie of invloed op de prestaties is geconstateerd.
- Controleren of Windows-agenten niet ook worden beheerd met Operations Manager anders dubbele gegevens leidt. Dit geldt ook voor Azure-gebaseerd-agenten die Azure Diagnostics ingeschakeld zijn.
- Controleer of de agent goed functioneert na de installatie van agents. Als niet, Controleer of die Cryptography API: volgende generatie (CNG) sleutel isolatie niet is uitgeschakeld met behulp van Groepsbeleid.
- Sommige oplossingen logboek Analytics gelden aanvullende systeemvereisten



## <a name="sign-up-in-3-steps-using-the-operations-management-suite"></a>In 3 stappen het Operations Management Suite met aanmelden

1. Ga naar de website van [Operations Management Suite](http://microsoft.com/oms) en klik op **Probeer het gratis**. Meld u aan met uw Microsoft-account zoals Outlook.com, of met een organisatie account die door uw bedrijf of onderwijsinstelling gebruiken met Office 365 of andere Microsoft-services.
2. Geef een unieke werkruimte naam. Een werkruimte is een logische container waarin uw gegevens is opgeslagen. Het biedt een manier om partitiegegevens tussen verschillende teams in uw organisatie, als de gegevens exclusief voor de werkruimte zijn. Geef een e-mailadres en de regio waar u uw gegevens hebt opgeslagen.  
    ![werkruimte maken en koppelen van abonnement](./media/log-analytics-get-started/oms-onboard-create-workspace-link01.png)
3. Vervolgens kunt u een nieuw abonnement op Azure of een koppeling naar een bestaande Azure-abonnement. Als u doorgaan met de gratis proefversie wilt, klikt u op **Nu niet**.  
  ![werkruimte maken en koppelen van abonnement](./media/log-analytics-get-started/oms-onboard-create-workspace-link02.png)

U bent klaar om te beginnen met de portal Operations Management Suite.

U kunt meer informatie over het instellen van uw werkruimte en koppelen Azure bestaande accounts voor werkruimten die zijn gemaakt met het Operations Management Suite op [de toegang beheren tot Analytics logboek](log-analytics-manage-access.md).

## <a name="sign-up-quickly-using-microsoft-azure"></a>Meld u snel met Microsoft Azure

1. Gaat u naar de [portal Azure](https://portal.azure.com) en aanmelden, bladeren in de lijst van services en selecteer **Logboek Analytics (OMS)**.  
    ![Azure portal](./media/log-analytics-get-started/oms-onboard-azure-portal.png)
2. Klik op **toevoegen**en selecteer vervolgens opties voor de volgende items:
    - De naam van **De werkruimte OMS**
    - **Abonnement** - als er meerdere abonnementen, kiezen die u wilt koppelen aan de nieuwe werkruimte.
    - **Resourcegroep**
    - **Locatie**
    - **Prijzen laag**  
        ![snel tabellen maken](./media/log-analytics-get-started/oms-onboard-quick-create.png)
3. Klik op **maken** en ziet u de details van de werkruimte in de portal Azure.       
    ![details van de werkruimte](./media/log-analytics-get-started/oms-onboard-workspace-details.png)         
4. Klik op de koppeling van **OMS Portal** de Operations Management Suite-website met uw nieuwe werkruimte geopend.

U bent klaar om te starten met behulp van de portal Operations Management Suite.

U kunt meer informatie over het instellen van uw werkruimte en bestaande werkruimten die u hebt gemaakt met het Operations Management Suite naar Azure abonnementen bij [de toegang beheren tot logboek Analytics](log-analytics-manage-access.md)koppelen.

## <a name="get-started-with-the-operations-management-suite-portal"></a>Aan de slag met de portal Operations Management Suite
Om te kiezen oplossingen en verbinding maken met de servers die u wilt beheren, klikt u op de tegel **Instellingen** en volg de stappen in deze sectie.  

![aan de slag](./media/log-analytics-get-started/oms-onboard-get-started.png)  

1. **Toevoegen-oplossingen** - uw geïnstalleerde oplossingen bekijken.  
    ![oplossingen](./media/log-analytics-get-started/oms-onboard-solutions.png)  
    Klik op **Ga naar de galerie** wilt toevoegen meer oplossingen.  
    ![oplossingen](./media/log-analytics-get-started/oms-onboard-solutions02.png)  
    Selecteer een oplossing en klik vervolgens op **toevoegen**.
2. **Verbinding maken met een bron** - kiezen hoe u verbinding maakt met uw serveromgeving om gegevens te verzamelen:
    - Verbinding maken met een Windows-Server of client rechtstreeks door een agent installeren.
    - Linux servers verbinden met de Agent OMS voor Linux.
    - Gebruik een account Azure opslag is geconfigureerd met de Windows- of Linux-Azure diagnostische VM-extensie.
    - System Center Operations Manager om uw beheer groepen of de volledige implementatie van de Operations Manager toe te gebruiken.
    - Telemetrie Windows Upgrade Analytics gebruiken inschakelen.
        ![verbonden bronnen](./media/log-analytics-get-started/oms-onboard-data-sources.png)    

3. **Gegevens verzamelen** Ten minste één gegevensbron om in te vullen gegevens naar uw werkruimte configureren. Wanneer u klaar bent, klikt u op **Opslaan**.    

    ![gegevens verzamelen](./media/log-analytics-get-started/oms-onboard-logs.png)    


## <a name="optionally-connect-servers-directly-to-the-operations-management-suite-by-installing-an-agent"></a>U kunt desgewenst servers rechtstreeks aansluiten op het Operations Management Suite door een agent installeren

In het volgende voorbeeld ziet u hoe u een Windows-agent installeren.

1. Klik op de tegel **Instellingen** , klikt u op het tabblad **Bronnen verbonden** , klikt u op een tabblad voor het type gegevensbron dat u wilt toevoegen en een download van een agent of Klik hier voor informatie over het inschakelen van een agent. Klik bijvoorbeeld op **Windows-Agent downloaden (64-bits)**. Als u Windows wilt toevoegen, kunt u alleen de agent in Windows Server 2008 SP 1 of hoger of Windows 7 SP1 of hoger installeren.
2. De agent op een of meer servers installeren. Kunt u één voor één agenten of met een meer geautomatiseerde methode met een [aangepast script](log-analytics-windows-agents.md)of als u een bestaande distributie softwareoplossing die u wellicht kunt gebruiken.
3. Nadat u akkoord met de gebruiksrechtovereenkomst gaat en u de installatiemap kiest, selecteert u **verbinding maken met de agent naar Azure logboek Analytics (OMS)**.   
    ![Agent setup](./media/log-analytics-get-started/oms-onboard-agent.png)

4. Op de volgende pagina, wordt u gevraagd om uw werkruimte-ID en sleutel werkruimte. Uw werkruimte-ID en sleutel worden weergegeven op het scherm waar u de agent-bestand hebt gedownload.  
    ![DRA-sleutels](./media/log-analytics-get-started/oms-onboard-mma-keys.png)  

    ![servers toevoegen](./media/log-analytics-get-started/oms-onboard-key.png)
5. Tijdens de installatie kunt u **Geavanceerd** desgewenst uw proxy-server instellen en verificatie-informatie bevatten. Klik op de knop **volgende** om terug te keren naar het scherm van de werkruimte-informatie.
6. Klik op **volgende** om te controleren uw werkruimte-ID en sleutel. Als er fouten zijn gevonden, kunt u **terug** te verbeteren. Als uw werkruimte-ID en sleutel worden gevalideerd, klikt u op **installeren** om de installatie van de agent.
7. In het Configuratiescherm, klikt u op Microsoft Monitoring Agent > tabblad Azure logboek Analytics (OMS). Een groen vinkje wordt weergegeven wanneer de agents met de service Operations Management Suite communiceren. Aanvankelijk, duurt dit ongeveer 5-10 minuten.

>[AZURE.NOTE] De capaciteit beheer- en beoordeling oplossingen worden momenteel niet ondersteund door servers direct verbonden met het Operations Management Suite.


Ook kunt u de agent naar System Center Operations Manager 2012 SP1 en hoger. Hiertoe selecteert u **verbinding maken met de agent naar System Center Operations Manager**. Wanneer u die optie, u gegevens naar de service verzenden zonder extra hardware of de belasting op het beheer van groepen.

U kunt meer lezen over het aansluiten van de agenten op het Operations Management Suite op [logboek Analytics verbinding maken met Windows-computers](log-analytics-windows-agents.md).

## <a name="optionally-connect-servers-using-system-center-operations-manager"></a>Indien gewenst, maak verbinding tussen servers met System Center Operations Manager

1. Selecteer in de console Operations Manager **beheer**.
2. Vouw het knooppunt **Operationeel inzichten** en **Operationele inzichten verbinding**selecteren.

  >[AZURE.NOTE] Afhankelijk van welke Update Rollup van SCOM u gebruikt, ziet u een knooppunt voor *System Center Advisor*, *Operationele inzichten*of *Operations Management Suite*.

3. Klik op de koppeling **naar operationele inzichten registreren** rechts bovenaan en volg de instructies.
4. Nadat de registratiewizard is voltooid, klikt u op de koppeling **een computergroep toevoegen** .
5. In het dialoogvenster **Computer zoeken** kunt u zoeken naar computers of groepen die zijn gecontroleerd door Operations Manager. Computers of groepen selecteren op on-board ze naar logboek Analytics, klikt u op **toevoegen**en klik vervolgens op **OK**. U kunt controleren dat de OMS-service is ontvangen van gegevens via de tegel voor **Gebruik** in de portal Operations Management Suite. Gegevens moeten worden weergegeven in 5-10 minuten.

U kunt meer lezen over het aansluiten van Operations Manager op het Operations Management Suite op [Operations Manager verbinding maken met Analytics logboek](log-analytics-om-agents.md).

## <a name="optionally-analyze-data-from-cloud-services-in-microsoft-azure"></a>Gegevens van services in Microsoft Azure cloud eventueel analyseren

Het Operations Management Suite, kunt u snel zoeken gebeurtenis en de IIS-logboeken voor cloud services en virtuele machines met diagnostische gegevens voor Azure Cloud Services inschakelen. U kunt ook aanvullende inzichten voor uw Azure virtuele machines ontvangen door het installeren van de Agent Microsoft controleren. Meer informatie over het configureren van uw Azure-omgeving voor het gebruik van het Operations Management Suite op [logboek Analytics verbinding Azure opslag](log-analytics-azure-storage.md).


## <a name="next-steps"></a>Volgende stappen

- [Oplossingen voor logboek Analytics toevoegen uit de galerie met oplossingen](log-analytics-add-solutions.md) voor functionaliteit toevoegen en gegevens te verzamelen.
- Vertrouwd raken met [zoekopdrachten log](log-analytics-log-searches.md) gedetailleerde informatie verzameld door oplossingen te bekijken.
- [Dashboards](log-analytics-dashboards.md) gebruiken om te slaan en uw eigen aangepaste zoekacties.
