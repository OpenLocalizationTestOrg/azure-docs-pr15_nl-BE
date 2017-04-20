<properties
    pageTitle="Instellen van een virtuele machine als een server IPython laptop | Microsoft Azure"
    description="Instellen van een Azure Virtual Machine voor gebruik in een omgeving met wetenschappelijke gegevens met de IPython Server voor geavanceerde analytics."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="set-up-an-azure-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Een Azure virtuele machine als een server IPython-laptops voor geavanceerde analytics instellen

Dit onderwerp bevat het inrichten en een Azure virtual machine voor geavanceerde analytics die kan worden gebruikt als onderdeel van een data science-omgeving configureren. De virtuele Windows-computer is geconfigureerd met hulpmiddelen zoals zoals IPython-laptop, Azure Opslagverkenner, AzCopy, maar ook andere hulpprogramma's die handig voor geavanceerde analytics-projecten zijn ondersteunen. Azure Opslagverkenner en AzCopy, bijvoorbeeld handige manieren gegevens uploaden naar Azure blob-opslag van uw lokale computer of op de lokale computer downloaden vanaf de blob-opslag te bieden.

## <a name="create-vm"></a>Stap 1: Een algemeen doel Azure virtuele machine maken

Als u al een Azure virtuele machine wilt instellen op een laptop IPython-server op deze, kunt u deze stap overslaan en doorgaan naar [stap 2: een eindpunt voor IPython notitieblokken toevoegen aan een bestaande virtuele machine](#add-endpoint).

Voordat u begint het proces van het maken van een virtuele machine op Azure, moet u het formaat van de machine die nodig is voor het verwerken van de gegevens voor hun project. Kleinere machines hebben minder geheugen en CPU-kernen minder dan grotere machines, maar ze zijn ook goedkoper. Zie de pagina <a href="http://azure.microsoft.com/pricing/details/virtual-machines/" target="_blank">Virtuele Machines prijzen</a> voor een lijst van de computer typen en prijzen

1. Log in op <a href="https://manage.windowsazure.com" target="_blank">Azure klassieke Portal</a>en klik op **Nieuw** in de linkerbenedenhoek. Een venster wordt weergegeven. Selecteer **berekenen** -> **VIRTUAL MACHINE** -> **FROM GALERIE**.

    ![Werkruimte maken][24]

2. Kies een van de volgende afbeeldingen:

    * Windows Server 2012 R2 Datacenter
    * Windows Server Essentials-ervaring (Windows Server 2012 R2)

    Klik vervolgens op de pijl naar rechts rechts onderin de volgende configuratiepagina te gaan.

    ![Werkruimte maken][25]

3. Voer een naam voor de virtuele machine die u wilt maken, selecteert u de grootte van de machine (standaard: A3) op basis van de grootte van de gegevens zal de computer verwerken en hoe krachtig u wilt dat de computer (de grootte van geheugen en het aantal cores compute), een gebruikersnaam en wachtwoord opgeven voor de machine. Klik vervolgens op de pijl naar rechts te gaan naar de volgende configuratiepagina.

    ![Werkruimte maken][26]

4. Selecteer **Regio AFFINITEIT/groep/VIRTUAL NETWORK** **STORAGE-ACCOUNT** die u wilt gebruiken voor deze virtuele machine bevat en selecteert u die account voor opslag. Een eindpunt onderaan in het veld **EINDPUNTEN** toevoegen door het invoeren van de naam van het eindpunt ("IPython" hier). U kunt een willekeurige tekenreeks als de **naam** van het eindpunt en een willekeurig geheel getal tussen 0 en 65536 is **beschikbaar** als de **Openbare poort**. De **PRIVATE poort** is **9999**. Gebruikers moeten **voorkomen dat** het gebruik van openbare havens die al zijn toegewezen voor internet-services. <a href="http://www.chebucto.ns.ca/~rakerman/port-table.html" target="_blank">Poorten voor Internet-Services</a> biedt een lijst met poorten die zijn toegewezen en die moeten worden vermeden.

    ![Werkruimte maken][27]

5. Klik op het selectievakje start de virtuele machine proces inrichten.

    ![Werkruimte maken][28]


Het duurt 15 tot 25 minuten de virtuele machine proces inrichten. Nadat u de virtuele machine hebt gemaakt, moet de status van deze computer worden **uitgevoerd**weergegeven.

![Werkruimte maken][29]

## <a name="add-endpoint"></a>Stap 2: Een eindpunt voor IPython notitieblokken toevoegen aan een bestaande virtuele machine

Als u de virtuele machine door de instructies in stap 1 hebt gemaakt, het eindpunt voor IPython-laptop is toegevoegd en deze stap worden overgeslagen.

Als de virtuele machine al bestaat, moet u een eindpunt voor IPython notitieblok dat u in stap 3 hieronder eerste logboek in Azure klassieke Portal installeren wilt toevoegen, selecteert u de virtuele machine en het eindpunt voor IPython-laptop-server toevoegen. In de volgende afbeelding bevat een schermafdruk van de portal nadat het eindpunt voor IPython-laptop is toegevoegd aan een virtuele Windows-computer.

![Werkruimte maken][17]

## <a name="run-commands"></a>Stap 3: Installeer IPython laptop en andere ondersteunende hulpmiddelen

Nadat u de virtuele machine hebt gemaakt, gebruikt Remote Desktop Protocol (RDP) aan te melden op de virtuele Windows-computer. Zie voor instructies [hoe u zich aanmeldt bij een Windows Server virtuele Machine wordt uitgevoerd](../virtual-machines/virtual-machines-windows-classic-connect-logon.md). Open de **opdrachtprompt** (**niet het opdrachtvenster Powershell**) als een **beheerder** en voert u de volgende opdracht.

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Wanneer de installatie is voltooid, de laptop IPython-server gestart automatisch in de *C:\\gebruikers\\\<gebruikersnaam\>\\documenten\\IPython-laptops* directory.

Wanneer dat wordt gevraagd, moet u een wachtwoord opgeven voor de laptop IPython en het wachtwoord van de beheerder van de computer. Hiermee kunt het notitieblok IPython worden uitgevoerd als een service op de computer.

## <a name="access"></a>Stap 4: Access IPython-laptops in een webbrowser
Voor toegang tot de server IPython notitieblok, open een web browser en input *https://&#60;virtual DNS-computernaam >: & #60; openbare poortnummer >* in het tekstvak URL. Hier, de *& #60; openbare poortnummer >* moet het poortnummer dat u hebt opgegeven bij het eindpunt IPython notitieblok is toegevoegd.

De *& #60; virtuele machine DNS-naam >* kunt u vinden op de klassieke Portal Azure. Nadat registratie in de klassieke Portal op **virtuele MACHINES**, selecteer de computer die u hebt gemaakt en selecteer vervolgens **DASHBOARD**, de DNS-naam wordt als volgt weergegeven:

![Werkruimte maken][19]

U ontvangt een waarschuwing met de melding dat _Er is een probleem met het beveiligingscertificaat van deze website_ (Internet Explorer) of _uw verbinding is niet persoonlijk_ (chroom), zoals in de volgende afbeelding. Klik op **Doorgaan naar deze website (niet aanbevolen)** (Internet Explorer) of een **Geavanceerd** en vervolgens * *Doorgaan naar & #60;* DNS-naam*> (onveilig) ** (verchroomd) om door te gaan. Vervolgens het wachtwoord die u eerder hebt opgegeven voor toegang tot de IPython-laptops worden ingevoerd.

**Internet Explorer:**
![werkruimte maken][20]

**Chroom:**
![werkruimte maken][21]

Nadat u zich aanmeldt bij de IPython-laptop, een directory *DataScienceSamples* wordt weergegeven in de browser. Deze map bevat monster IPython-laptops die worden gedeeld door Microsoft waarmee gebruikers gegevens wetenschap taken uitvoeren. Deze steekproef IPython-laptops zijn uitgecheckt uit de [**opslagplaats van Github**](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) tot de virtuele machines tijdens de IPython-laptop-server ingesteld proces. Microsoft onderhoudt en deze opslagplaats regelmatig worden bijgewerkt. Gebruikers kunnen de Github opslagplaats als u de meest recente, bijgewerkte monster IPython laptops bezoeken.
![Werkruimte maken][18]

## <a name="upload"></a>Stap 5: Upload een bestaand notitieblok van IPython van een lokale computer naar de server IPython-laptop

IPython-laptops bieden een gemakkelijke manier voor gebruikers van een bestaand notitieblok IPython op hun lokale computer uploaden naar de server IPython notitieblok op de virtuele machines. Nadat de gebruikers zich aanmelden bij de laptop IPython in een webbrowser, klikt u op naar de **map** die de laptop IPython om te worden geüpload. Selecteer vervolgens een IPython laptop .ipynb-bestand uploaden van de lokale computer in de **Verkenner**en sleept deze naar de map IPython notitieblok op de webbrowser. Klik op de knop **uploaden** om het uploaden van het bestand .ipynb naar de server IPython-laptop. Andere gebruikers kunnen vervolgens gaan gebruiken in vanuit hun webbrowser.

![Werkruimte maken][22]

![Werkruimte maken][23]


##<a name="shutdown"></a>Afsluiten en de virtuele machine wanneer niet in gebruik

Azure virtuele Machines worden geprijsd als u **betaalt alleen voor wat u gebruikt**. Om ervoor te zorgen dat u bent niet in rekening wordt gebracht wanneer de virtuele machine niet wordt gebruikt, heeft deze de status **(Deallocated) gestopt** wanneer niet in gebruik.

> [AZURE.NOTE] Als u de virtuele machine van binnen de VM afgesloten (met energiebeheer van Windows), de VM is gestopt maar blijft toegewezen. Zodat u niet moet worden gefactureerd door virtuele machines van de [Klassieke Portal Azure](http://manage.windowsazure.com/)altijd te stoppen. U kunt ook de VM via Powershell stoppen door het aanroepen van **ShutdownRoleOperation** met 'PostShutdownAction' gelijk is aan 'StoppedDeallocated'.

Afsluiten en de toewijzing van de virtuele machine:

1. Log in op de [Azure klassieke Portal](http://manage.windowsazure.com/) met uw account.  

2. Selecteer de **virtuele MACHINES** in de linkernavigatiebalk.

3. Klik op de naam van de virtuele machine en Ga naar **de dashboardpagina** in de lijst van virtuele machines.

4. Onderaan op de pagina, klikt u op **Afsluiten**.

![VM afsluiten][15]

De virtuele machine worden vrijgegeven maar niet verwijderd. U kan uw virtuele machine opnieuw op elk gewenst moment uit de klassieke Azure-Portal.

## <a name="your-azure-vm-is-ready-to-use-whats-next"></a>Uw Azure VM is klaar voor gebruik: Wat is het volgende?

De virtuele machine is nu klaar voor gebruik in uw gegevens wetenschap oefeningen. De virtuele machine is ook klaar voor gebruik als een laptop IPython-server voor de exploratie en verwerking van gegevens en andere taken in combinatie met Azure Machine Learning en het Team gegevens wetenschap proces.

De volgende stappen in de procedure voor het Team gegevens wetenschap zijn toegewezen in het [Pad leren](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) en kunnen bevatten stappen waarmee u gegevens naar een HDInsight, verwerken en het monster in de voorbereiding op het leren van de gegevens met de computer leren werken met Azure.


[15]: ./media/machine-learning-data-science-setup-virtual-machine/vmshutdown.png
[17]: ./media/machine-learning-data-science-setup-virtual-machine/add-endpoints-after-creation.png
[18]: ./media/machine-learning-data-science-setup-virtual-machine/sample-ipnbs.png
[19]: ./media/machine-learning-data-science-setup-virtual-machine/dns-name-and-host-name.png
[20]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning-ie.png
[21]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning.png
[22]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-1.png
[23]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-2.png
[24]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-1.png
[25]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-2.png
[26]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-3.png
[27]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-4.png
[28]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-5.png
[29]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-6.png
