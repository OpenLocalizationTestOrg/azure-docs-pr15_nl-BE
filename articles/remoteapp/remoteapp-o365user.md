
<properties 
    pageTitle="Azure RemoteApp gebruiken met Office 365 gebruikersaccounts | Microsoft Azure"
    description="Informatie over het gebruik van Azure RemoteApp met mijn Office 365-gebruikersaccounts"
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-to-use-azure-remoteapp-with-office-365-user-accounts"></a>Azure RemoteApp gebruiken met Office 365-gebruikersaccounts

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Als u een Office 365-abonnement hebt een Azure Active Directory waarin uw gebruikersnamen en wachtwoorden voor toegang tot Office 365-services opgeslagen. Bijvoorbeeld: wanneer gebruikers Office 365 ProPlus activeert ze worden geverifieerd bij Azure AD om licenties te controleren. De meeste klanten willen gebruiken dezelfde map met Azure RemoteApp.

Als u RemoteApp Azure implementeert u waarschijnlijk een Azure-abonnement dat is gekoppeld aan een andere Azure AD gebruikt. Wilt gebruiken in de Office 365-map, moet u het abonnement Azure verplaatsen naar die map.

Zie voor informatie over het implementeren van Office 365-clienttoepassingen, [hoe u uw abonnement op Office 365 met RemoteApp-Azure](remoteapp-officesubscription.md).
 
## <a name="phase-1-register-your-free-office-365-azure-active-directory-subscription"></a>Fase 1: Uw gratis abonnement op Office 365 Azure Active Directory registreren
Gebruik de stappen in [uw gratis abonnement op Azure Active Directory registreren](https://technet.microsoft.com/library/dn832618.aspx) als u de klassieke Azure portal voor administratieve toegang tot uw Azure advertentie via het Portal Azure Management. Als het resultaat van dit proces moet u zich aanmelden bij de Azure portal en Zie uw directory er – op dit moment ziet u niet meer omdat het volledige Azure abonnement dat u in combinatie met Azure RemoteApp in een andere map is.

Vergeet niet de naam en het wachtwoord van de administrator-account die u in deze stap hebt gemaakt: ze nodig hebben in fase 2.

Als u de portal Azure controleren hoe [registreren en activeren van een vrije Azure Active Directory met behulp van Office 365 portal](http://azureblogger.com/2016/01/how-to-register-and-activate-a-free-azure-active-directory-using-office-365-portal/).

## <a name="phase-2-change-the-azure-ad-associated-with-your-azure-subscription"></a>Fase 2: Wijzig de Azure advertentie die is gekoppeld aan uw abonnement op Azure.
We gaan uw abonnement te wijzigen Azure vanuit de huidige map naar de map Office 365 die we in fase 1 gewerkt.

Volg de instructies die worden beschreven in [de huurder Azure Active Directory in Azure RemoteApp wijzigen](remoteapp-changetenant.md). Speciale aandacht besteden aan de volgende stappen uit:

- Stap #1: Als u RemoteApp-Azure (ARA) in dit abonnement hebt geïmplementeerd, moet dat u alle gebruikersaccounts in AD Azure uit alle collecties ARA eerst verwijderen, voordat u iets anders. U kunt ook overwegen verwijderen van de bestaande collecties.
- Stap #2: Dit is een belangrijke stap. U moet een Microsoft-account gebruiken (bijv. @outlook.com) als servicebeheerder op het abonnement; dit is omdat we geen gebruikersaccounts van de bestaande Azure advertentie die is gekoppeld aan het abonnement – als we doen, is niet mogelijk om deze te verplaatsen naar een andere Azure AD.
- Stap #4: Wanneer u een bestaande map toevoegt, het systeem vraagt u zich aanmelden met de administrator-account voor die map. Controleer of u de administrator-account van fase 1.
- Stap #5: Wijzig de bovenliggende map van het abonnement in de map Office 365. Het eindresultaat moet dat onder Instellingen -> abonnementen uw abonnement geeft een overzicht van de map Office 365. 
![Wijzig de bovenliggende map van het abonnement](./media/remoteapp-o365user/settings.png)
 

Op dit moment is uw Azure RemoteApp-abonnement gekoppeld aan uw Office 365, Azure AD; met Azure RemoteApp kunt u de bestaande gebruikersaccounts voor Office 365!




