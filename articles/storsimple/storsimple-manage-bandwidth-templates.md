<properties
   pageTitle="Beheer uw bandbreedte StorSimple sjablonen | Microsoft Azure"
   description="Beschrijving van het StorSimple bandbreedte sjablonen waarmee u kunt de besturing van bandbreedtegebruik beheren."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-storsimple-bandwidth-templates"></a>De StorSimple Manager-service gebruiken voor het beheren van sjablonen voor StorSimple bandbreedte

## <a name="overview"></a>Overzicht

Bandbreedte-sjablonen kunt u verbruik van de netwerkbandbreedte tussen meerdere schema's een tijd van de dag de gegevens van het apparaat StorSimple naar de cloud trapsgewijs configureren.

Met bandbreedtebeperking planningen kunt u:

- Bandbreedte voor aangepaste schema's afhankelijk van het gebruik van de werkbelasting netwerk opgeven.

- Centraal beheer en de schema's over meerdere apparaten op een eenvoudige en naadloze wijze hergebruiken.

> [AZURE.NOTE] Deze functie is alleen beschikbaar voor StorSimple fysieke apparaten en niet voor virtuele apparaten.

De bandbreedte-sjablonen voor uw service in tabelvorm worden weergegeven en bevat de volgende gegevens:

- **Naam** : een unieke naam aan de sjabloon bandbreedte toegewezen toen deze werd gemaakt.

- **Planning** – het nummer van de schema's die zich in een bepaalde bandbreedte sjabloon.

- **Gebruikt door** : het aantal volumes met de bandbreedte-sjablonen.

U kunt de pagina StorSimple Manager service **configureren** in de klassieke Azure portal bandbreedte sjablonen beheren.

Ook vindt u aanvullende informatie over sjablonen in bandbreedte configureren:

- Vragen en antwoorden over bandbreedte-sjablonen
- Aanbevolen procedures voor bandbreedte sjablonen

## <a name="add-a-bandwidth-template"></a>Een bandbreedte sjabloon toevoegen

Voer de volgende stappen uit om een nieuwe bandbreedte-sjabloon te maken.

#### <a name="to-add-a-bandwidth-template"></a>Een bandbreedte sjabloon toevoegen

1. Klik op **toevoegen/bewerken bandbreedte sjabloon**op de pagina StorSimple Manager service **configureren** .

2. Klik in het dialoogvenster **Add/Edit bandbreedte sjabloon** :

   1. Selecteer in de vervolgkeuzelijst **sjabloon** **Nieuw** toevoegen van een nieuwe sjabloon van de bandbreedte.
   2. Geef een unieke naam voor de sjabloon van uw bandbreedte.

3. Een **bandbreedte planning**definiëren. Een planning maken:

   1. Kies in de vervolgkeuzelijst de dagen van de week in die de planning is geconfigureerd voor. U kunt meerdere dagen selecteren door de selectievakjes vóór de desbetreffende dagen in de lijst.
   2. Selecteer de optie **Alle dag** als de planning wordt afgedwongen voor de hele dag. Wanneer deze optie is ingeschakeld, kunt u niet langer een **Begintijd** of **Eindtijd**van een opgeven. De planning wordt uitgevoerd van 12:00 uur tot 23:59 uur.
   3. Selecteer een **Begintijd**in de vervolgkeuzelijst. Dit is wanneer de planning begint.
   4. Selecteer in de vervolgkeuzelijst een **Eindtijd**. Dit is als het schema wordt beëindigd.

         > [AZURE.NOTE] Overlappende planningen zijn niet toegestaan. Als de begin- en eindtijden in een overlappende planning resulteren zal, ziet u een foutbericht toe.

   5. Geef het **percentage van de bandbreedte**. Dit is de bandbreedte in Megabits per seconde (Mbps) die wordt gebruikt door het apparaat StorSimple in operaties met betrekking tot de cloud (uploads en downloads). Geef een getal tussen 1 en 1000 voor dit veld.

   6. Klik op het pictogram voor ![pictogram voor](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). De sjabloon die u hebt gemaakt wordt toegevoegd aan de lijst met sjablonen op de servicepagina **configureren** bandbreedte.

    ![Nieuwe sjabloon maken op bandbreedte](./media/storsimple-manage-bandwidth-templates/HCS_CreateNewBT1.png)

4. Klik op **Opslaan** onder aan de pagina en klik vervolgens op **Ja** wanneer u wordt gevraagd om bevestiging. Wijzigingen die u hebt aangebracht in de configuratie wordt opgeslagen.

## <a name="edit-a-bandwidth-template"></a>Een bandbreedte sjabloon bewerken

Voer de volgende stappen uit om een bandbreedte-sjabloon bewerken.

### <a name="to-edit-a-bandwidth-template"></a>Een bandbreedte sjabloon bewerken

1. Klik op **toevoegen/bewerken bandbreedte sjabloon**.

2. Klik in het dialoogvenster **Add/Edit bandbreedte sjabloon** :

   1. Kies een bestaande sjabloon voor de bandbreedte die u wilt wijzigen in de vervolgkeuzelijst **sjabloon** .
   2. Uw wijzigingen worden voltooid. (U kunt wijzigen van de bestaande instellingen.)
   3. Klik op het pictogram ![Pictogram](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). Ziet u de gewijzigde sjabloon in de lijst met sjablonen van bandbreedte op de pagina service configureren.

3. Als u wilt uw wijzigingen wilt opslaan, klikt u op **Opslaan** onder aan de pagina. Klik op **Ja** wanneer u wordt gevraagd om bevestiging.

> [AZURE.NOTE] U wordt niet toegestaan de wijzigingen wilt opslaan als de bewerkte planning overlapt met een bestaand schema in de sjabloon van de bandbreedte die u wilt wijzigen.

## <a name="delete-a-bandwidth-template"></a>Een bandbreedte-sjabloon verwijderen

Voer de volgende stappen uit om een bandbreedte sjabloon verwijderen.

#### <a name="to-delete-a-bandwidth-template"></a>Een bandbreedte-sjabloon verwijderen

1. Selecteer de sjabloon die u wilt verwijderen in de lijst in tabelvorm van de bandbreedte-sjablonen voor uw service. Een verwijderpictogram (**x**) wordt uiterst rechts van de geselecteerde sjabloon weergegeven. Klik op het pictogram **x** om de sjabloon verwijderen.

2. U wordt om bevestiging gevraagd. Klik op **OK** om door te gaan.

Als de sjabloon die gebruikt door een volume wordt, wordt u niet toegestaan om deze te verwijderen. Er verschijnt een foutbericht met de mededeling dat de sjabloon gebruikt wordt. Het dialoogvenster een foutbericht wordt weergegeven waarin wordt gemeld dat de verwijzingen naar de sjabloon moeten worden verwijderd.

Door toegang tot de pagina **Volume Containers** en de volume-containers die deze sjabloon gebruiken zodat ze een andere sjabloon gebruiken of een aangepaste of onbeperkte bandbreedte-instelling te wijzigen, kunt u de verwijzingen naar de sjabloon verwijderen. Als alle verwijzingen zijn verwijderd, kunt u de sjabloon verwijderen.

## <a name="use-a-default-bandwidth-template"></a>Een standaardsjabloon bandbreedte gebruiken

Een standaardsjabloon bandbreedte wordt geleverd en wordt gebruikt door een volume containers standaard gebruikt om besturingselementen bandbreedte bij de toegang tot de cloud. De standaardsjabloon dient tevens als een verwijzing klaar voor gebruikers die hun eigen sjablonen maken. De details van deze standaardsjabloon zijn:

- **Naam** : onbeperkt aantal nachten en weekends

- **Planning** – een enkel schema van maandag tot vrijdag, die van toepassing een snelheid bandbreedte van 1 Mbps tussen 08: 00 en 17: 00-apparaat is. De bandbreedte is voor de rest van de week ingesteld op onbeperkt.

De standaardsjabloon kan worden bewerkt. Het gebruik van deze sjabloon (inclusief de bewerkte versies) wordt bijgehouden.

## <a name="create-an-all-day-bandwidth-template-that-starts-at-a-specified-time"></a>Een hele dag duurt bandbreedte sjabloon maken die op de opgegeven tijd gestart

Volg deze procedure om het maken van een planning die begint op de opgegeven tijd en de hele dag wordt uitgevoerd. In het voorbeeld wordt de planning begint om 9 uur 's morgens en wordt uitgevoerd tot 9 uur de volgende ochtend. Het is belangrijk te weten dat de begin- en eindtijden voor een bepaald schema moeten beide zich bevinden op hetzelfde schema voor 24 uur per dag en kan niet meerdere dagen beslaan. Als u sjablonen bandbreedte meerdere dagen instellen, moet u meerdere schema's gebruiken (zoals in het voorbeeld).

#### <a name="to-create-an-all-day-bandwidth-template"></a>Een hele dag duurt bandbreedte-sjabloon maken

1. Maak een planning die begint om 9 uur 's morgens en tot middernacht uitgevoerd.

2. Een ander schema wilt toevoegen. Configureer het tweede schema voor het uitvoeren van middernacht tot 9 uur's ochtends.

3. De bandbreedte-sjabloon opslaan.

De samengestelde planning vervolgens starten op een moment van uw keuze en voer de hele dag duurt.

## <a name="questions-and-answers-about-bandwidth-templates"></a>Vragen en antwoorden over bandbreedte-sjablonen

**Q**. Wat gebeurt er met de bandbreedte besturingselementen wanneer u tussen de schema's? (Een schema is afgelopen en andere nog niet is gestart).

**A**. In dergelijke gevallen wordt geen bandbreedte-besturingselementen worden gebruikt. Dit betekent dat het apparaat onbeperkte bandbreedte gebruiken kunt bij het trapsgewijs schakelen naar de cloud.

**Q**. Kunt u sjablonen op een apparaat offline bandbreedte wijzigen?

**A**. Niet mogelijk bandbreedte sjablonen op containers volumes wijzigen als het corresponderende apparaat offline is.

**Q**. Kunt u een bandbreedte sjabloon die is gekoppeld aan een container volume wanneer de gekoppelde volumes off line zijn bewerken?

**A**. U kunt een bandbreedte sjabloon die is gekoppeld aan een volume container waarvan volumes off line zijn wijzigen. Houd er rekening mee dat wanneer volumes off line bent, geen gegevens worden trapsgewijs worden geschakeld vanaf het apparaat naar de cloud.

**Q**. Kunt u een standaardsjabloon wilt verwijderen?

**A**. Hoewel u een sjabloon verwijderen kunt, is het niet verstandig om dit te doen. Het gebruik van een standaardsjabloon, met inbegrip van de bewerkte versies worden bijgehouden. De trackingsgegevens worden geanalyseerd en in de loop van de tijd die wordt gebruikt voor het verbeteren van de standaardsjabloon.

**Q**. Hoe bepaal u dat uw bandbreedte sjablonen moeten worden gewijzigd?

**A**. Een van de tekens die u wilt wijzigen de bandbreedte sjablonen is bij het starten van het netwerk traag of onderdrukking meerdere keren in een dag te zien. Als dit gebeurt, kijken naar de grafieken i/o-prestaties en de doorvoer van het netwerk controleren de opslag en het gebruik van het netwerk.

Uit de gegevens van de doorvoer netwerk identificeren het tijdstip en de volume-containers waarin het knelpunt netwerk plaatsvindt. Als dit gebeurt wanneer u gegevens naar de cloud is worden doorverbonden (deze informatie vinden in het i/o-prestaties voor alle containers van volume voor apparaat cloud), hoeft u de bandbreedte sjablonen die zijn gekoppeld aan uw containers volume aanpassen.

Nadat u de gewijzigde sjablonen worden gebruikt, moet u het netwerk opnieuw voor aanzienlijke vertragingstijden controleren. Als deze nog steeds bestaat, moet u terugkeren naar uw bandbreedte-sjablonen.

**Q**. Wat gebeurt er als containers voor meerdere volume op mijn apparaat plant die elkaar overlappen, maar verschillende beperkingen toepassen op elk?

**A**. We gaan ervan uit dat een apparaat met 3 volume containers. De schema's die zijn gekoppeld aan deze containers volledig overlappen. Voor elk van deze containers zijn de bandbreedte gebruikt 5, 10 en 15 Mbps respectievelijk. Wanneer op hetzelfde moment i/o's op alle van deze containers optreden, het minimum van 3 bandbreedte grenzen kan worden toegepast: in dit geval 5 Mbps als i/o-verzoeken de oude delen dezelfde wachtrij.

## <a name="best-practices-for-bandwidth-templates"></a>Aanbevolen procedures voor bandbreedte sjablonen

Volg deze best practices voor het apparaat StorSimple:

- Bandbreedte sjablonen configureren op het apparaat waarmee de variabele beperken van de doorvoer van het netwerk door het apparaat op verschillende tijdstippen van de dag. Deze sjablonen bandbreedte gebruikt in combinatie met een back-up schema's kunnen effectief gebruikmaken van extra netwerkbandbreedte voor cloud-bewerkingen tijdens rustige uren.

- Bereken de werkelijke bandbreedte vereist is voor een bepaalde implementatie op basis van de grootte van de implementatie en de vereiste herstel tijd doelstelling (RTO).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
