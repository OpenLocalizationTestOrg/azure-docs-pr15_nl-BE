<properties
   pageTitle="Lokaal controleren en diagnosticeren services geschreven met Azure Service Fabric | Microsoft Azure"
   description="Informatie over het controleren en analyseren van uw services geschreven met Microsoft Azure Service weefsel op een machine voor plaatselijke ontwikkeling."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/24/2016"
   ms.author="subramar"/>


# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>Controleren en diagnosticeren van services in de instellingen voor een lokale computer ontwikkeling


> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
- [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)

Monitoring, detectie, diagnose en probleemoplossing kunt services wilt gaan met minimale overlast voor de gebruikerservaring. Bewaking en diagnose zijn van cruciaal belang in een gedistribueerde de werkelijke productie-omgeving. Een vergelijkbaar model tijdens de ontwikkeling van diensten vast te stellen, zorgt u ervoor dat de diagnostische pijpleiding werkt wanneer u naar een productieomgeving. Fabric-service kunt u gemakkelijk voor ontwikkelaars voor het implementeren van diagnostische gegevens die naadloos in zowel plaatselijke ontwikkeling van één computer instellingen en werkelijke productie-instellingen voor cluster werken kunnen service.


## <a name="debugging-service-fabric-java-applications"></a>Foutopsporing in Service Fabric Java-toepassingen

[Meerdere kaders voor logboekregistratie](http://en.wikipedia.org/wiki/Java_logging_framework) zijn beschikbaar voor Java-toepassingen. Aangezien `java.util.logging` is de standaardoptie met Java Runtime Environment, wordt ook gebruikt voor de [voorbeelden van code in de github](http://github.com/Azure-Samples/service-fabric-java-getting-started).  Het volgende onderwerp wordt uitgelegd hoe u de `java.util.logging` framework. 
 
Met java.util.logging kunt u de toepassingslogboeken omleiden naar geheugen, uitvoerstromen, consolebestanden of sockets. Voor elk van deze opties zijn standaard mailhandlers al in het kader. Kunt u een `app.properties` bestand voor het configureren van de bestands-handler voor de toepassing alle logboeken omleiden naar een lokaal bestand. 

Het volgende stukje code bevat een van de voorbeeldconfiguratie: 

```java 
handlers = java.util.logging.FileHandler
 
java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log             
```

De map waarnaar wordt verwezen door de `app.properties` bestand moet bestaan. Na de `app.properties` wordt gemaakt, moet u ook uw post punt script wijzigen `entrypoint.sh` in de `<applicationfolder>/<servicePkg>/Code/` map voor het instellen van de eigenschap `java.util.logging.config.file` te `app.propertes` bestand. De vermelding moet er uitzien zoals in het volgende fragment:

```sh 
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path to app.properties> -jar <service name>.jar
```
 
 
Deze configuratie resulteert in Logboeken worden verzameld in een draaiende op `/tmp/servicefabric/logs/`. De **%u** en **%g** toestaan voor het maken van meer bestanden met bestandsnamen, mysfapp0.log en mysfapp1.log. Als geen handler expliciet is geconfigureerd, wordt standaard de handler console geregistreerd. Een kunt de logboeken weergeven in syslog onder /var/log/syslog.
 
Voor meer informatie, Zie de [voorbeelden van code in de github](http://github.com/Azure-Samples/service-fabric-java-getting-started).  



## <a name="next-steps"></a>Volgende stappen
De diagnostische gegevens van de toepassing op een cluster Azure werkt ook dezelfde tracering code toegevoegd aan uw toepassing. Lees deze artikelen bespreken de verschillende opties voor de hulpprogramma's en wordt beschreven hoe u kunt deze instellen.
* [Hoe u logboeken met Azure diagnostische gegevens verzamelen](service-fabric-diagnostics-how-to-setup-lad.md)
