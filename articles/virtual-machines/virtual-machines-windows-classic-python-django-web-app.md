<properties
    pageTitle="Python web app met Django | Microsoft Azure"
    description="Deze zelfstudie leert u hoe u als host voor een website op basis van Django op Azure met behulp van een Windows Server 2012 R2 Datacenter virtuele machine met behulp van het implementatiemodel klassiek."
    services="virtual-machines-windows"
    documentationCenter="python"
    authors="huguesv"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>


<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="web" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="huvalo"/>


# <a name="django-hello-world-web-application-on-a-windows-server-vm"></a>Django Hello World-webtoepassing op een Windows Server-VM

> [AZURE.SELECTOR]
- [Windows](virtual-machines-windows-classic-python-django-web-app.md)
- [Mac/Linux](virtual-machines-linux-python-django-web-app.md)

<br>

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

In deze zelfstudie wordt beschreven hoe u als host voor een website op basis van Django op Microsoft Azure met behulp van een Windows Server virtuele machine. In deze zelfstudie wordt ervan uitgegaan dat u geen eerdere ervaring hebt met Azure. Na het voltooien van deze zelfstudie hebt u een Django-toepassing van en het uitvoeren in de cloud.

U leert hoe u:

* Een Azure virtuele machine host Django instellen. Terwijl in deze zelfstudie wordt uitgelegd hoe u kunt dit doen onder Windows Server, kan hetzelfde ook met een VM in Azure gehost op Linux worden uitgevoerd.
* Maak een nieuwe Django toepassing van Windows.

Door deze zelfstudie te volgen, bouwt u een eenvoudige Hello World-webtoepassing. De toepassing zal worden gehost in een Azure virtuele machine.

Een schermafbeelding van de voltooide toepassing verschijnt het volgende.

![Een weergave van de pagina Hallo wereld op Azure browservenster][1]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="creating-and-configuring-an-azure-virtual-machine-to-host-django"></a>Maken en configureren van een virtuele machine met Azure host Django

1. Volg de instructies gegeven [hier](virtual-machines-windows-classic-tutorial.md) een Azure virtuele machine van de distributie van Windows Server 2012 R2 Datacenter maken.

1. Azure poort 80 verkeer van het web naar poort 80 op de virtuele machine te instrueren:
 - Ga naar de zojuist gemaakte virtuele machine in de klassieke Azure portal en klik op het tabblad **EINDPUNTEN** .
 - Klik op de knop **toevoegen** onder aan het scherm.
    ![eindpunt toevoegen](./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-addendpoint.png)

 - Open het **TCP** -protocol's **openbare poort 80** als **PRIVATE poort 80**.
![][port80]
1. Klik op **VERBINDEN** om **Extern bureaublad** op afstand inloggen op de zojuist gemaakte Azure virtual machine gebruiken op het tabblad **DASHBOARD** .  

**Belangrijke opmerking:** Alle onderstaande instructies wordt ervan uitgegaan dat u de virtuele machine correct ingelogd en opdrachten er in plaats van de lokale computer uitvoert.

## <a id="setup"> </a>Voor het installeren van Python, Django, WFastCGI

**Opmerking:** Wilt downloaden met Internet Explorer moet u wellicht configureren van instellingen voor verbeterde beveiliging (Start/Administrative Tools/Server Manager/lokale Server, klikt u vervolgens op **Internet Explorer Enhanced Security Configuration**, ingesteld op uit).

1. Installeer de meest recente Python 2.7 of 3.4 van [python.org][].
1. De wfastcgi en django-pakketten met pip installeren.

    Gebruik de volgende opdracht voor Python-2.7.

        c:\python27\scripts\pip install wfastcgi
        c:\python27\scripts\pip install django

    Gebruik de volgende opdracht voor Python-3.4.

        c:\python34\scripts\pip install wfastcgi
        c:\python34\scripts\pip install django

## <a name="installing-iis-with-fastcgi"></a>IIS installeren met FastCGI

1. IIS installeren met de FastCGI-ondersteuning.  Dit kan enige tijd duren om uit te voeren.

        start /wait %windir%\System32\PkgMgr.exe /iu:IIS-WebServerRole;IIS-WebServer;IIS-CommonHttpFeatures;IIS-StaticContent;IIS-DefaultDocument;IIS-DirectoryBrowsing;IIS-HttpErrors;IIS-HealthAndDiagnostics;IIS-HttpLogging;IIS-LoggingLibraries;IIS-RequestMonitor;IIS-Security;IIS-RequestFiltering;IIS-HttpCompressionStatic;IIS-WebServerManagementTools;IIS-ManagementConsole;WAS-WindowsActivationService;WAS-ProcessModel;WAS-NetFxEnvironment;WAS-ConfigurationAPI;IIS-CGI

## <a name="creating-a-new-django-application"></a>Een nieuwe Django-toepassing maken

1.  Voer de volgende opdracht om een nieuw project voor Django in *C:\inetpub\wwwroot*:

    Gebruik de volgende opdracht voor Python-2.7.

        C:\Python27\Scripts\django-admin.exe startproject helloworld

    Gebruik de volgende opdracht voor Python-3.4.

        C:\Python34\Scripts\django-admin.exe startproject helloworld

    ![Het resultaat van de opdracht Nieuw AzureService](./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-cmd-new-azure-service.png)

1.  De opdracht **django admin** genereert een basisstructuur voor websites op basis van Django:

  -   **helloworld\manage.PY** helpt u bij het starten hosting en stoppen met het hosten van uw website op basis van Django
  -   **helloworld\helloworld\settings.PY** bevat instellingen voor uw toepassing Django.
  -   **helloworld\helloworld\urls.PY** bevat de code van de toewijzing tussen elke url en de weergave.

1.  Maak een nieuw bestand met de naam **views.py** in de map *C:\inetpub\wwwroot\helloworld\helloworld* . Dit bevat de weergave die de pagina 'hello world' wordt weergegeven. Start uw editor en voer de volgende gegevens:

        from django.http import HttpResponse
        def home(request):
            html = "<html><body>Hello World!</body></html>"
            return HttpResponse(html)

1.  Vervangen door de inhoud van het bestand urls.py de volgende.

        from django.conf.urls import patterns, url
        urlpatterns = patterns('',
            url(r'^$', 'helloworld.views.home', name='home'),
        )

## <a name="configuring-iis"></a>IIS configureren

1. De sectie-handlers in de globale applicationhost.config ontgrendelen.  Hiermee schakelt u het gebruik van de python-handler in uw web.config.

        %windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers

1. WFastCGI inschakelen.  Hiermee wordt een toepassing toevoegen aan de globale applicationhost.config die naar de Python interpreter uitvoerbaar en het script wfastcgi.py verwijst.

    2.7 Python:

        c:\python27\scripts\wfastcgi-enable

    3.4 Python:

        c:\python34\scripts\wfastcgi-enable

1. Maak een web.config-bestand in *C:\inetpub\wwwroot\helloworld*.  De waarde van de `scriptProcessor` kenmerk moet overeenkomen met de uitvoer van de vorige stap.  Zie de pagina voor [wfastcgi][] op pypi voor meer op wfastcgi-instellingen.

    2.7 Python:

        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python27\python.exe|C:\Python27\Lib\site-packages\wfastcgi.pyc" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>

    3.4 Python:

        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python34\python.exe|C:\Python34\Lib\site-packages\wfastcgi.py" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>

1. De locatie van de IIS Default Web Site verwijzen naar de map django project bijwerken.

        %windir%\system32\inetsrv\appcmd set vdir "Default Web Site/" -physicalPath:"C:\inetpub\wwwroot\helloworld"

1. Ten slotte worden de webpagina in de browser geladen.

![Een weergave van de pagina Hallo wereld op Azure browservenster][1]


## <a name="shutting-down-your-azure-virtual-machine"></a>Uw Azure virtuele machine wordt afgesloten

Wanneer u met deze zelfstudie bent klaar, afsluiten en/of verwijderen van de zojuist gemaakte Azure virtuele machine om bronnen vrij te maken voor andere zelfstudie te kiezen en te voorkomen dat Azure kosten.

[1]: ./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-browser-azure.png

[port80]: ./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-port80.png

[Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Python.org]: https://www.python.org/downloads/
[wfastcgi]: https://pypi.python.org/pypi/wfastcgi
