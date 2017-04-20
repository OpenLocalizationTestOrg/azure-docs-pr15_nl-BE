<properties
    pageTitle="MDM beleid en de instellingen van de module | Microsoft Azure"
    description="Geeft informatie over Groepsbeleid en het mobiele apparaat (MDM) instellingen die moeten worden gebruikt op bedrijven die eigendom zijn van apparaten. Deze beleidsregels worden toegepast op het hele apparaat van de gebruiker."
    services="active-directory"
    keywords="Wat zijn groep beleid en instellingen voor Enterprise staat Roaming, onderneming staat Roaming, windows cloud MDM"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="group-policy-and-mdm-settings"></a>Instellingen voor Groepsbeleid en MDM

Deze Groepsbeleid en instellingen voor mobile device management (MDM) alleen gebruiken voor bedrijven die eigendom zijn van apparaten omdat deze beleidsregels worden toegepast op het hele apparaat van de gebruiker. Een synchronisatie voor een persoonlijke instellingen uitschakelen MDM beleid toe te passen, zullen apparaat-eig nadelig beïnvloeden het gebruik van het apparaat. Andere gebruikersaccounts op het apparaat wordt daarnaast ook beïnvloed door het beleid.

Ondernemingen die u wilt beheren met roaming voor persoonlijke (zonder begeleiding) apparaten via de Azure portal kunnen in- of uitschakelen van roaming in plaats van met behulp van Groepsbeleid of MDM.
De volgende tabellen worden de beschikbare beleidsinstellingen beschreven.

## <a name="mdm-settings"></a>MDM-instellingen
De MDM-beleidsinstellingen zijn van toepassing op zowel Windows 10 en Windows 10 Mobile.  Windows 10 Mobile ondersteuning bestaat alleen voor Microsoft-account op basis van roaming via OneDrive-account van de gebruiker.  Raadpleeg de sectie 'Apparaten en eindpunten' voor meer informatie over welke apparaten worden ondersteund voor het synchroniseren van Azure AD gebaseerd.

| Naam                               | Beschrijving                                                          |
|------------------------------------|----------------------------------------------------------------------|
| Microsoft-Account verbinding toestaan | Hiermee kunnen gebruikers verifiëren met behulp van een Microsoft-account op het apparaat |
| Sync Mijn instellingen toestaan             | Gebruikers in staat Windows-instellingen en gegevens van app; Als u dit beleid uitschakelt, sync als back-ups op mobiele apparaten worden uitgeschakeld                  |

## <a name="group-policy-settings"></a>Instellingen voor Groepsbeleid
De instellingen voor Groepsbeleid van toepassing op Windows 10-apparaten die zijn gekoppeld aan een Active Directory-domein. De tabel bevat ook oudere instellingen die voor het beheren van synchronisatie-instellingen zou lijken, maar die niet werken voor de onderneming staat Roaming voor Windows 10, die worden aangegeven met 'Niet gebruiken' in de beschrijving.

| Naam                                | Beschrijving |
|-------------------------------------|-------------|
| Accounts: Blok Microsoft-Accounts  |Dit beleid voorkomt dat gebruikers van het toevoegen van nieuwe Microsoft-accounts op deze computer|
| Synchroniseer niet                         |Hiermee voorkomt u dat gebruikers zwerven app gegevens en instellingen van Windows|
| Synchroniseer niet aanpassen             |Schakelt synchronisatie van de groep thema 's|
| Synchroniseer de browserinstellingen niet        |Schakelt synchronisatie van de groep van Internet Explorer|
| Synchroniseer geen wachtwoorden               |Schakelt synchronisatie van wachtwoorden groep|
| Synchroniseer niet andere Windows-instellingen  |Schakelt synchronisatie van de groep Overige Windows-instellingen|
| Synchroniseer niet personalisering bureaublad |Niet gebruiken; heeft geen effect|
| Synchroniseer niet gemeten verbindingen  |Schakelt op roaming gemeten verbindingen, zoals cellulaire 3 G|
| Synchroniseer niet apps                    |Niet gebruiken; heeft geen effect|
|Synchroniseer niet app instellingen             |Hiermee schakelt u zwervende app gegevens|
|Synchroniseer Startinstellingen niet           |Niet gebruiken; heeft geen effect|


## <a name="related-topics"></a>Verwante onderwerpen
- [Enterprise staat Roaming-overzicht](active-directory-windows-enterprise-state-roaming-overview.md)
- [Enterprise staat roaming in Azure Active Directory inschakelen](active-directory-windows-enterprise-state-roaming-enable.md)
- [Instellingen en gegevens zwervende Veelgestelde vragen](active-directory-windows-enterprise-state-roaming-faqs.md)
- [Windows 10 zwervende instellingen-naslag](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
