<properties 
   pageTitle="Syslog berichten in een logboek Analytics | Microsoft Azure"
   description="Syslog is een protocol voor het vastleggen van gebeurtenis voor Linux.   In dit artikel wordt beschreven hoe verzameling Syslog berichten in een logboek Analytics en de details van de records die zij in de opslagplaats OMS maken configureren."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />


# <a name="syslog-data-sources-in-log-analytics"></a>Syslog-gegevensbronnen in logboek Analytics

Syslog is een protocol voor het vastleggen van gebeurtenis voor Linux.  Toepassingen worden berichten verzenden die kunnen worden opgeslagen op de lokale computer of geleverd aan een verzamelaar Syslog.  Wanneer de Agent OMS voor Linux is geïnstalleerd, configureert de lokale Syslog daemon voor het doorsturen van berichten naar de agent.  De agent vervolgens bericht het naar logboek Analytics waar een overeenkomstige record in de OMS opslagplaats wordt gemaakt.  

> [AZURE.NOTE]Logboek Analytics ondersteunt een verzameling berichten die afkomstig zijn van rsyslog, syslog-ng. De standaard syslog daemon op versie 5 van Red Hat Enterprise Linux CentOS en Oracle Linux versie (sysklog) wordt niet ondersteund voor syslog gebeurtenis-collectie. Syslog om gegevens te verzamelen van deze versie van deze verdelingen, moet de [daemon rsyslog](http://rsyslog.com) worden geïnstalleerd en geconfigureerd voor de sysklog vervangen.

![Syslog-collectie](media/log-analytics-data-sources-syslog/overview.png)


## <a name="configuring-syslog"></a>Configureren van Syslog
De Agent OMS voor Linux verzamelt alleen gebeurtenissen met de faciliteiten en severities die zijn opgegeven in de configuratie.  U kunt Syslog configureren via de portal OMS of configuratiebestanden beheren op uw Linux-agenten.


### <a name="configure-syslog-in-the-oms-portal"></a>Syslog in de portal OMS configureren

Syslog in het [menu Data in Analytics instellingen](log-analytics-data-sources.md#configuring-data-sources)configureren.  Deze configuratie wordt geleverd aan het configuratiebestand op elke Linux-agent.

U kunt een nieuwe opslagruimte toevoegen door te typen in de naam en klikt u op **+**.  Voor elke faciliteit, worden alleen berichten met de geselecteerde severities verzameld.  Controleer de severities voor de specifieke functie die u wilt verzamelen.  U kan geen bieden aanvullende criteria om berichten te filteren.

![Syslog configureren](media/log-analytics-data-sources-syslog/configure.png)


Standaard worden alle wijzigingen in de configuratie automatisch geduwd naar alle agenten.  Als u wilt Syslog handmatig configureren op elke Linux-agent, schakel dan het selectievakje *toepassen onder configuratie van mijn Linux machines*in.


### <a name="configure-syslog-on-linux-agent"></a>Syslog op Linux-agent configureren

Wanneer de [OMS agent is geïnstalleerd op een Linux-client](log-analytics-linux-agents.md), wordt er een standaard syslog-configuratiebestand waarin de faciliteit en de ernst van de berichten die worden verzameld.  U kunt dit bestand als u de configuratie kunt wijzigen.  Het configuratiebestand is afhankelijk van de Syslog daemon die de client heeft geïnstalleerd.

> [AZURE.NOTE] Als u de configuratie van syslog bewerkt, moet u opnieuw de syslog daemon om de wijzigingen van kracht te laten worden.

#### <a name="rsyslog"></a>rsyslog

Het configuratiebestand voor rsyslog bevindt zich op **/etc/rsyslog.d/95-omsagent.conf**.  Hieronder ziet u de Standaardinhoud.  Dit verzamelt syslog berichten van de lokale agent voor alle voorzieningen met een niveau van waarschuwing of hoger.

    kern.warning       @127.0.0.1:25224
    user.warning       @127.0.0.1:25224
    daemon.warning     @127.0.0.1:25224
    auth.warning       @127.0.0.1:25224
    syslog.warning     @127.0.0.1:25224
    uucp.warning       @127.0.0.1:25224
    authpriv.warning   @127.0.0.1:25224
    ftp.warning        @127.0.0.1:25224
    cron.warning       @127.0.0.1:25224
    local0.warning     @127.0.0.1:25224
    local1.warning     @127.0.0.1:25224
    local2.warning     @127.0.0.1:25224
    local3.warning     @127.0.0.1:25224
    local4.warning     @127.0.0.1:25224
    local5.warning     @127.0.0.1:25224
    local6.warning     @127.0.0.1:25224
    local7.warning     @127.0.0.1:25224

Een faciliteit kunt u verwijderen door het verwijderen van de sectie van het configuratiebestand.  U kunt de severities die worden verzameld voor een bepaalde opslagruimte doordat deze faciliteit van post beperken.  U zou bijvoorbeeld wilt beperken van de gebruiker faciliteit voor berichten met een prioriteit van de fout of hoger die regel van het configuratiebestand met de volgende wijzigen:

    user.error  @127.0.0.1:25224


#### <a name="syslog-ng"></a>Syslog-ng

Het configuratiebestand voor rsyslog is een locatie op de **/etc/syslog-ng/syslog-ng.conf**.  Hieronder ziet u de Standaardinhoud.  Dit syslog berichten van de lokale agent voor alle faciliteiten en alle severities worden verzameld.   

    #
    # Warnings (except iptables) in one file:
    #
    destination warn { file("/var/log/warn" fsync(yes)); };
    log { source(src); filter(f_warn); destination(warn); };
    
    #OMS_Destination
    destination d_oms { udp("127.0.0.1" port(25224)); };

    #OMS_facility = auth
    filter f_auth_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(auth); };
    log { source(src); filter(f_auth_oms); destination(d_oms); };

    #OMS_facility = authpriv
    filter f_authpriv_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(authpriv); };
    log { source(src); filter(f_authpriv_oms); destination(d_oms); };

    #OMS_facility = cron
    filter f_cron_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(cron); };
    log { source(src); filter(f_cron_oms); destination(d_oms); };

    #OMS_facility = daemon
    filter f_daemon_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(daemon); };
    log { source(src); filter(f_daemon_oms); destination(d_oms); };

    #OMS_facility = kern
    filter f_kern_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(kern); };
    log { source(src); filter(f_kern_oms); destination(d_oms); };
    
    #OMS_facility = local0
    filter f_local0_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local0); };
    log { source(src); filter(f_local0_oms); destination(d_oms); };
    
    #OMS_facility = local1
    filter f_local1_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local1); };
    log { source(src); filter(f_local1_oms); destination(d_oms); };
    
    #OMS_facility = mail
    filter f_mail_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(mail); };
    log { source(src); filter(f_mail_oms); destination(d_oms); };
    
    #OMS_facility = syslog
    filter f_syslog_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(syslog); };
    log { source(src); filter(f_syslog_oms); destination(d_oms); };
    
    #OMS_facility = user
    filter f_user_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };

Een faciliteit kunt u verwijderen door het verwijderen van de sectie van het configuratiebestand.  U kunt de severities die worden verzameld voor een bepaalde faciliteit door ze te verwijderen uit de lijst beperken.  U zou bijvoorbeeld alleen de gebruiker faciliteit alleen waarschuwingen en kritieke berichten dat gedeelte van het configuratiebestand op het volgende wijzigen:

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="changing-the-syslog-port"></a>De Syslog-poort wijzigen

De agent OMS luistert naar Syslog berichten op de lokale client in haven 25224.  U kunt deze poort wijzigen door de volgende sectie toe te voegen aan het configuratiebestand van de OMS-agent te vinden op **/etc/opt/microsoft/omsagent/conf/omsagent.conf**.  25224 in de post van de **poort** door het poortnummer dat u wilt vervangen.  Houd er rekening mee dat u ook moet het configuratiebestand voor de Syslog daemon om berichten te verzenden naar deze poort wijzigen.

    <source>
      type syslog
      port 25224
      bind 127.0.0.1
      protocol_type udp
      tag oms.syslog
    </source>


## <a name="data-collection"></a>Het verzamelen van gegevens

De agent OMS luistert naar Syslog berichten op de lokale client in haven 25224. Het configuratiebestand voor de daemon Syslog doorgestuurd Syslog berichten van toepassing op deze poort waar ze worden verzameld door Analytics logboek.


## <a name="syslog-record-properties"></a>Eigenschappen van de record Syslog

Syslog records hebben een soort **Syslog** en hebben de eigenschappen in de volgende tabel.

| Eigenschap | Beschrijving |
|:--|:--|
| Computer | De computer waarop de gebeurtenis is verzameld uit. |
| Faciliteit | Hiermee definieert u het deel van het systeem dat het bericht gegenereerd. |
| HostIP | IP-adres van het systeem te verzenden.  |
| Hostnaam | De naam van het systeem te verzenden. |
| Foutcode | De ernst van de gebeurtenis. |
| SyslogMessage | De tekst van het bericht. |
| Proces-id | De ID van het proces dat het bericht gegenereerd. |
| EventTime | Datum en tijd waarop de gebeurtenis is gegenereerd.



## <a name="log-queries-with-syslog-records"></a>Logboek voor query's met Syslog records

De volgende tabel bevat voorbeelden van logboek-query's die Syslog records ophalen.

| Query | Beschrijving |
|:--|:--|
| Type = Syslog | Alle Syslogs. |
| Type = Syslog SeverityLevel = fout | Alle Syslog-records met de ernst van de fout. |
| Type = Syslog & #124; count() door Computer meten | Telling van Syslog-records op de computer. |
| Type = Syslog & #124; count() maatregel door de faciliteit | Telling van Syslog records op faciliteit. |

## <a name="next-steps"></a>Volgende stappen

- Informatie over het [logboek zoeken](log-analytics-log-searches.md) voor het analyseren van de gegevens die worden verzameld uit gegevensbronnen en oplossingen. 
- [Aangepaste velden](log-analytics-custom-fields.md) gebruiken om gegevens van syslog records worden geparseerd in afzonderlijke velden.
- [Linux configureren agenten](log-analytics-linux-agents.md) voor het verzamelen van andere typen gegevens. 
