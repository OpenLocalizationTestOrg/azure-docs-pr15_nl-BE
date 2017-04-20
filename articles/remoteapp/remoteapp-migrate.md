
<properties
    pageTitle="Gegevens migreren van Azure RemoteApp | Microsoft Azure"
    description="Informatie over het migreren van uw gebruikersgegevens en naar Azure RemoteApp."
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



# <a name="how-to-migrate-data-into-and-out-of-azure-remoteapp"></a>Het migreren van gegevens naar en van Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

U kunt veel verschillende tools en methoden [Gebruikersgegevens](remoteapp-upd.md) overbrengen naar en van Azure RemoteApp. Hier volgen een paar methoden:

- Kopiëren en plakken via het Klembord delen
- Kopiëren van bestanden en gegevens op een bestandsserver
- Bestanden kopiëren naar OneDrive voor bedrijven via een browser
- Bestanden kopiëren met behulp van Mapomleiding

>[AZURE.NOTE] 
> Kunt u de OneDrive niet inschakelen voor bedrijfs- of agenten van sync - ze [worden niet ondersteund](remoteapp-onedrive.md) in Azure RemoteApp.

## <a name="use-copy-and-paste-in-file-explorer"></a>Kopiëren en plakken in de File Explorer

Kopiëren en plakken via het Klembord is in RemoteApp-implementaties [standaard](remoteapp-redirection.md)ingeschakeld. Hiermee kunnen gebruikers bestanden kopiëren tussen de lokale PC en RemoteApp-apps. Vaak via het normale verloop van het gebruik van apps in RemoteApp-hebben gebruikers opgeslagen bestanden in hun UPDs - verplaatsen dat gegevens van RemoteApp eenvoudig is:

1. [File Explorer publiceren als een app](remoteapp-publish.md) in een RemoteApp-collectie. (Let erop dat dit een beheertaak.)
2. Direct voor het starten van de toepassing File Explorer is gepubliceerd en kopiëren en plakken van bestanden, zowel in hun UPD en uit het gebruik van die gebruikers.

## <a name="upload-files-and-data-to-a-file-server-by-using-standard-network-file-copy"></a>Uploaden van bestanden en gegevens op een bestandsserver met behulp van standaard netwerk kopiëren

Bestandsservers vaak organisaties gebruiken voor het opslaan van algemene gegevens. Als u de naam van de server of locatie, kunnen gebruikers bladeren van de server op het lokale netwerk en kopieer vervolgens hun bestanden, worden er veel net boven. U zult opnieuw File Explorer naar RemoteApp publiceren en deze vervolgens delen met uw gebruikers.

>[AZURE.NOTE] 
> De bestandsserver moet zich op het netwerk van routeerbaar RemoteApp is geïmplementeerd in.

## <a name="copy-files-to-onedrive-for-business"></a>Bestanden kopiëren naar OneDrive voor bedrijven
Hoewel de OneDrive voor Business sync agent in RemoteApp-kan niet worden ingeschakeld, kunt u nog steeds bestanden kopiëren van uw UPD naar OneDrive voor bedrijven via een browser. 

1. File Explorer publiceren naar RemoteApp en geef vervolgens de gebruikers toegang hebben tot de bestanden via die app. 
2. Het gemakkelijkst bestanden over te brengen als ze zijn gecomprimeerd, zodat gebruikers een ZIP-bestand met alle bestanden te verplaatsen naar OneDrive voor bedrijven te maken.
3. Gebruikers vragen om gaat u naar de Office 365 portal, en ga vervolgens naar OneDrive en upload het ZIP-bestand.

## <a name="copy-files-by-using-drive-redirection"></a>Bestanden kopiëren met behulp van de omleiding van clientstations

Als u [omleiding van clientstations](remoteapp-redirection.md)hebt ingeschakeld, hebt u al een toegewezen station gemaakt voor uw gebruikers. In dit geval kunnen zij zip-bestanden van het omgeleide station en slaat u deze op de lokale computer.