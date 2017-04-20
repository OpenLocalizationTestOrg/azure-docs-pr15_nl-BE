<properties 
    pageTitle="Microsoft Azure meerledige verificatie gebruiker Staten"
    description="Informatie over gebruiker Staten in Azure MVR gesloten."
    services="multi-factor-authentication"
    documentationCenter=""
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

# <a name="user-states-in-azure-multi-factor-authentication"></a>Lidstaten in Azure meerledige verificatie gebruiker

Gebruikersaccounts in Azure meerledige verificatie mogelijk hebben de volgende drie verschillende statussen:

Staat | Beschrijving |Waarin dit probleem optreedt niet browser-apps| Notities
:-------------: | :-------------: |:-------------: |:-------------: |
Uitgeschakeld | De standaardstatus voor een nieuwe gebruiker niet ingeschreven in een meerledige verificatie.|Nee|De gebruiker niet meerledige verificatie gebruikt.
Ingeschakeld |De gebruiker heeft zijn ingeschreven in een meerledige verificatie.|Nr.  Ze blijven werken totdat de registratie is voltooid.|De gebruiker is ingeschakeld, maar niet de registratieprocedure heeft voltooid. Zij wordt gevraagd het proces op het volgende teken in te voeren.
Afgedwongen|De gebruiker heeft zijn ingeschreven en het registratieproces voor meerledige verificatie heeft voltooid.|Ja.  Apps vereist app wachtwoord. | De gebruiker kan of mag geen inschrijving hebt voltooid. Als ze het registratieproces hebt voltooid, gebruikt ze meerledige verificatie. Anders wordt wordt de gebruiker gevraagd het proces op het volgende teken in te voeren.

## <a name="changing-a-user-state"></a>De gebruikersstatus van een wijzigen
Een staat gebruikers verandert afhankelijk van het al dan niet instellen voor MVR gesloten is en of de gebruiker het proces is voltooid.  Wanneer u voor een gebruiker MVR gesloten inschakelt, wordt de status wordt gewijzigd van gebruikers ingeschakeld uitgeschakeld.  Zodra de gebruiker, waarvan de status is gewijzigd in ingeschakeld, zich aanmeldt en het proces is voltooid, wordt de status gewijzigd in afgedwongen.  

### <a name="to-view-a-users-state"></a>De status van een gebruiker weergeven
--------------------------------------------------------------------------------
1.  Aanmelden bij de **Azure klassieke portal** als beheerder.
2.  Aan de linkerkant, klikt u op **Active Directory**.
3.  Klik onder op de **map** op de map voor de gebruiker die u wilt inschakelen.
![Klik op map](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Aan de bovenkant, klikt u op **gebruikers**.
5.  Klik onderaan de pagina **Meerledige verificatie beheren**.
![Klik op map](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Hiermee opent u een nieuwe browser tab.  Kun je de status van gebruikers weergeven.
![Klik op map](./media/multi-factor-authentication-get-started-user-states/userstate1.png)

###<a name="to-change-the-state-from-disabled-to-enabled"></a>Uitgeschakeld als u wilt wijzigen van de status van ingeschakeld
1.  Aanmelden bij de **Azure klassieke portal** als beheerder.
2.  Aan de linkerkant, klikt u op **Active Directory**.
3.  Klik onder op de **map** op de map voor de gebruiker die u wilt inschakelen.
![Klik op map](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Aan de bovenkant, klikt u op **gebruikers**.
5.  Klik onderaan de pagina **Meerledige verificatie beheren**.
![Klik op map](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Hiermee opent u een nieuwe browser tab.  Zoek de gebruiker die u wilt inschakelen voor een meerledige verificatie. Wellicht moet u weer aan de bovenkant. Zorg ervoor dat de status **uitgeschakeld.** 
 ![Gebruiker](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7.  Plaats een **controleren** in het vak naast de naam.
7.  Aan de rechterkant, klikt u op **inschakelen**.
![Gebruiker inschakelen](./media/multi-factor-authentication-get-started-cloud/user1.png)
8.  Klik op **meerledige verificatie inschakelen**.
![Gebruiker inschakelen](./media/multi-factor-authentication-get-started-cloud/enable2.png)
9.  U zou er de dat status van de gebruiker is gewijzigd van **uitgeschakeld** in **ingeschakeld**.
![Gebruikers in staat stellen](./media/multi-factor-authentication-get-started-cloud/user.png)
10.  Nadat u uw gebruikers hebt ingeschakeld, is het raadzaam deze via e-mail te melden.  Hij moet ook kennis ze hoe ze hun browser-apps kunnen gebruiken om te voorkomen dat het wordt vergrendeld.

### <a name="to-change-the-state-from-enabledenforced-to-disabled"></a>De status wijzigen van ingeschakeld/ingesteld op uitgeschakeld
1.  Aanmelden bij de **Azure klassieke portal** als beheerder.
2.  Aan de linkerkant, klikt u op **Active Directory**.
3.  Klik onder op de **map** op de map voor de gebruiker die u wilt inschakelen.
![Klik op map](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Aan de bovenkant, klikt u op **gebruikers**.
5.  Klik onderaan de pagina **Meerledige verificatie beheren**.
![Klik op map](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Hiermee opent u een nieuwe browser tab.  Zoek de gebruiker die u wilt uitschakelen. Wellicht moet u weer aan de bovenkant. Zorg ervoor dat de status op **ingeschakeld** of **afgedwongen.**
7.  Plaats een **controleren** in het vak naast de naam.
7.  Aan de rechterkant, klikt u op **uitschakelen**.
![Gebruiker uitschakelen](./media/multi-factor-authentication-get-started-user-states/userstate2.png)
8.  U wordt gevraagd dit te bevestigen.  Klik op **Ja**.
![Gebruiker uitschakelen](./media/multi-factor-authentication-get-started-user-states/userstate3.png)
9.  Vervolgens ziet u dat gelukt is.  Klik op **sluiten.** 
 ![Van gebruikers uitschakelen](./media/multi-factor-authentication-get-started-user-states/userstate4.png)
