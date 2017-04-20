<properties
    pageTitle="Een Jupyter/IPython notitieblok maken | Microsoft Azure"
    description="Informatie over het implementeren van de Jupyter IPython/laptop op een Linux virtuele machine gemaakt met de resource manager implementatiemodel in Azure."
    services="virtual-machines-linux"
    documentationCenter="python"
    authors="crwilcox"
    manager="wpickett"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="python"
    ms.topic="article"
    ms.date="11/10/2015"
    ms.author="crwilcox"/>

# <a name="jupyter-notebook-on-azure"></a>Jupyter-laptop op Azure

Het [Jupyter project](http://jupyter.org), voorheen de [IPython-project](http://ipython.org)bevat een verzameling hulpprogramma's voor wetenschappelijke computing met behulp van krachtige interactieve houders die code kan worden uitgevoerd met het maken van een levend document rekenkundige combineren. Deze laptop-bestanden kunnen bevatten, willekeurige tekst, wiskundige formules, code invoer, resultaten, afbeeldingen, video's en andere soorten media dat een moderne webbrowser kan weergeven is. Of u nog nooit een absoluut Python en deze informatie in een leuke, interactieve omgeving of doen sommige ernstige parallel en technische computing, is de laptop Jupyter een geweldige keuze.

![Screenshot](./media/virtual-machines-linux-jupyter-notebook/ipy-notebook-spectral.png) met behulp van SciPy en Matplotlib-pakketten voor het analyseren van de structuur van een opnemen van geluid.


## <a name="jupyter-two-ways-azure-notebooks-or-custom-deployment"></a>Jupyter twee manieren: Azure-laptops of aangepaste implementatie
Azure biedt een service die u om [snel te gebruiken Jupyter gebruiken kunt ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).  Met behulp van de laptop Azure Service, kunt u snel toegang krijgen tot de Jupyter van toegankelijke web interface voor schaalbare rekenkracht met de kracht van Python en de vele bibliotheken.  Aangezien de installatie wordt uitgevoerd door de service, kunnen gebruikers toegang tot deze bronnen zonder de noodzaak voor beheer en configuratie van de gebruiker.

Blijf doorgaan met het lezen van dit artikel waarop zal het implementeren van de laptop Jupyter op Microsoft Azure, met behulp van Linux virtuele machines (VMs) als de laptop-service niet voor uw scenario werkt.

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="create-and-configure-a-vm-on-azure"></a>Maken en configureren van een VM in Azure

De eerste stap is het maken van een virtuele machine (VM) uitgevoerd op Azure.
Deze VM is een compleet besturingssysteem in de cloud en wordt gebruikt voor het uitvoeren van de Jupyter-laptop. Azure is kan worden uitgevoerd, zowel Linux als Windows virtuele machines en wordt ingegaan op de instellingen van de Jupyter op beide soorten virtuele machines.

### <a name="create-a-linux-vm-and-open-a-port-for-jupyter"></a>Maak een Linux VM en een poort openen voor Jupyter

Volg de instructies gegeven [hier] [ portal-vm-linux] voor het maken van een virtuele machine van de *Ubuntu* verdeling. In deze zelfstudie wordt Ubuntu Server 14.04 LTS. We gaan ervan uit dat de gebruiker de naam *azureuser*.

Nadat u de virtuele machine wordt uitgevoerd, implementeert moeten we een regel in de netwerkgroep te openen.  Ga naar **Netwerk-beveiligingsgroepen** en opent u het tabblad voor de beveiligingsgroep die overeenkomt met uw VM vanaf de Azure portal. U moet een binnenkomende regel toevoegen met de volgende instellingen: **TCP** voor het protocol, **\*** voor de bronpoort (openbaar) en **9999** voor de bestemmingshaven (particulier).

![Schermafbeelding](./media/virtual-machines-linux-jupyter-notebook/azure-add-endpoint.png)

Terwijl u in uw netwerk-beveiligingsgroep, klik op **Netwerkinterfaces** en noteer het **Openbare IP-adres** als dat nodig is verbinding maken met uw VM in de volgende stap.

## <a name="install-required-software-on-the-vm"></a>Vereiste software installeren op de VM

De laptop Jupyter op onze VM uitgevoerd, moeten we eerst Jupyter en de bijbehorende afhankelijkheden installeren. Verbinding maken met uw linux vm met behulp van ssh en de gebruikersnaam en het wachtwoord koppelen u hebt gekozen tijdens het maken van de vm. In deze zelfstudie wordt stopverf gebruiken en verbinding maken vanuit Windows.

### <a name="installing-jupyter-on-ubuntu"></a>Jupyter installeren op Ubuntu
Anaconda, een populaire wetenschap python gegevensdistributie, met een van de koppelingen van [Continuüm met Analytics](https://www.continuum.io/downloads)installeert.  Op het schrijven van dit document, de onderstaande koppelingen zijn de meeste datum-versies.

#### <a name="anaconda-installs-for-linux"></a>Anaconda-installaties voor Linux
<table>
  <th>3.4 Python</th>
  <th>Python-2.7</th>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64-bits</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64-bits</href>
    </td>
  </tr>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32-bits</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32-bits</href>
    </td>  
  </tr>
</table>


#### <a name="installing-anaconda3-230-64-bit-on-ubuntu"></a>Installeren van Anaconda3 2.3.0 64-bits op Ubuntu
Als voorbeeld is dit hoe u Anaconda kunt installeren op Ubuntu

    # install anaconda
    cd ~
    mkdir -p anaconda
    cd anaconda/
    curl -O https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh
    sudo bash Anaconda3-2.3.0-Linux-x86_64.sh -b -f -p /anaconda3

    # clean up home directory
    cd ..
    rm -rf anaconda/

    # Update Jupyter to the latest install and generate its config file
    sudo /anaconda3/bin/conda install jupyter -y
    /anaconda3/bin/jupyter-notebook --generate-config


![Schermafbeelding](./media/virtual-machines-linux-jupyter-notebook/anaconda-install.png)


### <a name="configuring-jupyter-and-using-ssl"></a>Jupyter configureren en het gebruik van SSL
Na de installatie moeten we even de tijd om de configuratiebestanden voor Jupyter. Als u dreigen ondervindt met Jupyter is het wellicht nuttig te kijken naar de [Documentatie van de Jupyter voor het uitvoeren van een laptop-Server](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html)configureren.

Volgende we `cd` naar onze SSL-certificaat maken en bewerken van het configuratiebestand profielen de map met het gebruikersprofiel.

Gebruik de volgende opdracht op Linux.

    cd ~/.jupyter

Gebruik de volgende opdracht voor het maken van het SSL-certificaat (Linux en Windows).

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Houd er rekening mee dat omdat u een zelfondertekend certificaat voor SSL, wanneer de browser verbinding te maken met de laptop u een beveiligingswaarschuwing krijgt.  Voor langdurig gebruik voor productie zult u een correct ondertekende certificaat is gekoppeld aan uw organisatie gebruiken.  Aangezien Certificaatbeheer buiten het bestek van deze demo valt, zullen we nu een zelfondertekend certificaat houden.

Naast een certificaat gebruikt, bieden u ook een wachtwoord voor uw laptop te beveiligen tegen onbevoegd gebruik.  Om veiligheidsredenen Jupyter gebruik van gecodeerde wachtwoorden in het configuratiebestand zodat moet u eerst uw wachtwoord coderen.  IPython beschikt over een hulpprogramma om te doen; Voer de volgende opdracht bij een opdrachtprompt.

    /anaconda3/bin/python -c "import IPython;print(IPython.lib.passwd())"

Dit vraagt u om een wachtwoord en de bevestiging en wordt vervolgens het wachtwoord afdrukken. Let op de volgende stap.

    Enter password:
    Verify password:
    sha1:b86e933199ad:a02e9592e59723da722.. (elided the rest for security)

Vervolgens bewerken we configuratiebestand van het profiel, wordt de `jupyter_notebook_config.py` bestand in de map in.  Opmerking dat dit bestand bestaat niet--alleen maken als dat het geval is.  Dit bestand heeft een aantal velden en standaard alle uit zijn toegelicht.  U kunt dit bestand openen met een teksteditor van uw eigen smaak en moet u ervoor zorgen dat zij ten minste de volgende inhoud heeft. **Zorg ervoor dat u de c.NotebookApp.password in het bestand Config.sys met de sha1 uit de vorige stap vervangen**.

    c = get_config()

    # You must give the path to the certificate file.
    c.NotebookApp.certfile = u'/home/azureuser/.jupyter/mycert.pem'

    # Create your own password as indicated above
    c.NotebookApp.password = u'sha1:b86e933199ad:a02e9592e5 etc... '

    # Network and browser details. We use a fixed port (9999) so it matches
    # our Azure setup, where we've allowed traffic on that port
    c.NotebookApp.ip = '*'
    c.NotebookApp.port = 9999
    c.NotebookApp.open_browser = False

### <a name="run-the-jupyter-notebook"></a>De laptop Jupyter uitvoeren

We zijn nu klaar om de Jupyter-laptop. Ga hiervoor naar de map die u wilt opslaan van laptops in en start de server Jupyter laptop met de volgende opdracht.

    /anaconda3/bin/jupyter-notebook

Nu moet u toegang kunnen krijgen tot uw laptop Jupyter op het adres `https://[PUBLIC-IP-ADDRESS]:9999`.

Als u eerst uw laptop, op de pagina inloggen om uw wachtwoord gevraagd. En als u zich aanmeldt, ziet u het 'Jupyter laptop Dashboard", die het midden van de ontrole voor alle bewerkingen van de laptop is.  U kunt op deze pagina nieuwe laptops maken en bestaande bestanden openen.

![Schermafbeelding](./media/virtual-machines-linux-jupyter-notebook/jupyter-tree-view.png)

### <a name="using-the-jupyter-notebook"></a>Met behulp van de laptop Jupyter

Als u op de knop **Nieuw** klikt, ziet u de volgende pagina.

![Schermafbeelding](./media/virtual-machines-linux-jupyter-notebook/jupyter-untitled-notebook.png)

Het gebied gemarkeerd met een `In []:` wordt het invoergebied en hier kunt u een willekeurige geldige Python code typen en deze wordt uitgevoerd wanneer u raakt `Shift-Enter` of klik op het pictogram 'Play' (de rechts wijzende driehoekje in de werkbalk).

## <a name="a-powerful-paradigm-live-computational-documents-with-rich-media"></a>Een krachtige paradigma: rekenkundige documenten met uitgebreide media live

De laptop zelf erop vertrouwen dat het heel natuurlijk voor iedereen die heeft Python en een tekstverwerker gebruikt omdat het een combinatie van beide in sommige opzichten is: blokken Python code kan worden uitgevoerd, maar u kunt ook behouden notities en andere tekst door de stijl van een cel naar 'Prijsverlaging' van 'Code' met het menu op de werkbalk.

Jupyter is veel meer dan een tekstverwerker omdat het mengen van berekening en rijke media (tekst, afbeeldingen, video en vrijwel alles wat die een moderne webbrowser kunt weergeven). U kunt mengen, tekst, code, video's en meer!

![Schermafbeelding](./media/virtual-machines-linux-jupyter-notebook/jupyter-editing-experience.png)

En met de kracht van de vele uitstekende bibliotheken van Python voor wetenschappelijke en technische computing, in de volgende schermafdruk dat een eenvoudige berekening kan worden uitgevoerd met het net zo gemakkelijk als een complex netwerk analyse, allemaal in één omgeving.

Dit paradigma van de kracht van het moderne web mengen met live berekening biedt tal van mogelijkheden en is ideaal voor de wolk; de laptop kan worden gebruikt:

* Als een rekenkundige Kladblok voor het vastleggen van experimentele werken op een probleem.

* Resultaten delen met collega's, in rekenkundige formulier 'live' of in de papier-indeling (HTML-, PDF-bestand).

* Te distribueren en te presenteren live onderwijs materialen met betrekking tot de berekening, zodat leerlingen direct met de echte code experimenteren kunnen wijzigen en opnieuw interactief uitvoeren.

* "Uitvoerbare papers" die de resultaten van onderzoek te op een manier die kan worden onmiddellijk opgenomen presenteren, gevalideerd en uitgebreid door anderen bieden.

* Als een platform voor collaborative computing: meerdere gebruikers op dezelfde server voor het delen van een rekenkundige livesessie laptop kunnen aanmelden.


Als u de verbinding met de IPython bron code [repository][], vindt u een hele map met voorbeelden van laptop die u kunt downloaden en vervolgens met experimenteren op uw eigen Jupyter Azure VM.  Download gewoon de `.ipynb` bestanden van de site en ze uploaden naar het dashboard van uw notitieblok Azure VM (of deze rechtstreeks in de VM downloaden).

## <a name="conclusion"></a>Conclusie

De laptop Jupyter biedt een krachtige interface voor toegang tot interactief de kracht van het ecosysteem Python op Azure.  Dit omvat een groot aantal gevallen gebruik inclusief eenvoudig te verkennen en leren van Python, gegevensanalyse en visualisatie, simulatie en parallel computing. De resulterende notitieblok-documenten bevatten een volledige record van de berekeningen die worden uitgevoerd en kan worden gedeeld met andere gebruikers van Jupyter.  De laptop Jupyter kan worden gebruikt als een lokale toepassing, maar het is ideaal voor implementaties op Azure cloud

De kernfuncties van Jupyter zijn ook beschikbaar in Visual Studio via [Python Tools voor Visual Studio][] (PTVS). PTVS is een gratis en open-source-invoegtoepassing van Microsoft waarmee Visual Studio in een geavanceerde Python development environment met een geavanceerde editor met IntelliSense foutopsporing, verandert profileren en parallel computing integratie.

## <a name="next-steps"></a>Volgende stappen

Zie de [Python Developer Center](/develop/python/)voor meer informatie.

[portal-vm-linux]: https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-tutorial-portal-rm/
[opslagplaats]: https://github.com/ipython/ipython
[Python-hulpprogramma's voor Visual Studio]: http://aka.ms/ptvs
