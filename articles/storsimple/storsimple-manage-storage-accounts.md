<properties 
   pageTitle="Beheren van uw account StorSimple opslag | Microsoft Azure"
   description="Wordt uitgelegd hoe u kunt de pagina StorSimple Manager configureren gebruiken voor het toevoegen, bewerken, verwijderen of draaien de beveiligingssleutels voor een account voor opslag."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/29/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-your-storage-account"></a>De StorSimple Manager-service gebruiken voor het beheren van uw opslag account

## <a name="overview"></a>Overzicht

De pagina **configureren** bevat alle algemene serviceparameters die kunnen worden gemaakt in de StorSimple Manager-service. Deze parameters kunnen worden toegepast op alle apparaten die zijn verbonden met de service en deze omvatten:

- Opslag-accounts 
- Bandbreedte-sjablonen 
- Records uit Access control 

In deze zelfstudie wordt uitgelegd hoe u kunt de pagina **configureren** wilt toevoegen, bewerken, of opslag accounts verwijderen of draaien de beveiligingssleutels voor een account voor opslag.

 ![Pagina configureren](./media/storsimple-manage-storage-accounts/HCS_ConfigureService.png)  

Opslag accounts bevatten de referenties die het apparaat toegang tot uw account opslag met de cloud-provider gebruikt. Voor Microsoft Azure opslag rekeningen zijn referenties, zoals de naam van de account en de van primaire toegangssleutel. 

Alle opslag-accounts die zijn gemaakt voor de facturering abonnement worden weergegeven op de pagina **configureren** in tabelvorm met de volgende gegevens:

- **Naam** : de unieke naam aan de account is toegewezen toen deze werd gemaakt.
- **SSL ingeschakeld** – of de SSL is ingeschakeld en apparaat-wolk-communicatie via het beveiligde kanaal.
- **Gebruikt door** : het aantal volumes met de account van de opslag.

Er zijn de meest gangbare taken met betrekking tot de opslag-accounts die kunnen worden uitgevoerd op de pagina **configureren** :

- Een opslag toevoegen 
- Een opslag account bewerken 
- Een opslag-account verwijderen 
- Key rotatie van opslag rekeningen 

## <a name="types-of-storage-accounts"></a>Soorten opslag rekeningen

Er zijn drie soorten accounts opslagruimte die kunnen worden gebruikt met uw apparaat StorSimple.

- **Opslag automatisch gegenereerde accounts** – zoals de naam al aangeeft, dit type opslag account wordt automatisch gegenereerd wanneer de service wordt gemaakt. Zie voor meer informatie over hoe deze opslag account is gemaakt, [stap 1: een nieuwe service maken](storsimple-deployment-walkthrough-u1.md#step-1-create-a-new-service) in [uw apparaat op gebouwen StorSimple implementeren](storsimple-deployment-walkthrough.md). 
- **Accounts in het serviceabonnement opslag** – dit zijn de Azure opslag rekeningen die gekoppeld aan het abonnement hetzelfde als die van de service zijn. Zie voor meer informatie over hoe deze opslag accounts worden gemaakt, [Azure opslag rekeningen](../storage/storage-create-storage-account.md). 
- **Opslag rekeningen buiten het serviceabonnement** – dit zijn de Azure opslag rekeningen die niet zijn gekoppeld aan de service en waarschijnlijk bestond voordat de service is gemaakt.

## <a name="add-a-storage-account"></a>Een opslag toevoegen

U kunt een opslag-account toevoegen door middel van een unieke naam en -referenties die zijn gekoppeld aan de account van de opslag (met de opgegeven wolk serviceprovider). U hebt ook de optie van het inschakelen van de modus van secure sockets layer (SSL) voor het maken van een beveiligd kanaal voor communicatie tussen het apparaat en de cloud.

U kunt meerdere accounts maken voor een bepaalde cloud-serviceprovider. Let wel, echter na een opslag-account is gemaakt, kunt u de cloud-serviceprovider niet wijzigen.

Terwijl de opslag account wordt opgeslagen, probeert de service om te communiceren met uw cloud serviceprovider. De referenties en de toegang tot materiaal dat u hebt opgegeven, wordt op dit moment worden geverifieerd. Een opslag-account wordt alleen gemaakt als de verificatie is geslaagd. Als de verificatie mislukt, wordt een toepasselijk foutbericht worden weergegeven.

Resource Manager opslag accounts gemaakt in Azure portal worden ook ondersteund met StorSimple. De bronnenbeheerder opslag rekeningen wordt niet weergegeven in de vervolgkeuzelijst voor de selectie bij het maken van een volume-container, alleen de opslag-accounts die zijn gemaakt in de klassieke Azure portal worden weergegeven. Resource Manager opslag accounts moet worden toegevoegd met behulp van de procedure een opslag toevoegen zoals hieronder beschreven.

> [AZURE.NOTE] De procedure voor het toevoegen van een account opslag verschilt op basis van de StorSimple softwareversie die u gebruikt. Zorg dat u Volg de juiste procedure voor uw versie van StorSimple.


[AZURE.INCLUDE [add-a-storage-account-update1](../../includes/storsimple-configure-new-storage-account-u1.md)]

[AZURE.INCLUDE [add-a-storage-account](../../includes/storsimple-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>Een opslag account bewerken

U kunt een opslag-account die wordt gebruikt door een volume container bewerken. Als u een opslag-account die wordt gebruikt, is het enige veld beschikbaar voor het wijzigen van de sneltoets voor de opslag. U kunt nieuwe opslag sneltoets opgeven en de bijgewerkte instellingen opslaan.

#### <a name="to-edit-a-storage-account"></a>Een opslag account bewerken

1. Op de openingspagina service uw service te selecteren, dubbelklikt u op de servicenaam van de en klik op **configureren**.

2. Klik op **Opslag Accounts toevoegen of bewerken**.

3. Klik in het dialoogvenster **Add/Edit opslag rekeningen** :

  1. Kies een bestaande account die u wilt wijzigen in de vervolgkeuzelijst van de **Rekeningen van de opslag**. Dit kan ook de opslag rekeningen die automatisch zijn gegenereerd wanneer de service is gemaakt.
  2. Indien nodig kunt u de selectie van de **SSL-modus inschakelen** .
  3. U kunt de toegangstoetsen opslag account draaien. Zie [Key rotatie van opslag-accounts](#key-rotation-of-storage-accounts) voor meer informatie over het uitvoeren van belangrijke rotatie.
  4. Klik op het pictogram voor ![Controleer pictogram](./media/storsimple-manage-storage-accounts/HCS_CheckIcon.png) de instellingen op te slaan. De instellingen worden bijgewerkt op de pagina **configureren** . Klik op **Opslaan** om de bijgewerkte instellingen opslaan.

     ![Een opslag account bewerken](./media/storsimple-manage-storage-accounts/HCs_AddEditStorageAccount.png)
  
## <a name="delete-a-storage-account"></a>Een opslag-account verwijderen

> [AZURE.IMPORTANT] Alleen als deze niet wordt gebruikt door een volume container kunt u een opslag account verwijderen. Als een account voor de opslag wordt gebruikt door een volume-container, de container volume eerst verwijderen en vervolgens de bijbehorende opslag verwijderen.

#### <a name="to-delete-a-storage-account"></a>Een opslag-account verwijderen

1. Op de openingspagina StorSimple Manager service uw service te selecteren, dubbelklikt u op de servicenaam van de en klik op **configureren**.

2. In de lijst in tabelvorm van de rekeningen van de opslag, de muisaanwijzer op de account die u wilt verwijderen.

3. Een verwijderpictogram (**x**) weergegeven in de extreme rechterkolom voor opslag. Klik op het pictogram **x** om de referenties te verwijderen.

4. Wanneer u om bevestiging wordt gevraagd, klikt u op **Ja** om door te gaan met het verwijderen. De aanbieding in tabelvorm worden bijgewerkt met de wijzigingen.

## <a name="key-rotation-of-storage-accounts"></a>Key rotatie van opslag rekeningen

Om veiligheidsredenen is key rotatie het vaak een vereiste van datacenters. 

> [AZURE.NOTE] De volgende belangrijke rotatie-informatie en de rotatie-procedure van toepassing op Microsoft Azure opslag alleen accounts. Als u een andere cloud-serviceprovider gebruikt, kunt u beheren opslag account toetsen via het dashboard van de provider.
 
Elk abonnement Microsoft Azure kan een of meer gekoppelde opslag accounts hebben. De toegang tot deze rekeningen wordt bepaald door de toetsen abonnement en toegang voor elke rekening voor opslag. 

Wanneer u een opslag-account maakt, genereert Microsoft Azure twee opslag 512 bits toegangstoetsen die worden gebruikt voor verificatie van de opslag-account is geopend. Met twee opslag toegangstoetsen kunt u opnieuw genereren van sleutels met geen onderbreking met uw opslagservice of toegang tot deze service. De sleutel die wordt gebruikt is de *primaire* sleutel en de back-toets wordt de *secundaire* sleutel genoemd. Een van deze twee sleutels moet worden opgegeven als uw Microsoft Azure StorSimple apparaat toegang uw cloud storage serviceprovider tot.

## <a name="what-is-key-rotation"></a>Wat is de belangrijkste draaien?

Toepassingen gebruiken meestal slechts één van de sleutels voor toegang tot uw gegevens. Na een bepaalde periode, kunt u overschakelen naar de tweede sleutel gebruiken toepassingen hebben. Nadat u uw toepassingen op de secundaire sleutel hebt gekozen, kunt u de eerste sleutel verwijderen en vervolgens een nieuwe sleutel te genereren. Met de twee toetsen op deze manier kunt uw toepassingen toegang tot de gegevens zonder enige uitvaltijd.

De sleutels opslag account worden altijd opgeslagen in de service in gecodeerde vorm. Deze kunnen echter worden hersteld via de StorSimple Manager-service. De service krijgt de primaire en secundaire sleutel voor alle accounts in de opslag in het hetzelfde abonnement, met inbegrip van de accounts die zijn gemaakt in de Storage-service als de standaard opslag accounts die worden gegenereerd wanneer de StorSimple Manager-service is voor het eerst gemaakt. De StorSimple Manager-service wordt altijd deze sleutels ophalen uit de klassieke Azure portal en deze vervolgens opslaan in een gecodeerde wijze.

## <a name="rotation-workflow"></a>Rotatie-werkstroom

Een beheerder Microsoft Azure kunt genereren of de primaire of secundaire sleutel wijzigen door rechtstreeks toegang tot de opslag-account (via de service Microsoft Azure Storage). De StorSimple Manager-service Zie deze wijziging niet automatisch.

Om de StorSimple Manager-service van de wijziging in kennis, moet u toegang tot de service Manager StorSimple toegang tot de account voor de opslag en synchroniseer vervolgens de primaire of secundaire sleutel (afhankelijk van welke is gewijzigd). De service vervolgens haalt de laatste sleutel, de sleutels worden gecodeerd en de gecodeerde sleutel naar het apparaat verzonden.

#### <a name="to-synchronize-keys-for-storage-accounts-in-the-same-subscription-as-the-service-azure-only"></a>Toetsen voor opslag rekeningen in het abonnement hetzelfde als de service (alleen Azure) synchroniseren

1. Klik op de pagina **Services** op het tabblad **configureren** .

2. Klik op **Opslag Accounts toevoegen of bewerken**.

3. In het dialoogvenster het volgende doen:

  1. Selecteer de account van de opslag van de toets die u wilt synchroniseren. De opslag account sleutels worden gecodeerd wanneer ze worden weergegeven.
  2. De StorSimple Manager-service moet u voor het bijwerken van de sleutel die eerder in de Microsoft Azure Storage-service is gewijzigd. Als de primaire toegangstoets (geregenereerde) is gewijzigd, klikt u op **primaire sleutel te synchroniseren**. Als de secundaire sleutel is gewijzigd, klikt u op **secundaire sleutel synchroniseren**.

    ![synchroniseren van sleutels](./media/storsimple-manage-storage-accounts/HCS_KeyRotationStorageAccountSameSubscriptionAsService.png)

#### <a name="to-synchronize-keys-for-storage-accounts-outside-of-the-service-subscription"></a>Voor het synchroniseren van sleutels voor opslag rekeningen buiten het serviceabonnement

1. Klik op de pagina **Services** op het tabblad **configureren** .

2. Klik op **Opslag Accounts toevoegen of bewerken**.

3. In het dialoogvenster het volgende doen:

  1. Selecteer de account van de opslag met de sneltoets die u wilt bijwerken.
  2. U moet de toegangstoets voor opslag in de service Manager StorSimple bijwerken. In dit geval kunt u de toegangstoets opslag zien. Geef de nieuwe sleutel in het vak **Toegangstoets voor opslag Account**y. 
  3. Sla uw wijzigingen. De toegangstoets opslag account moet nu worden bijgewerkt.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [beveiliging van StorSimple](storsimple-security.md).
- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
