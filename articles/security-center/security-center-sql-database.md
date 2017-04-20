<properties
   pageTitle="Azure Security Center en Azure SQL Database service | Microsoft Azure"
   description="In dit artikel ziet u hoe Security Center kunt u uw databases in Azure SQL-Database te beveiligen."
   services="sql-database"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-and-azure-sql-database-service"></a>Azure Security Center en Azure SQL-Database

[Azure Beveiligingscentrum](https://azure.microsoft.com/documentation/services/security-center/) helpt bij het voorkomen, opsporen en reageren op bedreigingen. Het biedt geïntegreerde beveiliging beleid en het toezicht op verschillende de Azure abonnementen, helpt detecteren bedreigingen die anders opgemerkt en werkt met een brede selectie van beveiligingsoplossingen.

In dit artikel ziet u hoe Security Center kunt u uw databases in Azure SQL-Database te beveiligen.

## <a name="why-use-security-center"></a>Waarom Beveiligingscentrum gebruiken?

Beveiligingscentrum helpt u bij het beschermen van gegevens in een SQL-Database door middel van inzicht in de beveiliging van uw servers en databases. Met Beveiligingscentrum kunt u:

- Beleid voor controle en codering voor SQL-Database definiëren.
- Toezicht op de beveiliging van de SQL-Database via uw abonnementen.
- Snel identificeren en verhelpen van problemen met de beveiliging.
- Meldingen van [Azure SQL Database dreiging detectie](../sql-database/sql-database-threat-detection-get-started.md)integreren.

Naast het beveiligen van uw resources voor SQL-Database, biedt Security Center ook beveiliging bewaking en beheer voor Azure virtuele machines, Cloud Services, App-Services, virtuele netwerken en meer. Meer informatie over de Security Center [hier](security-center-intro.md).

## <a name="prerequisites"></a>Vereisten

Om te beginnen met Security Center, hebt u een abonnement op Microsoft Azure. De vrij laag van Beveiligingscentrum is ingeschakeld bij uw abonnement. Zie [Security Center prijzen](https://azure.microsoft.com/pricing/details/security-center/)voor meer informatie over Security Center van vrije en standaard lagen.

Security Center ondersteunt toegang op basis van rollen. Zie voor meer informatie over op rollen gebaseerde toegangscontrole (RBAC) in Azure, [Azure Active Directory op rollen gebaseerde toegangscontrole](../active-directory/role-based-access-control-configure.md). De veelgestelde vragen over het Security Center vindt u informatie over [hoe machtigingen in Beveiligingscentrum worden verwerkt](security-center-faq.md#how-are-permissions-handled-in-azure-security-center).

## <a name="access-security-center"></a>Access Security Center

U openen Beveiligingscentrum vanaf de [portal Azure](https://azure.microsoft.com/features/azure-portal/). [Aanmelden bij de portal](https://portal.azure.com/) en selecteer de **optie Security Center**.

![Security Center-optie][1]

De blade **Security Center** wordt geopend.
![Security Center-blade][2]

## <a name="set-security-policy"></a>Beveiligingsbeleid instellen

Een beveiligingsbeleid bepaalt welke besturingselementen die worden aanbevolen voor bronnen binnen de opgegeven abonnement of resourcegroep. In Beveiligingscentrum definieert u het beleid voor uw abonnementen of resourcegroepen volgens de beveiligingsbehoeften van uw bedrijf en het type van toepassingen of de gevoeligheid van de gegevens in elk abonnement.

U kunt instellen dat een beleid aanbevelingen voor het controleren van de SQL- en SQL transparante gegevenscodering (TDE) weergeven.

- Als u **SQL-controle en detectie van bedreiging**inschakelt, Security Center raadt aan dat controle van de toegang tot Database Azure voor naleving, geavanceerde detectie en onderzoek doeleinden worden ingeschakeld.
- Als u **SQL transparante gegevenscodering**, Security Center aangeraden inschakelt zijn voor uw Azure SQL-Database, back-ups van gekoppeld en transactielogboekbestanden versleuteling in rust ingeschakeld.

Als u een beveiligingsbeleid, selecteert u het **beleid** naast elkaar op het blad Security Center. Selecteer het abonnement dat u wilt inschakelen, het beveiligingsbeleid op de bladeserver **beveiligingsbeleid** . Selecteer **in de preventie** en **op** de aanbevelingen voor de beveiliging die u wilt gebruiken voor dit abonnement.
![Beveiligingsbeleid][3]

Voor meer informatie, Zie [beveiligingsbeleid instellen](security-center-policies.md).

## <a name="manage-security-recommendation"></a>Aanbevelingen voor beveiliging beheren

Security Center analyseert regelmatig de beveiligingsstatus van uw resources Azure. Wanneer Beveiligingscentrum potentiële beveiligingsproblemen geeft, maakt deze aanbevelingen. De aanbevelingen helpen u bij het proces van het configureren van de benodigde besturingselementen.

Nadat u een beveiligingsbeleid hebt ingesteld, analyseert Beveiligingscentrum de beveiligingsstatus van uw middelen ter identificatie van beveiligingslekken. De aanbevelingen worden weergegeven in een tabel waarbij elke regel staat voor één bepaalde aanbeveling. Gebruik de volgende tabel als referentie om te begrijpen van de beschikbare aanbevelingen voor Azure SQL-Database en elke aanbeveling doet als u deze toepast. Selecteren van een aanbeveling gaat u naar een artikel met informatie over het implementeren van de aanbeveling in Beveiligingscentrum.

| Aanbeveling | Beschrijving |
| ----- | ----- |
| [Controle en bedreiging SQL servers-detectie inschakelen](security-center-enable-auditing-on-sql-servers.md) | Raadt aan dat u voor SQL-Database servers controleren en bedreiging detectie inschakelen. (Alleen voor SQL-Database-service. Bevat geen Microsoft SQL Server die op uw virtuele machines worden uitgevoerd.) |
| [Controle en bedreiging SQL-databases-detectie inschakelen](security-center-enable-auditing-on-sql-databases.md) | Raadt aan dat u voor de Database SQL databases controleren en de bedreiging detectie inschakelen. (Alleen voor SQL-Database-service. Bevat geen Microsoft SQL Server die op uw virtuele machines worden uitgevoerd.) |
| [Transparante codering inschakelen](security-center-enable-transparent-data-encryption.md) | Raadt aan dat u deze optie voor SQL-databases inschakelt. (Alleen SQL-Database-service). |

Selecteer de **aanbevelingen** naast elkaar op het blad Security Center aanbevelingen voor uw resources Azure vindt. Selecteer op het blad **aanbevelingen** een aanbeveling om details te bekijken. In dit voorbeeld kiest u we **controle inschakelen en bedreiging detectie op SQL-servers**.

![Aanbevelingen][4]

Zoals hieronder wordt weergegeven, ziet u Beveiligingscentrum de SQL-servers waarbij controle en bedreiging detectie niet zijn ingeschakeld. Nadat u controle hebt ingeschakeld, kunt u instellingen voor bedreiging detectie en e-mailadres voor het ontvangen van waarschuwingen. Detectie van bedreiging wordt u gewaarschuwd wanneer afwijkende databaseactiviteiten die wijzen op potentiële beveiligingsrisico's tot de database wordt gedetecteerd. De waarschuwingen worden weergegeven in het dashboard Security Center.
![Controle- en bedreiging detectie][5]

Volg de stappen in de [aan de slag met SQL Database dreiging detectie](../sql-database/sql-database-threat-detection-get-started.md) inschakelen en configureren van de detectie van de dreiging, en de lijst met e-mails die u waarschuwingen na detectie van afwijkende activiteiten ontvangt configureren.

Zie voor meer informatie over aanbevelingen, [aanbevelingen voor de beveiliging beheren](security-center-recommendations.md).

## <a name="monitor-security-health"></a>Beveiliging-health monitor

Nadat u [beveiligingsbeleid](security-center-policies.md) voor een abonnement van resources hebt ingeschakeld, wordt Beveiligingscentrum de beveiliging van uw resources voor het identificeren van potentiële problemen te analyseren.  U kunt de beveiligingsstatus van uw resources weergeven in de tegel **Resource beveiliging gezondheid** . Wanneer u **gegevens** in de **bron security health** tegel klikt, opent de blade **Gegevensbronnen** met SQL-aanbevelingen voor problemen met de controle en transparante gegevenscodering niet ingeschakeld. Het heeft ook aanbevelingen voor de algemene status van de database.
![Resource security health][6]

Voor meer informatie, Zie [Statuscontrole beveiliging](security-center-monitoring.md).

## <a name="manage-and-respond-to-security-alerts"></a>Beheren en reageren op de beveiligingswaarschuwingen

Security Center automatisch verzamelt, analyseert en logboekgegevens van [Azure SQL bedreiging detectie](../sql-database/sql-database-threat-detection-get-started.md), alsmede andere Azure bronnen aan reële dreigingen te detecteren en het beperken van vals-positief zijn geïntegreerd. Een lijst met naar prioriteit gerangschikte beveiligingswaarschuwingen weergegeven in Beveiligingscentrum samen met de informatie die u nodig hebt snel onderzoeken het probleem en aanbevelingen voor het verhelpen van een aanval.

Selecteer waarschuwingen vindt de **beveiligingswaarschuwingen** naast elkaar op het blad Security Center. Selecteer een waarschuwing voor meer informatie over de gebeurtenissen die de waarschuwing en wat, geactiveerd als alle stappen die u moet nemen om te herstellen van een aanval op de bladeserver **beveiligingswaarschuwingen** . In dit voorbeeld gaan we **potentiële SQL injection**selecteren.
![Beveiligingswaarschuwingen][7]

Zoals hieronder wordt weergegeven, Security Center biedt aanvullende informatie die inzicht bieden in wat de waarschuwing, de doelbron, indien van toepassing werd geactiveerd het bron-IP-adres en aanbevelingen over het te verhelpen.
![Potentiële SQL injection][8]

Zie voor meer informatie kunt [beheren en reageren op de beveiligingswaarschuwingen](security-center-managing-and-responding-alerts.md).

## <a name="next-steps"></a>Volgende stappen

- [Veelgestelde vragen over het Security Center](security-center-faq.md) , zoeken, veelgestelde vragen over het gebruik van de service.
- [Security Center-planning en operations guide](security-center-planning-and-operations-guide.md) - volgen een reeks stappen en de taken voor het optimaliseren van het gebruik van Beveiligingscentrum gebaseerd op de beveiligingsvereisten en cloud model voor het beheer van uw organisatie.
- [Security Center Data Security](security-center-data-security.md) – leren hoe Beveiligingscentrum verzamelt en verwerkt gegevens over uw Azure bronnen, zoals configuratiegegevens, metagegevens, gebeurtenislogboeken, crashdump-bestanden en meer.
- [Afhandelen van beveiligingsincidenten](security-center-incident.md) - informatie over het gebruik van de alert mogelijkheid beveiliging in Beveiligingscentrum helpt u bij het afhandelen van beveiligingsincidenten.

<!--Image references-->
[1]: ./media/security-center-sql-database/security-center.png
[2]: ./media/security-center-sql-database/security-center-blade.png
[3]: ./media/security-center-sql-database/security-policy.png
[4]: ./media/security-center-sql-database/recommendation.png
[5]: ./media/security-center-sql-database/turn-on-auditing.png
[6]: ./media/security-center-sql-database/monitor-health.png
[7]: ./media/security-center-sql-database/alert.png
[8]: ./media/security-center-sql-database/sql-injection.png
