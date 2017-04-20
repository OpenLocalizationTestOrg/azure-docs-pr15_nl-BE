<properties
    pageTitle="Installeer Python en de SDK - Azure"
    description="Informatie over het installeren van Python en de SDK voor gebruik met Azure."
    services=""
    documentationCenter="python"
    authors="lmazuel"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="lmazuel"/>

# <a name="installing-python-and-the-sdk"></a>Python en de SDK installeren

Python is eenvoudig te setup op Windows en Mac, Linux en [Bash voor Windows](https://msdn.microsoft.com/commandline/wsl/about)is voorgeïnstalleerd. Deze handleiding begeleidt u bij de installatie en de computer gereed voor gebruik met Azure ophalen.

## <a name="whats-in-the-python-azure-sdk"></a>Wat is in de Python Azure SDK?

De SDK Azure voor Python bevat onderdelen waarmee u kunt ontwikkelen, implementeren en beheren van toepassingen voor Azure Python. De Azure SDK for Python bevat de volgende gegevens op:

* **Beheer van bibliotheken**. Deze klassenbibliotheken bieden een interface Azure bronnen, zoals opslag, virtuele machines te beheren.

* **Runtime-bibliotheken**. Deze klassenbibliotheken bieden een interface voor toegang tot Azure-functies, zoals opslag- en bus.

## <a name="which-python-and-which-version-to-use"></a>Welke Python en welke versie u wilt gebruiken

Er zijn verschillende soorten van Python interpreters beschikbaar - voorbeelden zijn:

* CPython - de standaard en meest gebruikte Python-interpreter
* PyPy - snelle, compatibel is met alternatieve implementatie op CPython
* IronPython - Python interpreter die compatibel is met .net CLR /
* Jython - Python interpreter die wordt uitgevoerd op de Java Virtual Machine

**CPython** v2.7 zijn of v3.3 + PyPy 5.4.0 getest en de Python Azure SDK wordt ondersteund.

## <a name="where-to-get-python"></a>Waar u Python?

Er zijn verschillende manieren om CPython:

* Rechtstreeks vanuit [www.python.org][]
* Van een betrouwbare distro zoals [www.continuum.io][], [www.enthought.com][] of [www.activestate.com][]
* Bouwen van bron!

Tenzij u een specifieke behoefte hebt, raden we de eerste twee opties.

## <a name="sdk-installation-on-windows-linux-and-macos-client-libraries-only"></a>Installatie van de SDK voor Windows, Linux en MacOS (clientbibliotheken)

Als u al Python is geïnstalleerd, kunt u pip een bundel van alle clientbibliotheken in uw bestaande Python 2.7 of Python 3.3 + omgeving te installeren. Dit zal de pakketten downloaden van [Python pakket Index][] (PyPI).

Mogelijk hebt u beheerdersrechten nodig:

- Linux en MacOS, gebruikt u de `sudo` opdracht: `sudo pip install azure-mgmt-compute`.
- Windows: de PowerShell-opdracht prompt openen als beheerder

U kunt individueel in elke bibliotheek voor elke Azure service installeren:

```console
   $ pip install azure-batch          # Install the latest Batch runtime library
   $ pip install azure-mgmt-scheduler # Install the latest Storage management library
```

Voorbeeld-pakketten worden geïnstalleerd met behulp van de `--pre` vlag:

```console
   $ pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

U kunt ook een set van Azure bibliotheken installeren in een enkele regel met de `azure` meta-pakket. Aangezien niet alle pakketten in deze meta-pakket worden gepubliceerd als stabiel nog, de `azure` meta-pakket is nog steeds in de voorbeeldweergave. Echter, de core-pakketten van code kwaliteit/volledigheid perspectieven kunnen worden beschouwd als "stabiel" op dit moment
- het officieel heet als zodanig synchroon met andere talen zo snel mogelijk. We hebben geen plannen van verdere belangrijke wijzigingen die tot die tijd.

Aangezien dit een preview-versie, moet u de `--pre` vlag:

```console
   $ pip install --pre azure
```
   
of direct

```console
   $ pip install azure==2.0.0rc6
```

## <a name="getting-more-packages"></a>Meer pakketten ophalen

De [Python pakket Index][] (PyPI) heeft een uitgebreide selectie van Python bibliotheken.  Als u ervoor een Distro te installeren kiest, hebt u al de meeste van de interessante wetenswaardigheden voor verschillende scenario's van webontwikkeling technische computerbewerkingen.


## <a name="python-tools-for-visual-studio"></a>Python-hulpprogramma's voor Visual Studio

[Python-hulpprogramma's voor Visual Studio][] (PTVS) is een gratis/OSS-invoegtoepassing van Microsoft VS in een volwaardige Python IDE verandert:

![How-naar-installatie-python-ptvs](./media/python-how-to-install/how-to-install-python-ptvs.png)

Met behulp van PTVS is optioneel, maar wordt aanbevolen als u kunt ondersteuning voor Python en Project /-oplossing, debuggen, profilering, interactieve venster, sjabloon bewerken en Intellisense.

PTVS ook eenvoudig te implementeren op Microsoft Azure, met ondersteuning voor de implementatie voor [Cloud Services][] en [Websites][].

PTVS werkt met uw bestaande Visual Studio 2013 en 2015 installatie.  Zie voor documentatie, downloads en discussies, [Python Tools voor Visual Studio].  

## <a name="python-azure-scenarios-for-linux-and-macos"></a>Python Azure scenario's voor Linux en MacOS

Voor Linux of MacOS, belangrijkste Azure scenario's die worden ondersteund:

1. Azure Services die door de client libraries voor Python

2. Uw app uitgevoerd in een Linux-VM

3. Ontwikkelen en publiceren naar Azure Websites met Git

Het eerste scenario kunt u uitgebreide web apps die gebruik maken van de mogelijkheden van Azure PaaS zoals [blob-opslag][], [opslag van de wachtrij][], [tabelopslag][] etc. via Pythonic wrappers voor de Azure REST API's te ontwerpen. Deze werkt identiek voor Windows, Mac en Linux.  Ook kunt u deze clientbibliotheken van de ontwikkeling van lokale computer of een Linux VM op Azure.

Voor het scenario VM u gewoon een VM Linux van uw keuze (Ubuntu, CentOS, Suse) starten en uitvoeren/beheren wat u bevalt.  U kunt bijvoorbeeld [IPython][] REPL/laptop uitvoeren op uw computer Windows/Mac/Linux en wijs uw browser naar een Linux of Windows multi-proc VM met de IPython-Engine op Azure. Zie de [Laptop op Azure IPython][] -zelfstudie voor meer informatie.

Zie voor meer informatie over de installatie van een Linux VM zelfstudie voor het [maken van een virtuele Machine wordt uitgevoerd op Linux][] .

Git-implementatie met, als u een webtoepassing Python ontwikkelen en publiceren op een Website Azure op een ander besturingssysteem.  Wanneer u de bibliotheek push naar Azure, maakt automatisch een virtuele omgeving en pip uw vereiste pakketten worden geïnstalleerd.

Zie voor meer informatie over het ontwerpen en publiceren van Websites Azure de zelfstudies voor [Websites maken met Django][], [Websites maken met fles][]en [Websites maken met de kolf][]. Zie voor meer algemene informatie over het gebruik van een WSGI-compatibele framework [Python met Azure Websites configureren][].


## <a name="additional-software-and-resources"></a>Extra Software en bronnen:

* [Azure SDK for Python ReadTheDocs](http://azure-sdk-for-python.readthedocs.io/en/latest/)
* [Azure SDK for Python Github](https://github.com/Azure/azure-sdk-for-python)
* [Officiële Azure monsters voor Python](https://azure.microsoft.com/documentation/samples/?platform=python)
* [Continuüm met Analytics Python distributie][]
* [Python Enthought-distributie][]
* [Python ActiveState distributie][]
* [SciPy - een suite van Python wetenschappelijke bibliotheken][]
* [NumPy - een bibliotheek van numerieke waarden voor Python][]
* [Django Project - een volwassen web framework/CMS][]
* [IPython - een geavanceerde REPL/laptop voor Python][]
* [Laptop op Azure IPython][]
* [Python-hulpprogramma's voor Visual Studio op GitHub][]
* [Python voor ontwikkelaars](/develop/python/)

[Continuüm met Analytics Python distributie]: http://continuum.io
[Python Enthought-distributie]: http://www.enthought.com
[Python ActiveState distributie]: http://www.activestate.com
[www.Python.org]: http://www.python.org
[www.continuum.IO]: http://continuum.io
[www.enthought.com]: http://www.enthought.com
[www.ActiveState.com]: http://www.activestate.com
[SciPy - een suite van Python wetenschappelijke bibliotheken]: http://www.scipy.org
[NumPy - een bibliotheek van numerieke waarden voor Python]: http://www.numpy.org
[Django Project - een volwassen web framework/CMS]: http://www.djangoproject.com
[IPython - een geavanceerde REPL/laptop voor Python]: http://ipython.org
[IPython]: http://ipython.org
[Laptop op Azure IPython]: virtual-machines-linux-jupyter-notebook.md
[Cloud Services]: cloud-services-python-ptvs.md
[Websites]: web-sites-python-ptvs-django-mysql.md
[Python-hulpprogramma's voor Visual Studio]: http://aka.ms/ptvs
[Python-hulpprogramma's voor Visual Studio op GitHub]: https://github.com/microsoft/ptvs
[Python pakket Index]: http://pypi.python.org/pypi
[Microsoft Azure SDK for Python 2.7]: http://go.microsoft.com/fwlink/?LinkId=254281
[Microsoft Azure SDK for Python 3.4]: http://go.microsoft.com/fwlink/?LinkID=516990
[Setting up a Linux VM via the Azure portal]: create-and-configure-opensuse-vm-in-portal.md
[How to use the Azure Command-Line Interface]: crossplat-cmd-tools.md
[Maak een virtuele Machine waarop Linux]: virtual-machines-linux-quick-create-cli.md
[Websites maken met Django]: web-sites-python-create-deploy-django-app.md
[Websites maken met fles]: web-sites-python-create-deploy-bottle-app.md
[Websites maken met de kolf]: web-sites-python-create-deploy-flask-app.md
[Python met Azure Websites configureren]: web-sites-python-configure.md
[tabelopslag]: storage-python-how-to-use-table-storage.md
[wachtrij-opslag]: storage-python-how-to-use-queue-storage.md
[BLOB-opslag]: storage-python-how-to-use-blob-storage.md
