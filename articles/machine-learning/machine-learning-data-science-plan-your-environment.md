<properties
    pageTitle="Het identificeren van scenario's en plannen voor geavanceerde analytics gegevensverwerking | Microsoft Azure"
    description="Geavanceerde analytics plannen op basis van een reeks belangrijke vragen."
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
    ms.date="09/19/2016"
    ms.author="bradsev" />


# <a name="how-to-identify-scenarios-and-plan-for-advanced-analytics-data-processing"></a>Het identificeren van scenario's en plan voor de verwerking van geavanceerde analytics-gegevens

Welke bronnen moet u van plan bent om op te nemen bij het instellen van een omgeving voor geavanceerde analytics op een dataset verwerken? Dit artikel bevat een reeks vragen waarmee de taken en resources relevante identificeren uw scenario. De volgorde van stappen op hoog niveau voor anticiperende analytics geeft een overzicht van [Wat is het Team gegevens wetenschap proces (TDSP)?](data-science-process-overview.md). Elk van deze stappen vereist specifieke resources voor de taken die relevant zijn voor uw specifieke scenario. De belangrijke vragen voor uw scenario heeft betrekking op gegevens logistiek, kenmerken, de kwaliteit van de datasets, en de hulpprogramma's en talen die u wilt doen van de analyse.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="logistic-questions-data-locations-and-movement"></a>Logistieke vragen: data, locaties en beweging
De logistieke vragen betrekking hebben op de locatie van de **gegevensbron**, de **bestemming als doel** in Azure en vereisten voor het verplaatsen van de gegevens, inclusief de planning, het bedrag en de betrokken middelen. De gegevens wellicht meerdere malen tijdens de analytics worden verplaatst. Een gebruikelijk scenario is lokale gegevens verplaatsen naar een vorm van opslag van Azure en vervolgens in de Machine Learning Studio.

1. **Wat is uw gegevensbron?** Is het lokale of in de cloud? Bijvoorbeeld:
    - De gegevens is op een HTTP-adres openbaar.
    - De gegevens bevinden zich in een lokaal/netwerk locatie.
    - De gegevens zijn in een SQL Server-database.
    - De gegevens worden opgeslagen in een container Azure opslag

2. **Wat is de Azure bestemming?** Waar deze nodig heeft voor het verwerken of modeling? Bijvoorbeeld:
    - Azure Blob-opslag
    - Azure SQL-databases
    - SQL Server op Azure VM
    - HDInsight (Hadoop op Azure) of component tabellen
    - Azure Machine Learning
    - Koppelbaar Azure virtuele harde schijven.

3. **Hoe gaat u om de gegevens te verplaatsen?** De procedures en bronnen die beschikbaar zijn op te nemen gegevens laden in een groot aantal verschillende opslag en verwerking omgevingen worden beschreven in de volgende onderwerpen.

    -  [Gegevens laden in omgevingen met opslag voor analytics](machine-learning-data-science-ingest-data.md)
    -  Het [importeren van uw trainingsgegevens naar Azure Machine Learning Studio uit verschillende gegevensbronnen](machine-learning-data-science-import-data.md).

4. **Hoeft de gegevens worden regelmatig verplaatst of gewijzigd tijdens de migratie?** Overweeg het gebruik van Azure Factory (ADF) wanneer gegevens voortdurend worden gemigreerd moeten, met name als een hybride scenario die toegang heeft tot zowel lokale bronnen wolk is betrokken en wanneer de gegevens is verwerkt of moet worden gewijzigd of bedrijfslogica toegevoegd tijdens de migratie. Zie voor meer informatie, [gegevens van een lokale SQL server SQL Azure met Azure Data Factory](machine-learning-data-science-move-sql-azure-adf.md)

5. **Hoeveel van de gegevens moet worden verplaatst naar Azure?** DataSets die zeer groot mag hoger zijn dan de opslagcapaciteit van bepaalde omgevingen. Zie het onderwerp over de maximale grootte voor Machine Learning Studio in de volgende sectie voor een voorbeeld. In dergelijke gevallen kan een voorbeeld van de gegevens tijdens de analyse worden gebruikt. Zie voor meer informatie over hoe u een dataset in diverse omgevingen met Azure down-monster, [voorbeeldgegevens in het Team gegevens wetenschap proces](machine-learning-data-science-sample-data.md).


## <a name="data-characteristics-questions-type-format-and-size"></a>Gegevens kenmerken vragen: type, de opmaak en grootte
Deze vragen vormen de sleutel tot de planning van uw opslag en verwerking van omgevingen die geschikt zijn voor verschillende soorten gegevens en die elk bepaalde beperkingen hebben.

1. **Wat zijn de gegevenstypen?** Bijvoorbeeld:
    - Numerieke
    - Bestaan
    - Tekenreeksen
    - Binaire

2. **Hoe is uw gegevens opgemaakt?** Bijvoorbeeld:
    - Door komma's gescheiden (CSV) of tabs gescheiden (TSV) bestanden
    - Wel of niet gecomprimeerd
    - Azure BLOB 's
    - Hadoop component tabellen
    - SQL Server-tabellen

2. **Hoe groot is uw gegevens?**
    - Klein: minder dan 2GB
    - Normaal: Meer dan 2GB en minder dan 10GB
    - Groot: Groter zijn dan 10GB

Neem bijvoorbeeld de Azure Machine Learning Studio-omgeving:

- Voor een overzicht van de gegevensindelingen en typen die worden ondersteund door Azure Machine Learning Studio, Zie de sectie [-Opmaak en -gegevenstypen worden ondersteund](machine-learning-data-science-import-data.md#data-formats-and-data-types-supported) .
- Zie voor informatie over beperkingen voor Azure Machine Learning Studio, de **hoe groot de gegevensset is voor mijn modules?** gedeelte van het [importeren en exporteren van gegevens voor Machine Learning](machine-learning-faq.md#machine-learning-studio-questions)

Zie voor informatie over de beperkingen van andere Azure services gebruikt in het proces van analytics, [Azure abonnement Service grenzen, quota's en beperkingen](../azure-subscription-service-limits.md).

## <a name="data-quality-questions-exploration-and-pre-processing"></a>Data kwaliteit vragen: exploratie en voorbehandeling

1. **Wat weet u over uw gegevens?** Gegevens verkennen als u wilt krijgen een begrijp wat de fundamentele kenmerken. Wat patronen of trends deze voorwerpen, wat uitschieters heeft of het aantal waarden ontbreken. Deze stap is belangrijk voor het bepalen van de omvang van de voorbehandeling nodig is voor het formuleren van hypothesen die kunnen de meest geschikte functies voorgesteld of typ van analyse en voor het formuleren van plannen om extra gegevens te verzamelen. Beschrijvende statistiek berekenen en uitzetten van visualisaties zijn nuttige technieken voor de controle van de gegevens. Zie [gegevens in het Team gegevens wetenschap proces verkennen](machine-learning-data-science-explore-data.md)voor meer informatie over het verkennen van een dataset in diverse omgevingen met Azure.

2. **Hoeft de gegevens vooraf of reinigen?**
Vooraf en schoonmaken van de gegevens zijn belangrijke taken die normaal gesproken moeten worden uitgevoerd voordat de dataset effectief kan worden gebruikt voor de machine learning. Onbewerkte gegevens is vaak veel ruis en onbetrouwbare en waarden ontbreekt. Voor modellering met behulp van deze gegevens kan misleidende resultaten opleveren. Zie voor een beschrijving van [taken voorbereiden van gegevens voor verbeterde machine leren](machine-learning-data-science-prepare-data.md).

## <a name="tools-and-languages-questions"></a>Tools en talen vragen
Er zijn vele opties die hier zijn afhankelijk van welke talen en ontwikkelomgevingen of extra u nodig hebt of gebruik van de meest conformable zijn.

1. **Welke talen hebt u een voorkeur voor analyse?**  
    - R
    - Python
    - SQL

2. **Welke hulpmiddelen moet u gebruiken voor gegevensanalyse**
    - [Microsoft Azure Powershell](powershell-install-configure.md) - een scripttaal die wordt gebruikt voor het beheren van uw Azure resources in een scripttaal.
    - [Azure Machine Learning Studio](machine-learning-what-is-ml-studio/)
    - [Analytics revolutie](http://www.revolutionanalytics.com/revolution-r-open)
    - [RStudio](http://www.rstudio.com)
    - [Python-hulpprogramma's voor Visual Studio](http://microsoft.github.io/PTVS/)
    - [Anaconda](https://www.continuum.io/why-anaconda)
    - [Jupyter-laptops](http://jupyter.org/)
    - [Microsoft Power BI](http://powerbi.microsoft.com)


## <a name="identify-your-advanced-analytics-scenario"></a>Identificeren van uw scenario geavanceerde analytics
Nadat u de vragen in de vorige sectie hebt beantwoord, bent u klaar om te bepalen welk scenario beste past bij uw aanvraag. De monster scenario's worden beschreven in [scenario's voor geavanceerde analytics in Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md).
