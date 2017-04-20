<properties
    pageTitle="Met apps van App-V en Azure RemoteApp | Microsoft Azure"
    description="Informatie over het gebruik van App-V apps in Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="ericorman"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="using-app-v-apps-in-azure-remoteapp"></a>Met behulp van App-V apps in Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

U kunt toepassingen App-V in een collectie Azure RemoteApp-hybride, waarvoor aan domein toevoegen.

Voordat u begint, zorg ervoor dat de App-V 5.1-client installeren met de nieuwste updates. Moet u voor het maken van een [aangepaste installatiekopie van](remoteapp-create-custom-image.md) de client App-V.  

Het is eenvoudig te gebruiken van uw bestaande infrastructuur van App-V met Azure RemoteApp. Aangezien een hybride collectie is geïmplementeerd in een VNET met Azure die toegang tot de domeincontroller heeft en de VMs-domein gekoppeld zijn, kunt u gebruikmaken van uw bestaande App-v-infrastructuur en de implementatie van methoden voor easyily-hosttoepassing App-V in Azure RemoteApp. Hier zijn enkele overwegingen die u moet zich bewust zijn van de op basis van het type App-V-implementatie die u momenteel hebt:

| Configuratie-opties |                       | Positief                                                               | Negatief                                                                                              |
|-----------------------|-----------------------|------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| Leveringsmethode       | Streaming (op aanvraag) | App is altijd de meest recente en zoet                                     | Eerste keer latentie                                                                                    |
|                       | Gemonteerd               | Snelste; App is al aanwezig op de VM                              | Bol - inneemt Beeldruimte (127 GB limiet)                                                           |
| Locatie-App-opslag  | Gedeelde inhoud        | App wordt uitgevoerd in het geheugen van Azure RemoteApp-sessie                         | Eats geheugen en een goede verbinding met streaming server (bestand) waarin de toepassing zich bevindt                      |
|                       | Schijf (in cache)         | Snelle uitvoering. App is niet afhankelijk van de beschikbaarheid van de inhoudsbron | Bol - inneemt Beeldruimte (127 GB limiet)                                                           |
| Doelitems van het type             | Gebruiker                  | Volledige standalone App-V-infrastructuur vereist                          |                                                                                                       |
|                       | Global (machine)      |  Vooraf publiceren of publiceren met als doel server                         |  Moet uw Azure afbeelding bijwerken als u wilt dat de app (enorme) bijwerken. Nogal wat ruimte in de afbeelding. |

 Nadat u de aangepaste installatiekopie en uw verzameling hybride maken, publiceren van uw toepassing gebruikers toewijzen en geniet van uw bestaande App-V-toepassingen in Azure RemoteApp bezorgd bij elk apparaat ergens gehost.
