<properties
   pageTitle="Een Cloud-service met behulp van de hulpprogramma's voor Azure | Microsoft Azure"
   description="Meer informatie over Azure cloud serviceprojecten publiceren met behulp van Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="publishing-a-cloud-service-using-the-azure-tools"></a>Een Cloud-service met behulp van de hulpprogramma's voor Azure

U kunt uw Azure toepassing rechtstreeks vanuit Visual Studio publiceren via de Azure-hulpprogramma's voor Microsoft Visual Studio. Geïntegreerde publiceren naar de testdatabase of de productie-omgeving van een cloud-service biedt ondersteuning voor Visual Studio.

Voordat u een toepassing Azure publiceren kunt, moet u een Azure-abonnement hebben. U moet ook een cloud service- en account instellen die door uw toepassing worden gebruikt. U kunt instellen deze op de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885).

>[AZURE.IMPORTANT] Wanneer u publiceert, kunt u de werkomgeving voor uw cloud-service. Ook moet u een opslag-account die wordt gebruikt voor het opslaan van de toepassing Inpakken voor implementatie. Na de implementatie, is het toepassingspakket verwijderd van de opslag-account.

Bij het ontwikkelen en testen van een toepassing Azure kunt u webonderdelen implementeren voor het publiceren van wijzigingen stapsgewijs voor uw web-rollen. Nadat u de toepassing aan een werkomgeving uitgeeft, kunt Web implementeren u wijzigingen rechtstreeks implementeren in de virtuele machine die de Webrol wordt uitgevoerd. U hoeft niet te pakken en uw hele Azure toepassing telkens wanneer die u voor het bijwerken van uw web-functie voor het testen van de wijzigingen wilt publiceren. Met deze aanpak hebt u de rol van wijzigingen op het web beschikbaar in de wolk voor het testen zonder te wachten op uw toepassing gepubliceerd naar een werkomgeving.

Gebruik de volgende procedures voor het publiceren van uw toepassing Azure en de Webrol van een bijwerken met behulp van webonderdelen implementeren:

- Pakket van de toepassing in Visual Studio Azure of publiceren

- De rol van een webpagina als onderdeel van de ontwikkeling en het testen cyclus bijwerken

## <a name="publish-or-package-an-azure-application-from-visual-studio"></a>Pakket van de toepassing in Visual Studio Azure of publiceren

Wanneer u uw toepassing Azure publiceert, kunt u een van de volgende taken doen:

- Maak een servicepakket: U kunt dit pakket en het bestand van de configuratie van uw toepassing uitgeeft aan een werkomgeving van de [Klassieke Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885).

- Publiceer uw Azure project van Visual Studio: voor het publiceren van uw toepassing rechtstreeks naar Azure, gebruikt u de Wizard publiceren. Zie voor informatie [Azure Application Wizard publiceren](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-create-a-service-package-from-visual-studio"></a>Een servicepakket maken vanuit Visual Studio

1. Wanneer u klaar bent om te publiceren van uw toepassing, Solution Explorer openen, opent u het snelmenu voor het Azure project met uw rollen en kies publiceren.

1. Ga als volgt te werk om een servicepakket alleen maken:  

  1. Kies in het snelmenu voor het project Azure, **pakket**.

  1. De configuratie van de service waarvoor u een pakket maken wilt kiezen in het dialoogvenster **Pakket Azure toepassing** en kiest u de configuratie.

  1. (optioneel) Als u extern bureaublad voor de cloud-service nadat u deze hebt gepubliceerd, schakel het selectievakje **Extern bureaublad inschakelen voor alle rollen** en selecteer vervolgens de **Instellingen** voor het configureren van extern bureaublad. Als u fouten opsporen uw cloud-service wilt nadat u deze hebt gepubliceerd, schakelt u foutopsporing op afstand door **Externe foutopsporing inschakelen voor alle rollen**.

      Zie [Extern bureaublad met Azure rollen](vs-azure-tools-remote-desktop-roles.md)voor meer informatie.

  1. Het OLE-pakket maken, kiest u de koppeling van het **pakket** .

      File Explorer ziet u de locatie van het zojuist gemaakte pakket. U kunt deze locatie kopiëren zodat kunt u vanuit de [portal voor Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).

  1. Als dit pakket in een werkomgeving publiceert, moet u deze locatie als de locatie van het pakket als u een cloud-service maken en dit pakket in een omgeving met [Azure klassieke portal implementeert](http://go.microsoft.com/fwlink/?LinkID=213885).

1. (Optioneel) Kies **Annuleren en verwijderen**als u wilt annuleren in het snelmenu voor het artikel in het logboek voor faxactiviteit van het implementatieproces. Hiermee stopt het implementatieproces en verwijdert de implementatieomgeving van Azure.

    >[AZURE.NOTE] Als u wilt deze implementatieomgeving verwijderen nadat deze is geïmplementeerd, moet u de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885).

1. (Optioneel) Nadat de exemplaren van uw rol hebt gestart, weergegeven Visual Studio de implementatieomgeving automatisch in het knooppunt **Cloud Services** in Server Explorer. Hier ziet u de status van de rol van de afzonderlijke exemplaren. Zie [bronnen beheren Azure Cloud Explorer](vs-azure-tools-resources-managing-with-cloud-explorer.md). De volgende afbeelding ziet de exemplaren die rol, terwijl ze nog steeds de status Bezig met initialiseren:

    ![VST_DeployComputeNode](./media/vs-azure-tools-publishing-a-cloud-service/IC744134.png)

## <a name="update-a-web-role-as-part-of-the-development-and-testing-cycle"></a>De rol van een webpagina als onderdeel van de ontwikkeling en het testen cyclus bijwerken

Als uw app in de backend infrastructuur stabiel is, maar de rollen van de webpagina moeten vaker worden bijgewerkt, kunt u webonderdelen implementeren voor het bijwerken van de rol van een webpagina in uw project. Dit is handig als u niet wilt bouwen en implementeren van de back-end werknemer rollen of als er meerdere rollen van het web en u wilt slechts één van de functies van de webpagina bijwerken.

### <a name="requirements"></a>Vereisten

Hier volgen de vereisten voor het implementeren van Web gebruiken voor het bijwerken van uw Webrol:

- **Voor het ontwikkelen en testen van uitsluitend:** De wijzigingen rechtstreeks naar de virtuele machine waarop de Webrol wordt uitgevoerd. Als deze virtuele machine moet worden herhaald, gaan de wijzigingen verloren omdat het oorspronkelijke pakket dat u hebt gepubliceerd opnieuw maken van de virtuele machine voor de rol wordt gebruikt. U moet uw toepassing de meest recente wijzigingen ophalen voor de Webrol opnieuw publiceert.

- **Web-rollen kunnen worden bijgewerkt:** Rollen van de werknemer kunnen niet worden bijgewerkt. U kunt de RoleEntryPoint in web role.cs bovendien niet bijwerken.

- **Biedt slechts ondersteuning voor één exemplaar van een Webrol:** Er kan niet meerdere exemplaren van een web-functie in uw omgeving implementeren. Meerdere web rollen elk met slechts één exemplaar worden echter wel ondersteund.

- **Moet u verbindingen met extern bureaublad inschakelen:** Dit is nodig zodat Web implementeren kunt gebruiker en wachtwoord verbinding maakt met de virtuele machine voor het implementeren van de wijzigingen naar de server waarop Internet Information Services (IIS) wordt uitgevoerd. Bovendien moet u mogelijk verbinding maken met de virtuele machine naar een vertrouwd certificaat toevoegen aan IIS op deze virtuele machine. (Dit zorgt ervoor dat de externe verbinding voor IIS die wordt gebruikt door webonderdelen implementeren beveiligd is.)

De volgende procedure wordt ervan uitgegaan dat u van de wizard **publiceren Azure gebruikmaakt** .

### <a name="to-enable-web-deploy-when-you-publish-your-application"></a>Als u Web wanneer u publiceert uw toepassing implementeren

1. Als u de **Implementatie van Web inschakelen** voor alle rollen selectievakje voor het web, moet u eerst verbindingen met extern bureaublad configureren. Selecteer **Extern bureaublad inschakelen** voor alle rollen en geef vervolgens de referenties die worden gebruikt om verbinding te maken op afstand in het vak **Configuratie voor extern bureaublad** , die wordt weergegeven. Zie [Extern bureaublad met Azure rollen](vs-azure-tools-remote-desktop-roles.md) voor meer informatie.

1. Als u webonderdelen implementeren voor alle web-rollen in uw toepassing selecteert **Web implementeren voor alle web-functies inschakelen**.

    Er verschijnt een geel waarschuwingsdriehoekje. Web Deploy gebruikt een niet-vertrouwd zelfondertekend certificaat standaard niet verstandig om gevoelige gegevens te uploaden. Als u dit proces voor vertrouwelijke gegevens te beveiligen, kunt u een SSL-certificaat moet worden gebruikt voor het implementeren van Internet-verbindingen toevoegen. Dit certificaat moet een vertrouwd certificaat. Zie de sectie **Te maken Web implementeren Secure** verderop in dit onderwerp voor informatie over hoe u dit doet.

1. Kies **volgende** voor het weergeven van het scherm **Summary** en kies vervolgens **publiceren** naar de cloud-service implementeert.

    De service cloud wordt gepubliceerd. De virtuele machine die wordt gemaakt is voor externe verbindingen zijn ingeschakeld voor IIS, zodat de implementatie van Web kan worden gebruikt voor het bijwerken van uw web-rollen zonder deze opnieuw te publiceren.

    >[AZURE.NOTE] Als er meer dan één exemplaar dat is geconfigureerd voor de rol van een webpagina, verschijnt er een waarschuwingsbericht wordt weergegeven, waarin wordt verklaard dat de rol van elke webpagina beperkt tot één exemplaar in het pakket dat wordt gemaakt worden voor het publiceren van uw toepassing. Klik op **OK** om door te gaan. Zoals vermeld in de sectie, kunt u meer dan één web-functie, maar slechts één exemplaar van elke rol hebben.

### <a name="to-update-your-web-role-by-using-web-deploy"></a>Als u wilt uw Webrol bijwerken met behulp van Web implementeren

1. Voor het gebruik van Web implementeren, wijzig code voor het project voor een van uw web-rollen in Visual Studio die u wilt publiceren, klik met de rechtermuisknop op dit knooppunt in de oplossing en wijst u **publiceren**. Het dialoogvenster **Web publiceren** wordt weergegeven.

1. (Optioneel) Als u een vertrouwd certificaat voor SSL gebruiken voor externe verbindingen voor IIS hebt toegevoegd, kunt u het selectievakje **toestaan niet-vertrouwd certificaat** uitschakelen. Zie de sectie **Te maken Web implementeren beveiligde** verderop in dit onderwerp voor meer informatie over het toevoegen van een certificaat Web implementeren om veilig te maken.

1. Voor het gebruik van Web implementeren, moet het mechanisme publiceren de gebruikersnaam en het wachtwoord die u voor uw verbinding met extern bureaublad als u het pakket voor het eerst gepubliceerd.

  1. Voer in het vak **gebruikersnaam**de gebruikersnaam.

  1. Voer in het vak **wachtwoord**het wachtwoord.

  1. (Optioneel) Als u dit wachtwoord opslaan in dit profiel wilt, kiest u **opslaan van wachtwoord**.

1. Kies om de wijzigingen aan de rol van uw website publiceren, **publiceren**.

    De statusregel weergegeven **publiceren gestart**. **Publiceren is voltooid** wanneer het publiceren is voltooid, verschijnt. De wijzigingen zijn nu geïmplementeerd voor de rol van de webpagina op uw virtuele machine. U kunt nu uw Azure toepassing starten in de Azure-omgeving om uw wijzigingen te testen.

### <a name="to-make-web-deploy-secure"></a>Om Web-implementatie beveiligen

1. Web Deploy gebruikt een niet-vertrouwd zelfondertekend certificaat standaard niet verstandig om gevoelige gegevens te uploaden. Als u dit proces voor vertrouwelijke gegevens te beveiligen, kunt u een SSL-certificaat moet worden gebruikt voor het implementeren van Internet-verbindingen toevoegen. Dit certificaat moet een vertrouwd certificaat, die u van een certificate authority (CA ontvangt).

    Web implementeren om veilig te maken voor elke virtuele machine voor elk van de functies van uw website, moet u het vertrouwde certificaat dat u wilt gebruiken voor het web uploaden op de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885)implementeren. Dit zorgt ervoor dat het certificaat wordt toegevoegd aan de virtuele machine die wordt gemaakt voor de Webrol wanneer u de toepassing uitgeeft.

1. Als u wilt toevoegen een vertrouwd certificaat voor SSL in IIS wilt gebruiken voor externe verbindingen, als volgt te werk:

  1. Als u verbinding maakt met de virtuele machine die de Webrol wordt uitgevoerd, selecteert u het exemplaar van de Webrol in de **Cloud Explorer** of **Server Explorer**en kies vervolgens de opdracht **verbinding maken met extern bureaublad** . Zie voor gedetailleerde stappen over hoe u verbinding maakt met de virtuele machine [Met behulp van extern bureaublad met Azure rollen](vs-azure-tools-remote-desktop-roles.md).

      Uw browser, wordt u gevraagd voor het downloaden van een. RDP-bestand.

  1. Om een SSL-certificaat toevoegen, open de management-service in IIS-beheer. SSL in IIS-beheer inschakelen door het openen van de koppeling van de **Bindingen** in het deelvenster **actie** . Het dialoogvenster **Binding van Site toevoegen** wordt weergegeven. Kies **toevoegen**en kies in de vervolgkeuzelijst **Type** op HTTPS. Kies het SSL-certificaat dat u had ondertekend door een Certificeringsinstantie en dat u geüpload naar de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885)in de lijst **SSL-certificaat** . Zie [Verbindingsinstellingen voor de Management-Service configureren](http://go.microsoft.com/fwlink/?LinkId=215824)voor meer informatie.

      >[AZURE.NOTE] Als u een SSL-certificaat van vertrouwde het geel waarschuwingsdriehoekje niet langer weergegeven in de **Wizard publiceren**.

## <a name="include-files-in-the-service-package"></a>Include-bestanden in het servicepakket

Mogelijk moet u specifieke bestanden in uw-pakket opnemen, zodat ze beschikbaar zijn op de virtuele machine die wordt gemaakt voor een rol. U wilt een .exe of een MSI-bestand dat wordt gebruikt door een opstartscript is het servicepakket toevoegen. Of misschien moet u een assembly waarvoor een rol of werknemer rol webproject toevoegen. Bestanden die moeten worden toegevoegd aan de oplossing voor uw toepassing Azure opnemen.

### <a name="to-include-files-in-the-service-package"></a>Bestanden opnemen in het servicepakket

1. Als u wilt een assembly toevoegen aan een servicepakket, gebruiken de volgende stappen uit:

  1. Open het projectknooppunt voor het project dat in de assembly waarnaar wordt verwezen ontbreekt in de **Solution Explorer** .

  1. Als de assembly aan het project toevoegen, opent u het snelmenu voor de map **References** en kies vervolgens de **Verwijzing toevoegen**. Het dialoogvenster toevoegen verschijnt.

  1. Kies de verwijzing die u wilt toevoegen en klik vervolgens op **OK** .

      De verwijzing is toegevoegd aan de lijst onder de map **References** .

  1. Open het snelmenu voor de assembly die u hebt toegevoegd en kies **Eigenschappen**. Het venster **Eigenschappen** verschijnt.

      Deze assembly in het servicepakket in de **lijst met lokale kopie** kiezen om op te nemen **waar**.

1. Open het projectknooppunt voor het project dat in de assembly waarnaar wordt verwezen ontbreekt in de **Solution Explorer** .

1. Als de assembly aan het project toevoegen, opent u het snelmenu voor de map **References** en kies vervolgens de **Verwijzing toevoegen**. Het dialoogvenster **Toevoegen** verschijnt.

1. Kies de verwijzing die u wilt toevoegen en klik vervolgens op **OK** .

    De verwijzing is toegevoegd aan de lijst onder de map **References** .

1. Open het snelmenu voor de assembly die u hebt toegevoegd en kies **Eigenschappen**. Het venster Eigenschappen verschijnt.

1. Kies **waar**deze assembly opnemen in het servicepakket in de lijst **Lokale kopie** .

1. Om bestanden in het pakket van de service die aan uw webproject rol zijn toegevoegd, opent u het snelmenu voor het bestand en kies vervolgens **Eigenschappen**. Kies **inhoud** in de lijst **Actie maken** vanuit het venster **Eigenschappen** .

1. Om bestanden in het servicepakket die zijn toegevoegd aan het project van de rol van werknemer, opent u het snelmenu voor het bestand en kies vervolgens **Eigenschappen**. Kies **als nieuwere kopiëren** in de keuzelijst **kopiëren naar de uitvoermap** vanuit het venster **Eigenschappen** .

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het publiceren naar Azure vanuit Visual Studio, [Azure Application Wizard publiceren](vs-azure-tools-publish-azure-application-wizard.md).
