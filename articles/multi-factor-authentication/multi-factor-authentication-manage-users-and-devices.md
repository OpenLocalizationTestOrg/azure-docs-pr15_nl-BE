<properties 
    pageTitle="Azure meerledige verificatie rapporten"
    description="Beschrijving van het wijzigen van instellingen voor gebruikers bijvoorbeeld dwingen de gebruiker het bewijs van proces opnieuw doen."
    documentationCenter=""
    services="multi-factor-authentication"
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="managing-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Beheer van gebruikersinstellingen met Azure meerledige verificatie in de cloud

Als beheerder kunt u de volgende instellingen voor de gebruiker en het apparaat te beheren.  

- [Geselecteerde gebruikers moeten opnieuw contact methoden bieden](#require-selected-users-to-provide-contact-methods-again)
- [Bestaande wachtwoorden app gebruikers verwijderen](#delete-users-existing-app-passwords)
- [MVR gesloten op alle zwevende apparaten voor een gebruiker terugzetten](#restore-mfa-on-all-suspended-devices-for-a-user)






Dit is handig als een computer of apparaat wordt gestolen of u moet een gebruikerstoegang voor te verwijderen.


## <a name="require-selected-users-to-provide-contact-methods-again"></a>Geselecteerde gebruikers moeten opnieuw contact methoden bieden

Deze instelling wordt zorgt ervoor dat de gebruiker de registratieprocedure opnieuw uitvoeren als hij of zij zich aanmeldt. Zich bewust zijn dat de browser-apps blijft werken als de gebruiker wachtwoorden app voor hen heeft.  U kunt de app gebruikers wachtwoorden verwijderen door ook **alle bestaande app wachtwoorden worden gegenereerd door de geselecteerde gebruikers verwijderen**.

### <a name="how-to-require-users-to-provide-contact-methods-again"></a>Hoe moeten gebruikers contact methoden opnieuw opgeven




1. Aanmelden bij de klassieke Azure portal.
2. Aan de linkerkant, klikt u op Active Directory.
3. Klik onder op de map op de map voor de gebruiker die u vereisen wilt voor de contactwijze opnieuw.
4. Aan de bovenkant, klikt u op gebruikers.
5. Klik onderaan de pagina op beheren meerledige Auth. Hiermee opent u de pagina meerledige verificatie.
6. De gebruiker die u wilt beheren en schakel in het vak naast de naam te vinden. Wellicht moet u weer aan de bovenkant.
7. U krijgt de **gebruikersinstellingen beheren** rechts op de koppeling. Klik op het.
8. Schakel dit selectievakje in de **geselecteerde gebruikers om opnieuw contact methoden te bieden**.
![Beveiligingsmethoden voor contactpersoon](./media/multi-factor-authentication-manage-users-and-devices/reproofup.png)
10. Klik op opslaan.
11. Klik op sluiten

## <a name="delete-users-existing-app-passwords"></a>Bestaande wachtwoorden app gebruikers verwijderen

Hiermee verwijdert u alle wachtwoorden app die een gebruiker heeft gemaakt. Non-browser apps die gekoppeld aan deze app wachtwoorden zijn niet meer werken tot een nieuw wachtwoord voor de app is gemaakt.

### <a name="how-to-delete-users-existing-app-passwords"></a>Het verwijderen van bestaande wachtwoorden app gebruikers

1. Aanmelden bij de klassieke Azure portal.
2. Aan de linkerkant, klikt u op Active Directory.
3. Klik onder op de map op de map voor de gebruiker die u wilt verwijderen van de wachtwoorden voor app.
4. Aan de bovenkant, klikt u op gebruikers.
5. Klik onderaan de pagina op beheren meerledige Auth. Hiermee opent u de pagina meerledige verificatie.
6. De gebruiker die u wilt beheren en schakel in het vak naast de naam te vinden. Wellicht moet u weer aan de bovenkant.
7. U krijgt de **gebruikersinstellingen beheren** rechts op de koppeling. Klik op het.
8. Schakel dit selectievakje in **alle bestaande app wachtwoorden worden gegenereerd door de geselecteerde gebruikers verwijderen**.
![App wachtwoorden verwijderen](./media/multi-factor-authentication-manage-users-and-devices/deleteapppasswords.png)
10. Klik op opslaan.
10. Klik op sluiten.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>MVR gesloten op alle apparaten van onthouden voor een gebruiker terugzetten

Beheerders hebben de mogelijkheid om te herstellen van meerledige verificatie van gebruikers, apparaten en browsers. Wanneer u dit doet, wordt dit de bewaren MVR gesloten verwijderen van alle apparaten van de gebruiker en de browser en de gebruiker moet gebruiken MVR gesloten wanneer de volgende keer aanmelden.

### <a name="how-to-restore-mfa-on-all-suspended-devices-for-a-user"></a>MVR gesloten op alle zwevende apparaten voor een gebruiker terugzetten

1. Aanmelden bij de klassieke Azure portal.
2. Aan de linkerkant, klikt u op Active Directory.
3. Klik onder op de map op de map voor de gebruiker die u terugzetten de MVR gesloten wilt op.
4. Aan de bovenkant, klikt u op gebruikers.
5. Klik onderaan de pagina op beheren meerledige Auth. Hiermee opent u de pagina meerledige verificatie.
6. De gebruiker die u wilt beheren en schakel in het vak naast de naam te vinden. Wellicht moet u weer aan de bovenkant.
7. U krijgt de **gebruikersinstellingen beheren** rechts op de koppeling. Klik op het.
8. Schakel dit selectievakje in **meerledige verificatie op alle apparaten van onthouden herstellen**
![app wachtwoorden verwijderen](./media/multi-factor-authentication-manage-users-and-devices/rememberdevices.png)
9. Klik op opslaan.
10. Klik op sluiten.
