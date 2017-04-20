<properties
   pageTitle="Uitgeven van toepassingen aan individuele gebruikers in een Azure RemoteApp-collectie (voorbeeld) | Microsoft Azure"
   description="Informatie over hoe u apps voor afzonderlijke gebruikers kunt publiceren in plaats van afhankelijk van groepen in Azure RemoteApp."
   services="remoteapp-preview"
   documentationCenter=""
   authors="piotrci"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="piotrci"/>

# <a name="publish-applications-to-individual-users-in-an-azure-remoteapp-collection-preview"></a>Uitgeven van toepassingen aan individuele gebruikers in een Azure RemoteApp-collectie (voorbeeld)

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

In dit artikel wordt uitgelegd hoe u uitgeven aan afzonderlijke gebruikers in een Azure RemoteApp-collectie. Dit is nieuwe functionaliteit in Azure RemoteApp, die momenteel in de 'persoonlijke voorbeeld' en alleen beschikbaar voor vroege gebruikers voor evaluatiedoeleinden selecteren.

Oorspronkelijk Azure RemoteApp ingeschakeld maar op één manier van 'publiceren'-toepassingen: de beheerder van de apps uit de afbeelding zou publiceren en ze zijn zichtbaar voor alle gebruikers in de collectie.

Een gebruikelijk scenario is voor veel toepassingen in één afbeelding bevatten en één collectie implementeren om de beheerkosten verlagen. Vaak is het mogelijk niet alle toepassingen die relevant zijn voor alle gebruikers: beheerders liever apps voor afzonderlijke gebruikers publiceren zodat ze overbodige toepassingen in hun toepassing feed niet zien.

Dit is mogelijk in Azure RemoteApp – momenteel als een voorbeeldfunctie beperkt. Hier volgt een korte samenvatting van de nieuwe functionaliteit:

1. Een collectie kan in twee modi worden geïnstalleerd:
 
  - de oorspronkelijke 'collectie-modus', alle gebruikers in een collectie zien gepubliceerde alle toepassingen. Dit is de standaardmodus.
  - de nieuwe "toepassingsmodus", waar zien gebruikers alleen toepassingen die zijn expliciet toegewezen

2. Op het moment dat kan de toepassingsmodus alleen worden ingeschakeld met Azure RemoteApp-PowerShell-cmdlets.

  - Als de waarde naar de toepassingsmodus, worden niet gebruiker is toegewezen in de collectie beheerd via de portal Azure. Gebruikerstoewijzing moet worden beheerd via PowerShell-cmdlets.

3. Gebruikers zien alleen de aan hen uitgegeven toepassingen. Echter, het nog steeds mogelijk voor een gebruiker voor het starten van de toepassingen beschikbaar is op de afbeelding door deze rechtstreeks in het besturingssysteem.
  - Deze functie biedt een veilige vergrendeling van de toepassingen. het beperkt inzicht in de toepassing feed alleen.
  - Als u wilt dat gebruikers van toepassingen isoleren, moet u afzonderlijke collecties voor dat gebruik.

## <a name="how-to-get-azure-remoteapp-powershell-cmdlets"></a>Het verkrijgen van Azure RemoteApp-PowerShell-cmdlets

Probeer de nieuwe functie in de voorbeeldweergave, moet u de Azure PowerShell-cmdlets gebruiken. Het is momenteel niet mogelijk de Azure Management portal gebruikt u een nieuwe toepassing publiceren.

Controleer eerst of u beschikt over de [module Azure PowerShell](../powershell-install-configure.md) geïnstalleerd.

Vervolgens de PowerShell-console in de administrator-modus starten en de volgende cmdlet uitvoert:

        Add-AzureAccount

U wordt gevraagd om uw Azure gebruikersnaam en wachtwoord. Eenmaal aangemeld, kunt u zijn Azure RemoteApp-cmdlets uitvoeren tegen de Azure abonnementen.

## <a name="how-to-check-which-mode-a-collection-is-in"></a>Hoe u kunt controleren welke modus een collectie in

De volgende cmdlet uitvoert:

        Get-AzureRemoteAppCollection <collectionName>

![De collectie modus controleren](./media/remoteapp-perapp/araacllelvel.png)

De eigenschap AclLevel kan de volgende waarden hebben:

- Collectie: de oorspronkelijke publicatie modus. Alle gebruikers zien dat alle gepubliceerde apps.
- Toepassing: de nieuwe publishing modus. Gebruikers zien alleen de apps rechtstreeks naar gepubliceerd.

## <a name="how-to-switch-to-application-publishing-mode"></a>Het overschakelen naar publishing toepassingsmodus

De volgende cmdlet uitvoert:

        Set-AzureRemoteAppCollection -CollectionName -AclLevel Application

Toepassingen publiceren staat blijft: in eerste instantie zichtbaar voor alle gebruikers alle van de oorspronkelijke gepubliceerde apps.

## <a name="how-to-list-users-who-can-see-a-specific-application"></a>Gebruikers die een bepaalde toepassing kunnen zien hoe

De volgende cmdlet uitvoert:

        Get-AzureRemoteAppUser -CollectionName <collectionName> -Alias <appAlias>

Hier worden alle gebruikers die de toepassing kunnen zien.

Opmerking: U ziet de toepassing aliassen ('alias app' in de bovenstaande syntaxis genoemd) door het uitvoeren van Get-AzureRemoteAppProgram - NaamVerzameling <collectionName>.

## <a name="how-to-assign-an-application-to-a-user"></a>Hoe u een toepassing toewijst aan een gebruiker

De volgende cmdlet uitvoert:

        Add-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

De gebruiker ziet nu de toepassing in de client Azure RemoteApp- en kan tot stand te brengen.

## <a name="how-to-remove-an-application-from-a-user"></a>Een aanvraag van een gebruiker verwijderen

De volgende cmdlet uitvoert:

        Remove-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

## <a name="providing-feedback"></a>Geven van feedback
Bedankt voor uw feedback en suggesties met betrekking tot deze functie. Vul de [enquête](http://www.instant.ly/s/FDdrb) te laten weten wat u denkt.

## <a name="havent-had-a-chance-to-try-the-preview-feature"></a>Nog niet had de voorbeeldfunctie uitproberen?
Als u niet hebben meegewerkt in het voorbeeld nog, gebruikt u deze [enquête](http://www.instant.ly/s/AY83p) om toegang te vragen.
