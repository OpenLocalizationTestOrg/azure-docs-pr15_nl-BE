<properties
    pageTitle="Aan de slag met Relay-hybride verbindingen | Microsoft Azure"
    description="Het schrijven van een C#-consoletoepassing bij hybride verbindingen"
    services="service-bus"
    documentationCenter=".net"
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="hero-article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="jotaub"/>

# <a name="get-started-with-relay-hybrid-connections"></a>Aan de slag met Relay-hybride verbindingen

[AZURE.INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

## <a name="what-will-be-accomplished"></a>Wat doen

Aangezien het hybride verbindingen vereist zowel een client en een server-onderdeel, maken we twee consoletoepassingen in deze zelfstudie. Hier zijn de stappen:

1. Maak een Relay-naamruimte, via de portal Azure.

2. Een hybride verbinding maken, met de Azure portal.

3. Een server consoletoepassing schrijven om te ontvangen van berichten.

4. Schrijf een client consoletoepassing om berichten te verzenden.

## <a name="prerequisites"></a>Vereisten

1. [Visual Studio 2013 of Visual Studio 2015](http://www.visualstudio.com). De voorbeelden in deze zelfstudie gebruiken Visual Studio 2015.

2. Een abonnement op Azure.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Maak een naamruimte met de Azure portal

Hebt u al een Relay-naamruimte gemaakt, springt u naar de sectie [een hybride verbinding maken met behulp van de portal Azure](#2-create-a-hybrid-connection-using-the-azure-portal) .

[AZURE.INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a>2. een hybride verbinding maken met de Azure portal

Als u al een hybride verbinding gemaakt, gaat u naar de sectie [een server-toepassing maken](#3-create-a-server-application-listener) .

[AZURE.INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. een servertoepassing (listener) maken

Om te luisteren en berichten ontvangen van de Relay, worden we een C#-consoletoepassing met behulp van Visual Studio geschreven.

[AZURE.INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-dotnet-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Maak een clienttoepassing (afzender)

Om berichten te verzenden naar de Relay, worden we een C#-consoletoepassing met behulp van Visual Studio geschreven.

[AZURE.INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-dotnet-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. de toepassingen uitvoeren

1. De servertoepassing wordt uitgevoerd.

2. De clienttoepassing uitvoert en de tekst opgeven.

3. Zorg ervoor dat de application server-console wordt de tekst die is ingevoerd in de clienttoepassing.

![toepassingen uitvoeren](./media/relay-hybrid-connections-dotnet-get-started/running-applications.png)

Gefeliciteerd, u hebt een hybride verbindingen end-to-end toepassing gemaakt.

## <a name="next-steps"></a>Volgende stappen:

- [Veelgestelde vragen over doorsturen](relay-faq.md)
- [Een naamruimte maken](relay-create-namespace-portal.md)
- [Aan de slag met het knooppunt](relay-hybrid-connections-node-get-started.md)