<properties
    pageTitle="Azure Active Directory B2C: Page UI customization helper tool | Microsoft Azure"
    description="Een hulpprogramma voor het aantonen van de gebruikersinterface aanpassen functie in Azure Active Directory B2C helper"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-a-helper-tool-used-to-demonstrate-the-page-user-interface-ui-customization-feature"></a>Azure Active Directory B2C: Een helper tool gebruikt om aan te tonen van de functie pagina gebruiker gebruikersinterface (UI) aanpassen

Dit artikel is een aanvulling op de [belangrijkste UI customization artikel](active-directory-b2c-reference-ui-customization.md) in B2C Azure Active Directory (AD Azure). De volgende stappen wordt beschreven hoe u oefenen de functie pagina gebruikersinterface aanpassen met behulp van HTML en CSS voorbeeldinhoud die we hebben geleverd.

## <a name="get-an-azure-ad-b2c-tenant"></a>Een huurder Azure AD B2C ophalen

Voordat u iets aanpassen kunt, moet u [een huurder Azure AD B2C](active-directory-b2c-get-started.md) ophalen als u er nog geen hebt.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Maak een beleid voor aanmelden of aanmelden

De makers voorbeeldinhoud kan worden gebruikt om de customze van twee pagina's in een [beleid voor aanmelden of aanmelden](active-directory-b2c-reference-policies.md): de [centrale - aanmeldingspagina](active-directory-b2c-reference-ui-customization.md) en de [eigen kenmerken pagina krachtens](active-directory-b2c-reference-ui-customization.md). Bij het [maken van beleid voor aanmelden of aanmelden](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy), lokale Account (e-mailadres), Facebook, Google en Microsoft als **id-providers**toevoegen. Dit zijn de enige IDPs die onze voorbeeld HTML-inhoud worden geaccepteerd.  Als u wilt, kunt u een subset van deze IDPs toevoegen.

## <a name="register-an-application"></a>Een toepassing registreren

U moet [een toepassing registreren](active-directory-b2c-app-registration.md) in uw huurder B2C die kan worden gebruikt voor het uitvoeren van uw beleid. U hebt een aantal opties waarmee u uw aanmelding beleid daadwerkelijk worden uitgevoerd na de registratie van uw toepassing:

- Bouwen van een van de Azure AD B2C quick start toepassingen die worden vermeld in de sectie "Aan de slag" van [ondertekenen omhoog en meld je aan consumenten in uw toepassingen](active-directory-b2c-overview.md#getting-started).
- Gebruik de vooraf ontworpen [Speelplaats Azure AD B2C](https://aadb2cplayground.azurewebsites.net) -toepassing. Als u de Speelplaats gebruiken, moet u een toepassing registreren in de B2C-huurder met het **omleiden van URI** `https://aadb2cplayground.azurewebsites.net/`.
- Gebruik de knop **Nu uitvoeren** op uw beleid in [Azure portal](https://portal.azure.com/).

## <a name="customize-your-policy"></a>Het beleid aanpassen

Voor het aanpassen van het uiterlijk van uw beleid, moet u eerst maken met de specifieke regels van Azure AD B2C-HTML en CSS-bestanden. Vervolgens kunt u de statische inhoud uploaden naar een openbaar toegankelijke locatie zodanig dat het Azure AD B2C. Dit kan zijn eigen specifieke web-server, Azure Blob-opslag, Azure Content Delivery Network of een willekeurige andere statische resource-hosting-provider. De enige vereisten zijn dat uw inhoud via HTTPS beschikbaar is en toegankelijk is via CORS. Als u de statische inhoud op het web hebt weergegeven, kunt u uw beleid voor het verwijzen naar deze locatie en die inhoud presenteren aan uw klanten. De [belangrijkste UI customization artikel](active-directory-b2c-reference-ui-customization.md) beschrijft in detail de werking van de functie voor het aanpassen van Azure AD B2C.

Voor de toepassing van deze zelfstudie hebt we al enkele voorbeeldinhoud gemaakt en die Azure Blob-opslag. De inhoud van het monster wordt een eenvoudige aanpassing in het thema van ons fictieve bedrijf, "Speel goed". Als u wilt uitproberen in uw eigen beleid, als volgt te werk:

1. Aanmelden bij de huurder in [Azure portal](https://portal.azure.com/) en navigeer naar de blade B2C-functies.
2. Klik op **beleid voor aanmelden of aanmelden** en klik vervolgens op het beleid (bijvoorbeeld "b2c\_1\_teken\_van\_teken\_in").
3. Klik op de **pagina aanpassen** en **Unified aanmelden of aanmelden pagina**.
4. De **aangepaste pagina** -switch **Ja**in-of uitschakelen. Voer in het veld **pagina aangepaste URI** `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/unified.html`. Klik op **OK**.
5. Klik op de **aanmeldingspagina van lokale account**. Schakelen tussen de **aangepaste sjabloon gebruiken** overschakelen naar **Ja**. Voer in het veld **pagina aangepaste URI** `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/selfasserted.html`.
5. Herhaal dezelfde stap voor de **aanmeldpagina sociale account**.
 Klik op **OK** zodat het sluiten van de aanpassing UI blades.
6. Klik op **Opslaan**.

Nu kunt u het aangepaste beleid uitproberen. U kunt uw eigen toepassing of de Speelplaats Azure AD B2C als u wilt, maar u kunt ook gewoon de opdracht **Nu uitvoeren** in het blad van het beleid. Selecteer uw toepassing in de vervolgkeuzelijst en kies de juiste redirect URI. Klik op **nu uitvoeren** . Een nieuw tabblad in de browser wordt geopend en kunt u uitvoeren via de gebruikerservaring aanmelden voor uw toepassing met de nieuwe inhoud op zijn plaats!

## <a name="upload-the-sample-content-to-azure-blob-storage"></a>De voorbeeldinhoud uploadt naar Azure Blob-opslag

Als u dat wilt Azure Blob-opslag gebruiken voor het hosten van uw pagina-inhoud, kunt u uw eigen account opslag maken en onze B2C helper tool gebruiken om uw bestanden te uploaden.

### <a name="create-a-storage-account"></a>Maak een account voor opslag

1. Log in om de [Azure portal](https://portal.azure.com/).
2. Klik op **+ Nieuw** > **Data- +** > **opslag account**. U moet een Azure-abonnement voor het maken van een account Azure Blob-opslag. U kunt een gratis proefversie op de [Azure website](https://azure.microsoft.com/pricing/free-trial/)aanmelden.
3. Geef een **naam** voor de opslag (bijvoorbeeld "contoso') en kies vervolgens de gewenste instellingen voor de **prijzen laag**, **resourcegroep** en **abonnement**. Zorg ervoor dat u de **pincode op Startboard** -optie ingeschakeld hebt. Klik op **maken**.
4. Ga terug naar de Startboard en klik op de account van de opslag die u zojuist hebt gemaakt.
5. In de sectie **Samenvatting** op **Containers**en klik vervolgens op **+ toevoegen**.
6. Geef een **naam** voor de container (bijvoorbeeld "b2c") en selecteer het **type toegang** **Blob** . Klik op **OK**.
7. De container die u hebt gemaakt wordt weergegeven in de lijst op de **BLOB's** blade. Noteer de URL van de container; bijvoorbeeld het certificaat van Echtheid op `https://contoso.blob.core.windows.net/b2c`. Sluit de **BLOB's** blade.
8. Op het blad opslag-account op **toetsen** en noteer de waarden van de velden **Accountnaam opslag** en de **Primaire sleutel van Access** .

1. Log in om de [Azure portal](https://portal.azure.com/).
2. Klik op **+ Nieuw** > **Data- +** > **opslag account**. U moet een Azure-abonnement voor het maken van een account Azure Blob-opslag. U kunt een gratis proefversie op de [Azure website](https://azure.microsoft.com/pricing/free-trial/)aanmelden.
3. **Blob-opslag** onder **Type Account**selecteren en laat de andere waarden als standaard.  Kunt u de resourcegroep & locatie als u wilt.  Klik op **maken**.
4. Ga terug naar de Startboard en klik op de account van de opslag die u zojuist hebt gemaakt.
5. Klik in de sectie **Samenvatting** **+ Container**.
6. Geef een **naam** voor de container (bijvoorbeeld "b2c") en selecteer het **type toegang** **Blob** . Klik op **OK**.
7. Open de container **Eigenschappen**en noteer de URL van de container; bijvoorbeeld het certificaat van Echtheid op `https://contoso.blob.core.windows.net/b2c`. Sluit de container blade.
8. Klik op het **Sleutel-pictogram** op de bladeserver opslag-account en noteer de waarden van de velden **Accountnaam opslag** en de **Primaire sleutel van Access** .

> [AZURE.NOTE]
    **Primaire sleutel van Access** is een belangrijke.

### <a name="download-the-helper-tool-and-sample-files"></a>De helper en bestanden downloaden

U kunt de [Azure Blob-opslag helper en bestanden als een ZIP-bestand](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip) downloaden of het klonen van GitHub:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

Deze bibliotheek bevat een `sample_templates\wingtip` map voorbeeld HTML, CSS en afbeeldingen bevat. Deze sjablonen om te verwijzen naar uw eigen account Azure Blob-opslag, moet u de HTML-bestanden bewerken. Open `unified.html` en `selfasserted.html` en vervangt u alle instanties van `https://localhost` met de URL van uw eigen container die u hebt opgeschreven in de vorige stappen. U moet het absolute pad van de HTML-bestanden gebruiken, omdat in dat geval wordt de HTML-code worden geserveerd door Azure AD, onder het domein `https://login.microsoftonline.com`.

### <a name="upload-the-sample-files"></a>De voorbeeldbestanden uploaden

In de dezelfde opslagplaats unzip `B2CAzureStorageClient.zip` en de `B2CAzureStorageClient.exe` binnen het bestand. Dit programma wordt gewoon uploaden van de bestanden in de map die u bij uw account voor opslag opgeeft en CORS inschakelen voor die bestanden. Als u de bovenstaande stappen hebt gevolgd, wordt de HTML- en CSS-bestanden nu verwijzen naar uw rekening opslag. Houd er rekening mee dat de naam van de account van uw opslag is het gedeelte dat vóór `blob.core.windows.net`; bijvoorbeeld `contoso`. U kunt controleren of dat de inhoud correct is geüpload door te proberen toegang te krijgen tot `https://{storage-account-name}.blob.core.windows.net/{container-name}/wingtip/unified.html` in een browser. Ook [http://test-cors.org/](http://test-cors.org/) gebruiken om ervoor te zorgen dat de inhoud nu CORS ingeschakeld is. (Zoek naar "status XHR: 200 ' in het resultaat.)

### <a name="customize-your-policy-again"></a>Uw beleid opnieuw aanpassen

Nu dat u de inhoud van het monster hebt geüpload naar uw eigen account opslag, moet u ernaar verwijzen in uw aanmelding beleid bewerken. Herhaal de stappen uit de sectie ["Het beleid aanpassen"](#customize-your-policy) hierboven, met behulp van URL's van uw eigen account opslag. Bijvoorbeeld, de locatie van de `unified.html` bestand `<url-of-your-container>/wingtip/unified.html`.

Nu kunt u de knop **Nu uitvoeren** of uw eigen toepassing opnieuw uitvoeren van het beleid. Het resultaat ziet er bijna precies hetzelfde: u gebruikt dezelfde HTML en CSS monster in beide gevallen. Maar het beleid nu verwijzen naar uw eigen exemplaar van Azure Blob-opslag en u bent vrij om te bewerken en de bestanden opnieuw als u kunt uploaden. Raadpleeg het [belangrijkste UI customization-artikel](active-directory-b2c-reference-ui-customization.md)voor meer informatie over het aanpassen van HTML en CSS.
