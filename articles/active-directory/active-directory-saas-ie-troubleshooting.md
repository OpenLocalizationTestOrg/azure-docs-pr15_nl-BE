<properties
    pageTitle="Problemen met de uitbreiding van het Configuratiescherm toegang tot Internet Explorer | Microsoft Azure"
    description="Het gebruik van Groepsbeleid voor de implementatie van de invoegtoepassing voor Internet Explorer voor de portal mijn Apps."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/16/2016"
    ms.author="markvi"/>

#<a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a>De uitbreiding van het Configuratiescherm toegang tot het oplossen van problemen voor Internet Explorer

In dit artikel kunt u de volgende problemen:

- U bent geen toegang krijgen tot uw apps via de portal mijn Apps werken met Internet Explorer.
- Zelfs als u de software al hebt geïnstalleerd ziet u het bericht "Software installeren".

Als u een beheerder bent, Zie ook: [de uitbreiding van het Configuratiescherm toegang voor Internet Explorer met behulp van Groepsbeleid implementeren](active-directory-saas-ie-group-policy.md)

##<a name="run-the-diagnostic-tool"></a>Uitvoeren van het hulpprogramma

U kunt problemen installatie met de uitbreiding van het Configuratiescherm toegang tot door het downloaden en uitvoeren van het diagnostisch hulpprogramma voor Access-deelvenster:

1. [Klik hier om Diagnostisch hulpprogramma downloaden.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)

2. Open het bestand en druk op de knop **alle uitpakken** .

    ![Druk op alle uitpakken](./media/active-directory-saas-ie-troubleshooting/extract1.png)

3. Druk op de knop **ophalen** om door te gaan.

    ![Druk op Extract](./media/active-directory-saas-ie-troubleshooting/extract2.png)

4. Met het hulpprogramma wordt uitgevoerd, met de rechtermuisknop op het bestand met de naam **AccessPanelExtensionDiagnosticTool**en selecteer **openen met > Microsoft Windows gebaseerd Script Host**.

    ![Openen met > Microsoft Windows scripthost op basis](./media/active-directory-saas-ie-troubleshooting/open_tool.png)

5. Vervolgens wordt de volgende diagnostische venster waarin wordt beschreven wat misschien iets mis met de installatie.

    ![Een voorbeeld van het venster diagnose](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)

6. Klik op '**Ja**' als u wilt dat het programma de problemen opgelost die zijn gevonden.

7. U kunt deze wijzigingen opslaan, sluit alle vensters van Internet Explorer en open Internet Explorer opnieuw.<br />Als u nog steeds geen toegang uw toepassingen tot, probeert u de volgende stappen uit.

##<a name="check-that-the-access-panel-extension-is-enabled"></a>Controleer of de uitbreiding van het Configuratiescherm toegang is ingeschakeld

Om te controleren of de uitbreiding van het Configuratiescherm toegang is ingeschakeld in Internet Explorer:

1. Klik op het **pictogram van vistuig** in de rechterbovenhoek van het venster in Internet Explorer. Selecteer **Internet-opties**.<br />(In oudere versies van Internet Explorer vindt u onder **Extra > Internet-opties**.

    ![Ga naar Extra > Internet-opties](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)

2. Klik op het tabblad **programma's** , klik op de knop **Invoegtoepassingen beheren** .

    ![Klik op Invoegtoepassingen beheren](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)

3. In dit dialoogvenster, selecteer **Uitbreiding voor toegang tot Configuratiescherm** en klik vervolgens op de knop **inschakelen** .

    ![Klik op inschakelen](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)

4. U kunt deze wijzigingen opslaan, sluit alle vensters van Internet Explorer en open Internet Explorer opnieuw.

##<a name="enable-extensions-for-inprivate-browsing"></a>Extensies voor InPrivate-navigatie surfen inschakelen

Als u de InPrivate-navigatie surfen modus:

1. Klik op het **pictogram van vistuig** in de rechterbovenhoek van het venster in Internet Explorer. Selecteer **Internet-opties**.<br />(In oudere versies van Internet Explorer vindt u onder **Extra > Internet-opties**.

    ![Een voorbeeld van het venster diagnose](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)

2. Ga naar het tabblad **Privacy** en vervolgens **schakelt u** het selectievakje **werkbalken en extensies als gewenst wordt gestart uitschakelen**</p>

    ![Disable werkbalken en uitbreidingen uitschakelen wanneer InPrivate-navigatie surfen wordt gestart](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)

3. U kunt deze wijzigingen opslaan, sluit alle vensters van Internet Explorer en open Internet Explorer opnieuw.

##<a name="uninstall-the-access-panel-extension"></a>De uitbreiding van het Configuratiescherm toegang verwijderen

De extensie van Configuratiescherm toegang van uw computer verwijderen:

1. Druk op de **Windows-toets** om het menu Start op uw toetsenbord. Wanneer het menu geopend is, typt u als u wilt zoeken. Typ 'Configuratiescherm' en open vervolgens het **Configuratiescherm** wanneer deze wordt weergegeven in de zoekresultaten.

    ![Zoeken naar het Configuratiescherm](./media/active-directory-saas-ie-troubleshooting/search_sm.png)

2. In de rechterbovenhoek van het deelvenster Beheer omzetten in de optie **weergave met** **grote pictogrammen**. Vervolgens zoeken en klik op de knop **functies en programma's** .

    ![Chang de weergave grote pictogrammen weer te geven](./media/active-directory-saas-ie-troubleshooting/control_panel.png)

3. Selecteer in de lijst, **Uitbreiding van het Configuratiescherm toegang**en klik op op de knop **verwijderen** .

    ![Klik op verwijderen](./media/active-directory-saas-ie-troubleshooting/uninstall.png)

4. U kunt vervolgens installeert de extensie opnieuw om te zien of het probleem is opgelost.

Als u de extensie verwijderen problemen ondervindt, kunt u ook verwijderen met behulp van het hulpprogramma [Microsoft Fix It](https://go.microsoft.com/?linkid=9779673) .

## <a name="related-articles"></a>Verwante artikelen

- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md)
- [De uitbreiding van het Configuratiescherm toegang implementeren voor Internet Explorer met behulp van Groepsbeleid](active-directory-saas-ie-group-policy.md)
