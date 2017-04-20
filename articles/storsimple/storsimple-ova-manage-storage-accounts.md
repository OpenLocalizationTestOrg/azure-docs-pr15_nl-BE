<properties 
   pageTitle="Beheren van uw account StorSimple opslag | Microsoft Azure"
   description="Wordt uitgelegd hoe u kunt de pagina configureren StorSimple Manager toevoegen, bewerken, verwijderen of de beveiligingssleutels voor een opslag die is gekoppeld aan de virtuele matrix StorSimple draaien."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="09/29/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-storage-accounts-for-storsimple-virtual-array"></a>De StorSimple Manager-service gebruiken voor het beheren van opslag rekeningen voor StorSimple virtuele matrix

## <a name="overview"></a>Overzicht

De pagina **configureren** bevat de algemene serviceparameters die kunnen worden gemaakt in de StorSimple Manager-service. Deze parameters kunnen worden toegepast op alle apparaten die zijn verbonden met de service en deze omvatten:

- Opslag-accounts 
- Records uit Access control 

In deze zelfstudie wordt uitgelegd hoe u kunt de pagina **configureren** toevoegen, bewerken of verwijderen van accounts opslag voor uw virtuele matrix StorSimple. De informatie in deze zelfstudie is alleen van toepassing op de StorSimple virtuele matrix maart 2016 NH versie van de software wordt uitgevoerd.

 ![Pagina configureren](./media/storsimple-ova-manage-storage-accounts/configure_service_page.png)  

Opslag accounts bevatten de referenties die het apparaat toegang tot uw account opslag met de cloud-provider gebruikt. Voor Microsoft Azure opslag rekeningen zijn referenties, zoals de naam van de account en de van primaire toegangssleutel. 

Alle opslag-accounts die zijn gemaakt voor de facturering abonnement worden weergegeven op de pagina **configureren** in tabelvorm met de volgende gegevens:

- **Naam** : de unieke naam aan de account is toegewezen toen deze werd gemaakt.
- **SSL ingeschakeld** – of de SSL is ingeschakeld en apparaat-wolk-communicatie via het beveiligde kanaal.

Er zijn de meest gangbare taken met betrekking tot de opslag-accounts die kunnen worden uitgevoerd op de pagina **configureren** :

- Een opslag toevoegen 
- Een opslag account bewerken 
- Een opslag-account verwijderen 


## <a name="types-of-storage-accounts"></a>Soorten opslag rekeningen

Er zijn drie soorten accounts opslagruimte die kunnen worden gebruikt met uw apparaat StorSimple.

- **Opslag automatisch gegenereerde accounts** – zoals de naam al aangeeft, dit type opslag account wordt automatisch gegenereerd wanneer de service wordt gemaakt. Zie voor meer informatie over hoe deze opslag account is gemaakt, [een nieuwe service maken](storsimple-ova-manage-service.md#create-a-service). 
- **Accounts in het serviceabonnement opslag** – dit zijn de Azure opslag rekeningen die gekoppeld aan het abonnement hetzelfde als die van de service zijn. Zie voor meer informatie over hoe deze opslag accounts worden gemaakt, [Azure opslag rekeningen](../storage/storage-create-storage-account.md). 
- **Opslag rekeningen buiten het serviceabonnement** – dit zijn de Azure opslag rekeningen die niet zijn gekoppeld aan de service en waarschijnlijk bestond voordat de service is gemaakt.

Elke virtuele matrix StorSimple maakt een container (met een voorvoegsel hcs) in de opslag van gekoppelde account. Deze container heeft de cloud-gegevens voor uw apparaat. Deze container niet verwijderen door deze te openen via de Azure Storage-service als deze actie leiden verlies van gegevens tot zal.

## <a name="add-a-storage-account"></a>Een opslag toevoegen

U kunt een opslag-account toevoegen aan uw configuratie van de service Manager StorSimple door middel van een unieke naam en -referenties die zijn gekoppeld aan de account voor de opslag. U hebt ook de optie van het inschakelen van de modus van secure sockets layer (SSL) voor het maken van een beveiligd kanaal voor communicatie tussen het apparaat en de cloud.

U kunt meerdere accounts maken voor een bepaalde cloud-serviceprovider. Terwijl de opslag account wordt opgeslagen, probeert de service om te communiceren met uw cloud serviceprovider. De referenties en de toegang tot materiaal dat u hebt opgegeven, wordt op dit moment worden geverifieerd. Een opslag-account wordt alleen gemaakt als de verificatie is geslaagd. Als de verificatie mislukt, wordt een toepasselijk foutbericht worden weergegeven.

Resource Manager opslag accounts gemaakt in Azure portal worden ook ondersteund met StorSimple. De bronnenbeheerder opslag rekeningen weergegeven niet in de vervolgkeuzelijst voor de selectie, alleen de opslag accounts die zijn gemaakt in de klassieke Azure portal wordt weergegeven. Resource Manager opslag accounts moet worden toegevoegd met behulp van de procedure een toevoegen opslag zoals hieronder beschreven.

De procedure voor het toevoegen van een account Azure klassieke opslag wordt hieronder beschreven.

[AZURE.INCLUDE [add-a-storage-account](../../includes/storsimple-ova-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>Een opslag account bewerken

Kunt u een opslag-account die wordt gebruikt door het apparaat. Als u een opslag-account die wordt gebruikt, zijn de velden die beschikbaar zijn voor het wijzigen van de toegangstoets en de SSL-modus voor de opslag. U kunt nieuwe opslag sneltoets opgeven of wijzigen de Modusselectie **SSL inschakelen** en de bijgewerkte instellingen opslaan.

#### <a name="to-edit-a-storage-account"></a>Een opslag account bewerken

1. Op de openingspagina service uw service te selecteren, dubbelklikt u op de servicenaam van de en klik op **configureren**.

2. Klik op **Opslag Accounts toevoegen of bewerken**.

3. Klik in het dialoogvenster **Add/Edit opslag rekeningen** :

  1. Kies een bestaande account die u wilt wijzigen in de vervolgkeuzelijst van de **Rekeningen van de opslag**. 
  2. Indien nodig kunt u de selectie van de **SSL-modus inschakelen** .
  3. U kunt uw opslag account toegang sleutels te genereren. Voor meer informatie, Zie [de opslag account sleutels te genereren](storage-create-storage-account.md#manage-your-storage-access-keys). De nieuwe sleutel voor opslag account opgeven. Dit is de primaire toegangstoets voor een account Azure opslag. 
  4. Klik op het pictogram voor ![Controleer pictogram](./media/storsimple-ova-manage-storage-accounts/checkicon.png) de instellingen op te slaan. De instellingen worden bijgewerkt op de pagina **configureren** . 
  5. Onderaan op de pagina, klikt u op **Opslaan** om de bijgewerkte instellingen opslaan. 

     ![Een opslag account bewerken](./media/storsimple-ova-manage-storage-accounts/modifyexistingstorageaccount.png)
  
## <a name="delete-a-storage-account"></a>Een opslag-account verwijderen

> [AZURE.IMPORTANT] Alleen als deze niet gebruikt wordt, kunt u een opslag account verwijderen. Als u een account voor de opslag wordt gebruikt, wordt u gewaarschuwd.

#### <a name="to-delete-a-storage-account"></a>Een opslag-account verwijderen

1. Op de openingspagina StorSimple Manager service uw service te selecteren, dubbelklikt u op de servicenaam van de en klik op **configureren**.

2. In de lijst in tabelvorm van de rekeningen van de opslag, de muisaanwijzer op de account die u wilt verwijderen.

3. Een verwijderpictogram (**x**) weergegeven in de extreme rechterkolom voor opslag. Klik op het pictogram **x** om de referenties te verwijderen.

4. Wanneer u om bevestiging wordt gevraagd, klikt u op **Ja** om door te gaan met het verwijderen. De aanbieding in tabelvorm worden bijgewerkt met de wijzigingen.

5. Onderaan op de pagina, klikt u op **Opslaan** om de bijgewerkte instellingen opslaan.


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [beheren van uw virtuele matrix StorSimple](storsimple-ova-web-ui-admin.md).
