<properties
    pageTitle="Azure AD verbinden: Veelgestelde vragen | Microsoft Azure"
    description="Deze pagina bevat veelgestelde vragen over Azure AD verbinden."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-faq"></a>Azure AD Veelgestelde vragen over verbinding

## <a name="general-installation"></a>Algemene installatie
**V: installatie werkt als het globale beheer van Azure AD 2FA ingeschakeld?**  
Met de opbouw van februari 2016, wordt dit ondersteund.

**V: is er een manier om te installeren zonder toezicht Azure AD-verbinding?**  
Deze optie wordt alleen ondersteund Azure AD verbinding maken met de installatiewizard installeren. Een installatie zonder toezicht en stille wordt niet ondersteund.

**V: ik heb een forest waarin één domein is niet bereikbaar. Hoe installeer ik Azure AD verbinden**  
Met de opbouw van februari 2016, wordt dit ondersteund.

**V: de AD DS-health agent werkt op een server core?**  
Ja. Na het installeren van de agent, kunt u het registratieproces volledig doorlopen met behulp van de volgende PowerShell-commandlet: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

## <a name="network"></a>Netwerk
**V: ik heb een firewall, een netwerkapparaat of iets anders dat de maximale tijd verbindingen beperkt geopend in mijn netwerk kunt blijven. Hoe lang moet mijn client side-out drempel worden bij het gebruik van Azure AD verbinden?**  
Software voor alle netwerken, fysieke apparaten of iets anders dat beperkt de maximale tijd verbindingen open mogen blijven moet een drempel van ten minste 5 minuten (300 seconden) gebruiken voor de verbinding tussen de server waarop de Azure AD Connect-client is geïnstalleerd en Azure Active Directory. Dit geldt ook voor alle eerder uitgebrachte Microsoft Identity synchronisatie-hulpprogramma's.

**V: zijn niveaudomeinen (één Label domeinen) ondersteund?**  
Nee, Azure AD op ruimten bossen/domeinen met niveaudomeinen biedt geen ondersteuning voor verbinden.

**V: zijn "dotted" NetBios naam ondersteund?**  
Nee, Azure AD Connect biedt geen ondersteuning voor op gebouwen bossen/domeinen waarin de NetBIOS-naam met een punt '. ' in de naam.

## <a name="federation"></a>Federatie
**V: wat moet ik doen als ik een e-mail ontvangt die vragen om mijn Office 365-certificaat te vernieuwen**  
Gebruik de richtlijnen die wordt beschreven in het onderwerp [vernieuwen van certificaten](active-directory-aadconnect-o365-certs.md) voor het vernieuwen van het certificaat.

**V: ik heb 'Automatisch bijwerken gebruikmakende partij' ingesteld voor O365 gebruikmakende partij. Heb ik niets te doen als mijn certificaat automatisch token boven**  
Gebruik de richtlijnen die wordt beschreven in het artikel [certificaten vernieuwen](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Omgeving
**V: is het om de naam van de server na de installatie van Azure AD verbinden te ondersteund?**  
Nr. De naam van de server wijzigt, worden de synchronisatie-engine niet mogelijk verbinding maken met de SQL-database en de service is niet gestart.

## <a name="identity-data"></a>Id-gegevens
**V: het kenmerk UPN (userPrincipalName) in Azure AD niet overeenkomt met de UPN van prem - waarom?**  
Raadpleeg de volgende artikelen:

- [Gebruikersnamen in Office 365, Azure of Intune komen niet overeen met de UPN in de lokalen of andere aanmeldings-ID](https://support.microsoft.com/en-us/kb/2523192)
- [Wijzigingen worden niet door het hulpprogramma Active Directory-synchronisatie Azure gesynchroniseerd nadat u de UPN van een gebruikersaccount voor het gebruik van een ander domein federatieve wijzigen](https://support.microsoft.com/en-us/kb/2669550)

U kunt ook Azure AD zodat de synchronisatie-engine de userPrincipalName bijwerken zoals beschreven in [Azure AD verbinden sync servicefuncties](active-directory-aadconnectsyncservice-features.md).

## <a name="custom-configuration"></a>Aangepaste configuratie
**Q: waar zijn de PowerShell-cmdlets voor het verbinden van Azure AD beschreven?**  
Met uitzondering van de cmdlets beschreven op deze site, worden andere PowerShell-cmdlets gevonden in Azure AD verbinding niet ondersteund voor gebruik van de klant.

* *V: kan ik gebruiken "Server exportserver importeren' gevonden in de *Service met Synchronisatiebeheer* configuratie naar een andere servers? **  
Nr. Deze optie wordt alle configuratie-instellingen niet ophalen en mag niet worden gebruikt. In plaats daarvan moet u de wizard gebruiken de basisconfiguratie op de tweede server maken en de regeleditor sync gebruiken om een aangepaste regel verplaatsen tussen servers PowerShell-scripts genereren. Zie [aangepaste configuratie van actief naar de staging-server verplaatsen](active-directory-aadconnect-upgrade-previous-version.md#move-custom-configuration-from-active-to-staging-server).

## <a name="troubleshooting"></a>Het oplossen van problemen
**V: hoe krijg ik hulp bij het verbinden van Azure AD**

[De Microsoft Knowledge Base (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

- Zoek in de Microsoft Knowledge Base (KB) voor technische oplossingen voor problemen oplossen / herstellen over ondersteuning voor Azure AD verbinden.

[Microsoft Azure Active Directory-Forums](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

- U kunt zoeken en bladeren voor technische vragen en uit de Gemeenschap antwoorden of door te klikken op uw eigen vraag [hier](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Customer support van Azure AD verbinden](https://manage.windowsazure.com/?getsupport=true)

- Gebruik deze koppeling om ondersteuning te krijgen via de portal Azure.
