<properties
    pageTitle="Azure AD verbinden: De volgende stappen en het beheer van Azure AD verbinding | Microsoft Azure"
    description="Informatie over het uitbreiden van de standaardconfiguratie en operationele taken voor Azure AD verbinden."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a>Volgende stappen en het beheren van Azure AD verbinden
De volgende zijn geavanceerde operationele onderwerpen waarmee u Azure Active Directory verbinding maken om te voldoen aan de wensen en behoeften van uw organisatie aanpassen.  

## <a name="add-additional-sync-administrators"></a>Synchronisatie van extra beheerders toevoegen
Standaard worden alleen de gebruiker die de installatie en de lokale groep administrators heeft kunnen de geïnstalleerde sync-engine te beheren. Voor andere mensen te kunnen openen en beheren van de synchronisatie-engine, de groep met de naam ADSyncAdmins op de lokale server te vinden en toevoegen aan deze groep.

## <a name="assigning-licenses-to-azure-ad-premium-and-enterprise-mobility-users"></a>Toewijzen van licenties aan gebruikers van Azure AD Premium en Enterprise Mobility

Nu dat uw gebruikers zijn gesynchroniseerd met de cloud, moet u deze licentie toewijzen zodat ze kunnen aan de slag met cloud-apps zoals Office 365.

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>Voor het toewijzen van een Azure AD premie of een licentie voor Enterprise Mobility Suite
--------------------------------------------------------------------------------
1. Aanmelden op de Portal Azure als beheerder.
2. Selecteer **Active Directory**aan de linkerkant.
3. Dubbelklik op de map met de gebruikers die u wilt inschakelen op de pagina Active Directory.
4. Selecteer **licenties**aan de bovenkant van de pagina directory.
5. Selecteer Active Directory Premium of Enterprise Mobility Suite op de pagina licenties en klik op **toewijzen**.
6. Selecteer de gebruikers die u wilt toewijzen van licenties in het dialoogvenster en klikt u op het vinkje op de wijzigingen op te slaan.


## <a name="verifying-the-scheduled-synchronization-task"></a>De geplande synchronisatietaak controleren
Als u controleren op de status van een synchronisatie dat kunt u dit doen wilt door in de portal Azure controleren.

### <a name="to-verify-the-scheduled-synchronization-task"></a>Om te controleren of de geplande taak
--------------------------------------------------------------------------------
1. Aanmelden op de Portal Azure als beheerder.
2. Selecteer **Active Directory**aan de linkerkant.
3. Dubbelklik op de map met de gebruikers die u wilt inschakelen op de pagina Active Directory.
4. Selecteer boven aan de pagina directory, **Directory-integratie**.
5. Onder de integratie met lokale active directory-Opmerking de tijd van laatste synchronisatie.

<center>![Wolk](./media/active-directory-aadconnect-whats-next/verify.png)</center>

## <a name="starting-a-scheduled-synchronization-task"></a>Een geplande synchronisatietaak starten
Als u nodig hebt voor het uitvoeren van een taak kunt u hiervoor u via de wizard Azure AD verbinding opnieuw.  U moet uw Azure AD referenties op te geven.  In de wizard, selecteert u de taak **aanpassen synchronisatie-opties** en klik op volgende in de wizard. Aan het einde, ervoor te zorgen dat het selectievakje **Start de synchronisatie als de eerste configuratie is voltooid** is ingeschakeld.

<center>![Wolk](./media/active-directory-aadconnect-whats-next/startsynch.png)</center>

Voor meer informatie over de synchronisatie Azure AD verbinden: Scheduler, Zie [Taakplanner verbinding Azure AD](active-directory-aadconnectsync-feature-scheduler.md)


## <a name="additional-tasks-available-in-azure-ad-connect"></a>Extra taken die beschikbaar zijn in Azure AD-verbinding maken
Na de initiële installatie van Azure AD verbinden, kunt u altijd start de wizard opnieuw uit de Azure AD Connect start of het bureaublad snelkoppeling.  Ziet u dat opnieuw doorlopen van de wizard nieuwe opties in de vorm van extra taken biedt.  

De volgende tabel bevat een overzicht van deze taken en een korte beschrijving van elk van hen.

![Lid worden van de regel](./media/active-directory-aadconnect-whats-next/addtasks.png)


Extra taak | Beschrijving
------------- | ------------- |
Het geselecteerde scenario weergeven  |Kunt u uw huidige Azure AD verbinden oplossing weergeven.  Dit omvat de algemene instellingen, mappen gesynchroniseerd, synchronisatie-instellingen, enz.
Synchronisatie-opties aanpassen | Hiermee kunt u de huidige configuratie, met inbegrip van aanvullende Active Directory-forests toe te voegen aan de configuratie of het inschakelen van synchronisatie-opties zoals de gebruiker te wijzigen groep, apparaat of wachtwoord terugschrijven.
Staging-modus inschakelen |  Hiermee kunt u naar fase informatie die later worden gesynchroniseerd, maar niets worden geëxporteerd naar Azure AD of Active Directory.  Hiermee kunt u de synchronisatie bekijken voordat ze optreden.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md).
