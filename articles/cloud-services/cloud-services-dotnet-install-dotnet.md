<properties
   pageTitle=".NET installeren op een Cloud Service rol | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe u handmatig installeren van .NET framework op de Cloud Service Web en rollen van de werknemer"
   services="cloud-services"
   documentationCenter=".net"
   authors="thraka"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/10/2016"
   ms.author="adegeo"/>

# <a name="install-net-on-a-cloud-service-role"></a>.NET op de rol van een Cloud-Service installeren 

Dit artikel wordt beschreven hoe u .NET framework installeert op Cloud Service Web en rollen van de werknemer. U kunt deze stappen 4.6.1 .NET installeren op de Azure Gast OS familie 4. Zie voor de meest recente informatie over de Gast OS releases [Azure Gast OS Releases en SDK compatibiliteit Matrix](cloud-services-guestos-update-matrix.md).

De .NET installeren op uw web- en werknemer rollen moet eerst worden met inbegrip van het installatiepakket .NET als onderdeel van het Project wolk en het installatieprogramma starten als onderdeel van de taken van de rol van opstarten.  

## <a name="add-the-net-installer-to-your-project"></a>Het installatieprogramma van .NET aan uw project toevoegen
- Download de de web installer voor .NET framework installeren
    - [4.6.1 .NET web-Installer](http://go.microsoft.com/fwlink/?LinkId=671729)
- Voor de rol van een webpagina
  1. In de **Solution Explorer**onder In **rollen** in het project cloud service Klik met de rechtermuisknop op de rol en selecteert u **toevoegen > nieuwe map**. Maak een map met de naam *opslaglocatie*
  2. Klik met de rechtermuisknop op de map **bin** en selecteert u **toevoegen > bestaande**. Selecteer de installer voor .NET en toe te voegen aan de map bin.
- Voor de rol van een werknemer
  1. Klik met de rechtermuisknop op de rol en selecteert u **toevoegen > bestaande**. Selecteer de installer voor .NET en toe te voegen aan de rol. 

Bestanden toevoegen op deze manier naar de inhoudsmap rol toegevoegd aan het pakket cloud service en worden geïmplementeerd op een consistente locatie op de virtuele machine. Herhaal dit proces voor alle web- en werknemer rollen in de Cloud-Service, zodat alle functies een exemplaar van het installatieprogramma zijn.

> [AZURE.NOTE] Zelfs als uw toepassing .NET 4.6 is gericht, moet u .NET 4.6.1 op uw rol Cloud-Service installeren. Het Gastbesturingssysteem Azure bevat updates, [3098779](https://support.microsoft.com/kb/3098779) en [3097997](https://support.microsoft.com/kb/3097997). .NET 4.6 installeren op deze updates kan problemen veroorzaken bij het uitvoeren van uw .NET-toepassingen, zodat u direct .NET 4.6.1 in plaats van .NET 4.6 moet installeren. Zie [KB 3118750](https://support.microsoft.com/kb/3118750)voor meer informatie.

![Functie-inhoud met de installer-bestanden][1]

## <a name="define-startup-tasks-for-your-roles"></a>Taken starten voor de rollen definiëren
Van opstarttaken kunt u bewerkingen uit te voeren voordat u begint een rol. Installatie van .NET Framework als onderdeel van de taak starten zal ervoor zorgen dat het framework is geïnstalleerd voordat u een van de code van de toepassing wordt uitgevoerd. Zie voor meer informatie over het starten van taken: [Opstarttaken uitvoeren in Azure](cloud-services-startup-tasks.md). 

1. Voeg het volgende toe aan het bestand *ServiceDefinition.csdef* onder het knooppunt **WebRole** of **WorkerRole** voor alle rollen:
    
    ```xml
    <LocalResources>
      <LocalStorage name="NETFXInstall" sizeInMB="1024" cleanOnRoleRecycle="false" />
    </LocalResources>    
    <Startup>
      <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
        <Environment>
          <Variable name="PathToNETFXInstall">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='NETFXInstall']/@path" />
          </Variable>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
    ```

    De bovenstaande configuratie wordt de console opdracht *install.cmd* uitgevoerd met beheerdersbevoegdheden zodat het .NET framework kunt installeren. De configuratie wordt ook een LocalStorage gemaakt met de naam *NETFXInstall*. De map temp deze bron lokale opslag gebruiken, zodat het installatieprogramma van .NET framework worden gedownload en geïnstalleerd vanaf deze bron wordt ingesteld op het opstartscript. Het is belangrijk dat u de grootte van deze bron ingesteld op minimaal 1024MB om ervoor te zorgen dat het kader correct wordt geïnstalleerd. Zie voor meer informatie over het starten van taken: [gemeenschappelijk Cloud Service opstarttaken](cloud-services-startup-tasks-common.md) 

2. Een bestand **install.cmd** maken en toe te voegen aan alle rollen door rechts klikken op de rol en selecteren **toevoegen > bestaand Item...**. Zodat alle rollen hebt nu het installatiebestand .NET zoals het install.cmd bestand.
    
    ![Functie-inhoud met alle bestanden][2]

    > [AZURE.NOTE] Een eenvoudige teksteditor zoals Kladblok gebruiken om dit bestand te maken. Als u met Visual Studio Maak een tekstbestand en vervolgens de naam met 'cmd' het bestand bevat mogelijk nog een UTF-8-Byte Order Mark en de eerste regel van het script wordt uitgevoerd, resulteert in een fout. Als u Visual Studio gebruiken voor het maken van het bestand laat voegt een REM (Remark) naar de eerste regel van het bestand wordt genegeerd bij het uitvoeren. 

3. Het volgende script naar de **install.cmd** bestand toevoegen:

    ```
    REM Set the value of netfx to install appropriate .NET Framework. 
    REM ***** To install .NET 4.5.2 set the variable netfx to "NDP452" *****
    REM ***** To install .NET 4.6 set the variable netfx to "NDP46" *****
    REM ***** To install .NET 4.6.1 set the variable netfx to "NDP461" *****
    REM ***** To install .NET 4.6.2 set the variable netfx to "NDP462" *****
    set netfx="NDP461"
    
    REM ***** Set script start timestamp *****
    set timehour=%time:~0,2%
    set timestamp=%date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2%
    set "log=install.cmd started %timestamp%."
    
    REM ***** Exit script if running in Emulator *****
    if %ComputeEmulatorRunning%=="true" goto exit
    
    REM ***** Needed to correctly install .NET 4.6.1, otherwise you may see an out of disk space error *****
    set TMP=%PathToNETFXInstall%
    set TEMP=%PathToNETFXInstall%
    
    REM ***** Setup .NET filenames and registry keys *****
    if %netfx%=="NDP462" goto NDP462
    if %netfx%=="NDP461" goto NDP461
    if %netfx%=="NDP46" goto NDP46
        set "netfxinstallfile=NDP452-KB2901954-Web.exe"
        set netfxregkey="0x5cbf5"
        goto logtimestamp
    
    :NDP46
    set "netfxinstallfile=NDP46-KB3045560-Web.exe"
    set netfxregkey="0x6004f"
    goto logtimestamp
    
    :NDP461
    set "netfxinstallfile=NDP461-KB3102438-Web.exe"
    set netfxregkey="0x6040e"
    goto logtimestamp
    
    :NDP462
    set "netfxinstallfile=NDP462-KB3151802-Web.exe"
    set netfxregkey="0x60632"
    
    :logtimestamp
    REM ***** Setup LogFile with timestamp *****
    md "%PathToNETFXInstall%\log"
    set startuptasklog="%PathToNETFXInstall%log\startuptasklog-%timestamp%.txt"
    set netfxinstallerlog="%PathToNETFXInstall%log\NetFXInstallerLog-%timestamp%"
    echo %log% >> %startuptasklog%
    echo Logfile generated at: %startuptasklog% >> %startuptasklog%
    echo TMP set to: %TMP% >> %startuptasklog%
    echo TEMP set to: %TEMP% >> %startuptasklog%
    
    REM ***** Check if .NET is installed *****
    echo Checking if .NET (%netfx%) is installed >> %startuptasklog%
    set /A netfxregkeydecimal=%netfxregkey%
    set foundkey=0
    FOR /F "usebackq skip=2 tokens=1,2*" %%A in (`reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release 2^>nul`) do @set /A foundkey=%%C
    echo Minimum required key: %netfxregkeydecimal% -- found key: %foundkey% >> %startuptasklog%
    if %foundkey% GEQ %netfxregkeydecimal% goto installed
    
    REM ***** Installing .NET *****
    echo Installing .NET with commandline: start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog%  /chainingpackage "CloudService Startup Task" >> %startuptasklog%
    start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog% /chainingpackage "CloudService Startup Task" >> %startuptasklog% 2>>&1
    if %ERRORLEVEL%== 0 goto installed
        echo .NET installer exited with code %ERRORLEVEL% >> %startuptasklog%   
        if %ERRORLEVEL%== 3010 goto restart
        if %ERRORLEVEL%== 1641 goto restart
        echo .NET (%netfx%) install failed with Error Code %ERRORLEVEL%. Further logs can be found in %netfxinstallerlog% >> %startuptasklog%
    
    :restart
    echo Restarting to complete .NET (%netfx%) installation >> %startuptasklog%
    EXIT /B %ERRORLEVEL%
    
    :installed
    echo .NET (%netfx%) is installed >> %startuptasklog%
    
    :end
    echo install.cmd completed: %date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2% >> %startuptasklog%
    
    :exit
    EXIT /B 0
    ```
        
    Het script voor installatie wordt gecontroleerd of de opgegeven versie van .NET framework al is geïnstalleerd op de computer via query's in het register. Als de versie van .NET niet is geïnstalleerd en vervolgens .net Web Installer is gestart. Om te helpen bij het oplossen van problemen wordt het script in een bestand met de naam *startuptasklog-(currentdatetime) .txt* opgeslagen in lokale opslag *InstallLogs* alle activiteiten vastgelegd.

    > [AZURE.NOTE] Het script nog steeds wordt beschreven hoe u .NET 4.5.2 of .NET 4.6 installeren voor continuïteit. Er is niet nodig als deze al beschikbaar op het Gastbesturingssysteem Azure is .NET 4.5.2 handmatig installeren. In plaats van .NET 4.6 installeren moet u .NET 4.6.1 door [KB 3118750](https://support.microsoft.com/kb/3118750)direct te installeren.
      

## <a name="configure-diagnostics-to-transfer-the-startup-task-logs-to-blob-storage"></a>Diagnostische functies voor het overbrengen van de logboeken van de taak is gestart om blob-opslag configureren 
Ter vereenvoudiging van het oplossen van problemen met de installatie kunt u Azure diagnostische functies voor het overbrengen van de logboekbestanden die zijn gegenereerd door het opstartscript of door het installatieprogramma van .NET naar een blobopslag. Met deze methode kunt u de logboeken weergeven door gewoon downloaden van de logboekbestanden van blob-opslag plaats moeten extern bureaublad aan de rol.

Open diagnostische gegevens van de *diagnostics.wadcfgx* configureren en voeg de volgende onder het knooppunt **mappen** : 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

Dit wordt azure diagnostische functies voor alle bestanden in de map *logboek* onder de resource *NETFXInstall* transfer naar de rekening van de diagnostische gegevens opslag in de container *netfx installeren* blob geconfigureerd.

## <a name="deploying-your-service"></a>Uw service implementeren 
Wanneer u uw service implementeren moeten opstarttaken uitvoeren en het .NET framework te installeren als dit nog niet is geïnstalleerd. De rollen worden in de status bezet terwijl het framework wordt geïnstalleerd en kan zelfs opnieuw wordt opgestart als de installatie van framework dat vereist. 

## <a name="additional-resources"></a>Aanvullende bronnen

- [.NET Framework installeren][]
- [Procedure: bepalen welke versies van .NET Framework zijn geïnstalleerd.][]
- [Problemen met de installatie van .NET Framework][]

[Procedure: bepalen welke versies van .NET Framework zijn geïnstalleerd.]: https://msdn.microsoft.com/library/hh925568.aspx
[.NET Framework installeren]: https://msdn.microsoft.com/library/5a4x27ek.aspx
[Problemen met de installatie van .NET Framework]: https://msdn.microsoft.com/library/hh925569.aspx

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png

 
