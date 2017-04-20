<properties
   pageTitle="Problemen oplossen met functies die niet worden gestart | Microsoft Azure"
   description="Hier volgen enkele gangbare redenen waarom een rol Cloud-Service niet gestart. Oplossingen voor deze problemen zijn ook beschikbaar."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="troubleshoot-cloud-service-roles-that-fail-to-start"></a>Problemen met de Service Cloud-functies die niet worden gestart

Hier volgen enkele veelvoorkomende problemen en oplossingen met betrekking tot Azure Cloud Services rollen die niet worden gestart.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a>Ontbrekende dll's of afhankelijkheden

Niet-reagerende rollen en functies die tussen **Bezig met initialiseren**, **bezet**en **stoppen van** lidstaten zijn functioneert kunnen worden veroorzaakt door ontbrekende dll's of samenstellen.

Symptomen van ontbrekende dll-bestanden of verzamelingen zijn:

- Uw exemplaar van de rol is **Bezig met initialiseren**, **bezet**en lidstaten **stoppen** doorlopen.
- Uw exemplaar van de rol is verplaatst naar **Gereed** , maar als u naar uw webtoepassing navigeert, wordt de pagina wordt niet weergegeven.

Er zijn verschillende aanbevolen methoden voor het onderzoek naar deze problemen.

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a>Vaststellen van problemen met ontbrekende dll-bestand in een Webrol

Wanneer u navigeert naar een website die is geïmplementeerd in een web-rol en de browser een fout met de volgende strekking weergegeven, kan dit erop duiden dat een DLL-bestand ontbreekt.

![Serverfout in '/' Application.](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a>Vaststellen van problemen met het uitschakelen van aangepaste fouten

Gedetailleerde informatie over fouten kan worden bekeken door de service opnieuw te implementeren en configureren van het bestand web.config voor de Webrol ingesteld op de aangepaste fout uit.

Gedetailleerde fouten bekijken zonder het gebruik van extern bureaublad:

1. Open de oplossing in Microsoft Visual Studio.

2. Zoek het bestand web.config in de **Solution Explorer**en open het.

3. Ga naar de sectie system.web en voeg de volgende regel in het bestand web.config:

    ```xml
    <customErrors mode="Off" />
    ```

4. Sla het bestand.

5. Verpakken en implementeren van de service.

Zodra de service opnieuw is gedistribueerd, worden er een foutbericht weergegeven met de naam van de assembly of DLL-bestand ontbreekt.

## <a name="diagnose-issues-by-viewing-the-error-remotely"></a>Vaststellen van problemen met de fout op afstand bekijken

U kunt Extern bureaublad gebruiken voor toegang tot de rol en gedetailleerde foutgegevens extern weergeven. Gebruik de volgende stappen uit om de fouten weergeven met extern bureaublad:

1. Zorg ervoor dat de Azure SDK 1.3 of hoger is geïnstalleerd.

2. Tijdens de implementatie van de oplossing met behulp van Visual Studio "Configureren verbindingen met extern bureaublad..." kiezen. Voor meer informatie over het configureren van de verbinding met extern bureaublad Zie [Extern bureaublad met Azure rollen](../vs-azure-tools-remote-desktop-roles.md).

3. In de klassieke portal voor Microsoft Azure, zodra de instantie de status **Gereed geeft**, klikt u op een van de exemplaren van de rol.

4. Klik op het pictogram **verbinding maken** in het **RAS** -gedeelte van het lint.

5. Log in op de virtuele machine met behulp van de referenties die zijn opgegeven tijdens de configuratie van extern bureaublad.

6. Open een opdrachtvenster.

7. Type `IPconfig`.

8. Noteer de waarde van het IPv4-adres.

9. Open Internet Explorer.

10. Typ het adres en de naam van de webtoepassing. Bijvoorbeeld `http://<IPV4 Address>/default.aspx`.

Navigeren naar de website wordt nu meer expliciete geven foutberichten als resultaat:

* Serverfout in '/' Application.

* Beschrijving: Er is een onverwerkte uitzondering opgetreden tijdens de uitvoering van de huidige webaanvraag. Bekijk de stacktrace voor meer informatie over de fout en de oorsprong ervan in de code.

* Details van uitzondering: System.IO.FIleNotFoundException: kan bestand of assembly niet laden ' Microsoft.WindowsAzure.StorageClient, versie 1.1.0.0, Culture = neutral, PublicKeyToken = 31bf856ad364e35 =' of een van de afhankelijkheden. Het systeem kan het opgegeven bestand niet vinden.

Bijvoorbeeld:

![Expliciete Serverfout in toepassing '/ ''](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-the-compute-emulator"></a>Problemen opsporen met behulp van de compute-emulator

U kunt de Microsoft Azure compute-emulator opsporen en oplossen van problemen met ontbrekende afhankelijkheden en web.config fouten.

Gebruik voor de beste resultaten bij het gebruik van deze methode van de diagnose van een computer of virtuele machine met een schone installatie van Windows. Gebruiken om de beste simuleren de Azure-omgeving, Windows Server 2008 R2 x64.

1. De zelfstandige versie van de [Azure SDK](https://azure.microsoft.com/downloads/)installeren.

2. Bouw het project cloud-service op de ontwikkelcomputer.

3. Ga in Windows Verkenner naar de map bin\debug van het project cloud-service.

4. De .csx map- en .cscfg-bestand kopiëren naar de computer die u gebruikt voor het opsporen van de problemen.

5. Op de nieuwe computer, open een Azure SDK opdrachtpromptvenster en typ het `csrun.exe /devstore:start`.

6. Typ bij de opdrachtprompt `run csrun <path to .csx folder> <path to .cscfg file> /launchBrowser`.

7. Wanneer de rol wordt gestart, ziet u gedetailleerde gegevens over fouten in Internet Explorer. U kunt ook de standaard-Windows-hulpprogramma's voor probleemoplossing gebruiken voor verdere diagnose van het probleem.

## <a name="diagnose-issues-by-using-intellitrace"></a>Problemen opsporen met behulp van IntelliTrace

Voor de werknemer en web-functies die gebruikmaken van .NET Framework 4, kunt u [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx), die beschikbaar is in [Microsoft Visual Studio Ultimate](https://www.visualstudio.com/products/visual-studio-ultimate-with-MSDN-vs).

Ga als volgt te werk om de service te implementeren met IntelliTrace ingeschakeld:

1. Controleer of de Azure SDK 1.3 of hoger is geïnstalleerd.

2. De oplossing implementeren met behulp van Visual Studio. Tijdens de implementatie, het selectievakje **IntelliTrace inschakelen voor .NET 4 rollen** .

3. Wanneer de sessie wordt gestart, opent u de **Server Explorer**.

4. Vouw de **Azure\\Cloud Services** knooppunt en zoek de implementatie.

5. De implementatie uitbreiden totdat u de exemplaren van de rol. Klik met de rechtermuisknop op een van de exemplaren.

6. Kies **IntelliTrace weergave logboeken**. De **Samenvatting van de IntelliTrace** wordt geopend.

7. Ga naar de sectie uitzonderingen van de samenvatting. Als er uitzonderingen zijn, wordt de sectie label **Uitzonderingsgegevens**.

8. Vouw de **Uitzonderingsgegevens** en **System.IO.FileNotFoundException** fouten de volgende strekking:

![Uitzonderingsgegevens, ontbreekt een bestand of assembly](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a>Adres van ontbrekende dll's en verzamelingen

Ga als volgt te werk om de ontbrekende DLL en assembly fouten:

1. Open de oplossing in Visual Studio.

2. Open de map **References** in de **Solution Explorer**.

3. Klik op de algemene vergadering die in de fout.

4. In het deelvenster **Eigenschappen** Zoek **lokale kopie van de eigenschap** en de waarde ingesteld op **True**.

5. De cloud-service opnieuw.

Nadat u hebt gecontroleerd dat alle fouten hebt gecorrigeerd, kunt u de service implementeren zonder te controleren, het selectievakje **IntelliTrace inschakelen voor .NET 4 rollen** .

## <a name="next-steps"></a>Volgende stappen

Bekijk meer [artikelen probleemoplossing](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) voor cloud services.

Als u wilt weten hoe u problemen met cloud service rol met Azure PaaS computer diagnostische gegevens, Zie [blog-serie van Kevin Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
