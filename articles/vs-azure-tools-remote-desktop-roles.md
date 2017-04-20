<properties 
   pageTitle="Met extern bureaublad met Azure rollen | Microsoft Azure"
   description="Met behulp van extern bureaublad met Azure rollen"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="using-remote-desktop-with-azure-roles"></a>Met behulp van extern bureaublad met Azure rollen

Met behulp van de extern bureaublad-Services en Azure SDK is toegankelijk Azure rollen en virtuele machines die worden gehost door Azure. U kunt Extern bureaublad-Services configureren vanuit een Azure project in Visual Studio. Extern bureaublad-Services moet, u een project werken met een of meer rollen maken en deze vervolgens publiceren naar Azure.

>[AZURE.IMPORTANT] U moet toegang tot een Azure rol voor het oplossen van problemen of ontwikkeling alleen. Het doel van elke virtuele machine is aan een specifieke rol in de Azure toepassing, niet voor andere clienttoepassingen worden uitgevoerd. Zie toegang tot Azure virtuele Machines vanuit Server Explorer Azure voor het hosten van een virtuele machine die u voor andere doeleinden gebruiken kunt gebruiken.

## <a name="to-enable-and-use-remote-desktop-for-an-azure-role"></a>Inschakelen en gebruiken van extern bureaublad voor de rol van een Azure

1. Opent het snelmenu voor het project in de Solution Explorer en kies vervolgens **publiceren**.

    De wizard **Publiceren Azure-toepassing** wordt weergegeven.

    ![Opdracht voor een Cloud Service project publiceren](./media/vs-azure-tools-remote-desktop-roles/IC799161.png)

1. Selecteer onderaan de pagina van de wizard **Microsoft Azure publicatie-instellingen** , het **Extern bureaublad inschakelen** voor alle rollen. 

    Het dialoogvenster **Configuratie voor extern bureaublad** wordt weergegeven.

1. Onderaan in het dialoogvenster **Configuratie voor extern bureaublad** , klik op de knop **Meer opties** . 
 
    Een vervolgkeuzelijst waarmee u maken en kies een certificaat kunt, zodat u referenties gegevens coderen kunt wanneer u verbinding maakt via een extern bureaublad worden weergegeven.

1. Kies in de vervolgkeuzelijst ** &lt;maken >**, of kies een bestaande uit de lijst. 

    Als u een bestaand certificaat kiest, kunt u de volgende stappen overslaan.

    >[AZURE.NOTE] De certificaten die u nodig hebt om een verbinding met extern bureaublad zijn anders dan de certificaten die u voor andere verrichtingen Azure gebruikt. De RAS-certificaat moet een persoonlijke sleutel hebben.

    Het dialoogvenster **Certificaat maken** wordt weergegeven.

    1. Geef een beschrijvende naam voor het nieuwe certificaat en klik op **OK** . Het nieuwe certificaat wordt weergegeven in de vervolgkeuzelijst.

    1. Geef een gebruikersnaam en een wachtwoord in het dialoogvenster **Configuratie voor extern bureaublad** .
    
        U kunt een bestaande account niet gebruiken. Geeft de beheerder als de gebruikersnaam voor de nieuwe account.

        >[AZURE.NOTE] Als het wachtwoord niet voldoet aan de complexiteitsvereisten, verschijnt er een rood pictogram naast het wachtwoordvak. Het wachtwoord moet hoofdletters, kleine letters, en getallen of symbolen bevatten.

    1. Kies een datum op waarop de account verloopt en na welke verbindingen met extern bureaublad, wordt geblokkeerd.

    1. Nadat u alle vereiste informatie hebt opgegeven, klik op de knop **OK** .
    
        Verschillende instellingen waarmee de RAS-service worden toegevoegd aan de bestanden .cscfg en .csdef.

1. In de wizard **Microsoft Azure publicatie-instellingen** , klik op **OK** wanneer u voor het publiceren van uw cloud-service.

    Als u niet wilt publiceren, klik op de knop **Annuleren** . De configuratie-instellingen worden opgeslagen en u kunt uw service cloud later publiceren.

## <a name="connect-to-an-azure-role-by-using-remote-desktop"></a>Via Extern bureaublad verbinding maken met een Azure-rol

Nadat u de service cloud op Azure publiceert, kunt u Server Explorer aan te melden bij de virtuele machines die Azure hosts. 

1. Vouw het knooppunt **Azure** in Server Explorer en vouw het knooppunt voor een cloud-service en een van de functies voor het weergeven van exemplaren.

1. Open het snelmenu voor een exemplaar van knooppunt en kies vervolgens **Verbinding maken met extern bureaublad**.

    ![Verbinding maken via Extern bureaublad](./media/vs-azure-tools-remote-desktop-roles/IC799162.png)

1. Voer de gebruikersnaam en het wachtwoord die u eerder hebt gemaakt. U bent nu aangemeld bij de externe sessie.


