<properties 
    pageTitle="Hoe leid u USB-apparaten in Azure RemoteApp? | Microsoft Azure" 
    description="Informatie over het gebruik van Mapomleiding voor USB-apparaten in Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-do-you-redirect-usb-devices-in-azure-remoteapp"></a>Hoe leid u USB-apparaten in Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Apparaatomleiding kan gebruikers de USB-apparaten aangesloten op hun computer of tablet met de apps in Azure RemoteApp gebruiken. Bijvoorbeeld, als u Skype via RemoteApp Azure gedeeld, uw gebruikers moeten hun apparaat camera's gebruiken.

Voordat u verder gaat, moet dat u de omleidingsgegevens voor USB-in het [werken met Mapomleiding in Azure RemoteApp-](remoteapp-redirection.md)lezen. Echter de aanbevolen nusbdevicestoredirect:s: * werkt niet voor USB-webcamera's en werkt mogelijk niet voor sommige USB-printers of USB-multifunctionele apparaten. In het ontwerp en om veiligheidsredenen heeft de beheerder Azure RemoteApp-omleiding inschakelen door de apparaatklasse-GUID of apparaatinstantie-ID voordat gebruikers deze apparaten kunnen gebruiken.

Hoewel dit artikel gesprekken over omleiding van web camera voert, kunt u een soortgelijke benadering omleiden USB-printers en andere USB-multifunctionele apparaten die niet worden omgeleid door de **nusbdevicestoredirect:s:*** opdracht.

## <a name="redirection-options-for-usb-devices"></a>Opties voor Mapomleiding voor USB-apparaten
Azure RemoteApp wordt zeer soortgelijke mechanismen voor USB-apparaten omleiden als die beschikbaar voor extern bureaublad-Services. De onderliggende technologie kunt u de van de juiste omleidingsmethode kiezen voor een bepaald apparaat, om het beste van beide op een hoog niveau en een RemoteFX USB-apparaat met behulp van omleiding van de **usbdevicestoredirect:s:** opdracht. Er zijn vier elementen voor deze opdracht:

| Verwerkingsvolgorde | Parameter           | Beschrijving                                                                                                                |
|------------------|---------------------|----------------------------------------------------------------------------------------------------------------------------|
| 1                | *                   | Hiermee selecteert u alle apparaten die niet worden opgenomen door de omleiding op hoog niveau. Opmerking: door ontwerp, * werkt niet voor USB-webcamera's.  |
|                  | {Apparaatklasse-GUID} | Hiermee selecteert u alle apparaten die overeenkomen met het opgegeven stuurprogramma-installatieklasse.                                                           |
|                  | USB\InstanceID      | Hiermee selecteert u een USB-apparaat dat is opgegeven voor de opgegeven exemplaar-ID.                                                                  |
| 2                | -USB\Instance ID    | Hiermee verwijdert u de instellingen voor apparaatomleiding voor het opgegeven apparaat.                                                                 |

## <a name="redirecting-a-usb-device-by-using-the-device-class-guid"></a>Omleiden van een USB-apparaat met behulp van de apparaatklasse-GUID
Er zijn twee manieren om te zoeken naar de apparaatklasse-GUID die kan worden gebruikt voor omleiding. 

De eerste optie is de [System-Defined apparaat Setup klassen beschikbaar voor leveranciers](https://msdn.microsoft.com/library/windows/hardware/ff553426.aspx)gebruiken. Kies de klasse die het meest overeenkomt met het apparaat is aangesloten op de lokale computer. Dit kan zijn een Imaging-apparaat klasse of videovastlegapparaat klasse voor digitale camera's. U moet enkele experimenten met apparaatklassen te vinden van de juiste klasse-GUID die geschikt is voor de lokaal aangesloten USB-apparaat (in ons geval de webcamera) doen.

Een betere manier of de tweede optie is als volgt te werk om te zoeken naar de specifieke klasse-GUID:

1. Apparaatbeheer openen, zoek het apparaat dat wordt omgeleid en met de rechtermuisknop op het en open vervolgens de eigenschappen.
![Apparaatbeheer openen](./media/remoteapp-usbredir/ra-devicemanager.png)
2. Kies op het tabblad **Details** de eigenschap **Klasse-Guid**. De waarde die wordt weergegeven is de GUID van de klasse voor dit type apparaat.
![Camera-eigenschappen](./media/remoteapp-usbredir/ra-classguid.png)
3. Gebruik de klasse-Guid-waarde apparaten die overeenkomen met het.

Bijvoorbeeld:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s:<Class Guid value>"

U kunt meerdere apparaat opgenomen omleidingen de cmdlet dezelfde combineren. Bijvoorbeeld: wilt omleiden van lokale opslag en een USB-webcamera, cmdlet zien er zo uit:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:<Class Guid value>"

Alle apparaten die voldoen aan deze klasse-GUID in de opgegeven collectie worden omgeleid wanneer u apparaatomleiding met klasse-GUID. Als er meerdere computers met dezelfde USB-webcamera's op het lokale netwerk, kunt u bijvoorbeeld een afzonderlijke cmdlet wilt omleiden op alle van de webcamera uitvoeren.

## <a name="redirecting-a-usb-device-by-using-the-device-instance-id"></a>Omleiden van een USB-apparaat met behulp van de apparaatinstantie-ID

Als u meer verfijnde controle wilt omleiden per apparaat wilt, kunt u de parameter **USB\InstanceID** omleiding.

Het lastigste onderdeel van deze methode het vinden van het USB-apparaat-ID. Je hebt toegang tot de computer en het USB-apparaat. Volg deze stappen:

1. De sessie van extern bureaublad-apparaatomleiding inschakelen, zoals beschreven in [hoe kan ik mijn apparaten en bronnen gebruiken in een extern bureaublad-sessie?](http://windows.microsoft.com/en-us/windows7/How-can-I-use-my-devices-and-resources-in-a-Remote-Desktop-session)
2. Open een verbinding met extern bureaublad en klikt u op **Opties weergeven**.
3. Klik op **Opslaan als** om de huidige verbindingsinstellingen opslaan in een RDP-bestand.  
    ![De instellingen opslaan als een RDP-bestand](./media/remoteapp-usbredir/ra-saveasrdp.png)
4. Kies een bestandsnaam en een locatie op, bijvoorbeeld "MyConnection.rdp" en "Deze PC\Documents" en sla het bestand.
5. Het MyConnection.rdp-bestand met een teksteditor openen en zoek de exemplaar-ID van het apparaat dat u wilt omleiden.

Nu de exemplaar-ID in de volgende cmdlet gebruiken:

    Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s: USB\<Device InstanceID value>"



### <a name="help-us-help-you"></a>Help ons u te helpen 
Wist u dat naast de beoordeling van dit artikel en het toevoegen van opmerkingen naar beneden onder, kunt u wijzigingen aanbrengen in het artikel zelf? Iets ontbreekt? Iets mis? Schrijf iets dat is gewoon verwarrend? Omhoog schuiven en klik op **bewerken op GitHub** wijzigingen aan te brengen - die voor revisie naar ons komen, en vervolgens, wanneer we deze afmelden, ziet u de wijzigingen en verbeteringen hier.