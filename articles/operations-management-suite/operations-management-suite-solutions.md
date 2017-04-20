<properties
   pageTitle="Oplossingen in Operations Management Suite (OMS) | Microsoft Azure"
   description="De functionaliteit van Operations Management Suite (OMS) uitbreiden oplossingen door middel van verpakte beheerscenario die klanten aan hun OMS-werkruimte kunnen toevoegen.  Dit artikel bevat informatie over hoe aangepaste oplossingen die zijn gemaakt door klanten en partners."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="bwren" />

# <a name="management-solutions-in-operations-management-suite-oms-preview"></a>Oplossingen voor werkstroombeheer in Operations Management Suite (OMS) (voorbeeld)

>[AZURE.NOTE]Dit is voorlopige documentatie voor oplossingen in OMS die momenteel in de voorvertoning.    

De functionaliteit van Operations Management Suite (OMS) uitbreiden oplossingen door middel van verpakte beheerscenario die klanten aan hun omgeving toevoegen kunnen.  Naast [oplossingen geleverd door Microsoft](../log-analytics/log-analytics-add-solutions.md), partners en klanten oplossingen voor beheer worden gebruikt in hun eigen omgeving kunnen maken of beschikbaar gesteld aan klanten door de Gemeenschap.

## <a name="finding-and-installing-management-solutions"></a>Zoeken en installeren van oplossingen
Er zijn meerdere methoden voor het zoeken en installeren van oplossingen zoals beschreven in de volgende secties.

### <a name="azure-marketplace"></a>Azure Marketplace
Oplossingen geleverd door Microsoft en vertrouwde partners uit de markt Azure in Azure portal kunnen worden geïnstalleerd.

1. Log in op de Azure portal.
2. Selecteer in het linkerdeelvenster **meer services**.
3. Ga naar **oplossingen** of *oplossingen* typt in het dialoogvenster **Filter** .
4. Klik op **+ toevoegen** .
5. Zoeken naar oplossingen die u door te bladeren interesseert, klikt u op de knop **Filter** of typen in het vak **Zoeken Everthing** .
6. Klik op een item marketplace om gedetailleerde informatie weer te geven.
4. Klik op **maken** om het venster **Oplossing toevoegen** te openen.
5. U wordt gevraagd om de vereiste informatie, zoals de [OMS werkruimte en account automatisering](#oms-workspace-and-automation-account) naast de waarden voor de parameters in de oplossing.
6. Klik op **maken** om de oplossing te installeren.

### <a name="oms-portal"></a>OMS-Portal
Oplossingen geleverd door Microsoft kunnen worden geïnstalleerd vanuit de galerie met oplossingen in de OMS-portal.

1. Log in op de OMS-portal.
2. Klik op de tegel **Galerie met oplossingen** .
2. Informatie over de verschillende oplossingen beschikbaar op de pagina Galerie met OMS oplossingen. Klik op de naam van de oplossing die u wilt toevoegen aan OMS.
3. Gedetailleerde informatie over de oplossing is weergegeven op de pagina voor de oplossing die u hebt gekozen. Klik op **toevoegen**.
4. Een nieuwe tegel voor de oplossing die u hebt toegevoegd, wordt weergegeven in het overzicht pagina OMS en u kunt gaan gebruiken nadat de OMS-service uw gegevens verwerkt.

### <a name="azure-quickstart-templates"></a>Azure Quickstart sjablonen
Leden van de Gemeenschap kunnen oplossingen voor Azure Quickstart sjablonen indienen.  U kunt deze sjablonen voor latere installatie downloaden of onderzoeken voor meer informatie over het [maken van uw eigen oplossingen](#creating-a-solution).

1. Volg de procedure in de [werkruimte OMS en automatisering account](#oms-workspace-and-automation-account) aan een werkruimte en account koppelen.
2. Ga naar [Quickstart Azure-sjablonen](https://azure.microsoft.com/documentation/templates/).  
3. Zoeken naar een oplossing waarin u geïnteresseerd bent.
4. Selecteer de oplossing van de resultaten om de details te bekijken.
5. Klik op **Deploy om Azure** .
6. U wordt gevraagd informatie te verstrekken zoals de resourcegroep en locatie naast de waarden voor de parameters in de oplossing.
7. Klik op **Inkoop** om te installeren van de oplossing.

### <a name="deploy-azure-resource-manager-template"></a>Azure Resource Manager sjabloon distribueren
Oplossingen krijgt u van de Gemeenschap of die u [zelf maakt](#creating-a-solution) worden geïmplementeerd als een sjabloon Resource Manager en kunt u een van de standaardmethoden voor het [implementeren van een sjabloon](../resource-group-template-deploy-portal.md).  Opmerking voor de installatie van de oplossing, moet u maken en koppelen de [OMS werkruimte en account automatisering](#oms-workspace-and-automation-account).

## <a name="oms-workspace-and-automation-account"></a>OMS-werkruimte en account automatisering
De meeste oplossingen vereisen een [werkruimte OMS](../log-analytics/log-analytics-manage-access.md) weergaven bevatten en een [account voor automatisering](../automation/automation-security-overview.md#automation-account-overview) runbooks en verwante bronnen bevat. De werkruimte en account moeten voldoen aan de volgende eisen voldoen.

- Een oplossing kan alleen een OMS werkruimte en één account voor automatisering gebruiken.  
- De OMS werkruimte en automatisering-account die wordt gebruikt door een oplossing moeten worden gekoppeld aan elkaar. Een werkruimte OMS kan alleen worden gekoppeld aan één account voor automatisering en een account voor automatisering kan alleen worden gekoppeld aan één OMS-werkruimte.
- Om te worden gekoppeld, moeten de OMS werkruimte en account automatisering in dezelfde resourcegroep en regio.  De uitzondering hierop is een werkruimte OMS in de regio Oost-VS en en automatisering in Oost-VS 2.

### <a name="creating-a-link-between-an-oms-workspace-and-automation-account"></a>Een koppeling tussen een werkruimte OMS en automatisering-account maken
Hoe u de werkruimte OMS en automatisering rekening is afhankelijk van de installatiemethode voor uw oplossing.

- Tijdens de installatie van een Microsoft-oplossing via het Kantoorbeheersysteem portal is geïnstalleerd in de huidige werkruimte met OMS en geen automatisering-rekening is vereist.

- Wanneer u een oplossing via de markt Azure installeert, wordt gevraagd of u voor een werkruimte OMS en automatisering account en de koppeling tussen deze voor u gemaakt.  

- Voor oplossingen buiten de markt Azure, moet u de OMS werkruimte en account automatisering koppelen voordat de installatie van de oplossing.  U kunt dit doen door een oplossing in de markt Azure selecteren en klikken op de OMS werkruimte en account automatisering.  U hebt geen daadwerkelijk de oplossing worden geïnstalleerd omdat de koppeling wordt gemaakt zodra de OMS werkruimte en account automatisering zijn geselecteerd.  Als de koppeling is gemaakt, kunt u voor elke oplossing die OMS werkruimte en account automatisering gebruiken. 

### <a name="verifying-the-link-between-an-oms-workspace-and-automation-account"></a>De koppeling tussen een werkruimte OMS en automatisering-account controleren
U kunt controleren of de koppeling tussen een werkruimte OMS en een automatisering rekening met de volgende procedure.

1. Selecteer de account automatisering in Azure portal.
2. Ga naar de onderkant van het venster **Instellingen** .
3. Als er een sectie met de naam **OMS-bronnen** in het deelvenster **Instellingen** , is deze account gekoppeld aan een werkruimte OMS.
4. Selecteer de **werkruimte** tot de details van de werkruimte OMS is gekoppeld aan dit account automatisering.


## <a name="listing-management-solutions"></a>Oplossingen voor aanbieding
Gebruik de volgende procedure om de oplossingen in de werkruimten die zijn gekoppeld aan uw abonnement Azure weergeven.

1. Log in op de Azure portal.
2. Selecteer in het linkerdeelvenster **meer services**.
3. Ga naar **oplossingen** of *oplossingen* typt in het dialoogvenster **Filter** .
4. Oplossingen is geïnstalleerd in alle van uw werkruimten worden weergegeven.

Houd er rekening mee dat u alleen de Microsoft oplossingen is geïnstalleerd in de huidige werkruimte met de OMS portal kunt weergeven.

## <a name="removing-a-management-solution"></a>Een oplossing verwijderen
Als een oplossing wordt verwijderd, worden alle bronnen in de oplossing ook verwijderd.  

1. Zoek de oplossing in de Azure portal met de procedure in het [aanbieden van oplossingen](#listing-solutions).
2. Selecteer de oplossing die u wilt verwijderen.
3. Klik op de knop **verwijderen** .

## <a name="creating-a-management-solution"></a>Een management-oplossing te maken
Volledige instructies over het maken van oplossingen zijn beschikbaar op [oplossingen maken in het Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md). 


## <a name="next-steps"></a>Volgende stappen

- Zoeken [Azure Quickstart sjablonen](https://azure.microsoft.com/documentation/templates) voor voorbeelden van verschillende sjablonen voor Resource Manager.
- Maak uw eigen [oplossingen](operations-management-suite-solutions-creating.md).
 