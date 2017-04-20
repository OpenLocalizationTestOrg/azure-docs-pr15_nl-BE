Sommige pakketten mogelijk niet geïnstalleerd met pip wanneer uitgevoerd op Azure.  Eenvoudig kan het zijn dat het pakket niet beschikbaar op de Index Python pakket is.  Is het mogelijk dat een compiler vereist is (een compiler is niet beschikbaar op de computer waarop de web app in Azure App-Service).

In dit gedeelte bekijken we manieren om de behandeling van dit vraagstuk.

### <a name="request-wheels"></a>Aanvraag wielen

Als de installatie van het pakket een compiler vereist, kunt u contact opnemen met de eigenaar van het pakket om te vragen dat wielen worden beschikbaar gesteld voor het pakket moet proberen.

Met de recente beschikbaarheid van [Microsoft Visual C++ Compiler voor Python 2.7][]is het nu gemakkelijker te maken van pakketten met native code voor Python 2.7.

### <a name="build-wheels-requires-windows"></a>Wielen bouwen (vereist voor Windows)

Opmerking: Als u deze optie gebruikt, zorg ervoor dat het pakket een met Python gebruikt die overeenkomt met het platform/architectuur/versie die wordt gebruikt op de web-app in de App-Service Azure compileren (Windows/32-bit/2.7 of 3.4).

Als het pakket niet geïnstalleerd omdat hiervoor een compiler, kunt u de compiler installeren op uw lokale computer en een wiel voor het pakket, u ook in uw bibliotheek opnemen wordt bouwen.

Mac/Linux-gebruikers: Als u geen toegang tot een Windows-computer hebt, Zie [maken van een virtuele Machine waarop Windows][] voor informatie over het maken van een VM in Azure.  U kunt deze wielen bouwen, deze toevoegen aan de bibliotheek en de VM als u dit wilt negeren. 

2.7 Python, kunt u [Microsoft Visual C++ Compiler voor Python 2.7][]installeren.

Voor 3.4 Python, kunt u [Microsoft Visual C++ 2010 Express][]installeren.

Wielen bouwen, moet u het pakket wiel:

    env\scripts\pip install wheel

Gebruikt u `pip wheel` voor het compileren van een afhankelijkheid:

    env\scripts\pip wheel azure==0.8.4

Hiermee maakt u een bestand .whl in de map \wheelhouse.  De \wheelhouse map en stuur de bestanden toevoegen aan uw bibliotheek.

Bewerken van uw requirements.txt toe te voegen de `--find-links` optie boven. Zo weet pip om te zoeken naar een exacte overeenkomst in de lokale map voordat u de index python pakket.

    --find-links wheelhouse
    azure==0.8.4

Als u wilt dat alle afhankelijkheden in uw opnemen in de map \wheelhouse en de index python pakket helemaal niet gebruikt, kunt u forceren pip de index van het pakket wordt genegeerd door toe te voegen `--no-index` tot de bovenkant van de requirements.txt.

    --no-index

### <a name="customize-installation"></a>Installatie aanpassen

U kunt het script implementeren om een pakket te installeren in de virtuele omgeving met een ander installatieprogramma, zoals eenvoudig\_installeren.  Zie deploy.cmd voor een voorbeeld van is toegelicht.  Zorg ervoor dat deze pakketten zijn niet in requirements.txt opgenomen, om te voorkomen dat pip installeren.

Dit toevoegen aan het implementatiescript:

    env\scripts\easy_install somepackage

Mogelijk ook eenvoudig te gebruiken\_te installeren vanaf een exe installer (sommige zijn zip-compatibel, dus eenvoudig\_installatie worden ondersteund).  Het installatieprogramma naar uw bibliotheek toevoegen en gemakkelijk oproepen\_installeren door het pad naar het uitvoerbare bestand.

Dit toevoegen aan het implementatiescript:

    env\scripts\easy_install "%DEPLOYMENT_SOURCE%\installers\somepackage.exe"

### <a name="include-the-virtual-environment-in-the-repository-requires-windows"></a>De virtuele omgeving opnemen in de bibliotheek (Windows vereist)

Opmerking: Als u deze optie gebruikt, moet u een virtuele omgeving die overeenkomt met het platform/architectuur/versie die wordt gebruikt op het web app in Azure App-Service (Windows/32-bit/2.7 of 3.4).

Als u de virtuele omgeving in de bibliotheek opneemt, kunt u voorkomen dat het implementatiescript beheer virtuele omgeving op Azure doen door een leeg bestand te maken:

    .skipPythonDeployment

Wij raden aan dat u de bestaande virtuele omgeving op de app, om te voorkomen dat de overgebleven bestanden uit wanneer de virtuele omgeving kan worden beheerd automatisch te verwijderen.


[Maak een virtuele Machine met Windows]: http://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/
[Microsoft Visual C++ Compiler voor Python 2.7]: http://aka.ms/vcpython27
[Microsoft Visual C++ 2010 Express]: http://go.microsoft.com/?linkid=9709949
