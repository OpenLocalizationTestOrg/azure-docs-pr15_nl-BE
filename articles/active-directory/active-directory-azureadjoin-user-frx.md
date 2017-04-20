<properties
    pageTitle="Instellen van een nieuw apparaat met Azure Active Directory tijdens de installatie | Microsoft Azure"
    description="Een onderwerp waarin wordt uitgelegd hoe gebruikers kunnen instellen Azure AD lid tijdens hun eerste ervaring van de uitvoering."
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

# <a name="set-up-a-new-device-with-azure-ad-during-setup"></a>Instellen van een nieuw apparaat met Azure Active Directory tijdens de installatie

In Windows 10 gebruikers kunnen deelnemen aan hun apparaten naar Azure Active Directory (AD Azure) in de eerste sessie (FRX). Hierdoor kan organisaties krimp apparaten aan hun werknemers of de studenten distribueren of laat ze kiezen hun eigen apparaten (CYOD).
Als Windows 10 Professional of Windows 10 Enterprise edities is geïnstalleerd op een apparaat, standaard de ervaring van het setup-proces voor apparaten die eigendom zijn van een bedrijf.

## <a name="to-join-a-device-to-azure-ad"></a>Een apparaat toevoegen aan AD Azure


1. Wanneer u het nieuwe apparaat inschakelen en het setup-proces te starten, ziet u het bericht **Gereed krijgen** . Volg de aanwijzingen voor het instellen van het apparaat.
2. Begin met het aanpassen van uw regio en taal. Accepteer de licentievoorwaarden voor Microsoft-Software.
![Aanpassen voor uw regio](./media/active-directory-azureadjoin/active-directory-azureadjoin-customize-region.png)
3. Selecteer het netwerk dat u gebruiken wilt voor de verbinding met het Internet.
4. Geef aan of u een persoonlijk apparaat of een apparaat voor het bedrijf eigendom. Als het bedrijf eigendom is, klikt u op **Dit apparaat mijn organisatie behoort**. Hiermee start u de Azure AD deelnemen aan ervaring. Hieronder ziet u een scherm dat u ziet als u Windows 10 Professional gebruikt.
<center>
![Wie de eigenaar van deze PC-scherm](./media/active-directory-azureadjoin/active-directory-azureadjoin-who-owns-pc.png)

5.  Geef de referenties die zijn die u van uw organisatie.
<center>
![Aanmeldingsscherm](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)
6.  Nadat u uw gebruikersnaam hebt ingevoerd, wordt een overeenkomende huurder bevindt zich in Azure AD. Als u een federatieve domein, wordt u omgeleid naar de server met Secure Token Service (STS) op gebouwen--bijvoorbeeld Active Directory Federation Services (AD FS).
7. Als u een gebruiker in een domein niet federatieve, Voer uw referenties rechtstreeks op de Azure AD gehoste pagina. Als huisstijlen is geconfigureerd, wordt u ook Zie logo van uw organisatie en ondersteuning van tekst.
8.  U wordt gevraagd naar een meerledige verificatie uitdaging. Dit probleem kan worden geconfigureerd door een IT-beheerder.
9.  Azure AD controleert of deze gebruikersapparaat inschrijving in een mobiel Apparaatbeheer vereist.
10. Windows het apparaat in de map van de organisatie geregistreerd in Azure AD en zich inschrijft in een mobiel Apparaatbeheer, indien van toepassing.
11. Als u een gebruiker beheerde, gaat Windows u naar het bureaublad van het proces voor automatische aanmelding.
12. Als u een federatieve gebruiker, wordt u doorgestuurd naar het Windows-aanmeldingsscherm uw referenties moet invoeren.

> [AZURE.NOTE] Lid worden van een Windows Server Active Directory-domein op gebouwen in de Windows-out-of-box experience wordt niet ondersteund. Dus als u een computer toevoegen aan een domein, moet u de koppeling **Windows met een lokale account instellen** in plaats daarvan. U kunt vervolgens deelnemen aan het domein van de instellingen op uw computer als u eerder hebt uitgevoerd.

## <a name="additional-information"></a>Als u meer informatie
* [Windows 10 voor de onderneming: apparaten gebruik voor werk](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud-mogelijkheden aan Windows 10-apparaten via Azure Active Directory deelnemen aan uitbreiden](active-directory-azureadjoin-user-upgrade.md)
* [Verificatie van identiteiten zonder wachtwoorden via Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Meer informatie over gebruiksscenario's voor Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Domein behoren apparaten aansluiten op Azure AD voor Windows 10 ervaringen](active-directory-azureadjoin-devices-group-policy.md)
* [Azure AD Join instellen](active-directory-azureadjoin-setup.md)
