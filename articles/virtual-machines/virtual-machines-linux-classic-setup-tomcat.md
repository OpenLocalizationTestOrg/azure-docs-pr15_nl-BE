<properties
    pageTitle="Instellen van de Tomcat Apache op een Linux VM | Microsoft Azure"
    description="Meer informatie over het instellen van Apache Tomcat7 met behulp van een Azure VM (virtual machine) waarop Linux wordt uitgevoerd."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="NingKuang"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="ningk"/>

#<a name="how-to-set-up-tomcat7-on-a-linux-virtual-machine-with-microsoft-azure"></a>Het instellen van Tomcat7 op een virtuele Linux Machine met Microsoft Azure

Apache Tomcat (of gewoon Tomcat, voorheen ook Jakarta Tomcat) is een open-source webserver en een servlet container is ontwikkeld door de Apache Software Foundation (ASF). Tomcat implementeert de Java Servlet en de specificaties JavaServer Pages (JSP) van Sun Microsystems, en biedt een zuivere Java HTTP web serveromgeving voor het uitvoeren van Java-code. In de eenvoudigste configuratie Tomcat uitgevoerd in een proces één besturingssysteem. Dit proces wordt uitgevoerd een Java virtual machine (JVM). Elke HTTP-aanvraag van een browser aan Tomcat wordt verwerkt als een aparte thread in het proces Tomcat.  

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


In deze handleiding vindt u tomcat7 installeren op een Linux-image en implementeert u deze in Microsoft Azure.  

U leert:  

-   Het maken van een virtuele machine in Azure.
-   Het voorbereiden van de virtuele machine voor tomcat7.
-   Het installeren van tomcat7.

Wordt uitgegaan van de lezer heeft al een abonnement op Azure.  Als dit niet kunt u zich aanmelden voor een gratis proefperiode op [http://azure.microsoft.com](https://azure.microsoft.com/). Als u een MSDN-abonnement hebt, raadpleegt u [Microsoft Azure speciale prijzen: MSDN, MPN en Bizspark voordelen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39). Zie voor meer informatie over Azure, [Wat is Azure?](https://azure.microsoft.com/overview/what-is-azure/).

In dit onderwerp wordt ervan uitgegaan dat u basiskennis werken van tomcat en Linux.  

##<a name="phase-1-create-an-image"></a>Fase 1: Een afbeelding maken
In deze fase maakt u een virtuele machine met behulp van een Linux-image in Azure.  

###<a name="step-1-generate-an-ssh-authentication-key"></a>Stap 1: Een SSH-Verificatiesleutel genereren
SSH is een belangrijk hulpmiddel voor systeembeheerders. Configureren van beveiliging voor toegang op basis van een mens bepaald wachtwoord is echter niet aanbevolen. Kwaadwillende gebruikers kunnen opdelen in uw systeem op basis van een gebruikersnaam en een zwak wachtwoord.

Het goede nieuws is dat er een manier is om RAS open te laten en niet te hoeven maken over wachtwoorden hebben. De methode bestaat uit de verificatie met asymmetrische cryptografie. De persoonlijke sleutel van de gebruiker is waarmee de verificatie wordt verleend. Ook kunt u de account van de gebruiker dat deze volledig wachtwoordverificatie niet vergrendelen.

Een ander voordeel van deze methode is dat u niet verschillende wachtwoorden aan te melden hoeft op verschillende servers. U kunt verifiëren met behulp van de persoonlijke sleutel persoonlijke op alle servers die u voorkomt verschillende wachtwoorden onthouden.

Het is ook mogelijk aanmelden met een wachtwoord in te stellen met deze methode.

Volg deze stappen om de SSH-Verificatiesleutel genereren.

1.  Download en installeer puttygen vanaf de volgende locatie: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2.  PUTTYGEN uitvoeren. EXE.
3.  Klik op **genereren** om de sleutels te genereren. U kunt in het proces willekeurigheid verhogen door de muis op het lege gebied in het venster.  
![][1]
4.  Na het proces genereren weergegeven Puttygen.exe uw gegenereerde sleutel. Bijvoorbeeld:  
![][2]
5.  Selecteer en kopieer de openbare sleutel in **sleutel** en opslaan in een bestand met de naam publicKey.pem. Niet op **Opslaan openbare sleutel**, omdat de bestandsindeling opgeslagen van de openbare sleutel verschilt van de openbare sleutel die we willen.
6.  Klik op de **persoonlijke sleutel opslaan** en opslaan in een bestand met de naam privateKey.ppk.

###<a name="step-2-create-the-image-in-the-azure-portal"></a>Stap 2: Maak de afbeelding in de portal Azure.
Klik op **Nieuw** in de taakbalk voor het maken van een afbeelding, de Linux-afbeelding op basis van uw behoeften te kiezen in de [portal Azure](https://portal.azure.com/). In het volgende voorbeeld wordt de afbeelding Ubuntu 14.04.
![][3]

Voor de **Host-naam** opgeven voor de URL die u en Internet-clients gebruiken voor toegang tot deze virtuele machine. Het laatste gedeelte van de DNS-naam, bijvoorbeeld tomcatdemo, definiëren en Azure genereert de URL als tomcatdemo.cloudapp.net.  

Voor **SSH verificatie-sleutel**, waarde van de sleutel van het bestand **publicKey.pem** de openbare sleutel die is gegenereerd door puttygen bevat te kopiëren.  
![][4]

Indien nodig andere instellingen configureren, en klik vervolgens op maken.  

##<a name="phase-2-prepare-your-virtual-machine-for-tomcat7"></a>Fase 2: Voorbereiden op de virtuele machine Tomcat7
In deze fase zal u een eindpunt voor tomcat verkeer configureren en vervolgens verbinding maken met uw nieuwe virtuele machine.
###<a name="step-1-open-the-http-port-to-allow-web-access"></a>Stap 1: Open de HTTP-poort voor webtoegang
Eindpunten in Azure bestaan uit een protocol (TCP of UDP) en een openbare en particuliere poort. De private poort is de poort waarop de service op de virtuele machine luistert. De openbare poort is de poort die de Azure cloud-service op extern voor binnenkomend verkeer op basis van het Internet luistert.  

TCP-poort 8080 is het standaardpoortnummer op welke tomcat luistert. Deze poort te openen met een eindpunt Azure kunnen u en andere clients toegang tot het Internet aan tomcat pagina's.  

1.  Klik op **Bladeren**in het portal voor Azure -> **virtuele Machine**, en klik vervolgens op de virtuele machine die u hebt gemaakt.  
![][5]
2.  Als een eindpunt aan uw virtuele machine wilt toevoegen, klikt u op het vak **eindpunten** .
![][6]
3.  Klik op **toevoegen**.  
    1.  Typ een naam voor het eindpunt in het eindpunt voor het **eindpunt**en typ in **Openbare poort**80.  

        Als u deze op 80 instelt hoeft te bevatten het nummer van de poort in de URL waarmee u toegang tot tomcat. Bijvoorbeeld http://tomcatdemo.cloudapp.net.    

        Als u deze op een andere waarde, zoals 81 instelt, moet u het poortnummer toevoegen aan de URL voor toegang tot de tomcat. Bijvoorbeeld, http://tomcatdemo.cloudapp.net:81 /.
    2.  Typ Private poort 8080. Tomcat luistert standaard op TCP-poort 8080. Als u hebt gewijzigd de standaard poort van tomcat luisteren, particuliere poort zijn dat hetzelfde als de tomcat poort voor luisteren moet worden bijgewerkt.  
    ![][7]

4.  Klik op **OK** om het eindpunt toevoegen aan uw virtuele machine.



###<a name="step-2-connect-to-the-image-you-created"></a>Stap 2: Verbinding maken met de afbeelding die u hebt gemaakt.
U kunt een gereedschap SSH verbinding maken met de virtuele machine. In dit voorbeeld gebruiken we de stopverf.  

U eerst de DNS-naam van de virtuele machine vanuit de Azure portal. **Klik op Bladeren** -> **virtuele machines** de naam van uw virtuele computer -> **Eigenschappen**-> en zoek vervolgens in het veld **Domein de naam** van het venster **Eigenschappen** .  

Haal het poortnummer voor SSH verbindingen uit het veld **SSH** . Hier volgt een voorbeeld.  
![][8]

Putty downloaden vanaf [hier](http://www.putty.org/) .  

Nadat u hebt gedownload, klikt u op het uitvoerbare bestand STOPVERF. EXE. De basisopties configureren met de hostnaam en poortnummer afkomstig van de eigenschappen van de virtuele machine. Hier volgt een voorbeeld:  
![][9]

Klik in het linkerdeelvenster op **verbinding** -> **SSH** -> **-verificatie** en klik vervolgens op **Bladeren** om de locatie van het bestand **privateKey.ppk** de persoonlijke sleutel bevat gegenereerd door puttygen in fase 1: een afbeelding maken. Hier volgt een voorbeeld:  
![][10]

Klik op **openen**. U kunt worden gewaarschuwd door een berichtvenster. Als u de DNS-naam is geconfigureerd en goed nummer van de poort, klikt u op **Ja**.
![][11]  


Zie de volgende:  
![][12]

Geef de gebruikersnaam die is opgegeven bij het maken van de virtuele machine in fase 1: een afbeelding maken. Worden er ongeveer zo uit:  
![][13]





##<a name="phase-3-install-software"></a>Fase 3: Installeren van software
In deze fase installeert u de Java runtime environment, tomcat en andere onderdelen tomcat.  

###<a name="java-runtime-environment"></a>Java runtime environment
Tomcat is geschreven in Java. Er zijn twee soorten Java Development Kits (JDKs) (OpenJDK en Oracle JDK) en u kunt kiezen die u wilt.  

>AZURE. Opmerking: Beide JDKs hebben bijna dezelfde code voor de klassen in de API voor Java, maar de code voor de virtuele machine is echt anders. Wanneer het gaat om bibliotheken, vaak OpenJDK open bibliotheken gebruiken terwijl Oracle meestal gesloten die gebruiken. Maar Oracle JDK heeft meer klassen en een aantal bugs vaste en Oracle JDK is stabieler dan OpenJDK.

De volgende opdrachten downloaden de verschillende JDKs.  

Open-jdk   

    sudo apt-get update  
    sudo apt-get install openjdk-7-jre  

Oracle-jdk  

-   De JDK downloaden vanaf de Oracle-website:  

        wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u5-b13/jdk-8u5-linux-x64.tar.gz  

-   De JDK-bestanden een map maken:  

        sudo mkdir /usr/lib/jvm  

-   De JDK-bestanden uitpakken in de map usr/lib/jvm /:  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/  

-   Oracle JDK instellen als standaard JVM:  

        sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_05/bin/java 100  
        sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_05/bin/javac 100  

####<a name="test"></a>Test:
U kunt een opdracht als de volgende testen als de Java runtime environment correct is geïnstalleerd:  

    java -version  

Als u open jdk hebt geïnstalleerd, ziet u een bericht als volgt uit:![][14]

Als u een oracle-jdk hebt geïnstalleerd, wordt er een bericht als volgt uit:![][15]

###<a name="tomcat7"></a>Tomcat7
Gebruik de volgende opdracht voor het installeren van tomcat7:  

    sudo apt-get install tomcat7  

Als u geen tomcat7, gebruikt u de juiste variatie van deze opdracht.  

####<a name="test"></a>Test:

Als u wilt controleren of de tomcat7 met succes is geïnstalleerd, gaat u naar de tomcat-server DNS-naam (http://tomcatexample.cloudapp.net/ is de URL van het voorbeeld in dit artikel). Als u een pagina als volgt ziet, hebt u tomcat7 geïnstalleerd correct.
![][16]


###<a name="install-other-tomcat-components"></a>Andere Tomcat-onderdelen installeren
Er zijn andere tomcat optionele onderdelen die u ook kunt installeren.  

Gebruik de opdracht **sudo apt-cache search-tomcat7** voor een overzicht van de beschikbare onderdelen. De volgende opdrachten zijn voorbeelden bepaalde bruikbare onderdelen installeren.  

    sudo apt-get install tomcat7-admin      #admin web applications
    sudo apt-get install tomcat7-user         #tools to create user instances  

##<a name="phase-4-configure-tomcat"></a>Fase 4: Configureer Tomcat
In deze fase beheert u tomcat.
###<a name="start-and-stop-tomcat7"></a>Starten en stoppen tomcat7
De tomcat7-server automatisch gestart wanneer u deze installeert. U kunt ook starten uzelf met de volgende opdracht:   

    sudo /etc/init.d/tomcat7 start

Tomcat7 stoppen:  

    sudo /etc/init.d/tomcat7 stop

De status weergeven van tomcat7:  

    sudo /etc/init.d/tomcat7 status

Tomcat-services opnieuw starten:  

    sudo /etc/init.d/tomcat7 restart

###<a name="tomcat-administration"></a>Beheer van Tomcat
Kunt u het bestand Tomcat configuratie om uw referenties admin met de volgende opdracht in te stellen:  

    sudo vi  /etc/tomcat7/tomcat-users.xml   

Hier volgt een voorbeeld:  
![][17]  

>AZURE. Opmerking: Een sterk wachtwoord voor de gebruikersnaam admin maken.  

Na het bewerken van dit bestand, moet u opnieuw opstarten tomcat7 services met de volgende opdracht om ervoor te zorgen dat de wijzigingen doorgevoerd worden:  

    sudo /etc/init.d/tomcat7 restart  

Open uw browser en typ de URL **http://<your tomcat server DNS name>manager/HTML-**. De in dit artikel, bijvoorbeeld is de URL http://tomcatexample.cloudapp.net/manager/html.  

Nadat u een verbinding maakt, worden er iets met de volgende strekking:  
![][18]

##<a name="common-issues"></a>Veelvoorkomende problemen

###<a name="cant-access-the-virtual-machine-with-tomcat-and-moodle-from-the-internet"></a>Geen toegang tot de virtuele machine met Tomcat en Moodle vanaf het Internet

-   **Symptoom**  
Tomcat wordt uitgevoerd, maar u kunt de standaardpagina Tomcat niet zien met uw browser.
-   **Mogelijke basis-aanvraag**   
    1.  De tomcat listen-poort is niet hetzelfde als de particuliere poort van eindpunt van de virtuele machine voor tomcat verkeer.  

        Controleer de poort openbare en particuliere poort eindpunt instellingen en controleer of dat de privé-poort is dat dezelfde als de tomcat poort voor luisteren. Zie fase 1: Een afbeelding maken voor instructies over het configureren van eindpunten voor uw virtuele machine.  

        Om te bepalen welke tomcat listen-poort, /etc/httpd/conf/httpd.conf (Red Hat versie) of /etc/tomcat7/server.xml (Debian release) te openen. Standaard is de tomcat luisteren naar poort 8080. Hier volgt een voorbeeld:  

            <Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"  URIEncoding="UTF-8"            redirectPort="8443" />  

        Als u een virtuele machine zoals Debian of Ubuntu en u wijzigen de standaard poort wilt van Tomcat luisteren (bijvoorbeeld 8081) gebruikt, moet u ook de poort openen voor het besturingssysteem. Open eerst het profiel:  

            sudo vi /etc/default/tomcat7  

        Vervolgens Opmerking verwijderen de laatste regel en wijzigen van 'no' op 'yes'.  

            AUTHBIND=yes

    2.  De firewall is de poort voor luisteren van tomcat uitgeschakeld.

        Als u alleen de Tomcat standaardpagina van de lokale host ziet, is het probleem waarschijnlijk dat de poort waarop wordt geluisterd door tomcat is geblokkeerd door de firewall. U kunt het hulpprogramma w3m de pagina bladeren. De volgende opdrachten w3m installeren en blader naar de standaardpagina Tomcat:  

            sudo yum  install w3m w3m-img
            w3m http://localhost:8080  

-   **Oplossing**
    1. Als de tomcat luisteren poort is niet hetzelfde als de privé-haven van het eindpunt voor het verkeer naar de virtuele machine, moet u de particuliere-poort zijn dat hetzelfde als de tomcat poort voor luisteren wijzigen.   

    2.  Als het probleem wordt veroorzaakt door de firewall/iptables, voeg de volgende regels aan /etc/sysconfig/iptables:  

            -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
            -A INPUT -p tcp -m tcp --dport 443 -j ACCEPT  

        Let op de tweede regel is alleen vereist voor https-verkeer.  

        Zorg ervoor dat dit hoger is dan de regels die wereldwijd toegang, zoals de volgende zou beperken:  

            -A INPUT -j REJECT --reject-with icmp-host-prohibited  

        Als u wilt de iptables laden, moet u de volgende opdracht uitvoeren:  

            service iptables restart  

        Dit is getest op CentOS 6.3.

###<a name="permission-denied-when-upload-you-project-files-to-varlibtomcat7webapps"></a>Toestemming geweigerd tijdens uploaden projectbestanden op /var/lib/tomcat7/webapps /  

-   **Symptoom**  
Wanneer u een SFTP-client (bijvoorbeeld FileZilla) verbinding maken met uw virtuele computer en Ga naar /var/lib/tomcat7/webapps/de site publiceert, verschijnt u een foutbericht met de volgende strekking:  

        status: Listing directory /var/lib/tomcat7/webapps
        Command:    put "C:\Users\liang\Desktop\info.jsp" "info.jsp"
        Error:  /var/lib/tomcat7/webapps/info.jsp: open for write: permission denied
        Error:  File transfer failed

-   **Mogelijke basis-aanvraag** U bent niet gemachtigd voor toegang tot de map /var/lib/tomcat7/webapps.  
-   **Oplossing**  
U moet de toestemming krijgt van de root-account. Kunt u de eigenaar van de map in de hoofdmap voor de gebruikersnaam die u gebruikt tijdens het inrichten van de machine. Hier volgt een voorbeeld met de naam azureuser:  

        sudo chown azureuser -R /var/lib/tomcat7/webapps

    De optie -R toe te passen de machtigingen voor alle bestanden in een map te gebruiken.  

    Houd er rekening mee dat deze opdracht ook voor mappen werkt. De optie -R de machtigingen voor alle bestanden en mappen in de map gewijzigd. Hier volgt een voorbeeld:  

        sudo chown -R username:group directory  

    Met deze opdracht wijzigt de eigendom (gebruiker en groep) voor alle bestanden en mappen in de map en de map zelf.  

    De volgende opdracht wijzigt de machtigingen van de mapdirectory alleen maar blijven de bestanden en mappen binnen de map alleen.  

        sudo chown username:group directory



[1]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-01.png
[2]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-02.png
[3]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-03.png
[4]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-04.png
[5]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-05.png
[6]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-06.png
[7]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-07.png
[8]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-08.png
[9]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-09.png
[10]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-10.png
[11]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-11.png
[12]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-12.png
[13]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-13.png
[14]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-14.png
[15]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-15.png
[16]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-16.png
[17]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-17.png
[18]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-18.png
