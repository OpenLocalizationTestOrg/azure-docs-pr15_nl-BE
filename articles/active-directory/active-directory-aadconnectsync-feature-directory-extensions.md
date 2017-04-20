<properties
   pageTitle="Azure AD verbinden sync: extensies Directory | Microsoft Azure"
   description="Dit onderwerp beschrijft de functie van de extensies directory in Azure AD verbinden."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/19/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-directory-extensions"></a>Azure AD verbinden sync: map extensies
Directory-extensies kunt u het schema uitbreiden in Azure AD met uw eigen kenmerken van Active Directory voor het bedrijf. Met deze functie kunt u kenmerken die u doorgaan met het beheren van lokalen die LOB-toepassingen te bouwen. Deze kenmerken kunnen via [Azure AD Graph map extensies](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) of [Microsoft Graph](https://graph.microsoft.io/)worden verbruikt. Hier ziet u de kenmerken die beschikbaar zijn met respectievelijk [explorer Azure AD Graph](https://graphexplorer.cloudapp.net) en [Microsoft Graph explorer](https://graphexplorer2.azurewebsites.net/) .

Op dit moment verbruikt geen Office 365 werkbelasting deze kenmerken.

U configureren welke extra kenmerken die u wilt synchroniseren in het pad van de aangepaste instellingen in de installatiewizard.
![Schema uitbreiding Wizard](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png) de installatie bevat de volgende kenmerken geldige kandidaten zijn:

- Objecttypen gebruiker en groep
- Kenmerken met één waarde: String, Booleaans, Integer, binair
- Kenmerken met meerdere waarden: String, Binary

De lijst met kenmerken wordt gelezen uit het cachegeheugen die zijn gemaakt tijdens de installatie van Azure AD verbinden. Als u extra kenmerken het Active Directory-schema hebt uitgebreid, het [schema moet worden vernieuwd](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) voordat deze nieuwe kenmerken worden weergegeven.

Een object kan maximaal 100 extensies directory-kenmerken hebben. De maximale lengte is 250 tekens. Als een waarde van het kenmerk langer is, wordt het afgekapt door de synchronisatie-engine.

Een toepassing is geregistreerd waarin deze kenmerken beschikbaar zijn tijdens de installatie van Azure AD verbinden. U kunt deze toepassing in de portal Azure zien.  
![Schema uitbreiding App](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3.png)

Deze kenmerken zijn nu beschikbaar via de grafiek:  
![Grafiek](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

De kenmerken worden voorafgegaan door de extensie\_{AppClientId}\_. De AppClientId heeft dezelfde waarde voor alle kenmerken in de directory Azure AD.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het configureren van [Azure AD Connect worden gesynchroniseerd](active-directory-aadconnectsync-whatis.md) .

Meer informatie over de [integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md).
