<properties 
   pageTitle="Beheer van Azure automatisering | Microsoft Azure"
   description="Dit artikel bevat meerdere onderwerpen voor het beheren van een automatisering Azure-omgeving.  Momenteel bevat gegevens en back-ups van Azure automatisering noodherstel in Azure automatisering."
   services="automation"
   documentationCenter=""
   authors="SnehaGunda"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/02/2016"
   ms.author="bwren;sngun" />

# <a name="managing-azure-automation-data"></a>Azure automatisering gegevens beheren

Dit artikel bevat meerdere onderwerpen voor het beheren van een automatisering Azure-omgeving.

## <a name="data-retention"></a>Gegevens bewaren

Wanneer u een resource in Azure automatisering verwijdert, blijft deze gedurende 90 dagen voor controledoeleinden voordat het definitief wordt verwijderd.  U kan zien of de resource gedurende deze tijd gebruiken.  Dit beleid geldt ook voor resources die horen bij een automatisering-account die wordt verwijderd.

Azure automatisering wordt automatisch verwijderd en verwijdert u taken die ouder zijn dan 90 dagen.

De volgende tabel bevat een overzicht van het bewaarbeleid voor verschillende resources.

|Gegevens|Beleid|
|:---|:---|
|Rekeningen|90 dagen nadat de account is verwijderd door een gebruiker permanent verwijderd.|
|Activa|90 dagen nadat het activum is verwijderd door een gebruiker of 90 dagen na de rekening met dat het activum is verwijderd door een gebruiker permanent verwijderd.|
|Modules|90 dagen nadat de module is verwijderd door een gebruiker of 90 dagen na de rekening met dat de module is verwijderd door een gebruiker permanent verwijderd.|
|Runbooks|90 dagen nadat de resource is verwijderd door een gebruiker of 90 dagen na de rekening met dat de resource is verwijderd door een gebruiker permanent verwijderd.|
|Taken|Verwijderde en permanent verwijderde 90 dagen na de laatste wordt gewijzigd. Het is mogelijk nadat de taak is voltooid, is gestopt of is onderbroken.|
|Knooppunt configuraties/MOF-bestanden| Oude knooppuntconfiguratie wordt permanent verwijderd van 90 dagen nadat de configuratie van een nieuw knooppunt wordt gegenereerd.|
|DSC-knooppunten| 90 dagen nadat het knooppunt niet-geregistreerde van automatisering rekening met Azure portal of de cmdlet [Registratie-AzureRMAutomationDscNode](https://msdn.microsoft.com/library/mt603500.aspx) in Windows PowerShell wordt verwijderd definitief. Knooppunten zijn 90 dagen na de rekening met dat het knooppunt is verwijderd door een gebruiker ook permanent verwijderd. |
|Knooppunt rapporten| 90 dagen nadat een nieuw rapport wordt gegenereerd voor dat knooppunt definitief verwijderd|

Het bewaarbeleid wordt toegepast op alle gebruikers en kan niet op dit moment worden aangepast.

## <a name="backing-up-azure-automation"></a>Back-ups van Azure automatisering

Als u een account voor automatisering in Microsoft Azure verwijdert, worden alle objecten in de account verwijderd met inbegrip van runbooks, modules, configuraties, instellingen, taken en activa. De objecten kunnen niet worden hersteld nadat de account is verwijderd.  De volgende informatie kunt u de inhoud van uw account Automatisering een back-up voordat u het verwijdert. 

### <a name="runbooks"></a>Runbooks

U kunt uw runbooks exporteren naar scriptbestanden met behulp van de Portal Azure Management of de cmdlet [Get-AzureAutomationRunbookDefinition](https://msdn.microsoft.com/library/dn690269.aspx) in Windows PowerShell.  Deze scriptbestanden kunnen worden geïmporteerd in een andere account voor automatisering, zoals besproken in [maken of importeren van een Runbook](https://msdn.microsoft.com/library/dn643637.aspx).


### <a name="integration-modules"></a>Integratiemodules

U kunt integratiemodules exporteren vanuit Azure automatisering.  U moet ervoor zorgen dat ze buiten de account Automatisering beschikbaar zijn.

### <a name="assets"></a>Activa

U kunt [activa](https://msdn.microsoft.com/library/dn939988.aspx) exporteren vanuit Azure automatisering.  De Azure Management Portal moet u de details van de variabelen, referenties, certificaten, schema's en verbindingen noteren.  Vervolgens moet u handmatig maken alle activa die worden gebruikt door de runbooks die u in een ander automatisering importeert.

[Azure-cmdlets](https://msdn.microsoft.com/library/dn690262.aspx) kunt u ophalen van details van niet-gecodeerde activa en een opslaan voor toekomstig gebruik of gelijkwaardige activa maken in een andere account voor automatisering.

U kunt de waarde voor gecodeerde variabelen of het wachtwoordveld van referenties met behulp van cmdlets niet ophalen.  Als u deze waarden niet kent, vervolgens ophalen ze uit een runbook met de [Get-AutomationVariable](https://msdn.microsoft.com/library/dn940012.aspx) en [Get AutomationPSCredential](https://msdn.microsoft.com/library/dn940015.aspx) activiteiten.

U kunt certificaten exporteren vanuit Azure automatisering.  U moet ervoor zorgen dat alle certificaten beschikbaar buiten Azure.

### <a name="dsc-configurations"></a>DSC-configuraties

U kunt uw configuraties exporteren naar scriptbestanden met behulp van de Portal Azure Management of de cmdlet [Export-AzureRmAutomationDscConfiguration](https://msdn.microsoft.com/library/mt603485.aspx) in Windows PowerShell. Deze configuraties kunnen worden geïmporteerd en gebruikt in een andere account voor automatisering.


##<a name="geo-replication-in-azure-automation"></a>Geo-replicatie in Azure automatisering

Geo-replicatie, standaard in Azure automatisering rekeningen, back-ups van gegevens op een andere geografische regio voor redundantie. U kunt een primaire regio kiezen bij het instellen van uw account en vervolgens een tweede regio is toegewezen automatisch. De secundaire gegevens opgehaald uit de primaire regio wordt voortdurend bijgewerkt in het geval van verlies van gegevens.  

De volgende tabel ziet de beschikbare regio voor primaire en secundaire koppelingen.

|Primaire            |Secundaire
| ---------------   |----------------
|Zuid-centraal-Verenigde Staten   |Centrale Noord-Amerikaanse
|Verenigde Staten Oost 2          |Centrale VS
|West-Europa        |Noord-Europa
|Zuid-Oost-Azië    |Oost-Azië
|East Japan         |West Japan

In het onwaarschijnlijke geval dat een voor primaire regiogegevens verloren gegaan zijn, is Microsoft probeert te herstellen. Als de primaire gegevens kunnen niet worden hersteld, vervolgens geo-failover wordt uitgevoerd en de desbetreffende klanten krijgt hierover via hun abonnement.

