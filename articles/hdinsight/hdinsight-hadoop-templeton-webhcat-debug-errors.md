<properties
 pageTitle="Begrijpen en oplossen van fouten op HDInsight WebHCat"
 description="Informatie over hoe te over veelvoorkomende fouten geretourneerd door WebHCat in HDInsight en hoe deze op te lossen."
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"
 tags="azure-portal"/>

<tags
 ms.service="hdinsight"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="09/27/2016"
 ms.author="larryfr"/>

#<a name="understand-and-resolve-errors-received-from-webhcat-templeton-on-hdinsight"></a>Begrijpen en oplossen van fouten met betrekking tot WebHCat (Templeton), op HDInsight

Bij het gebruik van WebHCat (voorheen bekend als Templeton,) om te werken met HDInsight, kunnen er foutberichten. Dit document verschaft leidraden voor veelvoorkomende fouten: waarom ze zich voordoen en wat u kunt doen om deze fouten kunt oplossen.

##<a name="what-is-webhcat"></a>Wat is WebHCat?

[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) is een REST API voor [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), een tabel en storage management-laag voor Hadoop. WebHCat is standaard ingeschakeld op clusters van HDInsight en wordt gebruikt door diverse hulpprogramma's om taken, taakstatus, enz., zonder in te loggen met het cluster te vinden.

##<a name="modifying-configuration"></a>Configuratie wijzigen

> [AZURE.IMPORTANT] Verschillende van de in dit document vermelde fouten optreden omdat een geconfigureerde maximum is overschreden. Wanneer u de resolutie-stap wordt vermeld dat u een waarde kunt wijzigen, moet u een van de volgende opties voor het uitvoeren van de wijziging:

* Voor **Windows** -clusters: een scriptactie gebruiken voor het configureren van de waarde tijdens het maken van het cluster. Zie [scriptacties ontwikkelen](hdinsight-hadoop-script-actions.md)voor meer informatie.

* Voor **Linux** -clusters: gebruik Ambari (web of REST API) om de waarde te wijzigen. Voor meer informatie, Zie [beheren van HDInsight Ambari](hdinsight-hadoop-manage-ambari.md)

###<a name="default-configuration"></a>Standaardconfiguratie

Standaardconfiguratiewaarden die kunnen invloed op de prestaties van de WebHCat of fouten veroorzaken als deze waarden worden overschreden, zijn:

| Instelling | Wat het doet | Standaardwaarde |
| ------- | ------------ | ------------- |
| [yarn.scheduler.Capacity.maximum-toepassingen][maximum-applications] | Het maximum aantal taken dat tegelijkertijd actief kan zijn (in behandeling of wordt uitgevoerd) | 10.000 |
| [templeton.Exec.Max-processor][max-procs] | Het maximum aantal aanvragen dat tegelijkertijd kan worden geholpen | 20 |
| [mapreduce.jobhistory.Max-age-ms][max-age-ms] | Het aantal dagen dat de geschiedenis bewaard | 7 dagen |

##<a name="too-many-requests"></a>Te veel aanvragen

**HTTP-statuscode**: 429

| Oorzaak | Resolutie |
| ----- | ---------- |
| U hebt het maximum aantal gelijktijdige aanvragen die worden bediend door WebHCat per minuut (standaard 20) overschreden | Verminder uw werkbelasting om ervoor te zorgen dat u niet indienen van meer dan het maximum aantal gelijktijdige aanvragen of het aantal gelijktijdige aanvragen verhogen doordat `templeton.exec.max-procs`. Zie [de configuratie wijzigen](#modifying-configuration) voor meer informatie |

##<a name="server-unavailable"></a>Server niet beschikbaar

**HTTP-statuscode**: 503

| Oorzaak | Resolutie |
| ---------------- | ------------------- |
| Dit gebeurt meestal tijdens een overname tussen de primaire en secundaire HeadNode voor het cluster | Wacht twee minuten en probeer het opnieuw |

##<a name="bad-request-content-could-not-find-job"></a>Ongeldig verzoek inhoud: kan de taak niet vinden.

**HTTP-statuscode**: 400

| Oorzaak | Resolutie |
| ---------------- | ------------------- |
| Details van zijn gewist door de geschiedenis van Reinigingsmedium | De bewaartermijn voor de taakgeschiedenis is 7 dagen. Dit kan worden gewijzigd door aanpassing van `mapreduce.jobhistory.max-age-ms`. Zie [de configuratie wijzigen](#modifying-configuration) voor meer informatie |
| Taak is beëindigd vanwege een failover | Indiening van de taak voor maximaal twee minuten opnieuw |
| Een geldige taak-id is gebruikt. | Controleer of de taak-id correct is |

##<a name="bad-gateway"></a>Ongeldige gateway

**HTTP-statuscode**: 502

| Oorzaak | Resolutie |
| ---------------- | ------------------- |
| Interne opschoonbewerking plaatsvindt in het proces van WebHCat | Wachten op de opschoonbewerking is voltooid of de WebHCat-service opnieuw starten |
| Time-out voor wachten op een reactie van de ResourceManager-service. Dit kan gebeuren wanneer het aantal actieve toepassingen het geconfigureerde maximum (standaard 10.000 wordt) | Wachten voor de actieve taken te voltooien of gelijktijdige taaklimiet verhogen doordat `yarn.scheduler.capacity.maximum-applications`. Zie [de configuratie wijzigen](#modifying-configuration) voor meer informatie  |
| Bij het ophalen van alle taken die door de aanroep [ophalen van /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) bij `Fields` is ingesteld op`*` | Niet *alle* details van het project te halen, in plaats daarvan gebruiken `jobid` voor het ophalen van gegevens voor taken alleen groter is dan een bepaalde taak-id. Of niet te gebruiken`Fields` |
| De WebHCat-service is niet beschikbaar tijdens failover HeadNode | Wacht twee minuten en probeer het opnieuw |
| Er zijn meer dan 500 onvoltooide taken worden ingediend via WebHCat | Wacht totdat het momenteel in behandeling zijnde taken hebben voltooid alvorens meer banen |

[maximum-applications]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://hive.apache.org/javadocs/hcat-r0.5.0/configuration.html
[max-age-ms]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
 
