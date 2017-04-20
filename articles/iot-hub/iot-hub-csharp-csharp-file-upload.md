<properties
    pageTitle="Uploaden van bestanden van apparaten met IoT Hub | Microsoft Azure"
    description="Volg deze zelfstudie voor meer informatie over het uploaden van bestanden van apparaten met Azure IoT Hub met C#."
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/21/2016"
     ms.author="elioda"/>

# <a name="tutorial-how-to-upload-files-from-devices-to-the-cloud-with-iot-hub"></a>Zelfstudie: Het uploaden van bestanden van apparaten naar de cloud met IoT Hub

## <a name="introduction"></a>Inleiding

Azure IoT Hub is een volledig beheerde service waarmee betrouwbare en veilige bi-directionele communicatie tussen miljoenen IoT apparaten en een toepassing back-end. Vorige zelfstudies ([aan de slag met IoT Hub] en [wolk naar apparaat verzenden met IoT Hub]) illustreren de fundamentele apparaat voor cloud en wolk naar apparaat messaging functionaliteit van IoT Hub en de zelfstudie [proces apparaat naar Cloud berichten] een manier betrouwbaar apparaat-wolk-berichten opslaan in Azure blob-opslag beschreven. Echter, in sommige scenario's kan niet gemakkelijk koppelt u de gegevens die uw apparaten verzenden in een relatief klein apparaat naar cloud berichten IoT Hub accepteert. Voorbeelden zijn grote bestanden die afbeeldingen, video's, bemonsterd met hoge frequentie trillingen-gegevens bevatten, of die een vorm van Voorbewerkte gegevens bevatten. Deze bestanden zijn meestal batch verwerkt in de cloud met hulpmiddelen zoals [Azure Data Factory] of de stack [Hadoop] . Wanneer het bestand uploaden vanaf een apparaat hebben de voorkeur voor het verzenden van gebeurtenissen, kunt u functionaliteit voor IoT Hub beveiliging en betrouwbaarheid.

Deze zelfstudie is gebaseerd op de code van de [wolk naar apparaat verzenden met IoT Hub] zelfstudie aan hoe u de mogelijkheden voor het uploaden van bestand van IoT Hub gebruiken. U ziet hoe u veilig een apparaat met een Azure blob-URI voor het uploaden van een bestand en het gebruik van de kennisgevingen IoT Hub bestand uploaden voor het verwerken van het bestand in uw back-end van app starten.

Aan het einde van deze zelfstudie kunt u twee Windows consoletoepassingen uitvoeren:

* **SimulatedDevice**, een gewijzigde versie van de app in de zelfstudie [wolk naar apparaat verzenden met IoT Hub] die een bestand uploadt naar een opslag met een SAS-URI geleverd door de hub IoT gemaakt.
* **ReadFileUploadNotification**, die het bestand uploaden meldingen van uw hub IoT ontvangt.

> [AZURE.NOTE] IoT Hub ondersteunt veel apparaat platforms en talen (zoals C, Java en Javascript) via Azure IoT apparaat SDK's. Raadpleeg de [Azure IoT Developer Center] voor stap voor stap instructies over hoe u uw apparaat aansluit op de code die in deze zelfstudie wordt weergegeven en in het algemeen op Azure IoT-Hub.

Wilt u deze zelfstudie hebt voltooid, moet u het volgende:

+ Microsoft Visual Studio 2015,

+ Een actieve account Azure. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] binnen een paar minuten.)

## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Een account Azure opslag om IoT Hub te koppelen

Omdat het gesimuleerde apparaat een bestand naar een blob geüpload, moet u een [Opslag Azure] -account die is gekoppeld aan de IoT Hub hebben. Wanneer u een opslag Azure-account aan een IoT hub koppelen, kan de hub een URI SAS waarmee een apparaat veilig een bestand te uploaden naar een blob-container kunt genereren. De IoT Hub-service en de SDK's van apparaat coördinatie van het proces dat de URI SAS genereert en beschikbaar gemaakt voor een apparaat wordt gebruikt om een bestand te uploaden.

Volg de instructies in de [bestandsupload configureren met behulp van de portal Azure] [ lnk-configure-upload] een rekening Azure opslag op uw hub IoT koppelen.

## <a name="upload-a-file-from-a-simulated-device"></a>Uploaden van een bestand van een gesimuleerde apparaat.

In deze sectie kunt u de toepassing van het gesimuleerde apparaat wijzigt u in een [wolk naar apparaat verzenden met IoT Hub] cloud naar apparaat om berichten te ontvangen van de hub IoT gemaakt.

1. Klik met de rechtermuisknop op het **SimulatedDevice** -project in Visual Studio, klikt u op **toevoegen**en **Bestaande Item**klikt. Ga naar een afbeeldingsbestand en opnemen in uw project. In deze zelfstudie wordt ervan uitgegaan dat de afbeelding met de naam `image.jpg`.

2. Klik met de rechtermuisknop op de afbeelding en klik vervolgens op **Eigenschappen**. Zorg ervoor dat **kopiëren naar de uitvoermap** is ingesteld op **altijd kopiëren**.

    ![][1]

3. In het bestand **Program.cs** , moet u de volgende instructies toevoegen aan het begin van het bestand:

        using System.IO;

4. De volgende methode toevoegen aan de klasse van het **programma** :
         
        private static async void SendToBlobAsync()
        {
            string fileName = "image.jpg";
            Console.WriteLine("Uploading file: {0}", fileName);
            var watch = System.Diagnostics.Stopwatch.StartNew();

            using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
            {
                await deviceClient.UploadToBlobAsync(fileName, sourceData);
            }

            watch.Stop();
            Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
        }

    De `UploadToBlobAsync` methode wordt in de naam en de stroom bron van het bestand te uploaden en het uploaden naar de opslag worden verwerkt. De consoletoepassing wordt weergegeven de tijd die nodig is om het bestand te uploaden.

5. De volgende methode in de methode **Main** toevoegen na de `Console.ReadLine()` regel:

        SendToBlobAsync();

> [AZURE.NOTE] Deze zelfstudie implementeert voor eenvoud van mogelijk te houden, niet elk beleid opnieuw. In de productiecode te opnieuw beleid (zoals exponentiële backoff), zoals aangegeven in het MSDN-artikel [Tijdelijke fout afhandeling]nemen.

## <a name="receive-a-file-upload-notification"></a>Een bestand uploaden melding

In dit gedeelte vindt schrijven u een console-app voor Windows dat bestand uploaden meldingen van IoT Hub ontvangt.

1. Maak een nieuw Visual C# Windows-project met behulp van de **Console Application** projectsjabloon in de huidige Visual Studio-oplossing. Naam van het project **ReadFileUploadNotification**.

    ![Nieuw project in Visual Studio][2]

2. In de Solution Explorer met de rechtermuisknop op het **ReadFileUploadNotification** -project en klik vervolgens op **Beheren NuGet pakketten**.

    Het venster beheren NuGet pakketten worden weergegeven.

2. Zoeken naar `Microsoft.Azure.Devices`en klik op **installeren**, accepteer de gebruiksvoorwaarden. 

    Dit downloadt, installeert en een verwijzing toegevoegd naar de [Azure IoT - Service SDK NuGet package] in het **ReadFileUploadNotification** -project.

3. In het bestand **Program.cs** , moet u de volgende instructies toevoegen aan het begin van het bestand:

        using Microsoft.Azure.Devices;

4. De volgende velden toevoegen aan de klasse van het **programma** . Vervang de aanduidingswaarde van de tijdelijke met de verbindingsreeks IoT-hub uit [aan de slag met IoT Hub]:

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
        
5. De volgende methode toevoegen aan de klasse van het **programma** :
   
        private async static Task ReceiveFileUploadNotificationAsync()
        {
            var notificationReceiver = serviceClient.GetFileNotificationReceiver();

            Console.WriteLine("\nReceiving file upload notification from service");
            while (true)
            {
                var fileUploadNotification = await notificationReceiver.ReceiveAsync();
                if (fileUploadNotification == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received file upload noticiation: {0}", string.Join(", ", fileUploadNotification.BlobName));
                Console.ResetColor();

                await notificationReceiver.CompleteAsync(fileUploadNotification);
            }
        }

    Houd er rekening mee dat het patroon ontvangen dezelfde is als wolk naar apparaat om berichten te ontvangen van de app van het apparaat wordt gebruikt.

6. Ten slotte, voeg de volgende regels aan de **Main** -methode:

        Console.WriteLine("Receive file upload notifications\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        ReceiveFileUploadNotificationAsync().Wait();
        Console.ReadLine();

## <a name="run-the-applications"></a>De toepassingen worden uitgevoerd.

U bent nu gereed voor de toepassingen worden uitgevoerd.

1. Klik met de rechtermuisknop op uw oplossing in Visual Studio en projecten **Opstarten ingesteld**. Selecteer **meerdere projecten voor opstarten**en vervolgens de actie **Start** voor **ReadFileUploadNotification** en **SimulatedDevice**.

2. Druk op **F5**. Beide toepassingen te starten. Hier ziet u het uploaden is voltooid in een console app en de upload-melding wordt ontvangen door de andere console app. U kunt de [Azure portal] of Visual Studio Server Explorer controleren op de aanwezigheid van het geüploade bestand in uw account Azure opslag.

  ![][50]


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe het bestand uploaden-capaciteit van IoT Hub geüploade bestanden van apparaten te vereenvoudigen. U kunt verder verkennen IoT hub functies en scenario's met de volgende artikelen:

- [Een hub IoT maken via programmacode][lnk-create-hub]
- [Inleiding tot de C-SDK][lnk-c-sdk]
- [SDK's IoT Hub][lnk-sdks]

Om de mogelijkheden van IoT Hub verder verkennen, Zie:

- [Een apparaat met de SDK IoT Gateway simuleren][lnk-gateway]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/create-identity-csharp1.png

<!-- Links -->

[Azure portal]: https://portal.azure.com/

[Azure Data Factory]: https://azure.microsoft.com/documentation/services/data-factory/
[Hadoop]: https://azure.microsoft.com/documentation/services/hdinsight/

[Berichten met IoT Hub wolk naar apparaat verzenden]: iot-hub-csharp-csharp-c2d.md
[Verwerken berichten van apparaat naar Cloud]: iot-hub-csharp-csharp-process-d2c.md
[Aan de slag met IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot

[Fout met betrekking tot tijdelijke behandeling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure opslag]: ../storage/storage-create-storage-account.md#create-a-storage-account
[lnk-configure-upload]: iot-hub-configure-file-upload.md
[Azure IoT - Service SDK NuGet package]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md


