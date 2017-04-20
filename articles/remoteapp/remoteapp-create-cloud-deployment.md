<properties 
    pageTitle="Het maken van een wolk Azure RemoteApp-collectie | Microsoft Azure" 
    description="Informatie over het maken van een implementatie van Azure RemoteApp dat gegevens worden opgeslagen in de cloud Azure." 
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

# <a name="how-to-create-a-cloud-collection-of-azure-remoteapp"></a>Het maken van een wolk Azure RemoteApp-collectie

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Er zijn twee soorten [Azure RemoteApp collecties](remoteapp-collections.md): 

- Cloud: bevindt zich volledig in Azure. U kunt alle gegevens opslaan in de cloud (zodat een wolk alleen-lezen collectie) of aan uw verzameling verbinding met een VNET en er gegevens opslaan.   
- Hybride: bevat een virtueel netwerk voor toegang voor gebouwen - moet het gebruik van AD Azure en een Active Directory-omgeving op gebouwen.

Deze zelfstudie doorloopt u het proces van het maken van een cloud-collectie. Er zijn vier stappen: 

1.  Maak een Azure RemoteApp-collectie.
2.  Desgewenst adreslijstsynchronisatie configureren. Als u van Azure AD gebruikmaakt + Active Directory moeten gebruikers, contactpersonen en wachtwoorden van uw Active Directory op de gebouwen aan de huurder Azure AD synchroniseren.
5.  Apps publiceren.
6.  Configureer de gebruikerstoegang.


**Voordat u begint**

U moet voordat u de collectie maakt als volgt:

- [Meld u aan](https://azure.microsoft.com/services/remoteapp/) voor Azure RemoteApp. 
- Informatie verzamelen over de gebruikers die u toegang wilt verlenen aan. Dit kan Microsoft gegevens of gegevens over de account Active Directory werk voor gebruikers zijn.
- Deze procedure wordt ervan uitgegaan dat u beide wilt gebruiken een van de sjabloon-afbeeldingen die deel uitmaken van uw abonnement bent of dat u al hebt geüpload de Sjabloonafbeelding die u wilt gebruiken. Als u nodig hebt voor het uploaden van de afbeelding van een andere sjabloon, kunt u dat doen vanaf de pagina sjabloon-afbeeldingen. Klik op **de Sjabloonafbeelding van een uploaden** en volg de stappen in de wizard. 
- Wilt u de afbeelding met Office 365 ProPlus gebruiken? Bekijk info [hier](remoteapp-officesubscription.md).
- Aangepaste apps bieden of LOB-programma's wilt? Een nieuwe [afbeelding](remoteapp-imageoptions.md) maken en gebruiken in uw verzameling wolk.
- Uitzoeken of u moet verbinding maken met een VNET. Als u verbinding maakt met een VNET, zorg ervoor dat het voldoet aan de [richtlijnen van de grootte](remoteapp-vnetsizing.md) en dat deze [verbinding kunt maken met RemoteApp](remoteapp-vnet.md). Bekijk de [planning artikel VNET ](remoteapp-planvnet.md)voor meer informatie.
- Als u een VNET gebruikt, moet u beslissen of u wilt toevoegen aan uw lokale Active Directory-domein.

## <a name="step-1-create-a-cloud-collection---with-or-without-a-vnet"></a>Stap 1: Maak een wolk collectie - met of zonder een VNET##


Gebruik de volgende stappen voor het **maken van een wolk alleen-lezen collectie**:

1. Ga naar de RemoteApp-pagina in de portal beheren.
2. Klik op **Nieuw > Snelle invoer**.
3. Voer een naam voor de verzameling en selecteer uw regio.
4. Kies het schema dat u gebruiken wilt-standaard of basic.
5. Kies de sjabloon die u wilt gebruiken voor deze collectie. 

    **Tip:** Uw abonnement voor RemoteApp wordt geleverd met de [sjabloon-afbeeldingen](remoteapp-images.md) met Office 365 of Office 2013 (voor het testgebruik) programma's, sommige gepubliceerd (zoals Word) en anderen publiceren. U kunt ook een nieuwe [afbeelding](remoteapp-imageoptions.md) maken en gebruiken in uw verzameling wolk.


1. Klik op **maken van RemoteApp-collectie**.
    
    **Belangrijk:** Het kan inrichten van uw verzameling tot 30 minuten duren.

Nadat u uw Azure RemoteApp-collectie hebt gemaakt, dubbelklikt u op de naam van de collectie. Dat brengt de pagina **Quick Start** - dit is waar u klaar bent met het configureren van de collectie.

Gebruik de volgende stappen voor het maken van een **wolk + VNET-collectie**:

1. Ga naar de Azure RemoteApp-pagina in de portal beheren.
2. Klik op **nieuwe** > **met VNET maken**.
3. Voer een naam voor de verzameling.
4. Kies het schema dat u gebruiken wilt-standaard of basic.
5. Kies de VNET die u al hebt gemaakt. Weet niet hoe dat moet doen? Op dit moment zijn de stappen in het onderwerp [hybride](remoteapp-create-hybrid-deployment.md) .
6. Beslissen of u wilt uw verzameling toevoegen aan uw domein. Zo ja, moet u AD Azure integreren met AD verbinden en uw Active Directory-omgeving. Dat valt onder in **stap 2**.
6. Klik op **maken van RemoteApp-collectie**.


## <a name="step-2-configure-active-directory-directory-synchronization-optional"></a>Stap 2: Configureer Active Directory directory synchronisatie (optioneel) ##

Als u Active Directory gebruikt, moet de Azure RemoteApp directory synchronisatie tussen Azure Active Directory en Active Directory in de lokalen voor het synchroniseren van gebruikers, contactpersonen en wachtwoorden aan uw huurder Azure Active Directory. Zie [Active Directory configureren voor RemoteApp Azure](remoteapp-ad.md) voor uitgebreide informatie. U kunt ook gaan rechtstreeks naar [AD verbinding maken](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) voor meer informatie.

## <a name="step-3-publish-apps"></a>Stap 3: Toepassingen publiceren ##

Een Azure RemoteApp-app is de app of programma dat u aan uw gebruikers levert. Het bevindt zich in de Sjabloonafbeelding die u hebt geüpload voor de collectie. Wanneer een gebruiker toegang krijgt een app tot, de app wordt weergegeven in de lokale omgeving uit te voeren, maar het echt wordt uitgevoerd in een virtuele machine in Azure. 

Voordat gebruikers toegang toepassingen tot, moet u deze publiceren: apps kunt publiceren uw gebruikers toegang hebben tot de apps via de extern-bureaubladclient.
 
U kunt meerdere apps aan uw verzameling Azure RemoteApp publiceren. De publicatiepagina, klik op **publiceren** als een programma wilt toevoegen. U kunt ofwel publiceren in het menu **Start** van de Sjabloonafbeelding of het pad opgeeft voor de Sjabloonafbeelding voor de app. Als u toevoegen in het menu **Start wilt** , kies de app te publiceren. Als u het pad naar de app, Geef een naam voor de toepassing en het pad naar de op de Sjabloonafbeelding is geïnstalleerd.

## <a name="step-4-configure-user-access"></a>Stap 4: Configureer de gebruikerstoegang ##

Nu u uw verzameling hebt gemaakt, moet u de gebruikers die u wilt kunnen gebruiken uw externe bronnen toe te voegen. Als u Active Directory gebruikt, de gebruikers toegang tot de moet bestaan in de Active Directory-huurder die is gekoppeld aan het abonnement te geven u gebruikt voor het maken van deze collectie.

1.  Klik op de pagina Quick Start **toegang configureren**. 
2.  Voer de die u toegang wilt verlenen voor werk-rekening (van Active Directory) of Microsoft-account.

    **Opmerkingen:** 

    Zorg dat u de “user@domain.com” indeling.

    Als u van Office 365 ProPlus in de collectie gebruikmaakt, moet u de Active Directory-identiteit voor uw gebruikers. Op deze manier valideren Licentieverlening. 

3.  Nadat de gebruikers worden gevalideerd, klikt u op **Opslaan**.


## <a name="next-steps"></a>Volgende stappen ##

Dat is het - je hebt gemaakt en uw verzameling RemoteApp-Azure cloud geïmplementeerd. De volgende stap is om de gebruikers te downloaden en installeren van de extern-bureaubladclient. U vindt de URL voor de client op de pagina Quick Start van Azure RemoteApp. Vervolgens hebben gebruikers worden aangemeld bij de client en de toegang tot de toepassingen die u hebt gepubliceerd.

### <a name="help-us-help-you"></a>Help ons u te helpen 
Wist u dat naast de beoordeling van dit artikel en het toevoegen van opmerkingen naar beneden onder, kunt u wijzigingen aanbrengen in het artikel zelf? Iets ontbreekt? Iets mis? Schrijf iets dat is gewoon verwarrend? Omhoog schuiven en klik op **bewerken op GitHub** wijzigingen aan te brengen - die voor revisie naar ons komen, en vervolgens, wanneer we deze afmelden, ziet u de wijzigingen en verbeteringen hier.