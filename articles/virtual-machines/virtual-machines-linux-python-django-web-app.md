<properties 
    pageTitle="Python web app met Django op Linux | Microsoft Azure" 
    description="Informatie over het hosten van een webtoepassing Django gebaseerd op een Linux virtuele machine met Azure." 
    services="virtual-machines-linux" 
    documentationCenter="python" 
    authors="huguesv" 
    manager="wpickett" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="web" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="11/17/2015" 
    ms.author="huvalo"/>
    
# <a name="django-hello-world-web-application-on-a-linux-vm"></a>Django Hello World-webtoepassing op een Linux VM

> [AZURE.SELECTOR]
- [Windows](virtual-machines-windows-classic-python-django-web-app.md)
- [Mac/Linux](virtual-machines-linux-python-django-web-app.md)

<br>

In deze zelfstudie wordt beschreven hoe u als host voor een website op basis van Django op Microsoft Azure met behulp van een Linux virtuele machine. In deze zelfstudie wordt ervan uitgegaan dat u geen eerdere ervaring hebt met Azure. Bij de voltooiing van deze handleiding, hebt u een Django-toepassing van en het uitvoeren in de cloud.

U leert hoe u:

* Setup een Azure virtuele machine host Django. Terwijl in deze zelfstudie wordt uitgelegd hoe u kunt dit doen onder **Linux**, kan hetzelfde ook met een Windows Server VM in Azure gehost worden uitgevoerd. 
* Maak een nieuwe Django toepassing van Linux.

Door deze zelfstudie te volgen, bouwt u een eenvoudige Hello World-webtoepassing. De toepassing zal worden gehost in een Azure virtuele machine.

Een schermafbeelding van de voltooide toepassing lager is dan:

![Een weergave van de pagina Hallo wereld op Azure browservenster](./media/virtual-machines-linux-python-django-web-app/mac-linux-django-helloworld-browser.png)

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="creating-and-configuring-an-azure-virtual-machine-to-host-django"></a>Maken en configureren van een virtuele machine met Azure host Django

1. Volg de instructies gegeven [hier](virtual-machines-linux-quick-create-portal.md) een Azure virtuele machine van de distributie van *Ubuntu Server 14.04 LTS* maken.  Als u wilt, kunt u verificatie in plaats van openbare SSH-sleutel met een wachtwoord.

1. Bewerken van de beveiligingsgroep netwerk dat binnenkomend HTTP-verkeer op poort 80 met behulp van de instructies [hier](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).

1. Standaard heeft de nieuwe virtuele machine geen volledig gekwalificeerde domeinnaam (FQDN).  Kunt u maken door de instructies [hier](virtual-machines-linux-portal-create-fqdn.md).  Deze stap is optioneel voor het voltooien van deze zelfstudie.

## <a id="setup"> </a>De ontwikkelomgeving instellen

**Opmerking:** Als u wilt installeren, Python of wilt gebruiken, de Client Libraries, Zie de [Handleiding voor installatie van Python](../python-how-to-install.md).

De Ubuntu Linux VM al wordt geleverd met Python 2.7 vooraf geïnstalleerd, maar heeft geen Apache of django geïnstalleerd.  Volg deze stappen om te verbinden met uw VM en installatie van Apache en django.

1.  Start een nieuwe **Terminal** -venster.
    
1.  Voer de volgende opdracht om de verbinding met de Azure VM.  Als u een FQDN-naam niet zelf hebt gemaakt, kunt u het openbare IP-adres weergegeven in de virtuele machine in de klassieke Azure portal overzicht.

        $ ssh yourusername@yourVmUrl

1.  Voer de volgende opdrachten django installeren:

        $ sudo apt-get install python-setuptools python-pip
        $ sudo pip install django

1.  Voer de volgende opdracht installeren Apache met mod-wsgi:

        $ sudo apt-get install apache2 libapache2-mod-wsgi


## <a name="creating-a-new-django-application"></a>Een nieuwe Django-toepassing maken

1.  Open het venster **Terminal** die u hebt gebruikt in de vorige sectie om ssh in uw VM.
    
1.  Voer de volgende opdrachten voor het maken van een nieuw project voor Django:

        $ cd /var/www
        $ sudo django-admin.py startproject helloworld

    Het script **django admin.py** genereert een basisstructuur voor websites op basis van Django:
    -   **HelloWorld/Manage.PY** helpt u bij het starten hosting en stoppen met het hosten van uw website op basis van Django
    -   **HelloWorld/HelloWorld/Settings.PY** bevat instellingen voor uw toepassing Django.
    -   **HelloWorld/HelloWorld/URLs.PY** bevat de code van de toewijzing tussen elke url en de weergave.

1.  Maak een nieuw bestand met de naam **views.py** in de map **/var/www/helloworld/helloworld** . Dit bevat de weergave die de pagina 'hello world' wordt weergegeven. Start uw editor en voer de volgende gegevens:
        
        from django.http import HttpResponse
        def home(request):
            html = "<html><body>Hello World!</body></html>"
            return HttpResponse(html)

1.  Nu vervangen door de inhoud van het bestand **urls.py** de volgende opties:

        from django.conf.urls import patterns, url
        urlpatterns = patterns('',
            url(r'^$', 'helloworld.views.home', name='home'),
        )


## <a name="setting-up-apache"></a>Het instellen van Apache

1.  Maak een Apache virtual host configuratie bestand **/etc/apache2/sites-available/helloworld.conf**. De inhoud wordt ingesteld op de volgende en *yourVmName* vervangen door de werkelijke naam van de computer die u (voor het voorbeeld *pyubuntu gebruikt*).

        <VirtualHost *:80>
        ServerName yourVmName
        </VirtualHost>
        WSGIScriptAlias / /var/www/helloworld/helloworld/wsgi.py
        WSGIPythonPath /var/www/helloworld

1.  Inschakelen van de site met de volgende opdracht:

        $ sudo a2ensite helloworld

1.  Opnieuw Apache starten met de volgende opdracht:

        $ sudo service apache2 reload

1.  Tot slot laadt de webpagina in uw browser:

    ![Een weergave van de pagina Hallo wereld op Azure browservenster](./media/virtual-machines-linux-python-django-web-app/mac-linux-django-helloworld-browser.png)


## <a name="shutting-down-your-azure-virtual-machine"></a>Uw Azure virtuele machine wordt afgesloten

Wanneer u klaar bent met deze zelfstudie, afsluiten en/of verwijderen de zojuist gemaakte Azure virtuele machine om bronnen vrij te maken voor andere zelfstudie te kiezen en te voorkomen dat Azure kosten.
