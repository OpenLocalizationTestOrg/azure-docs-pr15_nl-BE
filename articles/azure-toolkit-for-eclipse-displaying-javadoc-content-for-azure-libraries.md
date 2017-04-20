<properties
    pageTitle="Javadoc inhoud weergeven in Eclips voor het pakket Azure Libraries voor Java"
    description="Het weergeven van de inhoud van de Javadoc voor de Azure bibliotheken in Eclips."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->

# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a>Javadoc inhoud weergeven in Eclips voor het pakket Azure Libraries voor Java #

De inhoud van de Javadoc voor de Azure Libraries voor Java kan worden bekeken in uw omgeving Eclips waarbij de Javadoc inhoud naar de Azure Libraries voor Java. De volgende stappen uit hoe u deze functionaliteit binnen de Eclips.

Deze procedure wordt ervan uitgegaan dat u de Azure-bibliotheek voor Java al toegevoegd aan de build-pad.

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a>Javadoc inhoud in Eclips weergeven voor de Azure Libraries voor Java ##

* Open in de sectie **Bibliotheken waarnaar wordt verwezen** in uw project binnen de Eclips Project Explorer het contextmenu voor de bibliotheek Azure voor Java JAR. Bijvoorbeeld **microsoft-windowsazure-api-0.1.0.jar** (het versienummer is mogelijk anders, afhankelijk van welke versie u hebt geïnstalleerd).
* Klik op **Eigenschappen**.
* Klik op **Javadoc locatie**van het dialoogvenster **Eigenschappen** in het linkerdeelvenster. Het dialoogvenster **Javadoc locatie** wordt weergegeven.
* U kunt een **URL Javadoc**of een **Javadoc in archief**.
    * Als u een **Javadoc-URL**opgeeft, gebruikt de URL's zoals **http://dl.windowsazure.com/javadoc** of **http://dl.windowsazure.com/storage/javadoc**.
    * Als u kiest voor **Javadoc in archief**, kunt u een extern bestand of een bestand in de werkruimte.
    Maak uw keuze en desgewenst bladeren/valideren. In het volgende voorbeeld wordt de Azure Libraries voor Java met de bijbehorende Javadoc JAR die lokaal is gedownload naar een map met de naam **c:\MyAzureJARs**.
    ![][ic553487]
* *Optioneel*: klik op **valideren**. Mogelijke problemen met de JAR-Javadoc kunnen hier worden weergegeven.
* Klik op **OK**.

Eenmaal gekoppeld aan de bibliotheek, moet de inhoud Javadoc binnen de IDE Eclips worden weergegeven. Bijvoorbeeld als `blob` wordt gedefinieerd van het type `CloudBlockBlob` in de code, de volgende is een voorbeeld van Javadoc-inhoud die wordt weergegeven wanneer u typt `blob.acquireLease` in de code:

![][ic553488]

## <a name="see-also"></a>Zie ook ##

[Azure Toolkit voor Eclips][]

[Een toepassing Hallo wereld maakt voor Azure in Eclips][]

[Installatie van de Toolkit Azure voor Eclips][] 

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkID=699529
[Een toepassing Hallo wereld maakt voor Azure in Eclips]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installatie van de Toolkit Azure voor Eclips]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png