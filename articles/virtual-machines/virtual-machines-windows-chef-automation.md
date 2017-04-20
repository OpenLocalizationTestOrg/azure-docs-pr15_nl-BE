<properties
   pageTitle="Implementatie van Azure virtual machine met chef-kok | Microsoft Azure"
   description="Informatie over het gebruik van chef-kok VM geautomatiseerde implementatie en configuratie op Microsoft Azure"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="diegoviso"
   manager="timlt"
   tags="azure-service-management,azure-resource-manager"
   editor=""/>

<tags ms.service="virtual-machines-windows" ms.workload="infrastructure-services"
ms.tgt_pltfrm="vm-multiple"
ms.devlang="na"
ms.topic="article"
ms.date="05/19/2015"
ms.author="diviso"/>

# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Automatiseren van de implementatie van Azure virtual machine met chef-kok

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Chef-kok is een geweldig hulpmiddel voor het leveren van automatisering en de gewenste configuraties staat.

Onze meest recente versie van cloud api biedt chef-kok naadloze integratie met Azure, waardoor u kunt inrichten en implementeren van lidstaten configuratie via een enkele opdracht.

In dit artikel, ik, ziet u het instellen van uw chef-kok omgeving inrichten Azure virtuele machines en helpt u bij het maken van een beleid of "CookBook" en vervolgens dit kookboek implementeert op een Azure virtuele machine.

Laten we beginnen!

## <a name="chef-basics"></a>Grondbeginselen van chef-kok

Voordat u begint, stel ik voor dat u de basisconcepten van chef-kok bekijken. Er is geweldig materiaal <a href="http://www.chef.io/chef" target="_blank">hier</a> en ik raden u hebt een snelle lezen voordat u deze procedure probeert. Ik zal evenwel de basisbeginselen recap voordat we aan de slag.

In het volgende diagram ziet u de geavanceerde architectuur van chef-kok.

![][2]

Chef-kok heeft drie hoofdonderdelen architectuur: chef-kok Server, chef-kok-Client (knooppunt) en chef-kok werkstation.

De chef-kok Server onze beheerpunt is en er zijn twee opties voor de Server van de chef-kok: een gehoste oplossing of een oplossing op locatie. We gebruiken een gehoste oplossing.

De chef-kok-Client (knooppunt) is de agent die bevindt zich op de servers die u beheert.

Het werkstation van de chef-kok is onze admin werkstation waar we ons beleid maken en uitvoeren van onze opdrachten voor het beheer. We het **mes** -opdracht uitgevoerd vanaf het werkstation van de chef-kok om onze infrastructuur te beheren.

Er is ook het concept van "Cookbooks" en "Recepten". Dit zijn daadwerkelijk het beleid dat we definiëren en toepassen op onze servers.

## <a name="preparing-the-workstation"></a>Voorbereiding van het werkstation

Ten eerste kunt het werkstation voorbereiden. Ik gebruik een standaard Windows-werkstation. We moeten een map voor het opslaan van onze configuratiebestanden en cookbooks maken.

Eerst maakt u een map met de naam C:\chef.

Maak een tweede map met de naam c:\chef\cookbooks.

Nu moeten we onze Azure-instellingenbestand te downloaden zodat chef-kok met onze Azure abonnement communiceren kan.

Download de publicatie-instellingen van [hier](https://manage.windowsazure.com/publishsettings/).

Sla het instellingenbestand publiceren in C:\chef.

##<a name="creating-a-managed-chef-account"></a>Een beheerd chef-kok-account maken

Aanmelden voor een gehoste chef-kok account [hier](https://manage.chef.io/signup).

Tijdens het aanmeldingsproces wordt u gevraagd een nieuwe organisatie te maken.

![][3]

Als uw organisatie is gemaakt, download de starterskit.

![][4]

> [AZURE.NOTE] Als u gevraagd waarschuwing wordt dat de sleutels worden ingesteld, is het ok om verder te gaan als er geen bestaande infrastructuur nog geconfigureerd.

Deze starter kit zip-bestand bevat uw organisatie configuratiebestanden en sleutels.

##<a name="configuring-the-chef-workstation"></a>De chef-kok workstation configureren

Pak de inhoud van de chef-kok-starter.zip naar C:\chef.

Kopieer alle bestanden onder chef-starter\chef-repo\.chef-kok naar de map c:\chef.

De map ziet er nu ongeveer uit zoals in het volgende voorbeeld.

![][5]

U hebt nu vier bestanden, met inbegrip van de Azure publishing-bestand in de hoofdmap van het c:\chef.

De PEM-bestanden bevatten uw organisatie en het beheer van persoonlijke sleutels voor communicatie, terwijl het bestand knife.rb de configuratie van uw mes bevat. Moeten we het bestand knife.rb te bewerken.

Open het bestand in de editor van keuze en het wijzigen van de "cookbook_path" door het verwijderen van de /... / van het pad, zodat deze wordt weergegeven als de volgende weergegeven.

    cookbook_path  ["#{current_dir}/cookbooks"]

Voeg ook de volgende regel als gevolg van de naam van uw Azure instellingenbestand publiceren.

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

Het bestand knife.rb moet er nu uitzien zoals in het volgende voorbeeld.

![][6]

Deze regels wordt ervoor dat mes verwijst naar de map cookbooks onder c:\chef\cookbooks, en ook onze Azure publicatie-instellingen-bestand wordt gebruikt tijdens bewerkingen Azure.

## <a name="installing-the-chef-development-kit"></a>De chef-kok Development Kit installeren

Volgende [downloaden en installeren van](http://downloads.getchef.com/chef-dk/windows) de ChefDK (chef-kok Development Kit) voor het instellen van uw chef-kok-werkstation.

![][7]

In de standaardlocatie van de c:\opscode installeren. Deze installatie duurt ongeveer 10 minuten.

Bevestig dat de variabele PATH bevat ingangen voor C:\opscode\chefdk\bin; C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin

Als ze er niet bij bent, zorg ervoor dat u deze paden toevoegt!

*NOTEER DAT DE VOLGORDE VAN HET PAD IS BELANGRIJK!* Als uw opscode paden niet in de juiste volgorde zijn hebt u problemen.

Voordat u verder gaat, start opnieuw op uw werkstation.

Vervolgens zullen we de uitbreiding mes Azure te installeren. Dit biedt mes met de invoegtoepassing' Azure'.

Voer de volgende opdracht.

    chef gem install knife-azure ––pre

> [AZURE.NOTE] Het argument – pre zorgt dat u ontvangt de nieuwste RC-versie van de invoegtoepassing voor mes Azure die toegang tot de meest recente set API's biedt.

Het is waarschijnlijk dat een aantal afhankelijkheden ook worden geïnstalleerd op hetzelfde moment.

![][8]


Als u dat alles correct is geconfigureerd, moet u de volgende opdracht uitvoeren.

    knife azure image list

Als alles correct is geconfigureerd, ziet u een lijst met beschikbare Azure afbeeldingen door te schuiven.

Van harte gefeliciteerd. Het werkstation is ingesteld.

##<a name="creating-a-cookbook"></a>Een kookboek maken

Een kookboek wordt door chef-kok gebruikt voor het definiëren van een reeks opdrachten die u wilt uitvoeren op uw beheerde client. Een kookboek te maken is eenvoudig en wij de opdracht **chef-kok cookbook genereren** voor het genereren van onze kookboek sjabloon. Ik zal bellen mijn webserver kookboek als ik zou willen dat een beleid dat IIS automatisch geïmplementeerd.

Voer de volgende opdracht in de map C:\Chef.

    chef generate cookbook webserver

Dit genereert een reeks bestanden in de map C:\Chef\cookbooks\webserver. Nu moeten we definiëren de set opdrachten dat willen we graag onze chef-kok-client uit te voeren op onze beheerde virtuele machine.

De opdrachten worden opgeslagen in het bestand default.rb. In dit bestand, moet ik een reeks opdrachten waarmee u IIS installeert, wordt IIS wordt gestart en een sjabloonbestand gekopieerd naar de wwwroot-map definiëren.

Wijzig het bestand C:\chef\cookbooks\webserver\recipes\default.rb en voeg de volgende regels.

    powershell_script 'Install IIS' do
        action :run
        code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
        action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
        source 'Default.htm.erb'
        rights :read, 'Everyone'
    end

Sla het bestand op als u klaar bent.

## <a name="creating-a-template"></a>Een sjabloon maken

Zoals we eerder vermeld, moeten we een sjabloonbestand dat wordt gebruikt als onze pagina default.html genereren.

Voer de volgende opdracht om de sjabloon te genereren.

    chef generate template webserver Default.htm

Ga nu naar het bestand C:\chef\cookbooks\webserver\templates\default\Default.htm.erb. Bewerk het bestand door het toevoegen van een eenvoudige "Hello World" HTML-code en sla het bestand.



## <a name="upload-the-cookbook-to-the-chef-server"></a>Het Cookbook uploaden naar de Server van de chef-kok

We zijn in deze stap is een kopie van het Cookbook die we hebben gemaakt op de lokale computer en uploaden naar de chef-kok Hosted Server. Eenmaal geüpload, weergegeven op het tabblad **beleid** het Cookbook.

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Implementatie van een virtuele machine met Azure mes

We nu een Azure VM implementeren en toepassen van het kookboek 'Webserver' die onze IIS web service en de standaard webpagina wordt geïnstalleerd.

Hiervoor gebruikt u de opdracht **mes azure-server maken** .

Ben het voorbeeld van de opdracht volgende verschijnt.

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

De parameters zijn geen uitleg. Vervangen door de specifieke variabelen en uit te voeren.

> [AZURE.NOTE] Via de command line ik ben ook automatiseren mijn filterregels eindpunt netwerk met behulp van de parameter tcp-eindpunten. Ik heb hebt van poort 80 en 3389 voor toegang tot de webpagina en de RDP-sessie geopend.

Als u de opdracht hebt uitgevoerd, gaat u naar de portal Azure en ziet u uw computer begint te verrichten.

![][13]

De MS-DOS-prompt verschijnt het volgende.

![][10]

Zodra de installatie voltooid is, moeten we verbinding maken met de web-service via poort 80 als we hadden de poort geopend wanneer we de virtuele machine met de opdracht mes Azure ingericht worden. Deze virtuele machine is de enige virtuele machine in mijn cloud-service, ik moet verbinding te maken met de url van de wolk.

![][11]

Zoals u ziet, krijg ik creatief met mijn HTML-code.

Vergeet niet dat we kunnen ook verbinding maken via een RDP-sessie vanaf de Azure klassieke portal via poort 3389.

Ik hoop dat dit nuttig is geweest! Ga en de infrastructuur als code reis met Azure vandaag starten!


<!--Image references-->
[2]: ./media/virtual-machines-windows-chef-automation/2.png
[3]: ./media/virtual-machines-windows-chef-automation/3.png
[4]: ./media/virtual-machines-windows-chef-automation/4.png
[5]: ./media/virtual-machines-windows-chef-automation/5.png
[6]: ./media/virtual-machines-windows-chef-automation/6.png
[7]: ./media/virtual-machines-windows-chef-automation/7.png
[8]: ./media/virtual-machines-windows-chef-automation/8.png
[9]: ./media/virtual-machines-windows-chef-automation/9.png
[10]: ./media/virtual-machines-windows-chef-automation/10.png
[11]: ./media/virtual-machines-windows-chef-automation/11.png
[13]: ./media/virtual-machines-windows-chef-automation/13.png


<!--Link references-->
