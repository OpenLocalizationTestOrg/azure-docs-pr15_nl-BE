<properties
pageTitle="Gebruik van gegevens uit een SQL Server-database op de ruimten in Machine Learning | Azure"
description="Gegevens uit een SQL Server-database op gebouwen gebruiken voor het uitvoeren van geavanceerde analytics met Azure Machine Learning."
services="machine-learning"
documentationCenter=""
authors="garyericson"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="machine-learning"
ms.workload="data-services"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="09/16/2016"
ms.author="garye;krishnan"/>

# <a name="perform-advanced-analytics-with-azure-machine-learning-using-data-from-an-on-premises-sql-server-database"></a>Geavanceerde analytics met Azure Machine Learning met gegevens uit een SQL Server-database op de bedrijfsruimten uitvoeren


Vaak ondernemingen die met de gegevens voor de lokalen werken wil geprofiteerd van de schaal en de flexibiliteit van de wolk voor hun werkbelasting leren machine. Maar ze willen niet hun huidige bedrijfsprocessen en werkstromen te verstoren doordat hun gegevens op het bedrijf naar de cloud. Azure Machine Learning ondersteunt nu uw gegevens lezen uit een SQL Server-database op gebouwen en vervolgens training en scoren van een model met deze gegevens. U hoeft niet langer handmatig kopiëren en synchroniseren van gegevens tussen de wolk en de server op locatie. In plaats daarvan vindt de module **Gegevens importeren** in Azure Machine Learning Studio nu rechtstreeks vanuit uw SQL Server-database op de locatie voor uw opleiding en scoren van taken. 

Dit artikel bevat een overzicht van de ingress op-premises SQL server-gegevens in Azure Machine Learning. Hierbij wordt ervan uitgegaan dat u bekend met Azure Machine Learning concepten zoals werkruimten, modules, datasets, experimenten, *enz bent*...

> [AZURE.NOTE] Deze functie is niet beschikbaar voor gratis werkruimten. Zie voor meer informatie over de prijs van Machine Learning en lagen, [Azure Machine Learning prijzen](https://azure.microsoft.com/pricing/details/machine-learning/).

<!-- --> 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="install-the-microsoft-data-management-gateway"></a>De Microsoft Data Management-Gateway installeren

U moet downloaden en installeren van de Microsoft Data Management Gateway voor toegang tot een SQL Server-database op de ruimten in Azure Machine Learning. Wanneer u de gateway verbinding in Machine Learning Studio configureert hebt u de mogelijkheid om te downloaden en installeren van de gateway met behulp van het dialoogvenster **downloaden en registreren gegevensgateway** , die hieronder worden beschreven.

U kunt de Data Management Gateway vooraf installeren door het downloaden en uitvoeren van het MSI-installatiepakket van het [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717). Kies de meest recente versie, 32-bits of 64-bits voor uw computer selecteren. Het MSI-bestand kan ook worden gebruikt om te upgraden van een bestaande Data Management Gateway naar de nieuwste versie met alle instellingen blijven behouden.

De gateway heeft de volgende vereisten:

- De ondersteunde Windows-besturingssystemen zijn Windows 7, Windows 8 / 8.1 Windows 10, Windows Server 2008 R2, Windows Server 2012 en Windows Server 2012 R2.
- De aanbevolen configuratie voor de gateway-machine is ten minste 2 GHz, 4 cores, 8 GB RAM en 80 GB schijf.
- Als de hostcomputer in de slaapstand, de gateway niet mogelijk te reageren op verzoeken van de gegevens. Daarom een energieschema geschikt configureren op de computer voordat u de gateway. De installatie van gateway, wordt er een bericht weergegeven als de computer is geconfigureerd voor de slaapstand.
- Omdat kopie activiteit op een bepaalde frequentie plaatsvindt, volgt het gebruik van bronnen (CPU, geheugen) op de computer ook hetzelfde patroon met piek en niet-actieve tijd. Brongebruik is ook sterk afhankelijk van de hoeveelheid gegevens die wordt verplaatst. Wanneer meerdere kopie taken uitgevoerd worden zult u omhoog gaan tijdens piekuren Resourcegebruik ziet. De minimale configuratie bovenstaande technisch voldoende is, kunt u een configuratie met meer bronnen dan de minimale configuratie is afhankelijk van uw specifieke belasting voor verplaatsing van gegevens.

Overweeg het volgende bij het instellen en gebruiken van een Data Management Gateway:

- U kunt slechts één exemplaar van Data Management Gateway installeren op één computer.
- U kunt één gateway voor meerdere gegevensbronnen die op gebouwen.
- U kunt meerdere gateways op verschillende computers verbinden met dezelfde gegevensbron op gebouwen.
- U configureren een gateway voor slechts één werkruimte op een tijdstip. Gateways kunnen niet worden gedeeld met werkruimten op dit moment.
- U kunt meerdere gateways voor een enkele werkruimte configureren. U wilt bijvoorbeeld een gateway die verbonden met uw gegevensbronnen testen tijdens de ontwikkeling en productie-gateway gebruiken wanneer u nu mogelijk maken.
- De gateway niet hoeft te worden op dezelfde computer als de gegevensbron, maar blijven dichter bij de gegevensbron minder tijd voor de gateway verbinding maken met de gegevensbron. Het is raadzaam de gateway te installeren op een computer die verschilt van die host is van de gegevensbron in de ruimten zodat de gateway en de gegevensbron niet concurreren voor resources.
- Als u al een gateway is geïnstalleerd op uw computer voor Power BI of Azure Data Factory-scenario's, een aparte gateway installeren voor Azure Machine Learning op een andere computer. 

    > [AZURE.NOTE] Data Management Gateway en Power BI-Gateway kan niet op dezelfde computer worden uitgevoerd.

- U moet de Data Management Gateway gebruiken voor Azure Machine Learning, zelfs als u ExpressRoute Azure voor andere gegevens. De gegevensbron moet worden beschouwd als een gegevensbron op gebouwen (die zich achter een firewall) zelfs wanneer u ExpressRoute gebruiken, en de Data Management Gateway verbinding tussen Machine Learning en de gegevensbron tot stand te gebruiken. 

U vindt gedetailleerde informatie over de vereisten voor de installatie, installatiestappen en tips voor probleemoplossing in het artikel, het [verplaatsen van gegevens tussen de bronnen op gebouwen en wolken met Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway), beginnend met de sectie [Overwegingen bij het gebruik van Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway).

## <a name="span-idusing-the-data-gateway-step-by-step-walk-classanchorspan-idtoc450838866-classanchorspanspaningress-data-from-your-on-premises-sql-server-database-into-azure-machine-learning"></a><span id="using-the-data-gateway-step-by-step-walk" class="anchor"><span id="_Toc450838866" class="anchor"></span></span>Gegevens van uw SQL Server-database op de ruimten in Azure Machine Learning ingress


In dit scenario wordt u Data Management Gateway instellen in een werkruimte Azure Machine Learning, configureert en leest u gegevens uit een SQL Server-database op gebouwen.

> [AZURE.TIP] Voordat u begint, pop-upblokkering voor de browser uitschakelen `studio.azureml.net`. Als u de browser Google Chrome gebruikt, download en installeer een van de verschillende plug-ins beschikbaar op Google Chrome WebStore [Klik eenmaal App extensie](https://chrome.google.com/webstore/search/clickonce?_category=extensions).

### <a name="step-1-create-a-gateway"></a>Stap 1: Een gateway maken

De eerste stap is om te maken en de gateway ingesteld voor toegang tot de SQL-database op de ruimten.

1. Aanmelden bij de [Azure Machine Learning Studio](https://studio.azureml.net/Home/) en selecteert u de werkruimte die u wilt bewerken.

2. Klik op het blad van de **Instellingen** aan de linkerkant en klik vervolgens op het tabblad **GEGEVENSGATEWAYS** boven.

3. Klik op **Nieuwe GEGEVENSGATEWAY** onderaan het scherm.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-button.png)

4. Voer de **Naam van de Gateway** en eventueel een **Beschrijving**toevoegen in het dialoogvenster **nieuwe data gateway** . Klik op de pijl in de rechterbenedenhoek om te gaan met de volgende stap van de configuratie.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-dialog-enter-name.png)

5. In het journaal en Download gateway dialoogvenster gegevens, de GATEWAY registratie sleutel naar het Klembord te kopiëren.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/download-and-register-data-gateway.png)

6. <span id="note-1" class="anchor"></span>Als u nog niet gedownload en geïnstalleerd het Microsoft Data Management Gateway, klikt u op **data management gateway downloaden**. Hiermee gaat u naar het Microsoft Download Center waar u kunt selecteren van de gateway-versie die u moet downloaden en installeren. In de secties van het begin van het [verplaatsen van gegevens tussen de bronnen op gebouwen en wolken met Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)-artikel vindt u gedetailleerde informatie over de vereisten voor de installatie, installatiestappen en tips voor probleemoplossing.

7. Nadat de gateway is geïnstalleerd, de Data Management Gateway Configuration Manager wordt geopend en het dialoogvenster **gateway registreren** wordt weergegeven. **Gateway registratie sleutel** die u hebt gekopieerd naar het Klembord te plakken en klikt u op **registreren**.

8. Als u al een gateway is geïnstalleerd, Data Management Gateway Configuration Manager uitvoeren, klikt u op **de sleutel wijzigen**  **Gateway registratie sleutel** die u hebt gekopieerd naar het Klembord te plakken en klik op **OK**.

9. Wanneer de installatie voltooid is, wordt het dialoogvenster **gateway registreren** voor Microsoft Data Management Gateway Configuration Manager weergegeven. De GATEWAY registratie sleutel die u hebt gekopieerd naar het Klembord boven plakken en klikt u op **registreren**.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-register-gateway.png)

10. De configuratie van de standaardgateway is voltooid wanneer de volgende waarden worden ingesteld op het tabblad **Start** in Microsoft Data Management Gateway Configuration Manager:

    - **Gateway-naam** en de **naam van de instantie** ingesteld op de naam van de gateway.

    - **Registratie** is ingesteld op **geregistreerd**.

    - **De status** is ingesteld op **gestart**.

    - De statusbalk onderaan wordt **verbonden met de Data Management Gateway Cloud Service** samen met een groen vinkje weergegeven.

     ![](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-registered.png)

     Azure Machine Learning Studio wordt ook bijgewerkt wanneer de registratie gelukt is.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\gateway-registered.png)

11. In het dialoogvenster **downloaden en registreren van, gegevensgateway** , klikt u op het vinkje om de installatie te voltooien. De pagina **-Instellingen** geeft de status van de gateway als de "On line". In het rechterdeelvenster vindt u de status en andere nuttige informatie.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\gateway-status.png)

12. In de switch van Microsoft Data Management Gateway Configuration Manager op het tabblad **certificaten** . Het certificaat dat u op dit tabblad wordt gebruikt om de referenties voor het gegevensarchief op de ruimten die u opgeeft in de portal voor coderen/decoderen. Dit is de standaardcertificaat gegenereerd. Microsoft raadt u aan uw eigen certificaat dat u back-up van uw certificaat management-systeem wijzigen. Klik op **wijzigen** om uw eigen certificaat gebruiken.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\data-gateway-configuration-manager-certificate.png)

13. (optioneel) Als u uitgebreide logboekregistratie om het oplossen van problemen met de gateway inschakelen, in de Microsoft Data Management Gateway Configuration Manager overschakelen naar het tabblad **Diagnostische gegevens** en schakel de optie voor **uitgebreide logboekregistratie inschakelen voor het oplossen van problemen** . Gegevens in het logboek vindt u in de Windows-Logboeken onder **Logboeken toepassingen en Services**  - &gt; **Data Management Gateway** knooppunt. U kunt ook het tabblad **Diagnostische gegevens** op test de verbinding met een gegevensbron op ruimten is via de gateway.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\data-gateway-configuration-manager-verbose-logging.png)

Dit is de gateway ingesteld proces in Azure Machine Learning voltooid.
U bent nu klaar voor gebruik van uw gegevens op gebouwen.

U kunt maken en instellen van meerdere gateways in Studio voor elke werkruimte. Bijvoorbeeld, wellicht een gateway die u wilt verbinden met uw gegevensbronnen testen tijdens de ontwikkeling en een andere gateway voor uw productie-gegevensbronnen. Azure Machine Learning biedt u de flexibiliteit voor het instellen van meerdere gateways, afhankelijk van uw bedrijfsomgeving. Momenteel kunt u een gateway tussen werkruimten niet delen en slechts één gateway kan worden geïnstalleerd op één computer. Zie [Overwegingen bij het gebruik van Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway) in het artikel, het [verplaatsen van gegevens tussen de bronnen op gebouwen en wolken met Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)voor meer overwegingen bij de installatie van de gateway.

### <a name="step-2-use-the-gateway-to-read-data-from-an-on-premises-data-source"></a>Stap 2: De gateway gebruiken om gegevens te lezen uit een gegevensbron in de lokalen

Nadat u de gateway hebt ingesteld, kunt u een module **Gegevens importeren** toevoegen aan een experiment dat de gegevens uit de SQL Server-database op gebouwen-ingangen.

1.  In Studio Machine Learning, selecteert u het tabblad **EXPERIMENTEN** op **+ Nieuw** in de linkerbenedenhoek en selecteer **Lege Experiment** (of Selecteer een van de verschillende monster experimenten beschikbaar).

2.  Zoeken en sleept u de module **Gegevens importeren** naar het canvas experiment.

3.  Klik hieronder op het canvas op **Opslaan als** . "Azure Machine Learning On-Premises SQL Server zelfstudie' voor de naam van het experiment, selecteert u de werkruimte, en klik op **OK** uit te schakelen.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\experiment-save-as.png)

4.  Klik op de module **Gegevens importeren** om deze te selecteren in het deelvenster **Eigenschappen** aan de rechterkant van het canvas en selecteer vervolgens 'On-Premises SQL Database' in de vervolgkeuzelijst voor de **gegevensbron** .

5.  Selecteer de **gegevensgateway** u geïnstalleerd en geregistreerd. U kunt een andere gateway instellen door te selecteren "(nieuwe gegevensgateway... toevoegen)".

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\import-data-select-on-premises-data-source.png)

6.  Voer in de SQL **server-databasenaam** en de **naam van de Database**en de SQL- **query van de Database** u wilt uitvoeren.

7.  Klik op **Enter waarden** onder de **gebruikersnaam en wachtwoord** en geef uw databasereferenties. U kunt Windows-verificatie of SQL Server-verificatie, afhankelijk van hoe uw op ruimten SQL-Server is geconfigureerd.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\database-credentials.png)
    
    Het bericht 'waarden vereist' wordt gewijzigd in "grenswaarden" met een groen vinkje. U hoeft alleen maar eenmaal voert u de referenties tenzij de database-informatie of het wachtwoord is gewijzigd. Azure Machine Learning maakt gebruik van het certificaat dat u hebt opgegeven tijdens de installatie van de gateway voor het coderen van de referenties in de cloud. Azure nooit op gebouwen-referenties zonder codering opgeslagen.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\import-data-properties-entered.png)

8.  Klik op **uitvoeren** om het experiment uitvoert.

Zodra het experiment is voltooid kunt waarop u de gegevens die u uit de database geïmporteerd door te klikken op de uitvoerpoort van de module **Gegevens importeren** en selecteer **visualiseren**visualiseren.

Zodra u klaar bent met het ontwikkelen van uw experiment, kunt u het implementeren en mogelijk maken van het model. Met behulp van de Batch-Service worden uitgevoerd, gegevens uit de SQL Server-database op ruimten geconfigureerd in de module **Gegevens importeren** gelezen en gebruikt voor het scoren. Terwijl u de responsservice aanvragen kunt voor het scoren van de gegevens in de lokalen, is het raadzaam om de [Excel-invoegtoepassing](machine-learning-excel-add-in-for-web-services.md) gebruiken. Op dit moment wordt schrijven naar een SQL-Server op locatie database **Exporteren** van gegevens niet ondersteund in uw experimenten of gepubliceerde webservices.

