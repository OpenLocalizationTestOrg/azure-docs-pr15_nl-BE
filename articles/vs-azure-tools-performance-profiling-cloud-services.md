<properties 
   pageTitle="Testen van de prestaties van een cloud service | Microsoft Azure"
   description="Test de werking van een cloud-service met behulp van de Visual Studio-profiler"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />


# <a name="testing-the-performance-of-a-cloud-service"></a>De prestaties van een cloud-service testen 

##<a name="overview"></a>Overzicht

U kunt de prestaties van een cloud-service op de volgende manieren testen:

- Azure diagnostische gegevens gebruiken voor het verzamelen van informatie over het aanvragen en verbindingen en site statistieken die laten zien hoe de service uitvoert vanuit een klantperspectief bekijken. Zie aan de slag met [Diagnostische gegevens configureren voor Azure Cloud Services en virtuele Machines]( http://go.microsoft.com/fwlink/p/?LinkId=623009).

- De Visual Studio-profiler gebruiken om een diepgaande analyse van de rekenkundige aspecten van de manier waarop de service wordt uitgevoerd. Zoals in dit onderwerp wordt beschreven, kunt u de profiler gebruiken om prestaties te meten als een service wordt uitgevoerd in Azure. Zie [de prestaties van een Azure Cloud Service lokaal in de Compute Emulator met behulp van de Visual Studio-Profiler testen](http://go.microsoft.com/fwlink/p/?LinkId=262845)voor meer informatie over het gebruik van de profiler om prestaties te meten als een service wordt lokaal uitgevoerd in een compute-emulator.



## <a name="choosing-a-performance-testing-method"></a>Kiezen van een methode voor prestatietest

###<a name="use-azure-diagnostics-to-collect"></a>Azure diagnostische gegevens gebruiken om te verzamelen:###

- Statistieken op webpagina's of services, zoals aanvragen en verbindingen.

- Statistieken op rollen, bijvoorbeeld hoe vaak een rol opnieuw wordt opgestart.

- Algemene informatie over het geheugengebruik, zoals het percentage van de tijd die de garbage collector nodig heeft of het geheugen instellen van een actieve rol.

###<a name="use-the-visual-studio-profiler-to"></a>De Visual Studio-profiler te gebruiken:###

- Bepalen welke functies de meeste tijd in beslag nemen.

- Meet hoe lang elk deel van een programma rekenkracht nodig.

- Gedetailleerde prestatierapporten voor twee versies van een service met elkaar vergelijken.

- Toewijzing van geheugen in meer detail dan het niveau van afzonderlijke geheugentoewijzingen analyseren.

- Gelijktijdigheidsproblemen in multithreaded code analyseren.

Wanneer u de profiler gebruikt, kunt u gegevens verzamelen wanneer een cloud-service wordt uitgevoerd als lokaal of Azure.

###<a name="collect-profiling-data-locally-to"></a>Lokaal profileren gegevens te verzamelen:###

- Test de prestaties van een deel van een cloud-service, zoals het uitvoeren van een bepaalde werknemer rol, die een realistische gesimuleerde werkdruk niet nodig.

- De prestaties van een cloud-service testen in een geïsoleerde omgeving onder gecontroleerde omstandigheden.

- De prestaties van een cloud-service testen voordat u het implementeert voor Azure.

- De prestaties van een cloud service privé, testen zonder de bestaande installaties te storen.

- De prestaties van de service testen zonder dat de kosten voor het uitvoeren van in Azure.

###<a name="collect-profiling-data-in-azure-to"></a>Profileren gegevens verzamelen in Azure aan:###

- De prestaties van een cloud-service onder een gesimuleerde of echte belasting testen.

- Gebruik de methode instrumentatie van het verzamelen van gegevens, profileren, zoals in dit onderwerp wordt later beschreven.

- Test de prestaties van de service in de hetzelfde als wanneer de service wordt uitgevoerd in de productie-omgeving.

U meestal simuleren een belasting met test cloud services onder normaal of stress omstandigheden.

## <a name="profiling-a-cloud-service-in-azure"></a>Een cloud-service in Azure profilering

Wanneer u uw cloud-service vanuit Visual Studio publiceert, kunt u de service profile en het profileren instellingen waarmee u de informatie die u wilt opgeven. Een sessie profileren is gestart voor elk exemplaar van een rol. Zie [publiceren naar een Azure Cloud Service vanuit Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)voor meer informatie over het publiceren van uw service vanuit Visual Studio.

Meer informatie over prestaties in Visual Studio profilering Zie [Beginners handleiding voor het profileren van prestaties](https://msdn.microsoft.com/library/azure/ms182372.aspx) en [Analyseren van de prestaties door profilering Tools gebruiken](https://msdn.microsoft.com/library/azure/z9z62c29.aspx).

>[AZURE.NOTE] U kunt IntelliTrace of profilering wanneer u uw cloud-service publiceert inschakelen. U kunt beide niet inschakelen.

###<a name="profiler-collection-methods"></a>Methoden van de collectie Profiler

U kunt verschillende methoden gebruiken voor profilering, op basis van uw prestaties:

- De **bemonstering van de CPU** - deze methode verzamelt-statistieken die handig voor de eerste analyse van de problemen met CPU-gebruik zijn. Bemonstering van de CPU is de aanbevolen methode voor het starten van de meeste onderzoeken van de prestaties. Er is een geringe invloed op de toepassing waarmee het profileren van wanneer u CPU bemonstering gegevens verzamelen.

- **Instrumentation** -deze methode verzamelt timinggegevens over gedetailleerde is handig voor het analyseren van i/o-prestaties en gerichte analyse. De methode instrumentation legt elke post, afsluiten en functie-aanroep van de functies in een module tijdens een beoordeling uitvoeren. Deze methode is handig voor het verzamelen van voor gedetailleerde timinginformatie over een gedeelte van de code en voor het effect van de invoer en uitvoer-bewerkingen op de prestaties van toepassingen. Deze methode is uitgeschakeld voor een computer met een 32-bits besturingssysteem. Deze optie is alleen beschikbaar als u de service cloud in Azure, niet lokaal in de compute-emulator uitvoeren.

- **Toewijzing van .NET-geheugen** - deze methode worden .NET Framework geheugen toewijzingsgegevens verzameld met behulp van de methode profilering bemonstering. De verzamelde gegevens omvatten het aantal en de grootte van objecten toegewezen.

- **Gelijktijdigheid** - deze methode verzamelt bronconflicten resourcegegevens en proces- en thread uitvoering van gegevens is nuttig in toepassingen met meerdere threads en met meerdere processen analyseren. De methode gelijktijdigheid verzamelt gegevens voor elke gebeurtenis die blokken uitvoering van de code, zoals wanneer een thread wacht op toegang tot een bron van toepassing te worden vrijgemaakt vergrendeld. Deze methode is handig voor het analyseren van multithread-toepassingen.

- Ook kunt u **Lagen interactie profilering**, waarmee u meer informatie over de uitvoering van tijden van synchrone ADO.NET roept in de functies van toepassingen met meerdere lagen die met een of meer databases communiceren. U kunt lagen interactie gegevens verzamelen met behulp van de methoden profileren. Zie voor meer informatie over het niveau interactie profilering [Laag Interacties weergeven](https://msdn.microsoft.com/library/azure/dd557764.aspx).

## <a name="configuring-profiling-settings"></a>Profileren-instellingen configureren

In de volgende afbeelding ziet u hoe uw profileren-instellingen van het dialoogvenster publiceren Azure toepassing configureren.

![Profilering-instellingen configureren](./media/vs-azure-tools-performance-profiling-cloud-services/IC526984.png)

>[AZURE.NOTE] Schakel het selectievakje **Enable profiling** door moet de profiler is geïnstalleerd op de lokale computer die u gebruikt voor het publiceren van uw cloud-service zijn. De profiler is standaard geïnstalleerd tijdens de installatie van Visual Studio.

### <a name="to-configure-profiling-settings"></a>Profileren-instellingen configureren

1. In de Solution Explorer, open het snelmenu voor het project Azure en kies vervolgens **publiceren**. Zie voor gedetailleerde stappen over het publiceren van een cloud service [publiceert een cloud service met behulp van de hulpprogramma's voor Azure](http://go.microsoft.com/fwlink/p?LinkId=623012).

1. In het dialoogvenster **Publiceren Azure toepassing** kiest het tabblad **Geavanceerde instellingen** .

1. Als u profielen, selecteert u het selectievakje **Enable profiling** .

1. Kies uw profileren om instellingen te configureren, de **Instellingen** hyperlink. In het dialoogvenster Instellingen voor profilering wordt weergegeven.

1. Kies het type van het profiel dat u nodig hebt van de keuzerondjes **van profilering methode u wilt gebruiken** .

1. Schakel het selectievakje **Profilering interactie met laag inschakelen** voor het verzamelen van de gegevens voor het profileren van laag-interactie.

1. Als u de instellingen wilt opslaan, klik op de knop **OK** .

    Als u deze toepassing publiceert, worden deze instellingen gebruikt voor het maken van de sessie profileren voor elke rol.

## <a name="viewing-profiling-reports"></a>Profilering van rapporten bekijken

Een sessie profileren is gemaakt voor elk exemplaar van een rol in de cloud-service. U kunt uw profileren om rapporten te bekijken van elke sessie van Visual Studio, de Server Explorer-venster weergeven en kies vervolgens het knooppunt Azure berekenen kunt u een exemplaar van een rol te selecteren. Vervolgens kunt u het rapport profileren bekijken, zoals wordt weergegeven in de volgende afbeelding.

![Rapport van Azure profilering weergeven](./media/vs-azure-tools-performance-profiling-cloud-services/IC748914.png)

### <a name="to-view-profiling-reports"></a>Profileren rapporten weergeven

1. Als de Server Explorer-venster weergeven in Visual Studio, in het menu weergave kiezen, Server Explorer.

1. Kies het knooppunt Azure berekenen en kies vervolgens het knooppunt Azure-implementatie voor het geselecteerde profiel wanneer u gepubliceerd vanuit Visual Studio service cloud.

1. Profileren om rapporten te bekijken voor een instantie, de rol in de service kiezen, opent u het snelmenu voor een specifieke instantie en kies vervolgens **Profilering rapport weergeven**.

    Het rapport, een .vsp-bestand wordt nu gedownload via Azure en de status van het downloaden wordt weergegeven in het logboek voor faxactiviteit Azure. Wanneer het downloaden is voltooid, het profileren rapport wordt weergegeven op een tabblad in de editor voor Visual Studio met de naam <Role name> _<Instance Number>_ <identifier>.vsp. Samenvattende gegevens voor het rapport wordt weergegeven.

1. Om verschillende weergaven van het rapport in de lijst huidige weergave, kiest u het type weergave dat u wilt. Voor meer informatie Zie [Extra rapportweergaven profilering](https://msdn.microsoft.com/library/azure/bb385755.aspx).

## <a name="next-steps"></a>Volgende stappen

[Debugging Cloud Services](https://msdn.microsoft.com/library/azure/ee405479.aspx)

[Publiceren naar een Azure Cloud-Service vanuit Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)

