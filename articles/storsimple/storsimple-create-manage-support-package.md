<properties
   pageTitle="Maak een pakket StorSimple ondersteuning | Microsoft Azure"
   description="Informatie over het maken en bewerken van een ondersteuningspakket voor uw apparaat StorSimple decoderen."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />


# <a name="create-and-manage-a-storsimple-support-package"></a>Maken en beheren van een StorSimple support-pakket

## <a name="overview"></a>Overzicht

Een StorSimple support-pakket is een mechanisme voor gemakkelijk te gebruiken dat alle relevante Logboeken om te helpen Microsoft Support verzamelt bij het oplossen van eventuele problemen met de StorSimple apparaat. De verzamelde logboeken worden gecodeerd en gecomprimeerd.

Deze zelfstudie bevat stapsgewijze instructies voor het maken en beheren van de support-pakket.

## <a name="create-and-upload-a-support-package-in-the-azure-classic-portal"></a>Maken en uploaden van een support-pakket in de klassieke Azure portal

U kunt maken en support-pakket te uploaden naar de website van Microsoft Support via de pagina **onderhoud** van de service in de klassieke Azure portal.

> [AZURE.NOTE] Een support-sleutel nodig is voor het uploaden. De ondersteuningstechnicus dient dit aan u in een e-mailbericht.

Een gecodeerde en gecomprimeerde support-pakket (CAB-bestand) wordt gemaakt en geüpload naar de site ondersteuning. Vervolgens kan de ondersteuningstechnicus van dit pakket opgehaald van de website voor ondersteuning voor het oplossen van het probleem.

De volgende stappen uitvoeren in de klassieke portal een support-pakket maken.

#### <a name="to-create-a-support-package-in-the-azure-classic-portal"></a>Een support-pakket in de klassieke Azure portal maken

1. Selecteer **apparaten** > **onderhoud**.

2. Selecteer in de sectie **Support pakket** **support-pakket maken en uploaden**.

3. Klik in het dialoogvenster **maken en uploaden support-pakket** door het volgende te doen:

    ![Support-pakket maken](./media/storsimple-create-manage-support-package/IC740923.png)

    - Geef de sleutel in het tekstvak **Sleutel ondersteuning** . De ondersteuningstechnicus van Microsoft moet deze sleutel per e-mail naar u verzenden.

    - Schakel het selectievakje in als u toestemming geven om automatisch support-pakket te uploaden naar de website van Microsoft Support.

    - Klik op het pictogram ![Pictogram](./media/storsimple-create-manage-support-package/IC740895.png).


## <a name="manually-create-a-support-package"></a>Maak handmatig een support-pakket

In sommige gevallen moet u handmatig de support-pakket via Windows PowerShell voor StorSimple te maken. Bijvoorbeeld:

- Als u gevoelige gegevens verwijderen uit de logboekbestanden voor delen met Microsoft Support.

- Als u problemen bij het uploaden van het pakket verbindingsproblemen ondervindt.

U kunt uw ondersteuningspakket handmatig gegenereerde delen met Microsoft Support via e-mail. Voer de volgende stappen uit om een ondersteuningspakket in Windows PowerShell voor StorSimple te maken.

#### <a name="to-create-a-support-package-in-windows-powershell-for-storsimple"></a>Een support-pakket maken in Windows PowerShell voor StorSimple

1. Een Windows PowerShell-sessie starten als beheerder op de externe computer waarmee u verbinding maakt met uw apparaat StorSimple, voer de volgende opdracht:

    `Start PowerShell`

2. In de Windows PowerShell-sessie, verbinding maken met de Console van de SSAdmin van het apparaat:

    - Voer bij de opdrachtprompt:

        `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`

    1. Voer in het dialoogvenster het beheerderswachtwoord. Het standaardwachtwoord is:

        `Password1`

        ![In het dialoogvenster PowerShell-referentie](./media/storsimple-create-manage-support-package/IC740962.png)

    2. Klik op **OK**.
    1. Voer bij de opdrachtprompt:

        `Enter-PSSession $MS`

3. Voer de gewenste opdracht in de sessie wordt geopend.

    - Voer voor netwerkshares die beveiligd met een wachtwoord zijn:

        `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`

        Wordt u gevraagd een wachtwoord in te voeren, een pad naar de gedeelde netwerkmap en een wachtwoordzin codering (omdat de support-pakket is gecodeerd). Een support-pakket wordt vervolgens in de opgegeven map gemaakt.

    - Voor shares die niet beveiligd met een wachtwoord zijn, hoeft u niet de `-Credential` parameter. Voer de volgende gegevens:

        `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`

        De support-pakket is gemaakt voor beide domeincontrollers in de opgegeven gedeelde netwerkmap. Het is een gecodeerd, gecomprimeerd bestand dat kan worden verzonden naar Microsoft Support voor het oplossen van problemen. Zie voor meer informatie [Contact met Microsoft-ondersteuning](storsimple-contact-microsoft-support.md).


### <a name="the-export-hcssupportpackage-cmdlet-parameters"></a>De parameters van de cmdlet Export-HcsSupportPackage
U kunt de volgende parameters gebruiken met de cmdlet Export-HcsSupportPackage.

| Parameter            | Vereist/optioneel | Beschrijving                                                                                                                                                             |
|----------------------|-------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `-Path`                 | Vereist          | Gebruiken om de locatie van de gedeelde netwerkmap waarin de support-pakket wordt geplaatst.                                                                 |
| `-EncryptionPassphrase` | Vereist          | Gebruik een wachtwoordzin te support-pakket te coderen.                                                                                                        |
| `-Credential`           | Optioneel          | Gebruik access referenties voor de gedeelde netwerkmap.                                                                                        |
| `-Force`                | Optioneel          | Met kunt u de codering wachtwoordzin bevestiging overslaan.                                                                                                                |
| `-PackageTag`           | Optioneel          | Met geeft u een map onder het *pad* waarin de support-pakket wordt geplaatst. De standaardinstelling is [naam]-[huidige datum en time:yyyy-MM-dd-HH-mm-ss].       |
| `-Scope`                | Optioneel          | **Cluster** (standaard) voor het maken van een ondersteuningspakket voor beide domeincontrollers opgeven. Als u maken van een pakket alleen voor de huidige controller wilt, geef **Controller**. |


## <a name="edit-a-support-package"></a>Een support-pakket bewerken

Nadat u een support-pakket hebt gegenereerd, moet u mogelijk voor het bewerken van het pakket om gevoelige gegevens te verwijderen. Dit kunt volumenamen, apparaat IP-adressen en namen van de logboekbestanden van back-up opnemen.

> [AZURE.IMPORTANT] U kunt alleen een support-pakket dat is gegenereerd via Windows PowerShell voor StorSimple bewerken. U kunt een pakket gemaakt in Azure klassieke portal in StorSimple Manager-service niet bewerken.

Een support-pakket bewerken voordat u het uploaden naar de website van Microsoft Support eerst decodeert het ondersteuningspakket, de bestanden bewerken en vervolgens opnieuw coderen. Voer de volgende stappen.

#### <a name="to-edit-a-support-package-in-windows-powershell-for-storsimple"></a>Een ondersteuningspakket in Windows PowerShell voor StorSimple bewerken

1. Een support-pakket genereren zoals eerder beschreven, in [een ondersteuningspakket in Windows PowerShell voor StorSimple maken](#to-create-a-support-package-in-windows-powershell-for-storsimple).

2. [Download het script](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) lokaal op de client.

3. Importeer de module voor Windows PowerShell. Geef het pad naar de lokale map waarin u het script hebt gedownload. Voer voor het importeren van de module:

    `Import-module <Path to the folder that contains the Windows PowerShell script>`

4. Alle bestanden zijn *.aes* -bestanden die zijn gecomprimeerd en gecodeerd. Als u wilt decomprimeren en decoderen van bestanden, invoeren:

    `Open-HcsSupportPackage <Path to the folder that contains support package files>`

    Houd er rekening mee dat de werkelijke bestandsextensies voor alle bestanden worden weergegeven.

    ![Support-pakket bewerken](./media/storsimple-create-manage-support-package/IC750706.png)

5. Wanneer u wordt gevraagd voor de codering wachtwoordzin, de wachtwoordzin invoeren die u hebt gebruikt om de support-pakket is gemaakt.

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:EncryptionPassphrase: ****

6. Blader naar de map waarin de logboekbestanden. Aangezien de logboekbestanden worden nu gedecomprimeerd en gedecodeerd, wordt deze oorspronkelijke extensies hebben. Deze bestanden wijzigt om klantspecifieke informatie, zoals volumenamen en IP-adressen van apparaat verwijderen en de bestanden opslaan.

7. Sluit de bestanden te comprimeren met gzip en versleuteld met AES-256. Dit is de snelheid en beveiliging bij het overbrengen van het ondersteuningspakket via een netwerk. Als u wilt comprimeren en coderen van bestanden, u het volgende:

    `Close-HcsSupportPackage <Path to the folder that contains support package files>`

    ![Support-pakket bewerken](./media/storsimple-create-manage-support-package/IC750707.png)

8. Geef desgevraagd een wachtwoordzin codering voor de gewijzigde support-pakket.

        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for the following parameters:EncryptionPassphrase: ****

9. Noteer de nieuwe wachtwoordzin, zodat u deze met Microsoft Support delen kunt verzoek.


### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a>Voorbeeld: Bestanden bewerken in een support-pakket in een wachtwoord beveiligde share

In het volgende voorbeeld ziet u hoe te decoderen, te bewerken en opnieuw coderen support-pakket.

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1

        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Close-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32>

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [ondersteuningspakketten en logboeken apparaat oplossen van problemen met de implementatie van uw apparaat](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting)gebruiken.

- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
