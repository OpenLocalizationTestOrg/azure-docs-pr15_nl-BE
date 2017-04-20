<properties 
   pageTitle="Webproxy voor een StorSimple apparaat instellen | Microsoft Azure"
   description="Informatie over het gebruik van Windows PowerShell voor StorSimple web proxy-instellingen voor uw apparaat StorSimple configureren."
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

# <a name="configure-web-proxy-for-your-storsimple-device"></a>Webproxy voor uw apparaat StorSimple configureren

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt beschreven hoe u met behulp van Windows PowerShell voor StorSimple configureren en web proxy-instellingen voor uw apparaat StorSimple weergeven. De web proxy-instellingen worden gebruikt door het apparaat StorSimple bij de communicatie met de cloud. Een web proxy-server wordt gebruikt voor het toevoegen van een extra beveiligingslaag, filterinhoud, cache bandbreedtevereisten gemak of zelfs hulp met analytics.

Webproxy is een optionele configuratie voor het apparaat StorSimple. U kunt webproxy alleen via Windows PowerShell voor StorSimple. De configuratie is een proces als volgt:

1. U eerst de web proxy-instellingen via de wizard setup of Windows PowerShell voor StorSimple cmdlets.

2. U schakelt de geconfigureerde web proxy-instellingen via de Windows PowerShell voor StorSimple cmdlets.

Nadat de web proxy-configuratie voltooid is, kunt u de geconfigureerde web proxy-instellingen in de service Microsoft Azure StorSimple Manager en de Windows PowerShell voor StorSimple weergeven. 

Na het lezen van deze zelfstudie, is het mogelijk om:

- Webproxy configureren met behulp van de wizard setup en cmdlets
- Webproxy inschakelen met behulp van cmdlets
- Web proxy-instellingen weergeven in de klassieke Azure portal
- Oplossen van fouten tijdens het configureren van web proxy


## <a name="configure-web-proxy-via-windows-powershell-for-storsimple"></a>Webproxy via Windows PowerShell voor StorSimple configureren

U kunt een van de volgende web proxy-instellingen configureren:

- De wizard Setup om u te begeleiden bij de configuratiestappen.

- Cmdlets in Windows PowerShell voor StorSimple.

Elk van deze methoden wordt in de volgende secties besproken.

## <a name="configure-web-proxy-via-the-setup-wizard"></a>Webproxy via de wizard setup configureren

Om u te begeleiden bij de stappen voor de web proxy-configuratie kunt u de wizard setup. Voer de volgende stappen uit om de webproxy configureren op uw apparaat.

#### <a name="to-configure-web-proxy-via-the-setup-wizard"></a>Webproxy via de wizard setup configureren

1. Kies optie 1, **Meld u aan met volledige toegang tot de** in de seriële console-menu en het **apparaat administrator-wachtwoord**opgeven. Typ de volgende opdracht een sessie van de wizard setup te starten:

    `Invoke-HcsSetupWizard`

2. Als dit de eerste keer dat u de wizard setup hebt gebruikt voor de registratie, moet u de vereiste netwerkinstellingen configureren tot de web proxy-configuratie. Als uw apparaat al is geregistreerd, kunt u alle instellingen geconfigureerd netwerk accepteren totdat de web proxy-configuratie. In de wizard setup wanneer u wordt gevraagd naar de web proxy-instellingen configureren, typt u **Ja**.

3. Geef het IP-adres of de FQDN-naam (Fully Qualified Domain Name) van de web proxy-server en het TCP-poortnummer dat u uw apparaat om te gebruiken dat wilt bij de communicatie met de cloud voor de **Web Proxy-URL**. Gebruik de volgende notatie:

    `http://<IP address or FQDN of the web proxy server>:<TCP port number>`

    Standaard wordt TCP-poortnummer 8080 opgegeven.

4. Kies het verificatietype als **NTLM**, **Basic**, of **geen**. Standaard is de minst veilige verificatie voor de configuratie van de proxyserver. NT LAN Manager (NTLM) is een zeer veilige en complexe verificatieprotocol dat gebruikmaakt van een drievoudige berichtensysteem (soms vier als extra integriteit vereist is) om een gebruiker te verifiëren. De standaardverificatie wordt NTLM. Zie [standaardschijven](http://hc.apache.org/httpclient-3.x/authentication.html) en [NTLM-verificatie](http://hc.apache.org/httpclient-3.x/authentication.html)voor meer informatie. 

    > [AZURE.IMPORTANT] **In de service Manager StorSimple grafieken controle apparaat werken niet als Basic of NTLM-verificatie is ingeschakeld in de configuratie van de proxyserver voor het apparaat. De monitoring grafieken wilt werken, moet u ervoor zorgen dat de verificatie is ingesteld op geen.**

5. Als u van verificatie gebruikmaakt, leveren een **Web Proxy-gebruikersnaam** en een **Wachtwoord voor Web Proxy**. U moet ook het wachtwoord te bevestigen.

    ![Webproxy configureren op StorSimple Device1](./media/storsimple-configure-web-proxy/IC751830.png)

Als u het apparaat voor het eerst registreert, kunt u doorgaan met de inschrijving. Als uw apparaat al is geregistreerd, wordt de wizard afgesloten. De geconfigureerde instellingen worden opgeslagen.

Webproxy wordt nu ook ingeschakeld. U kunt het [inschakelen van webproxy](#enable-web-proxy) overslaan en ga direct naar de [weergave web proxy-instellingen in de klassieke Azure portal](#view-web-proxy-settings-in-the-azure-classic-portal).


## <a name="configure-web-proxy-via-windows-powershell-for-storsimple-cmdlets"></a>Webproxy via Windows PowerShell voor cmdlets StorSimple configureren

Er is een alternatieve manier om web proxy-instellingen configureren via de Windows PowerShell voor StorSimple cmdlets. Voer de onderstaande stappen om webproxy te configureren.

#### <a name="to-configure-web-proxy-via-cmdlets"></a>Webproxy via cmdlets configureren

1. Kies optie 1, **zich aanmelden met volledige toegang**in het menu van de seriële console. Geef desgevraagd het **beheerderswachtwoord apparaat**. Het standaardwachtwoord is `Password1`.

2. Typ het volgende achter de opdrachtprompt:

    `Set-HcsWebProxy -Authentication NTLM -ConnectionURI "<http://<IP address or FQDN of web proxy server>:<TCP port number>" -Username "<Username for web proxy server>"`

    Bieden en Bevestig het wachtwoord wanneer daarom wordt gevraagd, zoals hieronder wordt weergegeven.

    ![Webproxy configureren op StorSimple Device3](./media/storsimple-configure-web-proxy/IC751831.png)

De webproxy is nu geconfigureerd en moet worden ingeschakeld.

## <a name="enable-web-proxy"></a>Webproxy inschakelen

Webproxy is standaard uitgeschakeld. Nadat u de web proxy-instellingen op uw apparaat StorSimple configureert, moet u de web proxy-instellingen inschakelen met de Windows PowerShell voor StorSimple.

> [AZURE.NOTE] **Deze stap niet worden vereist als u de wizard setup gebruikt voor webproxy configureren. Webproxy wordt automatisch na een sessie van de wizard setup standaard ingeschakeld.**

De volgende stappen uitvoeren in Windows PowerShell voor StorSimple webproxy op uw apparaat inschakelen:

#### <a name="to-enable-web-proxy"></a>Webproxy inschakelen

1. Kies optie 1, **zich aanmelden met volledige toegang**in het menu van de seriële console. Geef desgevraagd het **beheerderswachtwoord apparaat**. Het standaardwachtwoord is `Password1`.

2. Typ het volgende achter de opdrachtprompt:

    `Enable-HcsWebProxy`

    U hebt nu de web proxy-configuratie op uw apparaat StorSimple ingeschakeld.

    ![Webproxy configureren op StorSimple Device4](./media/storsimple-configure-web-proxy/IC751832.png)

## <a name="view-web-proxy-settings-in-the-azure-classic-portal"></a>Web proxy-instellingen weergeven in de klassieke Azure portal

De web proxy-instellingen worden geconfigureerd via de Windows PowerShell-interface en kunnen niet worden gewijzigd in de klassieke portal. U kunt deze instellingen echter bekijken in de klassieke portal. Voer de volgende stappen uit om webproxy.

#### <a name="to-view-web-proxy-settings"></a>Web proxy-instellingen te bekijken
1. Ga naar **StorSimple Manager service > apparaten**. Selecteer en klik op een apparaat te **configureren**.
1. Schuif naar beneden op de pagina **configureren** om **Web proxy-instellingen** sectie. U kunt de geconfigureerde web proxy-instellingen weergeven op uw apparaat StorSimple zoals hieronder wordt weergegeven.

    ![Webproxy weergeven in de Portal beheren](./media/storsimple-configure-web-proxy/ViewWebProxyPortal_M.png)
 
## <a name="errors-during-web-proxy-configuration"></a>Fouten tijdens de web proxy-configuratie

Als de web proxy-instellingen onjuist zijn geconfigureerd, worden foutberichten weergegeven voor de gebruiker in Windows PowerShell voor StorSimple. De volgende tabel worden enkele van deze foutberichten wordt weergegeven, hun mogelijke oorzaken en aanbevolen acties.

|Serienummer of Lotnr.|HRESULT-fout Code|Mogelijke oorzaak|Aanbevolen actie|
|:---|:---|:---|:---|
|1.|0x80070001|Opdracht van de passieve-controller wordt uitgevoerd en niet in staat om te communiceren met de domeincontroller actief is.|De opdracht uitvoeren op de actieve domeincontroller. Om de opdracht van de passieve domeincontroller uitvoert, moet u de connectiviteit van passieve naar actieve domeincontroller herstellen. U moet Microsoft Support uitoefenen als deze verbinding verbroken wordt.|
|2.|0x800710dd - de bewerkings-id is ongeldig|Proxy-instellingen worden niet ondersteund op StorSimple virtueel apparaat.|Proxy-instellingen worden niet ondersteund op StorSimple virtueel apparaat. Deze optie kunnen alleen worden geconfigureerd op een fysiek apparaat StorSimple.|
|3.|0x80070057 - ongeldige parameter|Een van de parameters die voor de proxy-instellingen is niet geldig.|De URI is niet beschikbaar in de juiste indeling. Gebruik de volgende notatie:`http://<IP address or FQDN of the web proxy server>:<TCP port number>`|
|4.|0x800706ba - RPC-server niet beschikbaar|De oorzaak is een van de volgende opties:</br></br>Het cluster is niet omhoog.</br></br>DataPath service is niet actief.</br></br>De opdracht van passieve domeincontroller wordt uitgevoerd en niet in staat om te communiceren met de domeincontroller actief is.|Neem namens Microsoft Support om ervoor te zorgen dat het cluster actief is en gegevenspad-service wordt uitgevoerd.</br></br>De opdracht uitvoeren vanaf de actieve domeincontroller. Als u wilt dat de opdracht uitvoeren vanaf de passieve controller, moet u om ervoor te zorgen dat de passieve controller kan communiceren met de actieve domeincontroller. U moet Microsoft Support uitoefenen als deze verbinding verbroken wordt.|
|5.|0x800706be - RPC-aanroep mislukt|Cluster is niet beschikbaar.|Neem Microsoft Support om te controleren of het cluster van uitoefenen.|
|6.|0x8007138f - clusterbron is niet gevonden|Service-clusterbron platform is niet gevonden. Dit kan gebeuren wanneer de installatie niet goed is.|Mogelijk moet u een fabriek op het apparaat opnieuw uitvoeren. Wellicht moet u een platform-bron maken. Neem contact op met Microsoft Support voor volgende stappen.|
|7.|0x8007138c - clusterbron niet online|Niet zijn-platform of datapath clusterbronnen online.|Neem contact op met Microsoft Support om ervoor te zorgen dat de bron van het gegevenspad en platform service on line zijn.|

> [AZURE.NOTE] 
> 
> -  De bovenstaande lijst met foutberichten is niet limitatief. 
> - Fouten met betrekking tot de web proxy-instellingen worden niet weergegeven in de Azure klassieke portal in uw StorSimple Manager-service. Als er een probleem met de webproxy nadat de configuratie is voltooid, de status van het apparaat **offline** wordt gewijzigd in de klassieke portal. |

## <a name="next-steps"></a>Volgende stappen

- Als er problemen optreden tijdens het implementeren van het apparaat of de web proxy-instellingen configureren, raadpleegt u [problemen met uw apparaat StorSimple implementatie](storsimple-troubleshoot-deployment.md).

- Als u wilt weten hoe u de StorSimple Manager-service wilt gebruiken, gaat u naar [de StorSimple Manager-service voor het beheren van uw apparaat StorSimple gebruiken](storsimple-manager-service-administration.md).
