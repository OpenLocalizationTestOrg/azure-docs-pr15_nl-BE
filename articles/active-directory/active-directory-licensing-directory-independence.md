<properties
   pageTitle="Toevoegen en beheren van meerdere Azure Active Directory directory's | Microsoft Azure"
   description="Instructies en richtlijnen voor het toevoegen en beheren van uw Azure Active Directory mappen en mappen als een volledig onafhankelijke bronnen waarin wordt uitgelegd"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="add-and-manage-multiple-azure-active-directory-directories"></a>Toevoegen en beheren van meerdere Azure Active Directory-adreslijsten

In Azure Active Directory (AD Azure), elke map is een volledig onafhankelijke bron: een peer, volledig uitgerust en logisch onafhankelijk van andere mappen die u wilt beheren. Er is geen bovenliggend-onderliggende relatie tussen mappen. Deze onafhankelijkheid tussen mappen bevat resource onafhankelijkheid, administratieve onafhankelijkheid en onafhankelijkheid van de synchronisatie.

##<a name="resource-independence"></a>Onafhankelijkheid van de bron

Als u maken of verwijderen van een resource in een bepaalde map, heeft dit geen invloed op elke bron in een andere map, klikt u met de gedeeltelijke uitzondering van externe gebruikers, zoals hieronder beschreven. Als u een aangepast domein 'contoso.com' met een bepaalde map, worden niet gebruikt voor een andere map.

##<a name="administrative-independence"></a>Administratieve onafhankelijkheid

Als een gebruiker zonder beheerdersrechten van map 'Contoso' maakt vervolgens een testmap 'Test':
- Standaard is de gebruiker die een map maakt als een externe gebruiker in een nieuwe map toegevoegd en de rol van globale beheerder in de desbetreffende map toegewezen.
- De beheerders van 'Contoso' directory beheerdersmachtigingen geen directe naar map 'Test' tenzij een beheerder van 'Test' ze specifiek deze bevoegdheden verleent. Van 'Contoso' kunnen beheerders de toegang tot de map 'Test' als de besturing van de gebruikersaccount die 'Test'.
- Als u wilt wijzigen (of verwijderen add) een beheerdersrol voor een gebruiker in een bepaalde map, de wijziging heeft geen invloed op de rol van beheerder die de gebruiker heeft mogelijk in een andere map.

##<a name="synchronization-independence"></a>Onafhankelijkheid van synchronisatie

U kunt elke map Azure AD om gegevens gesynchroniseerd van één exemplaar van een onafhankelijk van elkaar te configureren:
  - Het hulpprogramma adreslijstsynchronisatie (DirSync) om gegevens te synchroniseren met een AD forest.
  - Azure Active Directory Connector voor Forefront identiteitsbeheer, om gegevens te synchroniseren met een of meer forests op gebouwen en/of niet-Azure AD-gegevensbronnen.

##<a name="add-an-azure-ad-directory"></a>Een Azure AD-map toevoegen

Een directory Azure AD in de klassieke Azure portal toevoegen, selecteert u de extensie Azure Active Directory aan de linkerkant en tik op **toevoegen**.

> [AZURE.NOTE]   In tegenstelling tot andere bronnen Azure zijn uw mappen onderliggende bronnen van een Azure-abonnement. Als u uw abonnement verloopt op Azure toestaan of annuleren, u nog steeds toegang tot de directory-gegevens met Azure PowerShell de Azure Graph API of andere interfaces zoals Office 365 Admin Center. U kunt ook een ander abonnement koppelen aan de directory.

Zie voor een uitgebreid overzicht van problemen met licentieverlening Azure AD en beste praktijken, [Wat is Azure Active Directory-licentieverlening?](active-directory-licensing-what-is.md).
