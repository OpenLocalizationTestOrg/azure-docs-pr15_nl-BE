<properties
   pageTitle="Registreren van gegevens uit het gegevensarchief Lake in Azure gegevenscatalogus | Microsoft Azure"
   description="Registreren van gegevens uit het gegevensarchief Lake in Azure gegevenscatalogus"
   services="data-lake-store,data-catalog" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="register-data-from-data-lake-store-in-azure-data-catalog"></a>Registreren van gegevens uit het gegevensarchief Lake in Azure gegevenscatalogus

In dit artikel leert u Azure Lake gegevensarchief integreren met Azure gegevenscatalogus om uw gegevens binnen een organisatie kan worden gedetecteerd door de integratie met de catalogus met gegevens. Zie voor meer informatie over gegevens catalogiseren, [Azure gegevenscatalogus](../data-catalog/data-catalog-what-is-data-catalog.md). Zie [algemene scenario's voor een catalogus met Azure gegevens](../data-catalog/data-catalog-common-scenarios.md)inzicht in scenario's waarin u de catalogus met gegevens kunt gebruiken.

## <a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie hebt u het volgende:

- **Azure een abonnement**. Zie [Azure krijg gratis proefperiode](https://azure.microsoft.com/pricing/free-trial/).

- **Uw abonnement op Azure inschakelen** voor gegevens Lake winkel Public Preview. Zie de [instructies](data-lake-store-get-started-portal.md#signup).

- **Azure Lake gegevensarchief account**. Volg de instructies op de [aan de slag met Azure Lake gegevensarchief maken met de Portal Azure](data-lake-store-get-started-portal.md). Voor deze zelfstudie laat ons een gegevensarchief Lake account genaamd **datacatalogstore**maken. 

    Nadat u de account hebt gemaakt, een verzameling voorbeeldgegevens te uploaden. Voor deze zelfstudie laat ons de CSV-bestanden in de map **AmbulanceData** in de [Azure gegevens Lake Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/)uploaden. Verschillende clients, zoals [Azure Opslagverkenner](http://storageexplorer.com/), kunt u het uploaden van gegevens naar een blob-container.

- **Catalogus-azure gegevens**. Uw organisatie moet al een catalogus met Azure gegevens voor uw organisatie zijn gemaakt. Slechts één catalogus is toegestaan voor elke organisatie.

## <a name="register-data-lake-store-as-a-source-for-data-catalog"></a>Register gegevensarchief Lake als bron voor gegevens-catalogus

>[AZURE.VIDEO adcwithadl] 

1. Ga naar `https://azure.microsoft.com/services/data-catalog`, en klik op **aan de slag**.

2. Log in op de portal catalogus Azure-gegevens en **gegevens publiceren**op.

    ![Een gegevensbron te registreren] (./media/data-lake-store-with-data-catalog/register-data-source.png "Een gegevensbron te registreren")

3. Klik op **Toepassing starten**op de volgende pagina. Dit zal het manifestbestand van toepassing op uw computer downloaden. Dubbelklik op het manifestbestand om de toepassing te starten.

4. Op de welkomstpagina klikt u op **aanmelden**en voer uw referenties.

    ![Welkom-scherm] (./media/data-lake-store-with-data-catalog/welcome.screen.png "Welkom-scherm")

5. Selecteer **Azure gegevens Lake**op het selecteren van een gegevensbron pagina en klik op **volgende**.

    ![Gegevensbron selecteren] (./media/data-lake-store-with-data-catalog/select-source.png "Gegevensbron selecteren")

6. Op de volgende pagina de naam van de Lake gegevensarchief dat u wilt registreren in de catalogus gegevens te bieden. Laat de andere opties als standaard en klik vervolgens op **verbinden**.

    ![Verbinding maken met gegevensbron] (./media/data-lake-store-with-data-catalog/connect-to-source.png "Verbinding maken met gegevensbron")

7. De volgende pagina kan worden onderverdeeld in de volgende segmenten.

    een. Het vak **Server hiërarchie** vertegenwoordigt structuur van de account Lake gegevensarchief. **$Root** vertegenwoordigt de hoofdmap van de account Lake gegevensopslag en **AmbulanceData** de map gemaakt in de hoofdmap van het account Lake gegevensarchief vertegenwoordigt.

    b. Het vak **beschikbare objecten** bevat de bestanden en mappen onder de map **AmbulanceData** .

    c. **Geregistreerde vak-objecten** worden de bestanden en mappen die u wilt registreren in de catalogus gegevens Azure.

    ![Structuur van de gegevens weergeven] (./media/data-lake-store-with-data-catalog/view-data-structure.png "Structuur van de gegevens weergeven")

8. Voor deze zelfstudie, moet u alle bestanden in de directory te registreren. Klikt u op de knop (![verplaatsen van objecten](./media/data-lake-store-with-data-catalog/move-objects.png "verplaatsen van objecten")) om alle bestanden verplaatsen om **objecten te registreren** in. 

    Omdat de gegevens in een catalogus met gegevens van de hele organisatie wordt geregistreerd, is een benadering aanbevolen toe te voegen sommige metagegevens die u later gebruiken kunt om de gegevens snel vinden. U kunt bijvoorbeeld een e-mailadres toevoegen voor de eigenaar van de gegevens (bijvoorbeeld één die het uploaden van de gegevens) of een label toevoegen om de gegevens te identificeren. De schermopname hieronder ziet u een label dat we aan de gegevens toevoegen.

    ![Structuur van de gegevens weergeven] (./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "Structuur van de gegevens weergeven")

    Klik op **registreren**.

8. De volgende afbeelding geeft aan dat de gegevens in de catalogus gegevens is geregistreerd.

    ![Inschrijving voltooid] (./media/data-lake-store-with-data-catalog/registration-complete.png "Structuur van de gegevens weergeven")

9. Klik op de **Portal weergeven** , gaat u terug naar de portal gegevenscatalogus en controleer dat u nu toegang de geregistreerde gegevens van de portal tot hebt. Als u wilt zoeken in de gegevens, kunt u de code die u hebt gebruikt tijdens het registreren van de gegevens.

    ![Gegevens zoeken in catalogus] (./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "Gegevens zoeken in catalogus")

10. Nu kunt u bewerkingen zoals het toevoegen van aantekeningen en documentatie op de gegevens uitvoeren. Zie de volgende koppelingen voor meer informatie.
    * [Gegevensbronnen in catalogus gegevens van aantekeningen voorzien](../data-catalog/data-catalog-how-to-annotate.md)
    * [Documentgegevensbronnen in catalogus gegevens](../data-catalog/data-catalog-how-to-documentation.md)

## <a name="see-also"></a>Zie ook

* [Gegevensbronnen in catalogus gegevens van aantekeningen voorzien](../data-catalog/data-catalog-how-to-annotate.md)
* [Documentgegevensbronnen in catalogus gegevens](../data-catalog/data-catalog-how-to-documentation.md)
* [Lake gegevensarchief integreren met andere Azure services](data-lake-store-integrate-with-other-services.md)
