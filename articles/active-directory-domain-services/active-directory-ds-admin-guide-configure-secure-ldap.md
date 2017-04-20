<properties
    pageTitle="Veilige LDAP (LDAPS) configureren in Azure AD Domain Services | Microsoft Azure"
    description="Veilige LDAP (LDAPS) configureren voor een beheerde Azure AD Domain Services-domein"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Veilige LDAP (LDAPS) configureren voor een beheerde Azure AD Domain Services-domein
Dit artikel wordt beschreven hoe u beveiligde Lightweight Directory Access Protocol (LDAPS) kunt inschakelen voor uw beheerde Azure AD Domain Services-domein. Veilige LDAP wordt ook wel ' Lightweight Directory Access Protocol (LDAP) via Secure Sockets Layer (SSL) / Transport Layer Security (TLS)'.

## <a name="before-you-begin"></a>Voordat u begint
Voor het uitvoeren van de taken die in dit artikel worden vermeld, hebt u het volgende nodig:

1. Een geldig **abonnement Azure**.

2. Een **directory Azure AD** - hetzij gesynchroniseerd met een op locatie of map alleen-wolk.

3. **Azure AD Domain Services** moet zijn ingeschakeld voor de map Azure AD. Als u dit nog niet hebt gedaan, voert u alle taken die worden beschreven in de [handleiding aan de slag](./active-directory-ds-getting-started.md).

4. Een **certificaat dat moet worden gebruikt voor beveiligde LDAP**.
    - **Aanbevolen** : een certificaat aanvragen bij uw ondernemingscertificeringsinstantie of een openbare certificeringsinstantie. Deze optie is veiliger.
    - Ook kunt u ook [een zelf-ondertekend certificaat maken](#task-1---obtain-a-certificate-for-secure-ldap) zoals verderop in dit artikel wordt weergegeven.

<br>

### <a name="requirements-for-the-secure-ldap-certificate"></a>Vereisten voor het beveiligde LDAP-certificaat
Een geldig certificaat per de volgende richtlijnen aan te schaffen voordat u secure LDAP inschakelen. Er optreden fouten als u probeert beveiligde LDAP voor uw domein beheerd met een ongeldige/onjuist certificaat inschakelen.

1. **Vertrouwde uitgever** - het certificaat moet worden uitgegeven door een instantie die wordt vertrouwd door computers die verbinding moeten maken met het domein gebruiken, veilige LDAP. Deze autoriteit kan zijn van uw organisatie ondernemingscertificeringsinstantie of een openbare certificeringsinstantie die wordt vertrouwd door deze computers.

2. **Levensduur** - certificaat moet geldig zijn voor ten minste de volgende 3-6 maanden. Veilige LDAP-toegang tot uw beheerde domein wordt onderbroken wanneer het certificaat is verlopen.

3. **Objectnaam** - de naam van het certificaat moet een jokerteken voor uw domein beheerd. Bijvoorbeeld, als uw domein met de naam 'contoso100.com', van de naam van de certificaathouder moet zijn ' *. contoso100.com'. De DNS-naam (alternatieve naam subject) ingesteld op de jokertekennaam van deze.

3. **Sleutelgebruik** - het certificaat moet worden geconfigureerd voor de volgende doeleinden - digitale handtekeningen en uitwisselen.

4. **Certificaatdoeleinde** - certificaat voor SSL-serververificatie geldig moet zijn.

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a>Taak 1: een certificaat aanvragen voor beveiligde LDAP.
De eerste taak heeft betrekking op het verkrijgen van een certificaat dat wordt gebruikt voor beveiligde LDAP-toegang tot het domein beheerd. U hebt twee opties:

- Een certificaat aanvragen bij een certificeringsinstantie. De autoriteit is van uw organisatie ondernemings-CA of een openbare certificeringsinstantie.

- Een zelfondertekend certificaat maken.


### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a>Een (aanbevolen) optie - een beveiligde LDAP-certificaat verkrijgen bij een certificeringsinstantie
Als uw organisatie een enterprise openbare-sleutelinfrastructuur (PKI) implementeert, moet u een certificaat aanvragen bij een ondernemingscertificeringsinstantie (CA) voor uw organisatie. Als uw organisatie van een openbare certificeringsinstantie de certificaten heeft verkregen, moet u de beveiligde LDAP-certificaat verkrijgen van een openbare certificeringsinstantie.

Als u een certificaat aanvraagt, ervoor te zorgen dat u voldoen aan de vereisten die worden beschreven in [vereisten voor het beveiligde LDAP-certificaat](#requirements-for-the-secure-ldap-certificate).

> [AZURE.NOTE] Clientcomputers die verbinding moeten maken met de beheerde domein gebruiken, veilige LDAP moeten de uitgever van het LDAPS-certificaat te vertrouwen.


### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a>Optie B - een zelf-ondertekend certificaat maken voor beveiligde LDAP.
U kunt kiezen voor het maken van een zelfondertekend certificaat voor secure LDAP als:

- in uw organisatie geen certificaten van een ondernemingscertificeringsinstantie of
- u niet verwacht dat het gebruik van een certificaat van een openbare certificeringsinstantie.

**Maken van een zelfondertekend certificaat met PowerShell**

Opent een nieuw venster PowerShell als **beheerder** op uw Windows-computer en typ de volgende opdrachten voor het maken van een nieuw, zelfondertekend certificaat.

    $lifetime=Get-Date

    New-SelfSignedCertificate -Subject *.contoso100.com -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment -Type SSLServerAuthentication -DnsName *.contoso100.com

In het voorgaande voorbeeld vervangen door 'contoso100.com' met de DNS-naam van het beheerde Azure AD Domain Services-domein.

![Azure AD map selecteren](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

Het nieuwe zelf-ondertekend certificaat wordt in het certificaatarchief van de lokale computer geplaatst.


## <a name="task-2---export-the-secure-ldap-certificate-to-a-pfx-file"></a>Taak 2 - secure LDAP exporteren certificaat naar een. PFX-bestand
Voordat u deze taak ervoor te zorgen dat u het beveiligde LDAP-certificaat hebt ontvangen van uw ondernemingscertificeringsinstantie of een openbare certificeringsinstantie of een zelf-ondertekend certificaat hebt gemaakt.

Doe het volgende, als u wilt exporteren naar het LDAPS-certificaat een. PFX-bestand.

1. Druk op de knop **Start** en typt u **R**. In het dialoogvenster **uitvoeren** , typ **mmc** en klik op **OK**.

    ![Start de MMC-console](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)

2. Klik in de **Gebruikersaccountbeheer** -prompt op **Ja** om op te starten in MMC (Microsoft Management Console) als beheerder.

3. Klik in het menu **bestand** op **module toevoegen/verwijderen-in...**.

    ![Module toevoegen aan MMC-console](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)

4. Selecteer in het dialoogvenster **toevoegen of verwijderen van modules** de module **certificaten** en klik op de **toevoegen >** knop.

    ![De module Certificaten toevoegen aan MMC-console](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)

5. Selecteer **computeraccount** en klik op **volgende**in de **module Certificaten** wizard.

    ![Module Certificaten voor een computeraccount toevoegen](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)

6. Selecteer op de pagina **Computer selecteren** **lokale computer: (de computer waarop deze console wordt uitgevoerd)** en klik op **Voltooien**.

    ![Module Certificaten - select computer toevoegen](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)

7. Klik op **OK** om de module Certificaten aan MMC toevoegen in het dialoogvenster **toevoegen of verwijderen van modules** .

    ![De module Certificaten toevoegen aan een MMC - gedaan](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)

8. Klik in de MMC-venster op **Consolebasis**. Ziet u de module Certificaten geladen. Klik op **certificaten (lokale Computer)** uit te vouwen. Klik op de **persoonlijke** knooppunt, gevolgd door het knooppunt **certificaten** .

    ![Archief persoonlijke certificaten openen](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)

9. U ziet de zelf-ondertekend certificaat gemaakt. U kunt de eigenschappen van het certificaat om ervoor te zorgen dat op de windows PowerShell gemeld bij het maken van het certificaat komt overeen met de vingerafdruk onderzoeken.

10. Selecteer het zelfondertekende certificaat en **Klik met de rechtermuisknop**. Selecteer **Alle taken** in het menu met de rechtermuisknop en selecteer **exporteren...**.

    ![Certificaat exporteren](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)

11. Klik op **volgende**in de **Wizard Certificaat exporteren**.

    ![De wizard Certificaat exporteren](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)

12. Selecteer **Ja, de persoonlijke sleutel exporteren**op de pagina **Persoonlijke sleutel exporteren** en klik op **volgende**.

    ![De persoonlijke certificaatsleutel exporteren](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)

    > [AZURE.WARNING] U moet de persoonlijke sleutel en het certificaat exporteren. Als u een PFX die de persoonlijke sleutel voor het certificaat niet bevat opgeeft, mislukt veilige LDAP voor uw beheerde domein inschakelen.

13. Selecteer op de pagina **Bestandsindeling voor Export** **Personal Information Exchange - PKCS #12 (. (PFX)** als de bestandsindeling voor het geëxporteerde certificaat.

    ![Certificaat-bestandsindeling exporteren](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)

    > [AZURE.NOTE] Alleen de. De PFX-indeling wordt ondersteund. Exporteer het certificaat niet de. CER-bestandsindeling.

14. Selecteer op de pagina **beveiliging** de optie **wachtwoord** en typ een wachtwoord te beschermen de. PFX-bestand. Dit wachtwoord onthouden omdat deze nodig in de volgende taak zijn. Klik op **volgende** om door te gaan.

    ![Wachtwoord voor het certificaat exporteren ](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)

    > [AZURE.NOTE] Noteer dit wachtwoord. U moet deze tijdens het inschakelen van beveiligde LDAP voor dit domein beheerd in [taak 3 - secure LDAP voor beheerde domein inschakelen](#task-3---enable-secure-ldap-for-the-managed-domain)

15. Geef de bestandsnaam en de locatie waar u wilt exporteren van het certificaat op de pagina **te exporteren bestand** .

    ![Pad voor certificaat exporteren](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)

16. Klik op **Voltooien** om het exporteren van het certificaat naar een PFX-bestand op de volgende pagina. Dialoogvenster voor bevestiging te zien wanneer het certificaat is geëxporteerd.

    ![Gedaan certificaat exporteren](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)


## <a name="task-3---enable-secure-ldap-for-the-managed-domain"></a>Taak 3 - secure LDAP inschakelen voor het domein beheerd
Zodat veilige LDAP als volgt configureren:

1. Ga naar de **[Azure klassieke portal](https://manage.windowsazure.com)**.

2. Selecteer het knooppunt **Active Directory** in het linkerdeelvenster.

3. Selecteer de Azure AD-map (ook wel genoemd 'huurder'), waarop Azure AD Domain Services is ingeschakeld.

    ![Azure AD map selecteren](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Klik op het tabblad **configureren** .

    ![Tabblad map configureren](./media/active-directory-domain-services-getting-started/configure-tab.png)

5. Ga naar de sectie **domeinservices**. Wordt er een optie **Secure LDAP (LDAPS)** met de titel zoals in de volgende schermafdruk:

    ![Sectie voor configuratie van Domain Services](./media/active-directory-domain-services-admin-guide/secure-ldap-start.png)

6. Klik op de knop **... certificaat configureren** om het dialoogvenster **Certificaat configureren voor beveiligde LDAP** .

    ![Certificaat voor veilige LDAP configureren](./media/active-directory-domain-services-admin-guide/secure-ldap-configure-cert-page.png)

7. Klik op het mappictogram onder **PFX-bestand met certificaat** het PFX-bestand waarin het certificaat dat u wilt gebruiken voor beveiligde LDAP-toegang tot de beheerde domein opgeven. Ook het wachtwoord dat u hebt opgegeven bij het exporteren van het certificaat naar een PFX-bestand invoeren. Klik vervolgens op de knop Gereed op de onderkant.

    ![Beveiligde LDAP PFX-bestand en een wachtwoord opgeven](./media/active-directory-domain-services-admin-guide/secure-ldap-specify-pfx.png)

8. De sectie **domain services** van het tabblad **configureren** grijs moet ophalen en in de **in behandeling...** staat voor een paar minuten. De LDAPS-certificaat wordt gecontroleerd op nauwkeurigheid en veilige LDAP is geconfigureerd voor uw beheerde domein tijdens deze periode.

    ![Veilige LDAP - in afwachting van staat](./media/active-directory-domain-services-admin-guide/secure-ldap-pending-state.png)

    > [AZURE.NOTE] Het duurt ongeveer 10 tot 15 minuten secure LDAP voor uw beheerde domein inschakelen. Als de opgegeven beveiligde LDAP-certificaat niet overeenkomt met de criteria, veilige LDAP is niet ingeschakeld voor de map en er een storing. Bijvoorbeeld de naam van het domein klopt niet, het certificaat is verlopen of verloopt binnenkort enz.

9. Wanneer u beveiligde LDAP is ingeschakeld voor uw domein beheerd, de **in behandeling...** bericht moet verdwijnen. U ziet de blauwdruk van het certificaat weergegeven.

    ![Veilige LDAP ingeschakeld](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled.png)

<br>


## <a name="task-4---enable-secure-ldap-access-over-the-internet"></a>Taak 4 - beveiligde LDAP-toegang inschakelen via het internet
**Optionele taak** - als u niet wilt toegang tot de beheerde domein gebruik van LDAPS via het internet, gaat deze configuratietaak.

Voordat u deze taak begint, moet u ervoor zorgen dat u de stappen in een [taak 3](#task-3---enable-secure-ldap-for-the-managed-domain)hebt voltooid.

1. Hier ziet u een optie **Inschakelen SECURE LDAP toegang via het INTERNET** in het gedeelte **domain services** van de pagina **configureren** . Deze optie is standaard ingesteld op **Nee** omdat internettoegang tot de beheerde domein via beveiligde LDAP is standaard uitgeschakeld.

    ![Veilige LDAP ingeschakeld](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled2.png)

2. In-of uitschakelen, **veilige LDAP-toegang via het INTERNET inschakelen** op **Ja**. Klik op de knop **Opslaan** in het onderste deelvenster.
    ![Veilige LDAP ingeschakeld](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)

3. De sectie **domain services** van het tabblad **configureren** grijs moet ophalen en in de **in behandeling...** staat voor een paar minuten. Na enige tijd, is internettoegang tot uw beheerde domein via beveiligde LDAP ingeschakeld.

    ![Veilige LDAP - in afwachting van staat](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access-pending-state.png)

    > [AZURE.NOTE] Het duurt ongeveer 10 minuten toegang tot het internet via beveiligde LDAP voor uw beheerde domein inschakelen.

4. Wanneer u beveiligde LDAP-toegang tot uw beheerde domein via het internet is ingeschakeld, de **in behandeling...** bericht moet verdwijnen. Hier ziet u het externe IP-adres dat kan worden gebruikt voor toegang tot uw map via LDAPS in het veld **Externe IP-adres voor LDAPS toegang**.

    ![Veilige LDAP ingeschakeld](./media/active-directory-domain-services-admin-guide/secure-ldap-internet-access-enabled.png)

<br>

## <a name="task-5---configure-dns-to-access-the-managed-domain-from-the-internet"></a>Taak 5 - toegang tot de beheerde domein van internet DNS configureren
**Optionele taak** - als u niet wilt toegang tot de beheerde domein gebruik van LDAPS via het internet, gaat deze configuratietaak.

Voordat u deze taak begint, moet u ervoor zorgen dat u de stappen in een [taak 4](#task-4---enable-secure-ldap-access-over-the-internet)hebt voltooid.

Als u eenmaal hebt ingeschakeld beveiligde LDAP-toegang via het internet voor uw beheerde domein, moet u DNS bijwerken zodat clientcomputers dit beheerde domein kunnen vinden. Een extern IP-adres wordt aan het einde van taak 4 weergegeven op het tabblad **configureren** in het **Externe IP-adres voor LDAPS toegang**.

De externe DNS-provider zodanig configureren dat de DNS-naam van het beheerde domein (bijvoorbeeld ' contoso100.com') naar deze externe IP-adres verwijst. We moeten de volgende DNS-vermelding maken in ons voorbeeld:

    contoso100.com  -> 52.165.38.113

Dat is het - u bent nu klaar om te verbinden met de beheerde domein gebruiken, veilige LDAP via het internet.

> [AZURE.WARNING] Houd er rekening mee dat clientcomputers de uitgever van het LDAPS-certificaat worden tot stand kunnen brengen met de beheerde domein gebruik van LDAPS moeten vertrouwen. Als u een ondernemingscertificeringsinstantie of een openbaar vertrouwde certificeringsinstantie gebruikt, hoeft u niet te ondernemen omdat deze certificaatverleners clientcomputers worden vertrouwd. Als u een zelf-ondertekend certificaat gebruikt, moet u het openbare deel van een zelf-ondertekend certificaat installeren in het archief met vertrouwde certificaten op de clientcomputer.

<br>

## <a name="related-content"></a>Verwante inhoud

- [Een beheerde Azure AD Domain Services-domein beheren](active-directory-ds-admin-guide-administer-domain.md)
