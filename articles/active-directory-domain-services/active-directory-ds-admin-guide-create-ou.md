<properties
    pageTitle="Azure Active Directory Domain Services: Administration Guide | Microsoft Azure"
    description="Maak een organisatie-eenheid (OU) op Azure AD beheerd domeinen"
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

# <a name="create-an-organizational-unit-ou-on-an-azure-ad-domain-services-managed-domain"></a>Maak een organisatie-eenheid (OU) op een beheerde Azure AD Domain Services-domein
Azure AD beheerde domeinen bevatten twee ingebouwde containers respectievelijk 'AADDC Computers' en 'AADDC Users' genoemd. De ' AADDC' container is computerobjecten voor alle computers die lid zijn van het domein van de beheerde. De ' AADDC' gebruikerscontainer bevat de gebruikers en groepen in de huurder Azure AD. In sommige gevallen mogelijk serviceaccounts maken op het beheerde domein implementeren werkbelasting. U kunt voor dit doel, een aangepaste organisatie-eenheid (OU) op het beheerde domein maken en serviceaccounts binnen de organisatie-eenheid maken. In dit artikel wordt beschreven hoe u een organisatie-eenheid maken in uw domein beheerd.


## <a name="install-ad-administration-tools-on-a-domain-joined-virtual-machine-for-remote-administration"></a>AD-beheerprogramma's installeren op een virtuele machine domein behoren voor extern beheer
Azure AD beheerde domeinen kunnen extern worden beheerd via vertrouwde Active Directory-beheerprogramma's zoals de Active Directory Administrative Center (ADAC) of AD PowerShell. Pachtersbeheerders beschikt niet over bevoegdheden op domeincontrollers in het domein beheerd via Extern bureaublad verbinding te maken. Voor het beheren van het domein beheerd, de functie van AD administration tools te installeren op een virtuele machine toegevoegd aan het domein beheerd. Raadpleeg het artikel met de titel van [een AD-domein Azure Services beheerd domein beheren](active-directory-ds-admin-guide-administer-domain.md) voor instructies.

## <a name="create-an-organizational-unit-on-the-managed-domain"></a>Een organisatie-eenheid maken in het domein beheerd
Nu dat de AD-beheerprogramma's zijn geïnstalleerd op het domein lid virtuele machine, we kunnen deze hulpprogramma's gebruiken voor het maken van een organisatie-eenheid in het domein beheerd. Voer de volgende stappen uit:

> [AZURE.NOTE] Alleen leden van de groep 'Beheerders AAD DC' hebben de vereiste bevoegdheden voor het maken van een aangepaste organisatie-eenheid. Zorg ervoor dat u de volgende stappen uitvoeren als een gebruiker die tot deze groep behoort.

1. Van het startscherm, klik op **Systeembeheer**. Hier ziet u de beheerprogramma's van AD op de virtuele machine is geïnstalleerd.

    ![Beheerprogramma's zijn geïnstalleerd op de server](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)

2. Klik op **Active Directory Administrative Center**.

    ![Active Directory Administrative Center](./media/active-directory-domain-services-admin-guide/adac-overview.png)

3. Klik op de domeinnaam in (bijvoorbeeld ' contoso100.com') in het linkerdeelvenster het domein.

    ![ADAC - domein weergeven](./media/active-directory-domain-services-admin-guide/create-ou-adac-overview.png)

4. Klik op **Nieuw** onder het knooppunt van de naam in **het deelvenster rechts** . In dit voorbeeld we klikt u op **Nieuw** onder het knooppunt 'contoso100(local)' in **het deelvenster rechts** .

    ![ADAC - nieuwe organisatie-eenheid](./media/active-directory-domain-services-admin-guide/create-ou-adac-new-ou.png)

5. Hier ziet u de optie voor het maken van een organisatie-eenheid. Klik op **Organisatie-eenheid** om het dialoogvenster **Organisatie-eenheid maken** .

6. Geef een **naam** voor de nieuwe organisatie-eenheid in het dialoogvenster **Organisatie-eenheid maken** . Geef een korte beschrijving voor de organisatie-eenheid. U kunt ook het veld **Beheerd door** voor de organisatie-eenheid instellen. Klik op **OK**om de aangepaste organisatie-eenheid.

    ![ADAC - dialoogvenster organisatie-eenheid maken](./media/active-directory-domain-services-admin-guide/create-ou-dialog.png)

7. De nieuwe organisatie-eenheid moet nu worden weergegeven in het AD Administrative Center (ADAC).

    ![ADAC - organisatie-eenheid gemaakt](./media/active-directory-domain-services-admin-guide/create-ou-done.png)


## <a name="permissionssecurity-for-newly-created-ous"></a>Beveiliging/toestemmingen voor nieuwe organisatie-eenheden
Standaard is de gebruiker die heeft gemaakt van de aangepaste organisatie-eenheid (lid van de groep 'Beheerders AAD DC') over de organisatie-eenheid beheerdersbevoegdheden (volledig beheer) toegekend. De gebruiker kan vervolgens doorgaan en bevoegdheden te verlenen aan andere gebruikers of aan de groep 'Beheerders AAD DC' naar wens. Zoals in de volgende schermafdruk, de gebruiker 'bob@domainservicespreview.onmicrosoft.com' volledige controle over de computer die de nieuwe organisatie-eenheid van 'MyCustomOU' gemaakt wordt verleend.

 ![ADAC - beveiliging voor nieuwe organisatie-eenheid](./media/active-directory-domain-services-admin-guide/create-ou-permissions.png)


## <a name="notes-on-administering-custom-ous"></a>Notities op een aangepaste organisatie-eenheden beheren
Nu dat u een aangepaste organisatie-eenheid hebt gemaakt, kunt u doorgaan en gebruikers, groepen, computers en serviceaccounts in deze organisatie-eenheid maken. U kunt gebruikers of groepen uit de 'AAD DC Users' OU naar aangepaste organisatie-eenheden verplaatsen.

> [AZURE.WARNING] Gebruikersaccounts, groepen, serviceaccounts en computerobjecten die u op een aangepaste organisatie-eenheden maakt zijn niet beschikbaar in uw huurder Azure AD. Met andere woorden, weergeven deze objecten niet met behulp van de API Azure AD grafiek of in de gebruikersinterface van Azure AD. Deze objecten zijn alleen beschikbaar in uw beheerde Azure AD Domain Services-domein.


## <a name="related-content"></a>Verwante inhoud

- [Een beheerde Azure AD Domain Services-domein beheren](active-directory-ds-admin-guide-administer-domain.md)

- [Active Directory Administrative Center: Aan de slag](https://technet.microsoft.com/library/dd560651.aspx)

- [Stapsgewijze handleiding voor serviceaccounts](https://technet.microsoft.com/library/dd548356.aspx)
