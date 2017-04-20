<properties 
   pageTitle="Opsporen van fouten in een gepubliceerde cloud service met IntelliTrace en Visual Studio | Microsoft Azure"
   description="Opsporen van fouten in een gepubliceerde cloud service met IntelliTrace en Visual Studio"
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

# <a name="debugging-a-published-cloud-service-with-intellitrace-and-visual-studio"></a>Opsporen van fouten in een gepubliceerde cloud service met IntelliTrace en Visual Studio

##<a name="overview"></a>Overzicht

Met IntelliTrace, kunt u uitgebreide informatie over foutopsporing voor een rol exemplaar registreren wanneer deze wordt uitgevoerd in Azure. Als u zoeken naar de oorzaak van een probleem wilt, kunt u de logboeken IntelliTrace stap voor stap de code van Visual Studio alsof deze in Azure uitvoert. In feite IntelliTrace records de belangrijkste code worden uitgevoerd en de omgevingsgegevens wanneer uw Azure-toepassing wordt uitgevoerd als een service cloud in Azure en kunt u het afspelen van de opgenomen gegevens van Visual Studio. Als alternatief kunt u foutopsporing op afstand te koppelen aan een cloud-service die wordt uitgevoerd in Azure rechtstreeks. Zie [Foutopsporing Cloud Services](http://go.microsoft.com/fwlink/p/?LinkId=623041).

>[AZURE.IMPORTANT] IntelliTrace voor foutopsporing scenario's alleen is bedoeld en moet niet worden gebruikt voor een productie-implementatie.

>[AZURE.NOTE] U kunt IntelliTrace als u Visual Studio Enterprise geïnstalleerd hebt en uw doelen Azure toepassing .NET Framework 4 of hoger gebruiken. IntelliTrace verzamelt gegevens voor uw Azure rollen. De virtuele machines voor deze rollen uitvoeren altijd 64-bits besturingssystemen.

## <a name="to-configure-an-azure-application-for-intellitrace"></a>Een Azure-toepassing te configureren voor IntelliTrace

Als u IntelliTrace voor een toepassing Azure, moet u maken en publiceren van de toepassing van een project van Visual Studio Azure. Voordat u deze naar Azure publiceert, moet u de IntelliTrace configureren voor uw toepassing Azure. Als u uw toepassing te publiceren zonder IntelliTrace maar vervolgens besluit dat u wilt doen, moet u de toepassing vanuit Visual Studio opnieuw publiceren. Zie [een Cloud-Service met behulp van de hulpprogramma's voor Azure publiceren](http://go.microsoft.com/fwlink/p/?LinkId=623012)voor meer informatie.

1. Als u wilt uw Azure-toepassingen distribueren, Controleer of uw project build doelen zijn voor **Foutopsporing**.

1. Open het snelmenu voor het Azure project in de Solution Explorer en kies **publiceren**.
 
    De wizard Publiceren Azure-toepassing wordt weergegeven.

1. Schakel het selectievakje **IntelliTrace inschakelen** voor het verzamelen van Logboeken voor uw toepassing IntelliTrace wanneer deze wordt gepubliceerd in de cloud.

    >[AZURE.NOTE] U kunt IntelliTrace of profilering wanneer u uw Azure toepassing publiceert inschakelen. U kunt beide niet inschakelen.

1. De basisconfiguratie van de IntelliTrace aanpassen, kiest u de hyperlink **Instellingen** .

    Het dialoogvenster IntelliTrace wordt weergegeven, zoals in de volgende afbeelding. U kunt opgeven welke gebeurtenissen in het logboek, of de oproepgegevens te verzamelen, welke modules en processen voor het verzamelen van Logboeken voor en hoeveel ruimte toe te wijzen aan de opname. Zie [Foutopsporing met IntelliTrace](http://go.microsoft.com/fwlink/?LinkId=214468)voor meer informatie over IntelliTrace.

    ![VST_IntelliTraceSettings](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

Het logboek van de IntelliTrace is een circulair logboekbestand de maximale grootte die is opgegeven in de IntelliTrace instellingen (de standaardgrootte is 250 MB). IntelliTrace-logboeken zijn verzameld in een bestand in het bestandssysteem van de virtuele machine. Wanneer u de logboeken aanvraagt, is een momentopname op dat moment genomen en naar uw lokale computer gedownload.

Nadat de Azure toepassing naar Azure is gepubliceerd, kunt u bepalen als IntelliTrace is ingeschakeld in het knooppunt Azure berekenen in Server Explorer, zoals wordt weergegeven in de volgende afbeelding:

![VST_DeployComputeNode](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## <a name="downloading-intellitrace-logs-for-a-role-instance"></a>IntelliTrace Logs downloaden voor een exemplaar van de rol

Logboeken voor een exemplaar van de rol van IntelliTrace kunt u downloaden van het knooppunt **Cloud Services** in **Server Explorer**. Vouw het knooppunt **Services Cloud** totdat u het exemplaar dat u geïnteresseerd bent in vinden, opent u het snelmenu voor dit exemplaar en kies **IntelliTrace-logboeken weergeven**. De IntelliTrace-logboeken worden gedownload naar een bestand in een map op uw lokale computer. Elke keer dat u de IntelliTrace aanvragen meldt, een nieuwe momentopname wordt gemaakt.

Wanneer de logboeken worden gedownload, geeft Visual Studio de voortgang van de bewerking in het venster logboek voor faxactiviteit Azure. Zoals in de volgende afbeelding wordt weergegeven, kunt u het artikel voor meer details van de bewerking kunt uitbreiden.

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

U kunt verder werken in Visual Studio, terwijl de IntelliTrace zich aanmeldt worden gedownload. Wanneer u het logboek voor het downloaden is voltooid, wordt deze automatisch geopend in Visual Studio.

>[AZURE.NOTE] De logboeken van de IntelliTrace mogelijk uitzonderingen die in het kader wordt gegenereerd en vervolgens worden verwerkt. Intern kader code genereert deze uitzonderingen als een normaal onderdeel van het opstarten van een rol, zodat u ze gewoon kan negeren.

## <a name="see-also"></a>Zie ook

[Debugging Cloud Services](https://msdn.microsoft.com/library/ee405479.aspx)

