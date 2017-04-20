<properties 
    pageTitle="Data Factory Use-Case - aanbevelingen" 
    description="Meer informatie over een use-case geïmplementeerd via Azure Data Factory samen met andere services." 
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
    ms.date="09/01/2016" 
    ms.author="shlo"/>

# <a name="use-case---product-recommendations"></a>Use-Case - aanbevelingen 

Azure Data Factory is een van de vele services gebruikt voor het implementeren van de Cortana Intelligence Suite solution accelerators.  Zie [Cortana Intelligence Suite](http://www.microsoft.com/cortanaanalytics) pagina voor meer informatie over dit pakket. Een gemeenschappelijke use-case die Azure gebruikers al hebben opgelost en geïmplementeerd met behulp van Azure Data Factory en andere Cortana Intelligence componentservices worden beschreven in dit document.

## <a name="scenario"></a>Scenario

Online winkels willen meestal ertoe aanzetten hun klanten producten te kopen door presentaties producten zijn waarschijnlijk in geïnteresseerd bent en dus waarschijnlijk te kopen. Om dit te bereiken, moeten on line winkels hun online gebruikerservaring aanpassen met persoonlijke aanbevelingen voor die specifieke gebruiker. Deze persoonlijke aanbevelingen moeten worden gemaakt op basis van hun huidige en historische gegevens gedrag, productinformatie, nieuwe merken en product- en klantenservices segmentatiegegevens winkelen.  Daarnaast kunnen ze bieden de gebruiker aanbevelingen op basis van de analyse van het totale gebruik van gedrag van de gebruikers die zijn gecombineerd.

Het doel van deze winkels is te optimaliseren voor de gebruiker klikt op verkoop conversies en hogere omzet te behalen.  Zij bereiken deze conversie door contextuele, gedrag gebaseerde aanbevelingen op basis van de belangen van de klant en acties te leveren. Voor deze use-case we online winkels gebruiken als een voorbeeld van bedrijven die u wilt optimaliseren voor hun klanten. Deze beginselen zijn evenwel van toepassing op elk bedrijf dat wil haar klanten over de goederen en diensten te verrichten en hun klanten koopervaring met persoonlijke aanbevelingen te verbeteren.

## <a name="challenges"></a>Uitdagingen

Er zijn veel uitdagingen die online winkels vlak bij het implementeren van dit type van use-case. 

Gegevens van verschillende vormen en maten moet eerst worden ingenomen uit meerdere gegevensbronnen, zowel op gebouwen en in de cloud. Deze gegevens bevatten productgegevens, gedrag van historische klantgegevens en gegevens van de gebruiker als de gebruiker de online retail site bladert. 

Tweede, persoonlijke aanbevelingen moeten redelijk en nauwkeurig berekend en voorspeld. Naast de product-, merk- en klantgegevens gedrag en de browser moeten online winkels ook feedback van eerdere aankopen te houden bij de bepaling van de beste aanbevelingen voor de gebruiker. 

Ten derde moet de aanbevelingen onmiddellijk aan de gebruiker een naadloze surfen inkopen en ervaring, en bieden de meest recente en relevante aanbevelingen product. 

Ten slotte moeten de detailhandelaren te meten van de effectiviteit van hun benadering door bij te houden van de algehele Upsell en klik op conversie verkoop successen cross-sell en aanpassen aan hun toekomstige aanbevelingen.

## <a name="solution-overview"></a>Overzicht van oplossingen

In dit voorbeeld van de use-case is opgelost en geïmplementeerd door gebruikers reële Azure met Azure Data Factory en andere Cortana Intelligence componentservices, met inbegrip van [HDInsight](https://azure.microsoft.com/services/hdinsight/) en [Power BI](https://powerbi.microsoft.com/).

De online detailhandelaar gebruikt een Azure Blob-winkel, een SQL-server op locatie, Azure SQL DB en een relationele datamart als de opties voor het opslaan van gegevens in de werkstroom.  De blob-opslag bevat klantgegevens, gedrag van klantgegevens en product informatie gegevens. De product informatie gegevens bevat productinformatie merk en opgeslagen op-gebouw in een SQL-datawarehouse van een product catalogus. 

Alle gegevens gecombineerd en ingevoerd in een product aanbeveling systeem voor het leveren van persoonlijke aanbevelingen op basis van de belangen van de klant en acties, terwijl de gebruiker producten in de catalogus op de website bladert. De klanten zien ook producten die zijn gerelateerd aan het product dat ze kijken op basis van algemene website gebruikspatronen die niet zijn gerelateerd aan een gebruiker een.

![use-case-diagram](./media/data-factory-product-reco-usecase/diagram-1.png)

Gigabyte van ruwe web logboekbestanden worden dagelijks gegenereerd vanaf de website van de online detailhandelaar als semi-gestructureerde bestanden. De ruwe web logboekbestanden en de klant en product catalogus wordt regelmatig ingenomen in een Azure Blob-opslag met behulp van Data Factory wereldwijd gedistribueerde gegevens verplaatsen als een service. De ruwe logboekbestanden voor de dag zijn gepartitioneerd (per jaar en maand) in een blob-opslag voor langere termijn opgeslagen.  [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) wordt gebruikt voor het partitioneren van ruwe logboekbestanden in de blob-opslag en verwerken de geconsumeerde logboeken op schaal met behulp van scripts voor component- en varken. De gepartitioneerde web gegevens geregistreerd wordt vervolgens verwerkt om uit te pakken van de benodigde invoer voor een machine learning aanbeveling systeem voor het genereren van de persoonlijke aanbevelingen.

De aanbeveling systeem dat wordt gebruikt voor de computer leren in dit voorbeeld is een open-source machine aanbeveling platform van [Apache Mahout](http://mahout.apache.org/)leren.  Alle [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) of aangepaste model kan worden toegepast op het scenario.  De Mahout-model wordt gebruikt om te voorspellen van de overeenkomsten tussen de items op de website op basis van algemene gebruikspatronen en genereert u de persoonlijke aanbevelingen op basis van de individuele gebruiker.

Ten slotte wordt de resultaatset van persoonlijke aanbevelingen verplaatst naar een relationele datamart voor consumptie door de website van de leverancier.  De resultaatset kan ook rechtstreeks toegankelijk vanuit blob-opslag door een andere toepassing of naar extra winkels voor andere consumenten en use-cases verplaatst.

## <a name="benefits"></a>Voordelen

Door het optimaliseren van hun product aanbeveling strategie en uitlijnen aan de bedrijfsdoelstellingen, de oplossing aan de online detailhandelaar merchandising en marketing doelstellingen voldaan. Bovendien konden zij mogelijk maken en beheren van de workflow van de aanbeveling product op een efficiënte, betrouwbare en efficiënte wijze. De aanpak van gemaakt om hun model bijwerken en de doeltreffendheid ervan op basis van de maatregelen van verkoop klik naar conversie successen aan te passen. Met behulp van Azure Data Factory, konden ze verlaten hun tijdrovend en kostbaar handmatige wolk resourcemanagement en gaat u naar de wolk op verzoek resourcemanagement. Dus konden ze bespaart tijd, geld, en het verminderen van hun tijd aan de implementatie van de oplossing. Lineage gegevensweergaven en operationele service gezondheid werd gemakkelijk te visualiseren en oplossen met de intuïtieve Data Factory-monitoring en beheer UI is beschikbaar vanaf de portal Azure. Hun oplossing kan nu worden gepland en beheerd zodat voltooide data op betrouwbare wijze wordt geproduceerd en geleverd aan gebruikers en gegevens en de verwerking van afhankelijkheden automatisch worden beheerd zonder menselijke tussenkomst.

Door middel van deze persoonlijke ervaring, de online winkel gemaakt van een meer concurrerende en boeiendere klant optreden en daarom verkoop- en algemene klanttevredenheid verhogen.



  