<properties
   pageTitle="Het beheren van configuraties en profielen | Microsoft Azure"
   description="Leren werken met configuratiebestanden en -profielen service | welke instellingen voor de werkomgevingen opslaan en publiceren van instellingen voor cloud services."
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

# <a name="how-to-manage-service-configurations-and-profiles"></a>Het beheren van configuraties en profielen

## <a name="overview"></a>Overzicht

Wanneer u een cloud-service publiceert, Visual Studio worden configuratiegegevens opgeslagen in twee typen configuratiebestanden: service en -profielen. Configuraties (.cscfg-bestanden) opslaan-instellingen voor de werkomgevingen voor een Azure cloud-service. Azure wordt deze configuratiebestanden gebruikt bij het beheren van uw cloud-services. Aan de andere kant profielen (.azurePubxml-bestanden) opslaan in publicatie-instellingen voor cloud services. Deze instellingen zijn vastgelegd wat u kiest wanneer u de wizard Publiceren en lokaal worden gebruikt door Visual Studio. Dit onderwerp wordt uitgelegd hoe u kunt werken met beide typen configuratiebestanden.

## <a name="service-configurations"></a>Configuraties

U kunt meerdere configuraties te gebruiken voor elk van uw implementatieomgevingen. U kunt bijvoorbeeld een serviceconfiguratie voor de lokale omgeving die u kunt uitvoeren en testen van een toepassing Azure en configuratie van een andere service voor uw productieomgeving maken.

U kunt toevoegen, verwijderen en de namen van deze configuraties op basis van uw behoeften aanpassen. Zoals in de volgende afbeelding wordt weergegeven, kunt u deze configuraties beheren vanuit Visual Studio.

![Configuraties beheren](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-service-config.png)

U kunt ook het dialoogvenster **Configuraties beheren** openen vanaf de pagina's van de rol. Als de eigenschappen voor een rol in het project Azure, opent u het snelmenu voor die rol en kies vervolgens **Eigenschappen**. Vouw de lijst van de **Configuratie van de Service** op het tabblad **Instellingen** en selecteer vervolgens **beheren** te openen in het dialoogvenster **Configuraties beheren** .

### <a name="to-add-a-service-configuration"></a>Configuratie van een service toevoegen

1. Open het snelmenu voor het project Azure in Solution Explorer en selecteer **Configuraties beheren**.

    Het dialoogvenster **Configuraties beheren** wordt weergegeven.

1. Als u wilt toevoegen in de configuratie van een service, moet u een kopie van een bestaande configuratie. Kies hiervoor de configuratie die u wilt kopiëren uit de lijst en selecteer vervolgens **kopie maken**.

1. (Optioneel) Configuratie van de service een andere naam geven, kiest u nieuwe configuratie van de service in de lijst naam en selecteer **naam wijzigen**. Typ in het vak **naam** de naam die u wilt gebruiken voor de configuratie van de service en klik vervolgens op **OK**.

    Een nieuwe service configuratiebestand met de naam ServiceConfiguration. [Naam van de nieuwe] .cscfg wordt toegevoegd aan het project Azure in de Solution Explorer.


### <a name="to-delete-a-service-configuration"></a>Configuratie van de service verwijderen

1. In de Solution Explorer, open het snelmenu voor het project Azure en selecteer **Configuraties beheren**.

    Het dialoogvenster **Configuraties beheren** wordt weergegeven.

1. Een als serviceconfiguratie wilt verwijderen, kiest u de configuratie die u wilt verwijderen uit **de lijst** en selecteer vervolgens **verwijderen**. Er verschijnt een dialoogvenster om te controleren dat u wilt verwijderen van deze configuratie.

1. Selecteer **verwijderen**.

     Het configuratiebestand van de service wordt verwijderd uit de Azure project in de Solution Explorer.


### <a name="to-rename-a-service-configuration"></a>Configuratie van een service wijzigen

1. Open het snelmenu voor het project Azure in Solution Explorer en selecteer **Configuraties beheren**.

    Het dialoogvenster **Configuraties beheren** wordt weergegeven.

1. Configuratie van de nieuwe service kiezen in de lijst **naam** wilt wijzigen van de configuratie van een service, en selecteer **naam wijzigen**. Typ de naam die u wilt gebruiken voor de configuratie van de service in het tekstvak **naam** en klik vervolgens op **OK**.

    De naam van het bestand van de configuratie wordt gewijzigd in het Azure project in de Solution Explorer.

### <a name="to-change-a-service-configuration"></a>De serviceconfiguratie van een wijzigen

- Als u wilt om de serviceconfiguratie van een te wijzigen, opent u het snelmenu voor de specifieke rol die u wilt wijzigen in het project Azure en selecteer vervolgens **Eigenschappen**. Zie [hoe: de rollen configureren voor een Azure Cloud Service met Visual Studio](https://msdn.microsoft.com/library/azure/hh369931.aspx) voor meer informatie.

## <a name="make-different-setting-combinations-by-using-profiles"></a>Verschillende combinaties maken met behulp van profielen

Een profiel gebruikt, kunt u automatisch ingevuld in de **Wizard publiceren** met verschillende combinaties van instellingen voor verschillende doeleinden. Bijvoorbeeld, kunt u een profiel voor foutopsporing hebt en een andere voor vrijgave maakt. In dat geval wordt uw profiel **Debug** zou hebben **IntelliTrace** ingeschakeld en de **Debug** -configuratie hebt geselecteerd en uw profiel **Release** zou hebben **IntelliTrace** uitgeschakeld en de **Release** -configuratie hebt geselecteerd. Ook kunt u verschillende profielen gebruiken voor de implementatie van een service met een andere opslag-account.

Wanneer u de wizard voor het eerst uitvoert, wordt een standaardprofiel gemaakt. Visual Studio wordt het profiel opgeslagen in een bestand met de extensie .azurePubXml, die wordt toegevoegd aan het project Azure onder de map **profielen** . Als u verschillende opties wanneer u de wizard later handmatig opgeeft, wordt het bestand automatisch bijgewerkt. Voordat u de volgende procedure uitvoert, moet u al hebt gepubliceerd uw cloud-service ten minste één keer.

### <a name="to-add-a-profile"></a>Een profiel toevoegen

1. Open het snelmenu voor het project Azure en selecteer vervolgens **publiceren**.

1. Selecteer de knop **Profiel opslaan** als de volgende afbeelding ziet u naast de lijst **doelprofiel** . Hiermee maakt u een profiel voor u.

    ![Een nieuw profiel maken](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/create-new-profile.png)

1. Nadat het profiel is gemaakt, selecteert u in de lijst **doelprofiel** **< beheren >** .

    Het dialoogvenster **Profielen beheren** wordt weergegeven, zoals de volgende afbeelding wordt getoond.

    ![Dialoogvenster profielen beheren](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-profiles.png)

1. Kies een profiel en selecteer **Kopie maken**in de lijst **naam** .

1. Kies de knop **sluiten** .

    Het nieuwe profiel wordt weergegeven in de profielenlijst.

1. Selecteer het profiel dat u zojuist hebt gemaakt in de lijst **doelprofiel** . De instellingen van de Wizard publiceren worden gevuld met de opties in het geselecteerde profiel.

1. Selecteer de knoppen **vorige** en **volgende** om weer te geven van elke pagina van de Wizard publiceren en pas vervolgens de instellingen voor dit profiel. Zie [Azure Application Wizard publiceren](http://go.microsoft.com/fwlink/p/?LinkID=623085) voor meer informatie.

1. Nadat u de instellingen aanpassen, selecteert u **volgende** om terug te gaan naar de pagina instellingen. Het profiel wordt opgeslagen wanneer u de service publiceert met behulp van deze instellingen, of als u **Opslaan** naast de lijst met profielen kiest.

### <a name="to-rename-or-delete-a-profile"></a>Hernoemen of verwijderen van een profiel

1. Open het snelmenu voor het project Azure en selecteer vervolgens **publiceren**.

1. Selecteer in de lijst **doelprofiel** **beheren**.

1. Selecteer het profiel dat u wilt verwijderen en selecteer **verwijderen**in het dialoogvenster **Profielen beheren** .

1. Klik in het dialoogvenster bevestiging op **OK**.

1. Selecteer **sluiten**.

### <a name="to-change-a-profile"></a>Een profiel wijzigen

1. Open het snelmenu voor het project Azure en selecteer vervolgens **publiceren**.

1. Selecteer het profiel dat u wilt wijzigen in de lijst **doelprofiel** .

1. Selecteert u de knoppen **vorige** en **volgende** op elke pagina van de Wizard publiceren weergeven en wijzig de gewenste instellingen. Zie [Azure Application Wizard publiceren](http://go.microsoft.com/fwlink/p/?LinkID=623085) voor meer informatie.

1. Nadat u de instellingen wijzigen, selecteert u **volgende** om terug te gaan naar de pagina **Instellingen** .

1. (Optioneel) Selecteer **publiceren** voor het publiceren van de cloud-service met behulp van de nieuwe instellingen. Als u voor het publiceren van uw cloud-service op dit moment niet wilt, sluit u de Wizard publiceren, Visual Studio wordt u gevraagd of u de wijzigingen opslaan in het profiel wilt maken.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het configureren van andere delen van uw Azure project van Visual Studio, Zie [een Project Azure configureren](http://go.microsoft.com/fwlink/p/?LinkID=623075)
