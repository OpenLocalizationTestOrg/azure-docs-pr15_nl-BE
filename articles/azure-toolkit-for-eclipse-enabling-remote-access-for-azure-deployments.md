<properties
    pageTitle="Externe toegang inschakelen voor Azure implementaties in Eclips"
    description="Informatie over hoe u externe toegang voor Azure implementaties met de Toolkit Azure voor Eclips."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690951.aspx -->

# <a name="enabling-remote-access-for-azure-deployments-in-eclipse"></a>Externe toegang inschakelen voor Azure implementaties in Eclips

U kunt om te helpen bij het oplossen van problemen met de distributie, inschakelen en met RAS verbinding maken met de virtuele machine die als host fungeert voor de implementatie. De RAS-functionaliteit is gebaseerd op de Remote Desktop Protocol (RDP). Nadat u deze hebt gepubliceerd naar Azure, of als u Eclips met een Windows-besturingssysteem gebruikt, u externe toegang configureren kunt voordat u naar Azure publiceert, kunt u externe toegang configureert voor uw implementatie. Opmerking u moet een extern-bureaubladclient die compatibel is met het besturingssysteem verbinding met de implementatie van de virtuele machine in Azure.

## <a name="how-to-enable-remote-access-before-you-deploy-to-azure"></a>Externe toegang inschakelen voordat u op Azure implementeren

> [AZURE.NOTE] Voor externe toegang inschakelen voordat u de toepassing naar Azure implementeert, moet u de Eclips worden uitgevoerd op Windows.

De volgende afbeelding ziet u de **RAS-** eigenschappen dialoogvenster gebruikt voor externe toegang inschakelen.

![][ic719494]

Er zijn twee manieren om het dialoogvenster **RAS** -eigenschappen weer te geven:

* Klik op de koppeling **Geavanceerd** in het **RAS** -gedeelte van het dialoogvenster **publiceren op Azure** .
* Open het dialoogvenster **Eigenschappen** van het project Azure.

Wanneer u een nieuw project voor Azure-implementatie maakt, het project geen externe toegang is standaard ingeschakeld. U kunt echter gemakkelijk RAS inschakelen door de gebruikersnaam en het wachtwoord op te geven in het dialoogvenster **publiceren op Azure** . De RAS-wachtwoord wordt gecodeerd met behulp van x.509-certificaten. Als u niet met uw eigen certificaat, bieden de codering is gebaseerd op een zelfondertekend certificaat met de invoegtoepassing voor Azure voor Eclips geleverd. Deze zelf-ondertekend certificaat is in de map van het **certificaat** van uw Azure project, dat is opgeslagen als een certificaat met openbare-bestand (SampleRemoteAccessPublic.cer) en als een bestand voor persoonlijke informatie-uitwisseling (PFX) (SampleRemoteAccessPrivate.pfx). Deze laatste bevat de persoonlijke sleutel voor het certificaat en het standaardwachtwoord **Wachtwoord1**heeft. Echter, aangezien dit wachtwoord algemeen bekend is, het standaardcertificaat moet worden alleen gebruikt voor leermateriaal, niet voor een productie-implementatie. Dus anders dan voor leren doeleinden, te kunt u externe sessies voor de distributie, ingeschakeld klikken de koppeling **Geavanceerd** in het dialoogvenster **publiceren op Azure** geeft u uw eigen certificaat. Houd er rekening mee dat u moet het PFX-versie van het certificaat uploaden naar uw gehoste service binnen de Azure Management Portal, zodat die Azure wachtwoord van de gebruiker decoderen kan.

De rest van de zelfstudie wordt beschreven hoe u externe toegang inschakelen voor een project Azure-implementatie die oorspronkelijk is gemaakt met externe toegang is uitgeschakeld. Voor de toepassing van deze zelfstudie, maken we een nieuwe zelfondertekende certificaat en het .pfx-bestand heeft een wachtwoord van uw keuze. U hebt ook de mogelijkheid van het gebruik van een certificaat dat is uitgegeven door een certificeringsinstantie.

## <a name="how-to-enable-remote-access-after-you-have-deployed-to-azure"></a>Externe toegang inschakelen nadat u hebt geïmplementeerd op Azure

Voor externe toegang inschakelen nadat u hebt geïmplementeerd in Azure, gebruikt u de volgende stappen uit:

1. Log in op de Azure management portal met uw account Azure
1. Selecteer uw service geïmplementeerde wolk in uw lijst met **Cloud Services**,
1. Klik op de koppeling **configureren** in de cloud-service-webpagina.
1. Klik op de onderkant van de configuratiepagina van de **externe** koppeling
1. Wanneer het dialoogvenster wordt weergegeven:
    * Geef de rol waarvoor u wilt externe toegang inschakelen
    * Schakel het selectievakje **Extern bureaublad inschakelen**
    * Geef een gebruikersnaam en wachtwoord die u wilt gebruiken voor externe toegang
    * Selecteer het certificaat te gebruiken
1. Klik op **OK** 

U ziet een bericht met de mededeling dat de wijziging in de configuratie wordt uitgevoerd, die enkele minuten in beslag kan nemen. Na het wijzigen van de configuratie is voltooid, voert u de stappen in de sectie **extern aanmelden** verderop in dit artikel.
    
## <a name="how-to-enable-remote-access-in-your-package"></a>Het inschakelen van externe toegang in het pakket

1. In het venster Project Explorer Eclips van met de rechtermuisknop op het project Azure en klik op **Eigenschappen**.

1. In het dialoogvenster **Eigenschappen van** **Azure** Vouw in het linkerdeelvenster en klikt u op **RAS**.

1. Controleer **dat alle rollen te accepteren van extern bureaublad-verbindingen met deze inloggegevens inschakelen** is ingeschakeld in het dialoogvenster **RAS** .

1. Geef een gebruikersnaam voor de verbinding met extern bureaublad.

1. Opgeven en Bevestig het wachtwoord voor de gebruiker. De waarden gebruikersnaam en het wachtwoord instellen in dit dialoogvenster wordt gebruikt bij het maken van verbinding met extern bureaublad. (Houd er rekening mee dat dit een afzonderlijk wachtwoord van het PFX-wachtwoord is.)

1. De verloopdatum voor de gebruikersaccount opgeven.

1. Klik op **Nieuw** om een nieuw, zelfondertekend certificaat maken. (Ook kunt u een certificaat respectievelijk van uw werkruimte of bestand systeem via de knoppen **werkruimte** of **bestandssysteem** selecteren, maar voor de doeleinden van deze zelfstudie maken we een nieuw certificaat.)

    * Geef in het dialoogvenster **Nieuw certificaat** en Bevestig het wachtwoord dat u voor het PFX-bestand gebruikt.

    * Accepteer de waarde voor **De naam (CN)**, of een aangepaste naam gebruiken.

    * Geef het pad en de naam waar het nieuwe certificaat in cer-formulier wordt opgeslagen. Voor deze stap en de volgende stap kunt u de map van het **certificaat** van het project Azure, maar u bent vrij om een andere locatie te kiezen. Voor de doeleinden van deze zelfstudie gebruiken we **c:\mycert\mycert.cer**. (Maak de map **c:\mycert** voordat u verdergaat of een bestaande map gebruiken indien gewenst.)

    * Geef het pad en de naam waar het nieuwe certificaat en de persoonlijke sleutel, in de vorm van .pfx, worden opgeslagen. Voor de doeleinden van deze zelfstudie gebruiken we **c:\mycert\mycert.pfx**. Het dialoogvenster **Nieuw certificaat** ziet er ongeveer als volgt (de paden naar mappen bijwerken als u geen **c:\mycert**hebt gebruikt):

        ![][ic712275]

    * Klik op **OK** om het dialoogvenster **Nieuw certificaat** te sluiten.

1. Het dialoogvenster **Remote Access** ziet er ongeveer als volgt:</p>

    ![][ic719495]

1. Klik op **OK** om **het dialoogvenster** te sluiten.
    
Uw toepassing, opnieuw met de build instellen voor de implementatie van de wolk.

## <a name="to-log-in-remotely"></a>Extern aanmelden

Nadat u een exemplaar van uw rol klaar is, kunt u extern aanmelden op de virtuele machine die als host voor uw toepassing fungeert.

* Als Eclips op Windows en u de optie **Extern bureaublad op Start implementeren** geselecteerd tijdens de implementatie naar Azure, u krijgt een aanmeldingsscherm voor de verbinding met extern bureaublad wanneer de installatie wordt gestart. Wanneer u wordt gevraagd om de gebruikersnaam en het wachtwoord, voer de waarden die u hebt opgegeven voor de externe gebruiker en kunnen zich aanmelden.

* Een andere manier om zich extern aanmelden is via de <a href="http://go.microsoft.com/fwlink/?LinkID=512959">Portal van Azure Management</a>:

    * Binnen de **Cloud Services** -weergave van de portal Management Azure, klikt u op uw cloud-service, klikt u op de **exemplaren**klikt u op een specifiek exemplaar en klik vervolgens op de knop **verbinding maken** . De knop **verbinding maken** wordt weergegeven als de volgende in de opdrachtbalk:

        ![][ic659273]

    * Wanneer u op de knop **verbinding maken** , wordt u gevraagd een RDP-bestand te openen. Open het bestand en volg de aanwijzingen. (U kan dit bestand ook opslaan op uw lokale computer en voert u het bestand door erop te dubbelklikken op extern aanmelden met uw virtuele machine zonder eerst de portal management gaan.)

    * Wanneer u wordt gevraagd om de gebruikersnaam en het wachtwoord, voer de waarden die u hebt opgegeven voor de externe gebruiker en kunnen zich aanmelden.

> [AZURE.NOTE] Als u op een niet-Windows-besturingssysteem, moet u een extern bureaublad-client die compatibel is met het besturingssysteem en volg de stappen als u wilt dat de client configureren met de instellingen in het RDP-bestand dat u hebt gedownload.

## <a name="see-also"></a>Zie ook

[Azure Toolkit voor Eclips][]

[Een toepassing Hallo wereld maakt voor Azure in Eclips][]

[Installatie van de Toolkit Azure voor Eclips][] 

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkID=699529
[Een toepassing Hallo wereld maakt voor Azure in Eclips]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installatie van de Toolkit Azure voor Eclips]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic712275]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic712275.png
[ic719495]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719495.png
[ic719494]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719494.png
[ic659273]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic659273.png
