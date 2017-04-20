<properties
   pageTitle="Aanbevolen procedures voor beveiliging en codering | Microsoft Azure"
   description="Dit artikel bevat een set van best practices voor beveiliging van gegevens en met behulp van codering ingebouwd in Azure mogelijkheden."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="yuridio"/>

#<a name="azure-data-security-and-encryption-best-practices"></a>Aanbevolen procedures voor Azure gegevensbeveiliging en -codering

Een van de sleutels tot bescherming van de gegevens in de cloud is voor de verrekening van de status van uw gegevens kan optreden en welke besturingselementen beschikbaar zijn voor die staat. Azure gegevens voor de beveiligings- en aanbevolen procedures voor de aanbevelingen worden rond de lidstaten de volgende gegevens:

- In de rust: Dit omvat alle informatie objecten voor gegevensopslag, containers en typen die statisch aanwezig zijn op een fysiek medium, worden deze magnetische of optische schijven.

- In Transit: Wanneer gegevens worden overgebracht tussen onderdelen, vestigingen of programma's, zoals via het netwerk, via een bus service (van-lokale cloud en omgekeerd, met inbegrip van hybride verbindingen zoals ExpressRoute) of tijdens een invoer/uitvoer, het is beschouwd als in beweging.

In dit artikel bespreken we een verzameling van gegevens Azure beveiligings- en best practices. Deze best practices zijn afgeleid van onze ervaring met Azure gegevensbeveiliging en -codering en de ervaringen van gebruikers zoals uzelf.

Voor elke beste leggen we:

- Wat de beste manier is
- Waarom u wilt dat het beste inschakelen
- Wat kan het resultaat zijn als u niet het beste inschakelen
- Mogelijke alternatieven voor de beste praktijken
- Hoe leert u het beste inschakelen

In dit artikel Azure gegevensbeveiliging en Best Practices van codering is gebaseerd op een advies consensus en Azure platformmogelijkheden en functiesets, die zijn op het moment dat dit artikel werd geschreven. Adviezen en technologieën veranderen en in dit artikel wordt op gezette tijden met deze wijzigingen bijgewerkt.

Azure gegevens beveiligings- en aanbevolen procedures die worden beschreven in dit artikel zijn:

- Meerledige verificatie afdwingen
- Gebruik rol gebaseerd toegangsbeheer RBAC)
- Coderen van Azure virtual machines
- Beveiligingsmodellen hardware gebruiken
- Met beveiligde werkstations beheren
- SQL-versleuteling van gegevens inschakelen
- Beveiligen van gegevens onderweg
- Bestand niveau gegevenscodering worden afgedwongen.


## <a name="enforce-multi-factor-authentication"></a>Meerledige verificatie afdwingen

De eerste stap bij de toegang tot gegevens en beheer in Microsoft Azure is om de gebruiker te verifiëren. [Azure meerledige verificatie (MVR gesloten)](../multi-factor-authentication/multi-factor-authentication.md) is een methode om te controleren de identiteit van de gebruiker via een andere methode dan alleen een gebruikersnaam en wachtwoord. Deze verificatie methode helpt beschermen toegang tot gegevens en toepassingen voldoen aan eisen van de gebruiker voor een eenvoudig aanmelden proces.

Azure MVR gesloten is ingeschakeld voor uw gebruikers, voegt u een tweede laag van beveiliging voor aanmeldingen van gebruikers en transacties. In dit geval een transactie kan toegang krijgen tot een document in een bestandsserver of in uw SharePoint Online. Azure MVR gesloten kunt u ook de kans dat een gekraakte referentie toegang tot gegevens van de organisatie van hebben IT.

Bijvoorbeeld: als u Azure MVR gesloten afdwingen voor uw gebruikers configureert voor het gebruik van een telefoongesprek of een SMS-bericht als de verificatie wordt uitgevoerd, als de referenties van de gebruiker gevaar loopt, toegang tot alle bronnen, omdat hij geen toegang tot de telefoon van de gebruiker de aanvaller niet mogelijk. Organisaties die deze extra laag van bescherming van de identiteit niet toevoegt, zijn gevoeliger voor referentie diefstal aanvallen, wat tot inbreuk op gegevens leiden kan.

Een alternatief voor organisaties die houden de verificatie controle willen op-ruimten is met [Azure meerledige verificatieserver](../multi-factor-authentication/multi-factor-authentication-get-started-server.md), ook wel genoemd MVR gesloten op gebouwen. Met deze methode zich u nog wel af te dwingen meerledige verificatie, terwijl de MVR gesloten server op-ruimten.

Lees het artikel [aan de slag met Azure meerledige verificatie in de cloud](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)voor meer informatie over Azure MVR gesloten.

## <a name="use-role-based-access-control-rbac"></a>Gebruik Role Based Access Control RBAC)
Toegang op basis van de beginselen van de beveiliging [moet weten](https://en.wikipedia.org/wiki/Need_to_know) en [minimale bevoegdheden](https://en.wikipedia.org/wiki/Principle_of_least_privilege) beperken. Dit is van cruciaal belang voor organisaties die willen af te dwingen voor toegang tot gegevens. Azure Role-Based Access Control RBAC () kan worden gebruikt om machtigingen toewijzen aan gebruikers, groepen en toepassingen op een bepaalde scope. Het bereik van een roltoewijzing is een abonnement, een resourcegroep of een enkele bron.

U kunt gebruikmaken van [ingebouwde RBAC-rollen](../active-directory/role-based-access-built-in-roles.md) in Azure bevoegdheden toewijzen aan gebruikers. Overweeg het gebruik van *Opslag Account Inzender* voor operators cloud die nodig zijn voor het beheren van opslag accounts en rol van de *Klassieke opslag Account Inzender* klassieke opslag accounts beheren. Voor operators cloud die moet VMs en opslag account beheren, kunt u ze toe te voegen aan de rol van *Inzender voor virtuele Machine* .

Organisaties die gegevens toegangsbeheer niet afdwingen door gebruik te maken van mogelijkheden zoals RBAC kunnen geven meer bevoegdheden beschikken dan noodzakelijk is voor hun gebruikers. Dit kan leiden tot inbreuk op gegevens door sommige gebruikers die toegang hebben tot gegevens die in eerste instantie niet moet hebben.

U kunt meer over Azure RBAC lezen van het artikel [Azure_Role-Based Access Control](../active-directory/role-based-access-control-configure.md).

## <a name="encrypt-azure-virtual-machines"></a>Coderen van Azure Virtual Machines
[Codering van gegevens in rust](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) is voor veel organisaties een verplichte stap in de richting van, gegevensprivacy, compliance en gegevens-soevereiniteit. Azure schijfversleuteling kunnen IT-beheerders voor het coderen van Windows en Linux IaaS Virtual Machine (VM) schijven. Azure schijf codering maakt gebruik van de industrie standaard BitLocker-functie van Windows en de functie DM-Crypt van Linux voor codering van het volume voor het besturingssysteem en de gegevensschijven.

U kunt gebruikmaken van Azure schijf codering te beschermen en de bescherming van uw gegevens voor de beveiliging van de organisatie en aan vereisten voldoen. Organisaties moeten ook overwegen codering te helpen verlichten van de risico's met betrekking tot onbevoegde gegevenstoegang. Het verdient ook stations voor vertrouwelijke gegevens schrijven naar deze te coderen.

Zorg ervoor dat voor het coderen van uw VM gegevensvolumes en opstartvolume ter bescherming van de gegevens in rust in uw account Azure opslag. Bescherming van de coderingssleutels en geheimen door gebruik te maken van [Azure sleutel kluis](../key-vault/key-vault-whatis.md).

Voor uw Windows-Servers op lokalen, kunt u de codering van de volgende aanbevolen procedures:

- [BitLocker](https://technet.microsoft.com/library/dn306081.aspx) gebruiken om gegevens te coderen
- Herstelgegevens in AD DS worden opgeslagen.
- Als er enige bezorgdheid dat BitLocker-sleutels is geknoeid, wordt aangeraden ofwel het station alle exemplaren van de BitLocker-metagegevens verwijderen uit het station te formatteren of u decoderen en het hele station opnieuw te coderen.

Organisaties die niet gegevenscodering afdwingen waarschijnlijk zullen worden blootgesteld aan problemen integriteit, zoals kwaadwillende of rogue gebruikers gegevens stelen en onbevoegde toegang krijgen tot gegevens in een leesbare indeling rekeningen aangetast. Naast deze risico's, moeten bedrijven die voldoen aan de voorschriften van de industrie, moeten bewijzen dat zij prima voor elkaar en de juiste beveiliging besturingselementen gebruikt ter verbetering van de beveiliging van gegevens.

Meer informatie over Azure schijfversleuteling kunt u lezen van het artikel [Azure schijf codering voor Windows en Linux IaaS VMs](azure-security-disk-encryption.md).

## <a name="use-hardware-security-modules"></a>Hardware Security Modules gebruiken

Geheime sleutels codering-oplossingen gebruiken om gegevens te coderen. Het is daarom essentieel dat deze sleutels veilig worden opgeslagen. Sleutelbeheer wordt een integraal onderdeel van de bescherming van gegevens, aangezien het zal worden gebruikt voor het opslaan van geheime sleutels die worden gebruikt om gegevens te coderen.

[Azure sleutel kluis](https://azure.microsoft.com/services/key-vault/) Azure schijf codering gebruikt voor het controleren en beheren van coderingssleutels schijf en geheimen in je abonnement sleutel kluis, terwijl ervoor te zorgen dat alle gegevens in de virtuele machine-schijven in rust in Azure opslag worden gecodeerd. Azure sleutel kluis moet u controleren en het beleid gebruik van sleutels.

Er zijn vele inherente risico's van besturingselementen voor de juiste beveiliging niet hebben om de geheime sleutels die zijn gebruikt voor het coderen van uw gegevens te beschermen. Als kwaadwillende gebruikers toegang tot de geheime sleutels hebben, mogelijk om de gegevens te decoderen en mogelijk toegang hebben tot vertrouwelijke informatie.

U kunt meer informatie over algemene aanbevelingen voor certificaatbeheer in Azure lezen van het artikel [Certificaatbeheer in Azure: Do's en dont](https://blogs.msdn.microsoft.com/azuresecurity/2015/07/13/certificate-management-in-azure-dos-and-donts/).

Lees voor meer informatie over Azure sleutel kluis, [aan de slag met Azure sleutel kluis](../key-vault/key-vault-get-started.md).

## <a name="manage-with-secure-workstations"></a>Met beveiligde werkstations beheren

Aangezien de overgrote meerderheid van de aanvallen als doel de eindgebruiker, wordt het eindpunt in een van de primaire punten van een aanval. Als een aanvaller het eindpunt te storen, kan hij de referenties van de gebruiker toegang krijgen tot de gegevens van de organisatie te benutten. De meeste eindpunt aanvallen kunnen profiteren van het feit dat eindgebruikers beheerders in hun lokale werkstations zijn.

U kunt deze risico's verminderen met behulp van een beheerswerkstation van beveiligde. U wordt aangeraden een [Bevoorrechte toegang werkstations (PAW)](https://technet.microsoft.com/library/mt634654.aspx) ter vermindering van de kwetsbaarheid in werkstations. Deze beveiligde externe werkstations kunt u een paar van deze aanvallen ervoor te zorgen dat uw gegevens veiliger is. Controleer of PAW gebruiken om te beschermen en uw werkstation vergrendelen. Dit is een belangrijke stap in de strenge beveiliging-garanties voor gevoelige accounts, taken en gegevensbescherming te bieden.

Gebrek aan beveiliging van eindpunt risico voor uw gegevens, moet u het beveiligingsbeleid afdwingen op alle apparaten die worden gebruikt voor het verwerken van gegevens, ongeacht de gegevenslocatie (cloud of op locatie).

Voor meer dat informatie over de juiste rechten werkstation toegang door het lezen van het artikel [Bevoorrechte toegang beveiligen](https://technet.microsoft.com/library/mt631194.aspx).

## <a name="enable-sql-data-encryption"></a>SQL-versleuteling van gegevens inschakelen

[De transparante gegevenscodering Azure SQL-Database](https://msdn.microsoft.com/library/dn948096.aspx) (TDE) biedt beveiliging tegen het gevaar van schadelijke activiteiten door middel van real-time codering en decodering van de database, back-ups van gekoppeld en transactielogboekbestanden in rust zonder wijzigingen in de toepassing.  De opslag van een volledige database codeert TDE met behulp van een symmetrische sleutel die de database coderingssleutel genoemd.

Zelfs wanneer de opslagperiode is gecodeerd, is het belangrijk dat ook de database zelf te coderen. Dit is een implementatie van de verdediging in diepte aanpak voor de bescherming van gegevens. Mocht u [Azure SQL-Database](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) gebruikt voor het beschermen van vertrouwelijke gegevens, zoals creditcard of sofi-nummers, kunt u de databases met FIPS 140-2 gevalideerd 256 bits AES-codering die voldoet aan de eisen van veel industriestandaarden (zoals HIPAA, PCI) coderen.

Het is belangrijk te begrijpen dat bestanden die gerelateerd zijn aan [buffer pool extensie](https://msdn.microsoft.com/library/dn133176.aspx) (BPE) niet zijn gecodeerd wanneer een database wordt versleuteld met TDE. Hebt u bestand systeem codering niveau's zoals BitLocker of het [Encrypting File System](https://technet.microsoft.com/library/cc700811.aspx) (EFS) voor BPE gerelateerde bestanden.

Sinds een geautoriseerde gebruiker, zoals een beveiligingsbeheerder of een databasebeheerder toegang tot de gegevens zelfs als de database is versleuteld met TDE, moet u ook volgen de onderstaande aanbevelingen:

- Op het databaseniveau van de SQL-verificatie
- Azure AD-verificatie met behulp van RBAC-rollen
- Gebruikers en toepassingen moeten afzonderlijke accounts gebruiken om te verifiëren. Op deze manier kunt u de machtigingen voor gebruikers en toepassingen te beperken en verminderen de risico's van schadelijke activiteiten
- Implementeren database beveiliging met vaste databaserollen (zoals db_datareader of db_datawriter) of u kunt aangepaste rollen voor de toepassing op expliciete machtigingen verlenen voor objecten in de geselecteerde database maken

Organisaties die geen van codering van de database-niveau gebruikmaken mogelijk gevoelig voor aanvallen waarbij gegevens die zich bevinden in de SQL-databases kunnen binnendringen.

U kunt meer over codering voor SQL TDE [Transparante gegevenscodering met Azure SQL-Database](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx)van het artikel te lezen.

## <a name="protect-data-in-transit"></a>Beveiligen van gegevens onderweg

Bescherming van gegevens tijdens de overdracht moet essentieel onderdeel van uw strategie voor gegevensbescherming. Aangezien gegevens wordt heen en weer worden verplaatst vanaf diverse locaties, is de algemene aanbeveling u altijd SSL/TLS-protocollen gebruiken voor het uitwisselen van gegevens over verschillende locaties. In sommige gevallen kunt u de hele communicatie-kanaal tussen uw lokale en cloud isoleren infrastructuur met behulp van een virtueel particulier netwerk (VPN).

Gegevens verplaatsen tussen uw infrastructuur op gebouwen en Azure, moet u rekening houden met passende beveiligingsmaatregelen zoals HTTPS of VPN.

Voor organisaties die behoefte hebben aan het beveiligen van de toegang van meerdere werkstations gebruiken bevindt op-ruimten naar Azure, [Azure VPN van site tot site](../vpn-gateway/vpn-gateway-site-to-site-create.md).

Voor organisaties die behoefte hebben aan veilige toegang tot een werkstation dat zich bevindt op ruimten naar Azure, [Point-to-Site VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md).

Grotere sets kunnen worden verplaatst via een speciale snelle WAN gegevenskoppeling zoals [ExpressRoute](https://azure.microsoft.com/services/expressroute/). Als u kiest voor ExpressRoute, kunt u ook de gegevens op het toepassingsniveau coderen met behulp van [SSL/TLS](https://support.microsoft.com/kb/257591) - of andere protocollen voor extra bescherming.

Als u opslag via de Portal Azure Azure worden gewerkt, worden alle transacties plaats via HTTPS. [Opslag REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx) via HTTPS kan ook worden gebruikt voor de interactie met [Azure opslag](https://azure.microsoft.com/services/storage/) en [Azure SQL-Database](https://azure.microsoft.com/services/sql-database/).

Organisaties die niet aan het beveiligen van gegevens onderweg zijn gevoeliger voor [man-in-the-middle-aanvallen](https://technet.microsoft.com/library/gg195821.aspx), [afluisteren](https://technet.microsoft.com/library/gg195641.aspx) en sessie kaping van. Deze aanvallen zijn de eerste stap bij het verkrijgen van toegang tot vertrouwelijke gegevens.

Meer informatie over Azure VPN-optie kunt u lezen van het artikel, [Planning en ontwerp voor een VPN-Gateway](../vpn-gateway/vpn-gateway-plan-design.md).

## <a name="enforce-file-level-data-encryption"></a>Bestand niveau gegevenscodering worden afgedwongen.

Een andere laag van bescherming die het niveau van beveiliging voor uw gegevens kunt verbeteren is het bestand zelf, ongeacht de locatie van het bestand te coderen.

[Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) maakt gebruik van codering, identiteit en vergunning voor de beveiliging van uw bestanden en e-mailadres. Azure RMS werkt via meerdere apparaten, telefoons, tablets en pc's door te beschermen, zowel binnen uw organisatie en buiten uw organisatie. Dit is mogelijk omdat de RMS Azure voegt een niveau van bescherming dat nog met de gegevens, zelfs wanneer de grenzen van uw organisatie verlaat.

Wanneer u uw bestanden beveiligen met RMS Azure, gebruikt u standaard-cryptografie met volledige ondersteuning van [FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/standards.html). Wanneer u gebruik wilt RMS Azure voor gegevensbescherming maken, hebben de zekerheid dat de bescherming van het bestand blijft, zelfs als deze wordt gekopieerd naar de opslag die niet onder het beheer van IT, zoals een cloud storage-service. Hetzelfde gebeurt voor bestanden die worden gedeeld via e-mail, het bestand is beveiligd, als bijlage bij een e-mailbericht, met instructies voor het openen van de beschermde bijlage.

Bij de planning voor de aanneming van Azure RMS wordt aangeraden de volgende opties:

- Installeer de [RMS app delen](https://technet.microsoft.com/library/dn339006.aspx). Deze toepassing is geïntegreerd met Office toepassingen door het installeren van een Office-invoegtoepassing zodat gebruikers eenvoudig van bestanden rechtstreeks beveiligen kunnen.
- Toepassingen en services voor de ondersteuning van Azure RMS configureren
- [Aangepaste sjablonen](https://technet.microsoft.com/library/dn642472.aspx) aan uw zakelijke vereisten te maken. Bijvoorbeeld: een sjabloon voor top geheime gegevens die moet worden toegepast bij alle top secret gerelateerde e-mails.

Organisaties die op de [indeling van gegevens](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) en bestand beveiliging zwak zijn mogelijk gevoeliger zijn voor gegevens lekken. Zonder juiste bescherming, organisaties niet mogelijk om meer inzicht te verkrijgen, controleren op misbruik en voorkomen dat schadelijke toegang tot bestanden.

U kunt meer over Azure RMS lezen van het artikel [Aan de slag met Azure Rights Management](https://technet.microsoft.com/library/jj585016.aspx).
