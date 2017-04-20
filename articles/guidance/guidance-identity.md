<properties
   pageTitle="Beheer van identiteit in Azure | Microsoft Azure"
   description="Uitgelegd en vergelijkt de verschillende methoden voor het beheren van de identiteit in hybride systemen die zich uitstrekken over de grens op-ruimten/cloud met Azure."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>
<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/26/2016"
   ms.author="telmosampaio"/>
   
# <a name="managing-identity-in-azure"></a>Identiteit in Azure beheren

In de meeste enterprise-systemen op basis van Windows gebruikt u Active Directory (AD) identity managementdiensten leveren aan uw toepassingen. AD goed werkt in een omgeving van gebouwen, maar wanneer u naar de cloud infrastructuur van uw netwerk uitbreiden, hebt u bepaalde belangrijke beslissingen nemen met betrekking tot het beheren van de identiteit. Moet u uw domeinen op ruimten VMs in de cloud opnemen uitbreiden? Moet u nieuwe domeinen in de cloud maken en zo ja, hoe? Moet u uw eigen forest implementeert in de cloud of moet u gebruik maken van Azure Active Directory (AAD)?

Algemene opties voor het voldoen aan de uitdagingen die in dit scenario wordt beschreven en kunt die u bepalen welke oplossing beter op uw behoeften op basis van uw behoeften.

## <a name="using-azure-active-directory"></a>Met behulp van Azure Active Directory

AAD kunt u een AD-domein in de cloud maken en koppelen aan een bedrijf op AD-domein. AAD kunt u eenmalige aanmelding (SSO) configureren voor gebruikers die werken met toepassingen die toegankelijk is via de cloud.

[! [0]][0]

AAD is een eenvoudige manier voor het implementeren van een beveiligingsdomein in de cloud. Het wordt gebruikt door veel Microsoft-toepassingen, zoals Microsoft Office 365. 

Voordelen van het gebruik van AAD:

- Er is niet nodig voor het onderhouden van een Active Directory-infrastructuur in de cloud. AAD is volledig beheerd en onderhouden door Microsoft.

- AAD biedt dezelfde identiteitsgegevens is beschikbaar op gebouwen.

- Verificatie kan in Azure, verminderen de noodzaak voor externe toepassingen en gebruikers contact opnemen met het domein van gebouwen gebeuren.

Punten bij de AAD:

- Identiteit zijn beperkt tot gebruikers en groepen. Er is geen mogelijkheid om service- en computeraccounts worden geverifieerd.

- U moet verbinding configureren met uw domein op gebouwen de AAD directory gesynchroniseerd te houden. 

- U bent verantwoordelijk voor het uitgeven van toepassingen die gebruikers toegang de in de cloud via AAD tot.

Lees [Uitvoering van Azure Active Directory]voor gedetailleerde informatie[implementing-aad].

## <a name="using-active-directory-in-the-cloud-joined-to-an-on-premises-forest"></a>Met Active Directory in de cloud deel uitmaakt van een forest op ruimten

U kunt hosten op ruimten AD Directory Services (AD DS) kan, maar in een hybride scenario waar de elementen van een toepassing in Azure bevinden zich efficiënter voor het repliceren van deze functionaliteit en de opslagplaats AD naar de cloud. Deze benadering verkleint de vertraging veroorzaakt door verificatie verzenden en lokale vergunning aanvragen uit de cloud terug naar AD DS op locatie uitgevoerd. 

[! [1]][1]

Deze aanpak vereist dat u uw eigen domein in de cloud maken en aan het forest op gebouwen toevoegen. U maakt een VMs als host voor de AD DS-services.

Voordelen van het gebruik van een afzonderlijk domein in de cloud:

- Biedt de mogelijkheid voor de verificatie van gebruikers-, service- en rekeningen op-ruimten en in de cloud.

- Biedt toegang tot dezelfde identiteitsgegevens is beschikbaar op gebouwen.

- Het is niet nodig voor het beheren van een afzonderlijk AD forest; het domein in de cloud kan deel uitmaken van het forest in gebouwen.

- U kunt Groepsbeleid is gedefinieerd door een GPO-objecten voor ruimten met het domein in de cloud kunt toepassen.

Overwegingen bij het gebruik van een afzonderlijk domein in de cloud:

- Moet u uw eigen servers in AD DS en een domein in de cloud maken en beheren.

- Het is mogelijk sommige latentie synchronisatie tussen de servers in het domein in de cloud en de servers die worden uitgevoerd op locatie.

Zie voor meer informatie over het configureren van deze architectuur [Uitbreiden van Active Directory Directory Services (ADDS) naar Azure][extending-adds].

## <a name="using-active-directory-with-a-separate-forest"></a>Met behulp van Active Directory met een afzonderlijk forest

Een organisatie met Active Directory (AD) van gebouwen hebben een forest die bestaat uit veel verschillende domeinen. U kunt domeinen isolatie tussen functionele gebieden die moeten worden geïsoleerd, mogelijk om veiligheidsredenen bieden, maar u kunt gegevens uitwisselen tussen domeinen door vertrouwensrelaties tot stand te brengen.

[! [2]][2]

Een organisatie die gebruikmaakt van afzonderlijke domeinen kunt profiteren van Azure verplaatst u een of meer van deze domeinen in een afzonderlijk forest in de cloud. U kunt ook een organisatie mogelijk wilt houden alle cloud bronnen logisch afzonderlijk van de aangehouden op-ruimten en informatie over de cloud in hun eigen directory opgeslagen als onderdeel van een forest ook gehouden in de cloud.

Voordelen van het gebruik van een afzonderlijk forest in de cloud:

- U kunt implementeren op ruimten identiteiten en afzonderlijke identiteiten Azure alleen-lezen.

- Is het niet nodig voor het repliceren van de ruimten op AD-forest naar Azure, waardoor de gevolgen van netwerkvertraging.

Overwegingen met betrekking tot:

- Verificatie van identiteiten voor gebouwen in de cloud voert extra netwerk *hops* tot de lokalen op AD-servers.

- Moet u uw eigen servers in AD DS en het forest te implementeren in de cloud, en de juiste vertrouwensrelaties tussen forests.

Het [maken van een forest met bronnen Active Directory Directory Services (ADDS) in Azure] document[ adds-forest-in-azure] wordt beschreven hoe u voor de uitvoering van deze aanpak in meer detail.

## <a name="using-active-directory-federation-services-adfs-with-azure"></a>Met behulp van Active Directory Federation Services (ADFS)-met Azure

ADFS kan op locatie worden uitgevoerd kan, maar in een hybride scenario waar toepassingen zich in Azure efficiënter voor het implementeren van deze functionaliteit in de cloud, zoals hieronder wordt weergegeven.

[! [3]][3]

Deze architectuur is vooral geschikt voor:

- Oplossingen die gebruikmaken van federatieve vergunning partnerorganisaties webtoepassingen worden blootgesteld.

- Systemen die toegang van buiten de firewall van de organisatie webbrowsers ondersteunen.

- Systemen waarmee gebruikers toegang hebben tot webtoepassingen door verbinding te maken naar geautoriseerde externe apparaten, zoals externe computers, laptops en andere mobiele apparaten. 

Voordelen van het gebruik van AD FS met Azure:

- U kunt gebruikmaken van claimbewuste toepassingen.

- Biedt de mogelijkheid om vertrouwde externe partners voor verificatie.

- Het biedt compatibiliteit met een groot aantal verificatieprotocollen.

Overwegingen voor het gebruik van AD FS met Azure:

- Dit is vereist voor het implementeren van uw eigen ADDS ADFS en AD FS Web Application Proxy servers in de cloud.

- Deze architectuur is eenvoudig te configureren.

Voor gedetailleerde informatie, lees de [Implementatie van Active Directory Federation Services (ADFS) in Azure][adfs-in-azure].

## <a name="next-steps"></a>Volgende stappen

De volgende bronnen wordt uitgelegd hoe voor de uitvoering van de in dit artikel beschreven architecturen.

- [Implementeren van Azure Active Directory][implementing-aad]
- [Active Directory directoryservices (ADDS) uitbreiden naar Azure][extending-adds]
- [Maken van een forest met bronnen van Active Directory Directory Services (ADDS) in Azure][adds-forest-in-azure]
- [Het implementeren van Active Directory Federation Services (ADFS) in Azure][adfs-in-azure]

<!-- Links -->
[0]: ./media/guidance-identity/figure1.png "Cloud identity-architectuur met Azure Active Directory"
[1]: ./media/guidance-identity/figure2.png "Hybride netwerkarchitectuur met Active Directory beveiligen"
[2]: ./media/guidance-identity/figure3.png "Secure hybride netwerkarchitectuur met afzonderlijke AD domeinen en forests"
[3]: ./media/guidance-identity/figure4.png "Hybride netwerkarchitectuur met AD FS beveiligde"
[implementing-aad]: ./guidance-identity-aad.md
[extending-adds]: ./guidance-identity-adds-extend-domain.md
[adds-forest-in-azure]: ./guidance-identity-adds-resource-forest.md
[adfs-in-azure]: ./guidance-identity-adfs.md