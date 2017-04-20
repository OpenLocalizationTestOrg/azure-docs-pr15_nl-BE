<properties
    pageTitle="Azure AD Join instellen voor uw gebruikers | Microsoft Azure"
    description="Wordt beschreven hoe beheerders kunnen instellen Azure AD Join voor directory op gebouwen en de registratie."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="setting-up-azure-ad-join-in-your-organization"></a>Azure AD Join instellen in uw organisatie

Voordat u Azure Active Directory deelnemen aan (Azure AD Join) instelt, moet u handmatig maken van beheerde accounts in Azure AD of synchroniseren van uw map op locatie van gebruikers naar de cloud.

Gedetailleerde instructies voor het synchroniseren van uw gebruikers op ruimten naar Azure AD valt [uw identiteiten op ruimten met Azure Active Directory integreren](active-directory-aadconnect.md).


Zie handmatig wilt maken en gebruikers in AD Azure beheren, [Gebruikersbeheer in Azure AD](https://msdn.microsoft.com/library/azure/hh967609.aspx).

## <a name="set-up-device-registration"></a>Instellen van de registratie
1. Meld u als beheerder op de Azure Portal.
2. Selecteer **Active Directory**in het linkerdeelvenster.
3. Selecteer op het tabblad **map** de map.
4. Selecteer in het tabblad **configureren** .
5. Ga naar de sectie **Devices** .
6. Stel op het tabblad **apparaten** in de volgende:  
   * **MAXIMUM aantal van apparaten PER gebruiker**: Selecteer het maximum aantal apparaten die een gebruiker in AD Azure kan hebben.  Als een gebruiker dit quotum is bereikt, is dat niet meer kunnen extra apparaten toevoegen totdat een of meer van hun bestaande apparaten worden verwijderd.
   * **Vereisen MEERLEDIGE verificatie op JOIN apparaten**: instellen of gebruikers moeten een tweede factor verificatie deelnemen aan hun apparaat naar Azure AD bieden. Zie voor meer informatie over Azure meerledige verificatie [aan de slag met Azure meerledige verificatie in de cloud](..\multi-factor-authentication\multi-factor-authentication-get-started-cloud.md).
   * **Gebruikers kunnen AZURE AD JOIN apparaten**: Selecteer de gebruikers en groepen die verbinding met apparaten naar Azure AD.
   * **Extra beheerders op AZURE AD verbonden apparaten**: met Azure AD Premium of Enterprise Mobility Suite (EMS), kunt u kiezen welke gebruikers krijgen lokale administrator-bevoegdheden voor het apparaat. Globale beheerders en eigenaars van het apparaat zijn lokale administrator-rechten standaard toegekend.

<center>![Instellen van het apparaat regisration](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png)</center>

Nadat u een Azure AD Join hebt ingesteld voor uw gebruikers, kunnen ze verbinding maken met Azure AD via hun zakelijke of persoonlijke apparaten.

Hieronder staan de drie scenario's die u gebruiken kunt om uw gebruikers deelnemen aan Azure AD instellen:

- Gebruikers lid worden van een bedrijf eigendom apparaat rechtstreeks naar Azure AD.
- Gebruikers domeinen aan een apparaat eigendom zijn van een bedrijf aan Active Directory op de lokalen en het apparaat vervolgens uit te breiden naar Azure AD.
- Gebruikers toevoegen werken of school accounts aan Windows op een persoonlijk apparaat.

## <a name="additional-information"></a>Als u meer informatie
* [Windows 10 voor de onderneming: apparaten gebruik voor werk](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud-mogelijkheden aan Windows 10-apparaten via Azure Active Directory deelnemen aan uitbreiden](active-directory-azureadjoin-user-upgrade.md)
* [Meer informatie over gebruiksscenario's voor Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Domein behoren apparaten aansluiten op Azure AD voor Windows 10 ervaringen](active-directory-azureadjoin-devices-group-policy.md)
* [Azure AD Join instellen](active-directory-azureadjoin-setup.md)
