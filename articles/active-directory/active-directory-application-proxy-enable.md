<properties
    pageTitle="Toepassingsproxy Azure AD inschakelen | Microsoft Azure"
    description="Toepassingsproxy inschakelen in de klassieke Azure portal en installeren van de verbindingslijnen voor reverse proxy."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="kgremban"/>

# <a name="enable-application-proxy-in-the-azure-portal"></a>Inschakelen van de toepassingsproxy in Azure portal

Dit artikel begeleidt u door de stappen voor het inschakelen van Microsoft Azure AD toepassingsproxy voor de cloud-map in Azure AD.

Als u niet bekend met de toepassingsproxy wat bent kunt u doen, meer informatie over [veilige externe toegang tot het op-premises toepassingen](active-directory-application-proxy-get-started.md).

## <a name="application-proxy-prerequisites"></a>Toepassingsvereisten Proxy
Voordat u kunt inschakelen en de Proxy-services gebruikt, moet u hebben:

- Een [abonnement op Microsoft Azure AD basic- of geavanceerde](active-directory-editions.md) en een Azure AD map waarvoor u een globale beheerder bent.
- Een server met Windows Server 2012 R2 en Windows 8.1 of hoger, waarop u de Proxy-Connector-toepassing kunt installeren. De server stuurt verzoeken naar de toepassingsproxy services in de cloud en moet een HTTP- of HTTPS-verbinding met de toepassingen die u wilt publiceren.

    - Voor eenmalige aanmelding voor uw gepubliceerde toepassingen, moet deze machine worden domein behoren in hetzelfde domein als de toepassingen die u wilt publiceren AD.

- Als er een firewall in het pad, zorg ervoor dat deze geopend is zodat de verbindingslijn kunt aanvragen voor HTTPS (TCP) naar de toepassingsproxy. De Connector gebruikt deze poorten met subdomeinen die deel van de msappproxy.net op hoog niveau domeinen en servicebus.windows.net uitmaken. Zorg ervoor dat de volgende poorten op **Uitgaand** verkeer te openen:

  	| Nummer van de poort | Beschrijving |
  	| --- | --- |
  	| 80 | Uitgaande HTTP-verkeer voor beveiligingsvalidatie inschakelen. |
  	| 443 | Gebruikersverificatie ten opzichte van Azure AD (alleen vereist voor het inschrijvingsproces Connector) inschakelen |
  	| 10100 – 10120 | LOB-HTTP-antwoorden dat is verzonden naar de proxy inschakelen |
  	| 9352, 5671 | Inschakelen van communicatie tussen de verbindingslijn ten opzichte van de Azure service voor binnenkomende aanvragen. |
  	| 9350 | Optioneel, betere prestaties voor binnenkomende aanvragen inschakelen |
  	| 8080 | De bootstrap volgorde Connector en de verbindingslijn automatisch bijwerken inschakelen |
  	| 9090 | Connector-registratie (alleen vereist voor het inschrijvingsproces Connector) inschakelen |
  	| 9091 | Connector vertrouwen certificaat automatisch vernieuwen inschakelen |

    Als uw firewall verkeer op basis van gebruikers van oorsprong afdwingt, opent u deze poorten voor verkeer afkomstig van Windows-services die worden uitgevoerd als een netwerkservice. Zorg er ook poort 8080 inschakelen voor NT Authority\System.

- Als uw organisatie gebruikmaakt van proxyservers verbinding met het internet, bekijk dan bij het [werken met bestaande op ruimten proxyservers](https://blogs.technet.microsoft.com/applicationproxyblog/2016/03/07/working-with-existing-on-prem-proxy-servers-configuration-considerations-for-your-connectors/) blogbericht voor meer informatie over hoe u deze configureert.

## <a name="step-1-enable-application-proxy-in-azure-ad"></a>Stap 1: De toepassingsproxy in Azure AD inschakelen
1. Log in als beheerder in [Azure klassieke portal](https://manage.windowsazure.com/).
2. Ga naar Active Directory en selecteer de map waarin u wilt inschakelen van Proxy-toepassing.

    ![Active Directory - pictogram](./media/active-directory-application-proxy-enable/ad_icon.png)

3. **Configureren** van de map pagina selecteren en Ga naar **De Proxy**.
4. Toggle **Proxy-toepassingsservices voor deze map inschakelen** **ingeschakeld**.

    ![Toepassingsproxy inschakelen](./media/active-directory-application-proxy-enable/app_proxy_enable.png)

5. Selecteer **nu downloaden**. Hiermee gaat u naar de **Azure AD Application Proxy Connector downloaden**. Lees en accepteer de licentievoorwaarden en klik op **downloaden** om op te slaan van het Windows Installer-bestand (.exe) voor de verbindingslijn.

## <a name="step-2-install-and-register-the-connector"></a>Stap 2: Installeren en registreren van de Connector
1. **AADApplicationProxyConnectorInstaller.exe** worden uitgevoerd op de server die u voorbereid volgens de vereisten.
2. Volg de instructies in de wizard te installeren.
3. Tijdens de installatie, u zal worden gevraagd voor het registreren van de connector met de toepassingsproxy van de huurder Azure AD.

  - De globale beheerder AD Azure referenties opgeven. De globale beheerder huurder afwijken van uw referenties voor Microsoft Azure.
  - Controleer of admin die de connector registreert is in dezelfde map waarin u de toepassingsproxy-service ingeschakeld. Bijvoorbeeld als de huurder domein contoso.com, admin moet zijn admin@contoso.com of een andere alias in dat domein.
  - Als **Verbeterde beveiliging van Internet Explorer** is ingesteld op **aan** op de server waar u de verbindingslijn wilt installeren, kunt u het registratiescherm mogelijk geblokkeerd. Volg de instructies in het foutbericht om toegang te verlenen. Zorg ervoor dat Internet Explorer Verbeterde beveiliging uitgeschakeld is.
  - Als connector registratie niet is gelukt, ziet u de [Toepassingsproxy oplossen](active-directory-application-proxy-troubleshoot.md).  

4. Wanneer de installatie is voltooid, worden twee nieuwe services aan de server toegevoegd:

    - **Microsoft AAD Application Proxy Connector** maakt connectiviteit
    - **Microsoft AAD Application Proxy Connector Updater** is een automatische update-service periodiek controleert of er nieuwe versies van de connector en de verbindingslijn wordt bijgewerkt als dit nodig is.

    ![App Connector-Proxy services - screenshot](./media/active-directory-application-proxy-enable/app_proxy_services.png)

5. Klik op **Voltooien** in het installatievenster.

Voor hoge beschikbaarheid, moet u ten minste twee aansluitingen te implementeren. Herhaal stap 2 en 3, hierboven voor de implementatie van meer verbindingslijnen. Elke verbindingslijn moet apart worden geregistreerd.

Als u wilt dat de verbindingslijn verwijdert, verwijdert u de Connector en de Updater service. Start de computer om de service volledig te verwijderen.


## <a name="next-steps"></a>Volgende stappen

U bent nu gereed om [toepassingen publiceren met de Proxy](active-directory-application-proxy-publish.md).

Als u toepassingen op afzonderlijke netwerken of op andere locaties hebt, kunt u groepen connector de andere verbindingslijnen indelen in logische eenheden. Meer informatie over het [werken met de Proxy-aansluitingen](active-directory-application-proxy-connectors.md).
