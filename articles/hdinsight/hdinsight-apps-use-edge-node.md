<properties
    pageTitle="Lege rand knooppunten gebruiken in HDInsight | Microsoft Azure"
    description="Hoe een randknooppunt ampty toevoegen aan een cluster van HDInsight die kan worden gebruikt als een client en test/host uw HDInsight toepassingen."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="use-empty-edge-nodes-in-hdinsight"></a>Gebruik van lege rand nodes in HDInsight

Informatie over hoe u een lege randknooppunt toevoegen aan een cluster van Linux-gebaseerde HDInsight. Een randknooppunt leeg is een virtuele Linux machine met de dezelfde hulpprogramma's geïnstalleerd en geconfigureerd als in de headnodes, maar geen hadoop-services die worden uitgevoerd. U kunt de randknooppunt voor toegang tot het cluster, uw client-toepassingen testen en uw client-toepassingen die als host fungeert. 

U kunt een lege randknooppunt toevoegen aan een bestaand cluster HDInsight om een nieuw cluster als u het cluster maakt. Toevoegen van een randknooppunt leeg wordt gedaan met behulp van bronbeheer Azure sjabloon.  In het volgende voorbeeld laat zien hoe dit gebeurt met behulp van een sjabloon:

    "resources": [
        {
            "name": "[concat(parameters('clusterName'),'/', variables('applicationName'))]",
            "type": "Microsoft.HDInsight/clusters/applications",
            "apiVersion": "[variables('clusterApiVersion')]",
            "properties": {
                "marketPlaceIdentifier": "EmptyNode",
                "computeProfile": {
                    "roles": [{
                        "name": "edgenode",
                        "targetInstanceCount": 1,
                        "hardwareProfile": {
                            "vmSize": "[parameters('edgeNodeSize')]"
                        }
                    }]
                },
                "installScriptActions": [{
                    "name": "[concat('emptynode','-' ,uniquestring(variables('applicationName')))]",
                    "uri": "https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh",
                    "roles": ["edgenode"]
                }],
                "uninstallScriptActions": [],
                "httpsEndpoints": [],
                "applicationType": "CustomApplication"
            }
        }
    ],

Zoals in het voorbeeld, kunt u desgewenst een [scriptactie](hdinsight-hadoop-customize-cluster-linux.md) voor het uitvoeren van aanvullende configuratie, zoals het installeren van [Apache kleurtoon](hdinsight-hadoop-hue-linux.md) in het randknooppunt aanroepen.

Nadat u een randknooppunt hebt gemaakt, kunt u verbinding maken met de randknooppunt via SSH en clienthulpprogramma's voor toegang tot het cluster Hadoop in HDInsight uitgevoerd.

## <a name="add-an-edge-node-to-an-existing-cluster"></a>Een randknooppunt toevoegen aan een bestaand cluster

In dit gedeelte kunt u een sjabloon bronnenbeheerder een randknooppunt toevoegen aan een bestaand cluster van HDInsight.  De sjabloon Resource Manager kan worden gevonden in de [GitHub](https://github.com/hdinsight/Iaas-Applications/tree/master/EmptyNode). De sjabloon Resource Manager roept een script actie script te vinden op https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh. Het script uitvoeren geen acties.  Dit is om aan te tonen in een sjabloon Resource Manager aanroepende scriptactie.

**Een randknooppunt leeg toevoegen aan een bestaand cluster**

1. Maak een cluster HDInsight als u nog geen hebt.  Zie [Hadoop zelfstudie: aan de slag met Linux-gebaseerde Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Klik op de onderstaande afbeelding om te melden bij Azure en open de sjabloon Azure Resource Manager in de portal Azure. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2FIaas-Applications%2Fmaster%2FEmptyNode%2Fazuredeploy.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. Configureer de volgende eigenschappen:

    - CLUSTERNAAM: Voer de naam van een bestaand cluster van HDInsight.
    - EDGENODESIZE: Selecteer een van de formaten VM.
    - EDGENODEPREFIX: De standaardwaarde is **Nieuw**.  De standaardwaarde wordt gebruikt, is de knooppuntnaam rand **nieuwe edgenode**.  U kunt het voorvoegsel van de portal aanpassen. U kunt ook de volledige naam van de sjabloon aanpassen.


4. Klik op **OK** om de wijzigingen opslaan.
5. Selecteer een resourcegroep in de **resourcegroep**.
6. Klik op **de juridische voorwaarden controleren**en klik vervolgens op **Inkoop**.
7. **Pin om dashboard te**selecteren en klik vervolgens op **maken**.

## <a name="add-an-edge-node-when-creating-a-cluster"></a>Een randknooppunt toevoegen bij het maken van een cluster

In dit gedeelte kunt u een sjabloon Resource Manager HDInsight cluster maken met een randknooppunt.  De bronnenbeheerder sjabloon vindt u in een openbare [Azure Blob opslag container](http://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json). De sjabloon Resource Manager roept een script actie script te vinden op https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh. Het script uitvoeren geen acties.  Dit is om aan te tonen in een sjabloon Resource Manager aanroepende scriptactie.

**Een randknooppunt leeg toevoegen aan een bestaand cluster**

1. Maak een cluster HDInsight als u nog geen hebt.  Zie [Hadoop zelfstudie: aan de slag met Linux-gebaseerde Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Klik op de onderstaande afbeelding om te melden bij Azure en open de sjabloon Azure Resource Manager in de portal Azure. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. Configureer de volgende eigenschappen:
        
    - CLUSTERNAAM: Voer een naam voor het nieuwe cluster te maken.
    - CLUSTERLOGINUSERNAME: Voer de gebruikersnaam van de HTTP-Hadoop.  De standaardnaam is **admin**.
    - CLUSTERLOGINPASSWORD: Voer het wachtwoord van de gebruiker Hadoop HTTP.
    - SSHUSERNAME: Geef de naam van de gebruiker SSH. De standaardnaam is **sshuser**.
    - SSHPASSWORD: Voer het wachtwoord van de gebruiker SSH.
    - LOCATIE: Geef de locatie van de naam van de Resource, het cluster en de standaardaccount voor opslag.
    - CLUSTERTYPE: De standaardwaarde is **hadoop**.
    - CLUSTERWORKERNODECOUNT: De standaardwaarde is 2.
    - EDGENODESIZE: Selecteer een van de formaten VM.
    - EDGENODEPREFIX: De standaardwaarde is **Nieuw**.  De standaardwaarde wordt gebruikt, is de knooppuntnaam rand **nieuwe edgenode**.  U kunt het voorvoegsel van de portal aanpassen. U kunt ook de volledige naam van de sjabloon aanpassen.

4. Klik op **OK** om de wijzigingen opslaan.
5. Voer een nieuwe naam voor de resourcegroep in de **resourcegroep**.
6. Klik op **de juridische voorwaarden controleren**en klik vervolgens op **Inkoop**.
7. **Pin om dashboard te**selecteren en klik vervolgens op **maken**. 


## <a name="access-an-edge-node"></a>Toegang tot een randknooppunt

Het randknooppunt ssh eindpunt is <EdgeNodeName>. <ClusterName>-ssh.azurehdinsight.net:22.  Bijvoorbeeld: nieuwe-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.

Het randknooppunt wordt weergegeven als een toepassing op de Azure portal.  De portal kunt u de gegevens voor toegang tot de randknooppunt via SSH.

**Om te controleren of het eindpunt rand knooppunt SSH**

1. Aanmelden op de [portal Azure](https://portal.azure.com).
2. Open het cluster HDInsight met een randknooppunt.
3. Klik op **toepassingen** in het cluster blade. U dient de randknooppunt zien.  De standaardnaam is de **nieuwe edgenode**.
4. Klik op het randknooppunt. U dient het eindpunt SSH zien.

**Component op het randknooppunt gebruiken**

5. Met SSH verbinding maken met het randknooppunt.  Zie [Gebruik SSH met Linux-gebaseerde Hadoop op HDInsight van Linux, Unix, of OS X](hdinsight-hadoop-linux-use-ssh-unix.md) of [SSH met Linux-gebaseerde Hadoop op HDInsight van Windows gebruiken](hdinsight-hadoop-linux-use-ssh-windows.md).
6. Nadat u hebt aangesloten op de randknooppunt via SSH, gebruikt u de volgende opdracht de component console openen:

        hive
7. Voer de volgende opdracht om de component tabellen weergeven in het cluster:

        show tables;

## <a name="delete-an-edge-node"></a>Een randknooppunt verwijderen

U kunt een randknooppunt van het Azure portal verwijderen.

**Toegang tot een randknooppunt**

1. Aanmelden op de [portal Azure](https://portal.azure.com).
2. Open het cluster HDInsight met een randknooppunt.
3. Klik op **toepassingen** in het cluster blade. Er wordt een lijst met knooppunten rand.  
4. Klik met de rechtermuisknop op de randknooppunt dat u wilt verwijderen en klik vervolgens op **verwijderen**.
5. Klik op **Ja** om te bevestigen.

## <a name="next-steps"></a>Volgende stappen

In dit artikel, kunt u het toevoegen van een randknooppunt en hoe u toegang tot het randknooppunt hebt geleerd. Voor meer informatie, Zie de volgende artikelen:

- [Toepassingen installeren HDInsight](hdinsight-apps-install-applications.md): informatie over het installeren van een toepassing HDInsight clusters.
- [Aangepaste toepassingen voor HDInsight](hdinsight-apps-install-custom-applications.md): informatie over het implementeren van een niet-gepubliceerde HDInsight toepassing HDInsight.
- [Toepassingen publiceren HDInsight](hdinsight-apps-publish-applications.md): informatie over het publiceren van uw aangepaste HDInsight toepassingen met Azure Marketplace.
- [MSDN: installeren van een toepassing HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): informatie over het definiëren van HDInsight toepassingen.
- [HDInsight aanpassen Linux gebaseerde clusters met scriptactie](hdinsight-hadoop-customize-cluster-linux.md): informatie over het gebruik van de actie Script om extra toepassingen te installeren.
- [Hadoop maken Linux gebaseerde clusters in HDInsight met behulp van bronbeheer sjablonen](hdinsight-hadoop-create-linux-clusters-arm-templates.md): informatie over het aanroepen van HDInsight clusters maken sjablonen Resource Manager.

