<properties
    pageTitle="Wetenschap virtuele machines Data in Azure | Microsoft Azure"
    description="Instellen van een Data Science virtuele Machine"
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
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="data-science-virtual-machines-in-azure"></a>Wetenschap virtuele machines Data in Azure

Instructies zijn hier beschikbaar die wordt beschreven hoe u een Azure VM en een Azure VM met SQL-Service instellen als laptop IPython-servers. De virtuele Windows-computer is geconfigureerd met hulpmiddelen zoals IPython-laptop, Azure Opslagverkenner AzCopy en andere hulpprogramma's die gebruikt voor data science projecten worden ondersteunen. Azure Opslagverkenner en AzCopy, bijvoorbeeld handige manieren gegevens uploaden naar Azure opslag van uw lokale computer of op de lokale computer downloaden vanaf de opslag te bieden. 

Dit bevat menukoppelingen naar onderwerpen over het instellen van de verschillende data science omgevingen die worden gebruikt door het [Team gegevens wetenschap proces (TDSP)](data-science-process-overview.md).

[AZURE.INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

Verschillende soorten Azure virtuele machines kunnen worden ingericht en geconfigureerd om te worden gebruikt als onderdeel van een wetenschap gegevens cloud-gebaseerde omgeving. De beslissing over welke versie van de virtuele machine te gebruiken is afhankelijk van het type en de hoeveelheid gegevens die worden gemodelleerd met machine learning en het doelgebied voor gegevens in de cloud. 

* Zie voor instructies over de vragen bij deze beslissing, [Van plan bent uw Azure Machine Data Science leeromgeving](machine-learning-data-science-plan-your-environment.md). 
* Zie [scenario's voor de technologie in Azure Machine Learning en geavanceerde Analytics proces](machine-learning-data-science-plan-sample-scenarios.md) voor een catalogus van sommige van de scenario's die u tegenkomen kunt bij het uitvoeren van geavanceerde analytics

Twee sets van instructies zijn beschikbaar:

* [Instellen van een Azure virtuele machine als een server IPython-laptops voor geavanceerde analytics](machine-learning-data-science-setup-virtual-machine.md) laat zien hoe een Azure VM inrichten met IPython laptop en andere hulpprogramma's gebruikt voor wetenschappelijke gegevens voor de gevallen waarin een formulier dan SQL Azure-opslag kan worden gebruikt voor het opslaan van de gegevens.

* [Instellen van een virtuele machine van Azure SQL Server als een server IPython-laptops voor geavanceerde analytics](machine-learning-data-science-setup-sql-server-virtual-machine.md) laat zien hoe een virtuele machine van Azure SQL Server inrichten met IPython laptop en andere hulpprogramma's gebruikt voor wetenschappelijke gegevens voor de gevallen waarin een SQL-database kan worden gebruikt voor het opslaan van de gegevens.

Eenmaal ingericht en geconfigureerd, zijn deze virtuele machines klaar voor gebruik als laptop IPython-servers voor de exploratie en verwerking van gegevens en voor andere taken die nodig zijn in combinatie met Azure Machine Learning en Team gegevens wetenschap proces (TDSP). De volgende stappen in het proces van de wetenschap gegevens worden toegewezen in het [pad van TDSP leren](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) en stappen die gegevens naar een SQL Server- of HDInsight, verwerken en het monster in de voorbereiding op het leren van de gegevens met de computer leren werken met Azure kunnen bevatten.


> [AZURE.NOTE] Azure virtuele Machines worden geprijsd als u **betaalt alleen voor wat u gebruikt**. Om ervoor te zorgen dat u bent niet in rekening wordt gebracht wanneer de virtuele machine niet gebruikt, is deze in staat van de [Klassieke Portal Azure](http://manage.windowsazure.com/) **gestopt (Deallocated)** . Zie voor stapsgewijze instructies of hoe u de virtuele machine toewijzing [afsluiten en de toewijzing van de virtuele machine wanneer niet in gebruik](machine-learning-data-science-setup-virtual-machine.md#shutdown)
 
