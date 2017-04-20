<properties
    pageTitle="Verbinding maken met Configuration Manager logboek Analytics | Microsoft Azure"
    description="In dit artikel worden de stappen beschreven voor Configuration Manager verbinding met Analytics logboek en beginnen met het analyseren van gegevens."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="banders"/>

# <a name="connect-configuration-manager-to-log-analytics"></a>Verbinding maken met Configuration Manager logboek Analytics

Kunt u System Center Configuration Manager logboek Analytics in OMS gegevens te synchroniseren apparaat-collectie. Hierdoor is gegevens uit de implementatie van Configuratiebeheer beschikbaar in de OMS.

Er zijn een aantal stappen dat nodig is verbinding maken met Configuration Manager OMS, dus hier een snel overzicht van het volledige proces is:

1. In de Azure Management Portal Configuration Manager registreren als een webtoepassing en/of Web API app en zorg ervoor dat de client-ID en de geheime sleutel van de registratie van Azure Active Directory client. Zie [portal gebruik te maken van Active Directory-toepassing en service principal die toegang krijgen tot bronnen](../resource-group-create-service-principal-portal.md) voor gedetailleerde informatie over het uitvoeren van deze stap.
2. In de Azure Management Portal [Configuration Manager (de geregistreerde web app) met OMS toegang bieden](#provide-configuration-manager-with-permissions-to-oms).
3. In Configuration Manager [een verbinding met de Wizard toevoegen OMS verbinding toevoegen](#add-an-oms-connection-to-configuration-manager).
4. Configuration Manager kunt u [de eigenschappen van de update](#update-oms-connection-properties) als de geheime sleutel wachtwoord of client ooit verloopt of verloren gegaan is.
5. Met de informatie uit het Kantoorbeheersysteem portal, [downloaden en installeren van de Agent Microsoft controle](#download-and-install-the-agent) op de computer de verbinding met de Configuration Manager met punt sitesysteemfunctie. De agent verzendt Configuration Manager gegevens naar OMS.
6. In de OMS [collecties van Configuration Manager importeren](#import-collections) als computergroepen.
7. In de OMS, weergeven van Configuratiebeheer als [computergroepen](log-analytics-computer-groups.md).

U kunt meer lezen over het aansluiten van Configuration Manager op OMS gegevens [synchroniseren van Configuratiebeheer voor Microsoft Operations Management Suite](https://technet.microsoft.com/library/mt757374.aspx).



## <a name="provide-configuration-manager-with-permissions-to-oms"></a>Configuration Manager machtigingen verstrekken aan OMS

De volgende procedure biedt de Azure Management Portal toegang tot OMS. Met name moet u de *rol Inzender* verlenen aan gebruikers in de resourcegroep. Op zijn beurt, waarmee de Azure Management Portal Configuration Manager verbinding kunnen maken met OMS.

>[AZURE.NOTE] Moet u machtigingen voor het Kantoorbeheersysteem van Configuratiebeheer. Anders ontvangt u een foutbericht weergegeven wanneer u de configuratiewizard in Configuration Manager.


1. Open de [Azure portal](https://portal.azure.com/) en klikt u op **Bladeren** > **Log Analytics (OMS)** voor het openen van het logboek Analytics Kantoorbeheersysteem blade.  
2. Klik op **toevoegen** om de bladeserver **OMS werkruimte** op het blad **Logboek Analytics (OMS)** .  
  ![OMS blade](./media/log-analytics-sccm/sccm-azure01.png)
3. Geef de volgende gegevens op het blad **OMS werkruimte** en klik op **OK**.
  - **OMS-werkruimte**
  - **Abonnement**
  - **Resourcegroep**
  - **Locatie**
  - **Prijzen laag**  
    ![OMS blade](./media/log-analytics-sccm/sccm-azure02.png)  

    >[AZURE.NOTE] In het bovenstaande voorbeeld wordt een nieuwe groep gemaakt. De resourcegroep wordt alleen Configuration Manager voorzien van machtigingen voor de werkruimte OMS in dit voorbeeld gebruikt.

4. Klik op **Bladeren** > **resourcegroepen** openen de **resourcegroepen** blade.
5. Klik op de resourcegroep die u hierboven hebt gemaakt te openen in de blade **resourcegroepen** , de &lt;Resourcegroepnaam&gt; instellingen blade.  
  ![resource groep instellingen blade](./media/log-analytics-sccm/sccm-azure03.png)
6. In de &lt;Resourcegroepnaam&gt; instellingen blade, toegangsbeheer (IAM) te openen klikt u op de &lt;Resourcegroepnaam&gt; gebruikers blade.  
  ![resource groep gebruikers blade](./media/log-analytics-sccm/sccm-azure04.png)  
7. In de &lt;Resourcegroepnaam&gt; blade gebruikers, klikt u op **toevoegen** om de bladeserver **toegang toevoegen** .
8. Klikt u op **Selecteer een rol**in de blade **toegang toevoegen** en selecteer vervolgens de rol **Inzender** .  
  ![Selecteer een rol](./media/log-analytics-sccm/sccm-azure05.png)  
9. Klik op **gebruikers toevoegen**, selecteert u de gebruiker Configuration Manager, klikt u op **selecteren**en klik op **OK**.  
  ![gebruikers toevoegen](./media/log-analytics-sccm/sccm-azure06.png)  


## <a name="add-an-oms-connection-to-configuration-manager"></a>Een verbinding OMS met Configuration Manager toevoegen

Toe te voegen een verbinding OMS, moet Configuratiebeheer beschikt u over een [verbinding met een punt](https://technet.microsoft.com/library/mt627781.aspx) voor on line-modus is geconfigureerd.

1. Selecteer in de werkruimte **beheer** van Configuratiebeheer **OMS-Connector**. Hiermee opent u de **Wizard voor OMS verbinding toevoegen**. Selecteer **volgende**.

2. Bevestig dat u de volgende acties hebt uitgevoerd en hebt details voor elk item en vervolgens selecteert u **volgende**in het scherm **algemene** .
  1. In het Portal voor Azure beheer inschrijving je Configuration Manager als een webtoepassing en/of Web API app en u hebt de [client-ID van de registratie](../active-directory/active-directory-integrating-applications.md).
  2. U hebt een geheime sleutel app voor de geregistreerde app in Azure Active Directory gemaakt in de Portal voor het beheer van Azure.  
  3. In Azure Management Portal, hebt u de geregistreerde web app met OMS toegang tot verstrekt.  
  ![Verbinding met de pagina OMS Wizard algemeen](./media/log-analytics-sccm/sccm-console-general01.png)

3. De verbindingsinstellingen voor OMS configureren door de **huurder** , **De Client-ID** en **Client geheime sleutel** en selecteer het **volgende**op het scherm **Azure Active Directory** .  
  ![Verbinding met de pagina OMS Wizard Azure Active Directory](./media/log-analytics-sccm/sccm-wizard-tenant-filled03.png)

4. Als u de procedures is voltooid, verschijnen de gegevens op het scherm **Configuratie OMS-verbinding** automatisch op deze pagina. Informatie van de verbindingsinstellingen wordt voor uw **abonnement Azure** , **Azure resourcegroep** en **Operations Management Suite werkruimte**weergegeven.  
  ![Verbinding met de pagina OMS Wizard OMS verbinding](./media/log-analytics-sccm/sccm-wizard-configure04.png)

5. De wizard verbinding met de OMS-service met behulp van de informatie die u hebt ingevoerd. Selecteer het apparaat collecties die u wilt synchroniseren met OMS en klik vervolgens op **toevoegen**.  
  ![Selecteer collecties](./media/log-analytics-sccm/sccm-wizard-add-collections05.png)

6. Controleer de verbindingsinstellingen op het scherm **Summary** en selecteer vervolgens **volgende**. Het scherm **vooruitgang** toont de status van de verbinding en vervolgens moet **Voltooien**.

>[AZURE.NOTE] U moet OMS verbinding naar de site van de bovenste laag in de hiërarchie. Als u OMS op een zelfstandige primaire site aansluit en vervolgens een centraal beheer-site aan uw omgeving toevoegen, hebt u te verwijderen en opnieuw de verbinding OMS binnen de nieuwe hiërarchie.

Nadat u een Configuration Manager aan OMS hebt gekoppeld, kunt u toevoegen of collecties verwijderen en de eigenschappen van de verbinding OMS.

## <a name="update-oms-connection-properties"></a>OMS eigenschappen bijwerken

Als een geheime sleutel wachtwoord of client ooit verloopt of verloren gaat, moet u de eigenschappen van de OMS handmatig bijwerken.

1. Configuration Manager Ga naar **Cloud-Services** en selecteer vervolgens **OMS-Connector** voor het openen van de pagina **Eigenschappen van de verbinding OMS** .
2. Op deze pagina, klikt u op het tabblad **Azure Active Directory** om uw **huurder**, **De Client-ID**, **geheime sleutel vervaldatum van de Client**. **Controleer of** uw **geheime sleutel Client** als deze is verlopen.


## <a name="download-and-install-the-agent"></a>Downloaden en installeren van de agent

1. In de OMS portal [downloaden van het setup-bestand van de agent uit het Kantoorbeheersysteem](log-analytics-windows-agents.md#download-the-agent-setup-file-from-oms).
2. Gebruik een van de volgende manieren installeren en configureren van de agent op de computer met de Configuration Manager service connection point-site rol:
  - [De agent die met behulp van setup installeren](log-analytics-windows-agents.md#install-the-agent-using-setup)
  - [Installeren van de agent via de opdrachtregel](log-analytics-windows-agents.md#install-the-agent-using-the-command-line)
  - [Installeren van de agent met DSC in Azure automatisering](log-analytics-windows-agents.md#install-the-agent-using-dsc-in-azure-automation)


## <a name="import-collections"></a>Collecties importeren

Nadat u hebt een verbinding OMS met Configuration Manager toegevoegd en de agent geïnstalleerd wijst op de computer de verbinding met de Configuration Manager met sitesysteemfunctie, de volgende stap collecties van Configuratiebeheer in OMS als computergroepen importeren.

Na invoer is ingeschakeld, collectie lidmaatschapsgegevens elke drie uur opgehaald om het lidmaatschap van de collectie actueel te houden. U kunt invoer op elk gewenst moment uitschakelen.

1. Klik op **Instellingen**in de portal OMS.
2. Klik op het tabblad **Computergroepen** en klik vervolgens op het tabblad **SCCM** .
3. Selecteer **Import Configuration Manager collectie lidmaatschappen** en klik vervolgens op **Opslaan**.  
  ![Computergroepen - tabblad SCCM](./media/log-analytics-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>Gegevens weergeven van Configuration Manager

Nadat u een verbinding met OMS met Configuration Manager toegevoegd en op de computer met de Configuration Manager service connection point-site rol van de agent is geïnstalleerd, worden de gegevens van de agent wordt verzonden naar OMS. In OMS weergegeven collecties Configuration Manager als [computergroepen](log-analytics-computer-groups.md). U kunt de groepen van de pagina **Configuration Manager** onder **Computergroepen** weergeven in **Instellingen**.

Nadat de verzamelingen zijn geïmporteerd, kunt u zien hoeveel computers met de collectie lidmaatschappen zijn gevonden. U ziet ook het aantal collecties die zijn geïmporteerd.

![Computergroepen - tabblad SCCM](./media/log-analytics-sccm/sccm-computer-groups02.png)

Wanneer u klikt op een zoekopdracht wordt geopend hetzij alle geïmporteerde groepen of alle computers die aan elke groep behoren. [Zoekfunctie](log-analytics-log-searches.md)kunt u diepgaande analyse voor Configuratiebeheer starten.

## <a name="next-steps"></a>Volgende stappen

- Met [Zoekfunctie](log-analytics-log-searches.md) kunt u gedetailleerde informatie over uw gegevens Configuration Manager.
