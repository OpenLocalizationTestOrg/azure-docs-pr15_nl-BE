<properties
    pageTitle="Python web werknemer rollen en met Visual Studio | Microsoft Azure"
    description="Overzicht van het gebruik van Python Tools voor Visual Studio voor het maken van Azure cloud services, met inbegrip van web-rollen en functies van de werknemer."
    services="cloud-services"
    documentationCenter="python"
    authors="thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.date="08/03/2016"
    ms.author="adegeo"/>


# <a name="python-web-and-worker-roles-with-python-tools-for-visual-studio"></a>Python web werknemer rollen en met Python's voor Visual Studio

Dit artikel biedt een overzicht van het gebruik van Python web en werknemer rollen met [Python Tools for Visual Studio][]. Hier leert u hoe u met Visual Studio kunt maken en implementeren van een standaard Cloud Service die gebruikmaakt van Python.

## <a name="prerequisites"></a>Vereisten

 - Visual Studio 2013 of 2015
 - [Python-hulpprogramma's voor Visual Studio][] (PTVS)
 - [Azure SDK-hulpprogramma's voor VS 2013][] of [Azure SDK's voor VS 2015][]
 - [Python 2.7 32-bits][] of [32-bits 3.5 Python][]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-are-python-web-and-worker-roles"></a>Wat zijn Python web en werknemer rollen?

Azure biedt drie modellen voor het uitvoeren van toepassingen berekenen: [Web Apps-functie in Azure App Service][execution model-web sites], [Azure virtuele Machines][execution model-vms], en [Azure Cloud Services][execution model-cloud services]. Alle drie de modellen ondersteuning voor Python. Cloud-Services, waaronder web en werknemer rollen, bieden *Platform als Service (PaaS)*. In een cloud-service biedt een Webrol front-end web host voor toepassingen, een specifieke Internet Information Services (IIS)-webserver een rol werknemer asynchrone, langdurige of permanente taken onafhankelijk van de interactie van de gebruiker of de invoer kan worden uitgevoerd.

Zie voor meer informatie [Wat is een Cloud Service?].

> [AZURE.NOTE]*Looking een eenvoudige website bouwt?*
Als uw scenario slechts een eenvoudige website front-end omvat, kunt u de functie voor lichtgewicht Web Apps in Azure App-Service. U kunt eenvoudig upgraden naar een Cloud-Service terwijl uw website groeit en uw behoeften veranderen. Zie de <a href="/develop/python/">Python Developer Center</a> voor artikelen die betrekking hebben op de ontwikkeling van de functie Web Apps in Azure App-Service.
<br />


## <a name="project-creation"></a>Maken van het project

U kunt in Visual Studio **Azure Cloud Service** onder **Python**in het dialoogvenster **Nieuw Project** selecteren.

![Dialoogvenster New Project](./media/cloud-services-python-ptvs/new-project-cloud-service.png)

In de wizard Azure Cloud-Service, kunt u nieuwe rollen voor web- en werknemer.

![Dialoogvenster voor Azure Cloud-Service](./media/cloud-services-python-ptvs/new-service-wizard.png)

De rol van werknemer sjabloon wordt geleverd met generieke code voor verbinding met een Azure opslag of Azure Service Bus.

![Cloud Service-oplossing](./media/cloud-services-python-ptvs/worker.png)

U kunt web of de werknemer toevoegen aan een bestaande cloud-service op elk gewenst moment.  U kunt bestaande projecten toevoegen in uw oplossing, of een nieuwe maken.

![Opdracht rol toevoegen](./media/cloud-services-python-ptvs/add-new-or-existing-role.png)

Uw service cloud kan rollen geïmplementeerd in verschillende talen bevatten.  U kunt bijvoorbeeld een Python Webrol geïmplementeerd met behulp van Django, met Python, of C# werknemer rollen hebben.  U kunt gemakkelijk communiceren tussen uw Service Bus of opslag wachtrijen met rollen.

## <a name="install-python-on-the-cloud-service"></a>Python installeren op de cloud-service

>[AZURE.WARNING] De setup-scripts die zijn geïnstalleerd met Visual Studio (op het moment dat dit artikel voor het laatst werd bijgewerkt) werken niet. In deze sectie wordt een tijdelijke oplossing beschreven.

Het belangrijkste probleem met de setup-scripts zijn dat ze niet python installeert. Eerst twee [opstarttaken](cloud-services-startup-tasks.md) definiëren in het bestand [ServiceDefinition.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) . De eerste taak (**PrepPython.ps1**) downloadt en installeert de Python runtime. De tweede taak (**PipInstaller.ps1**) wordt uitgevoerd pip wilt installeren, moet u mogelijk afhankelijkheden.

De onderstaande scripts zijn geschreven voor Python 3.5. Als u wilt gebruikmaken van de versie 2.x van python, zodanig dat het bestand **PYTHON2** **op** voor de twee opstarttaken en de runtime-taak: `<Variable name="PYTHON2" value="<mark>on</mark>" />`.


```xml
<Startup>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
  </Task>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
    
  </Task>

</Startup>
```

De variabelen **PYTHON2** en **PYPATH** moet worden toegevoegd aan de taak is gestart. De variabele **PYPATH** wordt alleen gebruikt als de variabele **PYTHON2** ingesteld op **aan**.

```xml
<Runtime>
  <Environment>
    <Variable name="EMULATED">
      <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
    </Variable>
    <Variable name="PYTHON2" value="off" />
    <Variable name="PYPATH" value="%SystemDrive%\Python27" />
  </Environment>
  <EntryPoint>
    <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
  </EntryPoint>
</Runtime>
```

#### <a name="sample-servicedefinitioncsdef"></a>Voorbeeld ServiceDefinition.csdef

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="AzureCloudServicePython" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRole1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
      <Setting name="Python2" />
    </ConfigurationSettings>
    <Startup>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="EMULATED">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
        </Variable>
        <Variable name="PYTHON2" value="off" />
        <Variable name="PYPATH" value="%SystemDrive%\Python27" />
      </Environment>
      <EntryPoint>
        <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
      </EntryPoint>
    </Runtime>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
  </WorkerRole>
</ServiceDefinition>
```



Vervolgens maakt u de bestanden **PrepPython.ps1** en **PipInstaller.ps1** in de **. / bin** map van uw rol.

#### <a name="preppythonps1"></a>PrepPython.ps1

Dit script installeert python. Als de variabele **PYTHON2** -omgeving is ingesteld op **op** en vervolgens 2.7 Python zijn geïnstalleerd, anders Python 3.5 wordt geïnstalleerd.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if python is installed...$nl"
    if ($is_python2) {
        & "${env:SystemDrive}\Python27\python.exe"  -V | Out-Null
    }
    else {
        py -V | Out-Null
    }

    if (-not $?) {

        $url = "https://www.python.org/ftp/python/3.5.2/python-3.5.2-amd64.exe"
        $outFile = "${env:TEMP}\python-3.5.2-amd64.exe"

        if ($is_python2) {
            $url = "https://www.python.org/ftp/python/2.7.12/python-2.7.12.amd64.msi"
            $outFile = "${env:TEMP}\python-2.7.12.amd64.msi"
        }
        
        Write-Output "Not found, downloading $url to $outFile$nl"
        Invoke-WebRequest $url -OutFile $outFile
        Write-Output "Installing$nl"

        if ($is_python2) {
            Start-Process msiexec.exe -ArgumentList "/q", "/i", "$outFile", "ALLUSERS=1" -Wait
        }
        else {
            Start-Process "$outFile" -ArgumentList "/quiet", "InstallAllUsers=1" -Wait
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Already installed"
    }
}
```

#### <a name="pipinstallerps1"></a>PipInstaller.ps1

Dit script roept van pip en alle afhankelijkheden in het bestand **requirements.txt** geïnstalleerd. Als de variabele **PYTHON2** -omgeving is ingesteld op **op** en vervolgens Python 2.7 wordt gebruikt, anders Python 3.5 wordt gebruikt.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if requirements.txt exists$nl"
    if (Test-Path ..\requirements.txt) {
        Write-Output "Found. Processing pip$nl"

        if ($is_python2) {
            & "${env:SystemDrive}\Python27\python.exe" -m pip install -r ..\requirements.txt
        }
        else {
            py -m pip install -r ..\requirements.txt
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Not found$nl"
    }
}
```

#### <a name="modify-launchworkerps1"></a>LaunchWorker.ps1 wijzigen

>[AZURE.NOTE] **LauncherWorker.ps1** bestand is voor een project **de rol van de werknemer** vereist voor het uitvoeren van het opstartbestand. In project **Webrol** wordt het opstartbestand in plaats daarvan gedefinieerd in de Projecteigenschappen.

De **bin\LaunchWorker.ps1** is gemaakt om een groot aantal voorbereidende werk doen maar niet echt. De inhoud van dat bestand vervangen door het volgende script.

Dit script wordt het bestand **worker.py** van uw project python. Als de variabele **PYTHON2** -omgeving is ingesteld op **op** en vervolgens Python 2.7 wordt gebruikt, anders Python 3.5 wordt gebruikt.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated)
{
    Write-Output "Running worker.py$nl"

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
else
{
    Write-Output "Running (EMULATED) worker.py$nl"

    # Customize to your local dev environment

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
```

#### <a name="pscmd"></a>PS.cmd

Moeten de sjablonen voor Visual Studio hebt gemaakt een bestand **ps.cmd** in de **. / bin** map. Dit shellscript aanroept om de bovenstaande PowerShell wrapper scripts en biedt logboekregistratie op basis van de naam van de PowerShell-wrapper genoemd. Als dit bestand niet is gemaakt, is hier wat het moet zijn. 

```bat
@echo off

cd /D %~dp0

if not exist "%DiagnosticStore%\LogFiles" mkdir "%DiagnosticStore%\LogFiles"
%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe -ExecutionPolicy Unrestricted -File %* >> "%DiagnosticStore%\LogFiles\%~n1.txt" 2>> "%DiagnosticStore%\LogFiles\%~n1.err.txt"
```



## <a name="run-locally"></a>Lokaal uitvoeren

Als u uw service cloud project als het opstartproject en druk op F5, de cloud-service wordt uitgevoerd in de lokale emulator Azure.

Hoewel PTVS ondersteunt werkt starten in de emulator, foutopsporing (bijvoorbeeld onderbrekingspunten) niet.

Voor foutopsporing op uw web- en werknemer rollen, stelt u de functie van project als het project is gestart en fouten die in plaats daarvan.  U kunt ook meerdere startup projecten instellen.  De oplossing met de rechtermuisknop en selecteer **Opstarten projecten instellen**.

![De opstarteigenschappen Project oplossing](./media/cloud-services-python-ptvs/startup.png)

## <a name="publish-to-azure"></a>Publiceren naar Azure

Als u wilt publiceren, klik met de rechtermuisknop op het project van de cloud-service in de oplossing en selecteer vervolgens **publiceren**.

![Microsoft Azure publiceren inloggen](./media/cloud-services-python-ptvs/publish-sign-in.png)

Volg de wizard. Extern bureaublad inschakelen als u wilt. Extern bureaublad is handig als u nodig hebt voor foutopsporing van iets.

Wanneer u klaar bent met het configureren van instellingen, klikt u op **publiceren**.

Sommige voortgang wordt weergegeven in het uitvoervenster en vervolgens ziet u het venster Microsoft Azure activiteitenlogboek.

![Venster van Microsoft Azure activiteit logboek](./media/cloud-services-python-ptvs/publish-activity-log.png)

Implementatie duurt enkele minuten in beslag en vervolgens uw website en/of werknemer rollen wordt uitgevoerd op Azure!

### <a name="investigate-logs"></a>Onderzoeken van Logboeken

Nadat de virtuele machine van cloud-service wordt gestart en Python geïnstalleerd, kunt u de logboeken naar foutberichten zoeken bekijken. Deze logboekbestanden bevinden zich in de **C:\Resources\Directory\\\LogFiles {rol}** map. **PrepPython.err.txt** hebben ten minste één fout in vanaf wanneer het script probeert te detecteren als Python is geïnstalleerd en **PipInstaller.err.txt** een klacht over een verouderde versie van pip indienen.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de documentatie van de PTVS voor meer gedetailleerde informatie over het werken met het web en werknemer rollen in Python Tools voor Visual Studio:

- [Cloud serviceprojecten][]

Raadpleeg de volgende artikelen voor meer informatie over het gebruik van Azure services van uw web en werknemer rollen zoals het gebruik van Azure opslag- of -Service-Bus.

- [BLOB-Service][]
- [Tabel Service][]
- [Queue-Service][]
- [Service Bus wachtrijen][]
- [Service Bus onderwerpen][]


<!--Link references-->

[Wat is een Cloud-Service?]: cloud-services-choose-me.md
[execution model-web sites]: ../app-service-web/app-service-web-overview.md
[execution model-vms]: ../virtual-machines/virtual-machines-windows-about.md
[execution model-cloud services]: cloud-services-choose-me.md
[Python Developer Center]: /develop/python/

[BLOB-Service]: ../storage/storage-python-how-to-use-blob-storage.md
[Queue-Service]: ../storage/storage-python-how-to-use-queue-storage.md
[Tabel Service]: ../storage/storage-python-how-to-use-table-storage.md
[Service Bus wachtrijen]: ../service-bus-messaging/service-bus-python-how-to-use-queues.md
[Service Bus onderwerpen]: ../service-bus-messaging/service-bus-python-how-to-use-topics-subscriptions.md


<!--External Link references-->

[Python-hulpprogramma's voor Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs
[Cloud serviceprojecten]: http://go.microsoft.com/fwlink/?LinkId=624028
[Azure SDK-hulpprogramma's voor VS 2013]: http://go.microsoft.com/fwlink/?LinkId=323510
[Azure SDK-hulpprogramma's voor VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32-bits]: https://www.python.org/downloads/
[Python 3.5 32-bits]: https://www.python.org/downloads/
