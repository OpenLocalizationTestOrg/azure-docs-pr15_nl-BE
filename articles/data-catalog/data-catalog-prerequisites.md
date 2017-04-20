<properties
   pageTitle="Vereisten voor Azure catalogus gegevens | Microsoft Azure"
   description="Azure catalogus gegevens voorwaarden voor-wat u nodig hebt om aan de slag met Azure gegevenscatalogus."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-prerequisites"></a>Vereisten voor Azure catalogus gegevens

## <a name="what-do-i-need-to-get-started-with-azure-data-catalog"></a>Wat moet ik aan de slag met Azure gegevenscatalogus

Er zijn een paar dingen die gedaan hebt je voordat u de **Catalogus van Azure gegevens**kunt instellen. Geen paniek: ze won't lang duurt!

## <a name="azure-subscription"></a>Azure-abonnement
Catalogus met Azure gegevens instellen, moet u de eigenaar of mede-eigenaar van een Azure-abonnement.

Azure-abonnementen kunnen u toegang tot bronnen van cloud service zoals catalogus met Azure gegevens indelen. Ze ook help u hoe de weergave Resourcegebruik wordt gemeld bepalen, in rekening gebracht en betaald. Elk abonnement kan een verschillende facturerings- en instellingen hebben zodat u beschikt over verschillende abonnementen en verschillende plannen per afdeling, project en regionaal kantoor. Elke cloud service deel uitmaakt van een abonnement en moet u een abonnement op voordat u de catalogus gegevens Azure instelt. Voor meer informatie, Zie [beheren van Accounts, abonnementen, en administratieve rollen](../active-directory/active-directory-assign-admin-roles.md).

## <a name="azure-active-directory"></a>Azure Active Directory
Azure gegevenscatalogus stelt moet u zijn aangemeld met een gebruikersaccount Azure Active Directory.

Azure Active Directory (AD Azure) biedt een eenvoudige manier voor uw bedrijf voor het beheren van identiteits- en toegangsbeheer, zowel in de cloud en op gebouwen. Gebruikers kunnen een enkel werk of school-account gebruiken voor eenmalige aanmelding naar de cloud en webtoepassing op gebouwen. Azure AD catalogus Azure gegevens gebruikt voor verificatie-on. Voor meer informatie, Zie [Wat is Azure Active Directory](../active-directory/active-directory-whatis.md).

> [AZURE.NOTE] De [Azure portal](http://portal.azure.com/) kunnen gebruikers zich aanmelden met behulp van een persoonlijke Microsoft-Account of een Azure Active Directory werkt of school account. Om catalogus Azure gegevens met behulp van de Azure portal of via de [portal gegevenscatalogus](http://www.azuredatacatalog.com) in te stellen moet u zijn aangemeld met een Azure Active Directory-account, niet een persoonlijke account.

## <a name="active-directory-policy-configuration"></a>Configuratie van Active Directory-beleid

In sommige gevallen kunnen gebruikers een situatie waar ze aanmelden kunnen op de portal catalogus Azure-gegevens, maar wanneer ze proberen aan te melden op de data source registratie tool die zij een foutmelding dat verhindert dat ze zich aanmelden ontvangt op optreden. Dit probleem kan gebeuren wanneer de gebruiker op het netwerk van het bedrijf, of alleen plaatsvinden kan wanneer de gebruiker verbinding vanaf buiten het bedrijfsnetwerk maakt.

Het hulpprogramma voor de registratie van gegevens wordt Forms-verificatie gebruikt voor het valideren van aanmeldingen van gebruikers met Active Directory. Voor een geslaagde aanmelding moet Forms-verificatie zijn ingeschakeld in het globaal beleid voor verificatie door de beheerder van Active Directory.

Het globaal beleid voor verificatie kunt verificatiemethoden moeten afzonderlijk worden ingeschakeld voor het intranet en extranet-verbindingen, zoals hieronder wordt geïllustreerd. Aanmeldingsfouten kunnen optreden als de verificatie van formulieren is niet ingeschakeld voor het netwerk van waaruit de gebruiker verbinding maakt.

 ![Globale verificatie-Active Directory-beleid](./media/data-catalog-prerequisites/global-auth-policy.png)

Zie [Beleid voor verificatie configureren](https://technet.microsoft.com/library/dn486781.aspx)voor meer informatie.
