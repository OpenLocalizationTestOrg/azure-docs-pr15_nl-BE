<properties
    pageTitle="Wat zijn de wachtwoorden in Azure MVR gesloten App?"
    description="Deze pagina helpt gebruikers begrijpen wat app wachtwoorden zijn en waarvoor ze worden gebruikt met inachtneming van Azure MVR gesloten."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>



# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a>Wat zijn de wachtwoorden App in Azure meerledige verificatie?

Bepaalde browser-apps, zoals de Apple native e-client die gebruikmaakt van Exchange ActiveSync, ondersteuning momenteel geen voor meerledige verificatie. Meerledige verificatie is per gebruiker ingeschakeld. Dit betekent dat als een gebruiker is ingeschakeld voor een meerledige verificatie en ze probeert te gebruiken browser-apps, worden deze niet kan doen. Een app wachtwoord kan dit optreden.

>[AZURE.NOTE] Moderne verificatie voor de Clients Office 2013
>
> Office 2013-clients (inclusief Outlook) nu nieuwe verificatieprotocollen ondersteunen en ondersteuning meerledige verificatie kunnen worden ingeschakeld.  Dit betekent dat eenmaal is ingeschakeld, app wachtwoorden zijn niet vereist voor gebruik met Office 2013-clients.  Zie [Office 2013 moderne verificatie public preview aangekondigd](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)voor meer informatie.

## <a name="how-to-use-app-passwords"></a>Het gebruik van app-wachtwoorden

Hier volgen enkele punten om te onthouden over het gebruik van app-wachtwoorden.

- Het wachtwoord zelf wordt automatisch gegenereerd en niet door de gebruiker worden verstrekt. Dit is omdat de automatisch gegenereerde wachtwoord moeilijker door een aanvaller te raden en is veiliger.
- Er is momenteel een maximum van 40 wachtwoorden per gebruiker. Als u probeert een te maken nadat u hebt de limiet bereikt, wordt u gevraagd een van uw bestaande app wachtwoorden verwijderen om een nieuwe maken.
- Het wordt aanbevolen dat app wachtwoorden per apparaat en per toepassing niet worden gemaakt. U kunt bijvoorbeeld een app wachtwoord maken voor uw laptop en dat app-wachtwoord gebruikt voor alle toepassingen op die laptop.
- Krijgt u een wachtwoord voor de app de eerste keer dat u aanmelden.  Als u extra bestanden nodig hebt, kunt u deze maken.

![Setup](./media/multi-factor-authentication-end-user-app-passwords/app.png)

Als u een app wachtwoord hebt, kunt u dit gebruiken in plaats van het oorspronkelijke wachtwoord met deze browser-apps.  Dus bijvoorbeeld, als u van meerledige verificatie en de Apple standaard e-mailclient op uw telefoon gebruikmaakt.  Het app-wachtwoord gebruiken zodat deze kan meerledige verificatie overslaan en doorgaan met werken.

## <a name="creating-and-deleting-app-passwords"></a>Maken en verwijderen van wachtwoorden app
Tijdens uw eerste aanmelden krijgt u een wachtwoord app die u kunt gebruiken.  U kunt bovendien ook maken en later op app wachtwoorden verwijderen.  Hoe u dit doet, is afhankelijk van hoe u een meerledige verificatie gebruiken.  Kies degene die de meeste op u van toepassing.

Het gebruik van meerdere factoren, authentiation|Beschrijving
:------------- | :------------- |
[Ik gebruik in combinatie met Office 365](#creating-and-deleting-app-passwords-with-office-365)|  Dit betekent dat u wilt maken van wachtwoorden via de Office 365 portal app.
[Ik weet het niet](#creating-and-deleting-app-passwords-with-myapps-portal)|Hierdoor dat is het verstandig wachtwoorden app via [https://myapps.microsoft.com](https://myapps.microsoft.com)
[Ik gebruik in combinatie met Microsoft Azure](#create-app-passwords-in-the-azure-portal)| Betekent dit dat zal u wilt wachtwoorden via het portal Azure app maken.

## <a name="creating-and-deleting-app-passwords-with-office-365"></a>Maken en verwijderen van wachtwoorden met Office 365 app

Als u met Office 365 u wilt maken en verwijderen van de app wachtwoord via de Office 365 portal meerledige verificatie.

### <a name="to-create-app-passwords-in-the-office-365-portal"></a>App wachtwoorden maken in de Office 365 portal
--------------------------------------------------------------------------------

1. Meld u op de [Office 365 portal](https://login.microsoftonline.com/).
2. Selecteer het object en kiest u Office 365-instellingen in de rechterbovenhoek.
3. Klik op verificatie voor extra beveiliging.
4. Klik op de koppeling waarin wordt gemeld aan de rechterkant, **bijwerken Mijn telefoonnummers gebruikt voor accountbeveiliging.** 
 ![Setup](./media/multi-factor-authentication-end-user-manage/o365a.png)
5. Hiervoor gaat u naar de pagina waarmee u de instellingen te wijzigen.
![Wolk](./media/multi-factor-authentication-end-user-manage/o365b.png)
6. Boven, naast extra beveiliging verificatie, klikt u op **wachtwoorden app.**
7. Klik op **maken**.
![Wolk](./media/multi-factor-authentication-end-user-app-passwords-create-o365/apppass.png)
8. Voer een naam voor het app-wachtwoord en klik op **volgende**.
![Een app wachtwoord maken](./media/multi-factor-authentication-end-user-app-passwords/create1.png)
9. Het app-wachtwoord naar het Klembord kopiëren en plakken in uw app.
![Een app wachtwoord maken](./media/multi-factor-authentication-end-user-app-passwords/create2.png)


### <a name="to-delete-app-passwords-using-the-office-365-portal"></a>App wachtwoord via de Office 365 portal verwijderen
--------------------------------------------------------------------------------


1. Meld u op de [Office 365 portal](https://login.microsoftonline.com/).
2. Selecteer het object en kiest u Office 365-instellingen in de rechterbovenhoek.
3. Klik op verificatie voor extra beveiliging.
4. Klik op de koppeling waarin wordt gemeld aan de rechterkant, **bijwerken Mijn telefoonnummers gebruikt voor accountbeveiliging.** 
 ![Setup](./media/multi-factor-authentication-end-user-manage/o365a.png)
5. Hiervoor gaat u naar de pagina waarmee u de instellingen te wijzigen.
![Wolk](./media/multi-factor-authentication-end-user-manage/o365b.png)
6. Boven, naast extra beveiliging verificatie, klikt u op **wachtwoorden app.**
7. Naast het app-wachtwoord dat u wilt verwijderen, klikt u op **verwijderen**.
![Een app wachtwoord verwijderen](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)
8. Bevestig de verwijdering door te klikken op **Ja**.
![Bevestig het verwijderen](./media/multi-factor-authentication-end-user-app-passwords/delete2.png)
9. Nadat u het wachtwoord voor de app verwijderd Klik u op **sluiten**.
![Sluiten](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)


## <a name="creating-and-deleting-app-passwords-with-myapps-portal"></a>Maken en wachtwoorden met Myapps portal app verwijderen.
Als u niet zeker weet hoe u een meerledige verificatie gebruiken, kunt klik u altijd maken en verwijderen app wachtwoord via de portal myapps.

### <a name="to-create-an-app-password-using-the-myapps-portal"></a>Voor het maken van een app wachtwoord via de portal Myapps

1. Aanmelden bij [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Boven het profiel te selecteren.
3. Schakel verificatie voor extra beveiliging.
![Wolk](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. Hiervoor gaat u naar de pagina waarmee u de instellingen te wijzigen.
![Setup](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)
5. Boven, naast extra beveiliging verificatie, klikt u op **wachtwoorden app.**
6. Klik op **maken**.
![Een app wachtwoord maken](./media/multi-factor-authentication-end-user-app-passwords/create3.png)
7. Voer een naam voor het app-wachtwoord en klik op **volgende**.
![Een app wachtwoord maken](./media/multi-factor-authentication-end-user-app-passwords/create1.png)
8. Het app-wachtwoord naar het Klembord kopiëren en plakken in uw app.
![Een app wachtwoord maken](./media/multi-factor-authentication-end-user-app-passwords/create2.png)

### <a name="to-delete-an-app-password-using-the-myapps-portal"></a>Een app wachtwoord via de portal Myapps verwijderen

1. Aanmelden bij [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Boven het profiel te selecteren.
3. Schakel verificatie voor extra beveiliging.
![Wolk](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. Hiervoor gaat u naar de pagina waarmee u de instellingen te wijzigen.
![Setup](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)
5. Boven, naast extra beveiliging verificatie, klikt u op **wachtwoorden app.**
6. Naast het app-wachtwoord dat u wilt verwijderen, klikt u op **verwijderen**.
![Een app wachtwoord verwijderen](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)
7. Bevestig de verwijdering door te klikken op **Ja**.
![Bevestig het verwijderen](./media/multi-factor-authentication-end-user-app-passwords/delete2.png)
8. Nadat u het wachtwoord voor de app verwijderd Klik u op **sluiten**.
![Sluiten](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)


## <a name="create-app-passwords-in-the-azure-portal"></a>App wachtwoorden maken in de portal voor Azure

Als u meerledige verificatie mogelijk met Azure is het verstandig wachtwoorden via het portal Azure app te maken.

### <a name="to-create-app-passwords-in-the-azure-portal"></a>App wachtwoorden maken in de portal voor Azure

1. Aanmelden op de portal Management Azure.
2. Boven, met de rechtermuisknop op de naam van de gebruiker en selecteert u aanvullende beveiliging-verificatie.
3. Selecteer op de pagina proofup boven de app wachtwoorden
4. Klik op **maken**.
5. Voer een naam voor het app-wachtwoord en klik op **volgende**
6. Het app-wachtwoord naar het Klembord kopiëren en plakken in uw app.
![Wolk](./media/multi-factor-authentication-end-user-app-passwords-create-azure/app2.png)

### <a name="to-delete-app-passwords-in-the-azure-portal"></a>App wachtwoorden in Azure portal verwijderen

1. Aanmelden op de portal Management Azure.
2. Boven, met de rechtermuisknop op de naam van de gebruiker en selecteert u aanvullende beveiliging-verificatie.
3. Boven, naast extra beveiliging verificatie, klikt u op **wachtwoorden app.**
4. Naast het app-wachtwoord dat u wilt verwijderen, klikt u op **verwijderen**.
5. Bevestig de verwijdering door te klikken op **Ja**.
6. Nadat u het wachtwoord voor de app verwijderd Klik u op **sluiten**.
![Sluiten](./media/multi-factor-authentication-end-user-app-passwords/delete3.png)
