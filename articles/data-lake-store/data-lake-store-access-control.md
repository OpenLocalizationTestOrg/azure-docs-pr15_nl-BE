<properties
   pageTitle="Overzicht van toegangsbeheer in het gegevensarchief Lake | Microsoft Azure"
   description="Begrijpen hoe toegangsbeheer in Azure Lake gegevensarchief"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/06/2016"
   ms.author="nitinme"/>

# <a name="access-control-in-azure-data-lake-store"></a>Toegangsbeheer in Azure Lake gegevensarchief

Lake gegevensarchief implementeert een model voor toegangsbeheer die is afgeleid van HDFS en op zijn beurt weer van het toegangsbeheermodel POSIX. In dit artikel worden de basisbeginselen van het model voor gegevensopslag Lake voor toegangsbeheer. Voor meer informatie over de HDFS toegangsbeheer Zie model [HDFS machtigingen Guide](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).

## <a name="access-control-lists-on-files-and-folders"></a>ACL's voor bestanden en mappen

Er zijn twee soorten toegang toegangsbeheerlijsten (ACL's) - **ACL's van Access** en **Standaard ACL's**.

* **Access-ACL's** – deze controle toegang tot een object. Hebben de ACL's voor toegang tot bestanden en mappen.

* **Standaard-ACL's** – 'Sjabloon' van ACL's die zijn gekoppeld aan een map met de Access-ACL's voor alle onderliggende items gemaakt onder die map bepalen. Bestanden hebben geen standaard ACL's.

![ACL's Lake gegevensarchief](./media/data-lake-store-access-control/data-lake-store-acls-1.png)

Zowel Access ACL's standaard ACL's hebben dezelfde structuur.

![ACL's Lake gegevensarchief](./media/data-lake-store-access-control/data-lake-store-acls-2.png)

>[AZURE.NOTE] Wijzigen van de standaard-ACL op een bovenliggend element heeft geen invloed op de toegang tot of de standaard ACL van onderliggende items die al bestaan.

## <a name="users-and-identities"></a>Gebruikers en identiteiten

Elk bestand en elke map afzonderlijk machtigingen heeft voor deze identiteiten:

* De gebruiker die eigenaar van het bestand
* De groep die eigenaar
* Benoemde gebruikers.
* Benoemde groepen
* Alle andere gebruikers

De identiteit van gebruikers en groepen worden identiteiten Azure Active Directory (AAD) dus tenzij anders aangegeven per "gebruiker", in het kader van het gegevensarchief Lake, ofwel zou bijvoorbeeld een AAD-gebruiker of een beveiligingsgroep AAD.

## <a name="permissions"></a>Machtigingen

De machtigingen voor een bestandssysteemobject zijn **lezen**, **schrijven**, en **uitvoeren** en ze kunnen worden gebruikt voor bestanden en mappen, zoals wordt weergegeven in de onderstaande tabel.

|            |    Bestand     |   Map |
|------------|-------------|----------|
| **Read (R)** | De inhoud van een bestand kunnen lezen | Moet **lezen** en **uitvoeren** om de inhoud van de map.|
| **Schrijven (W)** | Kan schrijven of aan een bestand toevoegen | Moet **schrijven en uitvoeren** om onderliggende items in een map. |
| **Uitvoeren (X)** | Betekent niet dat alles in het kader van het gegevensarchief Lake | Nodig om de onderliggende items van een map te bladeren. |

### <a name="short-forms-for-permissions"></a>Korte formulieren voor machtigingen

**RWX**wordt gebruikt om aan te geven, **lezen + schrijven en uitvoeren**. Een numerieke versmalde formulier bestaat waarin **lezen = 4**, **schrijven = 2**, en **Execute = 1** en hun som vertegenwoordigt de machtigingen. Hieronder volgen enkele voorbeelden.

| Numerieke vorm | Korte vorm |      Wat het betekent     |
|--------------|------------|------------------------|
| 7            | RWX        | Lezen + schrijven en uitvoeren |
| 5            | R-X        | Lezen en uitvoeren         |
| 4            | R--        | Lezen                   |
| 0            | ---        | Geen machtigingen         |


### <a name="permissions-do-not-inherit"></a>Geen overnemen machtigingen

In het POSIX-stijl model gebruikt door Lake gegevensarchief, worden machtigingen voor een item op het item zelf opgeslagen. Met andere woorden, kunnen niet machtigingen voor een item worden overgenomen van de bovenliggende items.

## <a name="common-scenarios-related-to-permissions"></a>Gebruikelijke scenario's die verband houden met machtigingen

Hier volgen enkele algemene scenario's om te begrijpen welke machtigingen nodig zijn voor het uitvoeren van bepaalde acties op een account Lake gegevensarchief.

### <a name="permissions-needed-to-read-a-file"></a>Machtigingen die nodig zijn om een bestand te lezen

![ACL's Lake gegevensarchief](./media/data-lake-store-access-control/data-lake-store-acls-3.png)

* De beller moet het bestand te lezen - machtigingen voor **lezen**
* Voor alle mappen in de mapstructuur waarin het bestand - moet de aanroeper machtigingen voor **uitvoeren**

### <a name="permissions-needed-to-append-to-a-file"></a>Machtigingen die nodig zijn om toe te voegen aan een bestand

![ACL's Lake gegevensarchief](./media/data-lake-store-access-control/data-lake-store-acls-4.png)

* Voor het bestand moet worden toegevoegd aan - moet de aanroeper **schrijfmachtigingen**
* Voor de mappen waarin het bestand - moet de aanroeper machtigingen voor **uitvoeren**

### <a name="permissions-needed-to-delete-a-file"></a>Machtigingen die nodig zijn om een bestand te verwijderen

![ACL's Lake gegevensarchief](./media/data-lake-store-access-control/data-lake-store-acls-5.png)

* Voor de bovenliggende map - moet de aanvrager de machtigingen **schrijven + uitvoeren**
* Voor alle andere mappen in het pad van het bestand - moet de aanroeper machtigingen voor **uitvoeren**

>[AZURE.NOTE] Schrijf de machtigingen voor het bestand is niet vereist voor het bestand te verwijderen als de twee bovenstaande voorwaarden voldaan wordt.

### <a name="permissions-needed-to-enumerate-a-folder"></a>Machtigingen die nodig zijn voor een map opsommen

![ACL's Lake gegevensarchief](./media/data-lake-store-access-control/data-lake-store-acls-6.png)

* De beller moet de map opsommen - **Read + Execute** -machtigingen
* De aanvrager moet alle bovenliggende mappen - machtigingen tot **uitvoeren**

## <a name="viewing-permissions-in-the-azure-portal"></a>Machtigingen weergeven in de portal voor Azure

Klik op **toegang** om te zien de ACL's voor een bestand of map uit het gegevensarchief Lake-account **Data Explorer** blade. In het screenshot hieronder, klik op overzicht van de ACL's voor de map **catalogus** onder de account **mydatastore** .

![ACL's Lake gegevensarchief](./media/data-lake-store-access-control/data-lake-store-show-acls-1.png)

Daarna klikt u op **Eenvoudige weergave** voor eenvoudiger weergave uit de **Access** -blade.

![ACL's Lake gegevensarchief](./media/data-lake-store-access-control/data-lake-store-show-acls-simple-view.png)

Klik op **Weergave Geavanceerd** om de geavanceerde weergave te bekijken.

![ACL's Lake gegevensarchief](./media/data-lake-store-access-control/data-lake-store-show-acls-advance-view.png)

## <a name="the-super-user"></a>De super-gebruiker

Een super-gebruiker heeft de meeste rechten van alle gebruikers in het gegevensarchief Lake. Een super-gebruiker:

* RWX machtigingen heeft voor **alle** bestanden en mappen
* kan de machtigingen wijzigen voor een bestand of map.
* kunt de eigenaar-gebruiker of -groep die eigenaar van een bestand of map wijzigen.

In Azure heeft een gegevensarchief Lake account meerdere Azure rollen:

* Eigenaren
* Medewerkers
* Lezers
* Enz.

Iedereen in de rol van de **eigenaars** voor een gegevensarchief Lake-account wordt automatisch een supergebruiker voor die account. Meer over Azure rol op basis van Access Control (RBAC) Zie [functies gebaseerd toegangsbeheer](../active-directory/role-based-access-control-configure.md).

## <a name="the-owning-user"></a>De gebruiker die eigenaar

De gebruiker die het item heeft gemaakt, wordt automatisch de gebruiker die eigenaar van het item. Een gebruiker die eigenaar kan:

* De machtigingen wijzigen van een bestand dat eigendom is
* De groep die eigenaar van een bestand dat eigendom is gewijzigd als de gebruiker die eigenaar ook een lid van de doelgroep is.

>[AZURE.NOTE] De eigenaar gebruiker **niet kan** wijzigen de gebruiker die eigenaar van het bestand met een andere eigenaar. Alleen controle gebruikers kunnen de gebruiker die eigenaar van een bestand of map wijzigen.

## <a name="the-owning-group"></a>De groep die eigenaar

Elke gebruiker is in het POSIX-ACL's gekoppeld aan een "primaire groep". "Lisa" gebruikers kan bijvoorbeeld behoren tot de groep 'finance'. Alice kan tot meerdere groepen behoren, maar één groep is altijd aangewezen als haar primaire groep. Wanneer Lisa een bestand maakt, wordt de groep die eigenaar van het bestand in POSIX ingesteld op haar primaire groep, in dit geval is 'finance'.
 
Wanneer een nieuw bestandssysteem-item wordt gemaakt, Lake gegevensarchief een waarde toegewezen aan de groep die eigenaar. 

* **Geval 1** - de hoofdmap "/". Deze map wordt gemaakt wanneer een gegevensarchief Lake-account is gemaakt. In dit geval is de groep die eigenaar ingesteld voor de gebruiker die de account heeft gemaakt.
* **Voorbeeld 2** (alle andere gevallen) - wanneer een nieuw item wordt gemaakt, de groep die eigenaar is overgenomen van de bovenliggende map.

De groep die eigenaar kan worden gewijzigd door:
* Gebruikers controle
* De eigenaar gebruiker als de gebruiker die eigenaar ook een lid van de doelgroep is.

## <a name="access-check-algorithm"></a>Access controle algoritme

In de volgende afbeelding geeft de toegang controle algoritme voor gegevensopslag Lake accounts.

![Lake gegevensarchief ACL's algoritme](./media/data-lake-store-access-control/data-lake-store-acls-algorithm.png)


## <a name="the-mask-and-effective-permissions"></a>Het masker en de 'effectieve machtigingen'

Het **masker** is een RWX waarde die wordt gebruikt om toegang te beperken voor **gebruikers met de naam**, de **eigenaar van de groep**en **benoemde groepen** bij het uitvoeren van toegangscontrole-algoritme. Hier vindt u de basisbegrippen voor het masker. 

* Het masker wordt gemaakt 'effectieve machtigingen', dat wil zeggen, worden de machtigingen gewijzigd ten tijde van de toegangscontrole.
* Het masker kan rechtstreeks worden bewerkt door de bestandseigenaar van het en eventuele controle gebruikers.
* Het masker is de mogelijkheid om machtigingen voor het maken van de effectieve machtigingen te verwijderen. Het masker **kan geen** toevoegen machtigingen aan de effectieve machtigingen. 

Laten we kijken naar enkele voorbeelden. Het masker wordt hieronder, **RWX**, wat betekent dat het masker verwijdert niet de machtigingen die zijn ingesteld. U ziet dat de effectieve machtigingen voor naam gebruiker en groep die eigenaar benoemde groep niet worden gewijzigd tijdens de toegangscontrole.

![ACL's Lake gegevensarchief](./media/data-lake-store-access-control/data-lake-store-acls-mask-1.png)

In het volgende voorbeeld wordt het masker ingesteld op **R-X**. Ja, deze **schakelt u de machtiging schrijven** voor **gebruiker met de naam** **groep die eigenaar is**en de **groep met de naam** op het moment van toegang controleren.

![ACL's Lake gegevensarchief](./media/data-lake-store-access-control/data-lake-store-acls-mask-2.png)

Als verwijzing is hier waar het masker voor een bestand of map wordt weergegeven in de Portal Azure.

![ACL's Lake gegevensarchief](./media/data-lake-store-access-control/data-lake-store-show-acls-mask-view.png)

>[AZURE.NOTE] Het masker voor de toegang tot en de standaard-ACL van de hoofdmap (/') worden voor een nieuw gegevensarchief Lake account standaard behandeld als RWX.

## <a name="permissions-on-new-files-and-folders"></a>Machtigingen voor bestanden en mappen

Wanneer een nieuw bestand of map in een bestaande map wordt gemaakt, bepaalt de standaard ACL in de bovenliggende map:

* De standaard ACL en toegang tot een onderliggende map
* Een onderliggend bestand van Access ACL (bestanden beschikt niet over een standaard-ACL)

### <a name="a-child-file-or-folders-access-acl"></a>Een onderliggend bestand of map toegang ACL

Wanneer een onderliggend bestand of map wordt gemaakt, wordt standaard ACL van de moedermaatschappij als het onderliggende bestand of map toegang ACL gekopieerd. Ook als **andere** gebruiker RWX machtigingen in de bovenliggende standaard ACL, wordt deze volledig verwijderd uit de ACL van het onderliggende item van Access.

![ACL's Lake gegevensarchief](./media/data-lake-store-access-control/data-lake-store-acls-child-items-1.png)

De bovenstaande informatie is in de meeste gevallen hoeft u moet weten over hoe de ACL van een onderliggend item van Access wordt bepaald. Echter als u bekend met de POSIX-systemen bent en wilt gedetailleerde begrijpen hoe deze transformatie wordt bereikt, Zie de sectie [van Umask rol in de ACL toegang voor nieuwe bestanden en mappen maken](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) , verderop in dit artikel.
 

### <a name="a-child-folders-default-acl"></a>Van een onderliggende map standaard ACL

Als een onderliggende map wordt gemaakt onder een bovenliggende map, worden standaard ACL van de bovenliggende map wordt gekopieerd, zoals, aan de onderliggende map standaard ACL is.

![ACL's Lake gegevensarchief](./media/data-lake-store-access-control/data-lake-store-acls-child-items-2.png)

## <a name="advanced-topics-for-understanding-acls-in-data-lake-store"></a>Geavanceerde onderwerpen voor ACL's in het gegevensarchief voor meer informatie over

Hier volgen een aantal geavanceerde onderwerpen om te begrijpen hoe de ACL's voor gegevensopslag meer bestanden of mappen worden bepaald.

### <a name="umasks-role-in-creating-the-access-acl-for-new-files-and-folders"></a>De rol van Umask in de ACL toegang voor nieuwe bestanden en mappen maken

Het algemene begrip is dat umask is een 9-bits waarde op de bovenliggende map de machtiging voor de **gebruiker die eigenaar is** **eigenaar van de groep**en **andere** op een nieuw onderliggend bestand of map toegang ACL transformeren in een POSIX-compatibel systeem. De bits van een umask bepalen welke bits uit te schakelen in de ACL van het onderliggende item van Access. Het is dus om selectief te voorkomen dat het doorgeven van machtigingen voor een gebruiker, groep, die eigenaar is van die eigenaar gebruikte en andere.
  
De umask is in een systeem HDFS meestal een hele site configuratieoptie die wordt beheerd door beheerders. Lake gegevensarchief wordt gebruikt voor een **hele account umask** die niet kunnen worden gewijzigd. De volgende tabel bevat meer gegevensarchief van umask.

| Gebruikersgroep  | Instelling | Effect op de nieuwe onderliggende item van Access ACL |
|------------ |---------|---------------------------------------|
| Eigendom van gebruiker | ---     | Geen effect                             |
| Eigenaar van de groep| ---     | Geen effect                             |
| Andere       | RWX     | Verwijder lezen + schrijven + uitvoeren         | 

In de volgende afbeelding ziet u deze umask in actie. Het netto-effect is te **lezen + schrijven en uitvoeren** voor een **andere** gebruiker verwijderen. Aangezien de umask geen bits voor de **gebruiker die eigenaar is** en **die groep eigenaar**hebt opgegeven, worden deze machtigingen niet getransformeerd.

![ACL's Lake gegevensarchief](./media/data-lake-store-access-control/data-lake-store-acls-umask.png) 

### <a name="the-sticky-bit"></a>De sticky bit

De sticky bit is een geavanceerde voorziening van een POSIX-bestandssysteem. In het kader van het gegevensarchief Lake is het onwaarschijnlijk dat de sticky bit nodig zijn.

De onderstaande tabel ziet u de werking van de sticky bit in Lake gegevensarchief.

| Gebruikersgroep         | Bestand    | Map |
|--------------------|---------|-------------------------|
| Sticky bit **uit** | Geen effect   | Geen effect           |
| Sticky bit **ON**  | Geen effect   | Niemand behalve **controle gebruikers** en de **gebruiker die eigenaar is** van een onderliggend item van het verwijderen of hernoemen van onderliggend item.               |

De sticky bit wordt niet weergegeven in de Portal Azure.

## <a name="common-questions-for-acls-in-data-lake-store"></a>Veelgestelde vragen voor de ACL's in het gegevensarchief Lake

Hier volgen enkele vragen die afkomstig van vaak van ACL's in Lake gegevensarchief zijn.

### <a name="do-i-have-to-enable-support-for-acls"></a>Heb ik ondersteuning voor ACL's inschakelen?

Nr. Toegangsbeheer via ACL's is altijd ingeschakeld voor de account van een gegevensarchief Lake.

### <a name="what-permissions-are-required-to-recursively-delete-a-folder-and-its-contents"></a>Welke machtigingen zijn vereist om recursief het verwijderen van een map en de inhoud ervan?

* De bovenliggende map moet **schrijven + uitvoeren**.
* De map wordt verwijderd en elke map in, moet **lezen + schrijven en uitvoeren**.
>[AZURE.NOTE] Verwijderen van de bestanden in mappen is, wordt schrijven niet op deze bestanden vereist. Ook de hoofdmap "/" kan **nooit** worden verwijderd.

### <a name="who-is-set-as-the-owner-of-a-file-or-folder"></a>Die is ingesteld als de eigenaar van een bestand of map?

De maker van een bestand of map wordt de eigenaar.

### <a name="who-is-set-as-the-owning-group-of-a-file-or-folder-at-creation"></a>Die is ingesteld als de groep die eigenaar van een bestand of map te maken?

Het wordt gekopieerd uit de groep die eigenaar van de bovenliggende map waarin het nieuwe bestand of de map is gemaakt.

### <a name="i-am-the-owning-user-of-a-file-but-i-dont-have-the-rwx-permissions-i-need-what-do-i-do"></a>Ik ben de gebruiker die eigenaar van een bestand, maar ik heb de RWX machtigingen die ik nodig. Wat moet ik doen?

De gebruiker die eigenaar kunt de machtigingen van het bestand machtigingen te geven zelf een RWX hoeven te wijzigen.

### <a name="does-data-lake-store-support-inheritance-of-acls"></a>Lake gegevensarchief biedt ondersteuning voor de overname van ACL's?

Nr.

### <a name="what-is-the-difference-between-mask-and-umask"></a>Wat is het verschil tussen het masker en de umask?

| masker | umask|
|------|------|
| De eigenschap **mask** is beschikbaar op elk bestand en elke map. | De **umask** is een eigenschap van het gegevensarchief Lake-account. Er is dus slechts een enkele umask in het gegevensarchief Lake.    |
| De eigenschap mask in een bestand of map kan worden gewijzigd door de gebruiker die eigenaar of de eigenaar van de groep van een bestand of een supergebruiker. | De eigenschap umask kan niet worden gewijzigd door alle gebruikers, zelfs een super-gebruiker. Het is een niet te wijzigen, een constante waarde.|
| De eigenschap mask wordt gebruikt om tijdens het algoritme toegangscontrole in runtime te bepalen of een gebruiker het recht voor het uitvoeren van de bewerking op een bestand of map. De rol van het masker is voor het maken van 'effectieve machtigingen' ten tijde van de toegangscontrole. | De umask is niet worden gebruikt tijdens een toegangscontrole. De umask is gebruikt om de ACL van de toegang van nieuwe onderliggende items van een map. |
| Het masker is een 3-bits RWX waarde die voor de benoemde gebruiker en groep naam gebruiker eigenaar op het moment van de toegangscontrole geldt.| De umask is een 9-bits waarde die voor de gebruiker die eigenaar, eigenaar, groep en van een nieuw onderliggend element geldt.| 

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a>Waar vind ik meer informatie over POSIX model voor toegangsbeheer?

* [http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.HTML](http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html)

* [HDFS machtiging gids](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html) 

* [POSIX-VEELGESTELDE VRAGEN](http://www.opengroup.org/austin/papers/posix_faq.html)

* [POSIX 1003.1 2008](http://standards.ieee.org/findstds/standard/1003.1-2008.html)

* [POSIX 1003.1e 1997](http://users.suse.com/~agruen/acl/posix/Posix_1003.1e-990310.pdf)

* [POSIX ACL op Linux](http://users.suse.com/~agruen/acl/linux-acls/online/)

* [ACL via lijsten voor toegangscontrole op Linux](http://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## <a name="see-also"></a>Zie ook

* [Overzicht van Azure Lake gegevensarchief](data-lake-store-overview.md)

* [Aan de slag met Azure gegevens Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md)





