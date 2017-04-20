<properties
    pageTitle="Ruby op Rails website op een Linux VM host | Microsoft Azure"
    description="Instellen en hosten van een Ruby op Rails-gebaseerde website op een virtuele Linux machine met Azure."
    services="virtual-machines-linux"
    documentationCenter="ruby"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="web"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="ruby-on-rails-web-application-on-an-azure-vm"></a>Ruby op Rails webtoepassing op een Azure VM

Deze zelfstudie laat zien hoe Ruby op Rails website op Azure hosten met behulp van een Linux virtuele machine.  

Deze zelfstudie is gevalideerd met behulp van Ubuntu Server 14.04 LTS. Als u een andere Linux-distributie, moet u mogelijk de stappen voor het installeren van de Rails wijzigen.

> [AZURE.IMPORTANT] Azure heeft twee verschillende modellen voor het maken en werken met resources: [Resource Manager en klassiek](../../../resource-manager-deployment-model.md).  In dit artikel beschreven hoe u met de klassieke implementatiemodel. Microsoft raadt de meeste nieuwe implementaties gebruiken het model Resource Manager.

## <a name="create-an-azure-vm"></a>Maak een Azure VM

Start een Azure VM met een Linux-installatiekopie maken.

U maakt de VM, kunt u de klassieke Azure portal of Azure opdrachtregelinterface (CLI).

### <a name="azure-management-portal"></a>Azure Management Portal

1. Inloggen op de [Azure klassieke portal](http://manage.windowsazure.com)
2. Klik op **nieuwe** > **berekenen** > **Virtual Machine** > **snel tabellen maken**. Selecteer een afbeelding met Linux.
3. Geef een wachtwoord.

Nadat de VM is ingericht, klikt u op de naam van de VM en op **Dashboard**. Het eindpunt van SSH, vermeld onder **SSH Details**vinden.

### <a name="azure-cli"></a>Azure CLI

Volg de stappen in het [maken van een virtuele Machine wordt uitgevoerd op Linux][vm-instructions].

Nadat de VM is ingericht, kunt u het eindpunt SSH opvragen door de volgende opdracht uit te voeren:

    azure vm endpoint list <vm-name>  

## <a name="install-ruby-on-rails"></a>Installeer Ruby op Rails

1. Met SSH verbinding maken met de VM.

2. Gebruik de volgende opdrachten Ruby installeren op de VM uit de SSH-sessie:

        sudo apt-get update -y
        sudo apt-get upgrade -y
        sudo apt-get install ruby ruby-dev build-essential libsqlite3-dev zlib1g-dev nodejs -y

    De installatie kan enkele minuten duren. Wanneer deze is voltooid, gebruikt u de volgende opdracht controleren of Ruby is geïnstalleerd:

        ruby -v

    Hiermee wordt de versie van Ruby die is geïnstalleerd.

3. Gebruik de volgende opdracht op de Rails installeren:

        sudo gem install rails --no-rdoc --no-ri -V

    Gebruik--geen rdoc en--geen ri vlaggen overslaan de documentatie sneller is te installeren.
    Deze opdracht zal waarschijnlijk lang duren om uit te voeren, zodat -V toe te voegen, wordt informatie over de voortgang van de installatie weergegeven.

## <a name="create-and-run-an-app"></a>Maken en uitvoeren van een app

Hoewel nog steeds aangemeld via SSH, voer de volgende opdrachten:

    rails new myapp
    cd myapp
    rails server -b 0.0.0.0 -p 3000

De [nieuwe](http://guides.rubyonrails.org/command_line.html#rails-new) opdracht maakt u een nieuwe Rails app. De [server](http://guides.rubyonrails.org/command_line.html#rails-server) opdracht start de WEBrick webserver die deel van de Rails uitmaakt. (Voor gebruik in productie, u waarschijnlijk wilt gebruiken van een andere server, zoals Eenhoorn of passagier.)

Hier ziet u uitvoer van de volgende strekking.

    => Booting WEBrick
    => Rails 4.2.1 application starting in development on http://0.0.0.0:3000
    => Run `rails server -h` for more startup options
    => Ctrl-C to shutdown server
    [2015-06-09 23:34:23] INFO  WEBrick 1.3.1
    [2015-06-09 23:34:23] INFO  ruby 1.9.3 (2013-11-22) [x86_64-linux]
    [2015-06-09 23:34:23] INFO  WEBrick::HTTPServer#start: pid=27766 port=3000

## <a name="add-an-endpoint"></a>Een eindpunt toevoegen

1. Ga naar de [Azure klassieke portal] [ management-portal] en selecteer uw VM.

    ![lijst van de virtuele machine][vmlist]

2. Selecteer de **EINDPUNTEN** aan de bovenkant van de pagina en klik op **+ EINDPUNT toevoegen** onder aan de pagina.

    ![pagina Tunneleindpunten][endpoints]

3. Selecteer 'Het eindpunt van een zelfstandige toevoegen' in het dialoogvenster **Toevoegen EINDPUNT** en klikt u op de pijl **volgende** gemarkeerd.

    ![dialoogvenster Nieuwe eindpunt][new-endpoint1]

3. In de volgende pagina van het dialoogvenster de volgende gegevens invoeren:

    * **Naam**: HTTP

    * **PROTOCOL**: TCP

    * **Openbare poort**: 80

    * **Particuliere poort**: 3000

    Hiermee maakt u een openbare poort 80 die verkeer naar de private poort van 3000 doorsturen, waar de Rails server luistert.

    ![dialoogvenster Nieuwe eindpunt][new-endpoint]

4. Klik op het selectievakje om op te slaan van het eindpunt.

5. Een bericht moet worden weergegeven dat **UPDATE IN uitvoering**. Als dit bericht verdwijnt, is het eindpunt actief. U kan nu uw toepassing testen door te navigeren naar de DNS-naam van de virtuele machine. De website moet worden met de volgende strekking weergegeven:

    ![standaardpagina van rails][default-rails-cloud]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u de meeste van de stappen handmatig. In een productieomgeving, zou u uw app schrijven op een andere machine en deze implementeren voor Azure VM. Ook hosttoepassing de meeste productieomgevingen de Rails in combinatie met een andere server-proces bijvoorbeeld Apache of NginX, welke ingangen bronnen van statische routering naar meerdere exemplaren van de toepassing van de Rails en op aanvraag. Zie http://rubyonrails.org/deploy/ voor meer informatie.

Bezoek voor meer informatie over Ruby op Rails, [Ruby op Rails hulplijnen]van de[rails-guides].

Gebruik van Azure services vanuit uw toepassing Ruby, Zie:

* [Ongestructureerde gegevens met behulp van BLOB's opslaan][blobs]

* [Winkel sleutel/waarde-paren met behulp van tabellen][tables]

* [Inhoud van hoge bandbreedte met het netwerk van inhoud levering][cdn-howto]

<!-- WA.com links -->
[blobs]: ../../../storage/storage-ruby-how-to-use-blob-storage.md
[cdn-howto]: https://azure.microsoft.com/develop/ruby/app-services/
[management-portal]: https://manage.windowsazure.com/
[tables]: ../../../storage/storage-ruby-how-to-use-table-storage.md
[vm-instructions]: ../../virtual-machines-linux-classic-createportal.md

<!-- External Links -->
[rails-guides]: http://guides.rubyonrails.org/
[sqlite3]: http://www.sqlite.org/

<!-- Images -->

[default-rails-cloud]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/basicrailscloud.png
[vmlist]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/vmlist.png
[endpoints]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/endpoints.png
[new-endpoint]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint.png
[new-endpoint1]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint1.png
