<properties
    pageTitle="Azure AD Domain Services: Azure AD Domain Services inschakelen | Microsoft Azure"
    description="Aan de slag met Azure Active Directory Domain Services"
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
    ms.topic="get-started-article"
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="enable-azure-ad-domain-services"></a>Azure AD Domain Services inschakelen

## <a name="task-3-enable-azure-ad-domain-services"></a>Taak 3: Azure AD Domain Services inschakelen
In deze taak kunt u Azure AD Domain Services voor uw map. De volgende configuratiestappen zodat Azure AD Domain Services voor uw map uitvoeren.

1. Ga naar de **Azure klassieke portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Selecteer het knooppunt **Active Directory** in het linkerdeelvenster.

3. Selecteer de huurder Azure AD (map) dat u wilt inschakelen, Azure AD Domain Services.

    ![Azure AD map selecteren](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Klik op het tabblad **configureren** .

    ![Tabblad map configureren](./media/active-directory-domain-services-getting-started/configure-tab.png)

5. Blader naar een sectie **domeinservices**.

    ![Sectie voor configuratie van Domain Services](./media/active-directory-domain-services-getting-started/domain-services-configuration.png)

6. Schakel de optie met de titel **domain services voor deze map in te schakelen** op **Ja**. Ziet u een paar meer configuratieopties voor Azure AD Domain services weergegeven op de pagina.

    ![Domain Services inschakelen](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

    > [AZURE.NOTE] Wanneer u een AD-domein Azure Services voor uw huurder inschakelt, wordt AD Azure genereert en de referentie Kerberos en NTLM-hashes, die vereist zijn voor het verifiëren van gebruikers worden opgeslagen.

7. Geef de **DNS-domeinnaam van domeinservices**.

   - Naam van het standaarddomein van de map (dat wil zeggen, eindigend met de **. onmicrosoft.com** domeinachtervoegsel) is standaard ingeschakeld.

   - De lijst bevat alle domeinen die zijn geconfigureerd voor de map Azure AD – inclusief gecontroleerd als niet-geverifieerde domeinen die u in het tabblad 'Domeinen configureert'.

   - Daarnaast kunt u ook een aangepaste domeinnaam typen. In dit voorbeeld hebben we in aangepaste domeinnaam 'contoso100.com' ingevoerd.

     > [AZURE.WARNING] Controleer of het voorvoegsel van het domein van de domeinnaam die u opgeeft (bijvoorbeeld 'contoso100' in het domein 'contoso100.com') minder dan 15 tekens is. U kunt een domein Azure AD Domain Services maken met een voorvoegsel domein meer dan 15 tekens bevatten.

8. Zorg ervoor dat de DNS-domeinnaam die u hebt gekozen voor het beheerde domein niet in het virtuele netwerk bestaat nog. Controleer met name indien:

   - u hebt al een domein met dezelfde DNS-domeinnaam op het virtuele netwerk.

   - het virtuele netwerk dat u hebt geselecteerd heeft een VPN-verbinding met het netwerk op gebouwen en hebt u een domein met dezelfde DNS-domeinnaam in het netwerk op gebouwen.

   - u hebt een bestaande service cloud met die naam op het virtuele netwerk.

9. De volgende stap is het selecteren van een virtueel netwerk waar u graag Azure AD Domain Services beschikbaar zijn. Selecteer de virtuele netwerk en specifieke subnet dat u hebt gemaakt in de vervolgkeuzelijst met de titel **domain services verbinding maken met deze virtuele netwerk**.

   - Zorg ervoor dat het virtuele netwerk dat u hebt opgegeven tot een Azure regio worden ondersteund door Azure AD Domain Services behoort. Zie de pagina [Azure services per regio](https://azure.microsoft.com/regions/#services/) te weten de Azure gebieden waarin Azure AD Domain Services beschikbaar is.

   - Virtuele netwerken die deel uitmaken van een regio waar Azure AD Domain Services wordt niet ondersteund worden niet weergegeven in de vervolgkeuzelijst.
   
   - Gebruik een specifiek subnet binnen het virtuele netwerk voor Azure AD Domain Services. Zorg ervoor dat u het subnet gateway inschakelt. Zie [Overwegingen bij het netwerk](active-directory-ds-networking.md). 

   - Virtuele netwerken die zijn gemaakt met behulp van bronbeheer Azure niet op dezelfde manier in de vervolgkeuzelijst. Virtuele netwerken op basis van een Resource Manager worden momenteel niet ondersteund door Azure AD Domain Services.

10. Azure AD-domein als Services wilt inschakelen, klikt u op **Opslaan** in het taakvenster onder aan de pagina.

11. De pagina bevat een ' in behandeling... " status, terwijl Azure AD Domain Services wordt ingeschakeld voor de map.

    ![Domain Services - staat in afwachting van inschakelen](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

    > [AZURE.NOTE] Azure AD Domain Services biedt een hoge beschikbaarheid van uw domein beheerd. Nadat u AD-Domain Azure Services inschakelt, ziet u de IP-adressen waarop Domain Services beschikbaar op de virtuele netwerk één voor één te zien zijn. Het tweede IP-adres weergegeven kort, zoals de service kunt u snel hoge beschikbaarheid voor uw domein. Wanneer de hoge beschikbaarheid is geconfigureerd en ingeschakeld voor uw domein, worden er twee IP-adressen in het gedeelte **domain services** van het tabblad **configureren** .

12. Na 20-30 minuten ziet u het eerste IP-adres waarop Domain Services beschikbaar in het virtuele netwerk in het veld **IP-adres** op de pagina **configureren is** .

    ![Domain Services ingeschakeld - eerste IP-adres ingericht](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)

13. Als hoge beschikbaarheid voor uw domein operationeel is, ziet u twee IP-adressen die worden weergegeven op de pagina. Uw beheerde domein is beschikbaar op de geselecteerde virtuele netwerk op deze twee IP-adressen. Ziet u de IP-adressen zodat u kunt bijwerken de DNS-instellingen voor het virtuele netwerk. In deze stap kunt virtuele machines op het virtuele netwerk verbinding maken met het domein voor bewerkingen zoals aan domein toevoegen.

    ![Domain Services ingeschakeld - ingericht voor zowel IP-adressen](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [AZURE.NOTE] Afhankelijk van de grootte van uw huurder Azure AD (van gebruikers, groepen, enz.), synchronisatie met uw beheerde domein duurt. Het synchronisatieproces gebeurt op de achtergrond. Grote huurders met tienduizenden objecten duurt een dag of twee voor alle gebruikers, groepslidmaatschappen en referenties te synchroniseren.

<br>

## <a name="task-4---update-dns-settings-for-the-azure-virtual-network"></a>Taak 4 - Update DNS-instellingen voor het virtuele netwerk van Azure
De volgende configuratietaak is voor het [bijwerken van de DNS-instellingen voor het virtuele netwerk van Azure](active-directory-ds-getting-started-dns.md).
