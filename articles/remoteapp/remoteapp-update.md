<properties
   pageTitle="Bijwerken van uw Azure RemoteApp-collectie | Microsoft Azure"
   description="Informatie over het bijwerken van uw Azure RemoteApp-collectie"
   services="remoteapp"
   documentationCenter=""
   authors="lizap"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="update-a-collection-in-azure-remoteapp"></a>Een collectie in Azure RemoteApp-update

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Er komt een tijd onvermijdelijk, wanneer u moet de afbeelding in uw verzameling Azure RemoteApp of apps bijwerken. Als u een van de afbeeldingen die deel uitmaken van uw abonnement Azure RemoteApp in een cloud of een hybride-collectie, worden alle updates worden verwerkt door Azure RemoteApp zelf, zodat u gemakkelijk kunt er.

Echter als u een aangepaste afbeelding (dat u zelf hebt gemaakt of dat u hebt gemaakt door in een van onze afbeeldingen), bent u verantwoordelijk voor het onderhouden van de afbeelding en apps. Als u uw image of een van de apps in het bijwerken, moet u een bijgewerkte versie van de afbeelding maken en vervolgens de bestaande afbeelding in uw verzameling vervangen door deze nieuwe, bijgewerkte installatiekopie.

Dus hoe doet u dit over het bijwerken van uw verzameling? Het is zeer eenvoudig:

1. De afbeelding die u hebt gebruikt in de collectie bijwerken. Toepassing van alle patches of updates nodig zijn en vervolgens met een nieuwe naam opslaan.
2. [Upload](remoteapp-uploadimage.md) of [importeren](remoteapp-image-on-azurevm.md) die afbeelding naar RemoteApp.
3. Klik nu op de collectie pagina op **bijwerken**.
4. De nieuwe afbeelding kiezen uit de lijst met **Afbeeldingen van de sjabloon** .
4. Hier is het moeilijkste - moet u beslissen hoe u kunt omgaan met alle gebruikers die momenteel van een app in de collectie gebruikmaken. U hebt de volgende opties:
    - **Gebruikers geven 60 minuten na de update**. Zodra de update is voltooid, verschijnt Azure RemoteApp een bericht naar alle actieve gebruikers hun werk opslaan en afmelden en opnieuw inloggen om door te geven. Na 60 minuten wordt actieve gebruikers die niet hebben afgemeld automatisch afgemeld. Gebruikers kunnen meteen aanmelden terug.
    - **Gebruikers onmiddellijk te ondertekenen**. Zodra de update is voltooid, kunt u zich afmelden bij alle gebruikers automatisch zonder waarschuwing. Als u deze optie kiest, kunnen gebruikers gegevens verliezen. Echter deze verbinding kunnen maken met de app direct.

1. Klik op het selectievakje om de update te starten.
