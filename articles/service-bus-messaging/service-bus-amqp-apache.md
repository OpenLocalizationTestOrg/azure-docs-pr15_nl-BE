<properties 
    pageTitle="Het installeren van Apache Qpid Proton-C op een Linux VM | Microsoft Azure"
    description="Het maken van een CentOS Linux VM met Azure virtuele Machines en maken en installeren van de Apache Qpid Proton-C-bibliotheek."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="install-apache-qpid-proton-c-on-an-azure-linux-vm"></a>Installeren van Apache Qpid Proton-C op een Azure Linux VM

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

In deze sectie wordt beschreven hoe een CentOS Linux VM met Azure virtuele Machines maken en downloaden, bouwen en installeren van de Apache Qpid Proton-C bibliotheek en de Python en PHP taal-bindingen. Nadat u deze stappen uitvoert, kun je de Python en PHP monsters die met deze handleiding uitvoeren.

De eerste stap wordt uitgevoerd met behulp van de [Azure klassieke portal][]. De volgende schermafbeelding ziet u hoe een CentOS VM met de naam 'scott centos' is gemaakt:

![Proton op een Azure Linux VM][0]

Na het inrichten, de portal wordt het volgende weergegeven:

![Proton op een Azure Linux VM][1]

Aan te melden op de computer, moet u de poort van eindpunt voor SSH kent. Deze waarde kunt van het [Azure klassieke portal][] u verkrijgen door de nieuwe VM te selecteren en te klikken op het tabblad **eindpunten** . De volgende schermafbeelding ziet u de openbare SSH-poort voor deze computer is 57146.

![Proton op een Azure Linux VM][2]

U kunt nu verbinding maken met de computer via SSH. In dit voorbeeld wordt de stopverf tool, zoals in de volgende schermafdruk:

![Proton op een Azure Linux VM][3]

In dit voorbeeld wordt voor de Python en PHP toepassingen, het Proton clientbibliotheken van Apache. Deze bibliotheken zijn beschikbaar voor het downloaden van [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html). Het Leesmij-bestand in het distributiepakket voor worden de stappen beschreven die de afhankelijkheden installeren en bouwen van Proton. Hier volgt een overzicht van de stappen uit:

1.  Bewerk de yum configuratiebestand (/ etc/yum.conf) en het commentaar van de uitsluiting voor updates kernel-headers (\# uitsluiten kernel =\*). Dit is nodig voor de installatie van de gcc-compiler.

2.  Installeer de vereiste pakketten:

    ```
    # required dependencies 
    yum install gcc cmake libuuid-devel
    
    # dependencies needed for ssl support
    yum install openssl-devel
    
    # dependencies needed for bindings
    yum install swig python-devel ruby-devel php-devel java-1.6.0-openjdk
    
    # dependencies needed for python docs
    yum install epydoc
    ```

1.  De bibliotheek Proton downloaden:

    ```
    [azureuser@this-user ~]$ wget http://apache.panu.it/qpid/proton/0.9/qpid-proton-0.9.tar.gz
    --2016-04-17 14:45:03--  http://apache.panu.it/qpid/proton/0.9/qpid-proton-0.9.tar.gz
    Resolving apache.panu.it (apache.panu.it)... 81.208.22.71
    Connecting to apache.panu.it (apache.panu.it)|81.208.22.71|:80... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 868226 (848K) [application/x-gzip]
    Saving to: ‘qpid-proton-0.9.tar.gz’
    
    qpid-proton-0.9.tar.gz                               
    
    100%[====================================================================================================================>] 847.88K   102KB/s    in 8.4s    
    
    2016-04-17 14:45:12 (101 KB/s) - ‘qpid-proton-0.9.tar.gz’ saved [868226/868226]
    ```

1.  De Proton-code uit het archief van de distributie uit te pakken.

    ```
    tar xvfz qpid-proton-0.9.tar.gz
    ```

1.  Compileren en installeren van de code met behulp van de volgende stappen uit, afkomstig van het Leesmij-bestand:

    ```
    From the directory where you found this README file:    
    
    mkdir build cd build
            
    # Set the install prefix. You may need to adjust depending on your      
    # system.       
    cmake -DCMAKE\_INSTALL\_PREFIX=/usr ..
            
    # Omit the docs target if you do not wish to build or install       
    # documentation.        
    make all docs
            
    # Note that this step will require root privileges.     
    make install
    ```

Nadat u deze stappen uitvoert, is de Proton op de computer is geïnstalleerd en gereed voor gebruik.

## <a name="next-steps"></a>Volgende stappen

Wilt u meer informatie? Ga naar de volgende koppeling:

- [Overzicht Service Bus AMQP][]

[Overzicht Service Bus AMQP]: service-bus-amqp-overview.md
[0]: ./media/service-bus-amqp-apache/amqp-apache-1.png
[1]: ./media/service-bus-amqp-apache/amqp-apache-2.png
[2]: ./media/service-bus-amqp-apache/amqp-apache-3.png
[3]: ./media/service-bus-amqp-apache/amqp-apache-4.png

[Azure klassieke portal]: http://manage.windowsazure.com


