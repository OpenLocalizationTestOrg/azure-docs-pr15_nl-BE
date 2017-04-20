<properties
    pageTitle="RStudio met R-Server op HDInsight (voorbeeld) installeren | Microsoft Azure"
    description="Het installeren van RStudio met R-Server op HDInsight (voorbeeld)."
    services="hdinsight"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/16/2016"
   ms.author="jeffstok"/>


# <a name="installing-rstudio-with-r-server-on-hdinsight-preview"></a>Installeren van RStudio met R-Server op HDInsight (voorbeeld)

Er zijn meerdere geïntegreerde ontwikkelomgevingen (IDE) beschikbaar voor R vandaag, [R-hulpprogramma's voor Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS), een familie van desktop- en hulpprogramma's van de [RStudio](https://www.rstudio.com/products/rstudio-server/)of de Walware op basis van een Eclips [StatET](http://www.walware.de/goto/statet)inclusief Microsoft onlangs aangekondigd. Van de meest populaire op Linux is het gebruik van [RStudio-Server](https://www.rstudio.com/products/rstudio-server/) waarmee u een browser gebaseerde IDE voor externe clients.  RStudio Server installeren op de randknooppunt van een cluster HDInsight Premium kan biedt een volledige IDE-ervaring voor de ontwikkeling en uitvoering van scripts R R-Server in het cluster en aanzienlijk efficiënter dan standaard gebruik van de R-Console.

In dit artikel leert u hoe u de Gemeenschap (gratis) versie van RStudio-Server installeren op de randknooppunt van een cluster met behulp van een aangepast script. Als u liever de commerciële licentie Pro-versie van RStudio-Server, moet u de installatie-instructies volgen van [RStudio-Server](https://www.rstudio.com/products/rstudio/download-server/).

> [AZURE.NOTE] De stappen in dit document vereist een R-Server in het cluster HDInsight en zullen niet goed werken als u een HDInsight-cluster waarin R is geïnstalleerd via de [Actie van R-Script installeren](hdinsight-hadoop-r-scripts-linux.md).

## <a name="prerequisites"></a>Vereisten

* Een cluster Azure-HDInsight met R-Server geïnstalleerd. Zie [aan de slag met R-Server op HDInsight-clusters](hdinsight-hadoop-r-server-get-started.md)voor meer informatie.
* SSH-client. Distributies van Linux en Unix of Macintosh OS X, de `ssh` opdracht wordt geleverd bij het besturingssysteem. Voor Windows, wordt aangeraden [Cygwin](http://www.redhat.com/services/custom/cygwin/) met de [optie OpenSSH](https://www.youtube.com/watch?v=CwYSvvGaiWU)of [stopverf](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).  


## <a name="install-rstudio-on-the-cluster-using-a-custom-script"></a>RStudio op het cluster met behulp van een aangepast script installeren

1. Identificeer de randknooppunt van het cluster. Voor een HDInsight-cluster met R Server volgt de naamgevingsconventie voor hoofd en rand knooppunten.

    * Knooppunt van de hoofd-`CLUSTERNAME-ssh.azurehdinsight.net`
    * Randknooppunt-`R-Server.CLUSTERNAME-ssh.azurehdinsight.net` 

2. SSH in de randknooppunt van het cluster met behulp van de bovenstaande naampatroon. 
 
    * Als u verbinding vanaf een client voor Linux maakt, Zie [verbinding maken met een cluster van Linux-gebaseerde HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster).
    * Als u verbinding vanaf een Windows-client maakt, Zie [verbinding maken met een HDInsight op basis van Linux-cluster met stopverf](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster).

3. Zodra u verbonden bent, wordt een Hoofdgebruiker op het cluster. Gebruik de volgende opdracht in de SSH-sessie.

        sudo su -

4. Download het aangepaste script RStudio installeren. Gebruik de volgende opdracht.

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. Wijzig de machtigingen voor het bestand aangepast script en voer het script uit. Gebruik de volgende opdrachten.

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. Als u een SSH wachtwoord tijdens het maken van een cluster HDInsight met R-Server gebruikt, kunt u deze stap overslaan en doorgaan met de volgende. Als u een SSH-sleutel in plaats daarvan gebruikt voor het maken van het cluster, moet u een wachtwoord instellen voor de gebruiker SSH. U moet dit wachtwoord wanneer u verbinding maakt met RStudio. Voer de volgende opdrachten. Druk op **ENTER**als u **huidige Kerberos-wachtwoord**wordt gevraagd.  Opmerking: u moet vervangen `USERNAME` met een SSH-gebruiker voor uw cluster HDInsight.

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:
        
    Als uw wachtwoord correct is ingesteld, wordt er een bericht als volgt.

        passwd: password updated successfully


    De SSH-sessie af te sluiten.

7. Maak een SSH-tunnel met het cluster door te wijzen `localhost:8787` op de cluster HDInsight naar de clientcomputer. U moet een SSH-tunnel maken voordat u een nieuwe browsersessie opent.

    * Open een terminal sessie en gebruikt u de volgende opdracht op een Linux-client of een Windows-client met [Cygwin](http://www.redhat.com/services/custom/cygwin/) vervolgens.

            ssh -L localhost:8787:localhost:8787 USERNAME@R-Server.CLUSTERNAME-ssh.azurehdinsight.net
            
        **Gebruikersnaam** vervangen door een SSH-gebruiker voor uw cluster HDInsight en **CLUSTERNAAM** vervangen door de naam van het cluster HDInsight kun je een SSH-sleutel in plaats van een wachtwoord door toe te voegen`-i id_rsa_key`     

    * Als een Windows-client met stopverf vervolgens

        1.  Open PuTTY en geef de verbindingsgegevens. Als u niet bekend met stopverf bent, Zie [Gebruiken SSH met Linux-gebaseerde Hadoop op HDInsight van Windows](hdinsight-hadoop-linux-use-ssh-windows.md) voor informatie over het gebruik in combinatie met HDInsight.
        2.  In de sectie **categorie** aan de linkerkant van het dialoogvenster **verbinding**Vouw, uit van **SSH**en selecteer vervolgens de **Tunnels**.
        3.  Geef de volgende gegevens op het formulier **Opties beheren SSH port forwarding** :

            * **Bronpoort** - de poort op de client die u wilt doorsturen. Bijvoorbeeld **8787**.
            * **Bestemming** - de bestemming die moet worden toegewezen aan de lokale computer. Bijvoorbeeld **localhost:8787**.

            ![Een SSH-tunnel maken] (./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "Een SSH-tunnel maken")

        4. Klik op **toevoegen** om de instellingen en klik vervolgens op **openen** als u wilt openen een SSH-verbinding.
        5. Wanneer dat wordt gevraagd, aanmelden bij de server. Dit zal een SSH-sessie tot stand brengen en inschakelen van de tunnel.

8. Open een webbrowser en typ de volgende URL op basis van de poort die u hebt opgegeven voor de tunnel.

        http://localhost:8787/ 

9. U wordt gevraagd de SSH-gebruikersnaam en wachtwoord verbinding maken met het cluster opgeven. Als u een SSH-sleutel gebruikt tijdens het maken van het cluster, moet u het wachtwoord dat u hebt gemaakt in stap 5 hierboven.

    ![Verbinding maken met R-Studio] (./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "Een SSH-tunnel maken")

10. Als u wilt testen of de RStudio-installatie voltooid is, kunt u een test uitvoeren script dat wordt uitgevoerd R MapReduce en Spark taken gebaseerd op het cluster. Ga terug naar de SSH-console en voer de volgende opdrachten voor het downloaden van het testscript uit te voeren in de RStudio.

    * Als u een cluster Hadoop met R hebt gemaakt, kunt u deze opdracht gebruikt.
        
            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r

    * Als u een cluster Spark met R hebt gemaakt, kunt u deze opdracht gebruikt.

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r

11. RStudio ziet u de testscript dat u hebt gedownload. Dubbelklikt u op het bestand te openen, selecteert u de inhoud van het bestand en klik vervolgens op **uitvoeren**. Hier ziet u de uitvoer in **het consolevenster** .
 
    ![Test de installatie] (./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "Test de installatie")

Een andere optie zou zijn om te typen `source(testhdi.r)` of `source(testhdi_spark.r)` voor het uitvoeren van het script.

## <a name="see-also"></a>Zie ook

- [Context opties voor R Server op clusters HDInsight berekenen](hdinsight-hadoop-r-server-compute-contexts.md)

- [Azure opslagopties R Server op HDInsight premium](hdinsight-hadoop-r-server-storage.md)


 
