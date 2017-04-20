Het implementatiescript wordt maken van de virtuele omgeving overgeslagen op Azure wanneer wordt vastgesteld dat er al een virtuele omgeving die compatibel is.  Dit kan aanzienlijk implementatie versnellen.  Pakketten die al zijn geïnstalleerd wordt door pip overgeslagen.

In bepaalde situaties kunt u ervoor zorgen dat virtuele omgeving te verwijderen.  U moet dit doen als u een virtuele omgeving opnemen als onderdeel van uw bibliotheek.  U kunt dit doen als u wilt verwijderen van bepaalde pakketten of wijzigingen in de requirements.txt te testen.

Er zijn een aantal opties voor het beheer van de bestaande virtuele omgeving op Azure:

### <a name="option-1-use-ftp"></a>Optie 1: FTP gebruiken

Met een FTP-client kunt verbinding met de server en u de env-map te verwijderen.  Overigens sommige FTP-clients (zoals webbrowsers) is alleen-lezen en mappen te verwijderen, dus wilt u controleren of een FTP-client gebruiken met die mogelijkheid niet mogelijk.  De naam van de FTP-host en de gebruiker worden weergegeven in uw webtoepassing blade van de [Azure Portal](https://portal.azure.com).

### <a name="option-2-toggle-runtime"></a>Optie 2: In-of uitschakelen-runtime

Hier is een alternatief dat u van het feit profiteert dat het implementatiescript de env map verwijdert wanneer deze niet overeenkomt met de gewenste versie van Python.  Dit wordt effectief verwijderen van de bestaande omgeving en een nieuwe maken.

1. Overschakelen naar een andere versie van Python (via runtime.txt of de bladeserver **Toepassingsinstellingen** in Azure Portal)
1. GIT push enkele wijzigingen (pip installatie fouten negeren indien van toepassing)
1. Ga terug naar de eerste versie van Python
1. GIT push enkele wijzigingen opnieuw

### <a name="option-3-customize-deployment-script"></a>Optie 3: Implementatiescript aanpassen

Als u een script voor de implementatie hebt aangepast, kunt u de code in de deploy.cmd te dwingen de env-map te verwijderen.
