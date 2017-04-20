<properties
   pageTitle="Verbinding maken met gegevensbronnen | Microsoft Azure"
   description="Verbinding maken met gegevensbronnen die met Azure gegevenscatalogus ontdekt markeren artikel."
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
   ms.date="09/15/2016"
   ms.author="maroche"/>


# <a name="how-to-connect-to-data-sources"></a>Verbinding maken met gegevensbronnen

## <a name="introduction"></a>Inleiding
**Catalogus gegevens van Microsoft Azure** is een volledig beheerde cloud service die als een systeem van registratie en het systeem van discovery voor gegevensbronnen van de onderneming fungeert. **Catalogus met Azure gegevens** is met andere woorden, helpen mensen ontdekken, te begrijpen en gegevensbronnen en helpt organisaties gebruikt om meer waarde uit hun bestaande gegevens bij. Een belangrijk aspect van dit scenario gebruikt de gegevens – zodra een gebruiker een gegevensbron ontdekt en het doel ervan begrijpt, de volgende stap is het verbinding maken met de gegevensbron om de gegevens te gebruiken.

## <a name="data-source-locations"></a>Gegevens laden
**Catalogus met Azure gegevens** ontvangt tijdens de registratie data source metagegevens over de gegevensbron. Deze metagegevens omvatten de details van de locatie van de gegevensbron. De details van de locatie afhankelijk van de gegevensbron op de gegevensbron, maar bevat altijd de informatie die nodig is om verbinding te maken. Bijvoorbeeld, bevat de locatie voor een SQL Server-tabel de naam van de server, databasenaam, schemanaam en tabelnaam, terwijl de locatie voor een SQL Server Reporting Services-rapport bevat de naam van de server en het pad naar het rapport. Andere typen gegevensbronnen hebben locaties die overeenkomen met de structuur en de mogelijkheden van het bronsysteem.

## <a name="integrated-client-tools"></a>Geïntegreerde clienthulpprogramma 's
De eenvoudigste manier om verbinding met een gegevensbron te gebruiken is het "openen in..." het menu in de **Catalogus van Azure gegevens** portal. Dit menu wordt een lijst met opties voor de verbinding met de activa van de geselecteerde gegevens weergegeven.
Als u de standaardweergave van de tegel, is dit menu beschikbaar op de elke tegel.

 ![Een SQL Server-tabel openen in Excel vanuit de tegel gegevens activa](./media/data-catalog-how-to-connect/data-catalog-how-to-connect1.png)

Wanneer u de lijstweergave, wordt het menu is beschikbaar op de balk boven aan het venster van de portal.

 ![Een SQL Server Reporting Services-rapport openen in Report Manager vanuit de zoekbalk](./media/data-catalog-how-to-connect/data-catalog-how-to-connect2.png)

## <a name="supported-client-applications"></a>Ondersteunde toepassingen
Bij het gebruik van de "openen in..." menu voor gegevensbronnen in de catalogus van Azure gegevens portal, de juiste clienttoepassing moet op de clientcomputer zijn geïnstalleerd.

| Openen in de toepassing | Bestandsextensie / protocol | Ondersteunde toepassingsversies |
| --- | --- | --- |
| Excel | ODC | Excel 2010 of hoger |
| Excel (boven 1000) | ODC | Excel 2010 of hoger |
| Power Query | XLSX | Excel 2016 of Excel 2010 of Excel 2013 met de Power Query voor Excel-invoegtoepassing geïnstalleerd
| Power BI-Desktop | .pbix | Power BI bureaublad juli 2016 of later |
| SQL Server Data Tools | vsweb: / / | Visual Studio 2013 Update 4 of hoger met SQL Server tooling geïnstalleerd |
| Report Manager | http:// | Zie [Browservereisten voor SQL Server Reporting Services](https://technet.microsoft.com/en-us/library/ms156511.aspx) |

## <a name="your-data-your-tools"></a>Uw gegevens, uw extra
De beschikbare opties in het menu zijn afhankelijk van het soort gegevens activum dat is geselecteerd. Natuurlijk niet alle mogelijke tools worden opgenomen in de "openen in..." menu, maar is het ook eenvoudig verbinding maken met de gegevensbron met behulp van een client gereedschap. Als een activum gegevens in de portal **Azure catalogus met gegevens** is geselecteerd, wordt de volledige locatie weergegeven in het deelvenster Eigenschappen.

 ![Verbindingsgegevens voor een SQL Server-tabel](./media/data-catalog-how-to-connect/data-catalog-how-to-connect3.png)

De verbindingsdetails verschillen gegevensbrontype voor het gegevensbrontype, maar de informatie die is opgenomen in de portal biedt u alles wat die u moet verbinding maken met de gegevensbron in een client gereedschap. Gebruikers kunnen kopiëren de verbindingsgegevens voor de gegevensbronnen die ze hebben gevonden met behulp van **Catalogus Azure-gegevens**, zodat ze werken met de gegevens in hun programma van uw keuze.

## <a name="connecting-and-data-source-permissions"></a>Verbinding maken en gegevens bron machtigingen
**Azure gegevenscatalogus** kunt u gegevensbronnen worden gedetecteerd, de toegang tot de gegevens zelf blijft staan onder toezicht van de data source eigenaar of beheerder. Ontdekking van een gegevensbron in **De catalogus gegevens Azure** geeft geen gebruiker alle machtigingen voor toegang tot de gegevensbron zelf.

Als u wilt maken het gemakkelijker voor gebruikers die niet gemachtigd zijn voor toegang tot de gegevens, maar Ontdek een gegevensbron, kunnen gebruikers gegevens invoeren in de eigenschap toegang aanvragen bij het toewijzen van aantekeningen aan een gegevensbron. Informatie, waaronder koppelingen naar het aanspreekpunt voor de toegang tot gegevens krijgen of – die wordt weergegeven naast de locatie informatie in de portal.

 ![Verbindingsgegevens met verzoek toegang instructies](./media/data-catalog-how-to-connect/data-catalog-how-to-connect4.png)

##<a name="summary"></a>Samenvatting
Een gegevensbron met een **Catalogus met Azure gegevens** registreren, kunt u die gegevens detecteerbaar door structurele en beschrijvende metagegevens uit de gegevensbron te kopiëren in de catalogus service. Nadat u een gegevensbron is geregistreerd en ontdekt, gebruikers kunnen verbinding maken met de gegevensbron vanaf de portal **Azure gegevenscatalogus** "geopend in..." " menu of met behulp van de hulpmiddelen voor gegevens van keuze.

## <a name="see-also"></a>Zie ook
- Zelfstudie [Aan de slag met Azure gegevenscatalogus](data-catalog-get-started.md) voor meer informatie over het maken van verbinding met gegevensbronnen.
