<properties
   pageTitle="Azure AD B2B samenwerking voorbeeld: hoe het werkt | Microsoft Azure"
   description="Hierin wordt beschreven hoe Azure Active Directory B2B samenwerking ondersteunt uw relaties tussen bedrijven zakenpartners selectief toegang krijgen tot uw zakelijke toepassingen inschakelen"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-how-it-works"></a>Azure AD B2B samenwerking voorbeeld: hoe het werkt
Azure AD B2B-samenwerking is gebaseerd op een uitnodiging en model inwisselen. U opgeven dat de e-mailadressen van de partijen die u wilt werken, samen met de toepassingen die u wilt laten gebruiken. Azure AD ze de uitnodiging van een e-mailbericht met een koppeling worden verzonden. De gebruiker volgt u de koppeling en wordt gevraagd aan te melden met hun Azure AD-account of meld je aan voor een nieuwe Azure AD-account.

1. Uw admin nodigt partner gebruikers door het uploaden van [een gestructureerde CSV-bestand](active-directory-b2b-references-csv-file-format.md) met behulp van de portal Azure.
2. De portal verzonden uitnodigen e-mailberichten voor deze partner gebruikers.
3. De partner-gebruikers klikt u op de koppeling in het e-mailbericht en wordt gevraagd of u aan te melden met hun werk-referenties (als ze al in Azure AD) of zich aanmelden als een gebruiker Azure AD B2B samenwerking.
4. Partner-gebruikers worden omgeleid naar de toepassing die zij zijn uitgenodigd, waar ze nu beschikken.

## <a name="directory-operations"></a>Directorybewerkingen
Partner-gebruikers bestaan in uw advertentie Azure als externe gebruikers. Dit betekent dat uw admin kunt inrichten licenties groepslidmaatschap toewijzen en verdere toegang geven tot bedrijfstoepassingen via de portal Azure of Azure PowerShell wilt, zoals voor gebruikers in uw bedrijf.

Tijdens een betaalde advertentie Azure abonnement (Basic of Premium) is het niet nodig gebruik Azure AD B2B, huurders die beschikken over een betaald abonnement Azure AD (Basic of Premium) ontvangen de volgende voordelen:

 - Beheerders kunnen groepen toewijzen aan apps, voor eenvoudiger beheer van de uitgenodigde gebruikerstoegang bieden.
 - Admin huurder huismerk wordt gebruikt om e-mails met de uitnodiging voor een merk en aflossing ervaring, meer context bieden gebruikers partner uitgenodigd.

## <a name="related-articles"></a>Verwante artikelen
 Onze andere artikelen over Azure AD B2B samenwerking bladeren

 - [Wat is Azure AD B2B samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
 - [Gedetailleerde scenario](active-directory-b2b-detailed-walkthrough.md)
 - [Verwijzing naar een CSV-bestand opmaken](active-directory-b2b-references-csv-file-format.md)
 - [Token-indeling van externe gebruiker](active-directory-b2b-references-external-user-token-format.md)
 - [Wijzigingen van kenmerken object externe gebruiker](active-directory-b2b-references-external-user-object-attribute-changes.md)
 - [Huidige voorvertoning beperkingen](active-directory-b2b-current-preview-limitations.md)
 - [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
