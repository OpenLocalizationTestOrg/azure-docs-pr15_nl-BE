<properties
    pageTitle="De stroom in een Cloud Services-toepassing met Azure diagnostische gegevens traceren | Microsoft Azure"
    description="Berichten voor tracering voor een toepassing Azure te helpen bij het opsporen van fouten, het meten van prestaties, controle, analyse van het netwerkverkeer en meer toevoegen."
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/20/2016"
    ms.author="robb"/>



# <a name="trace-the-flow-of-a-cloud-services-application-with-azure-diagnostics"></a>De stroom van een Cloud Services-toepassing met Azure diagnostische gegevens traceren

Tracering is een manier om te controleren van de uitvoering van een toepassing terwijl deze wordt uitgevoerd. De klassen [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx)en [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) kunt u informatie over fouten en uitvoeren van toepassingen registreren in Logboeken, bestanden of andere apparaten voor latere analyse. Zie voor meer informatie over tracering, [tracering en implementeren van toepassingen](https://msdn.microsoft.com/library/zs6s4h68.aspx).


## <a name="use-trace-statements-and-trace-switches"></a>Gebruik de instructies trace en trace switches

Tracering in uw toepassing Cloud Services implementeren door de [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) toe te voegen aan de configuratie van toepassingen en aanroepen van System.Diagnostics.Trace of System.Diagnostics.Debug maken in uw toepassingscode. Gebruik het configuratie bestand *app.config* voor werknemer rollen en *web.config* voor web-rollen. Wanneer u een nieuwe gehoste service met behulp van een Visual Studio-sjabloon maakt, Azure diagnostische gegevens wordt automatisch toegevoegd aan het project en de DiagnosticMonitorTraceListener wordt toegevoegd aan het betreffende configuratiebestand voor de rollen die u wilt toevoegen.

Zie voor informatie over het plaatsen van trace-instructies, [hoe: Trace-instructies toevoegen aan Code van de toepassing](https://msdn.microsoft.com/library/zd83saa2.aspx).

[Trace-Switches](https://msdn.microsoft.com/library/3at424ac.aspx) in uw code plaatst, kunt u bepalen of het traceren plaatsvindt en hoe uitgebreid is. Hiermee kunt u de status van de toepassing in een productieomgeving. Dit is vooral van belang in een zakelijke toepassing die gebruikmaakt van meerdere onderdelen die worden uitgevoerd op meerdere computers. Zie voor meer informatie [procedure: traceerlogboeken Switches configureren](https://msdn.microsoft.com/library/t06xyy08.aspx).

## <a name="configure-the-trace-listener-in-an-azure-application"></a>De traceringslistener in een Azure toepassing configureren

Traceren, foutopsporing en TraceSource, moet u instellen "listeners" om te verzamelen en vastleggen van de berichten die worden verzonden. Listeners verzamelen, opslaan en doorsturen van berichten voor tracering. Ze verwijzen de tracering wordt uitgevoerd naar een juiste doel, zoals een logboek, venster of tekstbestand. Azure Diagnostics gebruikt de klasse [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) .

Voordat u de volgende procedure kunt voltooien, moet u de diagnostische Azure monitor te initialiseren. Zie hiervoor [Diagnostiek inschakelen in Microsoft Azure](cloud-services-dotnet-diagnostics.md).

Houd er rekening mee dat als u de sjablonen die worden geleverd door Visual Studio, de configuratie van de listener wordt automatisch toegevoegd voor u.


### <a name="add-a-trace-listener"></a>Een traceringslistener toevoegen

1. Open het bestand web.config of app.config voor uw rol.
2. De volgende code toevoegen aan het bestand. Wijzig het versie-kenmerk voor het gebruik van het versienummer van de assembly waarnaar u verwijst. De assembly-versie wijzigen niet noodzakelijkerwijs met elke versie van de SDK van Azure tenzij er updates.

    ```
    <system.diagnostics>
        <trace>
            <listeners>
                <add type="Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener,
                  Microsoft.WindowsAzure.Diagnostics,
                  Version=2.8.0.0,
                  Culture=neutral,
                  PublicKeyToken=31bf3856ad364e35"
                  name="AzureDiagnostics">
                  <filter type="" />
                </add>
            </listeners>
        </trace>
    </system.diagnostics>
    ```
    >[AZURE.IMPORTANT] Controleer of dat u hebt een projectverwijzing naar de assembly Microsoft.WindowsAzure.Diagnostics. Werk het versienummer in het bovenstaande XML-bestand overeenkomt met de versie van de Microsoft.WindowsAzure.Diagnostics-assembly waarnaar wordt verwezen.

3. Sla het configuratiebestand.

Zie voor meer informatie over listeners [Listeners traceren](https://msdn.microsoft.com/library/4y5y10s7.aspx).

Nadat u de stappen voor het toevoegen van de listener, kunt u de instructies trace toevoegen aan uw code.


### <a name="to-add-trace-statement-to-your-code"></a>Traceringsinstructie toevoegen aan uw code

1. Open een bronbestand voor uw toepassing. Bijvoorbeeld, de <RoleName>:. cs-bestand voor de rol van de werknemer of de rol van de webpagina.
2. Voeg de volgende instructie gebruiken als deze nog niet is toegevoegd:
    ```
        using System.Diagnostics;
    ```
3. Trace-instructies waar u informatie over de status van uw toepassing wilt toevoegen. U kunt tal van methoden voor het opmaken van de uitvoer van de instructie Trace. Zie voor meer informatie [hoe: Trace-instructies toevoegen aan Code van de toepassing](https://msdn.microsoft.com/library/zd83saa2.aspx).
4. Sla het bronbestand.
