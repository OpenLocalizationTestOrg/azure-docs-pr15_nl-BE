<properties 
   pageTitle="Neem contact op met Microsoft Support | Microsoft Azure"
   description="Informatie over het maken van een verzoek om ondersteuning en support-sessie start op uw apparaat StorSimple."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="contact-microsoft-support"></a>Neem contact op met Microsoft Support

Als u problemen met uw Microsoft Azure StorSimple oplossing ondervindt, kunt u een aanvraag voor technische ondersteuning. In een online sessie met de ondersteuningstechnicus moet u mogelijk ook een ondersteuningssessie te starten op uw apparaat StorSimple. Dit artikel helpt u bij:

- Het maken van een verzoek om ondersteuning.
- Het starten van een ondersteuningssessie in de Windows PowerShell-interface van uw apparaat StorSimple.

Bekijk de [StorSimple 8000 Series Support Sla's en gegevens](https://msdn.microsoft.com/library/mt433077.aspx) voordat u een aanvraag voor ondersteuning.

## <a name="create-a-support-request"></a>Maak een aanvraag voor ondersteuning

Voer de volgende stappen voor het maken van een aanvraag voor ondersteuning:

#### <a name="to-create-a-support-request"></a>Voor het maken van een verzoek om ondersteuning

1. Klik op uw accountnaam en klik op **Contact met Microsoft-ondersteuning**in de [Azure klassieke portal](https://manage.windowsazure.com/), in de rechterbovenhoek.

    ![Ondersteuning van MS contact via ManagementPortal](./media/storsimple-contact-microsoft-support/Ibiza1.png)

2. U wordt omgeleid naar de nieuwe Azure portal (portal.azure.com). Klik op de tegel **nieuw ondersteuningsverzoek** .

    ![Neem contact op met MS Support via de nieuwe portal](./media/storsimple-contact-microsoft-support/Ibiza2.png)

    Aan de rechterkant van het scherm verschijnt het deelvenster **nieuw ondersteuningsverzoek** . 

    ![Nieuwe ondersteuning voor aanvraag deelvenster](./media/storsimple-contact-microsoft-support/Ibiza3a.png)

3. Vul het volgende in het dialoogvenster **Grondbeginselen** :                                
    1. Selecteer de vervolgkeuzelijst **type probleem** **technische**.
    2. Selecteer een **abonnement** in de vervolgkeuzelijst.
    3. Selecteer in de vervolgkeuzelijst **Service** **StorSimple**. 
    4. Selecteer een **ondersteuningsplan** in de vervolgkeuzelijst. U moet een ondersteuningsplan voor betaalde inschakelen van technische ondersteuning.

4. Klik op **volgende**. Het **probleem** van het dialoogvenster wordt weergegeven.

    ![Nieuwe ondersteuning voor aanvraag deelvenster](./media/storsimple-contact-microsoft-support/Ibiza5a.png) 

5. Vul het volgende in het dialoogvenster **probleem** :

    1.  Selecteer **een prioriteitsniveau** in de lijst.
    2.  Selecteer een **type probleem** in de vervolgkeuzelijst.
    3.  Selecteer een **categorie** in de vervolgkeuzelijst. 
    4.  In het vak **Details van** een korte beschrijving van uw probleem.
    5.  In het vak **periode** geven de datum, tijd en tijdzone die overeenkomt met het meest recente exemplaar van uw probleem.
    6.  Onder **bestand uploaden**, klikt u op het mappictogram om te bladeren naar de support-pakket.
    7.  Schakel het selectievakje **Diagnostische gegevens delen** .

6. Klik op **volgende**. In het dialoogvenster **gegevens van de contactpersoon** wordt weergegeven.

    ![Nieuwe ondersteuning voor aanvraag deelvenster](./media/storsimple-contact-microsoft-support/Ibiza6a.png) 

7. Voer uw contactgegevens in en selecteer een contactpersoon (telefoon of e-mail). 

8. Schakel het selectievakje **opslaan van wijzigingen in contactgegevens voor toekomstige ondersteuningsaanvragen** .

9. Klik op **maken**.

Nadat u uw aanvraag hebt ingediend, een technisch medewerker neemt contact met u zo spoedig mogelijk om door te gaan met uw aanvraag.

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a>Start een ondersteuningssessie in Windows PowerShell voor StorSimple

Voor het oplossen van eventuele problemen die u bij het StorSimple apparaat ondervindt mogelijk, moet u deel te nemen met het Microsoft Support team. Microsoft Support mogelijk een support-sessie aan te melden op uw apparaat. 

Voer de volgende stappen uit om een support sessie te starten:

#### <a name="to-start-a-support-session"></a>Een support-sessie te starten

1. Toegang tot het apparaat via de seriële console of via een Telnet-sessie vanaf een externe computer. De stappen hiervoor in [Gebruik stopverf verbinding maken met de seriële console apparaat](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

2. Druk op **ENTER om een opdrachtprompt** in de sessie wordt geopend.

3. In de seriële console-menu de optie 1, **Meld u aan met volledige toegang**.

4. Typ bij de opdrachtprompt het volgende wachtwoord: 

    `Password1`

5. Typ de volgende opdracht bij de opdrachtprompt:

    `Enable-HcsSupportAccess`

6. Een gecodeerde tekenreeks wordt aan u gepresenteerd. Kopieer deze reeks in een teksteditor zoals Kladblok.

7. Deze tekenreeks opslaan en verzenden in een e-mailbericht naar Microsoft Support. 

> [AZURE.IMPORTANT] U kunt ondersteuning toegang uitschakelen door het uitvoeren van `Disable-HcsSupportAccess`. Het apparaat StorSimple wordt ook geprobeerd access ondersteuning uitschakelen 8 uur na de sessie is gestart. Het is raadzaam uw inloggegevens StorSimple wijzigen nadat een support-sessie is gestart.
