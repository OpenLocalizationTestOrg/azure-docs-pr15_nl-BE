<properties
   pageTitle="Overzicht van beveiliging in het gegevensarchief Lake | Microsoft Azure"
   description="Begrijpen hoe Azure Lake gegevensarchief is een veiligere grote gegevensopslag"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/02/2016"
   ms.author="nitinme"/>

# <a name="security-in-azure-data-lake-store"></a>Beveiliging in Azure Lake gegevensarchief

Vele ondernemingen nemen profiteren van big data analytics voor zakelijke inzichten om slimme beslissingen te. Een organisatie kan een complexe en gereglementeerde omgeving, met een toenemend aantal diverse gebruikers hebben. Het is essentieel voor een onderneming om er zeker van te zijn dat bedrijfskritieke gegevens veiliger, met het juiste toegangsniveau verleend aan individuele gebruikers opgeslagen. Azure Lake gegevensarchief is ontworpen om te voldoen aan deze beveiligingsvereisten voldoen. In dit artikel meer informatie over de beveiligingsmogelijkheden van Lake gegevensarchief, met inbegrip van:

* Verificatie
* Autorisatie
* Netwerkisolatie
* Bescherming van gegevens
* Controle

## <a name="authentication-and-identity-management"></a>Verificatie en identity management

Verificatie is het proces waarmee de identiteit van een gebruiker is geverifieerd als de gebruiker werkt met Lake gegevensarchief of een service die is verbonden met het gegevensarchief Lake. Voor de verificatie en identiteitsbeheer gegevensarchief Lake [Azure Active Directory](../active-directory/active-directory-whatis.md), een uitgebreide identiteit en toegang cloud oplossing voor beheer het beheer van gebruikers en groepen vereenvoudigt.

Elk abonnement Azure kan worden gekoppeld aan een exemplaar van Azure Active Directory. Alleen gebruikers en service-identiteiten die zijn gedefinieerd in uw Azure Active Directory service toegang tot uw account gegevensarchief Lake met behulp van opdrachtregelprogramma's, de Azure portal of via toepassingen die uw organisatie stelt samen met Azure Data Lake winkel SDK. Belangrijkste voordelen van het gebruik van Azure Active Directory als een centraal mechanisme voor toegangsbeheer zijn:

* Vereenvoudigde identity lifecycle management. De identiteit van een gebruiker of een service (een principal-id) kan snel worden gemaakt en snel ingetrokken door gewoon verwijderen of uitschakelen van de account in Active directory.

* Meerledige verificatie. [Meerledige verificatie](../multi-factor-authentication/multi-factor-authentication.md) biedt een extra beveiligingslaag voor aanmeldingen van gebruikers en transacties.

* Verificatie van een client via een open standaard protocol, zoals OAuth of OpenID.

* Federatie met adreslijstservices enterprise en cloud id-providers.

## <a name="authorization-and-access-control"></a>Verificatie en toegangsbeheer

Nadat een gebruiker wordt geverifieerd zodat de gebruiker toegang heeft tot Azure Lake gegevensarchief in Azure Active Directory, toegangsmachtigingen vergunning besturingselementen voor gegevensopslag Lake. Lake gegevensarchief scheidt vergunning voor account gerelateerd en data activiteiten op de volgende wijze:

* [Toegangsbeheer op basis van rollen](../active-directory/role-based-access-control-what-is.md) (RBAC) wordt geleverd door Azure voor accountbeheer
* POSIX ACL voor toegang tot gegevens in de winkel


### <a name="rbac-for-account-management"></a>RBAC voor accountbeheer

Vier elementaire functies zijn voor gegevensopslag Lake standaard gedefinieerd. De rollen kunnen verschillende bewerkingen op een gegevensarchief Lake account via de portal Azure, PowerShell-cmdlets en REST API's. De eigenaar en de inzender rollen kunnen tal van functies voor beheer uitvoeren op de rekening. U kunt de rol van lezer toewijzen aan gebruikers die alleen met gegevens kunnen werken.

![RBAC-rollen] (./media/data-lake-store-security-overview/rbac-roles.png "RBAC-rollen")

Opmerking Hoewel rollen zijn toegewezen voor accountbeheer, sommige rollen toegang tot gegevens beïnvloeden. U moet toegang tot activiteiten die een gebruiker op het bestandssysteem uitvoeren kan met ACL's. De volgende tabel bevat een overzicht van het beheer van beheerrechten en -gegevens van toegangsrechten voor de standaardrollen.

| Rollen                    | Rights management               | Toegangsrechten voor gegevens | Uitleg |
| ------------------------ | ------------------------------- | ------------------ | ----------- |
| Geen rol toegewezen         | Geen                            | Vallende ACL    | De gebruiker niet de Azure portal of Azure PowerShell-cmdlets gebruiken om te bladeren Lake gegevensarchief. De gebruiker kan alleen de opdrachtregelprogramma's gebruiken.
| Eigenaar  | Alle  | Alle  | De rol eigenaar is een supergebruiker. Deze rol alles kunt beheren en volledige toegang heeft tot gegevens.
| Reader   | Alleen-lezen  | Vallende ACL    | De rol van lezer kunt Alles bekijken met betrekking tot accountmanagement, zoals welke gebruiker welke rol is toegewezen. De rol van lezer kunt u geen wijzigingen aanbrengen.   |
| Inzender              | Alles behalve rollen toevoegen en verwijderen | Vallende ACL    | De rol van inzender kan bepaalde aspecten van een account, zoals implementaties en bij het maken en beheren van waarschuwingen beheren. De rol van inzender kan toevoegen of verwijderen van rollen.
| Beheerder toegang | Toevoegen en verwijderen van rollen            | Vallende ACL    | De rol beheerder toegang kan gebruikerstoegang tot accounts beheren. |

Zie [gebruikers of beveiligingsgroepen aan Lake gegevensarchief accounts toewijzen](data-lake-store-secure-data.md#assign-users-or-security-groups-to-azure-data-lake-store-accounts)voor meer informatie.

### <a name="using-acls-for-operations-on-file-systems"></a>Met behulp van ACL's voor bewerkingen voor bestandssystemen

Gegevensarchief Lake is een hiërarchisch bestandssysteem zoals Hadoop Distributed bestand System (HDFS) en biedt ondersteuning voor [POSIX-ACL's](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Hiermee kunt u lezen (r), schrijven (w) en uitvoeren (x) machtigingen voor bronnen voor de rol van de eigenaar, de groep Eigenaren van en voor andere gebruikers en groepen. In de gegevens Lake winkel Public Preview (de huidige versie), ACL's voor de root-map, submappen en afzonderlijke bestanden ingeschakeld. De ACL's die u op de hoofdmap toepast zijn ook van toepassing op alle onderliggende mappen en bestanden.

U wordt aangeraden de ACL's te definiëren voor meerdere gebruikers met behulp van [beveiligingsgroepen](../active-directory/active-directory-accessmanagement-manage-groups.md). Gebruikers toevoegen aan een beveiligingsgroep, en vervolgens de ACL's voor een bestand of map toewijzen aan die beveiligingsgroep. Dit is handig als u toegang verlenen aangepast, wilt omdat u zich beperkt tot een maximum van negen vermeldingen voor aangepaste toegang toe te voegen. Zie [gebruikers toewijzen of beveiligingsgroep als ACL's naar het bestandssysteem Azure Lake gegevensarchief](data-lake-store-secure-data.md#filepermissions)voor meer informatie over het beter beveiligde gegevens opgeslagen in het gegevensarchief Lake met Azure Active Directory-beveiligingsgroepen.

![Lijst met standaard- en aangepaste toegang] (./media/data-lake-store-security-overview/adl.acl.2.png "Lijst met standaard- en aangepaste toegang")

## <a name="network-isolation"></a>Netwerkisolatie

Gebruik Lake gegevensarchief voor toegang tot uw gegevens opslaan op het netwerkniveau van het te bepalen. U kunt tot stand brengen van firewalls en een IP-adresbereik definiëren voor uw vertrouwde clients. Met een IP-adresbereik alleen clients die een IP-adres binnen het gedefinieerde bereik kunnen verbinden met Lake gegevensarchief.

![Firewall-instellingen en de IP-toegang] (./media/data-lake-store-security-overview/firewall-ip-access.png "Firewall-instellingen en het IP-adres")

## <a name="data-protection"></a>Bescherming van gegevens

Organisaties wilt ervoor te zorgen dat hun cruciale gegevens beveiligen gedurende de gehele levenscyclus. Voor de gegevens tijdens de overdracht gegevensarchief Lake het standaardprotocol Transport Layer Security (TLS) voor beveiliging van gegevens die tussen een client en een gegevensarchief Lake verplaatst.

Gegevensbescherming voor gegevens in rust zijn beschikbaar in toekomstige versies.

## <a name="auditing-and-diagnostic-logs"></a>Controle en diagnostische logboeken

U kunt controleren of de diagnostische logboeken, afhankelijk van of u op zoek naar Logboeken voor management activiteiten of activiteiten die betrekking hebben op gegevens.

*  Management gerelateerde activiteiten Azure Resource Manager API's gebruiken en via de controlelogboeken in Azure portal zijn opgehaald.
*  Gegevens gerelateerde activiteiten met WebHDFS REST API's en via diagnostische logboeken in Azure portal zijn opgehaald.

### <a name="auditing-logs"></a>Controlelogboeken

Om te voldoen aan regelgeving, mogelijk een organisatie voldoende audittrails als nodig is om u te verdiepen in specifieke incidenten. Lake gegevensopslag heeft ingebouwde monitoring en controle, en registreert alle account management activiteiten.

Voor account management audit-registratie weergeven en kies de kolommen die u wilt vastleggen. U kunt ook controlelogboeken exporteren naar Azure opslag.

![Controlelogboeken] (./media/data-lake-store-security-overview/audit-logs.png "Controlelogboeken")

### <a name="diagnostic-logs"></a>Diagnoselogboeken

U kunt data access audittrails in Azure portal (in diagnostische instellingen) en maak een Azure Blob-opslag account waarin de logboekbestanden worden opgeslagen.

![Diagnoselogboeken] (./media/data-lake-store-security-overview/diagnostic-logs.png "Diagnoselogboeken")

Nadat u diagnostische instellingen configureren, kunt u de logboeken op het tabblad **Diagnostische logboeken** bekijken.

Zie voor meer informatie over het werken met diagnostische logboeken met Azure Lake gegevensarchief [Diagnostische logboeken voor gegevensopslag Lake](data-lake-store-diagnostic-logs.md).

## <a name="summary"></a>Samenvatting

Zakelijke klanten vragen een analytics cloud platform dat is veilig en gemakkelijk te gebruiken. Azure Lake gegevensarchief is ontworpen om te helpen deze eisen door middel van identiteitsbeheer en verificatie via Azure Active Directory-integratie, autorisatie op basis van de ACL, netwerkisolatie, gegevenscodering in transit en op rusten (binnenkort in de toekomst) adres en controle.

Als u zien van de nieuwe functies in het gegevensarchief Lake wilt, stuur ons uw feedback in het [forum gegevens Lake winkel UserVoice](https://feedback.azure.com/forums/327234-data-lake).

## <a name="see-also"></a>Zie ook

- [Overzicht van Azure Lake gegevensarchief](data-lake-store-overview.md)
- [Aan de slag met Lake gegevensarchief](data-lake-store-get-started-portal.md)
- [Beveiliging van gegevens in het gegevensarchief Lake](data-lake-store-secure-data.md)
