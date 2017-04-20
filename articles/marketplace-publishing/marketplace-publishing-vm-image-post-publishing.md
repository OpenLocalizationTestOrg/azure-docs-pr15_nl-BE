<properties
   pageTitle="Beheren van uw image virtuele machine op de markt Azure | Microsoft Azure"
   description="Uitgebreide handleiding voor het beheren van uw image virtuele machine op de markt Azure na de oorspronkelijke publicatie."
   services="Azure Marketplace"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="Azure"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="hascipio;"/>

# <a name="post-production-guide-for-virtual-machine-offers-in-the-azure-marketplace"></a>Handleiding voor aanbiedingen van de virtuele machine op de markt Azure postproductie

In dit artikel wordt uitgelegd hoe u een live virtuele Machine-aanbod in de markt Azure kunt bijwerken. Ook kunt u op het proces van een of meer nieuwe SKU's toe te voegen aan een bestaande aanbieding en een actieve virtuele Machine aanbod of SKU verwijderen uit de markt Azure.

Nadat een aanbieding/SKU is klaargezet in de [Portal Azure](http://portal.azure.com), kunt u de onderstaande velden niet wijzigen:

- **Bieden id:** [Publiceren portal -> virtuele Machines -> Selecteer uw voorstel -> tabblad afbeeldingen VM-id bieden >]
- **SKU-id:** [Publiceren portal -> virtuele Machines -> Selecteer uw voorstel -> tabblad -> SKU's toevoegen een SKU]
- **Publisher Namespace:** [Publiceren portal -> virtuele Machines-scenario tabblad-Vertel ons over uw bedrijf (gevonden onder "Stap 2 registreren uw bedrijf") -> > > Publisher Namespace-Namespace >]

Zodra de aanbieding/SKU in de [Azure Marketplace](http://azure.microsoft.com/marketplace)wordt vermeld, kunt u de onderstaande velden niet wijzigen:

- **Bieden id:** [Publiceren portal -> virtuele Machines -> Selecteer uw voorstel -> tabblad afbeeldingen VM-id bieden >]
- **SKU-id:** [Publiceren portal -> virtuele Machines -> Selecteer uw voorstel -> tabblad -> SKU's toevoegen een SKU]
- **Publisher Namespace:** [Publiceren portal -> virtuele Machines-scenario tab -> > Vertel ons over uw bedrijf (gevonden in stap 2 registreren) Publisher Namespace-Namespace >]
- **Poorten** [Publiceren portal -> virtuele Machines -> Selecteer uw voorstel -> tabblad afbeeldingen VM-poorten openen >]
- **Wijziging van de genoemde SKU(s) prijzen**
- **Ander Model van genoemde SKU(s) facturering**
- **Verwijdering van de regio's van de vermelde SKU(s) facturering**
- **De telling van de schijf gegevens van de vermelde SKU(s) wijzigen**



## <a name="1-how-to-update-the-technical-details-of-a-sku"></a>1. het bijwerken van de technische details van een SKU

U kunt een nieuwe versie toevoegen aan de lijst SKU en uw aanbieding opnieuw te publiceren volgens de onderstaande stappen:

1. Meld u aan bij de [portal te publiceren](https://publish.windowsazure.com).
2. Ga naar het tabblad **virtuele MACHINES** en selecteer uw voorstel.
3. Klik op het tabblad **VM-afbeeldingen** in het menu links aan de zijkant.
4. Vanuit de **SKU's** sectie van het tabblad **VM-IMAGES** , zoek de SKU die u wilt bijwerken.
5. Daarna het nummer van een nieuwe versie van de SKU toevoegen en klik op de knop **"+"** . De nieuwe versie moet X.Y.Z indeling waarbij X, Y, Z zijn gehele getallen. Wijzigingen in versie moet alleen incrementele.
6. De handtekening van de gedeelde toegang die URI voor het besturingssysteem VHD gemaakt toevoegen en de wijzigingen opslaan in het vak **URL van OS VHD** .

    >[AZURE.IMPORTANT] U kan niet verhogen/verlagen het aantal schijven van gegevens van een SKU weergegeven. U moet in dat geval een nieuwe SKU maken. Raadpleeg de sectie 3 [. Het toevoegen van een nieuwe SKU onder een aanbieding genoemde](#3-how-to-add-a-new-sku-under-a-live-offer) voor gedetailleerde instructies.

7. Nadat u de wijzigingen hebt, Ga naar het tabblad **publiceren** en klik op de knop **DUWEN KLAARZETTEN**. Raadpleeg voor gedetailleerde instructies over het testen van uw aanbieding in de testomgeving deze [koppeling](marketplace-publishing-vm-image-test-in-staging.md)
8. Zodra u uw voorstel in staging getest hebt, Ga naar het tabblad **publiceren** in de publicatie portal en klik op de knop **Aanvragen goedkeuring te DUWEN naar productie** opnieuw publiceren van uw aanbieding in de markt Azure.

    ![tekening](media/marketplace-publishing-vm-image-post-publishing/img01_07.png)

## <a name="2-how-to-update-the-non-technical-details-of-an-offer-or-a-sku"></a>2. het bijwerken van de niet-technische details van een voorstel of een SKU

U kunt de niet-technische bijwerken (marketing, legal, ondersteuning, categorieën) details van uw aanbieding live of SKU op de markt Azure.

### <a name="21-update-the-offer-description-and-logos"></a>2.1 het voorstel en de logo's bijwerken

U kunt de details van de aanbieding bijwerken en uw aanbieding opnieuw publiceren door de volgende stappen:

1. Meld u aan bij de [portal te publiceren](https://publish.windowsazure.com).
2. Ga naar het tabblad **virtuele MACHINES** en selecteer uw voorstel.
3. Klik op het tabblad **MARKETING** in het menu links aan de zijkant.
4. Klik op de knop voor **ENGELS (VS)** .
5. Klik op het tabblad **DETAILS** in het menu links aan de zijkant. Onder de sectie *Beschrijving* van het tabblad **DETAILS** kunt u bijwerken van de titel van het voorstel, overzicht bieden, lang overzicht bieden en de wijzigingen opslaan.

    >[AZURE.NOTE] Neem zorgen voor de volgende terwijl u de details van de SKU wilt bijwerken.
    **Geef geen dubbele tekst onder de beschrijving van de aanbieding en de beschrijving van de SKU. Geef geen dubbele tekst onder de titel SKU en het aanbod van lange samenvatting. Geef geen dubbele tekst onder de SKU-titel en de samenvatting van het voorstel.**

    ![tekening](media/marketplace-publishing-vm-image-post-publishing/img02.1_05.png)

6. Onder het *logo* gedeelte van het tabblad **DETAILS** kunt u de logo's bijwerken. Er echter voor zorgen dat de logo's de [richtlijnen van Azure Marketplace volgen](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) (Zie de sectie stap 1: marketing inhoud bieden Marketplace -> Details -> Azure Marketplace-Logo richtsnoeren).

    >[AZURE.NOTE] Pictogram held is optioneel. U kunt niet een held pictogram uploaden. Echter zodra het pictogram held is geüpload, klik er is geen voorziening te verwijderen uit de publicatie portal. In dat geval moet u de [held pictogram richtlijnen](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) volgen (Zie de sectie stap 1: Marketplace bieden marketing inhoud -> Details -> aanvullende richtlijnen voor het spandoek held logo).

7. Nadat u de wijzigingen hebt, Ga naar het tabblad **publiceren** en klik op de knop **DUWEN KLAARZETTEN**. Zie deze [link](marketplace-publishing-vm-image-test-in-staging.md)voor gedetailleerde instructies over het testen van uw aanbieding in de testomgeving.
8. Zodra u uw voorstel in staging getest hebt, Ga naar het tabblad **publiceren** in de publicatie portal en klik op de knop **Aanvragen goedkeuring te DUWEN naar productie** opnieuw publiceren van uw aanbieding in de markt Azure.

    ![tekening](media/marketplace-publishing-vm-image-post-publishing/img02.1_08.png)

### <a name="22-update-the-sku-description"></a>2.2. Werk de beschrijving van de SKU

U kunt details van de SKU bijwerken en uw aanbieding opnieuw publiceren door de onderstaande stappen te volgen:

1. Meld u aan bij het [publiceren van portal](https://publish.windowsazure.com)
2. Ga naar het tabblad **virtuele MACHINES** en selecteer uw voorstel.
3. Klik op het tabblad **MARKETING** in het menu links aan de zijkant.
4. Klik op de knop voor **ENGELS (VS)** .
5. Klik op het tabblad **plannen** in het menu links aan de zijkant. U kunt de titel SKU, SKU-overzicht en details van de SKU-beschrijving bijwerken en de wijzigingen opslaan onder de sectie *SKU's* van het tabblad **plannen** .

    >[AZURE.NOTE] Neem zorgen voor de volgende terwijl u de details van de SKU wilt bijwerken. **Geef geen dubbele tekst onder de beschrijving van de aanbieding en de beschrijving van de SKU. Geef geen dubbele tekst onder de titel van de SKU's en de aanbieding lange samenvatting. Geef geen dubbele tekst onder de SKU-titel en de samenvatting van het voorstel.**

6. Nadat u de wijzigingen hebt, Ga naar het tabblad **publiceren** en klik op de knop **DUWEN KLAARZETTEN**. Raadpleeg voor gedetailleerde instructies over het testen van uw aanbieding in de testomgeving deze koppeling
7. Zodra u uw voorstel in staging getest hebt, Ga naar het tabblad **publiceren** in de publicatie portal en klik op de knop **Aanvragen goedkeuring te DUWEN naar productie** opnieuw publiceren van uw aanbieding in de markt Azure.

    ![tekening](media/marketplace-publishing-vm-image-post-publishing/img02.2_07.png)

### <a name="23-change-the-existing-links-or-add-new-links"></a>2.3 de bestaande koppelingen wijzigen of nieuwe koppelingen toevoegen

U kunt de bestaande koppelingen wijzigen of nieuwe koppelingen toevoegen en uw aanbieding opnieuw door de onderstaande stappen uit om te publiceren:

1. Meld u aan bij het [publiceren van portal](https://publish.windowsazure.com)
2. Ga naar het tabblad **virtuele MACHINES** en selecteer uw voorstel.
3. Klik op het tabblad **MARKETING** in het menu links aan de zijkant.
4. Klik op de knop voor **ENGELS (VS)** .
5. Klik op het tabblad **koppelingen** in het menu links aan de zijkant.
6. Als u een nieuwe koppeling toevoegt, onder de *Links* sectie klik vervolgens op de knop **Koppeling toevoegen** . Het dialoogvenster *"Link toevoegen"* wordt geopend. In dit dialoogvenster kunt u de titel van de koppeling toevoegen en URL velden en de wijzigingen opslaan. Kunt u een koppeling met gegevens die de klanten helpen kan.
7. Als u wilt bijwerken of een bestaande koppeling verwijderen, selecteert u de desbetreffende koppeling op en klikt u op de knop bewerken of de knop verwijderen dienovereenkomstig.

    >[AZURE.NOTE] Zorg ervoor dat de koppelingen die u hebt ingevoerd in deze sectie worden probleemloos functioneert, als deze koppelingen tijdens het productieproces van de aanvraag get gevalideerd.

8. Nadat u de wijzigingen hebt, Ga naar het tabblad **publiceren** en klik op de knop **DUWEN KLAARZETTEN**. Zie deze [link](marketplace-publishing-vm-image-test-in-staging.md)voor gedetailleerde instructies over het testen van uw aanbieding in de testomgeving.
9. Zodra u uw voorstel in staging getest hebt, Ga naar het tabblad **publiceren** in de publicatie portal en klik op de knop **Aanvragen goedkeuring te DUWEN naar productie** opnieuw publiceren van uw aanbieding in de markt Azure.

    ![tekening](media/marketplace-publishing-vm-image-post-publishing/img02.3_09-01.png)

    ![tekening](media/marketplace-publishing-vm-image-post-publishing/img02.3-2.png)

### <a name="24-change-an-existing-sample-image-or-add-a-new-sample-image"></a>2.4 voorbeeld van een bestaande afbeelding wijzigen of toevoegen van een nieuw voorbeeld van afbeelding

U wijzigt een bestaande voorbeeldafbeeldingen of nieuwe voorbeeldafbeeldingen en uw aanbieding opnieuw door de onderstaande stappen uit om te publiceren:

>[AZURE.NOTE] Slechts één voorbeeld van afbeelding wordt weergegeven in de [https://portal.azure.com](https://portal.azure.com).

1. Meld u aan bij het [publiceren van portal](https://publish.windowsazure.com)
2. Ga naar het tabblad **virtuele MACHINES** en selecteer uw voorstel.
3. Klik op het tabblad **MARKETING** in het menu links aan de zijkant.
4. Klik op de knop voor **ENGELS (VS)** .
5. Klik op het tabblad **VOORBEELDAFBEELDINGEN** in het menu links aan de zijkant.
6. Als u toevoegen een nieuw voorbeeld van afbeelding wilt, klik in de sectie *Voorbeeldafbeeldingen* Klik op de knop **Nieuwe afbeelding uploaden** en de wijzigingen op te slaan.

    >[AZURE.NOTE] Inclusief een voorbeeld van afbeelding is een optionele stap.

7. Als u wilt bijwerken of verwijderen van een bestaand voorbeeld van afbeelding, zoekt u het juiste voorbeeld van afbeelding en klik vervolgens op de knop **Afbeelding vervangen** of verwijderen dienovereenkomstig.

8. Nadat u de wijzigingen hebt, Ga naar het tabblad **publiceren** en klik op de knop **DUWEN KLAARZETTEN**. Zie deze [link](marketplace-publishing-vm-image-test-in-staging.md)voor gedetailleerde instructies over het testen van uw aanbieding in de testomgeving.
9. Zodra u uw voorstel in staging getest hebt, Ga naar het tabblad **publiceren** in de publicatie portal en klik op de knop **Aanvragen goedkeuring te DUWEN naar productie** opnieuw publiceren van uw aanbieding in de markt Azure.

    ![tekening](media/marketplace-publishing-vm-image-post-publishing/img02.4_09.png)

### <a name="25-update-the-legal-content"></a>2.5 de juridische inhoud bijwerken

U kunt de juridische inhoud bijwerken en opnieuw publiceren van uw voorstel door de onderstaande stappen te volgen:

1. Meld u aan bij het [publiceren van portal](https://publish.windowsazure.com)
2. Ga naar het tabblad **virtuele MACHINES** en selecteer uw voorstel.
3. Klik op het tabblad **MARKETING** in het menu links aan de zijkant.
4. Klik op de knop voor **ENGELS (VS)** .
5. Klik op het tabblad **juridische** in het menu links aan de zijkant. U kunt uw beleid/gebruiksvoorwaarden bijwerken onder het gedeelte met *juridische* . Invoeren of beleid/voorwaarden in de *Gebruiksvoorwaarden van* textbox en sla de wijzigingen.
6. Het maximum aantal tekens voor de juridische voorwaarden voor gebruiken is 1.000.000 tekens.
7. Nadat u de wijzigingen hebt, Ga naar het tabblad **publiceren** en klik op de knop **DUWEN KLAARZETTEN**. Raadpleeg voor gedetailleerde instructies over het testen van uw aanbieding in de testomgeving deze [koppeling](marketplace-publishing-vm-image-test-in-staging.md)
8. Zodra u uw voorstel in staging getest hebt, Ga naar het tabblad **publiceren** in de publicatie portal en klik op de knop **Aanvragen goedkeuring te DUWEN naar productie** opnieuw publiceren van uw aanbieding in de markt Azure.

    ![tekening](media/marketplace-publishing-vm-image-post-publishing/img02.5_08.png)

### <a name="26-update-the-support-information"></a>2.6 de ondersteuningsinformatie bijwerken

U kunt de ondersteuningsinformatie en uw aanbieding opnieuw publiceren door de volgende stappen uit:

1. Meld u aan bij het [publiceren van portal](https://publish.windowsazure.com)
2. Ga naar het tabblad **virtuele MACHINES** en selecteer uw voorstel.
3. Klik op het tabblad **ondersteuning** in het menu links aan de zijkant.
4. U kunt de contactgegevens bijwerken onder de sectie *Technische contactpersoon* van het tabblad **ondersteuning** . Deze gegevens worden gebruikt alleen voor interne communicatie tussen de partners en Microsoft.
5. U kunt de ondersteuning voor contactinformatie zoals **naam, e-mailadres, telefoonnummer** en **URL voor ondersteuning**bijwerken in de sectie *Customer Support* van het tabblad **ondersteuning** . Deze gegevens worden gebruikt alleen voor interne communicatie tussen de partners en Microsoft.

    >[AZURE.NOTE] Als u wilt dat alleen e-mail ondersteuning te bieden, bieden een dummy nummer onder de sectie voor **Klantondersteuning** . In dit geval wordt uw opgegeven e-mailadres gebruikt in plaats daarvan.

6. Nadat u de wijzigingen hebt, Ga naar het tabblad **publiceren** en klik op de knop **DUWEN KLAARZETTEN**. Raadpleeg voor gedetailleerde instructies over het testen van uw aanbieding in de testomgeving deze [koppeling](marketplace-publishing-vm-image-test-in-staging.md)
7. Zodra u uw voorstel in staging getest hebt, Ga naar het tabblad **publiceren** in de publicatie portal en klik op de knop **Aanvragen goedkeuring te DUWEN naar productie** opnieuw publiceren van uw aanbieding in de markt Azure.

    ![tekening](media/marketplace-publishing-vm-image-post-publishing/img02.6_07.png)

### <a name="27-update-the-categories"></a>2.7 de categorieën bijwerken

U kunt de categoriesectie werken voor uw voorstel en uw aanbieding opnieuw publiceren door de onderstaande stappen te volgen:

1. Meld u aan bij het [publiceren van portal](https://publish.windowsazure.com)
2. Ga naar het tabblad **virtuele MACHINES** en selecteer uw voorstel.
3. Klik op het tabblad **categorieën** in het menu links aan de zijkant.
4. U kunt onder de sectie *categorieën* de categorieën voor uw aanbieding bijwerken en de wijzigingen opslaan. U kunt maximaal vijf categorieën voor de galerie Azure Marketplace.
5. Nadat u de wijzigingen hebt, Ga naar het tabblad **publiceren** en klik op de knop **DUWEN KLAARZETTEN**. Raadpleeg voor gedetailleerde instructies over het testen van uw aanbieding in de testomgeving deze [koppeling](marketplace-publishing-vm-image-test-in-staging.md)
6. Zodra u uw voorstel in staging getest hebt, Ga naar het tabblad **publiceren** in de publicatie portal en klik op de knop **Aanvragen goedkeuring te DUWEN naar productie** opnieuw publiceren van uw aanbieding in de markt Azure.

    ![tekening](media/marketplace-publishing-vm-image-post-publishing/img02.7_06.png)

## <a name="3-how-to-add-a-new-sku-under-a-listed-offer"></a>3. het toevoegen van een nieuwe SKU onder een aanbieding vermelde

Door de onderstaande stappen kunt u een nieuwe SKU toevoegen onder uw live voorstel:

1. Meld u aan bij de [portal te publiceren](https://publish.windowsazure.com).
2. Ga naar het tabblad **virtuele MACHINES** en selecteer uw voorstel.
3. Klik op het tabblad **SKU's** in het menu links aan de zijkant. Na deze Klik op de knop **ADD A SKU**.  Een nieuw dialoogvenster wordt geopend. Een SKU-id invoeren in kleine letters. Schakel het selectievakje in voor facturering model(BYOL) brengen zelf als u wilt de nieuwe SKU facturatie model BYOL met publiceren. Schakel anders het selectievakje voor BYOL. Na deze Klik op de maatstreepjes van het dialoogvenster voor het maken van een nieuwe SKU. Als je niet voor de facturering model van BYOL voor de nieuwe SKU, worden vervolgens de facturering model automatisch ingesteld op het uurtarief dat voor de nieuwe SKU. Als u schakelen van de gratis proefversie 30days voor elk uur facturatie model wilt, vervolgens klikken op de optie "Een maand" voor "Is een gratis proefversie beschikbaar?". Anders selecteert u "Nee proef". [Opmerking: de optie "Is een gratis proefversie beschikbaar?" wordt alleen weergegeven als u geen BYOL in het dialoogvenster hebt tijdens het maken van de nieuwe SKU.]

    >[AZURE.IMPORTANT] De optie "Deze SKU van Marketplace verbergen omdat moet altijd worden gekocht via een sjabloon oplossing" moeten worden gemarkeerd als 'Ja' alleen als u voor het publiceren van een voorstel van de sjabloon oplossing in de markt Azure worden goedgekeurd. Anders is worden deze optie altijd gemarkeerd als 'NO'.

4. Nu in het menu links aan de zijkant, klikt u op het tabblad **VM-afbeeldingen** en ontdek de nieuwe SKU die u hebt gemaakt.
5. Als u de nieuwe SKU instellen, verwijzen naar de stap 5 van deze [koppeling](marketplace-publishing-vm-image-creation.md#5-obtain-certification-for-your-vm-image) voor instructies.
6. Als u wilt toevoegen het marketingmateriaal voor de nieuwe SKU, Zie de sectie stap 1: marketing inhoud bieden Marketplace -> Details -> punt nummers 2 tot en met 5 van deze [koppeling](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
7. Raadpleeg de sectie 2.1 doordat de prijsinformatie voor de nieuwe SKU. Stel uw VM prijzen van deze [koppeling](marketplace-publishing-push-to-staging.md#step-2-set-your-prices)
8. Nadat u de wijzigingen hebt, Ga naar het tabblad **publiceren** en klik op de knop **DUWEN KLAARZETTEN**. Raadpleeg voor gedetailleerde instructies over het testen van uw aanbieding in de testomgeving deze [koppeling](marketplace-publishing-vm-image-test-in-staging.md)
9. Zodra u uw voorstel in staging getest hebt, Ga naar het tabblad **publiceren** in de publicatie portal en klik op de knop **Aanvragen goedkeuring te DUWEN naar productie** opnieuw publiceren van uw aanbieding in de markt Azure.

    ![tekening](media/marketplace-publishing-vm-image-post-publishing/img03_09-01.png)

    ![tekening](media/marketplace-publishing-vm-image-post-publishing/img03_09-02.png)

## <a name="4-how-to-change-the-data-disk-count-for-a-listed-sku"></a>4. hoe u de telling van de schijf gegevens voor de SKU van een lijst wijzigen

U kan niet verhogen/verlagen het aantal schijven van gegevens van een SKU weergegeven. Moet u in dit geval een maken van een nieuwe SKU. Raadpleeg de sectie 3 [. Het toevoegen van een nieuwe SKU onder een aanbieding live](#3-how-to-add-a-new-sku-under-a-live-offer) voor gedetailleerde instructies.

## <a name="5---how-to-delete-a-listed-offer-from-the-azure-marketplace"></a>5. het verwijderen van een aanbieding weergegeven van de Azure Marketplace

Er zijn verschillende aspecten die worden afgehandeld in het geval van een verzoek moeten om een actieve aanbieding te verwijderen. Voer de volgende stappen om de richtlijnen van het support-team een vermelde aanbieding verwijderen uit de markt Azure ophalen:

1.  Een support ticket via deze [koppeling](https://support.microsoft.com/en-us/getsupport?wf=0&tenant=ClassicCommercial&oaspworkflow=start_1.0.0.0&locale=en-us&supportregion=en-us&pesid=15635&ccsid=635993707583706681) verhogen
2.  Soort probleem als **'Aanbiedingen beheren'** te selecteren en selecteer categorie **"Wijzigen van een aanbod en/of de SKU die reeds in productie"**
3.  De aanvraag indienen

Het ondersteuningsteam begeleidt u bij de verwijdering van de aanbieding/SKU.

>[AZURE.NOTE] U kunt altijd de aanbieding verwijderen terwijl deze de status concept is (dat wil zeggen, niet in STAGING- of productie) door te klikken op de knop **Concept verwijderen** op het tabblad **Geschiedenis** .

## <a name="6-how-to-delete-a-listed-sku-from-the-azure-marketplace"></a>6. het verwijderen van een lijst SKU van Azure Marketplace

Een lijst SKU kunt van Azure Marketplace u verwijderen door de onderstaande stappen te volgen:

1. Meld u aan bij de [portal te publiceren](https://publish.windowsazure.com).
2. Ga naar het tabblad **virtuele MACHINES** en selecteer uw voorstel.
3. Klik op het tabblad **SKU's** vanuit het deelvenster aan de linkerhand.
4. Selecteer de SKU die u wilt verwijderen en klik op de knop delete tegen deze SKU.
5. Zodra u klaar bent, gaat u naar het tabblad publiceren in de publicatie portal en klik op de knop **Aanvragen goedkeuring te DUWEN naar productie** naar het aanbod op de markt Azure opnieuw te publiceren.
6. Zodra de aanbieding opnieuw wordt gepubliceerd in de markt Azure, worden de SKU verwijderd uit de markt Azure en de Azure-Portal.

## <a name="7-how-to-delete-the-current-version-of-a-listed-sku-from-the-azure-marketplace"></a>7. het verwijderen van de huidige versie van de SKU van een lijst van Azure Marketplace

U kunt de huidige versie van de SKU van een lijst van Azure Marketplace verwijderen door de onderstaande stappen te volgen. Zodra het proces voltooid is, wordt de SKU teruggedraaid naar de vorige versie.

1. Meld u aan bij de [portal te publiceren](https://publish.windowsazure.com).
2.  Ga naar het tabblad **virtuele MACHINES** en selecteer uw voorstel.
3.  Klik op het tabblad **VM-afbeeldingen** uit het deelvenster aan de linkerhand.
4.  Selecteer de SKU waarvan u wilt verwijderen en klik op de knop verwijderen die versie ten opzichte van de huidige versie.
5.  Zodra u klaar bent, gaat u naar het tabblad **publiceren** in de publicatie portal en klik op de knop **Aanvragen goedkeuring te DUWEN naar productie** naar het aanbod op de markt Azure opnieuw te publiceren.
6.  Zodra het voorstel opnieuw in de markt Azure wordt gepubliceerd, worden de huidige versie van de vermelde SKU verwijderd uit de markt Azure en de Portal Azure. De SKU wordt teruggedraaid naar de vorige versie.

## <a name="8-how-to-revert-listing-price-to-production-values"></a>8. het terugdraaien van aanbieding prijs aan de productiewaarden
Ik de prijs van een SKU vermeld zijn gewijzigd (of ik facturering regio's van een vermelde SKU hebt verwijderd). Omdat deze niet wordt ondersteund in de markt Azure, wil ik mijn wijzigingen in de productiewaarden herstellen. Hoe bereikt die weergegeven?

Volg de onderstaande stappen:

1. Meld u aan bij de [portal te publiceren](https://publish.windowsazure.com).
2. Ga naar het tabblad **virtuele MACHINES** en selecteer uw voorstel.
3. In het menu links aan de zijkant, klikt u op het tabblad **prijzen** .
4. Selecteer onder het tabblad prijs een gebied waarvan prijzen die u opnieuw wilt instellen.

    ![tekening](media/marketplace-publishing-vm-image-post-publishing/img08-04.png)

5. In het geval van SKU's per uur facturatie model met reset de prijzen voor alle cores zijn in de productie voor de geselecteerde regio. Voor SKU's facturering model BYOL met de SKU beschikbaar maken in de regio door het selectievakje ten opzichte van de SKU in de sectie EXTERNALLY-LICENSED (BYOL) SKU beschikbaarheid controleren (Zie het screenshot hieronder).

    ![tekening](media/marketplace-publishing-vm-image-post-publishing/img08-05.png)

6. Klik nu op de knop **AUTOPRICE andere markten gebaseerd op prijzen IN de Verenigde Staten**.

    >[AZURE.NOTE] Label van de knop kan afwijken, afhankelijk van de regio die u hebt geselecteerd. Aangezien we Verenigde Staten hebt geselecteerd tijdens het maken van dit document, zodat de knop bestempeld als "Auto prijs andere markten op basis van de prijzen in de Verenigde Staten" in het screenshot hieronder.

    ![tekening](media/marketplace-publishing-vm-image-post-publishing/img08-06.png)

7. De wizard automatisch prijs wordt geopend. De eerste pagina geeft de selectie voor de basis-markt. De sectie en naar de volgende pagina gaan door te klikken op de knop **"->"** .

    ![tekening](media/marketplace-publishing-vm-image-post-publishing/img08-07.png)

8. De optie voor het selecteren van de cores en plannen wordt weergegeven op de pagina 2. Selecteer de gewenste plannen en de cores en klik op '->' knop.

    ![tekening](media/marketplace-publishing-vm-image-post-publishing/img08-08.png)

9. Pagina 3 geeft de markten en regio's. Klik op alle in-/ uitschakelen om alle regio's of handmatig de selectievakjes voor de regio. Klik op de knop "->" te verplaatsen naar de volgende pagina.

    ![tekening](media/marketplace-publishing-vm-image-post-publishing/img08-09.png)

10. Pagina 4 geeft de wisselkoersen. Klik op de knop Voltooien om de stappen te voltooien. De wizard opnieuw in de prijzen op basis van uw selectie.

11. Nu gaat u naar het tabblad prijzen en klik op de knop 'Overzicht en wijzigingen weergeven'.
'Concept' selecteren in de sectie 'Versie weergeven' en 'Productie' in de sectie 'Vergelijken met' (Zie het screenshot hieronder). Als u geen prijzen verschil ziet, betekent dat prijzen is teruggezet naar de productiewaarden met succes.

    ![tekening](media/marketplace-publishing-vm-image-post-publishing/img08-11.png)

12. Nadat u de wijzigingen hebt, Ga naar het tabblad publiceren en klik op de knop **DUWEN KLAARZETTEN**. Raadpleeg voor gedetailleerde instructies over het testen van uw aanbieding in de testomgeving deze [koppeling](marketplace-publishing-vm-image-test-in-staging.md)
13. Zodra u uw voorstel in staging getest hebt, Ga naar het tabblad publiceren in de publicatie portal en klik op de knop **Aanvragen goedkeuring te DUWEN naar productie** opnieuw publiceren van uw aanbieding in de markt Azure.

## <a name="9-how-to-revert-billing-model-to-production-values"></a>9. het terugdraaien van facturering model aan de productiewaarden
Ik hebt het factuuradres model van een vermelde SKU gewijzigd. Omdat deze niet wordt ondersteund in de markt Azure, wil ik mijn wijzigingen in de productiewaarden herstellen. Hoe bereikt die weergegeven?

Volg de onderstaande stappen:

1. Meld u aan bij de [portal te publiceren](https://publish.windowsazure.com).
2. Ga naar het tabblad **virtuele MACHINES** en selecteer uw voorstel.
3. Klik op het tabblad **SKU's** in het menu links aan de zijkant.
4. Klik op de knop bewerken als u het model voor facturering. Een venster wordt geopend. Schakel het selectievakje **'facturering en licenties gebeurt extern vanuit Azure (aka doen om uw eigen licentie)'** dienovereenkomstig of uit.

    ![tekening](media/marketplace-publishing-vm-image-post-publishing/img09-04.png)

5. Eenmaal verwijzen wij u naar het antwoord van de vraag 8 in dit document terug de prijzen.
6. Na dat gaat u naar het tabblad **publiceren** in de publicatie portal en push de aanbieding voor gefaseerde installatie te testen. Zodra u klaar bent met het testen van de aanbieding en vervolgens klikt u op de knop **Aanvragen goedkeuring te DUWEN naar productie** opnieuw publiceren van uw aanbieding in de markt Azure.

## <a name="10-how-to-revert-visibility-setting-of-a-listed-sku-to-the-production-value"></a>10. het terugdraaien van de zichtbaarheidsinstelling van een SKU weergegeven op de productiewaarde

Volg de onderstaande stappen:

1. Meld u aan bij de [portal te publiceren](https://publish.windowsazure.com).
2. Ga naar het tabblad **virtuele MACHINES** en selecteer uw voorstel.
3. Klik op het tabblad **SKU's** in het menu links aan de zijkant.
4. Selecteer de SKU en de zichtbaarheidsinstellingen van de SKU voor de productiewaarde te herstellen.

    ![tekening](media/marketplace-publishing-vm-image-post-publishing/img10-04.png)

5. Zodra u klaar bent met de wijzigingen en klik vervolgens op de knop **Aanvragen goedkeuring te DUWEN naar productie** opnieuw publiceren van uw aanbieding in de markt Azure.

## <a name="see-also"></a>Zie ook
- [Aan de slag: Het publiceren van een aanbod op de markt Azure](marketplace-publishing-getting-started.md)
- [Wat zijn de inzichten van de verkoper melden](marketplace-publishing-report-seller-insights.md)
- [Wat zijn de uitbetaling melden](marketplace-publishing-report-payout.md)
- [Het wijzigen van uw reseller Cloud Solution Provider stimulans](marketplace-publishing-csp-incentive.md)
- [Problemen publiceren in de markt](marketplace-publishing-support-common-issues.md)
- [Ondersteuning krijgen als een uitgever](marketplace-publishing-get-publisher-support.md)
- [Een installatiekopie maken voor VM op ruimten](marketplace-publishing-vm-image-creation-on-premise.md)
- [Een virtuele machine met Windows in Azure preview portal maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md)
