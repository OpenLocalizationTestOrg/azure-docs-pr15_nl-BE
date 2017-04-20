<properties 
    pageTitle="Het gebruik van opslag van Ruby wachtrij | Microsoft Azure" 
    description="Informatie over het gebruik van de service Azure wachtrij maken en verwijderen van wachtrijen en invoegen, ophalen en verwijderen van berichten. De voorbeelden geschreven in Ruby." 
    services="storage" 
    documentationCenter="ruby" 
    authors="robinsh" 
    manager="carmonm" 
    editor=""/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ruby" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="robinsh"/>


# <a name="how-to-use-queue-storage-from-ruby"></a>Het gebruik van opslag van Ruby wachtrij

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Overzicht

Deze handleiding wordt beschreven hoe u veelvoorkomende scenario's voor het gebruik van de Microsoft Azure Queue Storage-service uitvoeren. De monsters worden geschreven met de API voor Ruby Azure.
De scenario's die zijn **ingevoegd**, **inspecteren**, **ophalen**en **verwijderen van** berichten, alsmede **maken en verwijderen van wachtrijen**.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Een Ruby-toepassing maken

Maak een Ruby-toepassing. Zie voor instructies de [Ruby op Rails webtoepassing op een Azure VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-to-access-storage"></a>De toepassing toegang tot opslag configureren

Azure opslag gebruikt, moet u downloaden en gebruiken van de Ruby azure-pakket, inclusief een set met gemak bibliotheken die met de REST opslagservices communiceren.

### <a name="use-rubygems-to-obtain-the-package"></a>RubyGems downloaden van het pakket te gebruiken

1. Interface met een opdrachtregel zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix) gebruiken.

2. Typ "azure gem installeren" in het opdrachtvenster de gem en afhankelijkheden wilt installeren.

### <a name="import-the-package"></a>Het pakket importeren

Gebruik uw favoriete teksteditor, Voeg het volgende toe boven aan de Ruby-bestand waar u van plan bent opslag gebruiken:

    require "azure"

## <a name="setup-an-azure-storage-connection"></a>Setup een verbinding Azure opslag

De module azure leest de omgevingsvariabelen **AZURE\_opslag\_ACCOUNT** en **AZURE\_opslag\_ACCESS_KEY** voor meer informatie verbinding maken met uw account Azure opslag vereist. Als deze omgevingsvariabelen zijn niet ingesteld, moet u de gegevens voordat u **Azure::QueueService** met de volgende code:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your Azure storage access key>"

 
Als u deze waarden uit een klassieke of Resource Manager opslag account in Azure portal:

1. Log in op de [Azure portal](https://portal.azure.com).
2. Navigeer naar de opslag-account die u wilt gebruiken.
3. In het blad van de instellingen aan de rechterkant, klikt u op de **Toegangstoetsen**.
4. In het blad van de toegang tot sleutels die wordt weergegeven, ziet u de toegangstoets 1 en toegangssleutel 2. U kunt een van beide gebruiken. 
5. Klik op het pictogram kopiëren om de sleutel naar het Klembord kopiëren. 

Als u deze waarden uit een klassieke opslag account in de klassieke Azure portal:

1. Log in op de [klassieke Azure portal](https://manage.windowsazure.com).
2. Navigeer naar de opslag-account die u wilt gebruiken.
3. Klik op de **TOEGANGSTOETSEN beheren** onder aan het navigatiedeelvenster.
4. In het dialoogvenster pop ziet u de accountnaam voor opslag, toegang voor primaire en secundaire toegangssleutel. Voor een sneltoets, kunt u een primaire of secundaire een. 
5. Klik op het pictogram kopiëren om de sleutel naar het Klembord kopiëren.

## <a name="how-to-create-a-queue"></a>Procedure: Een wachtrij maken

De volgende code maakt een **Azure::QueueService** -object kunt u werken met wachtrijen.

    azure_queue_service = Azure::QueueService.new

Gebruik de methode **create_queue()** om een wachtrij te maken met de opgegeven naam.

    begin
      azure_queue_service.create_queue("test-queue")
    rescue
      puts $!
    end

## <a name="how-to-insert-a-message-into-a-queue"></a>Procedure: Plaats een bericht in een wachtrij

Gebruik de methode **create_message()** een nieuw bericht maken en aan de wachtrij toevoegen om een bericht in een wachtrij plaatst.

    azure_queue_service.create_message("test-queue", "test message")

## <a name="how-to-peek-at-the-next-message"></a>Procedure: Het volgende bericht bekijken

U kunt bekijken van het bericht aan de voorzijde van een wachtrij zonder het te verwijderen uit de wachtrij door het aanroepen van de **peek\_messages()** methode. Standaard **peek\_messages()** peeks op één bericht. U kunt ook opgeven hoeveel berichten u wilt kijken.

    result = azure_queue_service.peek_messages("test-queue",
      {:number_of_messages => 10})

## <a name="how-to-dequeue-the-next-message"></a>Procedure: Het volgende bericht in wachtrij

U kunt een bericht verwijderen uit een wachtrij in twee stappen.

1. Wanneer u belt **lijst\_messages()**, krijgt u het volgende bericht in een wachtrij standaard. U kunt ook opgeven hoeveel berichten op te halen. De berichten die zijn geretourneerd door **lijst\_messages()** wordt onzichtbaar is voor een andere code voor het lezen van berichten vanuit deze wachtrij. U doorgeven de zichtbaarheid van time-out in seconden als parameter.

2. Als u klaar bent met het bericht uit de wachtrij te verwijderen, moet u **delete_message()**aanroepen.

Dit proces van het verwijderen van een bericht bent u verzekerd dat wanneer uw code niet werkt voor het verwerken van een bericht door defecte hardware of software, een ander exemplaar van uw code kunt hetzelfde bericht krijgen en probeer het opnieuw. Het aanroepen van code **verwijderen\_message()** zodra het bericht is verwerkt.

    messages = azure_queue_service.list_messages("test-queue", 30)
    azure_queue_service.delete_message("test-queue", 
      messages[0].id, messages[0].pop_receipt)

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Procedure: De inhoud van een bericht in de wachtrij wijzigen

U kunt de inhoud van een bericht ter plaatse in de wachtrij wijzigen. De volgende code wordt de methode **update_message()** gebruikt voor het bijwerken van een bericht. De methode retourneert een tupel waarin de pop ontvangst van het bericht wachtrij en een UTC datum-tijdwaarde die aangeeft wanneer het bericht zichtbaar in de wachtrij zijn.

    message = azure_queue_service.list_messages("test-queue", 30)
    pop_receipt, time_next_visible = azure_queue_service.update_message(
      "test-queue", message.id, message.pop_receipt, "updated test message", 
      30)

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Procedure: Aanvullende opties voor waarbij berichten

Er zijn twee manieren waarop u kunt ophalen van berichten uit een wachtrij.

1. U kunt een batch van bericht.

2. U kunt een time-out invisibility langer of korter uw code zodat meer of minder tijd voor het verwerken van elk bericht volledig instellen.

Het volgende codevoorbeeld wordt de **lijst\_messages()** methode 15 berichten in één aanroep ophalen. Vervolgens worden afgedrukt en wordt elk bericht verwijderd. De time-out invisibility wordt ingesteld op vijf minuten voor elk bericht.

    azure_queue_service.list_messages("test-queue", 300
      {:number_of_messages => 15}).each do |m|
      puts m.message_text
      azure_queue_service.delete_message("test-queue", m.id, m.pop_receipt)
    end

## <a name="how-to-get-the-queue-length"></a>Procedure: De lengte van de wachtrij ophalen

U kunt een schatting van het aantal berichten in de wachtrij ophalen. De **u\_wachtrij\_metadata()** methode wordt gevraagd de wachtrijservice om het aantal berichten bij benadering en metagegevens over de wachtrij te retourneren.

    message_count, metadata = azure_queue_service.get_queue_metadata(
      "test-queue")

## <a name="how-to-delete-a-queue"></a>Procedure: Een wachtrij verwijderen

Voor het verwijderen van een wachtrij en alle berichten die dit, roept de **verwijderen\_queue()** methode voor het wachtrijobject.

    azure_queue_service.delete_queue("test-queue")

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd dat de basisbeginselen van de opslag van de wachtrij, volg deze koppelingen voor meer informatie over complexere opslagtaken.

- Ga naar de [opslag Azure-teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
- Ga naar de opslagplaats [Azure SDK for Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) op GitHub

Zie voor een vergelijking tussen de Azure wachtrijservice besproken in dit artikel en Azure Service Bus wachtrijen beschreven in het artikel [Service Bus wachtrijen gebruiken](/develop/ruby/how-to-guides/service-bus-queues/) , [wachtrijen Azure en Bus - wachtrijen vergeleken en Contrasted](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)
 
