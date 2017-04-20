<properties
    pageTitle="Het gebruik van de invoegtoepassing Azure slave met continue integratie Jenkins | Microsoft Azure"
    description="Beschrijving van het gebruik van de invoegtoepassing Azure slave met continue integratie Jenkins."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

# <a name="how-to-use-the-azure-slave-plug-in-with-jenkins-continuous-integration"></a>Het gebruik van de invoegtoepassing Azure slave met continue integratie Jenkins

U kunt de invoegtoepassing Azure slave voor Jenkins bepaling slave knooppunten op Azure bij uitvoeren van gedistribueerde samenstellen.

## <a name="install-the-azure-slave-plug-in"></a>Installeer de invoegtoepassing Azure slave

1. Klik in het dashboard Jenkins **Jenkins beheren**.

1. Klik op de pagina **Jenkins beheren** **Invoegtoepassingen beheren**.

1. Klik op het tabblad **beschikbaar** .

1. Typ in het filterveld boven de lijst met beschikbare invoegtoepassingen, **Azure** om de lijst beperken tot de desbetreffende plug-ins.

    Als u ervoor kiezen om te bladeren door de lijst met beschikbare invoegtoepassingen, vindt u de Azure slave invoegtoepassing onder de sectie **Clusterbeheer en gedistribueerd** .

1. Schakel het selectievakje **Azure Slave Plugin** .

1. Klik op **installeren zonder opnieuw op te starten** of **nu downloaden en installeren na de herstart**.

Nu dat de invoegtoepassing is geïnstalleerd, zijn de volgende stappen voor het configureren van de plug-in met uw profiel Azure abonnement en een sjabloon te maken die wordt gebruikt bij het maken van de virtuele machine voor het slave-knooppunt.


## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a>De invoegtoepassing Azure slave configureren met het profiel van uw abonnement

Een abonnement profiel, ook bekend als de publicatie-instellingen, is een XML-bestand met veilige referenties en aanvullende gegevens u moet werken met Azure in uw ontwikkelomgeving. De invoegtoepassing Azure slave configureren, hebt u het volgende nodig:

* Uw abonnement-id
* Een certificaat voor uw abonnement

Deze vindt u in uw [profiel abonnement]. Hieronder wordt een voorbeeld van een abonnement.

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

        <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

            ServiceManagementUrl="https://management.core.windows.net"

            Id="<Subscription ID value>"

            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

        </PublishProfile>

    </PublishData>

Nadat u het profiel van uw abonnement hebt, volgt u de invoegtoepassing Azure slave configureren:

1. Klik in het dashboard Jenkins **Jenkins beheren**.

1. Klik op het **systeem te configureren**.

1. Blader door de pagina naar het gedeelte van de **wolk** .

1. Klik op **nieuwe cloud toevoegen > Microsoft Azure**.

    ![cloud-sectie][cloud section]

    Hier ziet de velden waarin u moet invoeren, de details van uw abonnement.

    ![configuratie van abonnement][subscription configuration]

1. De abonnement-id en het beheer van certificaten waarden uit uw abonnement profiel kopiëren en plakken in de juiste velden.

    Neem niet de aanhalingstekens die de waarden bij het kopiëren van de abonnement-id en het beheer van het certificaat.

1. Klik op **configuratie controleren**.

1. Wanneer de configuratie correct is geverifieerd, klikt u op **Opslaan**.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Een virtuele machine sjabloon instellen voor de Azure slave invoegtoepassing

Een virtuele machine sjabloon definieert de parameters die de invoegtoepassing wordt gebruikt voor het maken van een knooppunt slave op Azure. In de volgende stappen maakt een sjabloon voor een virtuele machine van Ubuntu.

1. Klik in het dashboard Jenkins **Jenkins beheren**.

1. Klik op het **systeem te configureren**.

1. Blader door de pagina naar het gedeelte van de **wolk** .

1. In de sectie **Cloud** **Azure Virtual Machine**sjabloon toevoegen en klik vervolgens op **toevoegen**.

    ![vm-sjabloon toevoegen][add vm template]

    Hier ziet de velden waarin u gegevens invoeren over de sjabloon die u maakt.

    ![lege algemene configuratie][blank general configuration]

1. Voer in het vak **naam** een naam Azure cloud-service. Als de opgegeven naam naar een bestaande cloud-service verwijst, wordt de virtuele machine worden ingericht in deze service. Azure wordt anders een nieuwe maken.

1. Voer in het vak **Beschrijving** de beschrijving van de sjabloon die u maakt. Dit is alleen voor uw administratie en wordt niet gebruikt in een virtuele machine wordt ingericht.

1. Het vak **Labels** wordt gebruikt ter identificatie van de sjabloon die u maakt en wordt vervolgens gebruikt om te verwijzen naar de sjabloon bij het maken van een taak Jenkins. Voer voor ons doel, **linux** in dit vak.

1. Klik in de lijst **regio** op het gebied waar de virtuele machine wordt gemaakt.

1. Klik in de lijst **Grootte van de virtuele Machine** op de juiste grootte.

1. Geef in het vak **Accountnaam opslag** een opslag account waar u de virtuele machine wordt gemaakt. Zorg ervoor dat deze zich in hetzelfde gebied, als de cloud-service die u wilt gebruiken. Als u wilt dat nieuwe opslag moet worden gemaakt, kunt u dit vak leeg laat.

1. Retentietijd geeft het aantal minuten voordat Jenkins een niet-actieve slave wordt verwijderd. Dit laat de standaardwaarde van 60. U kunt ook de slave niet te verwijderen als deze niet actief is afgesloten. Schakel het selectievakje **Alleen-afsluiten (niet verwijderen) na de retentietijd** daarvoor.

1. In de lijst **Gebruik** , klikt u op de gewenste voorwaarde wanneer dit knooppunt slave wordt gebruikt. Voor deze oefening op **dit knooppunt zoveel mogelijk Utilize**.

    Op dit moment ziet uw formulier er enigszins vergelijkbaar met dit:

    ![CheckPoint algemene sjabloon config][checkpoint general template config]

    De volgende stap is om details over de installatiekopie van het besturingssysteem die u wilt dat uw slave moeten worden gemaakt.

1. In het vak **afbeelding- of -Id** die u moet opgeven welke installatiekopie wordt geïnstalleerd op de virtuele machine. Selecteren uit een lijst van families van de afbeelding of een aangepaste installatiekopie opgeven.

    Als u selecteren uit een lijst van families van de afbeelding wilt, voer het eerste teken (hoofdlettergevoelig) van de familienaam van de afbeelding. Bijvoorbeeld, krijgt typt **U** een lijst van families van Ubuntu Server. Nadat u de lijst hebt geselecteerd, gebruikt Jenkins de meest recente versie van deze systeemkopie van die familie tijdens het inrichten van uw virtuele machine.

    ![Lijst met voorbeelden van installatiekopie van het besturingssysteem][OS Image list sample]

    Als u een aangepaste afbeelding die u wilt gebruiken, voert u de naam van een aangepaste afbeelding. Aangepaste afbeelding namen worden niet weergegeven in een lijst, zodat u ervoor zorgen moet dat de naam juist is ingevoerd.

    Voor deze zelfstudie, typt **U** om een lijst met afbeeldingen in Ubuntu en klik vervolgens op **Ubuntu Server 14.04 LTS**.

1. Klik in de lijst **Methode starten** **SSH**.

1. Het onderstaande script kopiëren en plakken in het **Init Script** .

        # Install Java

        sudo apt-get -y update

        sudo apt-get install -y openjdk-7-jdk

        sudo apt-get -y update --fix-missing

        sudo apt-get install -y openjdk-7-jdk

        # Install git

        sudo apt-get install -y git

        #Install ant

        sudo apt-get install -y ant

        sudo apt-get -y update --fix-missing

        sudo apt-get install -y ant

    De init-script wordt uitgevoerd nadat u de virtuele machine hebt gemaakt. In dit voorbeeld wordt installeert het script Java en Git ant.

1. Voer de gewenste waarden voor de administrator-account die wordt gemaakt op de virtuele machine in de vakken **gebruikersnaam** en **wachtwoord** .

1. Klik op **Sjabloon controleren** om te controleren of de opgegeven parameters geldig zijn.

1. Klik op **Opslaan**.


## <a name="create-a-jenkins-job-that-runs-on-a-slave-node-on-azure"></a>Maak een Jenkins-taak die wordt uitgevoerd op een knooppunt slave op Azure

In dit gedeelte maakt u een Jenkins-taak die wordt uitgevoerd op een knooppunt slave op Azure. U moet met uw eigen project van op GitHub te volgen.

1. Klik op **Nieuw Item**in het dashboard Jenkins.

1. Voer een naam voor de taak die u wilt maken.

1. Klik op **Freestyle-project**voor het projecttype.

1. Klik op **Ok**.

1. Selecteer in de configuratiepagina van taak **beperken waarin dit project kan worden uitgevoerd**.

1. Voer in het vak **Label expressie** **linux**. In de vorige sectie wij een slave-sjabloon dat we met de naam **linux**, dat is wat we hier hebt opgegeven.

1. In de sectie **maken** op **toevoegen stap opbouwen** en selecteer **shell uitvoeren**.

1. Het volgende script **(uw accountnaam GitHub)**, **(de naam van het project)**en **(uw project directory)** vervangen door de juiste waarden bewerken en plak het script bewerkt in het gebied dat wordt weergegeven.

        # Clone from git repo

        currentDir="$PWD"

        if [ -e (your project directory) ]; then

            cd (your project directory)

            git pull origin master

        else

            git clone https://github.com/(your GitHub account name)/(your project name).git

        fi

        # change directory to project

        cd $currentDir/(your project directory)

        #Execute build task

        ant

1. Klik op **Opslaan**.

1. Plaats de muisaanwijzer op de taak die u zojuist hebt gemaakt en klik op de pijl-omlaag om opties voor taken weer te geven in het dashboard Jenkins.

1. Klik op **nu samenstellen**.

Jenkins wordt vervolgens een knooppunt slave maken met behulp van de sjabloon die in de vorige sectie hebt gemaakt en het script dat u hebt opgegeven in de stap maken voor deze taak uitvoeren.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[profiel van abonnement]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[cloud section]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-cloud-section.png
[subscription configuration]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-account-configuration-fields.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-add-vm-template.png
[blank general configuration]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration-blank.png
[checkpoint general template config]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration.png
[OS Image list sample]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-os-family-list-sample.png