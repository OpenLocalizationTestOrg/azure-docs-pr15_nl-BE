
<properties
    pageTitle="De huurder Azure Active Directory in Azure RemoteApp wijzigen | Microsoft Azure"
    description="Informatie over het wijzigen van de pachter Azure Active Directory is gekoppeld aan een RemoteApp Azure"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="change-the-azure-active-directory-tenant-in-azure-remoteapp"></a>De huurder Azure Active Directory in Azure RemoteApp wijzigen

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Azure RemoteApp gebruikt Azure Active Directory (AD Azure) gebruikerstoegang te verlenen. De huurder alleen Azure AD die u in Azure RemoteApp gebruiken kunt is die is gekoppeld aan het abonnement Azure. U kunt het abonnement gekoppeld weergeven op de pagina **-Instellingen** in de portal. Controleer de kolom **map** op het tabblad **abonnementen** .

> [AZURE.NOTE] Deze wijziging uitgevoerd, eerst alle gebruikers verwijderen uit de bestaande huurder Azure Active Directory uit alle Azure RemoteApp-collecties. Hiertoe gaat u naar de Portal Azure, Ga naar het tabblad **RemoteApp Azure** en elke Azure RemoteApp-collectie te openen. Ga naar het tabblad **gebruikers** en gebruikers die deel uitmaken van uw huidige huurder van Azure Active Directory verwijderen. Herhaal dit voor alle bestaande Azure RemoteApp-collecties. Dit niet doet, wordt u niet maken of patch collecties zijn.

Als u gebruiken een andere huurder wilt, gebruikt u deze stappen kunt u de koppeling met het abonnement:

1. Verwijder Azure AD gebruikers waarop u toegang tot RemoteApp-Azure collecties krijgen hebt in de portal. (Zie de opmerking hierboven voor stapsgewijze instructies over hoe u dit moet doen).


2. Een Microsoft-account (voorheen een Live ID) ingesteld als beheerder van de Service. (Weet niet of de service admin al zijn? U kunt uitzoeken door te klikken op **Instellingen -> Administrators**.) Nu, hier is hoe u deze instelling wijzigen:
    1. Klik op de gebruiker in de rechterbovenhoek en klik vervolgens op **Mijn factuur bekijken**.
    2. Klik op het abonnement. Op de nieuwe pagina, schuif naar beneden en klik op **abonnement details bewerken** in het rechter. (De midden onder rechts, als dat helpt u gesorteerd).
    3. Typ het Microsoft-account voor de gebruiker die de servicebeheerder.

3. Nu afmelden bij de portal en meld u weer met het Microsoft-account dat u hebt opgegeven in de vorige stap.


4. Klik op **Nieuw -> App Services -> Active Directory-map > -> aangepaste maken**.
5. Kies onder **map**, **bestaande directory gebruiken**. Gaan we dat nu afgemeld bij de portal, dus kies **dat ik ben klaar om te worden afgemeld**.
6. Je weer aanmelden bij de portal als een globale beheerder van de map die u wilt toevoegen. (Als je niet al een globale beheerder, zult u na een ronde van aanmelden en vervolgens afmelden.)
7. U wordt gevraagd wanneer u zich aanmeldt als u wilt uw bestaande AD huurder gebruiken bij uw abonnement. Klik op **Doorgaan**en klik vervolgens op **nu afmelden**.
5. Weer opnieuw inloggen en Ga terug naar **Instellingen -> abonnementen**. Selecteer uw abonnement en klik op **Map bewerken**. Selecteer de huurder Azure advertentie die u wilt gebruiken.



U kunt nu gebruik van de nieuwe Azure AD huurder toegang tot het abonnement Azure en gebruikerstoegang in Azure RemoteApp configureren.
