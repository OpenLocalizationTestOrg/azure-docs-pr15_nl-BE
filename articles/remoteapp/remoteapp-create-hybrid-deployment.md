<properties
    pageTitle="Het maken van een collectie hybride voor Azure RemoteApp | Microsoft Azure"
    description="Informatie over het maken van een implementatie van RemoteApp die is verbonden met het interne netwerk."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo"/>

# <a name="how-to-create-a-hybrid-collection-for-azure-remoteapp"></a>Het maken van een collectie hybride voor Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Er zijn twee soorten Azure RemoteApp-collecties:

- Cloud: bevindt zich volledig in Azure. U kunt alle gegevens opslaan in de cloud (zodat een wolk alleen-lezen collectie) of aan uw verzameling verbinding met een VNET en er gegevens opslaan.   
- Hybride: bevat een virtueel netwerk voor toegang voor gebouwen - moet het gebruik van AD Azure en een Active Directory-omgeving op gebouwen.

Weet u niet dat u nodig hebt? Ontdek [welke collectie hebt u nodig voor Azure RemoteApp](remoteapp-collections.md).

Deze zelfstudie doorloopt u het proces van het maken van een hybride-collectie. Er zijn acht stappen:

1.  Bepalen welke [afbeelding](remoteapp-imageoptions.md) moet worden gebruikt voor uw verzameling. U kunt een aangepaste installatiekopie maken of gebruik een van de Microsoft-afbeeldingen die deel uitmaken van uw abonnement.
2. Instellen van het virtuele netwerk. Bekijk informatie over de [planning van VNET](remoteapp-planvnet.md) en [formaat wijzigen](remoteapp-vnetsizing.md) .
2.  Een verzameling maken.
2.  Uw verzameling toevoegen aan uw lokale domein.
3.  Een sjabloon toevoegen aan de collectie.
4.  Adreslijstsynchronisatie configureren. Azure RemoteApp is vereist dat u integreren met Active Directory door beide 1) configureren Azure Active Directory-synchronisatie met de optie wachtwoord Sync of 2) configureren Azure Active Directory-synchronisatie zonder de optie wachtwoord Sync, maar met behulp van een domein dat voor AD FS is federatieve Azure. Bekijk de [informatie over de configuratie van Active Directory met RemoteApp](remoteapp-ad.md).
5.  RemoteApp-toepassingen publiceren.
6.  Configureer de gebruikerstoegang.

**Voordat u begint**

U moet voordat u de collectie maakt als volgt:

- [Meld u aan](https://azure.microsoft.com/services/remoteapp/) voor Azure RemoteApp.
- Een gebruikersaccount maken in Active Directory als de Azure RemoteApp-serviceaccount gebruiken. De machtigingen voor deze account beperken zodat deze alleen computers aan het domein toevoegen kunt.
- Informatie verzamelen over uw netwerk op ruimten: IP-adres VPN-apparaat gegevens.
- Installeer de module [Azure PowerShell](../powershell-install-configure.md) .
- Informatie verzamelen over de gebruikers die u toegang wilt verlenen aan. U moet de UPN-naam Azure Active Directory (bijvoorbeeld, name@contoso.com) voor elke gebruiker. Controleer of de UPN komt overeen met tussen AD Azure en Active Directory.
- Kies uw Sjabloonafbeelding. Een afbeelding Azure RemoteApp-sjabloon bevat de apps en programma's die u wilt publiceren voor uw gebruikers. Zie [Opties voor Azure RemoteApp-afbeelding](remoteapp-imageoptions.md) voor meer informatie.
- Wilt u de afbeelding met Office 365 ProPlus gebruiken? Bekijk info [hier](remoteapp-officesubscription.md).
- [Configureer Active Directory voor RemoteApp](remoteapp-ad.md).



## <a name="step-1-set-up-your-virtual-network"></a>Stap 1: Instellen van het virtuele netwerk
U kunt een hybride-collectie die een bestaande Azure virtueel netwerk implementeren of kunt u een nieuw virtueel netwerk. Een virtueel netwerk kunt uw gebruikers toegang tot gegevens op uw lokale netwerk via RemoteApp-externe bronnen. Met behulp van een virtueel netwerk van Azure beschikt uw collectie direct netwerk andere Azure services en virtuele machines met dat virtuele netwerk wordt geïmplementeerd.

Zorg ervoor dat u de [planning van de VNET](remoteapp-planvnet.md) en de [grootte van de VNET](remoteapp-vnetsizing.md) informatie bekijken voordat u uw VNET maakt.

### <a name="create-an-azure-vnet-and-join-it-to-your-active-directory-deployment"></a>Een Azure-VNET maken en toevoegen aan de Active Directory-implementatie

Begin met het maken van een [virtueel netwerk](../virtual-network/virtual-networks-create-vnet-arm-pportal.md). Dit wordt gedaan op het tabblad **netwerk** in de portal Azure. U moet het virtuele netwerk verbinding kunnen maken met de implementatie van Active Directory wordt gesynchroniseerd met uw huurder Azure Active Directory.

Zie [een virtueel netwerk met behulp van de portal Azure maken](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) voor meer informatie.

### <a name="make-sure-your-virtual-network-is-ready-for-azure-remoteapp"></a>Zorg ervoor dat het virtuele netwerk klaar is voor Azure RemoteApp
Voordat u uw verzameling maken, zullen we controleren of dat uw nieuwe virtuele netwerk klaar is. U kunt dit als volgt controleren:

1. Maak een virtuele machine met Azure binnen het subnet van het virtuele netwerk dat u zojuist hebt gemaakt voor RemoteApp.
2. Met extern bureaublad verbinding maken met de virtuele machine. (Klik op **verbinding maken**.)
3. Toevoegen aan de Active Directory-implementatie die u wilt gebruiken voor RemoteApp.

Werkte dat? Het virtuele netwerk en subnet zijn gereed voor RemoteApp Azure!

U vindt meer informatie over het maken van Azure virtual machines en hun verbinding met extern bureaublad [hier](https://msdn.microsoft.com/library/azure/jj156003.aspx).

## <a name="step-2-create-an-azure-remoteapp-collection"></a>Stap 2: Maak een Azure RemoteApp-collectie ##



1. Ga naar Azure RemoteApp-pagina in de [portal Azure](http://manage.windowsazure.com).
2. Klik op **Nieuw > maken met VNET**.
3. Voer een naam voor de verzameling.
4. Kies het schema dat u gebruiken wilt-standaard of basic.
5. Kies uw VNET uit de vervolgkeuzelijst lijst en klik vervolgens op uw subnet.
6. Wilt u lid worden van het domein.
5. Klik op **maken van RemoteApp-collectie**.

Nadat u uw Azure RemoteApp-collectie hebt gemaakt, dubbelklikt u op de naam van de collectie. Dat brengt de pagina **Quick Start** - dit is waar u klaar bent met het configureren van de collectie.

Is er iets mis gaan? Bekijk de [informatie over probleemoplossing hybride-collectie](remoteapp-hybridtrouble.md).

## <a name="step-3-link-your-collection-to-the-local-domain"></a>Stap 3: Uw collectie naar het lokale domein koppelen ##


1. Klik op de pagina **Quick Start** **join een lokaal domein**.
2. De Azure RemoteApp-service-account toevoegen aan uw lokale Active Directory-domein. U moet de naam van het domein, organisatie-eenheid, service account gebruikersnaam en wachtwoord.

    Dit is de informatie die u verzameld als u de stappen in [Active Directory configureren voor RemoteApp Azure](remoteapp-ad.md).


## <a name="step-4-link-to-an-azure-remoteapp-image"></a>Stap 4: Koppeling naar een afbeelding Azure RemoteApp ##

Een afbeelding van de sjabloon Azure RemoteApp-bevat programma's die u wilt delen met gebruikers. Een nieuwe [Sjabloonafbeelding](remoteapp-imageoptions.md) maakt of een koppeling naar een bestaande installatiekopie (een al geïmporteerd of geüpload naar Azure RemoteApp). U kunt ook koppelen aan een van de RemoteApp-Azure [sjabloon afbeeldingen](remoteapp-images.md) met Office 365 of Office 2013 (voor het testgebruik) programma's.

Als u de nieuwe afbeelding uploaden wilt, moet u de naam en de locatie van de afbeelding kiezen. Op de volgende pagina van de wizard u ziet een reeks PowerShell - cmdlets voor kopiëren en deze cmdlets uitvoeren vanaf een verhoogde Windows PowerShell-prompt voor het uploaden van de opgegeven afbeelding.

Als u een bestaande Sjabloonafbeelding koppelen wilt, gewoon de naam, locatie en bijbehorende Azure abonnement opgeven.



## <a name="step-5-configure-active-directory-directory-synchronization"></a>Stap 5: Synchronisatie van Active Directory directory configureren ##

Azure RemoteApp is vereist dat u integreren met Active Directory door beide 1) configureren Azure Active Directory-synchronisatie met de optie wachtwoord Sync of 2) configureren Azure Active Directory-synchronisatie zonder de optie wachtwoord Sync, maar met behulp van een domein dat voor AD FS is federatieve Azure.

[Verbinding maken met AD](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) uitchecken - dit artikel helpt u bij het instellen van de directory-integratie in 4 stappen.

Zie [overzicht van Directory-synchronisatie](http://msdn.microsoft.com//library/azure/hh967642.aspx) voor de planning, informatie en gedetailleerde stappen.

## <a name="step-6-publish-apps"></a>Stap 6: Apps publiceren ##

Een Azure RemoteApp-app is de app of programma dat u aan uw gebruikers levert. Het bevindt zich in de Sjabloonafbeelding die u hebt geüpload voor de collectie. Wanneer een gebruiker toegang krijgt een app tot, blijkt uit te voeren in hun lokale omgeving, maar het echt wordt uitgevoerd in Azure.

Voordat gebruikers toegang toepassingen tot, moet u deze publiceren – Hiermee uw gebruikers toegang krijgen tot de apps via de extern-bureaubladclient.

U kunt meerdere apps publiceren aan de collectie. Klik op **publiceren** als u wilt toevoegen een app van de publicatiepagina. U kunt ofwel publiceren in het menu **Start** van de Sjabloonafbeelding of het pad opgeeft voor de Sjabloonafbeelding voor de app. Als u in het menu **Start** toevoegen, kiest u het programma toe te voegen. Als u het pad naar de app, Geef een naam voor de toepassing en het pad naar de op de Sjabloonafbeelding is geïnstalleerd.

## <a name="step-7-configure-user-access"></a>Stap 7: Configureer de gebruikerstoegang ##

Nu u uw verzameling hebt gemaakt, moet u de gebruikers die u wilt kunnen gebruiken uw externe bronnen toe te voegen. De gebruikers toegang tot de moet bestaan in de Active Directory-huurder die is gekoppeld aan het abonnement op te geven die u gebruikt voor het maken van deze verzameling Azure RemoteApp.

1.  Klik op de pagina Quick Start **toegang configureren**.
2.  Voer de die u toegang wilt verlenen voor werk-rekening (van Active Directory) of Microsoft-account.

    **Opmerkingen:**

    Zorg dat u de “user@domain.com” indeling.

    Als u van Office 365 ProPlus in de collectie gebruikmaakt, moet u de Active Directory-identiteit voor uw gebruikers. Op deze manier valideren Licentieverlening.


3.  Zodra de gebruikers worden gevalideerd, klikt u op **Opslaan**.


## <a name="next-steps"></a>Volgende stappen ##
Dat is het - je hebt gemaakt en uw verzameling Azure RemoteApp-hybride geïmplementeerd. De volgende stap is om de gebruikers te downloaden en installeren van de extern-bureaubladclient. U vindt de URL voor de client op de pagina Quick Start van Azure RemoteApp. Vervolgens hebben gebruikers worden aangemeld bij de client en de toegang tot de toepassingen die u hebt gepubliceerd.



### <a name="help-us-help-you"></a>Help ons u te helpen
Wist u dat naast de beoordeling van dit artikel en het toevoegen van opmerkingen naar beneden onder, kunt u wijzigingen aanbrengen in het artikel zelf? Iets ontbreekt? Iets mis? Schrijf iets dat is gewoon verwarrend? Omhoog schuiven en klik op **bewerken op GitHub** wijzigingen aan te brengen - die voor revisie naar ons komen, en vervolgens, wanneer we deze afmelden, ziet u de wijzigingen en verbeteringen hier.
