<properties
   pageTitle="Een Service Fabric '-cluster beveiligen | Microsoft Azure"
   description="Beschrijft de security-scenario's voor een Service Fabric '-cluster en verschillende technologieën voor het implementeren van deze scenario's."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/19/2016"
   ms.author="chackdan"/>

# <a name="service-fabric-cluster-security-scenarios"></a>Service Fabric cluster scenario's voor netwerkbeveiliging

Een cluster Service Fabric is een bron die u bezit. Clusters moeten altijd worden beveiligd om te voorkomen dat onbevoegde gebruikers verbinding maken met uw cluster, met name wanneer deze productie werklasten uitgevoerd heeft. Hoewel het mogelijk maken een onbeveiligde cluster is, kan doen dus elke anonieme gebruiker tot stand te brengen als de eindpunten management met het openbare Internet worden getoond. 

Dit artikel bevat een overzicht van de scenario's voor netwerkbeveiliging voor clusters op Azure of zelfstandige en de verschillende technologieën voor het implementeren van deze scenario's worden gebruikt. De cluster security-scenario's:

- Knooppunt naar beveiliging
- Beveiliging van client-naar-knooppunt
- Op rollen gebaseerde toegangscontrole RBAC)

## <a name="node-to-node-security"></a>Knooppunt naar beveiliging
Communicatie tussen de computers in het cluster of VMs beveiligt. Dit zorgt ervoor dat alleen computers die gemachtigd zijn deel te nemen aan het cluster kunnen deelnemen aan die toepassingen en services in het cluster als host optreedt.

![Diagram van knooppunt-knooppuntcommunicatie][Node-to-Node]

Clusters op Azure of zelfstandige clusters met Windows kunt [Certificaatbeveiliging](https://msdn.microsoft.com/library/ff649801.aspx) of de [Windows-](https://msdn.microsoft.com/library/ff649396.aspx) computers met Windows Server.
### <a name="node-to-node-certificate-security"></a>Knooppunt naar Certificaatbeveiliging
Fabric-service maakt gebruik van x.509-certificaten die u als onderdeel van het type knooppunt configuraties opgeeft wanneer u een cluster maakt. Een beknopt overzicht van wat deze certificaten zijn en hoe verwerven of ze maken wordt aan het einde van dit artikel geleverd.

Certificaatbeveiliging is tijdens het maken van het cluster via de portal Azure, Azure Resource Manager sjablonen of een zelfstandige JSON-sjabloon geconfigureerd. U kunt een primaire certificaat en een optionele tweede certificaat dat wordt gebruikt voor rollovers certificaat. De primaire en secundaire certificaten die u opgeeft moeten afwijken van de admin-client en de alleen-lezen clientcertificaten u voor de [Client tot het knooppunt beveiliging](#client-to-node-security).

Lees het [instellen van een cluster met behulp van een sjabloon Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) voor meer informatie over het configureren van Certificaatbeveiliging in een cluster voor Azure.

Voor zelfstandige lezen Windows Server [een autonome cluster op Windows met behulp van x.509-certificaten beveiligen](service-fabric-windows-cluster-x509-security.md)

### <a name="node-to-node-windows-security"></a>Knooppunt naar windows-beveiliging
Voor zelfstandige lezen Windows Server [een autonome cluster op Windows met behulp van Windows-beveiliging veilig](service-fabric-windows-cluster-windows-security.md)

## <a name="client-to-node-security"></a>Beveiliging van client-naar-knooppunt
Clients worden geverifieerd en de communicatie tussen een client en de afzonderlijke knooppunten in het cluster worden beveiligd. Dit type beveiliging wordt geverifieerd en waarbij clientcommunicatie, die zorgt ervoor dat alleen geautoriseerde gebruikers toegang hebben tot het cluster en de toepassingen die worden gebruikt in het cluster. Clients zijn uniek aangeduid door middel van hun Windows-beveiliging-referenties of de beveiligingsreferenties van het certificaat.

![Diagram van client-knooppuntcommunicatie][Client-to-Node]

Clusters op Azure of zelfstandige clusters met Windows kunnen [Certificaatbeveiliging](https://msdn.microsoft.com/library/ff649801.aspx) of [Windows-beveiliging](https://msdn.microsoft.com/library/ff649396.aspx)gebruiken.

### <a name="client-to-node-certificate-security"></a>Van de client tot het knooppunt Certificaatbeveiliging
 Van de client tot het knooppunt Certificaatbeveiliging is tijdens het maken van het cluster door de Azure portal Resource Manager-sjablonen of een zelfstandige JSON-sjabloon door op te geven van een clientcertificaat admin en/of een clientcertificaat van de gebruiker geconfigureerd.  Admin client- en clientcertificaten die u opgeeft moet verschillen van de primaire en secundaire certificaten die u voor de [beveiliging van knooppunt naar knooppunt opgeeft](#node-to-node-security).

Clients die verbinding maken met het cluster via de admin-certificaat hebben volledige toegang tot de beheerfuncties.  Clients die verbinding maken met het cluster met behulp van het clientcertificaat van de gebruiker alleen-lezen hebben alleen leestoegang tot de beheerfuncties. Met andere woorden worden deze certificaten gebruikt voor de rol van basen toegangsbeheer (RBAC) die verderop in dit artikel wordt beschreven.

Lees het [instellen van een cluster met behulp van een sjabloon Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) voor meer informatie over het configureren van Certificaatbeveiliging in een cluster voor Azure.

Voor zelfstandige lezen Windows Server [een autonome cluster op Windows met behulp van x.509-certificaten beveiligen](service-fabric-windows-cluster-x509-security.md)

### <a name="client-to-node-azure-active-directory-aad-security-on-azure"></a>Beveiliging van client naar knooppunt Azure Active Directory (AAD) op Azure
Clusters op Azure kunnen ook toegang tot het beheer van eindpunten via Azure Active Directory (AAD) beveiligen. Zie [een cluster door met behulp van een sjabloon Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) voor meer informatie over het maken van de nodige AAD artefacten, hoe deze tijdens het maken van het cluster te vullen en daarna verbinding maken met deze clusters.

## <a name="security-recommendations"></a>Aanbevelingen voor beveiliging
Voor clusters met Azure verdient u AAD security gebruiken voor de verificatie van clients en certificaten voor de beveiliging van knooppunt naar knooppunt.

Voor zelfstandige Windows Server beheerd clusters, verdient het gebruik van Windows-beveiliging met een groep accounts (GMA) als u Windows Server 2012 R2 en Active Directory. Anders blijft Windows-beveiliging gebruiken met Windows-accounts.

## <a name="role-based-access-control-rbac"></a>Toegangsbeheer (RBAC) op basis van rollen
Met toegangsbeheer kunt Clusterbeheer voor de toegang beperken tot bepaalde clusterbewerkingen voor verschillende groepen gebruikers, het veiliger maken van het cluster. Twee verschillende access control-typen worden ondersteund voor clients die verbinding maken met een cluster: beheerdersrol en gebruikersrol.

Beheerders hebben volledige toegang tot de beheerfuncties (met inbegrip van mogelijkheden voor lezen/schrijven). Gebruikers hebben standaard alleen leestoegang tot de beheerfuncties (bijvoorbeeld querymogelijkheden) en de mogelijkheid om op te lossen, toepassingen en services.

U opgeven de rollen beheerder en gebruiker die client tijdens het maken van het cluster door middel van afzonderlijke identiteiten (certificaten, AAD enz.) voor elk. Zie [toegangsbeheer voor Service Fabric-clients op basis van rollen](service-fabric-cluster-security-roles.md)voor meer informatie over de standaardinstellingen voor toegangsbeheer en het wijzigen van de standaardinstellingen.


## <a name="x509-certificates-and-service-fabric"></a>X.509-certificaten en Service Fabric
Digitale x.509-certificaten worden meestal gebruikt voor het verifiëren van clients en servers te coderen en digitaal ondertekenen van berichten. Ga naar het [werken met certificaten](http://msdn.microsoft.com/library/ms731899.aspx)voor meer informatie over deze certificaten.

Enkele belangrijke overwegingen:

- Certificaten die worden gebruikt in clusters met productie werklast moeten worden gemaakt met behulp van een juist geconfigureerde Windows Server certificaatservice of verkregen van een erkende [Certificeringsinstantie (CA)](https://en.wikipedia.org/wiki/Certificate_authority).
- Gebruik nooit een tijdelijk en certificaten in de productie die zijn gemaakt met hulpmiddelen zoals MakeCert.exe testen.
- U kunt een zelfondertekend certificaat gebruiken, maar u moet alleen doen voor test-clusters en niet in productie.

### <a name="server-x509-certificates"></a>X.509-certificaten

Servercertificaten zijn de belangrijkste taak van een server (knooppunt) om clients te verifiëren of de verificatie van een server (knooppunt) naar een server (knooppunt). Een van de eerste controles wanneer een knooppunt wordt geverifieerd door een client of het knooppunt is als u de waarde van de algemene naam in het veld onderwerp. Deze algemene naam of een van de certificaten alternatieve namen van certificaathouders moet aanwezig zijn in de lijst met toegestane algemene namen.

Het volgende artikel wordt beschreven hoe u certificaten met alternatieve namen voor onderwerp (SAN): [een alternatieve naam voor onderwerp naar een beveiligde LDAP-certificaat toevoegen](http://support.microsoft.com/kb/931351).

Het veld onderwerp kan meerdere waarden bevatten, elk met een initialisatie geven het waardetype het voorvoegsel. Meestal is de initialisatie van de 'CN' voor gangbare naam. bijvoorbeeld "CN = www.contoso.com". Het is ook mogelijk om het onderwerpveld leeg is. Als de optionele alternatieve naam Subject-veld is ingevuld, moet deze bevatten zowel de algemene naam van het certificaat en één post per alternatieve naam voor onderwerp. Deze worden ingevoerd als waarden van de DNS-naam.

De waarde van het veld beoogde doeleinden van het certificaat moet een geschikte waarde, zoals 'Authentication Server' of 'Client Authentication' bevatten.

### <a name="client-x509-certificates"></a>X.509-certificaten

Certificaten worden meestal niet uitgegeven door een certificeringsinstantie van derden. In plaats daarvan bevat het persoonlijke archief van de huidige gebruikerslocatie gewoonlijk clientcertificaten die daar worden geplaatst door een basiscertificeringsinstantie met het beoogde doeleinde 'Client Authentication'. De client kan dit certificaat gebruiken als wederzijdse verificatie vereist is.

>[AZURE.NOTE] Alle beheertaken uit te voeren op een cluster Service Fabric vereist servercertificaat. Clientcertificaten kunnen niet worden gebruikt voor beheer.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->


## <a name="next-steps"></a>Volgende stappen

Dit artikel bevat algemene informatie over de beveiliging van het cluster. Volgende, [maken van een cluster in Azure met behulp van de sjabloon voor een Resource Manager](service-fabric-cluster-creation-via-arm.md) of via de [portal Azure](service-fabric-cluster-creation-via-portal.md).

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png
