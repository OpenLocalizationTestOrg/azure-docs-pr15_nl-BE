<properties
   pageTitle="Een Project Azure Cloud-Service configureren met Visual Studio | Microsoft Azure"
   description="Informatie over het configureren van een Azure cloud service-project in Visual Studio, afhankelijk van uw vereisten voor dat project."
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

# <a name="configure-an-azure-cloud-service-project-with-visual-studio"></a>Een Project Azure Cloud-Service configureren met Visual Studio

U kunt een project Azure cloud-service, afhankelijk van uw vereisten voor dat project. U kunt eigenschappen instellen voor het project voor de volgende categorieën:

- **Een cloud service publiceren naar Azure**

  U kunt instellen dat een eigenschap om ervoor te zorgen dat een bestaande cloud-service geïmplementeerd op Azure niet per ongeluk is verwijderd.

- **Worden uitgevoerd of fouten opsporen in een cloud-service op de lokale computer**

  Kunt u een configuratie van de service te gebruiken en aangeven of u wilt starten de emulator Azure opslag.

- **Een cloud-service-pakket valideren wanneer deze wordt gemaakt**

  U kunt waarschuwingen als fouten behandelen zodat kunt u ervoor zorgen dat het pakket cloud service zonder problemen implementeren gaat. Hierdoor wordt uw wachttijd als u implementeren en vervolgens ontdekt dat een storing is opgetreden.

De volgende afbeelding ziet hoe u een andere configuratie wilt gebruiken bij het uitvoeren van of de cloud-service lokaal debuggen. U kunt een van de eigenschappen van het project die u nodig vanuit dit venster hebt instellen zoals weergegeven in de afbeelding.

![Configureer een Microsoft Azure Project](./media/vs-azure-tools-configuring-an-azure-project/IC713462.png)

## <a name="to-configure-an-azure-cloud-service-project"></a>Een project Azure cloud-service configureren

1. Als u wilt configureren een cloud service project in de **Solution Explorer**, open het snelmenu voor het project cloud service en kiest u **Eigenschappen**.

  Er verschijnt een pagina met de naam van het project cloud-service in de editor van Visual Studio.

1. Kies het tabblad **ontwikkeling** .

1. Kies **waar**om ervoor te zorgen dat u niet per ongeluk een bestaande installatie in Azure, in de opdrachtprompt voordat u verwijdert een bestaande lijst van de implementatie te verwijderen.

1. Als u de gewenste configuratie van de service de configuratie van de service die u wilt gebruiken wanneer u fouten opsporen in de cloud-service lokaal, in de lijst **Service** of uitvoert.

  >[AZURE.NOTE] Als u maken van de configuratie van de service wilt te gebruiken, raadpleegt u het: configuraties beheren en profielen. Als u wilt wijzigen van de serviceconfiguratie van een voor een rol, Zie [het configureren van de rollen voor een Azure cloud service met Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

1. Kies **waar**u start de emulator Azure opslag wanneer u fouten opsporen in de cloud-service lokaal, in de **opslag-emulator Azure Start**of uitvoert.

1. Om ervoor te zorgen dat u niet publiceren als pakket validatie Kies fouten, **waarschuwingen als fouten behandelen**, in **True**.

1. Kies **waar**om ervoor te zorgen dat uw Webrol dezelfde poort gebruikt telkens wanneer die het lokaal in IIS Express in **Gebruik web project poorten**wordt gestart. Naar een specifieke poort te gebruiken voor een bepaald webproject, opent u het snelmenu voor het webproject kiest u het tabblad **Eigenschappen** , klik op het tabblad **Web** en wijzig het poortnummer in de instelling van de **Url van Project** in de sectie **IIS Express** . Voer bijvoorbeeld `http://localhost:14020` als de URL van project.

1. Sla eventuele wijzigingen die u hebt aangebracht in de eigenschappen van de service cloud project op. Kies de knop **Opslaan** op de werkbalk.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het configureren van Azure cloud serviceprojecten in Visual Studio, [configureren uw Azure project met meerdere configuraties](vs-azure-tools-multiple-services-project-configurations.md).
