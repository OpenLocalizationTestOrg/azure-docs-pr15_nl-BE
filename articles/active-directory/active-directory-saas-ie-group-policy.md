<properties
    pageTitle="De uitbreiding van het Configuratiescherm toegang implementeren voor Internet Explorer met behulp van Groepsbeleid | Microsoft Azure"
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

#<a name="how-to-deploy-the-access-panel-extension-for-internet-explorer-using-group-policy"></a>De uitbreiding van het Configuratiescherm toegang implementeren voor Internet Explorer met behulp van Groepsbeleid

Deze zelfstudie laat zien hoe met behulp van Groepsbeleid de extensie van Configuratiescherm toegang op afstand voor Internet Explorer installeren op computers van gebruikers. Deze extensie is vereist voor Internet Explorer-gebruikers hoeven aan te melden in toepassingen die zijn geconfigureerd met op [wachtwoorden gebaseerde eenmalige aanmelding](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

Het wordt aanbevolen dat beheerders automatiseren van de implementatie van deze extensie. Anders moet downloaden en installeren van de uitbreiding zelf, die gevoelig is voor de gebruikersfout en er zijn beheerdersrechten vereist. Deze zelfstudie heeft betrekking op één methode voor de implementatie van software automatiseren met behulp van Groepsbeleid. [Meer informatie over Groepsbeleid.](https://technet.microsoft.com/windowsserver/bb310732.aspx)

De extensie van Configuratiescherm toegang is ook beschikbaar voor [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) en [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), noch die beheerdersmachtigingen voor de installatie vereist.

##<a name="prerequisites"></a>Vereisten

- U [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx)hebt ingesteld en u computers van uw gebruikers aan uw domein hebt toegevoegd.
- U moet de machtiging 'Instellingen bewerken' om het bewerken van groepsbeleidsobjecten (GPO's) hebben. Leden van de volgende beveiligingsgroepen hebben standaard deze machtiging: Domeinadministrators, Ondernemingsadministrators en Maker Eigenaar Groepsbeleid. [Meer informatie.](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

##<a name="step-1-create-the-distribution-point"></a>Stap 1: Het distributiepunt maken

Eerst plaatst u het installer-pakket op een netwerklocatie die toegankelijk is vanaf alle computers die u de extensie wilt op afstand installeren. Ga hiervoor als volgt te werk:

1. Bij de server aanmelden als beheerder

2. Ga naar **bestanden en Services voor opslag**in het venster van **Serverbeheer** .

    ![Geopende bestanden en opslagservices](./media/active-directory-saas-ie-group-policy/files-services.png)

3. Ga naar het tabblad **Shares** . Klik op **taken** > **Nieuwe Share...**

    ![Geopende bestanden en opslagservices](./media/active-directory-saas-ie-group-policy/shares.png)

4. De **Wizard Nieuwe Share** en stel de machtigingen om ervoor te zorgen dat deze toegankelijk vanaf computers van uw gebruikers. [Meer informatie over aandelen.](https://technet.microsoft.com/library/cc753175.aspx)

5. Downloaden van de volgende Microsoft Windows Installer-pakket (MSI-bestand): [Access paneel Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)

6. Kopieer het installer-pakket op de gewenste locatie op de share.

    ![Kopie van het MSI-bestand voor u bent de share.](./media/active-directory-saas-ie-group-policy/copy-package.png)

8. Controleer of uw clientcomputers toegang tot het installer-pakket via de share. 

##<a name="step-2-create-the-group-policy-object"></a>Stap 2: De groepsbeleidsobjecten maken

1. Aanmelden bij de server die fungeert als host voor de installatie van Active Directory Domain Services (AD DS).

2. In de Server Manager, gaat u naar **Extra** > **Voor Groepsbeleidsbeheer**.

    ![Ga naar Extra > beleid Management groep](./media/active-directory-saas-ie-group-policy/tools-gpm.png)

3. In het linkerdeelvenster van het venster **Groepsbeleidsbeheer** , de hiërarchie van organisatie-eenheid (OU) weergeven en bepalen op welk bereik u wilt graag het Groepsbeleid toepassen. Zo wilt u misschien een kleine organisatie-eenheid naar een paar gebruikers implementeren voor het testen van pick of kiest u een hoogste organisatie-eenheid te implementeren voor uw hele organisatie.

    > [AZURE.NOTE] Als u wilt maken of bewerken van uw organisatie-eenheden (OU's), Ga terug naar de Server Manager en Ga naar **Extra** > **Active Directory: gebruikers en Computers**.

4. Zodra u een organisatie-eenheid hebt geselecteerd, klik met de rechtermuisknop op het en selecteer **een groepsbeleidsobject in dit domein maken en hier een koppeling...**

    ![Een nieuw groepsbeleidsobject maken](./media/active-directory-saas-ie-group-policy/create-gpo.png)

5. Typ in het dialoogvenster **Nieuw groepsbeleidsobject** een naam voor het nieuwe groepsbeleidobject.

    ![Naam voor het nieuwe groepsbeleidsobject](./media/active-directory-saas-ie-group-policy/name-gpo.png)

6. Klik met de rechtermuisknop op het groepsbeleidsobject dat u zojuist hebt gemaakt en selecteer **bewerken**.

    ![Het nieuwe Groepsbeleidobject bewerken](./media/active-directory-saas-ie-group-policy/edit-gpo.png)

##<a name="step-3-assign-the-installation-package"></a>Stap 3: Het installatiepakket toewijzen

1. Bepalen of u dat wilt voor de implementatie van de extensie op basis van **Computerconfiguratie** of **Gebruikersconfiguratie**. Wanneer u de [Computerconfiguratie](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), wordt de extensie geïnstalleerd op de computer, ongeacht welke gebruiker zich aanmeldt het. Met [Gebruikersconfiguratie](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx)heeft gebruikers aan de andere kant de extensie geïnstalleerd voor ongeacht welke computers ze zich aanmelden.

2. Ga naar een van de volgende mappaden van de, afhankelijk van het type van de configuratie die u hebt gekozen in het linkerdeelvenster van het venster **Editor voor Groepsbeleidsbeheer** :
    - `Computer Configuration/Policies/Software Settings/`
    - `User Configuration/Policies/Software Settings/`

3. Klik met de rechtermuisknop op **Software-installatie**en selecteer vervolgens **Nieuw** > **pakket...**

    ![Een nieuwe software-installatiepakket maken](./media/active-directory-saas-ie-group-policy/new-package.png)

4. Ga naar de gedeelde map met het installatiepakket van [stap 1: het distributiepunt maken](#step-1-create-the-distribution-point), selecteert u het MSI-bestand en klik op **openen**.

    > [AZURE.IMPORTANT] Als de share bevindt zich op deze server, controleert u of dat u het .msi via het pad van het netwerk in plaats van het pad naar het lokale bestand benaderen wilt.

    ![Selecteer het installatiepakket in de gedeelde map.](./media/active-directory-saas-ie-group-policy/select-package.png)

5. Selecteer in het dialoogvenster **Software distribueren** **toegewezen** voor uw implementatie. Klik vervolgens op **OK**.

    ![Selecteer toegewezen en klik vervolgens op OK.](./media/active-directory-saas-ie-group-policy/deployment-method.png)

De extensie wordt nu geïmplementeerd op de organisatie-eenheid die u hebt geselecteerd. [Meer informatie over Software-installatie in Groepsbeleid.](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

##<a name="step-4-auto-enable-the-extension-for-internet-explorer"></a>Stap 4: Auto-Enable de uitbreiding voor Internet Explorer 

Naast het uitvoeren van het installatieprogramma moet alle uitbreidingen voor Internet Explorer ingeschakeld zijn voordat deze kan worden gebruikt. De volgende stappen om de uitbreiding van het Configuratiescherm toegang met behulp van Groepsbeleid:

1. In het venster **Editor voor Groepsbeleidsbeheer** , gaat u naar een van de volgende paden, afhankelijk van het type configuratie gekozen [stap 3: het installatiepakket wijzen](#step-3-assign-the-installation-package):
    - `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
    - `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`

2. Klik met de rechtermuisknop op de **Lijst met invoegtoepassingen**en selecteer **bewerken**.
    ![Bewerken lijst met invoegtoepassingen.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)

3. Selecteer in het venster **Lijst met toegestane invoegtoepassingen** **ingeschakeld**. In de sectie **Opties** , klik op **weergeven...**.

    ![Klik op inschakelen en klik vervolgens op weergeven...](./media/active-directory-saas-ie-group-policy/edit-add-on-list-window.png)

4. In het venster **Inhoud weergeven** , voert u de volgende stappen uit:

    1. Voor de eerste kolom (het veld met de **Naam** ), kopieer en plak de volgende klasse-ID:`{030E9A3F-7B18-4122-9A60-B87235E4F59E}`

    2. Typ de volgende waarde voor de tweede kolom (het veld met de **waarde** ):`1`

    3. Klik op **OK** om het venster **Inhoud weergeven** te sluiten.

    ![Vul de waarden zoals hierboven.](./media/active-directory-saas-ie-group-policy/show-contents.png)

5. Klik op **OK** om uw wijzigingen toe te passen en sluit het venster **Lijst met invoegtoepassingen** .

De uitbreiding moet nu worden ingeschakeld voor computers in de geselecteerde organisatie-eenheid. [Meer informatie over het gebruik van Groepsbeleid of Internet Explorer-invoegtoepassingen uit te schakelen.](https://technet.microsoft.com/library/dn454941.aspx)

##<a name="step-5-optional-disable-remember-password-prompt"></a>Stap 5 (optioneel): "Wachtwoord onthouden" Prompt uitschakelen

Wanneer gebruikers aanmelden bij websites met de uitbreiding van het Configuratiescherm toegang, kunnen Internet Explorer de volgende gevraagd "Wilt u uw wachtwoord opslaan?" weergeven

![](./media/active-directory-saas-ie-group-policy/remember-password-prompt.png)

Als u voorkomen dat gebruikers zien deze vraag verschijnt wilt, volgt u de onderstaande stappen om te voorkomen dat automatisch aanvullen van het onthouden van wachtwoorden:

1. Ga naar het volgende pad in het venster **Editor voor Groepsbeleidsbeheer** . Opmerking deze configuratie-instelling is alleen beschikbaar onder **Gebruikersconfiguratie**.
    - `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`

2. De instelling van **de functie automatisch aanvullen voor gebruikersnamen en wachtwoorden op formulieren inschakelen**met de naam vinden.

    > [AZURE.NOTE] Een overzicht van vorige versies van Active Directory kunnen deze instelling met de naam **automatisch aanvullen voor het opslaan van wachtwoorden niet toestaan**. De configuratie voor die instelling verschilt van de instelling die in deze zelfstudie wordt beschreven.

    ![Vergeet niet om te zoeken naar dit onder instellingen.](./media/active-directory-saas-ie-group-policy/disable-auto-complete.png)

3. Klik met de rechtermuisknop op de bovenstaande instellingen en selecteer **bewerken**.

4. Selecteer **uitgeschakeld**in het venster met de titel **de functie automatisch aanvullen voor gebruikersnamen en wachtwoorden op formulieren**.

    ![Selecteer uitschakelen](./media/active-directory-saas-ie-group-policy/disable-passwords.png)

5. Klik op **OK** om deze wijzigingen toe te passen en sluit het venster.

Gebruikers zullen niet langer hun referenties of automatisch aanvullen gebruiken voor toegang tot eerder opgeslagen referenties op te slaan. Dit beleid evenwel toestaan dat gebruikers automatisch aanvullen gebruiken voor andere typen velden, zoals velden met zoekcriteria blijven.

> [AZURE.WARNING] Als dit beleid is ingeschakeld nadat u gebruikers hebt gekozen voor het opslaan van sommige referenties, dit beleid zal *niet* wissen van de referenties die al zijn opgeslagen.

##<a name="step-6-testing-the-deployment"></a>Stap 6: De implementatie testen

Volg de onderstaande stappen om te controleren of als de implementatie van extensie voltooid is:

1. Als u met de **Configuratie van de Computer**hebt geïmplementeerd, aanmelden bij een computer die deel uitmaakt van de organisatie-eenheid die u hebt geselecteerd in [stap 2: de groepsbeleidsobjecten maken](#step-2-create-the-group-policy-object). Als u met de **Configuratie van de gebruiker**hebt geïmplementeerd, moet u zich aanmelden als een gebruiker die deel uitmaakt van de organisatie-eenheid.

2. Het duurt een paar teken modules voor het Groepsbeleid volledig bijwerken met deze computer. Open **een opdrachtpromptvenster** als u de update, en voer de volgende opdracht uit:`gpupdate /force`

3. Moet u opnieuw opstarten van de computer voor de installatie plaatsvindt. Bootup kan aanzienlijk langer duren dan normaal tijdens de extensie installeert.

4. Start opnieuw op en open **Internet Explorer**. Klik op **Tools** (het pictogram van het vistuig) in de rechterbovenhoek van het venster en vervolgens selecteert u **Invoegtoepassingen beheren**.

    ![Ga naar Extra > invoegtoepassingen beheren](./media/active-directory-saas-ie-group-policy/manage-add-ons.png)

5. Controleer of de **Extensie van Configuratiescherm Access** is geïnstalleerd en dat de **Status** is ingesteld op **ingeschakeld**in het venster **Invoegtoepassingen beheren** .

    ![Controleer of de uitbreiding toegang tot Configuratiescherm is geïnstalleerd en is ingeschakeld.](./media/active-directory-saas-ie-group-policy/verify-install.png)

## <a name="related-articles"></a>Verwante artikelen

- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Toegang tot toepassingen en single sign-on met Azure Active Directory](active-directory-appssoaccess-whatis.md)
- [De uitbreiding van het Configuratiescherm toegang tot het oplossen van problemen voor Internet Explorer](active-directory-saas-ie-troubleshooting.md)
