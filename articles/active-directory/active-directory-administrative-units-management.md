<properties
   pageTitle="Beheer van bestuurlijke eenheden in Azure Active Directory"
   description="Voor meer gedetailleerde overdracht van machtigingen in Azure Active Directory met behulp van administratieve eenheden"
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

# <a name="administrative-units-management-in-azure-ad---public-preview"></a>Beheer van bestuurlijke eenheden in Azure AD - Public Preview

Dit artikel beschreven administratieve eenheden – een nieuwe Azure Active Directory-container van de middelen die kunnen worden gebruikt voor het overdragen van beheerdersmachtigingen over subsets van gebruikers en het toepassen van beleid in een subset van gebruikers. Administratieve eenheden kunnen in Azure Active Directory, centrale beheerders machtigingen voor administrators regionale overdragen of beleid instellen op een gedetailleerd niveau.

Dit is handig in bedrijven met onafhankelijke divisies, bijvoorbeeld een grote universiteit bestaat uit een groot aantal autonome scholen (Business school, Engineering school, enzovoort) die onafhankelijk van elkaar. Deze schotten hebben hun eigen IT-beheerders die toegang, gebruikers beheren en beleid instellen voor het hun afdeling. Centrale beheerders wilt kunnen verlenen deze divisie beheerders machtigingen via de gebruikers in hun specifieke sectoren. Meer in het bijzonder kunt het volgende voorbeeld centraal beheerder bent, bijvoorbeeld, maken een administratieve eenheid voor een bepaalde school (Business school) en gevuld met de Business school gebruikers. Vervolgens een centrale beheerder de Business school IT-personeel kunt toevoegen aan de rol van een bereik met andere woorden, verlenen de IT-afdeling van Business school beheerdersmachtigingen alleen via de Business school administratieve eenheid.

> [AZURE.IMPORTANT]
> U kunt maken en bestuurlijke eenheden alleen gebruiken als u premie van Azure Active Directory inschakelen. Zie voor meer informatie, [aan de slag met Azure AD Premium](active-directory-get-started-premium.md).

Een administratieve eenheid is uit oogpunt van de centrale beheerder, een Active directory-object dat kan worden gemaakt en gevuld met resources. **Deze bronnen kunnen alleen gebruikers worden in deze release.** Eenmaal gemaakt en gevuld, kan de administratieve eenheid worden gebruikt als een scope voor het beperken van de verleende machtiging alleen via de bronnen die zich bevinden in de administratieve eenheid.

## <a name="managing-administrative-units"></a>Administratieve eenheden beheren

In deze preview-versie kunt u maken en beheren van administratieve eenheden met de Azure Active Directory-Module voor Windows PowerShell-cmdlets.

Zie voor meer informatie over softwarevereisten en installatie van de module AD Azure en voor informatie over de cmdlets Azure AD Module voor het beheren van administratieve eenheden, inclusief syntaxis, parameter beschrijvingen en voorbeelden van [Azure AD met Windows PowerShell beheren](https://msdn.microsoft.com/library/azure/jj151815.aspx).


## <a name="next-steps"></a>Volgende stappen
[Azure Active Directory-edities](active-directory-editions.md)
