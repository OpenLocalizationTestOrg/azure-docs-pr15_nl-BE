<properties
    pageTitle="Toegang Hadoop GAREN toepassingslogboeken programmatisch | Microsoft Azure"
    description="Access-toepassing zich aanmeldt via programmering een cluster Hadoop in HDInsight."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

# <a name="access-yarn-application-logs-on-windows-based-hdinsight"></a>Access toepassingslogboeken GAREN op Windows gebaseerde HDInsight

In dit onderwerp wordt uitgelegd hoe u toegang tot de logboeken voor garens (nog een andere Resource onderhandelaar)-toepassingen die u op een cluster Hadoop in Azure, HDInsight hebt

> [AZURE.NOTE] De informatie in dit document geldt alleen voor HDInsight op basis van Windows-clusters. Voor informatie over toegang tot garens HDInsight Linux gebaseerde clusters zich aanmeldt, Zie [toepassingslogboeken Access garens op Linux gebaseerde Hadoop op HDInsight](hdinsight-hadoop-access-yarn-app-logs-linux.md)

### <a name="prerequisites"></a>Vereisten

- Een cluster van Windows-gebaseerde HDInsight.  Zie [Hadoop maken Windows-gebaseerde clusters in HDInsight](hdinsight-provision-clusters.md).


## <a name="yarn-timeline-server"></a>GARENS tijdlijn Server

De <a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">Garens tijdlijn Server</a> biedt algemene informatie over voltooide toepassingen zoals application framework-specifieke informatie via twee verschillende interfaces. Met name:

* Opslaan en ophalen van algemene toepassingsinformatie over clusters HDInsight is ingeschakeld met versie 3.1.1.374 of hoger.
* Het onderdeel application framework-specifieke informatie van de Server van de tijdlijn is momenteel niet beschikbaar op HDInsight-clusters.


Algemene informatie over toepassingen omvat de volgende soorten gegevens:

* De toepassings-ID, een unieke id van een toepassing
* De gebruiker die de toepassing heeft gestart
* Informatie over pogingen tot de toepassing voltooien
* De recipiënten die worden gebruikt door een bepaalde toepassing poging

Op HDInsight clusters, wordt deze informatie opgeslagen door Azure Resource Manager aan een winkel geschiedenis in de standaardcontainer van uw standaardaccount Azure opslag. Deze algemene gegevens van voltooide toepassingen kan worden opgehaald met een REST API:

    GET on https://<cluster-dns-name>.azurehdinsight.net/ws/v1/applicationhistory/apps


## <a name="yarn-applications-and-logs"></a>Logboeken voor toepassingen garens en

GARENS ondersteunt ontkoppeling Resourcemanagement van planning/controle van toepassingen meerdere programmeermodellen (MapReduce wordt één van beide). Dit wordt gedaan door middel van een globale *ResourceManager* (RM), per werknemer-knooppunt *NodeManagers* (NMs) en per toepassing *ApplicationMasters* (AMs). De AM per toepassing onderhandelt over de resources (CPU, geheugen, schijf, netwerk) voor het uitvoeren van uw toepassing met het RM. De RM werkt met NMs verlenen deze bronnen, worden toegekend als *containers*. De AM is verantwoordelijk voor het volgen van de voortgang van de containers die zijn toegewezen door de RM. Een toepassing mogelijk veel containers, afhankelijk van de aard van de toepassing.

Bovendien elke toepassing kan bestaan uit meerdere *toepassing probeert* te voltooien met vastlopen of door het verlies van de communicatie tussen een uur en een RM. Daarom worden containers verleend aan een specifieke poging van een toepassing. In zekere zin een container biedt de context voor de basiseenheid van het werk dat door een toepassing garens en al het werk dat wordt uitgevoerd binnen de context van een container wordt uitgevoerd op de werknemer één knooppunt waarop de container is toegewezen. Zie [Begrippen GAREN] [ YARN-concepts] voor verdere verwijzing.

Toepassingslogboeken (en de bijbehorende container Logboeken) zijn van cruciaal belang in de foutopsporing in toepassingen die problemen veroorzaken Hadoop. GARENS biedt een goed kader voor het verzamelen, aggregeren en toepassingslogboeken met de [Aggregatie van logboek] opslaan[ log-aggregation] functie. Het logboek aggregatie functie kunt u toegang tot toepassingslogboeken een deterministische manier aggregeert logboeken over alle containers op een knooppunt van de werknemer en worden opgeslagen als een geaggregeerde logboekbestand per werknemer knooppunt op het standaardbestandssysteem nadat een toepassing is voltooid. Uw toepassing kan honderden of duizenden containers gebruiken, maar altijd voor alle containers die worden uitgevoerd op een knooppunt met één werknemer worden samengevoegd in één bestand, wat resulteert in één logboekbestand per werknemer knooppunt gebruikt door de toepassing. Aggregatie van logboekbestanden is standaard ingeschakeld op clusters van HDInsight (versie 3.0 en hoger), en geaggregeerde logboeken kunnen u vinden in de standaardcontainer van het cluster op de volgende locatie:

    wasbs:///app-logs/<user>/logs/<applicationId>

Locatie van de *gebruiker* is de naam van de gebruiker die de toepassing heeft gestart en *applicationId* is de unieke id van een toepassing, zoals toegewezen door de RM. garens

De geaggregeerde logboeken zijn niet rechtstreeks kan worden gelezen, zoals ze zijn geschreven in een [TFile][T-file], [binaire indeling] [ binary-format] geïndexeerd door de container. GARENS biedt CLI hulpmiddelen voor het dumpen van deze logboeken als tekst zonder opmaak voor toepassingen of containers van belang. U kunt deze logboeken weergeven als tekst zonder opmaak door het uitvoeren van een van de volgende garens rechtstreeks op de clusterknooppunten opdrachten (na verbinding maken via RDP):

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>


## <a name="yarn-resourcemanager-ui"></a>GARENS ResourceManager UI

De ResourceManager GAREN UI wordt uitgevoerd op de headnode van het cluster en toegankelijk zijn via het portal Azure dashboard: 

1. Aanmelden bij [Azure portal](https://portal.azure.com/). 
2. Klik in het menu links klikt u op **Bladeren**, klik op **Clusters van HDInsight**, een op Windows gebaseerde cluster die u toegang wilt tot de toepassingslogboeken GAREN.
3. Klik in het bovenste menu op **Dashboard**. U ziet een pagina geopend op een nieuwe browser tab heet **HDInsight Query-Console**.
4. Klik op **Garens UI**- **HDInsight Query-Console**.




[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
