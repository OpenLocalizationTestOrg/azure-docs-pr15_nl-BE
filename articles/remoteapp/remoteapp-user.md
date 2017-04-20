<properties
    pageTitle="Een gebruiker toevoegen aan uw collectie RemoteApp-Azure | Microsoft Azure"
    description="Informatie over het toevoegen van gebruikers aan uw Azure RemoteApp-collectie"
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

# <a name="how-to-add-a-user-to-your-azure-remoteapp-collection"></a>Een gebruiker toevoegen aan uw collectie Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Voordat uw gebruikers kunnen zien en uw apps in Azure RemoteApp gebruiken, hebt u ze toegang verlenen tot uw verzameling. Dit is gemakkelijk: Geef de accountgegevens voor de gebruiker op het tabblad **Toegang** en klik op het vinkje.

Welke gegevens hebt u nodig? Dat is afhankelijk van het type verzameling u hebt gemaakt (cloud of hybride) en of u met Office 365 ProPlus in die collectie.

## <a name="supported-user-identities"></a>Ondersteunde gebruikersidentiteiten

De verschillende typen (cloud of hybride) ondersteuning voor het gebruik van verschillende gebruikersidentiteiten voor toegang tot toepassingen.  

Voor een hybride RemoteApp-collectie moet u instellen van een Active Directory-domeininfrastructuur van lokalen en een huurder Azure Active Directory met Directory-integratie (en eventueel ook eenmalige aanmelding). Bovendien moet u sommige Active Directory-objecten in de directory op gebouwen maken.  

Voor een cloud-collectie van RemoteApp-kan elke willekeurige gebruiker die Azure Active Directory ondersteunen identiteiten gebruikerstoegang worden verleend naar RemoteApp Microsoft Accounts opnemen.  Zie de onderstaande tabel.

Gebruikers van Office 365 zijn Azure Active Directory-gebruikers. Als ze hybride Azure Active Directory hebben, kunnen Directory-accounts gesynchroniseerd ze gebruikerstoegang worden verleend in een hybride RemoteApp-implementatie.   

U kunt deze tabel gebruiken als naslag voor die identiteit wordt ondersteund in uw collectie en wat de vereisten van Active Directory.

|Gebruikersaccounts |Wolk   |Hybride|
|--------------|--------|------|
|Microsoft-Account|     Ja|    Nee|
|Azure Active Directory (AD Azure)| | |
|Azure AD cloud    |Ja    |Nee |
|ADsync met wachtwoord synchronisatie  |Ja    |Ja    |
|ADsync zonder wachtwoord synchronisatie|  Ja |Nee |
|ADsync met AD FS  |Ja    |Ja    |
|[3de partij Azure id-providers ondersteund](https://msdn.microsoft.com/library/azure/jj679342.aspx)  (zoals Ping) |Ja    |Ja|
|Meerledige verificatie    |Ja    |Ja    |

[Meer informatie](remoteapp-ad.md) over het configureren van Active Directory voor RemoteApp uitchecken.


> [AZURE.NOTE] De Azure Active Directory-gebruikers moeten zijn van de huurder die is gekoppeld aan uw abonnement. (U kunt weergeven en wijzigen van uw abonnement op het tabblad **Instellingen** in de portal. Zie [de huurder Azure Active Directory wijzigen die wordt gebruikt door RemoteApp](remoteapp-changetenant.md) voor meer informatie.)

## <a name="office-365-proplus-user-account-information"></a>Office 365 ProPlus gebruikersaccountgegevens
Als u de Office 365 ProPlus afbeelding van de sjabloon in de collectie *of* als u een aangepaste afbeelding die gebruikmaakt van Office 365 hebt gemaakt, wordt u alleen toegestaan Azure Active Directory-gebruikers die Office 365-abonnementen voor het standaarddomein van uw abonnement hebt toe te voegen. [Met behulp van Office 365 met Azure RemoteApp-](remoteapp-o365.md) Zie voor meer informatie.
