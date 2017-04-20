<properties
    pageTitle="Schakel Microsoft Windows Hello voor bedrijven in uw organisatie | Microsoft Azure"
    description="Implementatie instructies Microsoft Passport is ingeschakeld in uw organisatie."
    services="active-directory"
    documentationCenter=""
    keywords="Microsoft Passport, Microsoft Windows Hello voor de implementatie van zakelijke configureren"
    authors="markusvi"
    manager="femila"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="markvi"/>


# <a name="enable-microsoft-windows-hello-for-business-in-your-organization"></a>Schakel Microsoft Windows Hello voor bedrijven in uw organisatie

Na het [aansluiten van een domein behoren Windows 10-apparaten met Azure Active Directory](active-directory-azureadjoin-devices-group-policy.md), doe het volgende als u Microsoft Windows Hello voor bedrijven in uw organisatie:

1. Implementatie van System Center Configuration Manager  

2. Beleidsinstellingen configureren

3. Het certificaat profiel configureren  




## <a name="deploy-system-center-configuration-manager"></a>Implementatie van System Center Configuration Manager 

Voor de implementatie van gebruikerscertificaten op basis van Windows Hello voor Business-sleutels, moet u het volgende:

- **Huidige tak van system Center Configuration Manager** - u moet versie 1606 of hoger installeren. Zie de [documentatie voor System Center Configuration Manager](https://technet.microsoft.com/library/mt346023.aspx) en [System Center Configuration Manager Team Blog](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx)voor meer informatie.
- **Openbare-sleutelinfrastructuur (PKI)** - waarmee Microsoft Windows Hello voor bedrijven met behulp van certificaten, moet u een PKI in plaats hebben. Als u er geen hebt, of u niet wilt gebruiken voor gebruikerscertificaten, kunt u een nieuwe domeincontroller met Windows Server 2016 build 10551 (of hoger) geïnstalleerd implementeren. Volg de stappen in [een replica-domeincontroller in een bestaand domein installeren](https://technet.microsoft.com/library/jj574134.aspx) of [Installeer een nieuwe Active Directory-forest als u een nieuwe omgeving maakt](https://technet.microsoft.com/library/jj574166). (De ISO-bestanden worden gedownload van [Signiant medium](https://datatransfer.microsoft.com/signiant_media_exchange/spring/main?sdkAccessible=true)).


## <a name="configure-policy-settings"></a>Beleidsinstellingen configureren

Configureren van de Microsoft Windows Hello voor Business-beleidsinstellingen, hebt u twee mogelijkheden:

- Groepsbeleid in Active Directory 
- De System Center Configuration Manager 


Via Groepsbeleid in Active Directory is de aanbevolen methode voor het configureren van Microsoft Windows Hello voor zakelijke instellingen. 



Met behulp van System Center Configuration Manager is de aanbevolen methode als u ook het gebruik van certificaten. In dit scenario:

- Zorgt u voor compatibiliteit met nieuwere implementatiescenario 's
- Op de client Windows 10 versie 1607 of hoger vereist.

### <a name="configure-microsoft-windows-hello-for-business-via-group-policy-in-active-directory"></a>Configureer Microsoft Windows Hello voor bedrijven via Groepsbeleid in Active Directory

 
**Stappen**:

1.  Open Serverbeheer en Ga naar **Extra** > **Voor Groepsbeleidsbeheer**.
2.  Ga naar het knooppunt dat overeenkomt met het domein waarin u wilt deelnemen aan Azure AD inschakelen van Group Policy Management.
3.  Klik met de rechtermuisknop op **Groepsbeleidsobjecten**en selecteer **Nieuw**. Het groepsbeleidsobject een naam geven, bijvoorbeeld inschakelen Windows Hello voor bedrijven. Klik op **OK**.
4.  Klik met de rechtermuisknop op het groepsbeleidsobject en selecteer vervolgens **bewerken**.
5.  Ga naar **Computerconfiguratie** > **beleid** > **Beheersjablonen** > **Windows-onderdelen** > **Windows Hello voor bedrijven**.
6.  Klik met de rechtermuisknop **Schakelen Windows Hello voor bedrijven**en selecteer vervolgens **bewerken**.
7.  Selecteer het keuzerondje **ingeschakeld** en klik vervolgens op **toepassen**. Klik op **OK**.
8.  Nu kunt u het groepsbeleidobject koppelen aan een locatie van uw keuze. Als u wilt dat dit beleid voor alle apparaten Windows 10 deel uitmaakt van een domein in uw organisatie, het groepsbeleid te koppelen aan het domein. Bijvoorbeeld:
 - Een specifieke organisatie-eenheid (OU) in Active Directory waarin Windows 10-computers deel uitmaakt van een domein gevonden worden
 - Een specifieke groep met Windows 10 domein behoren computers automatisch worden geregistreerd met Azure Active Directory


### <a name="configure-windows-hello-for-business-using-system-center-configuration-manager"></a>Configureer Windows Hello voor bedrijven met behulp van System Center Configuration Manager


**Stappen**:


1. Open **System Center Configuration Manager**en Ga naar **activa en naleving > naleving instellingen > toegang tot de bronnen van bedrijf > Windows Hello voor zakelijke profielen**.

    ![Hallo Windows configureren voor Business](./media/active-directory-azureadjoin-passport-deployment/01.png)


2. In de werkbalk op de bovenkant, klikt u op **Maken Windows Hello voor zakelijke profiel**.

    ![Hallo Windows configureren voor Business](./media/active-directory-azureadjoin-passport-deployment/02.png)

2. In het **algemene** dialoogvenster voert u de volgende stappen uit:

    ![Hallo Windows configureren voor Business](./media/active-directory-azureadjoin-passport-deployment/03.png)

    een. Typ in het tekstvak **naam** een naam voor uw profiel, bijvoorbeeld **Mijn WHfB profiel**.

    b. Klik op **volgende**.


2. Selecteer in het dialoogvenster **Ondersteunde Platforms voor** de platforms die zijn voorzien van deze Windows Hello voor zakelijke profiel en klik op **volgende**.

    ![Hallo Windows configureren voor Business](./media/active-directory-azureadjoin-passport-deployment/04.png)


2. In het dialoogvenster **Instellingen voor** de volgende stappen uitvoeren:

    ![Hallo Windows configureren voor Business](./media/active-directory-azureadjoin-passport-deployment/05.png)

    een. **Configureer Windows Hello voor bedrijven**, selecteren **ingeschakeld**.

    b. Als het **gebruik van een Trusted Platform Module (TPM)**, selecteer **vereist**. 

    c. Selecteer **op basis van certificaten**als **verificatiemethode**.

    d. Klik op **volgende**.



2. Klik op **volgende**in het dialoogvenster **Samenvatting** .

2. Klik op **sluiten**in het dialoogvenster **is voltooid** .


2. In de werkbalk op de bovenkant, klik op **Deploy**.

    ![Hallo Windows configureren voor Business](./media/active-directory-azureadjoin-passport-deployment/06.png)



## <a name="configure-the-certificate-profile"></a>Het certificaat profiel configureren 

Als u van verificatie op basis van certificaten voor verificatie voor gebouwen gebruikmaakt, die u wilt configureren en implementeren van een certificaat profiel. Deze taak moet u voor het instellen van een NDES-server en de rol van de site certificaat registratiepunt in System Center Configuration Manager. Zie de [vereisten voor het certificaat profielen in Configuratiebeheer](https://technet.microsoft.com/library/dn261205.aspx)voor meer informatie.

1. Open **System Center Configuration Manager**en Ga naar **activa en naleving > naleving instellingen > toegang tot de bronnen van bedrijf > profielen certificaat**.


2. Selecteer een sjabloon met een smartcard aanmelden uitgebreid sleutelgebruik (EKU).

Op de pagina **SCEP inschrijving** van het certificaat profiel moet u **installeren bij Passport voor werk, anders mislukken** als de **Sleutel Storage Provider**kiezen.



## <a name="next-steps"></a>Volgende stappen
* [Windows 10 voor de onderneming: apparaten gebruik voor werk](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud-mogelijkheden aan Windows 10-apparaten via Azure Active Directory deelnemen aan uitbreiden](active-directory-azureadjoin-user-upgrade.md)
* [Verificatie van identiteiten zonder wachtwoorden via Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Meer informatie over gebruiksscenario's voor Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Domein behoren apparaten aansluiten op Azure AD voor Windows 10 ervaringen](active-directory-azureadjoin-devices-group-policy.md)
* [Azure AD Join instellen](active-directory-azureadjoin-setup.md)
