<properties
    pageTitle="# Automatische registratie voor Windows 7-domein gekoppeld apparaten configureren | Microsoft Azure"
    description="Stappen voor het configureren van uw domein Windows 7 verbonden apparaten automatisch registreren met Azure Active Directory. en de stappen voor het implementeren van het softwarepakket voor registratie van apparaat aan uw domein Windows 7 verbonden apparaten met behulp van een distributiestelsel software zoals System Center Configuration Manager."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="MarkVi"/>

# <a name="configure-automatic-device-registration-for-windows-7-domain-joined-devices"></a>Automatische registratie voor Windows 7-domein gekoppeld apparaten configureren

Als IT-beheer kunt u uw Windows 7-domein gekoppeld apparaten automatisch registreren met Azure Active Directory. Hiervoor moet u het apparaat registratie softwarepakket installeren op uw domein Windows 7 verbonden apparaten met behulp van een distributiestelsel software zoals System Center Configuration Manager. Zorg ervoor dat lees en voltooien van de vereisten die zijn vermeld in de automatische registratie van apparaat met Azure Active Directory voor Windows-domein is verbonden apparaten.

>[AZURE.NOTE]
 Nieuwste Zie voor instructies voor het instellen van automatische registratie, [instellen van automatische inschrijving van een Windows-domein verbonden apparaten met Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).

##<a name="installing-the-device-registration-software-package-on-windows-7-domain-joined-devices"></a>Het apparaat registratie software-pakket installeren op Windows 7 verbonden domein apparaten

De registratie voor Windows 7 is beschikbaar als een [downloadbaar MSI-pakket](https://connect.microsoft.com/site1164). Het pakket moet worden geïnstalleerd op Windows 7-computers die lid zijn van een Active Directory-domein. U kunt het pakket met behulp van een distributiestelsel software zoals System Center Configuration Manager moet implementeren. Het MSI-pakket ondersteunt de standaard stille installatie-opties met de schakelopties/quiet parameter.
Het softwarepakket is beschikbaar op de [website van Microsoft Connect](https://connect.microsoft.com/site1164). Hier kunt u selecteren en download werkplek Join voor Windows 7.

![](./media/active-directory-conditional-access/device-registration-process-windows7.gif)

## <a name="workplace-join-with-azure-active-directory"></a>Join werkplek met Azure Active Directory
Apparaatregistratie voor apparaten voor Windows 7-domein gekoppeld vereist of gebruikersinterface. Eenmaal geïnstalleerd op de computer, een domeingebruiker die zich bij de computer aanmeldt automatisch en ongemerkt geregistreerd met een apparaatobject in Azure AD. Zal er een apparaatobject in Azure AD voor elke geregistreerde gebruiker van het fysieke apparaat.

Het installatieprogramma maakt een geplande taak op het systeem dat wordt uitgevoerd in de context van de gebruiker en wordt geactiveerd bij aanmelden van gebruiker. De taak zonder waarschuwing registreert de gebruiker en apparaat met Azure Active Directory nadat tekenen in de gebruiker is voltooid.
De geplande taak kunt u vinden in de bibliotheek voor Taakplanner onder **Microsoft** > **Werkplek deelnemen**.
De taak wordt uitgevoerd en registreren van alle Active Directory-gebruikers die bij de computer aanmelden.
In de volgende afbeelding bevat een stapsgewijze procedure voor automatische registratie.

![](./media/active-directory-conditional-access/automatic-device-registration-windows7.png)

1. (Informatiewerker) gebruiker zich aanmeldt bij een Windows 7-clientcomputer met behulp van referenties voor Active Directory-domein.
1. De werkplek deelnemen aan geplande taak wordt uitgevoerd.
1. De gebruiker wordt zonder bericht geverifieerd met AD FS met Windows geïntegreerde verificatie.
1. PC met Windows 7 is voor de gebruiker geregistreerd in Azure AD.
1. Een certificaat en een apparaatobject is gemaakt in Azure AD. Het object vertegenwoordigt de user@device.
1. De werkplek Join-certificaat is opgeslagen op de computer.

## <a name="unregistering-your-windows-7-domain-joined-devices"></a>Afmelden van uw domein Windows 7 verbonden apparaten

U kunt de registratie van uw domein gekoppeld Windows 7 apparaten door het volgende te doen: de werkplek Join verwijderen softwarepakket uit uw domein Windows 7 verbonden apparaten met behulp van een distributiestelsel software zoals System Center Configuration Manager.

Vervolgens opent u een opdrachtprompt op de computer met Windows 7 en uitvoeren van de volgende opdracht om de registratie van het apparaat:

    %ProgramFiles%\Microsoft Workplace Join\AutoWorkplace.exe /leave

>[AZURE.NOTE]
>Deze opdracht moet worden uitgevoerd in de context van de domeingebruiker die is ondertekend in de machine.
Logboeken en fouten in Windows 7 domein verbonden apparaten.

Berichten die betrekking hebben op de werkplek, deelnemen aan weergegeven het gebeurtenislogboek van Windows op de computer met Windows 7. U kunt berichten vinden voor zowel geslaagde als mislukte gebeurtenissen in de werkplek deelnemen aan. Het gebeurtenislogboek kan worden gevonden in de gebeurtenis Viewer onder logboeken toepassingen en Services > Microsoft werkplek deelnemen.

## <a name="additional-topics"></a>Extra onderwerpen

- [Azure Active Directory de registratie-overzicht](active-directory-conditional-access-device-registration-overview.md)
- [Automatische registratie met Azure Active Directory for Windows Domain-Joined apparaten](active-directory-conditional-access-automatic-device-registration.md)
- [Automatische registratie voor Windows 8.1 domein gekoppeld apparaten configureren](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)
- [Automatische registratie met een domein behoren Azure Active Directory voor Windows 10-apparaten](active-directory-azureadjoin-devices-group-policy.md)
