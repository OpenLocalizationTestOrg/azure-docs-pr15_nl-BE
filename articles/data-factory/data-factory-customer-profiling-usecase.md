<properties 
    pageTitle="Use-Case - klant profilering" 
    description="Informatie over hoe Azure Data Factory wordt gebruikt voor het maken van gegevensgestuurde werkstromen (pijpleiding) profiel gaming-klanten." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="shlo"/>

# <a name="use-case---customer-profiling"></a>Use-Case - klant profilering

Azure Data Factory is een van de vele services gebruikt voor het implementeren van de Cortana Intelligence Suite solution accelerators.  Bezoek voor meer informatie over Cortana Intelligence [Cortana Intelligence Suite](http://www.microsoft.com/cortanaanalytics). In dit document beschrijven we een eenvoudige use-case zodat u aan de slag met begrijpen hoe Azure Data Factory analytics problemen kunt oplossen.

Alles wat u nodig hebt voor toegang tot en het uitproberen van deze eenvoudige use-case is een [abonnement op Azure](https://azure.microsoft.com/pricing/free-trial/).  U kunt een voorbeeld waarin deze use-case is geïmplementeerd door de stappen in het artikel [voorbeelden](data-factory-samples.md) implementeren.

## <a name="scenario"></a>Scenario

Contoso is een gaming bedrijf dat wordt gemaakt van spellen voor meerdere platforms: game-consoles en handheld apparaten, personal computers (pc's). Spelers spelen deze games, wordt grote hoeveelheid gegevens geleverd die bijhoudt van de gebruikspatronen, gaming, stijl en voorkeuren van de gebruiker.  In combinatie met demografische, regionale, en productgegevens, Contoso analytics begeleidt ze over het verbeterde ervaring spelers kunt uitvoeren en doel hen voor upgrades en het spel koopt. 

Doelstelling van Contoso is het identificeren van up-sell/cross-sell-opportunities op basis van de gaming geschiedenis van de spelers en aantrekkelijke functies toevoegen aan de bedrijfsgroei en bieden een betere ervaring voor klanten. We gebruiken een gaming bedrijf als een voorbeeld van een bedrijf voor deze use-case. Het bedrijf wil de spellen op basis van het gedrag van de spelers te optimaliseren. Deze beginselen zijn van toepassing op elk bedrijf dat wil haar klanten over de goederen en diensten te verrichten en hun klanten ervaring te verbeteren.

## <a name="challenges"></a>Uitdagingen


## <a name="solution-overview"></a>Overzicht van oplossingen

Deze eenvoudige gebruiksvoorbeeld kan worden gebruikt als een voorbeeld van het gebruik van Azure Data Factory te nemen, voorbereiden, transformeren, analyseren en publiceren van gegevens.

![End-to-end-werkstroom](./media/data-factory-customer-profiling-usecase/EndToEndWorkflow.png)

In deze afbeelding ziet hoe de pijpleidingen gegevens worden weergegeven in de portal Azure nadat deze is geïmplementeerd.

1.  Het **PartitionGameLogsPipeline** leest de ruwe spel gebeurtenissen van blob-opslag en maakt u partities op basis van een jaar, maand en dag.
2.  De **EnrichGameLogsPipeline** joins gepartitioneerde spel gebeurtenissen met referentiegegevens geo-code en verrijkt de gegevens door de IP-adressen toewijzen aan de bijbehorende geo-locaties.
3.  De pijpleiding **AnalyzeMarketingCampaignPipeline** verrijkte gegevens gebruikt en verwerkt deze met de gegevens van de reclame voor het maken van de uiteindelijke uitvoer waarin de effectiviteit van marketing campagne.

In dit voorbeeld wordt de Data Factory goedkeuringen door activiteiten die invoergegevens, transformatie en verwerken van de gegevens kopiëren en de definitieve gegevens uitvoeren naar Azure SQL-Database gebruikt.  U kunt het netwerk van pijpleidingen gegevens visualiseren, beheren en controleren van de status van de gebruikersinterface.

## <a name="benefits"></a>Voordelen

Door de gebruiker profiel analytics optimaliseren en uitlijnen aan de bedrijfsdoelstellingen, kan gaming bedrijf snel gebruikspatronen te verzamelen en analyseren van de effectiviteit van marketingcampagnes.




