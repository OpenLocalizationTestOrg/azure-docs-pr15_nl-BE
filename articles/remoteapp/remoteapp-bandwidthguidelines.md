<properties 
    pageTitle="Netwerkbandbreedte Azure RemoteApp - algemene richtlijnen | Microsoft Azure"
    description="Enkele richtlijnen netwerk bandbreedte voor uw Azure RemoteApp-collecties en apps begrijpen."
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
    
# <a name="azure-remoteapp-network-bandwidth---general-guidelines-if-you-cant-test-your-own"></a>Netwerkbandbreedte Azure RemoteApp - algemene richtlijnen (als u niet zelf testen)

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Als u geen tijd of mogelijkheid om de [bandbreedte netwerktests](remoteapp-bandwidthtests.md) uitvoeren voor Azure RemoteApp, vindt hier u enkele tamelijk algemene richtlijnen die u hulp bij het inschatten van de bandbreedte van het netwerk per gebruiker.

Als u een combinatie van deze scenario's, wordt niet aanbevolen iets kleiner dan (of gelijk aan) 10 MB/s als de netwerkbandbreedte MINIMUM voor moderne internetverbinding apps in een externe omgeving. (Hoewel, zoals besproken, dit zal geen garantie voor een beter dan de gemiddelde gebruikerservaring.)

## <a name="complex-powerpoint-simple-powerpoint"></a>Complexe PowerPoint, eenvoudige PowerPoint

Azure RemoteApp wordt best op 100 MB LAN. In de 10 MB/s netwerkprofiel als jitter boven 120 ms is meer dan 5%, ziet de gebruiker een gemiddelde ervaring. Op 1 MB/s de verschillende is ongeautomatiseerde verwerking - bijvoorbeeld in een diavoorstelling, ziet de gebruiker niet bewegende overgangen helemaal omdat frames worden overgeslagen.

## <a name="internet-explorer-mixed-pdf-pdf-text"></a>Internet Explorer, gemengde PDF, PDF, tekst

10 MB/s netwerkprofiel lijkt LAN in de meeste aspecten. 1 MB/s biedt een ervaring met OK, hoewel er mogelijk enkele jitter wanneer een gebruiker er zijn afbeeldingen op het scherm.

## <a name="flash-video-youtube"></a>Flash video (YouTube)

100 MB/s LAN biedt de beste ervaring is 10 MB/s is aanvaardbaar (dat wil zeggen we houden de framesnelheid maar jitter toeneemt). Jitter is zeer hoog en opvallend op 1 MB/s.

## <a name="word-typing-word-remote-input"></a>Word-typen (Word externe invoer)
Dit is een lage bandbreedte gebruiksscenario. Bij 256 KB/s bieden we net zo goed een ervaring als LAN.

## <a name="learn-more"></a>Meer informatie
- [Schatting verbruik van de netwerkbandbreedte Azure RemoteApp](remoteapp-bandwidth.md)

- [Azure RemoteApp - hoe doet bandbreedte van het netwerk en de kwaliteit van zich werk samen?](remoteapp-bandwidthexperience.md)

- [Azure RemoteApp - tseting uw netwerkbandbreedte gebruikt met enkele algemene scenario's](remoteapp-bandwidthtests.md)