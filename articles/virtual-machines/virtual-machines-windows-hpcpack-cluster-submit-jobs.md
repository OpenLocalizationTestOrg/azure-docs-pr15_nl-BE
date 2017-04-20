<properties
 pageTitle="Verzenden taken naar een Pack HPC-cluster in Azure | Microsoft Azure"
 description="Meer informatie over het instellen van een computer op ruimten taken kunnen verzenden naar een in Azure Pack HPC-cluster"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="10/14/2016"
 ms.author="danlep"/>

# <a name="submit-hpc-jobs-from-an-on-premises-computer-to-an-hpc-pack-cluster-deployed-in-azure"></a>HPC taken van een computer op het bedrijf aan een geïmplementeerd in Azure Pack HPC-cluster

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Een clientcomputer op ruimten verzenden taken naar een cluster met [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) in Azure configureren. In dit artikel wordt beschreven hoe u voor het instellen van een lokale computer met client extra taak verzenden via HTTPS met het cluster in Azure. Op deze manier kunnen verschillende gebruikers van cluster taken naar een cluster met cloud-gebaseerde HPC-Pack, maar zonder een rechtstreekse verbinding met het hoofdkantoor VM-knooppunt of een Azure-abonnement toegang krijgen tot indienen.

![Een taak aan een cluster in Azure verzenden][jobsubmit]

## <a name="prerequisites"></a>Vereisten

* **HPC Pack head knooppunt geïmplementeerd in een VM Azure** - aangeraden geautomatiseerde hulpmiddelen zoals een [Azure quickstart sjabloon](https://azure.microsoft.com/documentation/templates/) of een [Azure PowerShell script](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) te gebruiken voor de implementatie van het hoofd-knooppunt en de cluster. U moet de DNS-naam van het knooppunt head en de referenties van een Clusterbeheer voor het voltooien van de stappen in dit artikel.

* **Client-computer** - u moet een Windows- of Windows Server-clientcomputer waarop HPC Pack client utilities (Zie [systeemvereisten](https://technet.microsoft.com/library/dn535781.aspx)). Als u alleen het webportaal HPC Pack of de REST API gebruikt wilt voor het indienen van projecten, kunt u een willekeurige computer van uw keuze.

* **Installatiemedia HPC Pack** - installeren van de HPC Pack client-hulpprogramma's, het vrije installatiepakket voor de meest recente versie van HPC Pack (HPC Pack 2012 R2) is beschikbaar via het [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024). Zorg ervoor dat u dezelfde versie van HPC Pack dat is geïnstalleerd op het hoofd knooppunt VM downloaden.

## <a name="step-1-install-and-configure-the-web-components-on-the-head-node"></a>Stap 1: Installeren en configureren van de web components op het knooppunt head

Als u een interface voor overige taken kunnen verzenden naar het cluster via HTTPS, ervoor te zorgen dat de web components van HPC Pack zijn geconfigureerd op het knooppunt van HPC Pack-hoofd. Als ze nog niet geïnstalleerd is, moet u eerst de web components installeren door het bestand HpcWebComponents.msi installatie. Configureer vervolgens de onderdelen door het HPC PowerShell script **Set-HPCWebComponents.ps1**uit te voeren.

Zie voor gedetailleerde procedures [de webcomponenten van Microsoft HPC Pack installeren](http://technet.microsoft.com/library/hh314627.aspx).

>[AZURE.TIP] Bepaalde sjablonen Azure quickstart voor HPC Pack installeren en configureren van de web components automatisch. Als u het [implementatiescript HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) gebruikt voor het maken van het cluster, kunt u desgewenst installeren en configureren van de web components als onderdeel van de implementatie.

**De web components installeren**

1. Verbinding maken met het hoofd knooppunt VM met behulp van de referenties van een beheerder van een cluster.

2. Vanuit de map HPC Pack Setup HpcWebComponents.msi op het hoofd knooppunt worden uitgevoerd.

3. Volg de stappen in de wizard de web components installeren

**Voor het configureren van de web components**

1. Klik op het knooppunt head HPC PowerShell te starten als administrator.

2. Als u de map naar de locatie van het script configureren, typ de volgende opdracht:

    ```powershell
    cd $env:CCP_HOME\bin
    ```
3. Configureer de interface voor de REST en start de webservice HPC, typ de volgende opdracht:

    ```powershell
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```

4. Wanneer u wordt gevraagd om een certificaat te selecteren, kiest u het certificaat dat overeenkomt met de naam van het knooppunt head. Bijvoorbeeld, als u het hoofd knooppunt VM met behulp van het implementatiemodel klassiek, naam van het certificaat ziet eruit als CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net. Als u het implementatiemodel Resource Manager gebruikt, de certificaatnaam ziet eruit als CN =&lt;*HeadNodeDnsName*&gt;. &lt; *regio*&gt;. cloudapp.azure.com.

    >[AZURE.NOTE] U selecteert dit certificaat later wanneer u taken aan het hoofd knooppunt vanaf een computer in de lokalen verzendt. Niet selecteren of een certificaat dat overeenkomt met de naam van het hoofd knooppunt in de Active Directory-domein configureren (bijvoorbeeld CN =*MyHPCHeadNode.HpcAzure.local*).

5. Als u wilt het webportaal voor de indiening van de taak configureren, typ de volgende opdracht:

    ```powershell
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
6. Nadat het script is voltooid, stopt en start de HPC Job Scheduler-Service opnieuw door de volgende opdrachten te typen:

    ```powershell
    net stop hpcscheduler
    net start hpcscheduler
    ```

## <a name="step-2-install-the-hpc-pack-client-utilities-on-an-on-premises-computer"></a>Stap 2: De HPC Pack client-hulpprogramma's installeert op een computer in de lokalen

Als u wilt dat de client HPC Pack's op uw computer te installeren, download de HPC Pack setup-bestanden (volledige installatie) van het [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024). Wanneer u de installatie begint, kies de optie voor het **HPC Pack client utilities**.

Gebruik de hulpprogramma's voor HPC Pack taken kunnen verzenden naar het hoofd VM-knooppunt, moet u ook het exporteren van een certificaat van de hoofd-knooppunt en installeert u deze op de clientcomputer. Het certificaat moet in. CER-indeling.

**Het certificaat exporteren vanuit het knooppunt head**

1. De module Certificaten aan een Microsoft Management Console voor de account van de lokale Computer toevoegen op het hoofd knooppunt. Zie [de module Certificaten aan een MMC toevoegen](https://technet.microsoft.com/library/cc754431.aspx)aan de module toevoegen.

2. Vouw in de consolestructuur **certificaten-lokale Computer** > **persoonlijke**, en klik vervolgens op **certificaten**.

3. Zoek het certificaat dat u hebt geconfigureerd voor de web components van HPC Pack in [stap 1: installeren en configureren van de web components op het knooppunt head](#step-1:-install-and-configure-the-web-components-on-the-head-node) (bijvoorbeeld CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net).

4. Klik met de rechtermuisknop op het certificaat en klik op **Alle taken** > **exporteren**.

5. Klik op **volgende**in de Wizard Certificaat exporteren en controleer of de optie **Nee, de persoonlijke sleutel niet exporteren** is ingeschakeld.

6. Voer de resterende stappen van de wizard exporteren van het certificaat in DER encoded binary X.509 (. (CER)-indeling.


**Voor het importeren van het certificaat op de clientcomputer**


1. Kopieer het certificaat dat u hebt geëxporteerd uit het hoofd knooppunt naar een map op de clientcomputer.

2. Op de client uitvoeren certmgr.msc.

3. In Certificaatbeheer vouwt u **certificaten – huidige gebruiker** > **Vertrouwde basiscertificeringsinstanties**, klik met de rechtermuisknop op **certificaten**en klik op **Alle taken** > **importeren**.

4. In de Wizard Certificaat importeren, klikt u op **volgende** en volg de stappen voor het importeren van het certificaat dat u hebt geëxporteerd uit het hoofd knooppunt naar het archief met vertrouwde basiscertificeringsinstanties.



>[AZURE.TIP] U ziet mogelijk een beveiligingswaarschuwing, omdat de certificeringsinstantie op het hoofd knooppunt wordt niet herkend door de clientcomputer. Voor testdoeleinden kunt u deze waarschuwing negeren en voltooit het certificaat importeren.

## <a name="step-3-run-test-jobs-on-the-cluster"></a>Stap 3: Run testtaken op het cluster

Om te controleren of uw configuratie, probeert de uitvoering van taken op het cluster in Azure van de computer op de locatie. Zo kunt u HPC Pack GUI-hulpprogramma's of opdrachten vanaf de opdrachtregel taken kunnen verzenden naar het cluster. U kunt ook een web-based portal taken.


**Project indienen van opdrachten uitvoeren op de client**


1. Start een opdrachtprompt op een client-computer waarop de hulpprogramma's voor HPC Pack client zijn geïnstalleerd.

2. Een voorbeeld van een opdracht typt. Bijvoorbeeld als u alle taken op het cluster, typt u een opdracht die vergelijkbaar is met een van de volgende, afhankelijk van de volledige DNS-naam van het knooppunt head:

    ```command
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
    
    of
    
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```

    >[AZURE.TIP] Gebruik de volledige DNS-naam van het hoofd knooppunt, niet het IP-adres in de URL van de Taakplanner. Als u het IP-adres opgeeft, treedt er een fout vergelijkbaar met "certificaat van de server moet een geldige certificaatketen hebben of worden geplaatst in het archief met vertrouwde basiscertificeringsinstanties."

3. Wanneer dat wordt gevraagd, typt u de gebruikersnaam (in de vorm &lt;domeinnaam&gt;\\&lt;gebruikersnaam&gt;) en het wachtwoord van de beheerder van de HPC-cluster of een andere cluster-gebruiker die u hebt geconfigureerd. U kunt de referenties voor meer bewerkingen van het project lokaal opslaan.

    Er verschijnt een lijst met taken.


**Gebruik van HPC Project Manager op de clientcomputer**

1. Als u niet eerder de domeinreferenties van een gebruiker cluster opslaan bij het indienen van een project, kunt u de referenties in Referentiebeheer toevoegen.

    een. Klik in het Configuratiescherm op de client start Referentiebeheer.

    b. Klik op de **Windows-referenties** > **een algemene referentie toevoegen**.

    c. Geef het adres op Internet (bijvoorbeeld https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler of https://&lt;HeadNodeDnsName&gt;.&lt; regio&gt;.cloudapp.azure.com/HpcScheduler), en de gebruikersnaam (&lt;domeinnaam&gt;\\&lt;gebruikersnaam&gt;) en het wachtwoord van de Clusterbeheer of een andere gebruiker van cluster die u hebt geconfigureerd.

2. Start Taakbeheer HPC op de clientcomputer.

3. Typ de URL naar het knooppunt head in Azure in het dialoogvenster **Hoofd-knooppunt selecteren** (bijvoorbeeld https://&lt;HeadNodeDnsName&gt;. cloudapp.net of https://&lt;HeadNodeDnsName&gt;.&lt; regio&gt;. cloudapp.azure.com).

    HPC Job Manager wordt geopend en toont een lijst met taken op het knooppunt head.

**Gebruik van het webportaal op het knooppunt head**

1. Start een webbrowser op de clientcomputer en voer een van de volgende adressen, afhankelijk van de volledige DNS-naam van het knooppunt head:

    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
    
    of
    
    ```
    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
2. Typ in het beveiligingsdialoogvenster de domeinreferenties van de HPC-cluster administrator. (U kunt ook andere cluster gebruikers toevoegen in verschillende rollen. Zie [Cluster-gebruikers beheren](https://technet.microsoft.com/library/ff919335.aspx).)

    Het webportaal wordt geopend in de weergave van de lijst taak.

3. Als een taak voorbeeld geeft als de tekenreeks 'Hello World' van het cluster resultaat, klikt u op **nieuwe taak** in de linkernavigatiebalk.

4. Klik op de pagina **Nieuwe baan** **van indiening pagina's**, klik op **Hallo wereld**. De taak indiening pagina wordt weergegeven.

5. Klik op **indienen**. Als u wordt gevraagd, bevatten de domeinreferenties van de HPC-cluster administrator. De taak wordt verzonden en de taak-ID wordt weergegeven op de pagina **Mijn taken** .

6. U kunt de resultaten van de taak die u hebt verzonden, klikt u op de taak-ID en klik op **Taken weergeven** om de uitvoer van de opdracht (onder **Output**) weer te geven.

## <a name="next-steps"></a>Volgende stappen

* U kunt ook taken met de Azure cluster met de [HPC Pack REST API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx)indienen.

* Als u wilt dat taken van een client Linux cluster, Zie het monster Python in de [SDK van HPC Pack 2012 R2 en voorbeeldcode](https://www.microsoft.com/download/details.aspx?id=41633).


<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png
