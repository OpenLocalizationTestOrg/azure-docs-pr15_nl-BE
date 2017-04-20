
<properties 
    pageTitle="Schatting verbruik van de netwerkbandbreedte RemoteApp-Azure | Microsoft Azure"
    description="Meer informatie over de bandbreedtevereisten netwerk voor uw Azure RemoteApp-collecties en apps."
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

# <a name="estimate-azure-remoteapp-network-bandwidth-usage"></a>Schatting verbruik van de netwerkbandbreedte Azure RemoteApp 

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Remote Desktop Protocol (RDP) Azure RemoteApp gebruikt voor de communicatie tussen toepassingen die worden uitgevoerd in de Azure cloud en uw gebruikers. Dit artikel bevat enkele richtlijnen die kunt u inschatten dat netwerkgebruik en mogelijk evalueren netwerkbandbreedte gebruikt per gebruiker Azure RemoteApp.

Schatten van bandbreedtegebruik per gebruiker is erg complex en meerdere toepassingen tegelijk worden uitgevoerd in multitasking-scenario's waarin toepassingen kunnen invloed hebben op elkaars prestaties op basis van hun vraag naar bandbreedte van het netwerk vereist. Ook het type extern bureaublad-client (bijvoorbeeld Mac client versus HTML5 client) kan leiden tot verschillende bandbreedte resultaten. Om te werken door middel van deze problemen, zullen we scenario's voor het gebruik opsplitsen in verschillende van de algemene categorieën voor het repliceren van realistische scenario's. (Indien de concrete scenario uiteraard een combinatie van categorieën is en per gebruiker verschilt.)

Voordat we verder - Houd er rekening mee dat we ervan uitgaan dat RDP biedt een goede tot uitstekende ervaring voor de meeste scenario's voor gebruik in netwerken met latentie dan 120 ms en bandbreedte meer dan 5 MB - dit is gebaseerd op de mogelijkheid van RDP dynamisch aanpassen met behulp van de beschikbare netwerkbandbreedte en de geschatte toepassing bandbreedte nodig. Dit artikel gaat verder dan de "meeste gebruiksscenario's" te kijken naar de rand, waar de scenario's beginnen te laten verdwijnen en gebruikerservaring beginnen af te nemen.

Bekijk nu de volgende artikelen voor meer informatie, met inbegrip van factoren te overwegen, aanbevelingen van de basislijn en wat we niet hebt opgenomen in onze ramingen.

- [Hoe doet bandbreedte van het netwerk en de kwaliteit van zich werk samen?](remoteapp-bandwidthexperience.md)
- [Het verbruik van de netwerkbandbreedte met enkele algemene scenario's testen](remoteapp-bandwidthtests.md)
- [Snelle richtlijnen als u geen tijd of mogelijkheid om te testen](remoteapp-bandwidthguidelines.md)


## <a name="what-are-we-not-including"></a>Wat we niet opneemt?

Als u de voorgestelde tests en de aanbevelingen van onze algemene (en weliswaar algemene) bekijkt, let er zijn verschillende factoren die we hebben geen rekening gehouden. Bijvoorbeeld, downloaden de gebruiker ervaring complicaties geleverd door de asymmetrische aard van uploaden en bandbreedte. De asymmetrische aard van de meeste Wi-Fi-netwerken is ook van invloed op de prestaties en de perceptie van gebruiker ervaring. Voor interactieve scenario's worden mogelijk lager dan de upstream, die kunnen verhogen van het aantal verloren frames voor video- of audio en daarom van invloed op de perceptie van de gebruiker van de streaming ervaring de downstream verkeer voorrang. U kunt uw eigen experimenten om te zien wat goed is voor uw specifieke use-case en netwerk uitvoeren.

Hoewel we apparaatomleiding bespreken, heeft we geen rekening de gevolgen van de bandbreedte van het netwerkverkeer dat wordt veroorzaakt door de aangesloten apparaten, zoals opslag, printers, scanners, camera's voor web en andere USB-apparaten. Het effect van deze apparaten meestal bereikt de vraag naar bandbreedte tijdelijk en verdwijnt wanneer de taak voltooid is. Maar als vaak wordt gedaan, die vraag bandbreedte kan erg opvallend.

Wij ook niet aan de orde hoe één gebruiker kan invloed hebben op andere gebruikers in hetzelfde netwerk. Bijvoorbeeld één gebruiker die 4K video op een 100 MB/s netwerk kan sterk van invloed op andere gebruikers op hetzelfde netwerk dezelfde taak wilt. Helaas wordt het steeds moeilijker om te bepalen van de gevolgen van het gelijktijdig gebruik om een gemeenschappelijke of alle waarmee aanbeveling over hoe het systeem wordt uitgevoerd op statistische. We kunnen zeggen, is dat de onderliggende protocol-technologie maakt optimaal gebruik van de beschikbare netwerkbandbreedte, maar heeft beperkingen.