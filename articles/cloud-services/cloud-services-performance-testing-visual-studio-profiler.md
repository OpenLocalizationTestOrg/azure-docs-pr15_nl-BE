<properties 
    pageTitle="Profilering van een Cloud-Service lokaal in de Emulator Compute | Microsoft Azure" 
    services="cloud-services"
    description="Onderzoeken van prestatieproblemen in cloud-services met de Visual Studio-profiler" 
    documentationCenter=""
    authors="TomArcher" 
    manager="douge" 
    editor=""
    tags="" 
    />

<tags 
    ms.service="cloud-services" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="07/30/2016" 
    ms.author="tarcher"/>

# <a name="testing-the-performance-of-a-cloud-service-locally-in-the-azure-compute-emulator-using-the-visual-studio-profiler"></a>De prestaties van een Cloud-Service lokaal te testen in de Emulator Azure Compute is met behulp van de Visual Studio-Profiler

Tal van hulpprogramma's en technieken voor het testen van de prestaties van cloud-services beschikbaar zijn.
Als u een cloud-service naar Azure publiceert, kunt u Visual Studio profileren gegevens verzamelen en lokaal analyseren als beschreven in [een toepassing Azure profilering]hebt[1].
Ook kunt u diagnostische gegevens voor het bijhouden van een aantal prestatiemeters, zoals wordt beschreven in [werken met prestatiemeteritems in Azure][2].
U kunt ook het profiel van uw toepassing lokaal in de compute-emulator voordat u deze distribueert naar de cloud.

Dit artikel heeft betrekking op de CPU bemonsteringsmethode van profilering, die lokaal kan worden uitgevoerd in de emulator. CPU-sampling is een methode van het profiel dat is niet erg opdringerige. Op een daartoe aangewezen controle-interval neemt de profiler een momentopname van de stack met opgeroepen procedures. De gegevens worden verzameld over een periode van tijd en in een rapport wordt weergegeven. Deze methode van profilering vaak om aan te geven waar in een toepassing rekenkracht de meeste CPU werk wordt verricht.  Hierdoor kunt u zich richten op de 'hot 'pad waar uw toepassing de meeste tijd besteedt.



## <a name="1-configure-visual-studio-for-profiling"></a>1: Visual Studio voor profielen configureren

Er zijn eerst enkele Visual Studio-configuratieopties die nuttig zijn kunnen bij het profiel. Om zinvolle profileren-rapporten, moet u voor uw toepassing en ook symbolen voor systeembibliotheken symbolen (.pdb bestanden). U zult om ervoor te zorgen dat u verwijzen naar de servers beschikbaar symbool. U doet dit door in het menu **Extra** van Visual Studio, kiest u **Opties**en kies vervolgens **Foutopsporing**, vervolgens **symbolen**. Zorg ervoor dat Microsoft Symbol-Servers wordt vermeld onder **Bestandslocaties (.pdb) symbool**.  U kunt ook verwijzen naar http://referencesource.microsoft.com/symbols die mogelijk extra bestanden.

![Symboolopties][4]

Indien gewenst, kunt u de rapporten die de profiler wordt gegenereerd door gewoon mijn Code te vereenvoudigen. Met alleen mijn Code is ingeschakeld, zijn functie stacks met opgeroepen procedures vereenvoudigd zodat de gesprekken volledig interne bibliotheken en het .NET Framework worden verborgen in de rapporten. Kies **Opties**in het menu **Extra** . Vervolgens het knooppunt **Prestatiehulpprogramma's** en kiest u **Algemeen**. Schakel het selectievakje **inschakelen alleen mijn**code voor profiler rapporten.

![Alleen mijn Code opties][17]

U kunt deze instructies gebruiken met een bestaand project of een nieuw project.  Als u probeert de hieronder beschreven technieken een nieuw project maakt, kiest u een C# **Azure Cloud Service** project en selecteer een **Webrol** en een **Rol van de werknemer**.

![Azure Cloud Service Projectrollen][5]

Doeleinden, worden bijvoorbeeld code toevoegen aan het project dat kost veel tijd en toont enkele voor de hand liggende prestatieprobleem. Bijvoorbeeld de volgende code toevoegen aan een project van de rol van werknemer:

    public class Concatenator
    {
        public static string Concatenate(int number)
        {
            int count;
            string s = "";
            for (count = 0; count < number; count++)
            {
                s += "\n" + count.ToString();
            }
            return s;
        }
    }

Deze code aanroepen vanuit de methode RunAsync in de rol van de werknemer RoleEntryPoint afgeleide klasse. (Negeer de waarschuwing over de methode synchroon uitgevoerd.)

        private async Task RunAsync(CancellationToken cancellationToken)
        {
            // TODO: Replace the following with your own logic.
            while (!cancellationToken.IsCancellationRequested)
            {
                Trace.TraceInformation("Working");
                Concatenator.Concatenate(10000);
            }
        }

Bouwen en uitvoeren van uw cloud service lokaal zonder foutopsporing (Ctrl + F5) met de configuratieset oplossing te **brengen**. Dit zorgt ervoor dat alle bestanden en mappen worden gemaakt voor de toepassing lokaal uitgevoerd en zorgt ervoor dat alle emulators worden gestart. De gebruikersinterface voor het berekenen van Emulator starten vanaf de taakbalk om te controleren of de rol van de werknemer wordt uitgevoerd.

## <a name="2-attach-to-a-process"></a>2: koppelen aan een proces

In plaats van de toepassing vanaf de Visual Studio 2010 IDE profilering, moet u de profiler koppelen aan een lopend proces. 

Om de profiler te koppelen aan een proces, in het menu **analyseren** **Profiler** en **Koppelen/ontkoppelen**te kiezen.

![De optie profiel koppelen][6]

Voor de rol van een werknemer, zoek het proces WaWorkerHost.exe.

![WaWorkerHost proces][7]

Als uw projectmap zich op een netwerkstation, de profiler wordt u gevraagd om een andere locatie op de rapporten profileren.

 U kunt ook koppelen aan een web-functie door te koppelen aan WaIISHost.exe.
Als er meerdere werkprocessen rol in uw toepassing, moet u de proces-id gebruiken om ze te onderscheiden. U kunt via programmacode de proces-id opvragen door de toegang tot het object proces. Bijvoorbeeld, als u deze code met de methode Run van de klasse die is afgeleid van RoleEntryPoint in een rol toevoegt, kunt u bekijken het logboek in de gebruikersinterface van de Emulator berekenen om te weten welk proces verbinding maken met.

    var process = System.Diagnostics.Process.GetCurrentProcess();
    var message = String.Format("Process ID: {0}", process.Id);
    Trace.WriteLine(message, "Information");

Als u het logboek bekijkt, start de Emulator berekenen UI.

![Start de Emulator Compute UI][8]

Open het venster werknemer rol logboek console in de gebruikersinterface van de Emulator berekenen door te klikken op de titelbalk van het consolevenster. Hier ziet u de proces-ID in het logboek.

![Proces-ID weergeven][9]

Een u gekoppeld hebt, voer de stappen uit in de gebruikersinterface van de toepassing (indien nodig) om het scenario te reproduceren.

Als u stoppen met profilering wilt, kiest u de koppeling **Profilering stoppen** .

![Stop de optie profilering][10]

## <a name="3-view-performance-reports"></a>3: van prestatierapporten weergeven

Het prestatierapport voor uw toepassing wordt weergegeven.

Op dit moment de profiler gestopt, worden gegevens opgeslagen in een bestand .vsp en een rapport met een analyse van deze gegevens wordt weergegeven.

![Profiler-rapport][11]


Als u String.wstrcpy in het hete pad ziet, klik op alleen mijn Code te wijzigen van de weergave alleen gebruikerscode.  Als u String.Concat, drukt u op de knop alle Code weergeven.

U ziet de tekst methode en de String.Concat een groot deel van de tijd in beslag nemen.

![Analyse van het rapport][12]

Als u de tekenreeks aaneenschakelen code in dit artikel hebt toegevoegd, ziet u een waarschuwing in de takenlijst voor deze. Ook wordt er een waarschuwing dat er een buitensporige garbagecollection, is door het aantal tekenreeksen die worden gemaakt en verwijderd is.

![Waarschuwingen voor prestaties][14]

## <a name="4-make-changes-and-compare-performance"></a>4: wijzigingen aanbrengen en het vergelijken van prestaties

U kunt ook de prestaties voor en na een codewijziging vergelijken.  Stop het proces en bewerken van de code te vervangen door de tekenreeks aaneenschakelen bewerking met behulp van een StringBuilder:

    public static string Concatenate(int number)
    {
        int count;
        System.Text.StringBuilder builder = new System.Text.StringBuilder("");
        for (count = 0; count < number; count++)
        {
             builder.Append("\n" + count.ToString());
        }
        return builder.ToString();
    }

Handelingen uitvoeren met een andere prestaties en vergelijk de prestaties. In de Explorer prestaties als het wordt uitgevoerd in dezelfde sessie, kunt u alleen beide rapporten selecteren, opent u het snelmenu en kies **Prestatierapporten vergelijken**. Als u vergelijken met een groep in een andere prestatiesessie wilt, opent u het menu **analyseren** en **Vergelijken prestatierapporten**kiezen. Geef beide bestanden in het dialoogvenster dat wordt weergegeven.

![De optie rapporten prestaties vergelijken][15]

De rapporten verschillen tussen de twee reeksen.

![Vergelijkend rapport][16]

Gefeliciteerd! U hebt verbeterd gestart met de profiler.

## <a name="troubleshooting"></a>Het oplossen van problemen

- Zorg ervoor dat u een Release-build zijn profilering en starten zonder foutopsporing.

- Als de optie Bijsluiten/loskoppelen niet in het menu Profiler is ingeschakeld, voert u de Wizard prestatie.

- Gebruik de Emulator berekenen gebruikersinterface om de status van de toepassing weer te geven. 

- Als u problemen ondervindt bij het starten van toepassingen in de emulator of de profiler, koppelen sluit naar beneden de compute-emulator en het programma opnieuw. Als dit het probleem niet oplost, probeert u opnieuw op te starten. Dit probleem kan optreden als u de Emulator berekenen schorsen en verwijderen van actieve implementaties.

- Als u hebt gebruikt de profileren opdrachten vanaf de opdrachtregel, met name de globale instellingen zorg VSPerfClrEnv /globaloff is aangeroepen en dat VsPerfMon.exe is afgesloten.

- Als de bemonstering, ziet u het bericht ' PRF0025: geen gegevens zijn verzameld, ' controleren of het proces dat u hebt gekoppeld aan de activiteit CPU heeft. Toepassingen die geen rekenkundige werk doen kunnen niet alle gegevens voor de bemonstering opleveren.  Het is ook mogelijk dat het proces is afgesloten voordat de bemonstering werd gedaan. Controleer de methode Run voor een rol die u aan het profiel worden niet beëindigd.

## <a name="next-steps"></a>Volgende stappen

Opzetten van Azure binaire bestanden in de emulator wordt niet ondersteund in de Visual Studio-profiler, maar als u wilt dat de toewijzing van geheugen te testen, kunt u die optie als profilering. U kunt ook gelijktijdigheid profilering, die helpt u bij het bepalen of threads tijdverspilling concurreren voor sloten of laag interactie profilering, die helpt bij het opsporen van prestaties wanneer de interactie tussen de lagen van een toepassing, meest gebruikt tussen de gegevenslaag en de rol van een werknemer.  U kunt de databasequery's die uw toepassing genereert weergeven en profileren gegevens gebruiken voor het verbeteren van uw gebruik van de database. Zie voor informatie over het niveau interactie profilering, de blog boeken [scenario: met de laag interactie Profiler in Visual Studio Team System 2010][3].



[1]: http://msdn.microsoft.com/library/azure/hh369930.aspx
[2]: http://msdn.microsoft.com/library/azure/hh411542.aspx
[3]: http://blogs.msdn.com/b/habibh/archive/2009/06/30/walkthrough-using-the-tier-interaction-profiler-in-visual-studio-team-system-2010.aspx
[4]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally09.png
[5]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally10.png
[6]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally02.png
[7]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally05.png
[8]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally010.png
[9]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally07.png
[10]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally06.png
[11]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally03.png
[12]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally011.png
[14]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally04.png 
[15]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally013.png
[16]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally012.png
[17]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally08.png
 