<properties
 pageTitle="Sjablonen van Logic App | Microsoft Azure"
 description="Informatie over het gebruik van vooraf gemaakte sjablonen van Logic app waarmee u aan de slag"
 authors="kevinlam1"
 manager="dwrede"
 editor=""
 services="app-service\logic"
 documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="klam"/>

# <a name="logic-app-templates"></a>Sjablonen van Logic-App

## <a name="what-are-logic-app-templates"></a>Wat zijn de sjablonen van logic app

Een logische app-sjabloon is een vooraf gemaakte logica app die u gebruiken kunt om snel uw eigen workflow te maken. 

Deze sjablonen zijn een goede manier om verschillende patronen die kunnen worden gebouwd met logica apps te ontdekken. U kunt deze sjablonen als gebruiken-is of aanpassen aan uw scenario.

## <a name="overview-of-available-templates"></a>Overzicht van de beschikbare sjablonen

Er zijn vele beschikbare sjablonen die momenteel in de logica app platform zijn gepubliceerd. Sommige categorieën voorbeeld, evenals het type connectors gebruikt, worden hieronder weergegeven.

### <a name="enterprise-cloud-templates"></a>Enterprise cloud-sjablonen
Sjablonen die de integratie van Dynamics CRM, televergaderingen, vak, Azure Blob en andere connectors voor de behoeften van uw onderneming wolk. Enkele voorbeelden van wat kan worden gedaan met deze sjablonen bevatten leads ordenen en back-ups van uw zakelijke gegevens.

### <a name="enterprise-integration-pack-templates"></a>Enterprise integration pack sjablonen
Configuraties van VETER (valideren, uitpakken, transformeren, verrijken, route) pijpleidingen, ontvangen een X12 EDI via AS2-document en het transformeren naar XML, als goed als X12 en AS2 bericht overslag.

### <a name="protocol-pattern-templates"></a>Protocol patroon sjablonen
Deze sjablonen bestaan uit logica apps met patronen zoals aanvraag/antwoord-protocol via HTTP, alsmede de integratie van verschillende FTP- en SFTP. Gebruik deze als ze bestaan, of als basis voor het maken van meer complexe patronen in het protocol.  

### <a name="personal-productivity-templates"></a>Sjablonen voor persoonlijke productiviteit
Patronen voor persoonlijke productiviteit verbeteren bevatten sjablonen die dagelijkse herinneringen instellen en automatiseren van langdurige taken naar een goedkeuringsstap één gebruiker zet belangrijke taken in takenlijsten.

### <a name="consumer-cloud-templates"></a>Sjablonen voor consumenten cloud
Eenvoudige sjablonen die integreren met sociale mediaservices als Twitter, e-mail, kan uiteindelijk versterken van sociale media marketing initiatieven. Deze omvatten ook sjablonen zoals bewolkt kopiëren, waarmee de productiviteit verhogen door tijd besteed aan het opslaan van oudsher terugkerende taken. 

## <a name="how-to-create-a-logic-app-using-a-template"></a>Het maken van een logica app met behulp van een sjabloon 

Ga om te beginnen met een sjabloon logica app in de app-ontwerper logica. Als u de ontwerpfunctie door het openen van een bestaande logica app invoert, wordt de logica app automatisch geladen in designer-weergave. Als u een nieuwe logica app maakt, ziet u het onderstaande scherm.  
 ![](../../includes/media/app-service-logic-templates/template7.png)  

Vanuit dit scherm, kunt u kiezen om te beginnen met een lege logica app of een vooraf gemaakte sjabloon. Als u een van de sjablonen selecteert, worden aanvullende informatie voorzien. In dit voorbeeld gebruiken we de sjabloon *wanneer een nieuw bestand wordt gemaakt in Dropbox, kopiëren naar OneDrive* .  
 ![](../../includes/media/app-service-logic-templates/template2.png)  

Als u de sjabloon wilt gebruiken, selecteert u de knop *deze sjabloon gebruiken* . Wordt u gevraagd om aan te melden bij uw accounts op basis op de verbindingslijnen die gebruikmaakt van de sjabloon. Of als u eerder een verbinding met deze aansluitingen hebt ingesteld, kunt u doorgaan zoals hieronder wordt weergegeven.  
 ![](../../includes/media/app-service-logic-templates/template3.png)  

Nadat de verbinding tot stand te brengen en *verder*selecteren, opent de app logica in designer-weergave.  
 ![](../../includes/media/app-service-logic-templates/template4.png)  

In het bovenstaande voorbeeld, zoals het geval met een groot aantal sjablonen, kunnen bepaalde verplichte eigenschapsvelden worden ingevuld binnen de connectors; echter sommige nog steeds mogelijk een waarde kan pas goed de app logica te implementeren. Als u probeert te implementeren zonder in te voeren op bepaalde velden ontbreken, u ontvangt een bericht met een foutbericht.

Als u terugkeren naar de sjabloon viewer wilt, selecteert u de knop *sjablonen* in de bovenste navigatiebalk. Door over te schakelen naar de kijker sjabloon, verliest u alle niet-opgeslagen voortgang. Voordat u overschakelt naar sjabloon viewer, wordt er een waarschuwingsbericht met de mededeling van deze.  
 ![](../../includes/media/app-service-logic-templates/template5.png)  

## <a name="how-to-deploy-a-logic-app-created-from-a-template"></a>Het implementeren van een logica app gemaakt op basis van een sjabloon

Zodra u hebt geladen van de sjabloon en de gewenste wijzigingen hebt aangebracht, schakelt u het opslaan knop in de linkerbovenhoek. Opgeslagen en uw app logica wordt gepubliceerd.  
 ![](../../includes/media/app-service-logic-templates/template6.png)  

Als u meer informatie wilt over bewerken hoe meer stappen in een bestaande logica app sjabloon toevoegen of in het algemeen meer bij het [maken van een app logica](app-service-logic-create-a-logic-app.md)