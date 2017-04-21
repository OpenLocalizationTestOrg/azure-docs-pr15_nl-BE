#<a name="how-to-use-the-azure-command-line-tools-for-mac-and-linux"></a>Het gebruik van de opdrachtregelprogramma's voor Azure voor Mac en Linux

Deze handleiding wordt beschreven hoe de Azure opdrachtregelprogramma's voor Mac en Linux gebruiken voor het maken en beheren van services in Azure. De scenario's die gedekt zijn **installeert de hulpprogramma's voor** **publicatie-instellingen importeren**, **maken en beheren van Websites met Azure**en **maken en beheren van Azure Virtual Machines**. Zie voor uitgebreide documentatie, [Azure opdrachtregelprogramma voor Mac en Linux documentatie][reference-docs]. 

##<a name="table-of-contents"></a>Inhoudsopgave
* [Wat zijn de Azure opdrachtregelprogramma's voor Mac en Linux](#Overview)
* [Het installeren van de Azure opdrachtregelprogramma's voor Mac en Linux](#Download)
* [Het maken van een account met Azure](#CreateAccount)
* [Het downloaden en importeren in publicatie-instellingen](#Account)
* [Het maken en beheren van een website Azure](#WebSites)
* [Het maken en beheren van een Azure Virtual Machine](#VMs)


<h2><a id="Overview"></a>Wat zijn de Azure opdrachtregelprogramma's voor Mac en Linux</h2>

De Azure opdrachtregelprogramma's voor Mac en Linux zijn een verzameling van opdrachtregelprogramma's voor het implementeren en beheren van Azure services.
 
De ondersteunde taken omvatten het volgende:

* Publicatie-instellingen importeren.
* Maken en beheren van Websites met Azure.
* Maken en beheren van Azure Virtual Machines.

Voor een volledige lijst van ondersteunde opdrachten, typt u `azure -help` op de opdrachtregel na de installatie van de hulpprogramma's, of Raadpleeg de [documentatie]van de[reference-docs].

<h2><a id="Download">Het installeren van de Azure opdrachtregelprogramma's voor Mac en Linux</a></h2>

De volgende lijst bevat informatie voor het installeren van de opdrachtregelprogramma's, afhankelijk van uw besturingssysteem:

* **Mac**: Download de [Installer SDK Azure][mac-installer]. Open het bestand .pkg gedownload en de installatie stappen als u hierom wordt gevraagd.

* **Linux**: Installeer de nieuwste versie van [Node.js] [ nodejs-org] (Zie [Node.js installeren via Package Manager][install-node-linux]), voert u de volgende opdracht:

        npm install azure-cli -g

    **Opmerking**: mogelijk moet u deze opdracht wilt uitvoeren met verhoogde bevoegdheden:

        sudo npm install azure-cli -g

* **Windows**: Voer het installatieprogramma Winows (MSI-bestand), die hier beschikbaar is: [Azure opdrachtregelprogramma's][windows-installer].


Test de installatie, typt u `azure` bij de opdrachtprompt. Als de installatie voltooid is, ziet u een lijst met alle beschikbare `azure` opdrachten.

<h2><a id="CreateAccount"></a>Het maken van een account met Azure</h2>

Gebruik de opdrachtregelprogramma's voor Azure voor Mac en Linux, moet u een account met Azure.

Open een webbrowser en Ga naar [http://www.windowsazure.com] [ windowsazuredotcom] en klikt u op de **gratis proefversie** in de rechterbovenhoek.

![Azure website][Azure Web Site]

Volg de instructies voor het maken van een account.

<h2><a id="Account"></a>Het downloaden en importeren in publicatie-instellingen</h2>

Om te beginnen moet u eerst downloaden en importeren de publicatie-instellingen. Hierdoor kunt u de hulpprogramma's gebruiken voor het maken en beheren van Azure Services. Voor het downloaden van uw publicatie-instellingen, gebruikt u de `account download` opdracht:

    azure account download

Hiermee opent u de browser en gevraagd u aan te melden de Portal beheren. Na het aanmelden, het `.publishsettings` bestand wordt gedownload. Noteer waar dit bestand wordt opgeslagen.

Importeer vervolgens de `.publishsettings` bestand met de volgende opdracht vervangen `{path to .publishsettings file}` door het pad naar uw `.publishsettings` bestand:

    azure account import {path to .publishsettings file}

Kunt u alle informatie die is opgeslagen door de <code>import</code> opdracht met behulp van de <code>account clear</code> opdracht:

    azure account clear

Voor een overzicht van de opties voor het `account` -opdrachten, gebruikt u de `-help` optie:

    azure account -help

Na het importeren van uw publicatie-instellingen, moet u de `.publishsettings` bestand om veiligheidsredenen.

> [AZURE.NOTE] Wanneer u importeert in publicatie-instellingen, de referenties voor toegang tot uw abonnement Azure worden opgeslagen in de `user` map. Uw `user` map wordt beveiligd door het besturingssysteem. Het is echter raadzaam dat u extra stappen voor het coderen van uitvoeren uw `user` map. U kunt dit doen op de volgende manieren:    
> 
> - Eigenschappen van de map wijzigen in Windows, of BitLocker gebruiken.
> - Schakel FileVault voor de map op de Mac.
> - Gebruik de functie gecodeerde Home directory op Ubuntu. Andere Linux-distributies bieden vergelijkbare functies.

U bent nu klaar om te worden maken en beheren van Websites met Azure en Azure virtuele Machines.  

<h2><a id="WebSites"></a>Het maken en beheren van een Website Azure</h2>

###<a name="create-a-website"></a>Een Website maken

Als u wilt een Azure website maakt, maakt u eerst een lege map met de naam `MySite` en blader naar deze map.

Voer de volgende opdracht:

    azure site create MySite --git

De uitvoer van deze opdracht wordt de standaard-URL voor de zojuist gemaakte website bevatten. De `--git` optie kunt u met behulp van git git opslaglocaties maken in zowel uw lokale toepassingsmap en in het datacenter van uw website publiceren op uw website. Houd er rekening mee dat als uw lokale map al een git repository, de opdracht een nieuwe externe wordt toegevoegd aan de bestaande bibliotheek, de bibliotheek in het datacenter van uw website aan te wijzen.

Opmerking: u kunt uitvoeren de `azure site create` opdracht met een van de volgende opties:

* `--location [location name]`. Met deze optie kunt u de locatie van het datacenter waarmee uw website wordt gemaakt (bijvoorbeeld ' West ons'). Als u deze optie weglaat, zult u promted een locatie te kiezen.
* `--hostname [custom host name]`. Met deze optie kunt u een aangepaste naam voor uw website opgeven.

Vervolgens kunt u inhoud toevoegen aan uw website directory. De normale git stroom gebruiken (`git add`, `git commit`) voor het doorvoeren van uw inhoud. Gebruik de volgende opdracht in de git om de inhoud van uw website naar Azure: 

    git push azure master

###<a name="set-up-publishing-from-github"></a>Publicatie van GitHub instellen

Gebruiken om in te stellen op doorlopend publiceren vanaf een opslagplaats van GitHub, de `--GitHub` optie bij het maken van een site:

    auzre site create MySite --github --githubusername username --githubpassword password --githubrepository githubuser/reponame

Als u een lokale kloon van een opslagplaats van GitHub of hebt u een opslagplaats met één externe verwijzing naar een opslagplaats van GitHub, deze opdracht automatisch code in de GitHub opslagplaats aan uw site wilt publiceren. Wijzigingen naar de opslagplaats GitHub geduwd wordt daarna automatisch naar uw site worden gepubliceerd.

Bij het instellen van de publicatie van GitHub is de standaardvertakking die wordt gebruikt in de master branch. Als u een andere vertakking, voeren de volgende opdracht uit uw lokale bibliotheek:

    azure site repository <branch name>

###<a name="configure-app-settings"></a>App-instellingen configureren

Instellingen van de App zijn sleutel / waarde-paren die beschikbaar voor de toepassing in runtime zijn. Als de waarde voor een Website Azure, vervangen app ingestelde waarden instellingen met dezelfde sleutel die zijn gedefinieerd in Web.config-bestand van uw site. Voor Node.js en PHP toepassingen zijn de instellingen app beschikbaar als omgevingsvariabelen. In het volgende voorbeeld ziet u hoe u een sleutel / waarde-paar:

    azure site config add <key>=<value> 

Voor een overzicht van alle sleutel/waarde-paren, gebruikt u het volgende:

    azure site config list 

Of als u weet dat de sleutel en de waarde wilt, kunt u gebruiken:

    azure site config get <key> 

Als u wilt dat de waarde van een bestaande sleutel wijzigen moet u eerst de bestaande sleutel wissen en vervolgens weer toevoegt. De opdracht duidelijk is:

    azure site config clear <key> 

###<a name="list-and-show-sites"></a>En de sites weergeven

U kunt uw websites gebruiken de volgende opdracht:

    azure site list

Als u gedetailleerde informatie over een site, gebruikt de `site show` opdracht. In het volgende voorbeeld ziet u de details voor `MySite`:

    azure site show MySite

###<a name="stop-start-or-restart-a-site"></a>Stoppen, starten of opnieuw starten van een site

Stoppen, starten of opnieuw starten van een site met de `site stop`, `site start`, of `site restart` opdrachten:

    azure site stop MySite
    azure site start MySite
    azure site restart MySite

###<a name="delete-a-site"></a>Een site verwijderen

Ten slotte kunt u een site met de `site delete` opdracht:

    azure site delete MySite

Houd er rekening mee dat als u met een van bovenstaande opdrachten uit in de map waar u hebt `site create`, hoeft u niet de naam van de site `MySite` als de laatste parameter.

Voor een volledig overzicht van de `site` -opdrachten, gebruikt u de `-help` optie:

    azure site -help 

<h2><a id="VMs"></a>Het maken en beheren van een Azure Virtual Machine</h2>

een Azure Virtual Machine is vanaf een virtuele machine-installatiekopie (VHD-bestand) die u verstrekt of die beschikbaar is in de afbeelding galerie gemaakt. Overzicht van afbeeldingen die beschikbaar zijn, gebruikt u de `vm image list` opdracht:

    azure vm image list

U kunt inrichten en een virtuele machine start vanaf een van de beschikbare afbeeldingen met de `vm create` opdracht. In het volgende voorbeeld ziet u hoe een Linux virtuele machine maken (genoemd `myVM`) van een afbeelding in de galerie met afbeelding (CentOS 6.2). De naam van de gebruiker root en het wachtwoord voor de virtuele machine worden `myusername` en `Mypassw0rd` respectievelijk. (Houd er rekening mee dat de `--location` parameter geeft het datacenter waar de virtuele machine wordt gemaakt. Als u geen waarde opgeeft de `--location` parameter, wordt u gevraagd een locatie te kiezen.)

    azure vm create myVM OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd myusername --location "West US"

Kunt u overwegen dat de `--ssh` vlag (Linux) of `--rdp` vlag (Windows) op `vm create` inschakelen van externe verbindingen met de zojuist gemaakte virtuele machine.

Als u liever een virtuele machine van een aangepaste afbeelding inrichten zou, kunt u een afbeelding uit een VHD-bestand met de `vm image create` opdracht en gebruik vervolgens de `vm create` opdracht voor het inrichten van de virtuele machine. In het volgende voorbeeld ziet u hoe een Linux-afbeelding maken (genoemd `myImage`) van een lokale VHD-bestand. (De `--location` parameter geeft de gegevens waarop de afbeelding wordt opgeslagen.)

    azure vm image create myImage /path/to/myImage.vhd --os linux --location "West US"

In plaats van een afbeelding maken van een lokale VHD, kunt u een afbeelding maken van een VHD opgeslagen in Azure Blob-opslag. U kunt hiervoor met de `blob-url` parameter:

    azure vm image create myImage --blob-url <url to .vhd in Blob Storage> --os linux

Na het maken van een afbeelding, kunt u een virtuele machine uit de afbeelding inrichten met behulp van `vm create`. De volgende opdracht maakt u een virtuele machine genoemd `myVM` uit de bovenstaande afbeelding (`myImage`).

    azure vm create myVM myImage myusername --location "West US"

Nadat u een virtuele machine hebt ingericht, kunt u eindpunten (bijvoorbeeld) voor externe toegang tot uw virtuele machine maken. In het volgende voorbeeld wordt de `vm create endpoint` de opdracht voor het openen van externe poort 22 en lokale poort 22 op `myVM`:

    azure vm endpoint create myVM 22 22

U kunt gedetailleerde informatie opvragen over een virtuele machine (inclusief IP-adres, DNS-naam en eindpunt informatie) met de `vm show` opdracht:

    azure vm show myVM

Start, afsluiten, of de virtuele machine opnieuw opstarten, gebruikt u een van de volgende opdrachten:

    azure vm shutdown myVM
    azure vm start myVM
    azure vm restart myVM

En ten slotte de VM verwijderen, de `vm delete` opdracht:

    azure vm delete myVM

Gebruik voor een volledige lijst met opdrachten voor het maken en beheren van virtuele machines, de `-h` optie:

    azure vm -h

<!-- IMAGES -->
[Azure Web Site]: ./media/crossplat-cmd-tools/freetrial.png

<!-- LINKS -->
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[windows-installer]: http://go.microsoft.com/fwlink/?LinkID=275464
[reference-docs]: http://go.microsoft.com/fwlink/?LinkId=252246
[windowsazuredotcom]: http://www.windowsazure.com

