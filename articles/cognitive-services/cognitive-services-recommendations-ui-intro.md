<properties
    pageTitle="Bouwen van een model met de UI Recommnendations | Microsoft Azure"
    description="Azure Machine Learning aanbevelingen - bouwen van een model met de gebruikersinterface van aanbevelingen"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="luisca"/>

# <a name="building-a-model-with-the-recommendations-ui"></a>Bouwen van een model met de gebruikersinterface van aanbevelingen

Dit document is een stapsgewijze handleiding. Ons doel is u stapsgewijs door de noodzakelijke stappen voor een model met behulp van de [Gebruikersinterface van de aanbevelingen](https://recommendations-portal.azurewebsites.net/)van de trein.
Via de oefening kunt u inzicht in het proces voor het bouwen van een model voordat u een programma kunt doen. Het ook familiarizes u met de gebruikersinterface die is handig als u de service gaan gebruiken.

In deze oefening duurt ongeveer 30 minuten.

<a name="Step1"></a>
## <a name="step-1---sign-in-to-the-recommendations-ui"></a>Stap 1 - teken in de gebruikersinterface van de aanbevelingen ##

1. Als u dit nog niet hebt gedaan, moet u [aanmelden](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) voor een abonnement op nieuwe [API voor aanbevelingen](https://www.microsoft.com/cognitive-services/en-us/recommendations-api) . U kunt zich aanmelden voor de service op **Azure** op [http://portal.azure.com/](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) en meld u met uw account Azure. Gedetailleerde instructies voor het aanmeldingsproces worden geleverd in *taak 1* van de [Getting Started Guide](cognitive-services-recommendations-quick-start.md) 

1. Zodra u over een **sleutel** voor uw abonnement aanbevelingen-API, gaat u naar de [UI voor aanbevelingen](https://recommendations-portal.azurewebsites.net/). 

1. Meld u aan bij de portal door uw sleutel invoeren in het veld **Rekening sleutel** en klik op de knop **aanmelden** .

    ![Aanbevelingen UI:-In het dialoogvenster.][reco_signin]


<a name="Step2"></a>
## <a name="step-2---lets-gather-some-training-data"></a>Stap 2: verzamelen we bepaalde trainingsgegevens ##

Voordat u een build maken kunt, moet twee soorten informatie: een catalogusbestand en een set bestanden voor gebruik. 

Het catalogusbestand bevat informatie over de items die u aan uw klant aanbiedt. Een gebruik-bestand bevat informatie over hoe deze items worden gebruikt, of de transacties van uw bedrijf.

Gewoonlijk zou u uw database archief voor deze stukjes informatie opvragen. Vandaag, hebben we voorzien voorbeeldgegevens zodat u kunt informatie over het gebruik van de API voor aanbevelingen.

De gegevens kunt u downloaden van [http://aka.ms/RecoSampleData](http://aka.ms/RecoSampleData). Kopiëren en het **Books.Zip** -bestand uitpakken naar een map op uw lokale computer. Bijvoorbeeld **c:\data**.

In het [verzamelen van gegevens naar het Model van de trein](cognitive-services-recommendations-collecting-data.md) -artikel vindt u gedetailleerde informatie over het schema van de catalogus en het gebruik van bestanden.
 
Voor deze oefening gaan we werken met een klein bestand zodat u zeer niet hoeven te wachten te lang voor de training. Als u proberen een meer realistische bestand wilt, hebben we ook **MsStoreData.zip** met transacties monster uit de Microsoft Store op [dezelfde locatie](http://aka.ms/RecoSampleData)geplaatst.

<a name="Step3"></a>
## <a name="step-3---create-a-project-and-upload-catalog-and-usage-data"></a>Stap 3 - project maken en uploaden van gegevens en het gebruik van catalogus ##

Bij het aanmelden bij de [Aanbevelingen UI](https://recommendations-portal.azurewebsites.net/), ziet u de pagina projecten. Als u projecten eerder hebt gemaakt, ziet u deze hier.
Een project (ook bekend als *een model* in de API reference) is een container voor uw catalogus en het gebruik van gegevens. U kunt verschillende *maakt* binnen het project. We begeleidt u door het proces in de volgende stappen.

1. Als u wilt een nieuw project maakt, typt u de naam op in het tekstvak (iets zoals "MyFirstModel" zou werken) en klik op **Project toevoegen**.
 
    ![Aanbevelingen UI: Projectpagina.][reco_projects]

1. Wanneer het project wordt gemaakt, klikt u op de knop **Bladeren naar het bestand** in de sectie **een catalogusbestand toevoegen** . Uploaden van de catalogus die u in stap 2 hebt gekregen. Als u deze op *c:\data*hebt opgeslagen, moet u om naar die map te navigeren.

    ![Aanbevelingen UI: Gegevens toevoegen aan een Project.][reco_firstmodel]

1. Nadat de catalogus is geüpload, klikt u op de knop **Bladeren naar het bestand** in de sectie **Gebruik bestanden toevoegen** . Voeg het bestand usage_large.txt.

> **Wat gebeurt er als ik een groot bestand van gebruiksgegevens hebben?**
>
> Alleen gebruik van bestanden die kleiner zijn dan 200 MB u kunt uploaden. Dat kunt het systeem opgewassen zijn tegen ter waarde van 2 GB van gegevens, zodat u kunt meer dan één bestand uploaden indien nodig.
> Wellicht even zoveel gegevens voor het genereren van een goed model, is niet alleen de grootte van de gegevens die van belang, maar de kwaliteit van de gegevens. Het is gebruikelijk voor een overzicht van gebruiksgegevens waar de meeste van de transacties zijn slechts in een handvol populaire objecten en er is 'wat het signaal' voor andere artikelen.

<a name="Step4"></a>
## <a name="step-4---lets-do-some-training"></a>Stap 4: training gaan we doen! ##

Nu dat u zowel de catalogus als de gebruiksgegevens hebt geüpload, gaan we de engine trainen zodat deze meer patronen uit onze gegevens.

1.  Klik op de knop **Nieuw opbouwen** .

1.  Selecteer het build-type **aanbevelingen** . Merk op dat wij een rangorde bouwen ondersteunen en (vaak gekocht bij elkaar) een FBT bouwen ook typen.

    ![Aanbevelingen UI: Dialoogvenster samenstellen.][reco_build_dialog.png]


    Een versie FBT kunt u patronen voor producten die gewoonlijk gekocht/verbruikt bij dezelfde transactie zijn te herkennen.
    Een rangschikking build wordt gebruikt voor het omgaan met functies van belang. 
    We won't go erg diep in FBT of rangorde in deze workshop maakt, maar als u geïnteresseerd bent moet je eens [type en model kwaliteit documentatiepagina bouwen](cognitive-services-recommendations-buildtypes.md).

1. Klik op de knop **Opbouwen** . Het bouwproces duurt ongeveer vijf minuten als u de gegevens van de boeken. Het duurt langer voor grotere datasets.

<a name="Step5"></a>
## <a name="step-5---lets-find-out-what-the-machine-learned"></a>Stap 5 - we weten wat de machine geleerd! ##

Zodra uw build is voltooid, ziet u een nieuwe build in de lijst builds. Deze versie kan worden doorzocht voor object- en aanbevelingen.

1. Zodra uw build is voltooid, klikt u op de **Score**. Hiermee kunt u spelen met het model en zien welke items worden aanbevolen.

    ![Aanbevelingen UI: Score-knop][reco_score_button]

1. Selecteer een item om te zien welke items worden geretourneerd als de aanbevelingen voor het artikel. Merk op dat als er niet genoeg transacties te voorspellen een aanbeveling voor een bepaald artikel, het systeem niet eventuele aanbevelingen voor het artikel retourneren zal.  Als om een bepaalde reden u een artikel geen aanbevelingen hebt retourneert, kunt u andere items scoren.

<a name="Step6"></a>
## <a name="step-6---next-steps"></a>Stap 6 - Vervolgstappen ##
Gefeliciteerd! u hebt een model zijn opgeleid en vervolgens aanbevelingen van dat model hebt gekregen.  De gebruikersinterface van aanbevelingen is een handig hulpprogramma waarmee u kunt u de status van uw projecten en maakt. 

Nu dat u een model hebt, kunt u meer informatie over het programmatisch doen van de bovenstaande stappen. API aan te roepen de programmatisch meer, nodigen u uit om te controleren de [Aanbevelingen API Reference](http://go.microsoft.com/fwlink/?LinkId=759348) en de [Voorbeeldtoepassing aanbevelingen](http://go.microsoft.com/fwlink/?LinkID=759344)downloaden.


[reco_signin]:../media/cognitive-services/reco_signin.PNG
[reco_projects]:../media/cognitive-services/reco_projects.PNG
[reco_firstmodel]:../media/cognitive-services/reco_firstmodel.png
[reco_build_dialog.png]:../media/cognitive-services/reco_build_dialog.png
[reco_score_button]:../media/cognitive-services/reco_score_button.png
