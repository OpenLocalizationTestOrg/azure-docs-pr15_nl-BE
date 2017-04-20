<properties 
    pageTitle="Azure RemoteApp - tests van het verbruik van de netwerkbandbreedte met enkele algemene scenario's | Microsoft Azure"
    description="Informatie over wat de behoeften voor uw netwerk bandbreedte voor RemoteApp-Azure veelvoorkomende gebruiksscenario's die u kunnen helpen uitzoeken."
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
    
# <a name="azure-remoteapp---testing-your-network-bandwidth-usage-with-some-common-scenarios"></a>Azure RemoteApp - het verbruik van de netwerkbandbreedte met enkele algemene scenario's testen

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Zoals we in de [raming Azure RemoteApp netwerkbandbreedte gebruikt besproken](remoteapp-bandwidth.md), is de beste manier om erachter te komen wat de impact van Azure RemoteApp op uw netwerk moet worden uitgevoerd sommige gebruik getest. Deze tests voor een gegeven periode uitvoeren en meten van de bandbreedte die nodig is voor elk scenario. Als u de mogelijkheid hebt, kunt u ook het netwerk pakket verlies en netwerk-jitter inzicht in het netwerk patronen die worden gemaakt in uw omgeving te meten.

    
Bij de beoordeling van het bandbreedtegebruik, houd er rekening mee dat gebruik tussen verschillende gebruikers binnen uw bedrijf varieert. Bijvoorbeeld tekst readers en writers gewoonlijk worden verbruikt minder bandbreedte dan gebruikers die met video werken. Voor de beste resultaten bestuderen van uw eigen behoeften van de gebruiker en maakt een combinatie van de volgende scenario's die de gebruikers in uw bedrijf het beste wordt aangeduid. Vergeet niet te [controleren van de factoren die invloed bandbreedtegebruik en de gebruiker zich](remoteapp-bandwidthexperience.md) - waarmee u de ideale testen te identificeren.

Eerst informatie lezen over de tests, kies uw mix en ze vervolgens uit te voeren. In de onderstaande tabel kunt u prestaties bijhouden.

>[AZURE.NOTE] Als u niet uw eigen netwerk testen of u hebt niet de tijd om dit te doen, kijk dan eens onze [netwerk bandbreedte ramingen/aanbevelingen](remoteapp-bandwidthguidelines.md). De kilometerstand kan echter variëren als u *kunt* uw eigen tests uitvoeren, moet u.


## <a name="the-usage-tests"></a>De tests gebruik
Elk van deze tests uitgevoerd voor verschillende hoeveelheden tijd en verschillende functies/functies die netwerk bandbreedte testen. Vergeet niet de mix van test kiezen dat het best overeenkomt met uw afzonderlijke bedrijfsgebruikers.
 
### <a name="executivecomplex-powerpoint---run-for-900-1000-seconds"></a>Executive/complexe PowerPoint - 900-1000 seconden uitvoeren

Het presenteert een gebruiker tussen 45-50 hifi-dia's met behulp van Microsoft Office PowerPoint in de modus volledig scherm. De dia's moeten bevatten afbeeldingen, overgangen (met animaties) en achtergronden met kleurverloop die typerend zijn voor uw bedrijf zijn. De gebruiker moet ten minste 20 seconden te besteden aan elke dia.
    
In dit scenario maakt bursty verkeer, wanneer een dia-naar de volgende dia in de presentatie overgangen.
    
### <a name="simple-powerpoint---run-for-620-seconds"></a>Eenvoudige PowerPoint - 620 ~ seconden uitvoeren

Presenteert een gebruiker een eenvoudige PowerPoint-bestand met ongeveer 30 dia's met behulp van Microsoft Office PowerPoint in de modus volledig scherm. De dia's tekst intensiever dan in het geval van PowerPoint Executive/complex zijn en eenvoudiger achtergronden en afbeeldingen (zwarte diagrammen). 
    
### <a name="internet-explorer---run-for-250-seconds"></a>Internet Explorer - 250 seconden uitvoeren

Een gebruiker bezoekt het web met Internet Explorer. De gebruiker bladert en een combinatie van tekst, natuurlijke beelden en sommige schema's wordt geschoven. De webpagina's opgeslagen op de lokale vaste schijf van de Remote Desktop Session Host (RD Session Host)-server als een. MHT-bestand. De gebruiker wordt geschoven met Page Up, Page Down, omhoog en omlaag toetsen, met verschillende intervallen voor elke sleuteltype van scroll:
    
    - Omlaag - 250 toetsaanslagen zeer 500 ms
    - Page Up - 36 toetsaanslagen elke 1000 ms
    - Down - 75 toetsaanslagen elke 100 ms
    - Page Down - 20 toetsaanslagen elke 500 ms
    - Maximaal - toetsaanslagen 120 door elke 300 ms
    
### <a name="pdf-document---simple---run-for-610-seconds"></a>PDF-document - eenvoudig - 610 ~ seconden uitvoeren
Een gebruiker leest en op verschillende manieren wordt gezocht naar een PDF-document met behulp van Adobe Acrobat Reader. Het document bestaat uit tabellen en eenvoudige grafieken meerdere lettertypen. Het document is 35-40 pagina's lang. De gebruiker wordt geschoven door twee verschillende tussenpozen, achterwaarts en doorstuurt, met een grootte van vier verschillende in-/ uitzoomen (aanpassen aan pagina, aanpassen aan de breedte van 100%, en een andere keuze). Het zoomen, zorgt u ervoor dat de tekst (lettertype) worden weergegeven in verschillende formaten. Schuiven is naar beneden met behulp van de Page Up, Page Down, omhoog en omlaag toetsen, met verschillende intervallen voor elke scroll.

### <a name="pdf-document---mixed---run-for-320-seconds"></a>PDF-document - gemengde - Run ~ 320 seconden
Een gebruiker leest en op verschillende manieren wordt gezocht naar een PDF-document met behulp van Adobe Acrobat Reader. Het document bestaat uit afbeeldingen van hoge kwaliteit (inclusief foto's), tabellen, eenvoudige grafieken en verschillende lettertypen. De gebruiker wordt geschoven door twee verschillende tussenpozen, achterwaarts en doorstuurt, met een grootte van vier verschillende in-/ uitzoomen (aanpassen aan pagina, aanpassen aan de breedte van 100%, en een andere keuze). Het zoomen, zorgt u ervoor dat de tekst (lettertype) worden weergegeven in verschillende formaten. Schuiven is naar beneden met behulp van de Page Up, Page Down, omhoog en omlaag toetsen, met verschillende intervallen voor elke scroll.

### <a name="flash-video-playback---run-for-180-seconds"></a>Afspelen van Flash video - ~ 180 seconden uitvoeren
Een gebruiker bekijkt een Adobe Flash-gecodeerde video ingesloten in een webpagina. De webpagina wordt opgeslagen in de lokale vaste schijf van de RD Session Host-server. De video wordt afgespeeld in Internet Explorer door een ingesloten speler die invoegtoepassing.

In dit scenario wordt geëmuleerd gebruikers rijke inhoud webpagina's met multimedia te bekijken. De meeste van de gegevens moet bo via VOBR.

### <a name="word-remote-typing---run-for-1800-seconds"></a>Externe typen Word - ~ 1800 seconden uitgevoerd
Een gebruiker typt een document via een RDP-sessie. Toetsaanslagen worden verzonden vanaf de client via de RDP-sessie naar een document in Microsoft Word uitgevoerd in de externe sessie. De typen is een teken van elk 250 ms (totale 7050 tekens). 

Dit is een van de meest voorkomende scenario's voor een kenniswerker. In dit scenario wordt getest de reactietijd van een gebruiker te typen in een moderne werk-processor. In dit scenario is gevoelig voor zelfs kleine veranderingen in het gebruik van de bandbreedte.

## <a name="tracking-the-test-results"></a>Bijhouden van de testresultaten

De volgende tabel kunt u de scenario's in uw omgeving te evalueren. De onderstaande gegevens is alleen ter illustratie - mogelijk sterk afwijken van wat u ziet. 

Voor eenvoud, nemen we aan dat alle scenario's zijn getest met behulp van de resolutie 1920 x 1080 pixels scherm en TCP-transport in een netwerk met latentie (vertraging) onder 200 ms en netwerk-jitter in het merk + 120 ms van ongeveer 1%.

Over de tabel:
- **Gemiddelde ervaring** bevat de bandbreedte van het netwerk waar de productiviteit van de gebruiker niet aanmerkelijk wordt beïnvloed maar af en toe problemen met video- of haperend niet uitsluit. Het systeem is snel herstellen door te profiteren van de dynamische logica. De netwerk bandbreedte ramingen poging tot waarborging van de kwaliteit van de gebruikerservaring.
 - **Noticeable problemen (breekpunt)** bevat de bandbreedte van het netwerk waar gebruikers tegenkomen belangrijke problemen in hun ervaring en hun productiviteit voor meetbare perioden wordt beïnvloed. Op dit moment de RDP-algoritmen hebben moeite en kwaliteit van de ervaring van de gebruiker vanwege onvoldoende bandbreedte kunnen niet garanderen.
 - **Aanbevolen** bevat de netwerkbandbreedte voor goed of uitstekend aanbevolen. Het is meestal een stap hoger dan de waarde in de overeenkomstige kolom met **gemiddelde ervaring** .
 - **Notities** bevatten opmerkingen.
 
| Test                  | Gemiddelde ervaring | Merkbare problemen (breekpunt) | Aanbevolen netwerkbandbreedte | Notities                                                              |
|-----------------------|--------------------|---------------------------------|-------------------------------|--------------------------------------------------------------------|
| Executive/complexe PPT | 10 MB/s             | 1 MB/s                           | > 10 MB/s, 100 MB/s, voorkeur    | 1 MB/s zijn veel animaties verloren                                   |
| Eenvoudige PPT            | 5 MB/s              | 256 KB/s                         | 10 MB/s                        | Bij 256 KB/s laden de dia's met een merkbare vertraging                   |
| Internet Explorer     | 10 MB/s             | 1 MB/s                           | > 10 MB/s, 100 MB/s, voorkeur    | Op 1 MB/s web video's zijn wazig en schokkerig, snelle schuiven problemen |
| Eenvoudige PDF            | 1 MB/s              | 256 KB/s                         | 5 MB/s                         | Bij 256 KB/s duurt het tijd om te laden van de pagina                       |
| Gemengde PDF             | 1 MB/s             | 256 KB/s                         | 5 MB/s                         | Op 256 KB/s, neemt de pagina een aanzienlijke hoeveelheid tijd te laden    |
| Flash-video afspelen  | 10 MB/s             | 1 MB/s                           | > 10 MB/s, 100 MB/s, voorkeur    | 1 MB/s, de video is korrelig en sommige frames zijn gedropt           |
| Externe typen in Word    | 256 KB/s            | 128 KB/s                         | 1 MB/s                         | Bij 256 KB/s gebruiker is het mogelijk de tijd tussen de toetsaanslagen             |

Evaluatie van de bandbreedte van het netwerk per gebruiker, een combinatie van de bovenstaande scenario's en het corresponderende gedeelte van de vereiste netwerkbandbreedte te maken. Kies het hoogste nummer nodig voor uw scenario's. Omdat gebruikers bijna nooit gebruikt als het systeem alleen, rekening houden met enkele reserve voor gebruikers die tegelijk op hetzelfde netwerk werken.
     
## <a name="learn-more"></a>Meer informatie
- [Schatting verbruik van de netwerkbandbreedte Azure RemoteApp](remoteapp-bandwidth.md)

- [Azure RemoteApp - hoe doet bandbreedte van het netwerk en de kwaliteit van zich werk samen?](remoteapp-bandwidthexperience.md)

- [Netwerkbandbreedte Azure RemoteApp - algemene richtlijnen (als u niet zelf testen)](remoteapp-bandwidthguidelines.md)