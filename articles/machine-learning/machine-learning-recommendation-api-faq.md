<properties 
    pageTitle="Instellen en gebruiken van de Machine Learning aanbevelingen API | Microsoft Azure" 
    description="Microsoft-aanbevelingen API gebouwd met Azure Machine Learning Veelgestelde vragen" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/> 

#<a name="setting-up-and-using-machine-learning-recommendations-api-faq"></a>Instellen en gebruiken van Machine Learning aanbevelingen API-Veelgestelde vragen


**Wat is aanbevelingen?**

>[AZURE.NOTE]U moet beginnen met de cognitieve aanbevelingen API-Service in plaats van deze versie. De cognitieve Service aanbevelingen zal deze service worden vervangen en de nieuwe functies zal er worden ontwikkeld. Het heeft nieuwe mogelijkheden, zoals batchen ondersteuning, een betere API Explorer, een duidelijkere API oppervlak, consistenter aanmelden/facturering ervaring, enz.
> Meer informatie over het [migreren naar de nieuwe cognitieve Service](http://aka.ms/recomigrate)

AANBEVELINGEN in Azure Machine Learning biedt voor organisaties en bedrijven die gebaseerd op aanbevelingen zijn tot cross-sell en up-sell producten en diensten aan hun klanten, een engine zelf aanbevelingen. Dit is een implementatie van collaborative filtering die factoriseren matrix als haar core-algoritme gebruikt. Ontwikkelaars van toepassingen kunnen toegang tot aanbevelingen met REST API's. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Wat kan ik doen met de aanbevelingen**

AANBEVELINGEN als invoer van een item of een set items en geeft als resultaat een lijst met relevante aanbevelingen. Bijvoorbeeld: een klant van een on line fotozaak op een product klikt. De online detailhandelaar verzendt dat product als input voor de aanbevelingen in ruil met deze eigenschap wordt een lijst met producten en beslist welke van deze producten aan de klant wordt weergegeven. U kunt u aanbevelingen voor het optimaliseren van uw online winkel of zelfs in kennis te stellen van de binnenkant verkoop afdeling of call center.

**Gelden er beperkingen gebruik?**

Aanbevelingen heeft de volgende syntaxis beperkingen:
* Maximum aantal modellen per abonnement: 10
* Maximum aantal items in een catalogus kan bevatten: 100.000
* Het maximum aantal punten gebruik worden bewaard is ~ 5,000,000. De oudste wordt verwijderd als u nieuwe bestanden worden geüpload of gerapporteerd.
* Maximale grootte van gegevens die kunnen worden verzonden in een e-mailadres (bijvoorbeeld catalogusgegevens importeren, gegevens importeren) is 200 MB
* Het aantal transacties per seconde (TPS) voor een model build van aanbevelingen die niet actief is ~ 2 TPS. Een model build van aanbevelingen die actief kan maximaal 20 TPS bevatten.

##<a name="purchase-and-billing"></a>Inkoop en facturering 


**Wat kost aanbevelingen tijdens de periode starten?**

Aanbevelingen is een abonnement gebaseerde service. Opladen is gebaseerd op het aantal transacties per maand. U kunt controleren [aanbiedingspagina] (https://datamarket.azure.com/dataset/amla/recommendations) in Microsoft Azure marktplaats voor prijsinformatie.

**Zijn er eventuele kosten die zijn gekoppeld aan de aanbevelingen die volgen en opslaan van gebruikersactiviteiten voor mij?**

Niet op het moment.

**Heeft aanbevelingen een gratis proefversie**

Er is een gratis spoor die beperkt tot 10.000 transacties per maand is.

**Wanneer ik een rekening krijgt voor aanbevelingen?**

Een betaald abonnement is een abonnement waarvoor een maandelijkse vergoeding is. Wanneer u een betaalde licentie aanschaft, wordt u direct betalen voor gebruik van de eerste maand. U het bedrag dat is gekoppeld aan de aanbieding op de pagina abonnement (plus toepasbare belastingen) in rekening worden gebracht. Deze maandelijkse kosten is er elke maand op dezelfde datum als de oorspronkelijke inkoop agenda totdat u het abonnement annuleren. 

**Hoe voer ik een upgrade naar een hoger niveau?**

U kunt kopen of je abonnement op [aanbiedingspagina] bijwerken pagina op Microsoft Azure Marketplace (https://datamarket.azure.com/dataset/amla/recommendations).

Wanneer u een upgrade uitvoert een abonnement:

* Transacties die op uw oude abonnement nog niet toegevoegd aan het nieuwe abonnement. 
* Je volledige prijs betalen voor het nieuwe abonnement ook al u hebt ongebruikte transacties op uw oude abonnement.

Proces van een abonnement:

* Nevigate [aanbieding pagina] (https://datamarket.azure.com/dataset/amla/recommendations).
* Log in op de markt als u nog niet heeft aangemeld.
* In het rechterdeelvenster staan alle beschikbare plannen. Klik op het keuzerondje voor het plan dat u upgraden wilt naar.
* Als u bijwerken wilt, klikt u op **OK**. Als u niet bijwerken wilt, klikt u op **Annuleren**.

**Belangrijk** Het dialoogvenster zorgvuldig lezen voordat u de upgrade omdat er gevolgen voor facturering en het gebruik.

**Wanneer wordt mijn abonnement om aanbevelingen te eindigen?**

Uw abonnement wordt beëindigd wanneer u deze annuleren. Zie de volgende instructies als u wilt annuleren uw abonnementen.

**Hoe annuleer ik mijn abonnement aanbevelingen**

Als u uw abonnement opzegt, kunt u de volgende stappen gebruiken. Als je huidige abonnement een betaald abonnement is, wordt uw abonnement van kracht blijft tot het einde van de huidige betalingsperiode. Als u de annulering onmiddellijk effectief zijn, contact met ons opnemen [Microsoft Support](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

**Opmerking** Er wordt geen restitutie wordt gegeven als u vóór het einde van een betalingsperiode of voor niet-gebruikte transacties in een betalingsperiode annuleert.

* Ga naar [aanbiedingspagina] (https://datamarket.azure.com/dataset/amla/recommendations).
* Log in op de markt als u nog niet heeft aangemeld.
* Klik op **Annuleren** om het recht van de dataset naam en status. U kunt dit abonnement tot het einde van de huidige betalingsperiode of uw transactie is bereikt (afhankelijk van wat zich het eerst voordoet).

Als u wilt om uw abonnement direct zodat u een nieuw abonnement kunt aanschaffen, het bestand een ticket op de [Ondersteuning van Microsoft](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

##<a name="getting-started-with-recommendations"></a>Aan de slag met aanbevelingen

**Aanbevelingen voor mij is?** 

Aanbevelingen in de Machine Learning is voor organisaties en bedrijven die afhankelijk van de aanbevelingen van de cross zijn-sell en up-sell producten of diensten aan hun klanten. Als u beschikt over een klantgerichte website, een verkoopafdeling, een verkoopafdeling of een callcenter en als u een catalogus van meer dan een paar dozijn producten of diensten aanbiedt, de onderste regel van aanbevelingen kunnen profiteren. 

Experimenteren met aanbevelingen is ontworpen dat deze redelijk eenvoudig. De huidige API-versie is basic programmeren vereist. Als u hulp nodig hebt, neem dan contact op met de leverancier die uw website ontwikkeld. Als u een interne IT-afdeling of een interne ontwikkelaar, is ze moeten kunnen aanbevelingen voor u aan het werk krijgen. 

**Wat zijn de vereisten voor het instellen van aanbevelingen?**

Aanbevelingen moet u een logboek van keuzes van gebruikers met betrekking tot de catalogus. Als u t die een logboek hebt en u hebt een klant gerichte website, aanbevelingen gebruikersactiviteiten voor u kunnen verzamelen. 

Aanbevelingen vereist ook een catalogus van uw producten of diensten. Als u t dat de catalogus, aanbevelingen met het werkelijke gebruik van klantgegevens en distill van een catalogus. Een impliciete catalogus bevat items die niet werden gerapporteerd als onderdeel van de gebruikerstransacties niet.

**Hoe kan ik aanbevelingen instellen voor de eerste keer?**

Na [abonneren] (https://datamarket.azure.com/dataset/amla/recommendations) aanbevelingen, moet u de API-documentatie in [Azure Machine Learning aanbevelingen introductiehandleiding](machine-learning-recommendation-api-quick-start-guide.md) voor het instellen van de service.

**Waar vind ik de API-documentatie?** 

De API-documentatie is [Azure Machine Learning aanbevelingen Quick Start Guide](machine-learning-recommendation-api-quick-start-guide.md).

**Welke opties heb ik voor het uploaden van catalogus-en gebruiksgegevens aanbevelingen**

U hebt twee opties voor het uploaden van uw gegevens en het gebruik van catalogus: kunt u de gegevens exporteren vanuit uw CRM-systeem of andere logboeken en upload deze aanbevelingen, of u kunt ook labels toevoegen aan uw website die u van activiteiten van gebruikers bijhouden wilt. Als u de laatste methode gebruikt, worden de gegevens worden opgeslagen in Azure.

##<a name="maintenance-and-support"></a>Onderhoud en ondersteuning

**Hoe groot mag mijn set gegevens zijn?**

Elke set gegevens kan bevatten maximaal 100.000 catalogusitems en 2048 MB aan gegevens over het gebruik.
Een abonnement kan bovendien maximaal 10 gegevenssets (modellen) bevatten.

**Waar kan ik technische ondersteuning voor aanbevelingen krijgen?**

Technische ondersteuning is beschikbaar op de site [Ondersteuning van Microsoft Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) .

**Waar vind ik de gebruiksrechtovereenkomst?**

[Microsoft Azure Machine Learning servicevoorwaarden aanbevelingen API](https://datamarket.azure.com/dataset/amla/recommendations#terms).



 
