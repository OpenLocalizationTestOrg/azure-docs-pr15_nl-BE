<properties
    pageTitle="Get Azure MVR gesloten gestart in de cloud | Microsoft Azure"
    description="Dit is de pagina Microsoft Azure meerledige verificatie die wordt beschreven hoe u aan de slag met Azure MVR gesloten in de cloud."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/17/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-azure-multi-factor-authentication-in-the-cloud"></a>Aan de slag met Azure meerledige verificatie in de cloud
Dit artikel vindt u hoe u aan de slag met Azure meerledige verificatie in de cloud.

> [AZURE.NOTE]  De volgende documentatie bevat informatie over het inschakelen van de **Klassieke Azure-Portal**-gebruikers. Als u op zoek bent voor informatie over het instellen van Azure meerledige verificatie voor gebruikers van O365, Zie [instellen voor Office 365-meerledige verificatie.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)

![MVR gesloten in de Cloud](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## <a name="prerequisites"></a>Vereisten
De volgende vereisten zijn vereist voordat u Azure meerledige verificatie voor uw gebruikers inschakelen kunt.


1. [Meld u aan voor een abonnement Azure](https://azure.microsoft.com/pricing/free-trial/) - als u nog geen abonnement op een Azure, moet u aanmelden voor een. Als u alleen uitgaan en met behulp van de MVR gesloten Azure kunt u een proefabonnement
2. [Een meerledige verificatie-Provider maken](multi-factor-authentication-get-started-auth-provider.md) en toewijzen aan de map of [licenties aan gebruikers toewijzen](multi-factor-authentication-get-started-assign-licenses.md)

> [AZURE.NOTE]  Licenties zijn beschikbaar voor gebruikers met Azure MVR gesloten, Azure AD Premium of Enterprise Mobility Suite (EMS).  MVR gesloten is in Azure AD Premium en de EMS opgenomen. Als u voldoende licenties hebt, hoeft u niet om een Auth-Provider te maken.


## <a name="turn-on-two-step-verification-for-users"></a>Schakel verificatie voor gebruikers moet u twee stappen
Starten twee start verificatie vereisen op voor een gebruiker, de status van de gebruiker te wijzigen van uitgeschakeld in ingeschakeld.  Zie voor meer informatie over de lidstaten van de gebruiker, [Gebruiker Staten in Azure meerledige verificatie](multi-factor-authentication-get-started-user-states.md)

Gebruik de volgende procedure MVR gesloten inschakelen voor uw gebruikers.

### <a name="to-turn-on-multi-factor-authentication"></a>Meerledige verificatie inschakelen

1.  Aanmelden bij de [Azure klassieke portal](https://manage.windowsazure.com) als beheerder.
2.  Aan de linkerkant, klikt u op **Active Directory**.
3.  Selecteer onder map, de map voor de gebruiker die u wilt inschakelen.
![Klik op map](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Aan de bovenkant, klikt u op **gebruikers**.
5.  Klik onderaan de pagina **Meerledige verificatie beheren**. Hiermee opent u een nieuwe browser tab.
![Klik op map](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Zoek de gebruiker die u wilt inschakelen voor verificatie in twee stappen. Wellicht moet u weer aan de bovenkant. Zorg ervoor dat de status **uitgeschakeld.** 
 ![Gebruiker](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7.  Plaats een **controleren** in het vak naast de naam.
7.  Aan de rechterkant, klikt u op **inschakelen**.
![Gebruiker inschakelen](./media/multi-factor-authentication-get-started-cloud/user1.png)
8.  Klik op **meerledige verificatie inschakelen**.
![Gebruiker inschakelen](./media/multi-factor-authentication-get-started-cloud/enable2.png)
9.  U ziet dat de status van de gebruiker is gewijzigd van **uitgeschakeld** in **ingeschakeld**.
![Gebruikers in staat stellen](./media/multi-factor-authentication-get-started-cloud/user.png)

Nadat u uw gebruikers hebt ingeschakeld, moet u deze per e-mail melden. De volgende keer dat ze proberen aan te melden, wordt ze gevraagd om in te schrijven hun account voor verificatie in twee stappen. Zodra ze met controle van de uit twee stappen bestaat beginnen, moet zij ook app wachtwoorden instellen om te voorkomen dat de browser-apps die is vergrendeld.


## <a name="use-powershell-to-automate-turning-on-two-step-verification"></a>PowerShell gebruiken voor het inschakelen van verificatie in twee stappen automatiseren

Als u wilt wijzigen in de [staat](multi-factor-authentication-whats-next.md) met [Azure AD PowerShell](../powershell-install-configure.md), kunt u het volgende.  Kunt u `$st.State` gelijk is aan een van de volgende statussen:

- Ingeschakeld
- Afgedwongen
- Uitgeschakeld  

> [AZURE.IMPORTANT]  We ontmoedigen tegen gebruikers die rechtstreeks van de staat uitschakelen aan de staat afgedwongen. Niet-browser gebaseerde toepassingen werkt niet omdat de gebruiker heeft geen MVR gesloten registratie doorlopen en een [app wachtwoord](multi-factor-authentication-whats-next.md#app-passwords)verkregen. Als uw browser gebaseerde apps en app wachtwoorden, raden we aan om te gaan van een uitgeschakelde status op ingeschakeld. Hierdoor kunnen gebruikers te registreren en hun wachtwoorden app downloaden. Daarna kunt u ze verplaatsen naar afgedwongen.

Met PowerShell is een optie voor gebruikers bulksgewijs. Momenteel kan niet bulksgewijs inschakelen in de portal Azure en moet u elke gebruiker afzonderlijk selecteren. Dit kan vrij een taak zijn als er veel gebruikers. Door het maken van een PowerShell script met behulp van de volgende, u kunt doorlopen in een lijst met gebruikers en hen in staat.

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "\*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Hier volgt een voorbeeld:

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "\*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }


Zie voor meer informatie [gebruiker-Staten in Azure meerledige verificatie](multi-factor-authentication-get-started-user-states.md)

## <a name="next-steps"></a>Volgende stappen
Nu dat u in de cloud een Azure meerledige verificatie hebt ingesteld, kunt u configureren en instellen van uw implementatie. Zie [Azure meerledige verificatie configureren](multi-factor-authentication-whats-next.md) voor meer informatie.
