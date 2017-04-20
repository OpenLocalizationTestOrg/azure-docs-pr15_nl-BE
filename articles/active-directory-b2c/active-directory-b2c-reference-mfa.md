<properties
    pageTitle="Azure Active Directory B2C: Meerledige verificatie | Microsoft Azure"
    description="Meerledige verificatie inschakelen in consument gerichte toepassingen die zijn beveiligd met Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="msmbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-enable-multi-factor-authentication-in-your-consumer-facing-applications"></a>Azure Active Directory B2C: Meerledige verificatie inschakelen in uw consument gerichte toepassingen

Azure Active Directory (AD Azure) B2C volledig is geïntegreerd met [Azure meerledige verificatie](../multi-factor-authentication/multi-factor-authentication.md) zodat u een tweede laag van beveiliging aan aanmelden en inloggen in uw consument gerichte toepassingen toevoegen kunt. En u kunt dit doen zonder één regel code te schrijven. We ondersteunen momenteel telefoongesprek en tekst bericht verifiëren. Als u al beleid aanmelden en aanmelden hebt gemaakt, kunt u nog steeds meerledige verificatie inschakelen.

> [AZURE.NOTE]
Meerledige verificatie kan ook worden ingeschakeld bij het maken van aanmelding en -in beleid, niet alleen door het bestaande beleid te bewerken.

Deze functie helpt bij het verwerken van scenario's voor de volgende toepassingen:

- U meerledige verificatie toegang tot één toepassing niet vereist, maar dit nodig is voor toegang tot een andere. Bijvoorbeeld, de consument kunt inloggen op een auto verzekering toepassing met een sociale of lokale account, maar het telefoonnummer dat u moet controleren voordat toegang tot de home verzekering toepassing geregistreerd in dezelfde map.
- U meerledige verificatie toegang krijgen tot een toepassing in het algemeen niet nodig, maar u nodig hebt om de gevoelige delen in het. Bijvoorbeeld de consument zich aanmelden bij een bank-toepassing met een sociale of lokale account en het selectievakje saldo, maar het telefoonnummer dat u moet controleren voordat u probeert een overschrijving.

## <a name="modify-your-sign-up-policy-to-enable-multi-factor-authentication"></a>Meerledige verificatie inschakelen in uw aanmelding beleid wijzigen

1. [Volg deze stappen om te navigeren naar de B2C functies blade van de Azure portal](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klik op **aanmelden bij beleid**.
3. Klik op uw aanmelding beleid (bijvoorbeeld ' B2C_1_SiUp') om deze te openen.
4. Klik op **meerledige verificatie** en schakel de **staat** op **ON**. Klik op **OK**.
5. Klik op **Opslaan** op de bovenkant van het blad.

U kunt de functie 'Direct uitvoeren' in het beleid om te controleren of de ervaring van de consument. Controleer het volgende:

Een rekening van de consument wordt gemaakt in de map voordat de stap meerledige verificatie plaatsvindt. Tijdens de stap van de consument gevraagd naar zijn of haar telefoonnummer opgeven en bevestig dit. Als de controle is uitgevoerd, wordt het telefoonnummer gekoppeld aan de rekening van de consument voor later gebruik. Zelfs als de consument wordt geannuleerd of wegvalt, kan hij of zij worden gevraagd om te controleren of een telefoonnummer dat tijdens de volgende aanmelden (met meerledige verificatie is ingeschakeld).

## <a name="modify-your-sign-in-policy-to-enable-multi-factor-authentication"></a>Wijzigen van uw beleid aanmelden meerledige verificatie inschakelen

1. [Volg deze stappen om te navigeren naar de B2C functies blade van de Azure portal](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klik op **beleid voor aanmelden**.
3. Klik op de-in beleid (bijvoorbeeld ' B2C_1_SiIn') om deze te openen. Klik op **bewerken** op de bovenkant van het blad.
4. Klik op **meerledige verificatie** en schakel de **staat** op **ON**. Klik op **OK**.
5. Klik op **Opslaan** op de bovenkant van het blad.

U kunt de functie 'Direct uitvoeren' in het beleid om te controleren of de ervaring van de consument. Controleer het volgende:

Wanneer de consument zich aanmeldt (met een sociale of lokale account), als een gecontroleerde telefoonnummer is gekoppeld aan de account van de consument, hij of zij gevraagd te controleren. Als er geen telefoonnummer is gekoppeld, wordt de consument gevraagd te leveren en bevestig dit. Het telefoonnummer is voor een geslaagde verificatie gekoppeld aan de rekening van de consument voor later gebruik.

## <a name="multi-factor-authentication-on-other-policies"></a>Meerledige verificatie op ander beleid

Het is ook mogelijk om het meerledige verificatie inschakelen op aanmelding voor aanmelding & -in-beleid hierboven is beschreven, of -in beleid en het wachtwoord opnieuw instellen beleid. Het zal binnenkort op profiel bewerken beleid beschikbaar zijn.
