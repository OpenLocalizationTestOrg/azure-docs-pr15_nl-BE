<properties
    pageTitle="Wat is een virtuele Machine voor Data Science? | Microsoft Azure"
    description="Informatie over belangrijke scenario's, functies, en hoe aan de slag met Data Science virtuele Machines, milieu en de toolkit klaar voor analytics."
    keywords="hulpmiddelen voor wetenschappelijke gegevens, gegevens wetenschap virtuele machine, hulpprogramma's voor wetenschappelijke gegevens, linux data science"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="bradsev" />


# <a name="introduction-to-the-cloud-based-data-science-virtual-machine-for-linux-and-windows"></a>Inleiding tot de cloud-gebaseerde Data Science virtuele Machine voor Linux en Windows

De virtuele Machine voor Data Science is een aangepaste installatiekopie VM op Microsoft Azure cloud speciaal gebouwd voor wetenschappelijke gegevens te doen. Heeft vele populaire gegevens wetenschap en andere hulpprogramma's die vooraf geïnstalleerd en vooraf geconfigureerd voor het snel bouwen van intelligente toepassingen voor geavanceerde analytics. Het is beschikbaar op Windows Server 2012 of op basis van OpenLogic 7.2 CentOS Linux-versies. 

In dit onderwerp wordt beschreven wat u kunt doen met de wetenschap gegevens VM, worden een aantal belangrijke scenario's voor het gebruik van de VM itemizes de belangrijkste functies die beschikbaar zijn op de versies van Windows en Linux en biedt instructies voor het werken met deze.


## <a name="what-can-i-do-with-the-data-science-virtual-machine"></a>Wat kan ik doen met de Data Science virtuele Machine?

Het doel van de gegevens wetenschap Virtual Machine is te voorzien van een wrijving vrij wetenschap gegevensomgeving gegevens professionals op alle niveaus van vaardigheden en rollen. Deze VM bespaart u veel tijd zou als u had een vergelijkbare omgeving op uw eigen uitgerold. In plaats daarvan uw project van de wetenschap gegevens onmiddellijk starten in een nieuwe VM-sessie. 

De wetenschap gegevens VM is ontworpen en geconfigureerd voor het werken met een brede gebruiksscenario's. U kunt uw omgeving schalen omhoog of omlaag als u het project moet wijzigen. U zijn kunt uw voorkeurstaal programma data science taken gebruiken. Als u andere hulpprogramma's installeert en het systeem aanpassen aan uw exacte behoeften beantwoorden.
 
## <a name="key-scenarios"></a>Belangrijke scenario 's
In deze sectie stelt enkele belangrijke scenario's waarvoor de wetenschap gegevens VM kan worden geïmplementeerd.

### <a name="preconfigured-analytics-desktop-in-the-cloud"></a>Vooraf geconfigureerde analytics desktop in de cloud

De wetenschap gegevens VM biedt een baseline configuratie voor data science teams die willen hun lokale bureaublad vervangen door een beheerde cloud desktop. Deze basislijn zorgt ervoor dat alle gegevens wetenschappers in een team een consistente installatie dat u hebben wilt controleren of experimenten en samenwerking te bevorderen. Ook verlaagt de kosten door de sysadmin belasting en het opslaan van de tijd die nodig is voor het evalueren, installeren en onderhouden van de diverse softwarepakketten die nodig zijn voor geavanceerde analytics doen.  

### <a name="data-science-training-and-education"></a>Data science opleiding en onderwijs

Enterprise opleiders en docenten leren die data science klassen bieden meestal een image van de virtuele machine om ervoor te zorgen dat hun leerlingen een consistente installatie hebben en de monsters volgens plan werken. De wetenschap gegevens VM maakt een omgeving op verzoek met een consistente installatie die de ondersteuning en incompatibiliteit uitdagingen vereenvoudigt. Gevallen waarin deze omgevingen moeten worden opgebouwd, vaak, vooral voor kortere cursussen, voordeel aanzienlijk.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Elastische capaciteit op verzoek voor grootschalige projecten.

Data science hackathons/wedstrijden of grootschalige gegevensmodellering en exploratie vereist schaalvergroting hardwarecapaciteit meestal voor korte duur. De wetenschap gegevens VM kunt repliceren van de omgeving van de wetenschap gegevens snel op verzoek, op schaal van servers die staan experimenten die volop computerbronnen moet worden uitgevoerd.

### <a name="short-term-experimentation-and-evaluation"></a>Op korte termijn experimenten en evaluatie

De wetenschap gegevens VM kan worden gebruikt om te evalueren of informatie over hulpprogramma's zoals Microsoft R Server, SQL Server, Visual Studio tools, Jupyter, diepe leren / ML toolkits en nieuwe hulpmiddelen die populair zijn in de Gemeenschap met minimale inspanning setup. Aangezien de wetenschap gegevens VM kan snel worden ingesteld, kan het worden toegepast in andere kortlopende gebruiksscenario's zoals repliceren gepubliceerde experimenten, demo's, het volgende scenario's in on line sessies of conferentie zelfstudies wordt uitgevoerd.


## <a name="whats-included-in-the-data-science-vm"></a>Wat opgenomen in de wetenschap gegevens VM?

De Data Science virtuele Machine heeft veel populaire wetenschap Gegevenshulpmiddelen al geïnstalleerd en geconfigureerd. Het bevat ook gereedschappen waarmee u gemakkelijk werken met diverse Azure data en analytics-producten. U kunt verkennen en voorspellende modellen bouwen op grote gegevenssets met behulp van de Server Microsoft R of met SQL Server 2016. Een groot aantal andere hulpprogramma's van de open-source Gemeenschap en van Microsoft zijn ook opgenomen, evenals voorbeelden van code en -laptops. De volgende tabel itemizes en vergelijkt de belangrijkste onderdelen opgenomen in de Windows- en Linux-versies van de Data Science Virtual Machine.


|**Windows-editie** | **Linux-editie** |
|----------------|---------------|
|Microsoft R Server Developer Edition | Microsoft R Server Developer Edition |
|Anaconda Python 2.7, 3,5-inch | Anaconda Python 2.7, 3,5-inch |
|Jupyter-laptop-Server (R, Python) | JupyterHub: Jupyter met meerdere gebruikers-laptops (R, Python, Julia) |
|Developer Edition van SQL Server 2016: Schaalbare in database analytics met R services | Postgres, eekhoorn SQL (Databasehulpprogramma), SQL Server-stuurprogramma's en vanaf de opdrachtregel (bcp, sqlcmd) |
|Visual Studio Community Edition 2015 (IDE) </br> -Azure HDInsight (Hadoop), gegevens-Lake, hulpprogramma's voor SQL Server-gegevens </br> -Gereedschappen voor Visual Studio Node.js, Python en R |IDEs en editors </br> -Met de invoegtoepassing voor Azure toolkit Eclips </br> -Emacs (met ESS, auctex) gedit |
|Power BI-desktop | -- |
|Leren van gereedschapswerktuigen </br> -Integratie met Azure Machine Learning </br> -CNTK (diep leren/AI) </br> -Xgboost (populaire ML gereedschap in data science wedstrijden) </br> -Wabbit Vowpal (snelle online cursist) </br> -Rattle (visuele quick start data en analytics tool) </br> -Mxnet (diep leren/AI) | Leren van gereedschapswerktuigen </br> -Integratie met Azure Machine Learning </br> -CNTK (diep leren/AI) </br> -Xgboost (populaire ML gereedschap in data science wedstrijden) </br> -Wabbit Vowpal (snelle online cursist) </br> -Rattle (visuele quick start data en analytics tool)  |
| SDK's Azure en Cortana Intelligence Suite van services | SDK's Azure en Cortana Intelligence Suite van services |
| Hulpmiddelen voor het verplaatsen van gegevens en het beheer van bronnen Azure en Big Data: Azure Opslagverkenner, CLI, PowerShell, AdlCopy (Lake Azure gegevens), AzCopy, dtui (voor DocumentDB), Microsoft Data Management Gateway | Hulpmiddelen voor het verplaatsen van gegevens en het beheer van bronnen Azure en Big Data: Azure Opslagverkenner, CLI |
| GIT, Visual Studio Team Services-invoegtoepassing | GIT |
| Windows-poort van de meest populaire Linux/Unix-hulpprogramma toegankelijk via GitBash/opdrachtprompt | -- |



## <a name="how-to-get-started-with-the-windows-data-science-vm"></a>Om te beginnen met de Windows Data Science VM

- Maak een instantie van VM in Windows door te navigeren naar [deze pagina](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) en de groene knop van de **virtuele Machine maken** .
- Log in om de VM van het externe bureaublad met de referenties die u hebt opgegeven bij het maken van de VM.
- Om te ontdekken en de hulpprogramma's te starten, klikt u op het menu **Start** .


## <a name="get-started-with-the-linux-data-science-vm"></a>Aan de slag met de Linux Data Science VM

- Maak een exemplaar van de VM op Linux (gebaseerd op CentOS OpenLogic) door te navigeren naar [deze pagina](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/) en de knop voor de **virtuele Machine maken** .
- Log in om de VM in een SSH-client zoals Putty of SSH opdracht met behulp van de referenties die u hebt opgegeven tijdens het maken van de VM.
- Typ in de shell-prompt dsvm-meer-info.
- Download de client X2Go voor uw client platform voor een grafische desktop [hier](http://wiki.x2go.org/doku.php/doc:installation:x2goclient) en volg de instructies in het document Linux Data Science VM [bepaling van de virtuele Machine van Linux Data Science](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).


## <a name="next-steps"></a>Volgende stappen

### <a name="for-the-windows-data-science-vm"></a>Voor de Windows Data Science VM

- Zie voor meer informatie over het uitvoeren van specifieke hulpmiddelen beschikbaar op de Windows-versie, [verstrekken Microsoft Data Science Virtual Machine](machine-learning-data-science-provision-vm.md) en
-  Zie voor meer informatie over het uitvoeren van verschillende taken die nodig zijn voor uw project van de wetenschap gegevens op de Windows-VM [tien dingen die u op de wetenschap gegevens virtuele Machine doen kunt](machine-learning-data-science-vm-do-ten-things.md).

### <a name="for-the-linux-data-science-vm"></a>Voor de wetenschappelijke gegevens Linux VM

- Zie voor meer informatie over het uitvoeren van specifieke hulpmiddelen beschikbaar op de Linux-versie, [dient de virtuele Machine van Linux Data Science](machine-learning-data-science-linux-dsvm-intro.md).
- Zie voor een scenario waarin wordt getoond hoe u verschillende algemene gegevens wetenschap uitvoert met de Linux VM, [wetenschappelijke gegevens op de Linux Data Science virtuele Machine](machine-learning-data-science-linux-dsvm-walkthrough.md).
