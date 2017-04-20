<properties
    pageTitle="Linux en Open-Source op Azure Computing | Microsoft Azure"
    description="Een lijst met Linux en Open-Source Computing artikelen op Azure, met inbegrip van de voornaamste gebruiksmogelijkheden van Linux, sommige fundamentele concepten over uitvoeren of het uploaden van afbeeldingen op Azure en andere inhoud over specifieke technologieën en optimalisatie van Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="06/27/2016"
    ms.author="rasquill"/>



# <a name="linux-and-open-source-computing-on-azure"></a>Linux en open source computing op Azure

Zoeken naar alle documenten die u wilt maken en beheren van Linux-gebaseerde virtuele machines in de klassieke implementatiemodel.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="get-started"></a>Aan de slag
- [Inleiding voor Linux op Azure](virtual-machines-linux-intro-on-azure.md)
- [Veelgestelde vragen over Azure virtuele Machines die is gemaakt met het implementatiemodel klassiek](virtual-machines-linux-classic-faq.md)
- [Informatie over afbeeldingen voor virtuele machines](virtual-machines-linux-classic-about-images.md)
- [Uw eigen Distro afbeelding uploaden](virtual-machines-linux-classic-create-upload-vhd.md) (en ook instructies met behulp van een [Verdeling van de Azure-Endorsed](virtual-machines-linux-endorsed-distros.md))
- [Meld u aan bij een Linux VM met behulp van de klassieke Azure portal](virtual-machines-linux-mac-create-ssh-keys.md)

## <a name="set-up"></a>Instellen

- [Installeren van Azure opdrachtregelinterface (CLI Azure)](../xplat-cli-install.md)


## <a name="tutorials"></a>Zelfstudies

- [De LAMP Stack installeren op een virtuele Linux machine in Azure](virtual-machines-linux-create-lamp-stack.md)
- [Ruby op Rails webtoepassing op een Azure VM](linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)
- [Procedure: installatie van Apache Qpid Proton-C voor AMQP en Service Bus](../service-bus-messaging/service-bus-amqp-apache.md)

### <a name="databases"></a>Databases
- [Optimaliseren van de prestaties van MySQL op Azure](virtual-machines-linux-classic-optimize-mysql.md)
- [MySQL Clusters](virtual-machines-linux-classic-mysql-cluster.md)
- [Met Cassandra en Linux op Azure en benaderen van Node.js](virtual-machines-linux-classic-cassandra-nodejs.md)
- [Maak een cluster met meerdere masters van MariaDbs](virtual-machines-linux-classic-mariadb-mysql-cluster.md)

### <a name="hpc"></a>HPC
- [Aan de slag met Linux computerknooppunten in een cluster HPC Pack in Azure](virtual-machines-linux-classic-hpcpack-cluster.md)
- [NAMD uitvoeren met Microsoft HPC Pack op Linux-computerknooppunten in Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)
- [Instellen van een RDMA Linux cluster MPI-toepassingen uit te voeren](virtual-machines-linux-classic-rdma-cluster.md)

### <a name="docker"></a>Docker
- [Met de extensie Docker VM vanaf de opdrachtregel Azure Interface (CLI Azure)](virtual-machines-linux-classic-cli-use-docker.md)
- [Bestand met de extensie Docker VM vanaf de portal Azure](virtual-machines-linux-classic-portal-use-docker.md)
- [Het gebruik van docker machine op Azure](virtual-machines-linux-docker-machine.md)

### <a name="ubuntu"></a>Ubuntu
- [Procedure: MySQL Clusters](virtual-machines-linux-classic-mysql-cluster.md)
- [Procedure: Node.js en Cassandra](virtual-machines-linux-classic-cassandra-nodejs.md)

### <a name="opensuse"></a>OpenSUSE
- [Procedure: installeren en uitvoeren van MySQL](virtual-machines-linux-classic-mysql-on-opensuse.md)

### <a name="coreos"></a>CoreOS
- [Procedure: CoreOS op Azure gebruiken](https://coreos.com/os/docs/latest/booting-on-azure.html)


## <a name="planning"></a>Planning
- [Richtlijnen voor de implementatie van infrastructuur Azure services](virtual-machines-linux-infrastructure-subscription-accounts-guidelines.md)
- [Linux-gebruikersnamen te selecteren](virtual-machines-linux-usernames.md)
- [Het configureren van de beschikbaarheid voor virtuele machines in de klassieke implementatiemodel instellen](virtual-machines-linux-classic-configure-availability.md)
- [Het geplande onderhoud op Azure VMs plannen](virtual-machines-linux-planned-maintenance-schedule.md)
- [De beschikbaarheid van virtuele machines te beheren](virtual-machines-linux-manage-availability.md)
- [Planmatig onderhoud voor Linux virtuele machines in Azure](virtual-machines-linux-planned-maintenance.md)


## <a name="deployment"></a>Implementatie
- [Maak een aangepaste virtuele machine waarop Linux](virtual-machines-linux-classic-createportal.md)
- [De grondbeginselen: het vastleggen van een Linux VM een sjabloon wilt maken](virtual-machines-linux-classic-capture-image.md)
- [Informatie voor niet-geviseerd verdelingen](virtual-machines-linux-create-upload-generic.md)


## <a name="management"></a>Management

- [SSH](virtual-machines-linux-mac-create-ssh-keys.md)
- [Het opnieuw instellen van een wachtwoord of eigenschappen van SSH voor Linux](virtual-machines-linux-classic-reset-access.md)
- [Met behulp van Root](virtual-machines-linux-use-root-privileges.md)


## <a name="azure-resources"></a>Azure Resources

- [De Azure Linux-Agent](virtual-machines-linux-agent-user-guide.md)
- [Functies en uitbreidingen voor Azure VM](virtual-machines-windows-extensions-features.md)
- [Injecteren van aangepaste gegevens in een VM met Cloud-init](virtual-machines-windows-classic-inject-custom-data.md)


## <a name="storage"></a>Opslag

- [Een schijf met gegevens koppelen aan een Linux-VM](virtual-machines-linux-classic-attach-disk.md)
- [Loskoppelen van een schijf met gegevens uit een Linux VM](virtual-machines-linux-classic-detach-disk.md)
- [RAID](virtual-machines-linux-configure-raid.md)


## <a name="networking"></a>Netwerken
- [Het instellen van de eindpunten van een klassieke virtuele machine in Azure](virtual-machines-linux-classic-setup-endpoints.md)


## <a name="troubleshooting"></a>Het oplossen van problemen
- [Oplossen van problemen met verbindingen met een Linux-gebaseerde Azure virtuele machine Secure Shell (SSH)](virtual-machines-linux-troubleshoot-ssh-connection.md)
- [Oplossen van implementatieproblemen met het maken van een nieuwe virtuele machine voor Linux in Azure klassieke](virtual-machines-linux-classic-troubleshoot-deployment-new-vm.md)  
- [Klassieke implementatieproblemen oplossen met opnieuw op te starten of het formaat van een bestaande Linux virtuele Machine in Azure](virtual-machines-linux-classic-restart-resize-error-troubleshooting.md) 


## <a name="reference"></a>Referentie

- [Azure CLI-opdrachten in de modus Azure Service Management (asm)](../virtual-machines-command-line-tools.md)
- [Azure Service Management REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx)




## <a name="general-links"></a>Algemene Links
De volgende koppelingen zijn voor Microsoft blogs, Technet-pagina's en externe sites plaats Azure.com documentatie die hierboven is beschreven. Aangezien zowel Azure als de open source computerwereld snel bewegende doelen, is het vrijwel zeker van zijn dat de volgende koppelingen verouderd, *ondanks* het feit dat we ons best doen moeten om voortdurend nieuwe onderwerpen toevoegen en verwijderen van verouderde bestanden. Als we een hebt gemist, geef laat het ons weten in de opmerkingen of pull te vragen om onze [GitHub repo](https://github.com/Azure/azure-content/).

- [ASP.NET 5 uitgevoerd op Linux met behulp van Docker Containers](http://blogs.msdn.com/b/webdev/archive/2015/01/14/running-asp-net-5-applications-in-linux-containers-with-docker.aspx)
- [Het implementeren van een CentOS VM afbeelding van OpenLogic](https://azure.microsoft.com/blog/2013/01/11/deploying-openlogic-centos-images-on-windows-azure-virtual-machines/)
- [Infrastructuur van SUSE](https://forums.suse.com/showthread.php?5622-New-Update-Infrastructure)
- [SUSE Linux Enterprise Server voor SAP Cloud toestel bibliotheek](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver11sp3forsapcloudappliance/)
- [Maximaal beschikbare Linux gebouw op Azure in 12 stappen](http://blogs.technet.com/b/keithmayer/archive/2014/10/03/quick-start-guide-building-highly-available-linux-servers-in-the-cloud-on-microsoft-azure.aspx)
- [Linux wordt ingericht op Azure met Azure CLI, node.js, jhawk automatiseren](http://blogs.technet.com/b/keithmayer/archive/2014/11/24/step-by-step-automated-provisioning-for-linux-in-the-cloud-with-microsoft-azure-xplat-cli-json-and-node-js-part-1.aspx)
- [GlusterFS op Azure](http://dastouri.azurewebsites.net/gluster-on-azure-part-1/)

### <a name="freebsd"></a>FreeBSD
- [FreeBSD uitgevoerd in Azure](https://azure.microsoft.com/blog/2014/05/22/running-freebsd-in-azure/)
- [Eenvoudige implementatie van FreeBSD](http://msopentech.com/blog/2014/10/24/easy-deploy-freebsd-microsoft-azure-vm-depot/)
- [Implementatie van een aangepaste installatiekopie van FreeBSD](http://msopentech.com/blog/2014/05/14/deploy-customize-freebsd-virtual-machine-image-microsoft-azure/)
- [Kaspersky AV voor Linux File Server](https://azure.microsoft.com/marketplace/partners/kaspersky-lab/kav-for-lfs-kav-for-lfs/)

### <a name="nosql"></a>NoSQL

- [8 NoSql open source Databases voor Azure](http://openness.microsoft.com/blog/2014/11/03/open-source-nosql-databases-microsoft-azure/)
- [Slideshare (MSOpenTech): Ervaringen met CouchDb op Azure](http://www.slideshare.net/brianbenz/experiences-using-couchdb-inside-microsofts-azure-team)
- [CouchDB-as-a-Service met node.js, CORS en heidens wordt uitgevoerd](http://msopentech.com/blog/2013/12/19/tutorial-building-multi-tier-windows-azure-web-application-use-cloudants-couchdb-service-node-js-cors-grunt-2/)

- [Bestand voor Windows in de Cache Azure bestand Vgx. Service Vgx.](http://msopentech.com/blog/2014/05/12/redis-on-windows/)
- [ASP.NET-sessiestatus Provider aangekondigd voor Preview-versie bestand Vgx.](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)

- [Blog: RavenHQ nu beschikbaar in de markt Azure](https://azure.microsoft.com/blog/2014/08/12/ravenhq-now-available-in-the-azure-store/)

### <a name="big-data"></a>BIG Data
- [Installeren van Hadoop op Azure Linux VMs](http://blogs.msdn.com/b/benjguin/archive/2013/04/05/how-to-install-hadoop-on-windows-azure-linux-virtual-machines.aspx)
- [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/)

### <a name="relational-database"></a>Relationele database
- [MySQL-hoge beschikbaarheid-architectuur in Microsoft Azure](http://download.microsoft.com/download/6/1/C/61C0E37C-F252-4B33-9557-42B90BA3E472/MySQL_HADR_solution_in_Azure.pdf)
- [Installeren van Postgres met corosync, pg_bouncer met ILB](https://github.com/chgeuer/postgres-azure)

### <a name="linux-high-performance-computing-hpc"></a>Linux high performance computing (HPC)

- [Quickstart sjabloon: een cluster SLURM draaien](https://github.com/Azure/azure-quickstart-templates/tree/master/slurm) (en [blogberichten](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx))
- [QuickStart sjabloon: een HPC-cluster maken met Linux-computerknooppunten](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/)

### <a name="devops-management-and-optimization"></a>Devops, beheer en optimalisatie

Als de wereld van devops, beheer en optimalisatie heel omvangrijk en zeer snel te wijzigen, kunt u overwegen de onderstaande uitgangspunt.

- [Video: Azure virtuele Machines: met behulp van chef-kok, Puppet en Docker voor het beheren van Linux VMs](https://azure.microsoft.com/blog/2014/12/15/azure-virtual-machines-using-chef-puppet-and-docker-for-managing-linux-vms/)

- [Volledige handleiding voor de implementatie van geautomatiseerde Kubernetes cluster met CoreOS en weefpatroon](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
- [Kubernetes Visualizer](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)

- [Invoegtoepassing voor Azure Jenkins-Slave](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
- [GitHub repo: invoegtoepassing voor Azure Jenkins-opslag](https://github.com/jenkinsci/windows-azure-storage-plugin)

- [Derden: Invoegtoepassing voor Azure Hudson-Slave](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
- [Derden: Invoegtoepassing voor Azure Hudson-opslag](https://github.com/hudson3-plugins/windows-azure-storage-plugin)

- [Video: Wat is chef-kok en hoe werkt het?](https://msopentech.com/blog/2014/03/31/using-chef-to-manage-azure-resources/)

- [Video: Het gebruik van Azure automatisering met Linux VMs](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)

- [Blog: Hoe Powershell DSC voor Linux](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
- [GitHub: Docker Client DSC](https://github.com/anweiss/DockerClientDSC)

- [Invoegtoepassing voor Azure verpakker](https://github.com/msopentech/packer-azure)
