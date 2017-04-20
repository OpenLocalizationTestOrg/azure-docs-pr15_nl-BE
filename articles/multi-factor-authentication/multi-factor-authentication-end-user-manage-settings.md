<properties
    pageTitle="De uit twee stappen bestaande controle-instellingen beheren | Microsoft Azure"
    description="Bepalen hoe u Azure meerledige verificatie mogelijk met inbegrip van uw contactgegevens wijzigen of het configureren van uw apparaten gebruiken."
    services="multi-factor-authentication"
    keywords = "multifactor-verificatie-client verificatieprobleem, correlatie-ID"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="manage-your-settings-for-two-step-verification"></a>Beheren van uw instellingen voor verificatie in twee stappen

Dit artikel bevat antwoorden op vragen over het bijwerken van de instellingen voor verificatie of meerledige verificatie van twee stappen. Als er problemen met het aanmelden bij uw account, raadpleegt u [problemen met verificatie in twee stappen](multi-factor-authentication-end-user-troubleshoot.md) voor probleemoplossing.


## <a name="where-to-find-the-settings-page"></a>Waar vind ik de pagina-instellingen
Afhankelijk van hoe uw bedrijf Azure meerledige verificatie instellen, zijn er enkele plaatsen waar u uw instellingen zoals uw telefoonnummer wijzigen kunt.

Als uw IT-beheerder wordt verzonden om een specifieke URL of de stappen voor het beheren van verificatie in twee stappen, volgt u deze instructies. De volgende instructies moeten anders werken voor iedereen anders. Als u deze stappen maar dezelfde opties niet ziet, betekent dit dat uw werk of op school een eigen portal wordt aangepast. Vraag uw beheerder voor de koppeling naar uw portal Azure meerledige verificatie.


1. Aanmelden bij [https://myapps.microsoft.com](https://myapps.microsoft.com)  
2. Boven het **profiel**te selecteren.  
3. Schakel **verificatie voor extra beveiliging**.  

    ![Myapps](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. De pagina Additional security-verificatie wordt geladen met de instellingen.

    ![Proofup](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)


## <a name="i-want-to-change-my-phone-number-or-add-a-secondary-number"></a>Ik wil mijn telefoonnummer wijzigen of een tweede nummer toevoegen

Het is belangrijk dat u een telefoonnummer van de tweede verificatie configureert.  Omdat uw telefoonnummer en uw mobiele app waarschijnlijk op de dezelfde telefoon, is het tweede telefoonnummer dat de enige manier is het mogelijk om terug te gaan naar uw rekening als je telefoon verloren of gestolen is.

> [AZURE.NOTE]
> Als u geen toegang tot uw primaire telefoonnummer hebt en hulp opvragen in op uw account, raadpleegt u onze help-onderwerpen in [problemen met verificatie in twee stappen](multi-factor-authentication-end-user-troubleshoot.md).

**Het primaire telefoonnummer wijzigen:**  

1. Selecteer het tekstvak met uw huidige telefoonnummer en bewerken met uw nieuwe telefoonnummer op de pagina Additional security-verificatie.  
2. Selecteer **Opslaan**.  
3. Als dit het nummer dat u voor uw gewenste verificatieoptie is gebruikt, hebt u het nieuwe nummer te controleren voordat u deze kunt opslaan.  


**Een tweede telefoonnummer toevoegen:**  

1. Op de pagina Additional security controle, schakel het selectievakje in naast **Telefoon alternatieve verificatie.**  
2. Het tweede telefoonnummer invoeren in het tekstvak.  
3. Selecteer **Opslaan** en de wijzigingen zijn voltooid.  


## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-to-a-new-one"></a>Hoe Microsoft Authenticator opschonen van mijn oude apparaat en ik verplaatsen naar een nieuwe?
Wanneer u de app van het apparaat verwijderen of het apparaat opnieuw ingesteld, wordt de activering op de achtergrond niet verwijderd. Moet u de stappen in [verplaatsen naar een nieuw apparaat](multi-factor-authentication-microsoft-authenticator.md#how-to-move-to-the-new-microsoft-authenticator-app).

## <a name="next-steps"></a>Volgende stappen
- Krijg tips voor probleemoplossing- en Help-informatie over [problemen met verificatie in twee stappen](multi-factor-authentication-end-user-troubleshoot.md)
- [App wachtwoorden](multi-factor-authentication-end-user-app-passwords.md) voor alle toepassingen die geen ondersteuning voor verificatie in twee stappen bieden instellen.
