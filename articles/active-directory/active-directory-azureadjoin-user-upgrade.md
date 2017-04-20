<properties
    pageTitle="Instellen van een Windows 10-apparaat met Azure AD uit instellingen | Microsoft Azure"
    description="Wordt beschreven hoe gebruikers kunnen deelnemen aan naar Azure AD via het menu instellingen."
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

# <a name="set-up-a-windows-10-device-with-azure-ad-from-settings"></a>Instellen van een Windows 10-apparaat met Azure AD van instellingen
Als u al Windows 7 of Windows 8 gebruikt en uw computer of apparaat is bijgewerkt naar Windows 10, kunt u via het menu instellingen Azure Active Directory (AD Azure) koppelen.

## <a name="to-join-to-azure-ad-from-the-settings-menu"></a>Lid worden van Azure AD in het menu instellingen


1. Klik op de prijs van de **Instellingen** in het menu **Start** .
2. Selecteer **systeem**van **Instellingen**,->**over**->**deelnemen aan AD Azure**.
<center>
![Azure AD in het menu instellingen deelnemen aan](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png)</center>

3. Klik op **Doorgaan** in het berichtvenster Azure AD deelnemen.
<center>
![Berichtvenster join Azure AD](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png)</center>
4. Geef uw referenties. Deze zich bevat de stappen die nodig om de volledige verificatie zijn. Als u deel van een federatieve huurder uitmaken, krijgt uw beheerder u de federation-ervaring die door uw organisatie wordt gehost.
<center>
![Referenties van de-in](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</center>
5. Als uw organisatie heeft Azure meerledige verificatie worden geconfigureerd voor het lidmaatschap van Azure AD, bieden de tweede factor voordat u verdergaat.
6. Klik op **accepteren** in het scherm voor **Dit apparaat moeten worden beheerd** .
7. Ziet u het bericht "het apparaat is nu lid van uw organisatie in Azure AD".


## <a name="additional-information"></a>Als u meer informatie
* [Meer informatie over gebruiksscenario's voor Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Domein behoren apparaten aansluiten op Azure AD voor Windows 10 ervaringen](active-directory-azureadjoin-devices-group-policy.md)
* [Azure AD Join instellen](active-directory-azureadjoin-setup.md)
* [Verificatie van identiteiten zonder wachtwoorden via Microsoft Passport](active-directory-azureadjoin-passport.md)
