<properties
   pageTitle="Installeer Visual Studio en SSDT voor SQL datawarehouse | Microsoft Azure"
   description="Installeer Visual Studio en SQL Server Development Tools (SSDT) voor Azure SQL datawarehouse"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="sonyama;barbkess"/>

# <a name="install-visual-studio-2015-and-ssdt-for-sql-data-warehouse"></a>Installeer Visual Studio 2015 en SSDT voor SQL datawarehouse

Het is raadzaam met behulp van Visual Studio 2015 met de meest recente versie van SQL Server Data Tools (SSDT) voor het ontwikkelen van toepassingen voor SQL Data Warehouse.  Visual Studio 2013 Update 5 met SSDT wordt ook ondersteund voor achterwaartse compatibiliteit.  

SSDT met Visual Studio kunt u de SQL Server-Object Explorer gebruiken om visueel verkennen van tabellen, weergaven, opgeslagen procedures en veel meer objecten in uw magazijn SQL gegevens alsmede de query's uitvoeren.

> [AZURE.NOTE] SQL Data Warehouse ondersteunt nog geen Database Visual Studio-projecten.  Deze functie wordt toegevoegd in een toekomstige versie.

## <a name="step-1-install-visual-studio-2015"></a>Stap 1: Installeer Visual Studio 2015

Klik op deze koppelingen als u wilt downloaden en installeren van Visual Studio 2015. Als u Visual Studio 2013 of 2015 geïnstalleerd al hebt, kunt u verder met stap 2, SSDT installeren.

1. [Download van Visual Studio 2015][].
2. Volg de gids voor de [Installatie van Visual Studio][] op MSDN en kiest u de standaardconfiguratie.

## <a name="step-2-install-ssdt"></a>Stap 2: Installeer SSDT

Als u wilt installeren SSDT voor Visual Studio gewoon zoeken naar een SSDT update van Visual Studio volgt.

1. Klik op **Extra**in Visual Studio / **uitbreidingen en Updates...**  /  **Updates**
2. Selecteer **Product Updates** en zoek naar de **Update voor Microsoft SQL Server voor database-gereedschap**

Als er een update wordt gevonden, moet u de meest recente versie is geïnstalleerd.  Klik op **Help**om te bevestigen SSDT is geïnstalleerd, / **Over Microsoft Visual Studio** en SQL Server Data Tools in de lijst zoekt.  De nieuwste versie van SSDT is 14.0.60525.0.  Als de optie voor het installeren van Visual Studio beschikbaar is, kunt ook u via de pagina [SSDT downloaden][] om te downloaden en SSDT handmatig installeren.

## <a name="next-steps"></a>Volgende stappen

Nu u hebt de nieuwste versie van SSDT, bent u klaar om [verbinding][] te maken naar uw magazijn SQL-gegevens.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[verbinding maken]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Download van Visual Studio 2015]: https://www.visualstudio.com/downloads/
[Installeren van Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT downloaden]: https://msdn.microsoft.com/library/mt204009.aspx
