<properties
    pageTitle="Automatische registratie voor Windows 8.1 domein gekoppeld apparaten configureren | Microsoft Azure"
    description=" Stappen voor het configureren van Groepsbeleid voor Windows 8.1 apparaten domein behoren automatisch registreren met Azure Active Directory. "
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
    ms.author="Markvi"/>

# <a name="configure-automatic-device-registration-for-windows-81-domain-joined-devices"></a>Automatische registratie voor Windows 8.1 domein gekoppeld apparaten configureren

U kunt Active Directory-groepsbeleid Windows 8.1 automatisch registreren met Azure Active Directory-domein gekoppeld apparaten configureren. Het om Groepsbeleid te configureren, moet u ten minste één domein gekoppelde Windows Server 2012 R2 of Windows 8.1 computer hebben met de functie Groepsbeleid beheren is geïnstalleerd. Als dit groepsbeleid is ingeschakeld voor uw domein, wordt een domeingebruiker die zich bij de computer aanmeldt automatisch en ongemerkt geregistreerd met een apparaatobject in Azure AD. Zal er een apparaatobject in Azure AD voor elke geregistreerde gebruiker van het fysieke apparaat. Zorg ervoor dat lees en voltooi de vereisten van de automatische registratie van apparaat met Azure Active Directory for Windows Domain-Joined-apparaten.

>[AZURE.NOTE]
 Nieuwste Zie voor instructies voor het instellen van automatische registratie, [instellen van automatische inschrijving van een Windows-domein verbonden apparaten met Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).



## <a name="configure-the-group-policy-for-your-windows-81-domain-joined-devices"></a>Het groepsbeleid voor uw domein gekoppeld Windows 8.1 apparaten configureren

1. Open Serverbeheer en Ga naar **Extra** > **Voor Groepsbeleidsbeheer**.
2. Ga naar het knooppunt dat overeenkomt met het domein waarin u wilt **Deelnemen aan automatische werkplek**inschakelen van Group Policy Management.
3. Klik met de rechtermuisknop op **Groepsbeleidsobjecten** en selecteer **Nieuw**. Geef uw groepsbeleidsobject een naam, bijvoorbeeld **Deelnemen aan automatische werkplek**. Klik op **OK**.
4. Klik met de rechtermuisknop op het nieuwe groepsbeleidsobject en selecteer vervolgens **bewerken**.
5. Ga naar **Computerconfiguratie** > **beleid** > **Beheersjablonen** > **Windows-onderdelen** > **Werkplek deelnemen**.
6. Klik met de rechtermuisknop automatisch werkplek join clientcomputers en selecteer vervolgens **bewerken**.
7. Selecteer het keuzerondje ingeschakeld en klik vervolgens op toepassen. Klik op **OK**.
8. U kunt het groepsbeleidsobject nu koppelingen naar een locatie van uw keuze. Koppel het groepsbeleid aan het domein zodat dit beleid voor alle Windows 8.1 apparaten domein gekoppeld aan uw organisatie.

## <a name="unregistering-your-windows-81-domain-joined-devices"></a>Afmelden van uw domein Windows 8.1 verbonden apparaten

U kunt de registratie van uw domein gekoppeld Windows 8.1-apparaten op de volgende manier: aanpassen van de werkplek Join groepsbeleidsinstellingen in de vorige sectie hebt gemaakt. Stel de automatisch werkplek join client computerbeleid op uitgeschakeld. Hierdoor kunnen nieuwe apparaten automatisch lid worden van de werkplek.

De registratie van het bestaande domein gekoppeld Windows 8.1-machines door volgende een van de volgende opties:

* Optie 1: **domein registratie een Windows 8.1 verbonden apparaat via PC-instellingen**
  1. Ga naar **PC-instellingen**op het apparaat met Windows 8.1 > **netwerk** > **Werkplek**
  2. Selecteer **laat**.
Dit proces moet worden herhaald voor elk domeingebruiker die is aangemeld bij de computer en automatisch werkplek verbonden is geweest.

* Optie 2: De registratie van een Windows 8.1 domein gekoppelde apparaat met behulp van een script
    1. Open een opdrachtprompt op de computer Windows 8.1 en voer de volgende opdracht uit:` %SystemRoot%\System32\AutoWorkplace.exe leave`
   
Deze opdracht moet worden uitgevoerd in de context van de domeingebruiker die is ondertekend in de machine.

##<a name="event-viewer--errors-for-windows-81-domain-joined-devices"></a>Logboeken en fouten in Windows 8.1 domein gekoppeld apparaten

Het Windows-gebeurtenislogboek op een computer met Windows 8.1 geeft berichten die betrekking hebben op de registratie. Berichten vindt u zowel geslaagde als mislukte gebeurtenissen. 

Het gebeurtenislogboek kan worden gevonden in Logboeken onder toepassingen en Services **Logs** > **Microsoft** > **Windows > werkplek deelnemen aan**.

##<a name="additional-details"></a>Aanvullende details

Het groepsbeleid kunt een geplande taak op het systeem dat wordt uitgevoerd in de context van de gebruiker en wordt geactiveerd bij aanmelden van gebruiker. De taak registreert zonder waarschuwing de gebruiker en het apparaat met Azure Active Directory na het aanmelden. De geplande taak kunt u vinden op de apparaten in de bibliotheek voor Taakplanner onder **Microsoft**Windows 8.1 > **Windows** > **Werkplek deelnemen**. De taak wordt uitgevoerd en alle Active Directory-gebruikers te registreren dat teken in. 

## <a name="additional-topics"></a>Extra onderwerpen
- [Azure Active Directory de registratie-overzicht](active-directory-conditional-access-device-registration-overview.md)
- [Automatische registratie met een domein behoren Azure Active Directory voor Windows 10-apparaten](active-directory-conditional-access-automatic-device-registration.md)
- [Automatische registratie voor Windows 7-domein gekoppeld apparaten configureren](active-directory-conditional-access-automatic-device-registration-windows7.md)

