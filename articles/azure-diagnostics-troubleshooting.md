<properties
    pageTitle="Azure diagnostische gegevens oplossen"
    description="Problemen oplossen bij het gebruik van diagnostische gegevens van Azure in Azure Cloud-Services, virtuele Machines en "
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="robb"/>


# <a name="azure-diagnostics-troubleshooting"></a>Azure Diagnostics, Troubleshooting
Met diagnostische gegevens Azure relevante informatie over het oplossen van problemen. Zie voor meer informatie over diagnostische gegevens van Azure, [Azure Modemdiagnose-overzicht](azure-diagnostics.md#cloud-services).

## <a name="azure-diagnostics-is-not-starting"></a>Azure diagnostische gegevens wordt niet gestart.

Diagnostische gegevens bestaat uit twee onderdelen: een invoegtoepassing Gast agent en de monitoring agent. U kunt de logboekbestanden voor meer informatie over waarom diagnostics start niet **DiagnosticsPluginLauncher.log** en **DiagnosticsPlugin.log** controleren.  
  
Logboekbestanden voor de invoegtoepassing Gast agent bevinden zich in een Cloud Service-rol in: 

``` 
C:\Logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.6.3.0\ 
``` 

In een Azure Virtual Machine bevinden logboekbestanden voor de invoegtoepassing Gast agent zich in: 

``` 
C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\1.6.3.0\Logs\ 
``` 
 
De laatste regel van de logboekbestanden wordt de afsluitcode bevatten.  

``` 
DiagnosticsPluginLauncher.exe Information: 0 : [4/16/2016 6:24:15 AM] DiagnosticPlugin exited with code 0 
``` 

De invoegtoepassing als resultaat de volgende exit-codes:

Afsluitcode|Beschrijving
---|---
0|Succes.
-1|Algemene fout.
-2|De rcf kan bestand niet laden.<p>Dit is een interne fout moet alleen gebeuren als de Gast-agent plugin starten is handmatig, niet juist hebt aangeroepen, op de VM.
-3|De diagnostische gegevens van het configuratiebestand laden niet.<p><p>Oplossing: Dit is het resultaat van een configuratiebestand schemavalidatie niet wordt doorgegeven. De oplossing is om een configuratiebestand dat in overeenstemming is met het schema.
-4|De lokale resource directory wordt al gebruikt door een ander exemplaar van de monitoring agent diagnostiek.<p><p>Oplossing: Geef een andere waarde voor **LocalResourceDirectory**.
-6|De Gast agent plugin starten poging tot starten van diagnostische gegevens met een ongeldige opdrachtregel.<p><p>Dit is een interne fout moet alleen gebeuren als de Gast-agent plugin starten is handmatig, niet juist hebt aangeroepen, op de VM.
-10|De invoegtoepassing Diagnostics is afgesloten met een niet-afgehandelde uitzondering.
-11|De Gast kon geen om het proces te starten en de controle van de monitoring agent verantwoordelijk.<p><p>Oplossing: Controleer of u voldoende systeembronnen beschikbaar zijn voor het starten van nieuwe processen.<p>
-101|Ongeldige argumenten bij het aanroepen van de invoegtoepassing voor diagnostische gegevens.<p><p>Dit is een interne fout moet alleen gebeuren als de Gast-agent plugin starten is handmatig, niet juist hebt aangeroepen, op de VM.
-102|Het proces van de invoegtoepassing is kan niet worden geïnitialiseerd.<p><p>Oplossing: Controleer of u voldoende systeembronnen beschikbaar zijn voor het starten van nieuwe processen.
-103|Het proces van de invoegtoepassing is kan niet worden geïnitialiseerd. Met name is het het object logger.<p><p>Oplossing: Controleer of u voldoende systeembronnen beschikbaar zijn voor het starten van nieuwe processen.
-104|De rcf kan bestand niet laden die door de agent Gast.<p><p>Dit is een interne fout moet alleen gebeuren als de Gast-agent plugin starten is handmatig, niet juist hebt aangeroepen, op de VM.
-105|De invoegtoepassing voor diagnostiek kan de diagnostische gegevens van het bestand niet openen.<p><p>Dit is een interne fout moet alleen gebeuren als de invoegtoepassing voor diagnostische gegevens wordt handmatig, niet juist hebt aangeroepen, op de VM.
-106|De diagnostische gegevens van het bestand kan niet worden gelezen.<p><p>Oplossing: Dit is het resultaat van een configuratiebestand schemavalidatie niet wordt doorgegeven. De oplossing is dus om een configuratiebestand dat in overeenstemming is met het schema. Hier vindt u het XML-bestand dat wordt geleverd aan de uitbreiding van de diagnostische gegevens in de map *%SystemDrive%\WindowsAzure\Config* op de VM. Open de juiste XML-bestand en zoek **Microsoft.Azure.Diagnostics**en vervolgens het veld **xmlCfg** . De gegevens zijn base64-gecodeerde zodat u [dit decoderen moet](http://www.bing.com/search?q=base64+decoder) voor een overzicht van het XML-bestand dat is geladen door diagnostische gegevens.<p>
-107|De resource directory pass naar de monitoring agent is ongeldig.<p><p>Dit is een interne fout moet alleen gebeuren als de monitoring agent is handmatig, niet juist hebt aangeroepen, op de VM.</p>
-108    |Kan het configuratiebestand diagnostische gegevens converteren naar het monitoring agent-configuratiebestand.<p><p>Dit is een interne fout moet alleen gebeuren als de invoegtoepassing voor diagnostische gegevens handmatig wordt aangeroepen met een ongeldige configuratiebestand.
-110|Diagnostische gegevens van algemene configuratiefout.<p><p>Dit is een interne fout moet alleen gebeuren als de invoegtoepassing voor diagnostische gegevens handmatig wordt aangeroepen met een ongeldige configuratiebestand.
-111|Kan geen monitoring agent te starten.<p><p>Oplossing: Controleer of dat er onvoldoende systeembronnen beschikbaar zijn.
-112|Algemene fout


## <a name="diagnostics-data-is-not-logged-to-azure-storage"></a>Diagnostische gegevens is niet geregistreerd in Azure opslag
Azure diagnostische gegevens van alle gegevens worden opgeslagen in Azure opslag.

De meest voorkomende oorzaak van ontbrekende gegevens zijn de accountgegevens onjuist gedefinieerde opslag.

Oplossing: Corrigeer uw configuratiebestand diagnostiek en diagnostische gegevens opnieuw te installeren.
Als het probleem zich blijft voordoen na het opnieuw installeren van de uitbreiding van de diagnostische gegevens hebt u mogelijk fouten opsporen verder door te kijken via de monitoring agent fouten. Voordat de gebeurtenisgegevens is geüpload naar uw account opslag, wordt deze opgeslagen in de LocalResourceDirectory.

De LocalResourceDirectory is voor de rol van Cloud-Service:

    C:\Resources\Directory\<CloudServiceDeploymentID>.<RoleName>.DiagnosticStore\WAD<DiagnosticsMajorandMinorVersion>\Tables

De LocalResourceDirectory is voor virtuele Machines:

    C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD<DiagnosticsMajorandMinorVersion>\Tables

Als er geen bestanden in de map LocalResourceDirectory, is de monitoring agent kan niet worden gestart. Dit wordt meestal veroorzaakt door een ongeldig configuratiebestand, een gebeurtenis die moet worden vermeld in de CommandExecution.log.

Als de Monitoring Agent met succes van gebeurtenisgegevens verzamelen is ziet u de bestanden voor elke gebeurtenis die is gedefinieerd in het bestand .tsf. De Monitoring Agent worden de fouten in het bestand MaEventTable.tsf. Als u wilt controleren van de inhoud van dit bestand kunt u de toepassing van de tabel2csv het .tsf-bestand converteren naar een bestand met door komma's gescheiden values(.csv):

Op de rol van een Cloud-Service:

    %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

*% SystemDrive %* van de rol van een Cloud-Service is doorgaans D:

Op een virtuele Machine:

    C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

De bovenstaande opdrachten wordt de log-bestand *maeventtable.csv*, dat u kunt openen en controleren op mislukte berichten gegenereerd.    


## <a name="diagnostics-data-tables-not-found"></a>Diagnostische gegevens tabellen niet gevonden
De tabellen in Azure opslag die Azure diagnostische gegevens worden benoemd op basis van de volgende code:

        if (String.IsNullOrEmpty(eventDestination)) {
            if (e == "DefaultEvents")
                tableName = "WADDefault" + MD5(provider);
            else
                tableName = "WADEvent" + MD5(provider) + eventId;
        }
        else
            tableName = "WAD" + eventDestination;

Hier volgt een voorbeeld:

        <EtwEventSourceProviderConfiguration provider=”prov1”>
          <Event id=”1” />
          <Event id=”2” eventDestination=”dest1” />
          <DefaultEvents />
        </EtwEventSourceProviderConfiguration>
        <EtwEventSourceProviderConfiguration provider=”prov2”>
          <DefaultEvents eventDestination=”dest2” />
        </EtwEventSourceProviderConfiguration>

Die 4 tabellen wordt gegenereerd:

Gebeurtenis|Naam van de tabel
---|---
provider = 'prov1' &lt;gebeurtenis-id = "1" /&gt;|WADEvent + MD5("prov1") + "1"
provider = 'prov1' &lt;gebeurtenis-id = "2" eventDestination = "dest1" /&gt;|WADdest1
provider = 'prov1' &lt;DefaultEvents /&gt;|WADDefault+MD5("prov1")
provider = 'prov2' &lt;DefaultEvents eventDestination = "dest2" /&gt;|WADdest2
