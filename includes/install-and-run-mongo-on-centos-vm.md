Volg deze stappen als u wilt installeren en uitvoeren van MongoDB op een virtuele machine waarop Linux CentOS.

> [AZURE.WARNING] MongoDB-beveiligingsfuncties, zoals verificatie en de binding tussen IP-adres, zijn standaard niet ingeschakeld. Voorzieningen voor beveiliging moeten zijn ingeschakeld voordat u MongoDB implementeert in een productieomgeving.  Zie [verificatie en beveiliging](http://www.mongodb.org/display/DOCS/Security+and+Authentication) voor meer informatie.

1. Het pakket Management System (YUM) zodanig configureren dat u MongoDB kunt installeren. Maak een bestand */etc/yum.repos.d/10gen.repo* bevatten informatie over de bibliotheek en voeg het volgende toe:

        [10gen]
        name=10gen Repository
        baseurl=http://downloads-distro.mongodb.org/repo/redhat/os/x86_64
        gpgcheck=0
        enabled=1

2. Sla het bestand repo en voert u de volgende opdracht om de database lokaal pakket bijwerken:

        $ sudo yum update

3. Het pakket installeert met de volgende opdracht installeert u de nieuwste stabiele versie van MongoDB en de bijbehorende hulpprogramma's te voeren:

        $ sudo yum install mongo-10gen mongo-10gen-server

    Een ogenblik geduld terwijl MongoDB gedownload en geïnstalleerd.

4. Maak een gegevensmap. Standaard MongoDB gegevens worden opgeslagen in de map */data/db* , maar u dient die map te maken. Om deze te maken, uitvoeren:

        $ sudo mkdir -p /srv/datadrive/data
        $ sudo chown `id -u` /srv/datadrive/data

    Zie voor meer informatie over het installeren van MongoDB op Linux, [Quickstart Unix][QuickstartUnix].

5. Starten van de database, worden uitgevoerd:

        $ mongod --dbpath /srv/datadrive/data --logpath /srv/datadrive/data/mongod.log

    Alle berichten in het logboek wordt doorgestuurd naar het bestand */srv/datadrive/data/mongod.log* als MongoDB-server wordt gestart, en preallocates bestanden. Het kan enkele minuten duren voordat MongoDB wijzen de journal-bestanden en beginnen met luisteren op voor verbindingen.

6. Als de administratieve MongoDB-shell, opent u een afzonderlijk venster met SSH of stopverf en uitvoeren:

        $ mongo
        > db.foo.save ( { a:1 } )
        > db.foo.find()
        { _id : ..., a : 1 }
        > show dbs  
        ...
        > show collections  
        ...  
        > help  

    De database wordt gemaakt door het menu invoegen.

7. Nadat MongoDB is geïnstalleerd moet u een eindpunt configureren zodat MongoDB extern toegankelijk zijn. In het Portal voor beheer van **virtuele Machines**, en vervolgens klikt u op de naam van de nieuwe virtuele machine, klik op **eindpunten**.
    
    ![Eindpunten][Image7]

8. Klik op het **Eindpunt toevoegen** onder aan de pagina.
    
    ![Eindpunten][Image8]

9. Een eindpunt toevoegen met de volgende instellingen:

 - **Naam**: Mongo
 - **Protocol**: TCP
 - **Openbare poort**: 27017
 - **Particuliere poort**: 27017
 
 Hierdoor MongoDB extern toegankelijk.



[QuickStartUnix]: http://www.mongodb.org/display/DOCS/Quickstart+Unix


[Image7]: ./media/install-and-run-mongo-on-centos-vm/LinuxVmAddEndpoint.png
[Image8]: ./media/install-and-run-mongo-on-centos-vm/LinuxVmAddEndpoint2.png
