<properties
   pageTitle="Beheren van uw toepassingen in Visual Studio | Microsoft Azure"
   description="Met Visual Studio kunt maken, ontwikkelen, inpakken, implementeren en fouten opsporen in uw Fabric Service toepassingen en services."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="seanmck;mikhegn"/>

# <a name="use-visual-studio-to-simplify-writing-and-managing-your-service-fabric-applications"></a>Visual Studio gebruiken om te schrijven en beheren van uw Fabric Service toepassingen vereenvoudigen

U kunt uw Azure Service Fabric toepassingen en services door Visual Studio kunt beheren. Nadat u [ingesteld in uw ontwikkelomgeving hebt](service-fabric-get-started.md), kunt u Visual Studio Service Fabric-toepassingen maken, diensten of pakket, register toevoegen en distribueren van toepassingen in het cluster voor plaatselijke ontwikkeling.

## <a name="deploy-your-service-fabric-application"></a>Uw Service Fabric-toepassingen distribueren

Standaard combineert een toepassing implementeert de volgende stappen uit in één bewerking:

1. Het toepassingspakket maken
2. Het toepassingspakket uploaden naar de afbeelding opslaan
3. Het toepassingstype registreren
4. Als u een actieve toepassingsexemplaren
5. U een nieuw toepassingsexemplaar

In Visual Studio wordt op **F5** te drukken ook uw toepassing te implementeren en de debugger koppelen aan alle toepassingsexemplaren. U kunt **Ctrl + F5** om een toepassing te implementeren zonder foutopsporing of u kunt publiceren naar een lokale of externe cluster met behulp van het publicatieprofiel. Zie [publiceren een toepassing naar een externe cluster met behulp van Visual Studio](service-fabric-publish-app-remote-cluster.md)voor meer informatie.

### <a name="application-debug-mode"></a>Foutopsporingsmodus toepassing

Visual Studio verwijdert bestaande exemplaren van het toepassingstype wanneer u stopt met het opsporen van fouten of (als u de app gebruikt zonder het foutopsporingsprogramma te koppelen), wanneer u de toepassing opnieuw distribueren. In dat geval de gegevens van de toepassing verwijderd. Bij het lokaal debuggen, kunt u de gegevens die u al hebt gemaakt bij het testen van een nieuwe versie van de toepassing, u wilt bewaren van de actieve toepassing of het gewenste foutopsporing voor latere sessies de toepassing bijwerken. Visual Studio Service Fabric Tools bieden een eigenschap met de naam van de **Toepassing foutopsporingsmodus**, welke besturingselementen of verwijder de **F5** de toepassing, de toepassing die wordt uitgevoerd houden nadat de debug-sessie wordt beëindigd of de toepassing op volgende sessies opsporen, bijgewerkt in plaats van verwijderd en opnieuw inschakelen.

#### <a name="to-set-the-application-debug-mode-property"></a>De eigenschap Application foutopsporingsmodus instellen

1. Kies **Eigenschappen** in het snelmenu van het application-project, (of druk op **F4** ).
2. Stel de eigenschap **Debug Mode van toepassing** in het venster **Eigenschappen** .

    ![Eigenschap toepassing Debug-modus instellen][debugmodeproperty]

Dit zijn de beschikbare opties voor **Toepassing foutopsporingsmodus** .

1. **Auto-Upgrade**: de aanvraag blijft actief wanneer de debug-sessie wordt beëindigd. De volgende **F5** wordt behandeld de installatie een upgrade met behulp van niet-beheerde automatische modus snel de toepassing naar een nieuwere versie bijwerken met een datumreeks die wordt toegevoegd. Het upgradeproces behoudt alle gegevens die u hebt ingevoerd in een vorige sessie voor foutopsporing.

2. **Bewaren-toepassing**: de toepassing in het cluster wordt uitgevoerd wanneer de debug-sessie wordt beëindigd. Op de volgende **F5** de toepassing wordt verwijderd en de nieuw gebouwde toepassing aan het cluster wordt geïmplementeerd.

3. **Toepassing verwijderen** zorgt ervoor dat de toepassing wordt verwijderd wanneer de debug-sessie wordt beëindigd.

Voor de **Auto-Upgrade** gegevens behouden blijft door toepassing van de toepassing upgrade mogelijkheden van weefsel-Service, maar het optimaliseren voor prestaties in plaats van veiligheid is afgestemd. Zie [Service Fabric toepassing upgraden](service-fabric-application-upgrade.md)voor meer informatie over het upgraden van toepassingen en hoe u een upgrade kunt uitvoeren in een echte omgeving.

![Voorbeeld van de nieuwe versie van de toepassing met datum toegevoegd][preservedata]

>[AZURE.NOTE] Deze eigenschap bestaat niet voorafgaand aan versie 1.1 van de Service Fabric-hulpprogramma's voor Visual Studio. Gebruik de eigenschap **Data behouden op Start** te bereiken hetzelfde gedrag voor 1.1. De optie 'Toepassing houden' werd geïntroduceerd in versie 1.2 van de Service Fabric-hulpprogramma's voor Visual Studio.

## <a name="add-a-service-to-your-service-fabric-application"></a>Een service toevoegen aan de Fabric Service-toepassing

U kunt nieuwe services toevoegen aan uw aanvraag om uitbreiding van de functionaliteit.  Om ervoor te zorgen dat de service wordt opgenomen in het pakket van uw toepassing, de service toevoegen via de menuopdracht **... Nieuwe Fabric-Service** .

![Een nieuwe fabric-service aan uw toepassing toevoegen][newservice]

Selecteer een type Service Fabric project toe te voegen aan uw toepassing en geef een naam voor de service.  Zie [het kiezen van een kader voor uw service](service-fabric-choose-framework.md) kunt u bepalen welk servicetype te gebruiken.

![Selecteer een type Fabric Service project toe te voegen aan uw toepassing][addserviceproject]

De nieuwe service wordt toegevoegd aan de oplossing en de bestaande toepassingspakket. De Serviceverwijzingen en een service-exemplaar wordt toegevoegd aan het manifest van de toepassing. De service wordt gemaakt en gestart de volgende keer dat u de toepassing implementeert.

![De nieuwe service wordt toegevoegd aan het manifest van de toepassing][newserviceapplicationmanifest]

## <a name="package-your-service-fabric-application"></a>Uw toepassing Fabric Service pakket

Voor de implementatie van de toepassing en de services ervan aan een cluster, moet u een toepassingspakket maken.  Het pakket organiseert het manifest van de toepassing, service, manifest(s) en andere benodigde bestanden in een specifieke indeling.  Visual Studio instellen en beheren van het pakket in de map application-project, in de map 'pak'.  **Pakket** te klikken in het contextmenu van de **toepassing** maakt of het toepassingspakket bijgewerkt.  U kunt dit doen als u de toepassing implementeren met behulp van aangepaste PowerShell scripts.

## <a name="remove-applications-and-application-types-using-cloud-explorer"></a>Verwijderen van toepassingen en toepassingstypen die met Cloud Explorer

U kunt bewerkingen basic cluster beheer van binnen Visual Studio met Cloud Explorer, die u vanuit het menu **Beeld starten kunt** . U kunt bijvoorbeeld verwijderen van toepassingen en toepassingstypen op lokale of externe clusters inrichting.

![Een toepassing verwijderen](./media/service-fabric-manage-application-in-visual-studio/removeapplication.png)

>[AZURE.TIP] Zie voor uitgebreidere functionaliteit cluster, [cluster Service Fabric Explorer visualiseren](service-fabric-visualizing-your-cluster.md).


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

- [Het toepassingsmodel Fabric-service](service-fabric-application-model.md)
- [Toepassingsimplementatie Fabric-service](service-fabric-deploy-remove-applications.md)
- [Toepassingsparameters voor omgevingen met meerdere beheren](service-fabric-manage-multiple-environment-app-configuration.md)
- [Foutopsporing in uw toepassing Service Fabric](service-fabric-debugging-your-application.md)
- [Het cluster visualiseren met behulp van Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[preservedata]:./media/service-fabric-manage-application-in-visual-studio/preservedata.png
[debugmodeproperty]:./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png
