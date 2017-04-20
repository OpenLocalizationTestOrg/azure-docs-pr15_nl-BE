<properties 
pageTitle="Opstarttaken uitvoeren in Azure Cloud Services | Microsoft Azure" 
description="Opstarttaken helpen bij het voorbereiden van uw cloud-omgeving voor uw app. Dit leert u hoe opstarttaken werken en hoe u ze" 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>



# <a name="how-to-configure-and-run-startup-tasks-for-a-cloud-service"></a>Het configureren en uitvoeren van taken voor een cloud-service starten

U kunt opstarttaken bewerkingen uit te voeren voordat u begint een rol. Bewerkingen die u wilt uitvoeren, zijn een onderdeel installeren, registreren van COM-onderdelen, registersleutels instellen of starten van een langdurige proces.

>[AZURE.NOTE] Opstarttaken zijn niet van toepassing op virtuele Machines, alleen op Cloud Service Web en rollen van de werknemer.

## <a name="how-startup-tasks-work"></a>De werking van start

Opstarttaken zijn acties die worden uitgevoerd voordat uw rollen beginnen en met behulp van het element [Task] in de [Startup] -element zijn gedefinieerd in het bestand [ServiceDefinition.csdef] . Vaak zijn opstarttaken batch-bestanden, maar ze kunnen ook worden consoletoepassingen of batchbestanden die PowerShell-scripts worden gestart.

Omgevingsvariabelen informatie doorgeven aan een taak is gestart en lokale opslag kan worden gebruikt voor het doorgeven van informatie van een taak is gestart. Bijvoorbeeld een omgevingsvariabele kunt het pad opgeven naar een programma dat u wilt installeren en bestanden kunnen worden geschreven naar de lokale opslag, die vervolgens door de rollen later kan worden gelezen.

Uw taak starten kunt vastleggen en -fouten naar de map die is opgegeven met de omgevingsvariabele **TEMP** . Tijdens de taak opstarten de omgevingsvariabele **TEMP** wordt omgezet in de *C:\\Resources\\tijdelijk\\[guid]. [ Rolnaam]\\RoleTemp* map bij het uitvoeren van de wolk.

Van opstarttaken kunnen ook worden uitgevoerd meerdere malen opnieuw wordt gestart. Bijvoorbeeld de taak gestart wordt uitgevoerd telkens wanneer die de rol wordt gerecycled en rol wordt gerecycled altijd eventueel opnieuw opstarten. Taken starten moeten worden geschreven op een manier waarmee ze verschillende keren zonder problemen te werken.

Opstarttaken moeten eindigen met een **errorlevel** of afsluitcode van nul voor de opstartprocedure te voltooien. Als een taak is gestart met een niet-nul **errorlevel**wordt beëindigd, wordt de rol wordt niet gestart.


## <a name="role-startup-order"></a>De opstartvolgorde rol

Hieronder vindt u de procedure voor het opstarten van rol in Azure:

1. De instantie is gemarkeerd als **starten** en ontvangt geen verkeer.

2. Alle opstarttaken worden uitgevoerd volgens hun **taskType** kenmerk.
    - **Eenvoudige** taken worden uitgevoerd, synchroon, één voor één.
    - De taken **en **achtergrond** ** worden gestart asynchroon, parallel aan de taak is gestart.  
       
    > [AZURE.WARNING] IIS kan niet volledig worden geconfigureerd tijdens de taak gestart tijdens het opstarten, zodat u specifieke gegevens zijn mogelijk niet beschikbaar. [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx)taken starten waarin specifieke gegevens moeten worden gebruikt.

3. De functie host wordt uitgevoerd en de site is gemaakt in IIS.

4. De methode [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) wordt aangeroepen.

5. De instantie is gemarkeerd als **Gereed** en verkeer wordt doorgestuurd naar het exemplaar.

6. De methode [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) wordt aangeroepen.


## <a name="example-of-a-startup-task"></a>Voorbeeld van een taak is gestart

Opstarttaken worden gedefinieerd in het bestand [ServiceDefinition.csdef] in het element **Task** . Het kenmerk **commandLine** geeft u de naam en de parameters van het batchbestand is gestart of het kenmerk **executionContext** console opdracht geeft de bevoegdheden van de taak is gestart en het kenmerk **taskType** geeft aan hoe de taak wordt uitgevoerd.

In dit voorbeeld wordt een variabele, **MyVersionNumber**, gemaakt voor de taak is gestart en ingesteld op de waarde "**1.0.0.0**".

**ServiceDefinition.csdef**:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
        <Environment>
            <Variable name="MyVersionNumber" value="1.0.0.0" />
        </Environment>
    </Task>
</Startup>
```

In het volgende voorbeeld wordt wordt het batchbestand **Startup.cmd** de regel 'de huidige versie is 1.0.0.0' naar het bestand StartupLog.txt in de map die is opgegeven met de omgevingsvariabele TEMP. De `EXIT /B 0` regel zorgt ervoor dat de taak is gestart met een **errorlevel** 0 eindigt.

```cmd
ECHO The current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [AZURE.NOTE] In Visual Studio de eigenschap **kopiëren naar de uitvoermap** voor het opstarten van batch-bestand moet worden ingesteld op **Altijd kopiëren** om ervoor te zorgen dat het batchbestand opstarten correct is geïmplementeerd in uw project op Azure (**approot\\bin** voor Web-rollen en **approot** voor werknemer rollen).

## <a name="description-of-task-attributes"></a>Beschrijving van de kenmerken van de taak

De volgende beschrijving van de kenmerken van het element **Task** in het bestand [ServiceDefinition.csdef] :

**commandLine** - Hiermee geeft u de opdrachtregel voor de taak is gestart:

- De opdracht, met optionele opdrachtregelparameters die de taak is gestart begint.
- Vaak is dit de bestandsnaam van een .cmd of .bat batchbestand.
- De taak is ten opzichte van de AppRoot\\Bin-map voor de implementatie. Omgevingsvariabelen zijn bij het bepalen van het pad en de bestandsnaam van de taak niet is uitgevouwen. Als uitbreiding van de omgeving vereist is, kunt u een kleine CMD-script waarmee de taak gestart wordt aangeroepen.
- Is een console-programma of een batchbestand waarmee een [PowerShell script](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task)wordt gestart.

**executionContext** - Hiermee wordt de bevoegdheid voor de taak is gestart. De bevoegdheden kan worden beperkt of verhoogde:

- **beperkte**  
De startup-taak wordt uitgevoerd met dezelfde rechten als de rol. Gebruikersrechten worden gebruikt als het kenmerk **executionContext** voor de [Runtime] -element ook **beperkt**is.

- **verhoogde**  
De startup-taak wordt uitgevoerd met beheerdersbevoegdheden. Hierdoor taken starten op programma's installeren, IIS-configuratie wijzigen, uitvoeren van wijzigingen in het register en andere taken administrator niveau zonder toename van de bevoegdheden van de rol zelf.  

> [AZURE.NOTE] Het machtigingsniveau van een taak starten hoeft niet hetzelfde als de rol zelf.

**taskType** - Hiermee geeft u de manier waarop een taak is gestart.

- **eenvoudige**  
Taken uitgevoerd synchroon, één voor één, in de volgorde in het bestand [ServiceDefinition.csdef] hebt opgegeven. Wanneer een **eenvoudige** opstarten taak met een **errorlevel** 0 eindigt, wordt de volgende **eenvoudige** opstarten taak wordt uitgevoerd. Als er geen meer **eenvoudige** opstarttaken uit te voeren, zal de rol zelf worden gestart.   

    > [AZURE.NOTE] Als het **eenvoudige** taak op een niet-nul **errorlevel eindigt**, worden de instantie geblokkeerd. Volgende **eenvoudige** opstarttaken en de rol van zelf, niet worden gestart.

    De opdracht uitvoeren om ervoor te zorgen dat het batchbestand met een **errorlevel** 0 eindigt, `EXIT /B 0` aan het einde van de batch-bestand wordt uitgevoerd.

- **achtergrond**  
Taken worden asynchroon uitgevoerd in samenspraak met het opstarten van de rol.

- **voorgrond**  
Taken worden asynchroon uitgevoerd in samenspraak met het opstarten van de rol. Het belangrijkste verschil tussen een **voorgrond** en een **achtergrond** taak is een **voorgrond** taak voorkomt u dat de rol van recycling of af te sluiten totdat de taak is beëindigd. De **achtergrond** taken beschikt niet over deze beperking.

## <a name="environment-variables"></a>Omgevingsvariabelen

Omgevingsvariabelen zijn een manier om informatie doorgeven aan een taak is gestart. U kunt bijvoorbeeld het pad naar een blob met een programma te installeren, of poortnummers die uw rol wilt gebruiken of instellingen van de functies van uw taak starten plaatsen.

Er zijn twee typen omgevingsvariabelen voor opstarttaken; statische omgevingsvariabelen en omgevingsvariabelen op basis van de leden van de klasse [RoleEnvironment] . Beide zijn in de [omgeving] -sectie van het bestand [ServiceDefinition.csdef] en gebruik de [variabele] element en het attribuut **name** .

Statische omgevingsvariabelen maakt gebruik van het kenmerk **value** van het [variabele] element. In het bovenstaande voorbeeld wordt de variabele **MyVersionNumber** met een statische waarde van '**1.0.0.0**' gemaakt. Een ander voorbeeld zou zijn voor het maken van een omgevingsvariabele **StagingOrProduction** die u handmatig op waarden van "**staging**" of "**productie**" instellen kunt opstarten met verschillende acties op basis van de waarde van de omgevingsvariabele **StagingOrProduction** uit te voeren.

Gebruik niet het kenmerk **value** van het [variabele] element omgevingsvariabelen op basis van de leden van de klasse RoleEnvironment. In plaats daarvan [RoleInstanceValue] onderliggend element met de juiste **XPath** waarde van het kenmerk worden gebruikt voor het maken van een omgevingsvariabele op basis van een bepaald element van de klasse [RoleEnvironment] . Waarden voor het kenmerk **XPath** voor toegang tot diverse [RoleEnvironment] waarden vindt u [hier](cloud-services-role-config-xpath.md).



Bijvoorbeeld, een omgevingsvariabele is '**true**' als de instantie wordt uitgevoerd in de compute-emulator en "**false**" Als u in de cloud maken, gebruiken de volgende elementen van de [variabele] en [RoleInstanceValue] :

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
    
            <!-- Create the environment variable that informs the startup task whether it is running
                in the Compute Emulator or in the cloud. "%ComputeEmulatorRunning%"=="true" when
                running in the Compute Emulator, "%ComputeEmulatorRunning%"=="false" when running
                in the cloud. -->
    
            <Variable name="ComputeEmulatorRunning">
                <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
            </Variable>
    
        </Environment>
    </Task>
</Startup>
```

## <a name="next-steps"></a>Volgende stappen
Informatie over het uitvoeren van sommige [opstarttaken](cloud-services-startup-tasks-common.md) met de Service Cloud.

[Pakket](cloud-services-model-and-package.md) uw Cloud-Service.  


[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[Taak]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Opstarten]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[Omgeving]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[Variabele]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx