<properties
    pageTitle="Het gebruik van de invoegtoepassing Azure slave met continue integratie Hudson | Microsoft Azure"
    description="Beschrijving van het gebruik van de invoegtoepassing Azure slave met continue integratie van Hudson."
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

# <a name="how-to-use-the-azure-slave-plug-in-with-hudson-continuous-integration"></a>Het gebruik van de invoegtoepassing Azure slave met continue integratie Hudson

De invoegtoepassing voor Hudson Azure slave kunt u knooppunten op Azure slave inrichten bij uitvoeren van gedistribueerde samenstellen.

## <a name="install-the-azure-slave-plug-in"></a>Installeer de invoegtoepassing Azure-Slave

1. Klik in het dashboard Hudson **Hudson beheren**.

1. Klik op **Invoegtoepassingen beheren**op de pagina **Hudson beheren** .

1. Klik op het tabblad **beschikbaar** .

1. Klik op **Zoeken** en typ **Azure** om de lijst beperken tot de desbetreffende plug-ins.

    Als u ervoor kiezen om te bladeren door de lijst met beschikbare invoegtoepassingen, vindt u de Azure slave invoegtoepassing onder de sectie **Clusterbeheer en verdeeld** in het **andere** tabblad.

1. Schakel het selectievakje voor **Azure Slave Plugin**.

1. Klik op **installeren**.

1. Hudson opnieuw.

Nu dat de invoegtoepassing is geïnstalleerd, worden de volgende stappen voor het configureren van de plug-in met uw profiel Azure abonnement en een sjabloon te maken die wordt gebruikt bij het maken van de VM voor het slave-knooppunt.

## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a>De invoegtoepassing Azure-Slave configureren met het profiel van uw abonnement

Een abonnement profiel, ook bekend als de publicatie-instellingen, is een XML-bestand met veilige referenties en aanvullende gegevens u moet werken met Azure in uw ontwikkelomgeving. De invoegtoepassing Azure slave configureren, hebt u het volgende nodig:

* Uw abonnement-id
* Een certificaat voor uw abonnement

Deze vindt u in uw [profiel abonnement]. Hieronder wordt een voorbeeld van een abonnement.

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

        <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

            ServiceManagementUrl="https://management.core.windows.net"

            Id="<Subscription ID>"

            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

        </PublishProfile>

    </PublishData>

Zodra u uw abonnement profiel hebt, gaat u als volgt te werk om de invoegtoepassing Azure slave.

1. Klik in het dashboard Hudson **Hudson beheren**.

1. Klik op het **systeem te configureren**.

1. Blader door de pagina naar het gedeelte van de **wolk** .

1. Klik op **nieuwe cloud toevoegen > Microsoft Azure**.

    ![toevoegen van nieuwe effecten met wolken][add new cloud]

    Hier ziet de velden waarin u moet invoeren, de details van uw abonnement.

    ![profiel configureren][configure profile]

1. De abonnement-id en het beheer van het certificaat van uw abonnement profiel kopiëren en plak deze in de juiste velden.

    Bij het kopiëren van de abonnement-id en het beheer van het certificaat bevatten **niet** de aanhalingstekens die de waarden.

1. Klik op **configuratie controleren**.

1. Wanneer de configuratie is met succes geverifieerd, klikt u op **Opslaan**.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Een virtuele machine sjabloon instellen voor de Slave Azure invoegtoepassing

Een virtuele machine sjabloon definieert de parameters die de invoegtoepassing wordt gebruikt voor het maken van een knooppunt slave op Azure. In de volgende stappen maakt we sjabloon voor een Ubuntu VM.

1. Klik in het dashboard Hudson **Hudson beheren**.

1. Klik op het **systeem te configureren**.

1. Blader door de pagina naar het gedeelte van de **wolk** .

1. In de sectie **wolk** **Azure Virtual Machine** sjabloon toevoegen en klikt u op de knop **toevoegen** .

    ![vm-sjabloon toevoegen][add vm template]

1. De naam van een cloud-service opgeven in het veld **naam** . Als u naar een bestaande cloud-service verwijst, wordt de VM in die service worden ingericht. Azure wordt anders een nieuwe maken.

1. Voer in het veld **Omschrijving** beschrijving van de sjabloon die u maakt. Deze informatie is uitsluitend bestemd voor administratieve doeleinden en wordt niet gebruikt in een VM wordt ingericht.

1. Voer in het veld **Labels** **linux**. Dit label wordt gebruikt ter identificatie van de sjabloon die u maakt en wordt vervolgens gebruikt om te verwijzen naar de sjabloon bij het maken van een taak van de Hudson.

1. Selecteer een regio waar de VM worden gemaakt.

1. Selecteer de gewenste grootte van de VM.

1. Geef een opslag account waar de VM worden gemaakt. Zorg ervoor dat deze zich in hetzelfde gebied, als de cloud-service die u wilt gebruiken. Als u wilt dat nieuwe opslag moet worden gemaakt, kunt u dit veld leeg laten.

1. Retentietijd geeft het aantal minuten voordat de Hudson een niet-actieve slave wordt verwijderd. Dit laat de standaardwaarde van 60.

1. In **Gebruik**, selecteert u de desbetreffende voorwaarde wanneer dit knooppunt slave wordt gebruikt. Selecteer nu **Utilize dit zo veel mogelijk knooppunt**.

    Op dit moment er uw formulier enigszins vergelijkbaar met dit:

    ![sjabloon-config][template config]

1. In de **familie van de afbeelding of de Id** die u moet opgeven welke installatiekopie wordt geïnstalleerd op uw VM. Selecteren uit een lijst van families van de afbeelding of een aangepaste installatiekopie opgeven.

    Als u selecteren uit een lijst van families van de afbeelding wilt, voer het eerste teken (hoofdlettergevoelig) van de familienaam van de afbeelding. Bijvoorbeeld, krijgt typt **U** een lijst van families van Ubuntu Server. Als u in de lijst selecteert, gebruikt Jenkins de meest recente versie van deze systeemkopie van die familie tijdens het inrichten van uw VM.

    ![OS-familie lijst][OS family list]

    Als u een aangepaste afbeelding die u wilt gebruiken, voert u de naam van een aangepaste afbeelding. Aangepaste afbeelding namen worden niet weergegeven in een lijst hebben om ervoor te zorgen dat de naam juist is ingevoerd.    

    Voor deze zelfstudie, typt **U** een lijst met afbeeldingen in Ubuntu en selecteer **Ubuntu Server 14.04 LTS**.

1. Voor de **methode starten**, selecteert u **SSH**.

1. Het onderstaande script kopiëren en plakken in het veld **Init script** .

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

    De **Init-script** wordt uitgevoerd nadat de VM is gemaakt. In dit voorbeeld wordt installeert het script Java en git ant.

1. Voer in de velden **gebruikersnaam** en **wachtwoord** de gewenste waarden voor de administrator-account die wordt gemaakt op de VM.

1. Klik op **Sjabloon controleren** om te controleren of de opgegeven parameters geldig zijn.

1. Klik op **Opslaan**.

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a>Maak een Hudson-taak die wordt uitgevoerd op een knooppunt slave op Azure

In dit gedeelte maakt u een Hudson-taak die wordt uitgevoerd op een knooppunt slave op Azure.

1. Klik op **Nieuwe taak**in het dashboard Hudson.

1. Voer een naam voor de taak die u wilt maken.

1. Selecteer **een taak vrije stijl software te bouwen**voor het taaktype.

1. Klik op **OK**.

1. Selecteer in de configuratiepagina van taak **beperken waarin dit project kan worden uitgevoerd**.

1. **Knooppunt- en** op het menu en selecteer **linux** (we opgegeven dit label bij het maken van de virtuele machine-sjabloon in de vorige sectie).

1. In de sectie **maken** op **toevoegen stap opbouwen** en selecteer **shell uitvoeren**.

1. Het volgende script **{uw accountnaam github}**, **{uw naam}**en **{uw project directory}** vervangen door de juiste waarden bewerken en plak het script bewerkt in het gebied dat wordt weergegeven.

        # Clone from git repo

        currentDir="$PWD"

        if [ -e {your project directory} ]; then

            cd {your project directory}

            git pull origin master

        else

            git clone https://github.com/{your github account name}/{your project name}.git

        fi

        # change directory to project

        cd $currentDir/{your project directory}

        #Execute build task

        ant

1. Klik op **Opslaan**.

1. Zoek in het dashboard Hudson de taak die u zojuist hebt gemaakt en klik op het pictogram van **een build van het schema** .

Hudson vervolgens maakt u een slave-knooppunt met de sjabloon die in de vorige sectie hebt gemaakt en het script dat u hebt opgegeven in de stap maken voor deze taak uitvoeren.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[profiel van abonnement]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png

