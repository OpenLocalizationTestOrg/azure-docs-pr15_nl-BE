<properties
    pageTitle="Azure Active Directory B2C: Self-service wachtwoord | Microsoft Azure"
    description="Een onderwerp aan te tonen het instellen van de self-wachtwoord opnieuw instellen voor de consumenten in Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="curtand"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>


# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a>Azure Active Directory B2C: Self-wachtwoord opnieuw instellen voor uw gebruikers instellen

De consumenten (die zich hebben aangemeld voor lokale accounts) kunnen hun wachtwoord op hun eigen opnieuw instellen met de functie voor het opnieuw instellen van wachtwoord zelf. Dit vermindert de belasting van uw medewerkers, vooral als uw toepassing miljoenen consumenten regelmatig gebruikt. Op dit moment alleen ondersteund met een geverifieerd e-mailadres als een methode voor het herstellen. We zullen meer herstelmethoden (gecontroleerde telefoonnummer, vragen over beveiliging, enz.) toevoegen in de toekomst.

> [AZURE.NOTE]
Dit artikel is van toepassing op de self-wachtwoord opnieuw instellen gebruikt in de context van een beleid voor aanmelden. Als u volledig aanpasbare beleid aangeroepen vanuit uw app opnieuw instellen van wachtwoord, raadpleegt u [dit artikel](./active-directory-b2c-reference-policies.md#create-a-password-reset-policy).

Standaard de map geen zelf-wachtwoord opnieuw instellen wordt ingeschakeld. Gebruik de volgende stappen uit te schakelen:

1. Aanmelden bij de [Azure klassieke portal](https://manage.windowsazure.com/) als abonnement beheerder. Dit is het hetzelfde werk of school account of het Microsoft-account die u gebruikt om de map te maken.
2. Ga naar de Active Directory-extensie op de navigatiebalk aan de linkerkant.
3. Zoeken naar uw map onder het tabblad **map** en klik erop.
4. Klik op het tabblad **configureren** .
5. Ga naar de sectie **beleid opnieuw instellen van wachtwoord van de gebruiker** en de optie **gebruikers ingeschakeld voor het wachtwoord opnieuw instellen** op **Ja**in-of uitschakelen. U ziet dat de optie **Alternatieve e-mailadres** wordt gecontroleerd; het laten zoals het is.

    ![Self-wachtwoord opnieuw instellen](./media/active-directory-b2c-reference-sspr/sspr.png)

6. Klik op **Opslaan** onder aan de pagina. U bent nu klaar!

Als u wilt testen, gebruikt u de functie 'Direct uitvoeren' op elk beleid aanmelden met lokale accounts als een id-provider. Op de lokale account aanmelden pagina (waar u een e-mailadres en wachtwoord, of een gebruikersnaam en wachtwoord), klikt u op **heeft geen toegang tot uw account?** om te controleren of de ervaring van de consument.

> [AZURE.NOTE]
Pagina's opnieuw instellen wachtwoord zelf kunnen worden aangepast met de [huisstijl functie bedrijf](../active-directory/active-directory-add-company-branding.md).
