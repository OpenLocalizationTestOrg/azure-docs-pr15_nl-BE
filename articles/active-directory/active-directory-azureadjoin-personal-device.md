

<properties
    pageTitle="Een persoonlijk apparaat toevoegen aan uw organisatie | Microsoft Azure"
    description="Wordt uitgelegd hoe gebruikers hun persoonlijke apparaten Windows 10 met hun bedrijfsnetwerk kunnen registreren en implementatiestappen biedt voor een BYOD-scenario."
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
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="join-a-personal-device-to-your-organization"></a>Een persoonlijk apparaat toevoegen aan uw organisatie

## <a name="to-join-a-windows-10-device-to-your-organization"></a>Deelnemen aan een Windows 10-apparaat aan uw organisatie

1.  Selecteer **Instellingen**in het menu **Start** .
2.  Selecteer **Accounts**en klik op **uw account**.
3.  Klik op **toevoegen werk of School account**en typt u vervolgens in uw organisatie-account.
4.  Voer uw gebruikersnaam en wachtwoord op de pagina aanmelden voor uw organisatie en klik vervolgens op **OK**.
5.  U wordt gevraagd een meerledige verificatie uitdaging. (Deze vraag kan worden geconfigureerd door een IT-beheerder.)
6.  Azure Active Directory (AD Azure) wordt gecontroleerd of het apparaat mobiel apparaat beheer inschrijving vereist.
7.  Windows het apparaat in de map van de organisatie geregistreerd in Azure AD en zich inschrijft in een mobiel Apparaatbeheer, indien van toepassing.
8.  Als u een gebruiker beheerde, gaat Windows u naar het bureaublad via het automatisch aanmelden.
9.  Als u een federatieve gebruiker, wordt u naar een Windows-aanmeldingsscherm genomen uw referenties moet invoeren.

## <a name="additional-information"></a>Als u meer informatie
* [Windows 10 voor de onderneming: apparaten gebruik voor werk](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud-mogelijkheden aan Windows 10-apparaten via Azure Active Directory deelnemen aan uitbreiden](active-directory-azureadjoin-user-upgrade.md)
* [Verificatie van identiteiten zonder wachtwoorden via Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Meer informatie over gebruiksscenario's voor Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Domein behoren apparaten aansluiten op Azure AD voor Windows 10 ervaringen](active-directory-azureadjoin-devices-group-policy.md)
* [Azure AD Join instellen](active-directory-azureadjoin-setup.md)
