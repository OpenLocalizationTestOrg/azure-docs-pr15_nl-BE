<properties 
   pageTitle="PowerShell voor Apparaatbeheer StorSimple | Microsoft Azure"
   description="Informatie over het beheren van uw apparaat StorSimple met Windows PowerShell voor StorSimple."
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
   ms.date="08/18/2016"
   ms.author="alkohli@microsoft.com" />

# <a name="use-windows-powershell-for-storsimple-to-administer-your-device"></a>Windows PowerShell voor StorSimple gebruiken voor het beheren van uw apparaat

## <a name="overview"></a>Overzicht

Windows PowerShell voor StorSimple biedt een opdrachtregelinterface waarmee u uw apparaat met Microsoft Azure StorSimple beheren. Zoals de naam al aangeeft, is Windows PowerShell gebaseerd interface met een opdrachtregel die is ingebouwd in een beperkte runspace. Vanuit het perspectief van de gebruiker op de opdrachtregel, wordt een beperkte runspace weergegeven als een beperkte versie van Windows PowerShell. Deze interface heeft terwijl een aantal essentiële mogelijkheden van Windows PowerShell extra speciale cmdlets die zijn gericht op het beheren van uw Microsoft Azure StorSimple apparaat. 

Dit artikel beschrijft de Windows PowerShell voor StorSimple-functies, met inbegrip van de manier waarop u verbinding kunt maken met deze interface, en bevat koppelingen naar stapsgewijze procedures of werkstromen die u kunt uitvoeren met behulp van deze interface. De workflows behoren het registreren van uw apparaat, de netwerkinterface op het apparaat te configureren, installeren van updates waarvoor het apparaat in de onderhoudsmodus, wijzigt de status van het apparaat, en los eventuele problemen die kunnen optreden.

Na het lezen van dit artikel, is het mogelijk om:

- Verbinding maken met het gebruik van Windows PowerShell voor StorSimple StorSimple-apparaat.

- Het gebruik van Windows PowerShell voor StorSimple StorSimple-apparaat beheren.

- Help-informatie opvragen in Windows PowerShell voor StorSimple.

>[AZURE.NOTE]   

>- Windows PowerShell voor StorSimple cmdlets kunt u uw apparaat StorSimple beheren vanaf een seriële console of op afstand via Windows PowerShell remoting. Ga voor meer informatie over elk van de afzonderlijke cmdlets die in deze interface kunnen worden gebruikt om te [verwijzen naar de cmdlets voor Windows PowerShell voor StorSimple](https://technet.microsoft.com/library/dn688168.aspx).

>- De StorSimple van Azure PowerShell-cmdlets zijn een andere verzameling van cmdlets waarmee u StorSimple serviceniveau te automatiseren en migratietaken vanaf de opdrachtregel. Ga naar de [Azure StorSimple cmdlet referentie](https://msdn.microsoft.com/library/azure/dn920427.aspx)voor meer informatie over de Azure PowerShell-cmdlets voor StorSimple.

U kunt de Windows PowerShell voor StorSimple met behulp van een van de volgende manieren openen:

- [Verbinding maken met StorSimple apparaat seriële console](#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console)
- [Extern verbinding maken met StorSimple met Windows PowerShell](#connect-remotely-to-storsimple-using-windows-powershell-for-storsimple)
    

## <a name="connect-to-windows-powershell-for-storsimple-via-the-device-serial-console"></a>Verbinding maken met Windows PowerShell voor StorSimple via de seriële console apparaat

U kunt [download stopverf](http://www.putty.org/) of soortgelijke terminal-emulatiesoftware StorSimple verbinding maken met Windows PowerShell. U moet configureren, stopverf speciaal voor toegang tot het Microsoft Azure StorSimple apparaat. De volgende onderwerpen bevatten de gedetailleerde stappen over het configureren van stopverf en verbinding maken met het apparaat. Verschillende opties voor het menu in de seriële console worden ook toegelicht.

### <a name="putty-settings"></a>Stopverf instellingen

Zorg dat u de volgende instellingen stopverf verbinding maken met de Windows PowerShell-interface van de seriële console.

#### <a name="to-configure-putty"></a>Stopverf configureren

1. Selecteer in het deelvenster **categorie** in het dialoogvenster stopverf **herconfiguratie** **toetsenbord**.

2. Zorg ervoor dat de volgende opties zijn ingeschakeld (dit zijn de standaardinstellingen als u een nieuwe sessie start). 

  	|Toetsenbord item|Selecteer|
  	|---|---|
  	|BACKSPACE-toets|Besturingselement-? (127)|
  	|Huis- en -toetsen|Standaard|
  	|Functietoetsen en toetsenblok|ESC [n ~|
  	|Aanvankelijke status van cursortoetsen|Normaal|
  	|De begintoestand van het numerieke toetsenblok|Normaal|
  	|Toetsenbord met extra functies inschakelen|CTRL + ALT + verschilt van AltGr|

    ![Ondersteunde stopverf instellingen](./media/storsimple-windows-powershell-administration/IC740877.png)

3. Klik op **toepassen**.

4. Selecteer in het deelvenster **categorie** **vertaling**.

5. Selecteer in de keuzelijst **externe tekenset** **UTF-8**.

6. Selecteer onder **lijntekening tekens verwerken**, **codepunten voor Unicode gebruiken lijntekening**. De volgende afbeelding ziet u de juiste selecties stopverf.

    ![UTF stopverf instellingen](./media/storsimple-windows-powershell-administration/IC740878.png)

7. Klik op **toepassen**.


U kunt nu stopverf verbinding maken met de seriële console apparaat met de volgende stappen uit te voeren.

[AZURE.INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]


### <a name="about-the-serial-console"></a>Over de seriële console

Wanneer u de Windows PowerShell-interface van uw StorSimple apparaat via de seriële console, een bannerbericht wordt weergegeven, gevolgd door menu-opties. 

Het paginavaandel bericht bevat StorSimple apparaat basisgegevens zoals model, naam, versie van de geïnstalleerde software en de status van de domeincontroller die u wilt openen. De volgende afbeelding toont een voorbeeld van een bannerbericht.

![Seriële bannerbericht](./media/storsimple-windows-powershell-administration/IC741098.png)

>[AZURE.IMPORTANT] Het paginavaandel bericht kunt u aangeven of de controller die u met verbonden bent actief of passief is.

De volgende afbeelding ziet u de verschillende runspace opties die beschikbaar in het menu van de seriële console zijn.

![Registreer uw apparaat 2](./media/storsimple-windows-powershell-administration/IC740906.png)

U kunt kiezen uit de volgende instellingen:

1. **Meld u aan met een volledige toegang** Met deze optie kunt u verbinding kunt maken (met de juiste referenties) naar de **SSAdminConsole** -runspace op de lokale domeincontroller. (De lokale domeincontroller is de controller die u momenteel via de seriële console van uw apparaat StorSimple opent.) Deze optie kan ook worden gebruikt om Microsoft Support toegang krijgen tot onbeperkte runspace (ondersteuningssessie) om problemen met alle mogelijke apparaten. Nadat u optie 1 aan te melden, kunt u de Microsoft Support-technicus toegang krijgen tot onbeperkte runspace door een bepaalde cmdlet wordt uitgevoerd. Raadpleeg voor meer informatie een support sessie te [starten](storsimple-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple).

2. **Log in op de peer-controller met volledige toegang** Deze optie is hetzelfde als 1, behalve dat u verbinding (met de juiste referenties) naar de **SSAdminConsole** -runspace op de peer-controller maken kunt. Omdat de StorSimple-apparaat een apparaat met hoge beschikbaarheid met twee controllers in de configuratie van een actief-passief is, peer heeft betrekking op de andere controller in het apparaat dat u via de seriële console benaderen wilt).
Net als bij optie 1, deze optie kan ook worden gebruikt om Microsoft Support toegang krijgen tot onbeperkte runspace op een domeincontroller peer.

3. **Verbinding maken met beperkte toegang** Deze optie wordt gebruikt voor toegang tot Windows PowerShell-interface in de beperkte modus. Niet gevraagd om toegangsreferenties. Deze optie verbindt met een meer beperkte runspace vergeleken met de opties 1 en 2.  Enkele van de taken die beschikbaar via de optie 1 zijn die **kan niet* worden uitgevoerd in deze runspace zijn:

    - De fabrieksinstellingen herstellen
    - Het wachtwoord wijzigen
    - In- of uitschakelen van de ondersteuning voor access
    - Updates toepassen
    - Hotfixes installeren 
                                                

    >[AZURE.NOTE] **Dit is de beste optie als u het apparaat administrator-wachtwoord vergeten bent en geen verbinding kunt met behulp van optie 1 of 2 maken.**

4. **Taal wijzigen** Met deze optie kunt u de weergavetaal op de Windows PowerShell-interface wijzigen. De talen die worden ondersteund zijn Engels, Japans, Russisch, Frans, Zuid-Koreaans, Spaans, Italiaans, Duits, Chinees en Portugees (Brazilië).


## <a name="connect-remotely-to-storsimple-using-windows-powershell-for-storsimple"></a>Extern verbinding maken met behulp van Windows PowerShell voor StorSimple StorSimple

U kunt Windows PowerShell remoting verbinding maken met uw apparaat StorSimple. Als u op deze manier verbinding maakt, ziet u een menu. (Ziet u een menu alleen als u de seriële console op het apparaat om verbinding te maken. Externe verbindingen gaat u rechtstreeks naar het equivalent van 'optie 1 – volledige toegang' op de seriële console.) Met Windows PowerShell remoting, de verbinding met een specifieke runspace. U kunt ook de taal opgeven. 

De weergavetaal is onafhankelijk van de taal die u hebt ingesteld met behulp van de optie **Taal wijzigen** in het menu van de seriële console. Remote PowerShell gewoon automatisch door de landinstellingen van het apparaat verbinding als er geen is opgegeven.

>[AZURE.NOTE] Als u met Microsoft Azure virtual hosts en StorSimple virtuele apparaten werkt, kunt u Windows PowerShell remoting en de virtuele host verbinding maken met het virtuele apparaat. Als u een gedeelde locatie op de host waarop de informatie van de Windows PowerShell-sessie opslaan hebt ingesteld, moet u zich bewust zijn dat de voornaamste iedereen alleen geverifieerde gebruikers bevat. Als u de share toegang door iedereen toestaan hebt ingesteld en u een verbinding maken zonder referenties, de niet-geverifieerde anonieme principal wordt gebruikt en ziet u een fout. Dit probleem kunt oplossen, host op de share die u moet de gastaccount inschakelen en geef de Gast account volledige toegang tot de share of geldige referenties samen met de Windows PowerShell-cmdlet opgeven.

U kunt HTTP- of HTTPS-verbinding via externe toegang van Windows PowerShell. Volg de instructies in de volgende cursussen:

- [Verbinding maken op afstand via HTTP](storsimple-remote-connect.md#connect-through-http)
- [Op afstand met behulp van HTTPS verbinding](storsimple-remote-connect.md#connect-through-https)

## <a name="connection-security-considerations"></a>Beveiligingsoverwegingen voor verbinding

U verbinding maken met Windows PowerShell voor StorSimple zijn Houd rekening met het volgende:

- Rechtstreeks verbinding maken met de seriële console apparaat veilig is, maar geen verbinding maken met de seriële console via netwerk-switches is. Wees voorzichtig over de beveiligingsrisico's met het verbinding maken met een serieel apparaat via netwerk-switches.

- Verbinding maken via een HTTP-sessie biedt meer beveiliging dan een verbinding via de seriële console via netwerk mogelijk. Hoewel dit niet de meest veilige methode is, is het aanvaardbaar op vertrouwde netwerken.

- Verbinding maken via een HTTPS-sessie is de veiligste en de aanbevolen optie.


## <a name="administer-your-storsimple-device-using-windows-powershell-for-storsimple"></a>Het gebruik van Windows PowerShell voor StorSimple StorSimple-apparaat beheren
De volgende tabel bevat een overzicht van de algemene beheertaken en complexe workflows die kunnen worden uitgevoerd binnen de Windows PowerShell-interface van uw apparaat StorSimple. Voor meer informatie over elke workflow, klikt u op het desbetreffende item in de tabel.

#### <a name="windows-powershell-for-storsimple-workflows"></a>Windows PowerShell voor workflows StorSimple

|Als u dit wilt doen...|Gebruik deze procedure.|
|---|---|
|Registreer uw apparaat|[Configureren en het gebruik van Windows PowerShell voor StorSimple apparaat registreren](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |
|Webproxy configureren</br>Web proxy-instellingen weergeven|[Webproxy voor uw apparaat StorSimple configureren](storsimple-configure-web-proxy.md)|
|DATA 0 netwerk interface-instellingen op uw apparaat wijzigen|[DATA 0 netwerkinterface voor het apparaat StorSimple wijzigen](storsimple-modify-data-0.md)|
|Een controller stoppen </br> Opnieuw opstarten of afsluiten van een domeincontroller </br> Afsluiten van een apparaat</br>Het apparaat standaardinstellingen opnieuw instellen|[Apparaatcontrollers beheren](storsimple-manage-device-controller.md)|
|Onderhoud modus updates en hotfixes installeren|[Uw apparaat bijwerken](storsimple-update-device.md)|
|Onderhoud modus </br>De onderhoudsmodus afsluiten|[StorSimple apparaat modi](storsimple-device-modes.md)|
|Maak een ondersteuningspakket</br>Decoderen en support-pakket bewerken|[Maken en beheren van een Support-pakket](storsimple-create-manage-support-package.md)|
|Start een ondersteuningssessie</br>|[Start een ondersteuningssessie in Windows PowerShell voor StorSimple](/storsimple-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple)
 

## <a name="get-help-in-windows-powershell-for-storsimple"></a>Help-informatie opvragen in Windows PowerShell voor StorSimple

In Windows PowerShell voor StorSimple is cmdlet Help beschikbaar. Een actuele versie van deze Help-informatie is ook beschikbaar, waarmee u de Help op uw systeem bijwerken.

Help-informatie opvragen in deze interface is vergelijkbaar met die in Windows PowerShell en de meeste van de verwante Help cmdlets zal werken. U vindt Help voor Windows PowerShell on line in de TechNet Library: [uitvoeren van scripts met Windows PowerShell](http://go.microsoft.com/fwlink/?LinkID=108518).

Hier volgt een korte beschrijving van de soorten Help-informatie voor deze interface van Windows PowerShell, met inbegrip van het bijwerken van de Help.

#### <a name="to-get-help-for-a-cmdlet"></a>Help-informatie opvragen voor een cmdlet

- Als u Help voor elke cmdlet of functie, gebruik de volgende opdracht:`Get-Help <cmdlet-name>`

- Als u on line Help voor elke cmdlet, gebruikt u de cmdlet vorige met de `-Online` parameter:`Get-Help <cmdlet-name> -Online`

- Volledige hulp nodig hebt, kunt u de `–Full` parameter en voor voorbeelden van de `–Examples` parameter.

#### <a name="to-update-help"></a>Help-informatie bijwerken

U kunt gemakkelijk de Help-informatie in de Windows PowerShell-interface bijwerken. Voer de volgende stappen uit om de Help op uw systeem bijwerken.

#### <a name="to-update-cmdlet-help"></a>Help cmdlet bijwerken

1. Windows PowerShell start met de optie **Als administrator uitvoeren** .

1. Typ het volgende achter de opdrachtprompt: `Update-Help`

1. De bijgewerkte Help-bestanden geïnstalleerd.

1. Nadat u de Help-bestanden zijn geïnstalleerd, typt u: `Get-Help Get-Command`. Hiermee wordt een lijst van cmdlets waarvoor Help-informatie beschikbaar is weergegeven.


>[AZURE.NOTE] Als u een lijst met de beschikbare cmdlets in een runspace, aanmelden bij de desbetreffende optie en voer de `Get-Command` cmdlet.

## <a name="next-steps"></a>Volgende stappen
Als u problemen met uw apparaat StorSimple optreden bij het uitvoeren van een van de bovenstaande werkstromen, Ga naar [hulpprogramma's voor het oplossen van StorSimple implementaties](storsimple-troubleshoot-deployment.md#tools-for-troubleshooting-storsimple-deployments).

