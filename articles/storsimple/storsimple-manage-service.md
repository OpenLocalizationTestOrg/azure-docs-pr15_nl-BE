<properties 
   pageTitle="De StorSimple Manager-service implementeren | Microsoft Azure"
   description="Hoe maken en de StorSimple Manager-service in de klassieke Azure portal verwijderen en wordt beschreven hoe u de sleutel voor de inschrijving te beheren."
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="deploy-the-storsimple-manager-service"></a>De StorSimple Manager-service implementeren

## <a name="overview"></a>Overzicht

De StorSimple Manager-service wordt uitgevoerd in Microsoft Azure en verbinding maakt met meerdere StorSimple apparaten. Nadat u de service hebt gemaakt, kunt u deze beheren de apparaten uit de Microsoft Azure klassieke portal wordt uitgevoerd in een browser. Hiermee kunt u voor het controleren van de apparaten die zijn verbonden met de service Manager StorSimple vanaf een centrale locatie, waardoor administratieve lasten te minimaliseren.

De openingspagina StorSimple Manager worden alle services StorSimple Manager die u gebruiken kunt voor het beheren van uw StorSimple opslagapparaten. De volgende informatie is bedoeld voor elke service StorSimple Manager op de pagina beheer StorSimple:

- **Naam** : de naam die aan uw StorSimple Manager-service is toegewezen toen deze werd gemaakt. De servicenaam van de kan niet worden gewijzigd nadat de service is gemaakt.

- **Status** : de status van de service, die **actief** **maken**of **Online worden kan**.

- **Locatie** : de geografische locatie waar het apparaat StorSimple wordt geïmplementeerd.

- **Abonnement** : het factuuradres abonnement dat is gekoppeld aan uw service.

Algemene taken die kunnen worden uitgevoerd via de pagina StorSimple Manager zijn:

- Een service maken
- Een service verwijderen
- Ophalen van de sleutel voor de inschrijving
- De registratie sleutel opnieuw genereren

In deze zelfstudie wordt beschreven hoe u deze taken uitvoert.

## <a name="create-a-service"></a>Een service maken

Gebruik de optie **Snelle invoer van** een service Manager StorSimple maken als u wilt dat uw apparaat StorSimple te implementeren. Als u een service maakt, nodig hebt u:

- Een abonnement met een Enterprise Agreement
- Een actieve Microsoft Azure opslag account
- De informatie die wordt gebruikt voor toegangsbeheer

U kunt ook een standaardaccount voor opslag te genereren bij het maken van de service.

Een enkele service kan meerdere apparaten te beheren. Een apparaat kan niet echter meerdere services omvatten. Een grote onderneming, kan meerdere exemplaren van de service voor het werken met verschillende abonnementen, organisaties of zelfs implementatie locaties hebben. Houd er rekening mee dat moet u afzonderlijke exemplaren van de service Manager StorSimple apparaten StorSimple 8000-serie en de StorSimple virtuele Arrays beheren.

Voer de volgende stappen uit om een service te maken.

[AZURE.INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

## <a name="delete-a-service"></a>Een service verwijderen

Voordat u een service verwijdert, zorg dat er geen aangesloten apparaten gebruiken. Als de service gebruikt wordt, schakelt u de aangesloten apparaten. De bewerking deactiveren de verbinding tussen het apparaat en de service Server, maar de apparaatgegevens in de cloud te bewaren. 

[AZURE.IMPORTANT] Een service is verwijderd, de bewerking kan niet ongedaan worden gemaakt. Elk apparaat dat werd gebruikt door de service moeten worden voordat deze kan worden gebruikt met een andere service factory. In dit scenario worden de lokale gegevens op het apparaat, als de configuratie, verloren gaan.

Voer de volgende stappen uit om een service te verwijderen.

### <a name="to-delete-a-service"></a>Een service verwijderen

1. Selecteer de service die u wilt verwijderen op de pagina **StorSimple Manager-service** .

1. Klik op **verwijderen** onder aan de pagina.

1. Klik op **Ja** in het bericht van de bevestiging. Duurt het enkele minuten duren voordat de service moet worden verwijderd.

## <a name="get-the-service-registration-key"></a>Ophalen van de sleutel voor de inschrijving

Nadat u een service hebt gemaakt, moet u het apparaat StorSimple registreren bij de service. Als u wilt uw eerste StorSimple apparaat registreren, moet u de sleutel voor de inschrijving. Om extra apparaten met een bestaande StorSimple-service registreert, moet u zowel de sleutel van de registratie en de coderingssleutel voor service gegevens (die wordt gegenereerd op het eerste apparaat tijdens de registratie). Zie voor meer informatie over de sleutel data service [StorSimple beveiliging](storsimple-security.md). De sleutel van de registratie kunt u krijgen via de **Sleutel van de registratie** op de pagina **Services** .

Voer de volgende stappen uit als u de sleutel voor de inschrijving.

[AZURE.INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]

De sleutel voor de inschrijving op een veilige plaats bewaren. U moet deze sleutel, als de service gegevens coderingssleutel, extra apparaten registreren bij deze service. Nadat de sleutel voor de inschrijving, moet u uw apparaat via de Windows PowerShell voor StorSimple-interface te configureren.

Zie voor meer informatie over het gebruik van deze registratie sleutel [stap 3: configureren en registreren van het apparaat via Windows PowerShell voor StorSimple](storsimple-deployment-walkthrough.md#step-2-configure-and-register-the-device-through-windows-powershell-for-storsimple).

## <a name="regenerate-the-service-registration-key"></a>De registratie sleutel opnieuw genereren

U wilt een registratie sleutel opnieuw genereren als u belangrijke rotatie uitvoeren of als de lijst met servicebeheerders is gewijzigd. Wanneer u de sleutel opnieuw genereren, wordt de nieuwe sleutel alleen gebruikt voor het registreren van de volgende apparaten. De apparaten die al zijn geregistreerd, worden niet beïnvloed door dit proces.

Voer de volgende stappen uit om een registratie sleutel opnieuw genereren.

### <a name="to-regenerate-the-service-registration-key"></a>De registratie sleutel genereren

1. Klik op de pagina **service Manager StorSimple** **Registratie sleutel**.

1. Klik op **toevoegen**in het dialoogvenster **Sleutel voor de inschrijving** .

1. Ziet u een bevestigingsbericht. Klik op **OK** om door te gaan met de regeneratie.

1. Een nieuwe service registratie sleutel wordt weergegeven.

1. Deze sleutel kopiëren en opslaan voor het registreren van nieuwe apparaten met deze service.

1. Klik op het pictogram ![Pictogram](./media/storsimple-manage-service/HCS_CheckIcon.png) om dit dialoogvenster te sluiten.


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [implementatie van StorSimple verwerken](storsimple-deployment-walkthrough.md).

- Meer informatie over het [beheren van uw account StorSimple opslag](storsimple-manage-storage-accounts.md).

- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).

 
