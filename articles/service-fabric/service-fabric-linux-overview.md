<properties
   pageTitle="Azure Service Fabric op Linux | Microsoft Azure"
   description="Clusters Fabric-service ondersteuning voor Linux en Java, wat betekent dat u zult kunt implementeren en host Service Fabric toepassingen geschreven in Java en C# op Linux."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="Java"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="SubramaR"/>

# <a name="service-fabric-on-linux"></a>Service Fabric op Linux

Het voorbeeld van de stof op Linux-Service kunt u bouwen, implementeren en beheren zeer schaalbare toepassingen met hoge beschikbaarheid op Linux, net als in Windows. De Service Fabric frameworks (betrouwbare diensten en betrouwbare actoren) zijn beschikbaar in Java op Linux naast C# (.NET Core).  U kunt ook [uitvoerbare services Gast](service-fabric-deploy-existing-app.md) met elke taal of het kader maken. Bovendien ondersteunt de voorvertoning regie van Docker containers. Docker containers kunnen Gast uitvoerbare bestanden of eigen Service configuratieservices, waarin de kaders Fabric-Service uitgevoerd.

Service-structuur op Linux is conceptueel gezien equivalent aan de Fabric-Service op Windows (behalve OS specificaties en programming language support). De meeste van onze [bestaande documentatie](http://aka.ms/servicefabricdocs) geldt dus, zodat u vertrouwd raken met de technologie.

> [AZURE.VIDEO service-fabric-linux-preview]

## <a name="supported-operating-systems-and-programming-languages"></a>Ondersteunde besturingssystemen en programmeertalen

Voorbeeld van de beperkte ondersteunt het maken van clusters-in-één ontwikkeling naast meerdere machines clusters in Azure Ubuntu Server 16.04 uitgevoerd. De voorvertoning ondersteunt de betrouwbare actoren en de betrouwbare Stateless Services frameworks in Java en C# Gast uitvoerbare bestanden en regie van Docker containers.  

>[AZURE.NOTE] Betrouwbare collecties worden nog niet ondersteund in Linux. Worden niet standaard alleen clusters ondersteund - ofwel slechts één vak en meerdere machines Azure Linux-clusters worden ondersteund in de voorvertoning.

## <a name="supported-tooling"></a>Ondersteunde tooling

Voorbeeld van de ondersteunt interactie met het cluster via Azure CLI. Voor ontwikkelaars van Java, is integratie met Eclips en Yeoman voorzien van Eclips ondersteund op Linux en OSX. De integratie van OSX maakt gebruik van een Linux VM achter de schermen via vagrant. Integratie met Yeoman krijgt Toepassingssjablonen genereren voor C#-ontwikkelaars.

## <a name="next-steps"></a>Volgende stappen


1. Vertrouwd raken met de [Betrouwbare Actors](service-fabric-reliable-actors-introduction.md) en [Betrouwbare Services](service-fabric-reliable-services-introduction.md) frameworks voor het programmeren.

2. [Bereid uw ontwikkelomgeving op Linux](service-fabric-get-started-linux.md)

3. [Bereid uw ontwikkelomgeving op OSX](service-fabric-get-started-mac.md)

4. [De eerste Service Fabric Java-toepassing maken op Linux](service-fabric-create-your-first-linux-application-with-java.md)
