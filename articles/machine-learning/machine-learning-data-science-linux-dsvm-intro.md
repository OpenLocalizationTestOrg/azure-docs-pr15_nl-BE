<properties
    pageTitle="Inrichten van de virtuele Machine Linux Data Science | Microsoft Azure"
    description="Configureren en maken van een virtuele Machine voor Linux Data Science op Azure analytics en machine learning."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="bradsev" />

# <a name="provision-the-linux-data-science-virtual-machine"></a>Inrichten van de virtuele Machine Linux Data Science

De virtuele Machine van Linux Data Science is een Azure virtuele machine die wordt geleverd met vooraf geïnstalleerde programma's. Deze hulpprogramma's worden vaak gebruikt voor het uitvoeren van data-analytics en machine learning. De belangrijkste softwareonderdelen opgenomen zijn:

- Microsoft R Server Developer Edition
- Anaconda Python distributie (versie 2.7 en 3,5-inch), met inbegrip van populaire data analysis-bibliotheken
- JupyterHub - een omgeving met meerdere gebruikers Jupyter laptop server ondersteunen R, Python, Julia kernels
- Azure Opslagverkenner
- Azure opdrachtregelinterface (CLI) voor het beheren van bronnen Azure
- PostgresSQL Database
- Leren van gereedschapswerktuigen
    - [Rekenkundige netwerk Toolkit (CNTK)](https://github.com/Microsoft/CNTK): een diepe leren software toolkit van Microsoft Research.
    - [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): een snelle machine leren technieken, zoals online, hash, allreduce, kortingen, learning2search, actief, ondersteunende systeem en interactief leren.
    - [XGBoost](https://xgboost.readthedocs.org/en/latest/): een hulpmiddel dat snelle en nauwkeurige gestimuleerd tree-implementatie.
    - [Rattle](http://rattle.togaware.com/) (de R analytische Tool om informatie gemakkelijk): een hulpprogramma waarmee u aan de slag met data analytics en machine learning in R eenvoudig, met GUI gebaseerde gegevens te verkennen en modellering met automatische R-code genereren.
- Azure SDK in Java, Python, node.js, Ruby, PHP
- Bibliotheken in R en Python voor gebruik in Azure Machine Learning en andere Azure diensten
- Ontwikkelprogramma's en editors (Eclips, Emacs, gedit, vi)

Voeren wetenschappelijke gegevens moet doorlopen op een reeks taken:

1. Zoeken, laden en vooraf verwerken van gegevens
2. Maken en testen van modellen
3. De modellen voor verbruik in intelligente toepassingen implementeren

Gegevens wetenschappers diverse hulpprogramma's gebruiken om deze taken te voltooien. Het kan veel tijd in beslag nemen om te zoeken naar de juiste versies van de software en klik om te downloaden, compileren en installeren van deze versies.

De virtuele Machine van Linux Data Science kunnen deze last aanzienlijk verminderen. Deze gebruiken om snel uw analytics-project. Hiermee kunt u taken in verschillende talen, waaronder R, Python, SQL, Java en C++. Eclips biedt een IDE voor het ontwikkelen en testen van de code die is gemakkelijk te gebruiken. De SDK Azure is opgenomen in de VM kunt u uw toepassingen bouwen met behulp van verschillende services op Linux voor het Microsoft cloud-platform. Bovendien hebt u toegang tot andere talen zoals Ruby, Perl, PHP en node.js die vooraf zijn geïnstalleerd.

Er zijn geen kosten voor software voor dit image data science VM. U betaalt alleen de Azure hardware-gebruikskosten die worden beoordeeld op basis van de grootte van de virtuele machine die u met de afbeelding VM inrichten. Meer informatie over de kosten berekenen kunnen u vinden op de [objectpagina op de markt voor Azure VM ](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).


## <a name="prerequisites"></a>Vereisten

Voordat u een virtuele Machine voor Linux Data Science maken kunt, hebt u het volgende:

- **Abonnement op een Azure**: Zie voor een [gratis proefversie Azure ophalen](https://azure.microsoft.com/free/).
- **Een Azure opslag account**: Zie maken van een [een account Azure opslag maken](storage-create-storage-account.md#create-a-storage-account). U kunt ook de opslag account kan worden gemaakt als onderdeel van het proces van het maken van de VM, als u niet wilt gebruiken van een bestaande account.


## <a name="create-your-linux-data-science-virtual-machine"></a>Uw gegevens Linux wetenschap virtuele Machine maken

Hier volgen de stappen voor het maken van een exemplaar van de Linux Data Science Virtual Machine:

1.  Navigeer naar de virtuele machine op de [Azure portal](https://portal.azure.com/#create/microsoft-ads.linux-data-science-vmlinuxdsvm)aanbieding.
2.   Klik op **maken** (onderin) om de wizard. ![configureren-gegevens-wetenschap-vm](./media/machine-learning-data-science-linux-dsvm-intro/configure-linux-data-science-virtual-machine.png)
3.   De volgende secties bevatten de ingangen voor elk van de stappen in de wizard (geïnventariseerd aan de rechterkant van de voorgaande afbeelding) gebruikt voor het maken van de virtuele Machine van Microsoft Data Science. Hier vindt u de "inputs" die nodig zijn voor het configureren van elk van deze stappen:

    een. **Basisbeginselen**:

  - **Naam**: naam van uw gegevens wetenschap-server die u maakt.
  - **Gebruikersnaam**: eerste account aanmelden ID.
  - **Wachtwoord**: eerste wachtwoord (u kunt openbare SSH-sleutel gebruiken in plaats van het wachtwoord).
  - **Abonnement**: als u meer dan één abonnement hebt, selecteert u een waarop de machine wordt gemaakt en gefactureerd. U moet bevoegd resource maken voor dit abonnement.
  - **Resourcegroep**: kunt u een nieuwe of een bestaande groep gebruiken.
  - **Locatie**: Selecteer het datacenter die het meest geschikt is. Het is meestal het datacenter die de meeste van uw gegevens, of heeft die het dichtst bij de fysieke locatie voor de snelste toegang tot het netwerk.

    b. **Grootte**:

  - Selecteer een van de server die voldoet aan de functionele eisen en kostenbeperkingen. Selecteer **Alles** om meer opties van VM formaten te bekijken.

    c. **Instellingen**:

  - **Type schijf**: **Premium** kiezen als u liever een solid state harde schijf (SSD). Anders kiest u **standaard**.
  - **Opslag-Account**: U kunt een nieuwe opslag Azure-account maken in uw abonnement of een bestaande gebruiken op de locatie die is gekozen in de **Grondbeginselen van** stap van de wizard.
  - **Andere parameters**: In de meeste gevallen gebruikt u gewoon de standaardwaarden. Rekening te houden met niet-standaard waarden, de muisaanwijzer op de informatieve koppeling voor Help-informatie over specifieke velden.

    d. **Samenvatting**:

  - Controleer of alle informatie die u hebt ingevoerd juist zijn.

    e. **Kopen**:

  - Klik op **kopen**om te beginnen met het inrichten. Een koppeling wordt met de voorwaarden van de transactie geleverd. VM heeft geen eventuele bijkomende kosten buiten de compute voor de servergrootte van de uit de stap **grootte** .

De inrichting moet ongeveer 10-20 minuten duren. De status van de provisioning wordt weergegeven op de portal Azure.

## <a name="how-to-access-the-linux-data-science-virtual-machine"></a>Toegang tot de virtuele Machine van Linux Data Science

Nadat de VM is gemaakt, kunt u ondertekenen het met behulp van SSH. De referenties die u hebt gemaakt in de sectie over de **Grondbeginselen** van stap 3 voor de shell tekst interface gebruikt. In Windows, kunt u een hulpprogramma voor SSH-client zoals [Putty](http://www.putty.org)downloaden. Als u liever een grafisch bureaublad (Windows-besturingssysteem), kan u de X2Go-client installeren of doorsturen op stopverf X11 gebruikt.

>[AZURE.NOTE] De X2Go-client uitgevoerd aanzienlijk beter dan X11 doorsturen testen. Wij raden u aan met de X2Go-client voor een grafische interface voor de desktop.


## <a name="installing-and-configuring-x2go-client"></a>Installeren en configureren van X2Go-client

De Linux VM nog ingericht met server X2Go en gereed voor clientverbindingen accepteert. De grafische desktop Linux VM verbinding wordt als volgt op de client:

1. Download en installeer de client X2Go voor uw client platform van [X2Go](http://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
2. Het uitvoeren van de X2Go en selecteer **Nieuwe sessie**. Er verschijnt een configuratievenster met meerdere tabbladen. Voer de volgende configuratieparameters:
    * **Tabblad sessies**:
        - **Host**: de hostnaam of het IP-adres van uw VM Linux Data Science.
        - **Inloggen**: gebruikersnaam op de Linux VM.
        - **SSH-poort**: laat het op 22, de standaardwaarde.
        - **Sessietype**: Wijzig de waarde in XFCE. De Linux VM ondersteunt momenteel alleen XFCE desktop.
    * **Tabblad Media**: kunt u uitschakelen geluidsondersteuning en client afdrukken als u niet hoeft te gebruiken.
    * **Gedeelde mappen**: als u mappen van de clientcomputers op de Linux VM gemonteerd wilt, de client machine mappen toevoegen die u wilt delen met de VM op dit tabblad.

Nadat u zich voor VM aanmelden met behulp van de SSH-client of de grafische desktop XFCE via de client X2Go, bent u klaar om te starten met de hulpprogramma's die zijn geïnstalleerd en geconfigureerd op de VM. Op XFCE ziet u de snelkoppelingen in het menu toepassingen en pictogrammen op het bureaublad voor veel van de hulpmiddelen.


## <a name="tools-installed-on-the-linux-data-science-virtual-machine"></a>Hulpprogramma's zijn geïnstalleerd op de Linux Data Science virtuele Machine

### <a name="microsoft-r-open"></a>Open Microsoft R
R is een van de meest populaire talen voor gegevensanalyse en machine learning. Als u R gebruiken voor uw analytics wilt, heeft de VM Microsoft R openen (BHT) met wiskunde Kernel bibliotheek (MKL). De MKL optimaliseert wiskundige bewerkingen gebruikelijk in analytical algoritmen. BHT is 100 procent compatibel met CRAN-R en een van de R-bibliotheken in CRAN gepubliceerd kan worden geïnstalleerd op de BHT. U kunt uw R-programma's in een van de standaard-editor zoals vi, Emacs, gedit of het bewerken. U kunt ook downloaden en gebruiken van andere IDEs, zoals [RStudio](http://www.rstudio.com). Voor uw gemak, is een eenvoudig script (installRStudio.sh) beschikbaar in de map **/dsvm/tools** die door RStudio worden geïnstalleerd. Als u de editor Emacs, is opmerking dat de Emacs ESS (Emacs spreekt statistieken) pakket, vereenvoudigt het werken met bestanden in de editor Emacs R vooraf geïnstalleerd.

Om te starten R, typ **R** de shell. Hiermee gaat u naar een interactieve omgeving. Voor het ontwikkelen van het programma R u gebruikt meestal in een editor zoals Emacs of vi of gedit en voer de scripts in R. Als u RStudio hebt geïnstalleerd, hebt u een volledige grafische IDE-omgeving voor de ontwikkeling van het programma R.

Er is ook een script R voor u de [Top 20 R-pakketten](http://www.kdnuggets.com/2015/06/top-20-r-packages.html) te installeren als u wilt. Dit script kan worden uitgevoerd als u de interactieve interface R, die kan worden ingevoerd (zoals vermeld) door te typen **R** in de shell.  

### <a name="python"></a>Python
Voor de ontwikkeling van het gebruik van Python, is Anaconda, Python distributie 2.7 en 3.5 geïnstalleerd. Deze verdeling bevat de basis Python met ongeveer 300 van de meest populaire math, engineering en data analytics pakketten. U kunt de standaard tekst-editor. Bovendien kunt u Spyder, een IDE Python, die wordt meegeleverd met Python Anaconda verdelingen. Spyder een grafische desktop- of X11 moet doorsturen. Een snelkoppeling naar de Spyder is beschikbaar in het grafisch bureaublad.

Omdat we Python 2.7 en 3.5 hebt, moet u expliciet de gewenste Python versie die u wilt werken in de huidige sessie activeren. Het activeringsproces wordt de variabele pad naar de gewenste versie van Python.

Python 2.7 activeren, voer de volgende vanuit de shell:

    source /anaconda/bin/activate root

2.7 Python is geïnstalleerd op de */anaconda/bin*.

Python 3.5 activeren, voer de volgende vanuit de shell:

    source /anaconda/bin/activate py35


3.5 Python is geïnstalleerd op de */anaconda/envs/py35/bin*.

Om aan te roepen in een interactieve sessie Python, typt u **python** in de shell. Als u zich op een grafische interface of X11 set up doorsturen, typt u **spyder** om de IDE Python starten.

### <a name="jupyter-notebook"></a>Jupyter-laptop

De verdeling van de Anaconda wordt ook geleverd met een laptop Jupyter, een omgeving voor het delen van code en -analyse. De Jupyter-laptop is toegankelijk via JupyterHub. U aanmelden met uw lokale Linux-gebruikersnaam en wachtwoord.

De server Jupyter laptop is vooraf geconfigureerd met 2 Python, Python 3 en R kernels. Er wordt een pictogram op het bureaublad met de naam "Jupyter laptop" voor het starten van de browser toegang tot de server van de laptop. Als u zich op de VM via SSH of X2Go-client, kunt u ook via [https://localhost:8000 /](https://localhost:8000/) voor toegang tot de server Jupyter laptop.

>[AZURE.NOTE] Als u waarschuwingen voor de certificaten worden voortgezet.

Toegang tot de server Jupyter laptop van een willekeurige host. Typ *https://\<VM DNS-naam of het IP-adres\>: 8000 /*

>[AZURE.NOTE] Poort 8000 wordt standaard geopend in de firewall wanneer de VM is ingericht.

We hebben verpakt monster laptops--één in Python en één in R. Nadat u de laptop Jupyter verifiëren met behulp van uw lokale Linux-gebruikersnaam en wachtwoord, kunt u de koppeling naar de monsters op de introductiepagina van de laptop bekijken. U kunt een nieuw notitieblok maken door **Nieuw**en de kernel voor de gewenste taal te selecteren. Als u de knop **Nieuw** niet ziet, klikt u op het pictogram van de **Jupyter** op de bovenste links te gaan naar de introductiepagina van de server van de laptop.


### <a name="ides-and-editors"></a>IDEs en editors

U hebt de keuze uit verschillende code-editors. Dit omvat vi/VIM, Emacs, gEdit en Eclips. gEdit Eclips grafische editors zijn en moet u zijn aangemeld bij een grafisch bureaublad gebruiken. Deze editors hebben desktop- en snelkoppelingen in menu Start ze.

Zijn de tekst gebaseerde editor **VIM** en **Emacs** . Op Emacs, hebben we een add-on-pakket Emacs spreekt statistieken (ESS) die werken met R gemakkelijker binnen de Emacs editor genoemd geïnstalleerd. Meer informatie vindt u op [ESS](http://ess.r-project.org/).

**Eclips** is een open-source, uitbreidbare IDE die meerdere talen ondersteunen. De ontwikkelaars van Java editie is het exemplaar op de VM is geïnstalleerd. Er zijn Plug-ins beschikbaar voor diverse populaire talen die kunnen worden geïnstalleerd om uit te breiden de Eclips-omgeving. We hebben ook een invoegtoepassing geïnstalleerd in Eclips **Azure Toolkit voor Eclips**genoemd. U kunt maken, ontwikkelen, testen en implementeren van Azure-toepassingen met behulp van de Eclips ontwikkelomgeving die talen zoals Java ondersteunt. Er is ook een **SDK voor Java Azure** toegang tot verschillende Azure services uit met een Java-omgeving. Meer informatie over Azure toolkit voor Eclips kan worden gevonden op [Azure Toolkit voor Eclips](../azure-toolkit-for-eclipse.md).

**LaTex** wordt geïnstalleerd via een Emacs invoegtoepassing [auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html) pakket ontwerpen in Emacs LaTex documenten vereenvoudigt het pakket texlive.  

### <a name="databases"></a>Databases

#### <a name="postgres"></a>Postgres
De brondatabase openen **Postgres** is beschikbaar op de VM, met de services die worden uitgevoerd en de initdb reeds voltooid. Moet u toch te maken, databases en gebruikers. Zie voor meer informatie de [documentatie bij de Postgres](https://www.postgresql.org/docs/).  

####  <a name="graphical-sql-client"></a>Grafische SQL client
Verbinding maken met andere databases (zoals Microsoft SQL Server, Postgres en MySQL) en voor het uitvoeren van SQL-query's is **SQL eekhoorn**, een grafische SQL client verschaft. U kunt deze uitvoeren vanuit een grafisch bureaublad-sessies (met behulp van de X2Go-client, bijvoorbeeld). Om aan te roepen eekhoorn SQL, kunt u starten via het pictogram op het bureaublad of Voer de volgende opdracht op de shell.

    /usr/local/squirrel-sql-3.7/squirrel-sql.sh

Vóór het eerste gebruik de stuurprogramma's en database-aliassen instellen. De JDBC-stuurprogramma's vinden op:

*/usr/share/Java/jdbcdrivers*

Voor meer informatie Zie [Eekhoorn SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

#### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Opdrachtregelprogramma's voor toegang tot Microsoft SQL Server

Het pakket met ODBC-stuurprogramma voor SQL Server wordt ook geleverd met twee opdrachtregelhulpprogramma's:

**BCP**: het hulpprogramma bcp bulk kopieert gegevens tussen een instantie van Microsoft SQL Server en een bestand in een door de gebruiker opgegeven indeling. Het hulpprogramma bcp kan een groot aantal nieuwe rijen in SQL Server-tabellen importeren of exporteren van gegevens uit tabellen in gegevensbestanden worden gebruikt. Om gegevens te importeren in een tabel, moet u een bestandsindeling die is gemaakt voor die tabel gebruiken of begrijpen van de structuur van de tabel en de soorten gegevens die geldig voor de kolommen zijn.

Zie [verbinding maken met bcp](https://msdn.microsoft.com/library/hh568446.aspx)voor meer informatie.

**sqlcmd**: Transact-SQL-instructies invoeren met het hulpprogramma sqlcmd, alsmede procedures voor systeem en scriptbestanden achter de opdrachtprompt. Dit hulpprogramma wordt ODBC gebruikt voor het uitvoeren van Transact-SQL-batches.

Zie [verbinding maken met sqlcmd](https://msdn.microsoft.com/library/hh568447.aspx)voor meer informatie.

>[AZURE.NOTE] Er zijn enkele verschillen in dit hulpprogramma tussen Linux en Windows-platforms. Raadpleeg de documentatie voor meer informatie.


#### <a name="database-access-libraries"></a>Database access bibliotheken

Bibliotheken zijn beschikbaar in R en Python naar access-databases.

- In R, het **RODBC** of **dplyr** pakket Hiermee kunt u een query of SQL-instructies uitvoeren op de databaseserver.
- In Python biedt de bibliotheek **pyodbc** toegang tot de database met behulp van ODBC als de onderliggende laag.  

Toegang krijgen tot **Postgres**:

- Gebruik het **RPostgreSQL**-pakket van tekst.
- Gebruik de bibliotheek **psycopg2** van Python:.


### <a name="azure-tools"></a>Azure-hulpprogramma 's
De volgende Azure hulpprogramma's zijn geïnstalleerd op de VM:

- **Azure opdrachtregelinterface**: de Azure CLI kunt u maken en beheren van Azure bronnen via shell-opdrachten. Als u wilt de Azure's aanroepen, typt u **help azure**. Zie de [pagina van Azure CLI-documentatie](../virtual-machines-command-line-tools.md)voor meer informatie.
- **Microsoft Azure Opslagverkenner**: Microsoft Azure Opslagverkenner is een grafisch hulpprogramma dat wordt gebruikt om te bladeren door de objecten die u hebt opgeslagen in uw account Azure opslag en voor het uploaden en downloaden van gegevens naar en van Azure BLOB's. Opslagverkenner is toegankelijk via het pictogram voor de snelkoppeling op het bureaublad. U kunt deze aanroepen vanuit een shell-prompt door **StorageExplorer**te typen. U moet zijn aangemeld vanaf een client X2Go of X11 set up doorsturen hebben.
- **Azure bibliotheken**: de volgende zijn enkele van de vooraf geïnstalleerde bibliotheken.

 - **Python**: de Azure-gerelateerde bibliotheken in Python die geïnstalleerd zijn **azure**, **azureml**, **pydocumentdb**en **pyodbc**. U kunt met de eerste drie bibliotheken Azure opslagservices Azure Machine Learning en Azure DocumentDB (een database NoSQL op Azure) openen. De vierde bibliotheek pyodbc (in combinatie met het ODBC-stuurprogramma voor SQL Server), kunt toegang tot SQL Server, Azure SQL-Database en Azure SQL Data Warehouse van Python met behulp van een ODBC-interface. **Pip-lijst** als u wilt zien van de genoemde bibliotheken invoeren. Zorg ervoor dat u deze opdracht in de Python 2.7 en 3,5 omgevingen.
 - **R**: de Azure-gerelateerde bibliotheken in R die zijn geïnstalleerd, zijn **AzureML** en **RODBC**.
 - **Java**: de lijst met Azure Java bibliotheken vindt u in de map **/dsvm/sdk/AzureSDKJava** op de VM. De belangrijkste bibliotheken zijn Azure opslag en het beheer-API's, DocumentDB en JDBC-stuurprogramma's voor SQL Server.  

Toegang tot de [Azure portal](https://portal.azure.com) van de vooraf geïnstalleerde Firefox-browser. Op de Azure portal kunt u maken, beheren en controleren Azure bronnen.

### <a name="azure-machine-learning"></a>Azure Machine Learning

Azure Machine Learning is een volledig beheerde cloud-service waarmee u kunt bouwen, implementeren en anticiperende analytische oplossingen delen. U maken uw experimenten en modellen van Azure Machine Learning Studio. Deze toegankelijk is vanuit een webbrowser op de virtuele machine voor data science via [Microsoft Azure Machine Learning](https://studio.azureml.net).

Nadat u zich voor Azure Machine Learning Studio aanmelden, hebt u toegang tot een canvas experimenten waarbij u een logische stroom voor de machine learning algoritmen kunt samenstellen. U ook toegang hebt tot een Jupyter laptop gehost op Azure Machine Learning en probleemloos samenwerken met de experimenten in de Studio van Machine Learning. Mogelijk maken de computer leren werken met modellen die u hebt gemaakt door ze in een webservice-interface. Hierdoor kunnen clients in een willekeurige taal zijn geschreven om op te roepen, voorspellingen van de computer leren werken met modellen. Zie voor meer informatie de [documentatie bij de computer leren](https://azure.microsoft.com/documentation/services/machine-learning/).

U kunt ook uw modellen in R of Python voortbouwen op de VM en implementeert u deze in de productie van Azure Machine Learning. We hebben bibliotheken geïnstalleerd in R (**AzureML**) en Python (**azureml**) deze functionaliteit in te schakelen.

Zie voor informatie over het implementeren van modellen in R en Python in Azure Machine Learning [tien dingen die u op de wetenschap gegevens virtuele Machine doen kunt](machine-learning-data-science-vm-do-ten-things.md) (met name de sectie ' R of Python modellen bouwen en ze mogelijk maken met Azure Machine Learning ").

>[AZURE.NOTE] Deze instructies zijn geschreven voor de Windows-versie van VM wetenschappelijke gegevens. Maar de verstrekte informatie over het implementeren van modellen met Azure Machine Learning is van toepassing op de Linux VM.

### <a name="machine-learning-tools"></a>Leren van gereedschapswerktuigen

De VM wordt geleverd met een paar machine learning tools en algoritmen die vooraf gecompileerd en lokaal geïnstalleerd. Deze omvatten:

* **CNTK** (Computational netwerk Toolkit van Microsoft Research): een diepe leren toolkit.
* **Vowpal Wabbit**: een snelle online leren algoritme.
* **xgboost**: een hulpprogramma waarmee u de structuur van geoptimaliseerde, gestimuleerd algoritmen.
* **Python**: Anaconda, Python wordt geleverd met een machine learning algoritmen met bibliotheken zoals Scikit leren. U kunt andere bibliotheken installeren met behulp van de `pip install` opdracht.
* **R**: een uitgebreide bibliotheek van machine learning functies is beschikbaar voor R. Sommige van de bibliotheken die vooraf geïnstalleerd zijn zijn lm, glm, randomForest, rpart. Andere bibliotheken kunnen worden geïnstalleerd door te voeren:

        install.packages(<lib name>)

Hier wordt aanvullende informatie over de eerste drie machine learning-hulpprogramma's in de lijst.

#### <a name="cntk"></a>CNTK
Dit is een open-source, diep leren toolkit. Het is een opdrachtregelprogramma (cntk) en is al in het pad.

Om een voorbeeld van een eenvoudige uitvoert, de volgende opdrachten worden uitgevoerd in de shell:

    # Copy samples to your home directory and execute cntk
    cp -r /dsvm/tools/CNTK-2016-02-08-Linux-64bit-CPU-Only/Examples/Other/Simple2d cntkdemo
    cd cntkdemo/Data
    cntk configFile=../Config/Simple.cntk

De uitvoer van het model is in *~/cntkdemo/Output/Models*.

Zie de sectie CNTK van [GitHub](https://github.com/Microsoft/CNTK)en de [CNTK wiki](https://github.com/Microsoft/CNTK/wiki)voor meer informatie.


#### <a name="vowpal-wabbit"></a>Vowpal Wabbit

Vowpal Wabbit is een machine-systeem maakt gebruik van technieken, zoals online, hash, allreduce, kortingen, learning2search, actief, leren en interactieve learning.

Als u het hulpprogramma uitvoert op een eenvoudig voorbeeld, het volgende doen:

    cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
    cd vwdemo
    vw house_dataset

Er zijn andere, grotere demo's in die map. Zie voor meer informatie over VW, [dit gedeelte van GitHub](https://github.com/JohnLangford/vowpal_wabbit)en de [wiki Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit/wiki).

#### <a name="xgboost"></a>xgboost
Dit is een bibliotheek die is ontworpen en geoptimaliseerd voor algoritmen gestimuleerd (structuur). Het doel van deze bibliotheek is om de grenzen van de berekening van machines voor de uiterste waarden die nodig zijn voor grootschalige structuur die verhoging van de schaalbare, draagbare en nauwkeurig.

Het wordt geleverd als een opdrachtregel zoals een R-bibliotheek.

U kunt voor het gebruik van deze bibliotheek in R start een interactieve sessie met R (gewoon door te typen **R** in de shell) en de bibliotheek.

Hier volgt een eenvoudig voorbeeld die u in de R-prompt uitvoeren kunt:

    library(xgboost)

    data(agaricus.train, package='xgboost')
    data(agaricus.test, package='xgboost')
    train <- agaricus.train
    test <- agaricus.test
    bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                    eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
    pred <- predict(bst, test$data)

Als u wilt uitvoeren op de opdrachtregel xgboost, vindt hier u de opdrachten uit te voeren in de shell:

    cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
    cd xgboostdemo
    xgboost mushroom.conf


Een .model-bestand naar de opgegeven map geschreven. Informatie over deze demo voorbeeld vindt u [op GitHub](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification).

Zie de [xgboost documentatiepagina](https://xgboost.readthedocs.org/en/latest/)en de [Github opslagplaats](https://github.com/dmlc/xgboost)voor meer informatie over xgboost.

#### <a name="rattle"></a>Rammelaar
Rammelaar (asily **E** **R** **A** **T**nalytical Inkthulpprogramma **T**o **L**verdienen) maakt gebruik van GUI-gegevens te verkennen en modellering. Deze statistische en visuele samenvattingen van gegevens, gegevens van transformaties die gemakkelijk kunnen worden gemodelleerd, zowel gecontroleerde als werkende modellen van de gegevens is gebaseerd, biedt de prestaties van modellen grafisch, presenteert en nieuwe gegevens scores worden ingesteld. R-code voor het repliceren van de bewerkingen in de gebruikersinterface dat kunnen worden uitgevoerd direct in R of gebruikt als uitgangspunt voor verdere analyse wordt ook gegenereerd.

Rammelaar uitvoeren, moet u in een grafisch bureaublad aanmelden sessie. Typ op de terminal, ```R``` in te voeren van de R-omgeving. Voer de volgende opdrachten bij de R-prompt:

    library(rattle)
    rattle()

Nu een grafische interface wordt geopend met een set tabbladen. Hier zijn de stappen snel starten in Rammelaar die nodig zijn voor een verzameling voorbeeldgegevens weer het bouwen van een model. In sommige van de onderstaande stappen wordt u gevraagd om automatisch te installeren en sommige vereiste R pakketten die nog niet op het systeem laden.

>[AZURE.NOTE] Als u geen toegang tot het pakket geïnstalleerd in de systeemmap (de standaardinstelling), wordt er een prompt op uw R consolevenster pakketten te installeren op uw persoonlijke bibliotheek. *Y* als u deze vragen worden beantwoord.

1. Klik op **uitvoeren**.
2. Een dialoogvenster verschijnt, waarin u wordt gevraagd als u graag de gegevensset voorbeeld weer gebruiken. Klik op **Ja** als u wilt laden in het voorbeeld.
3. Klik op het tabblad **Model** .
4. Klik op **uitvoeren** om het bouwen van een beslissingsstructuur.
5. Klik op **Tekenen** om de beslissingsstructuur weer te geven.
6. Klik op het keuzerondje **Forest** en **uitvoeren** voor het opbouwen van een willekeurige forest op.
7. Klik op het tabblad **evalueren** .
8. Klik op het keuzerondje **risico** en klikt u op **uitvoeren** om twee risico (cumulatief) prestaties waarnemingspunten weer te geven.
9. Klik op de tab **logboek** om de code genereren R voor de voorgaande bewerkingen weer te geven.
(Vanwege een fout in de huidige versie van de Rammelaar, hoeft u een *#* voor het *exporteren van dit logboek...* teken in de tekst van het logboek.)
10. Klik op de knop **exporteren** om het R-script-bestand met de naam *weather_script. R* naar de basismap.

U kunt afsluiten Rammelaar en R. U kunt nu het gegenereerde R script wijzigen of gebruiken als deze wordt uitgevoerd op elk gewenst moment herhalen alles dat is uitgevoerd in de gebruikersinterface van Rattle. Vooral voor beginners in R is dit een eenvoudige manier om snel analyses en leren in een eenvoudige grafische interface bij het automatisch genereren van code in R te wijzigen en/of informatie over de computer.


## <a name="next-steps"></a>Volgende stappen
Hier ziet u hoe u kunt blijven uw leren en verkennen:

* De [wetenschap gegevens op de Linux Data Science virtuele Machine](machine-learning-data-science-linux-dsvm-walkthrough.md) scenario wordt beschreven hoe u enkele algemene gegevens wetenschap taken uitvoeren met de Linux Data Science VM hier ingericht. 
* Verken de verschillende wetenschap hulpmiddelen voor gegevens op data science VM door het uitproberen van de hulpprogramma's die in dit artikel wordt beschreven. U kunt ook *dsvm-meer-info* uitvoeren op de houder binnen de virtuele machine voor een algemene inleiding en verwijzingen naar meer informatie over de hulpprogramma's geïnstalleerd op de VM.  
* Informatie over het analytische oplossingen voor end-to-end systematisch samenstellen met behulp van het [Team gegevens wetenschap proces](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).
* Ga naar de [Galerie met Cortana Analytics](http://gallery.cortanaanalytics.com) voor machine learning data analytics voorbeelden en die gebruikmaken van de Suite Cortana Analytics.
