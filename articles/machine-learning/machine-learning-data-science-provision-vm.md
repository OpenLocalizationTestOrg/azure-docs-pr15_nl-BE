<properties 
    pageTitle="Inrichten van de virtuele Machine van Microsoft Data Science | Microsoft Azure" 
    description="Configureren en maken van een virtuele Machine voor Data Science op Azure analytics en machine learning." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="bradsev" />


# <a name="provision-the-microsoft-data-science-virtual-machine"></a>Inrichten van de virtuele Machine van Microsoft Data Science

Microsoft Data Science Virtual Machine is een afbeelding Azure VM (virtual machine) vooraf is geïnstalleerd en geconfigureerd met diverse populaire programma's die vaak worden gebruikt voor de analyse van gegevens en machine learning. De hulpprogramma's zijn:

- Microsoft R Server Developer Edition
- Anaconda Python distributie
- Jupyter laptop (met R, Python kernels)
- Visual Studio Community Edition
- Power BI-desktop
- SQL Server 2016 Developer Edition
- Leren van gereedschapswerktuigen
    - [Rekenkundige netwerk Toolkit (CNTK)](https://github.com/Microsoft/CNTK): een diepe leren software toolkit van Microsoft Research.
    - [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): een snelle machine leren technieken, zoals online, hash, allreduce, kortingen, learning2search, actief, ondersteunende systeem en interactief leren.
    - [XGBoost](https://xgboost.readthedocs.org/en/latest/): een hulpmiddel dat snelle en nauwkeurige gestimuleerd tree-implementatie.
    - [Rattle](http://rattle.togaware.com/) (de R analytische Tool om informatie gemakkelijk): een hulpprogramma waarmee u aan de slag met data analytics en machine learning in R eenvoudig, met GUI gebaseerde gegevens te verkennen en modellering met automatische R-code genereren.
    - [mxnet](https://github.com/dmlc/mxnet): een diepe leren raamwerk ontworpen voor efficiëntie en flexibiliteit
- Bibliotheken in R en Python voor gebruik in Azure Machine Learning en andere Azure diensten
- GIT met Git Bash werken met bron code opslagplaatsen met inbegrip van GitHub, Visual Studio Team Services
- Windows-poorten van verschillende populaire Linux opdrachtregelprogramma's (met inbegrip van awk, sed, perl, grep, zoeken, wget, curl, enz) toegankelijk via de opdrachtprompt. 


Doorlopen op een reeks taken doen wetenschap gegevens omvat: vinden, laden en pre-Processing gegevens, bouwen en testen van modellen en implementeren van de modellen voor verbruik in intelligente toepassingen. Tal van extra gegevens wetenschappers gebruiken om deze taken te voltooien. Het zijn heel veel tijd in beslag om te zoeken naar de juiste versies van de software, en vervolgens downloaden en installeren. Microsoft Data Science Virtual Machine kan deze zware taak vereenvoudigen door middel van een kant-en-klare afbeelding die kan worden ingericht op Azure met alle verschillende populaire programma's vooraf geïnstalleerd en geconfigureerd. 

De virtuele Machine van Microsoft Data Science jump-starts uw analytics-project. Hiermee kunt u taken in verschillende talen, met inbegrip van R, Python, SQL, en C#. Visual Studio biedt een IDE voor het ontwikkelen en testen van de code die is gemakkelijk te gebruiken. De SDK Azure is opgenomen in de VM kunt u uw toepassingen met verschillende services op Microsoft's cloud platform te bouwen. 

Er zijn geen kosten voor software voor dit image data science VM. U betaalt alleen voor de Azure gebruikskosten welke afhankelijk van de grootte van de virtuele machine die u wilt inrichten. In de detailsectie op de pagina [gegevens wetenschap virtuele Machine](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) vindt u meer informatie over de kosten berekenen. 


## <a name="prerequisites"></a>Vereisten

Voordat u een virtuele Machine van Microsoft Data Science maken kunt, hebt u het volgende:

- **Abonnement op een Azure**: Zie voor een [gratis proefversie Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

*   **Een Azure opslag account**: Zie maken van een [een account Azure opslag maken](storage-create-storage-account.md#create-a-storage-account). U kunt ook kan de opslag account worden gemaakt als onderdeel van het proces van het maken van de VM als u niet wilt gebruiken van een bestaande account.


## <a name="create-your-microsoft-data-science-virtual-machine"></a>De Microsoft Data Science virtuele Machine maken

Hier volgen de stappen voor het maken van een exemplaar van Microsoft Data Science Virtual Machine:

1.  Navigeer naar de virtuele machine op [Azure portal](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm)aanbieding.
2.   Selecteer de knop **maken** onder in een wizard moeten worden genomen. ![configureren-gegevens-wetenschap-vm](./media/machine-learning-data-science-provision-vm/configure-data-science-virtual-machine.png)
3.   De wizard voor het maken van de virtuele Machine van Microsoft Data Science vereist **ingangen** voor elk van de **vijf stappen** geïnventariseerd aan de rechterkant van deze afbeelding. Hier vindt u de "inputs" die nodig zijn voor het configureren van elk van deze stappen:
    
    1.   **Grondbeginselen**
        1.   **Naam**: naam van uw gegevens wetenschap-server die u maakt.
        2.   **Gebruikersnaam**: Admin account aanmeldings-id.
        3.   **Wachtwoord**: Admin wachtwoord.
        4.   **Abonnement**: als u meer dan één abonnement hebt, selecteert u een waarop de machine wordt gemaakt en gefactureerd.
        5.   **Resourcegroep**: kunt u een nieuwe of een bestaande groep gebruiken.
        6.   **Locatie**: Selecteer het datacenter die het meest geschikt is. Het is meestal het datacenter die de meeste van uw gegevens of heeft die het dichtst bij de fysieke locatie voor de snelste toegang tot het netwerk.
         
    2.   **Grootte**: Selecteer een van de server die voldoet aan de functionele eisen en kostenbeperkingen. U krijgt meer keuzes van VM formaten 'Alles weergeven' te selecteren.
    
    3.   **Instellingen**:
        1.   **Type schijf**: Premium kiezen als u liever een solid-state drive (SSD), anders kiest u 'Standaard'.
        2.   **Opslag-Account**: U kunt een nieuwe opslag Azure-account maken in uw abonnement of een bestaande gebruiken op de *locatie* die is gekozen in de **Grondbeginselen van** stap van de wizard.
        3.   **Andere parameters**: meestal hebt u de standaardwaarden. U kunt de muisaanwijzer op de informatieve koppeling voor meer informatie over de specifieke velden als u wilt kunt u het gebruik van niet-standaard waarden.

    4.   **Samenvatting**: Controleer of alle informatie die u hebt ingevoerd juist is.
    5.   **Kopen**: klik op **kopen** om te beginnen met het inrichten. Een koppeling wordt met de voorwaarden van de transactie geleverd. VM heeft geen eventuele bijkomende kosten buiten de compute voor de servergrootte van de uit de stap **grootte** . 


>[AZURE.NOTE] De inrichting moet ongeveer 10-20 minuten duren. De status van de provisioning wordt weergegeven op de portal Azure.

## <a name="how-to-access-the-microsoft-data-science-virtual-machine"></a>Toegang tot de virtuele Machine van Microsoft Data Science

Zodra de VM is gemaakt, kunt u in de Admin-accountreferenties die u in **de vorige sectie** hebt geconfigureerd met extern bureaublad. 

Wanneer uw VM gemaakt en ingericht, bent u klaar om te starten met de hulpprogramma's die zijn geïnstalleerd en geconfigureerd op deze. Er zijn start menu tegels en pictogrammen op het bureaublad voor veel van de hulpmiddelen. 

## <a name="how-to-create-a-strong-password-on-the-jupyter-notebook-server"></a>Het maken van een sterk wachtwoord op de server Jupyter-laptop 

Wilt maken van uw eigen sterke wachtwoord voor de Jupyter-laptop-server op de computer zijn geïnstalleerd, worden uitgevoerd met de volgende opdracht uit vanaf een opdrachtprompt op de Data Science virtuele Machine.

    c:\anaconda\python.exe -c "import IPython;print IPython.lib.passwd()"

Kies een sterk wachtwoord wanneer daarom wordt gevraagd.

Ziet u de hash van het wachtwoord in de indeling 'sha1:xxxxxx' in de uitvoer. Kopieer deze hash van het wachtwoord en vervangen de bestaande hash die in uw notitieblok config-bestand vinden op: **C:\ProgramData\jupyter\jupyter_notebook_config.py** met een parameter de naam ***c.NotebookApp.password***.

Alleen het deel van de bestaande hash-waarde, die binnen de aanhalingstekens (xxxxxx) vervangen. De offertes en de ***sha1:*** voorvoegsel voor de waarde van de parameter moet worden bewaard.

Ten slotte moet u stoppen en opnieuw starten van de server Jupyter, die wordt uitgevoerd op de VM als een geplande taak van windows **Start_IPython_Notebook**genoemd. Als uw nieuwe wachtwoord niet wordt geaccepteerd nadat deze taak opnieuw is opgestart, probeert het doden van alle actieve python processen van Taakbeheer en start de geplande taak. U kunt ook proberen de virtuele machine opnieuw opstarten.

## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>Hulpprogramma's zijn geïnstalleerd op Microsoft Data Science Virtual Machine

### <a name="microsoft-r-server-developer-edition"></a>Microsoft R Server Developer Edition
Als u wenst te gebruiken R voor uw analytics, heeft de VM Microsoft R Server Developer edition is geïnstalleerd. Microsoft R Server is een algemeen bruikbare bedrijfsniveau analytics platform op basis van R die wordt ondersteund, schaalbare en veilige. Ondersteuning van een aantal grote gegevens statistieken, voorspellende modellen en mogelijkheden leren machine, ondersteunt R-Server het volledige bereik van analytics – exploratie, analyse, visualisatie en modellering. Gebruiken en uitbreiden van open-source R, is R-Microsoft-Server volledig compatibel met R-scripts, functies en CRAN pakketten, voor het analyseren van gegevens op de schaal van de onderneming. Biedt ook een oplossing de beperkingen in het geheugen van de Open Source-R door parallelle en gesegmenteerde verwerking van gegevens toe te voegen. Hiermee kunt u analytics uitvoeren op gegevens veel groter dan wat in het hoofdgeheugen past.  Visual Studio Community Edition opgenomen op de VM bevat de R-hulpprogramma's voor Visual Studio-extensie die een volledige IDE biedt voor het werken met R. U kunt ook downloaden en gebruiken van andere IDEs ook zoals [RStudio](http://www.rstudio.com). 

### <a name="python"></a>Python
Voor de ontwikkeling van het gebruik van Python, is Anaconda, Python distributie 2.7 en 3.5 geïnstalleerd. Deze verdeling bevat de basis Python met ongeveer 300 van de meest populaire math, engineering en data analytics pakketten. U kunt extra Python voor Visual Studio (PTVS) dat is geïnstalleerd in de editie van Visual Studio 2015 Gemeenschap of een van de IDEs gebundeld met Anaconda zoals niet-actief of Spyder. U kunt een van deze door te zoeken op de zoekbalk starten (**wint** + sleutel**S** ).

>[AZURE.NOTE] De Python's voor Visual Studio op Anaconda Python 2.7 en 3.5 punt, moet u voor het maken van aangepaste omgevingen voor elke versie. Deze paden omgeving om in te stellen met Visual Studio 2015 Community Edition, gaat u naar **Extra** -> **Extra Python** -> **Python omgevingen** en klik vervolgens op **+ aangepast**. 

Anaconda, Python 2.7 is geïnstalleerd onder C:\Anaconda en Anaconda Python 3.5 onder c:\Anaconda\envs\py35 is geïnstalleerd. Zie [PTVS documentatie](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) voor gedetailleerde stappen. 

### <a name="jupyter-notebook"></a>Jupyter-laptop
Verdeling van de anaconda wordt ook geleverd met een laptop Jupyter, een omgeving voor het delen van code en -analyse. Een laptop Jupyter server is vooraf geconfigureerd met 2 Python, Python 3 en R kernels. Er wordt een pictogram op het bureaublad met de naam "Jupyter laptop voor het starten van de browser toegang tot de server van de laptop. Als u van de VM via Extern bureaublad gebruikmaakt, kunt u ook via [https://localhost:9999 /](https://localhost:9999/) toegang tot de server van de laptop Jupyter wanneer aangemeld bij de VM.
 
>[AZURE.NOTE] Als u waarschuwingen voor de certificaten worden voortgezet. 

We hebben verpakt monster laptops in Python en R. De laptops Jupyter hoe werken met Microsoft R Server, SQL Server-Services voor 2016 R (analytics-database), Python en andere Azure technologieën zodra u zich op Jupyter aanmelden. Ziet u de koppeling naar de monsters op de introductiepagina van de laptop na verificatie van de Jupyter-laptop met het wachtwoord dat u hebt gemaakt in een eerdere stap. 

### <a name="visual-studio-2015-community-edition"></a>Edition van Visual Studio 2015 Gemeenschap
Visual Studio Community edition geïnstalleerd op de VM. Het is een gratis versie van de populaire IDE van Microsoft die u voor evaluatiedoeleinden en voor kleine teams gebruiken kunt. U kunt de licentievoorwaarden uitchecken [hier](https://www.visualstudio.com/support/legal/mt171547).  Visual Studio openen door te dubbelklikken op het pictogram op het bureaublad of in het menu **Start** . U kunt ook zoeken naar programma's met **Win** + **S** en "Visual Studio" invoeren. Zodra er kunt u projecten in talen zoals C#, Python, R, node.js. Plug-ins worden ook geïnstalleerd die werkt met Azure services, zoals een catalogus met Azure gegevens Azure HDInsight (Hadoop, Spark) en Azure gegevens Lake te vergemakkelijken. 

>[AZURE.NOTE] U krijgt mogelijk een bericht weergegeven dat de evaluatieperiode is verlopen. Geef uw referenties van Microsoft-account of maak een nieuwe gratis account toegang krijgen tot het Visual Studio Community Edition. 

### <a name="sql-server-2016-developer-edition"></a>SQL Server 2016 Developer edition
Een developer-versie van SQL Server 2016 met R-Services uit te voeren in de database analytics is beschikbaar op de VM. R-Services bieden een platform voor het ontwikkelen en implementeren van intelligente toepassingen. U kunt de R rijke en krachtige taal en de vele pakketten uit de Gemeenschap maken van modellen en voorspellingen voor uw SQL Server-gegevens genereren. U kunt analytics dicht bij de gegevens houden omdat R Services (In database) de taal R integratie met SQL Server. Hierdoor worden de kosten en de beveiligingsrisico's die zijn gekoppeld aan de verplaatsing van gegevens.

>[AZURE.NOTE] De developer edition van SQL Server 2016 kan alleen worden gebruikt voor ontwikkel- en testdoeleinden. U moet een licentie uit te voeren in de productie. 

Toegang tot de SQL-server met **SQL Server Management Studio**te starten. De naam van uw VM wordt gevuld als de naam van de Server. Windows-verificatie gebruiken als u bent aangemeld als de beheerder op Windows. Zodra u zich in SQL Server Management Studio kunt u andere gebruikers maken, databases maken, gegevens importeren en SQL-query's uitvoeren. 

Als u In de database analytics met behulp van Microsoft R, voert de volgende opdracht als een actie in een SQL Server management studio na aanmelden als beheerder van de tijd. 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 
        
        (Please replace the %COMPUTERNAME% with your VM name)


### <a name="azure"></a>Azure 
Verschillende Azure hulpprogramma's zijn geïnstalleerd op de VM:

- Er is een snelkoppeling op het bureaublad voor toegang tot de Azure SDK-documentatie. 
- **AzCopy**: gebruikt om gegevens in uw Account Microsoft Azure opslag te verplaatsen. Gebruik typt **Azcopy** achter de opdrachtprompt om te zien van het gebruik. 
- **Microsoft Azure Opslagverkenner**: gebruikt om te bladeren door de objecten die u hebt opgeslagen in uw Account voor Azure opslag en overdracht gegevens van en naar Azure opslag. U kunt **Opslagverkenner** Typ in zoeken of vinden in het menu Start van Windows om toegang tot dit programma. 
- **Adlcopy**: gebruikt om gegevens te verplaatsen naar Azure gegevens Lake. Overzicht van gebruik, typt u **adlcopy** in een MS-DOS-prompt. 
- **dtui**: gebruikt om gegevens te verplaatsen naar en van Azure DocumentDB, een database NoSQL in de cloud. Typ **dtui** op de opdrachtprompt. 
- **Microsoft Data Management Gateway**: Hiermee kunt u gegevens verplaatsen tussen gegevensbronnen op gebouwen en wolken. Het wordt gebruikt in hulpprogramma's zoals Azure Data Factory. 
- **Microsoft Azure Powershell**: een hulpprogramma voor het beheren van uw Azure resources in de Powershell scripttaal op uw VM is geïnstalleerd. 

###<a name="power-bi"></a>Power BI

Om u te helpen bouwen van dashboards en fantastische visualisaties, is de **Power BI-bureaublad** geïnstalleerd. Gebruik dit hulpprogramma pull gegevens uit verschillende bronnen aan uw dashboards en rapporten te ontwerpen en publiceren naar de cloud. Voor meer informatie, Zie de [Power BI](http://powerbi.microsoft.com) -site. Power BI bureaublad vindt u in het menu Start. 

>[AZURE.NOTE] U moet een Office 365 account voor toegang tot de Power BI. 

## <a name="additional-microsoft-development-tools"></a>Aanvullende Microsoft-ontwikkelprogramma 's
De [**Microsoft Web Platform Installer**](https://www.microsoft.com/web/downloads/platform.aspx) kan worden gebruikt om te ontdekken en andere ontwikkelprogramma's van Microsoft downloaden. Er is ook een snelkoppeling naar het hulpprogramma op het bureaublad van Microsoft Data Science Virtual Machine.  

## <a name="important-directories-on-the-vm"></a>Belangrijke mappen op de VM

| Artikel                          | Directory |
| ------------------------------| ---------------- |
|Jupyter laptop serverconfiguraties | C:\ProgramData\jupyter |
|Monsters Jupyter laptop-basismap| c:\dsvm\notebooks|
|Andere voorbeelden | c:\dsvm\samples|
| Anaconda (standaard: Python 2.7) | c:\Anaconda |
| Anaconda Python 3.5 omgeving | c:\Anaconda\envs\py35|
|De zelfstandige Server R exemplaar directory (standaard R exemplaar) | C:\Program Files\Microsoft SQL Server\130\R_SERVER |
| R-Server-database-instantie directory | C:\Program Files\Microsoft SQL Server\MSSQL13. MSSQLSERVER\R_SERVICES |
| Diverse hulpmiddelen | c:\dsvm\tools|

>[AZURE.NOTE] Exemplaren van de Microsoft Data Science Virtual Machine gemaakt voordat 1.5.0 (vóór 3 September 2016) gebruikt een mapstructuur voor iets anders dan in de voorgaande tabel opgegeven. 

## <a name="next-steps"></a>Volgende stappen
Hieronder staan enkele volgende stappen blijven uw leren en exploratie. 

* De verschillende wetenschap hulpmiddelen voor gegevens op de data science VM verkennen door te klikken op het startmenu en de controle van de hulpprogramma's die worden vermeld in het menu.
* Ga naar **C:\Program Files\Microsoft SQL Server\130\R_SERVER\library\RevoScaleR\demoScripts** voor monsters met behulp van de RevoScaleR-bibliotheek in R die data analytics op enterprise-niveau ondersteunt.  
* Lees het artikel: [10 dingen die u op de wetenschap gegevens virtuele Machine doen kunt](http://aka.ms/dsvmtenthings)
* Informatie over het bouwen van complete analytische oplossingen met systematisch [Team gegevens wetenschap proces](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).
* Ga naar de [Galerie met Cortana Intelligence](http://gallery.cortanaintelligence.com) voor machine learning data analytics voorbeelden en die gebruikmaken van de Cortana Intelligence Suite. We hebben ook een pictogram beschikbaar in het menu **Start** en op het bureaublad van de virtuele machine aan deze galerie.

