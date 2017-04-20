<properties
   pageTitle="Azure virtuele Machines openen vanuit Server Explorer | Microsoft Azure"
   description="Krijg een overzicht van het weergeven, maken en beheren van Azure virtual machines (VMs) in Server Explorer in Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="accessing-azure-virtual-machines-from-server-explorer"></a>Azure virtuele Machines openen vanuit Server Explorer

U kunt informatie weergeven over de virtuele machines die worden gehost op Azure via Server Explorer in Visual Studio.

## <a name="accessing-virtual-machines-in-server-explorer"></a>Toegang tot virtuele machines in Server Explorer

Als u virtuele machines die worden gehost op Azure, kunt u deze openen in Server Explorer. U moet eerst het aanmelden bij uw abonnement Azure om uw mobiele services weer te geven. Aanmelden, opent u het snelmenu voor het knooppunt Azure in Server Explorer en kies **verbinding maken met Microsoft Azure**.

### <a name="to-get-information-about-your-virtual-machines"></a>Informatie over de virtuele machines

1. In Server Explorer, kiest u een virtuele machine en kies vervolgens de toets F4 het eigenschappenvenster weer te geven.

    De volgende tabel ziet u welke eigenschappen beschikbaar zijn, maar ze zijn allemaal het kenmerk alleen-lezen. Om deze te wijzigen, de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885)te gebruiken.

  	|Eigenschap|Beschrijving|
  	|---|---|
  	|DNS-naam|De URL met de Internet-adres van de virtuele machine.|
  	|Omgeving|Voor een virtuele machine is de waarde van deze eigenschap altijd productie.|
  	|Naam|De naam van de virtuele machine.|
  	|Grootte|De grootte van de virtuele machine, dat overeenkomt met de hoeveelheid geheugen en schijfruimte ruimte die beschikbaar is. Zie procedures voor meer informatie: grootte van de virtuele Machine configureren.|
  	|Status|De waarden zijn starten, gestart, gestopt, gestopt en Status ophalen. Als het ophalen van de Status wordt weergegeven, is de huidige status onbekend. De waarden voor deze eigenschap afwijken van de waarden die worden gebruikt op de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885).|
  	|SubscriptionID|De abonnement-ID voor uw account Azure. Op de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885) kunt u deze gegevens weergeven door de eigenschappen voor een abonnement te bekijken.|

1. Kies een knooppunt eindpunt en bekijk het venster **Eigenschappen** .

1. De volgende tabel worden de beschikbare eigenschappen van eindpunten, maar ze zijn alleen-lezen. Als u wilt toevoegen of bewerken van de eindpunten voor een virtuele machine, de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885)te gebruiken. 

  	|Eigenschap|Beschrijving|
  	|---|---|
  	|Naam|Een id voor het eindpunt.|
  	|Particuliere poort|De poort voor toegang tot het netwerk binnen uw toepassing.|
  	|Protocol|Het protocol dat gebruikmaakt van de transport-laag voor dit eindpunt, TCP of UDP.|
  	|Openbare poort|De poort die wordt gebruikt voor toegang tot de toepassing.|

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van Azure rollen in Visual Studio, [Extern bureaublad met Azure rollen](vs-azure-tools-remote-desktop-roles.md).
