<properties
   pageTitle="Met de Emulator Express uit te voeren en fouten opsporen in een cloud-service op een lokale computer | Microsoft Azure"
   description="Met behulp van Emulator Express uit te voeren en fouten opsporen in een cloud-service op een lokale computer"
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


# <a name="using-emulator-express-to-run-and-debug-a-cloud-service-on-a-local-machine"></a>Met behulp van Emulator Express uit te voeren en fouten opsporen in een cloud-service op een lokale computer

U kunt met Emulator Express, testen en fouten opsporen in een cloud-service zonder Visual Studio wordt uitgevoerd als een beheerder. U kunt instellen dat de projectinstellingen Emulator Express of de volledige emulator, afhankelijk van de vereisten van uw cloud-service te gebruiken. Zie [een toepassing Azure in de Emulator berekenen uitvoeren](./storage/storage-use-emulator.md)voor meer informatie over de volledige emulator. Emulator Express werd voor het eerst opgenomen in Azure SDK 2.1 en op Azure SDK 2.3, is de standaard-emulator.

## <a name="using-emulator-express-in-the-visual-studio-ide"></a>In de Visual Studio IDE met behulp van Emulator Express

Bij het maken van een nieuw project in Azure SDK 2.3 of hoger is Emulator Express al geselecteerd. Volg deze stappen Schakel Emulator Express voor bestaande projecten die zijn gemaakt met een eerdere versie van de SDK.

### <a name="to-configure-a-project-to-use-emulator-express"></a>Voor het configureren van een project voor het gebruik van het Emulator

1. Kies **Eigenschappen**in het snelmenu voor het project Azure, en kies vervolgens het tabblad **Web** .

1. Kies de knop **optie gebruikt IIS snel** onder **Lokale ontwikkelingsserver**. Emulator Express is niet compatibel met de IIS-webserver.

1. Kies onder **Emulator**, het keuzerondje **Gebruik Emulator Express** .

    ![Express emulator](./media/vs-azure-tools-emulator-express-debug-run/IC673363.gif)

## <a name="launching-emulator-express-at-a-command-prompt"></a>Emulator Express starten vanaf een opdrachtprompt

U kunt de express-versie van de Emulator bij het berekenen van Azure, csrun.exe, bij een opdrachtprompt starten met de optie /useemulatorexpress.

## <a name="limitations"></a>Beperkingen

Voordat u Express Emulator gebruikt, moet u rekening houden met enkele beperkingen zijn:

- Emulator Express is niet compatibel met de IIS-webserver.

- Uw cloud-service kan meerdere rollen bevatten, maar elke rol is beperkt tot één exemplaar.

- U heeft geen toegang tot minder dan 1000-poortnummers. Als u een verificatieprovider die gewoonlijk een poort minder dan 1000 gebruikt, wilt u mogelijk deze waarde wijzigen in een poortnummer dat hoger is dan 1000.

- Alle beperkingen die voor de Emulator Azure berekenen gelden gelden ook voor de Emulator Express. Bijvoorbeeld geen exemplaren van meer dan 50 rol per distributie. Zie [een toepassing Azure in de Compute-Emulator wordt uitgevoerd](http://go.microsoft.com/fwlink/p/?LinkId=623050)

## <a name="next-steps"></a>Volgende stappen

[Debugging Cloud Services](https://msdn.microsoft.com/library/azure/ee405479.aspx)
