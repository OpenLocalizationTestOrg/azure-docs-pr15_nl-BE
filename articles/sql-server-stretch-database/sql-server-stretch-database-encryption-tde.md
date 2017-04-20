<properties
   pageTitle="Transparante gegevenscodering (TDE) voor uitrekken Database van SQL Server op Azure inschakelen | Microsoft Azure"
   description="Transparante gegevenscodering (TDE) voor uitrekken Database van SQL Server op Azure inschakelen"
   services="sql-server-stretch-database"
   documentationCenter=""
   authors="douglaslMS"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-server-stretch-database"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/14/2016"
   ms.author="douglaslMS"/>

# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure"></a>Transparante gegevenscodering (TDE) inschakelen voor uitrekken Database op Azure
> [AZURE.SELECTOR]
- [Azure portal](sql-server-stretch-database-encryption-tde.md)
- [TSQL](sql-server-stretch-database-tde-tsql.md)

Transparante gegevens codering (TDE) biedt beveiliging tegen het gevaar van schadelijke activiteiten door middel van real-time codering en decodering van de database, back-ups van gekoppeld en transactielogboekbestanden in rust zonder wijzigingen in de toepassing.

De opslag van een volledige database codeert TDE met behulp van een symmetrische sleutel die de database coderingssleutel genoemd. De coderingssleutel voor de database is beveiligd met een ingebouwde server-certificaat. De ingebouwde servercertificaat is uniek voor elke server Azure. Microsoft draait deze certificaten automatisch ten minste elke 90 dagen. Zie voor een algemene beschrijving van TDE [Transparante gegevens codering (TDE)].

##<a name="enabling-encryption"></a>Codering inschakelen

Om TDE inschakelen voor een Azure database voor het opslaan van de gegevens uit een database van SQL Server uitrekken ingeschakeld gemigreerd, het volgende doen:

1. Open de database in de [Azure portal](https://portal.azure.com)
2. Klik op de knop **Instellingen** in de database-blade
3. Selecteer de optie **transparant gegevenscodering**![][1]
4. Selecteer de instelling **in** en selecteer **Opslaan**
![][2]


##<a name="disabling-encryption"></a>Codering uitschakelen

TDE uitschakelen voor een Azure database voor het opslaan van de gegevens uit een database van SQL Server uitrekken ingeschakeld gemigreerd, het volgende doen:

1. Open de database in de [Azure portal](https://portal.azure.com)
2. Klik op de knop **Instellingen** in de database-blade
3. Selecteer de optie **transparant gegevenscodering**
4. Selecteer de instelling **uit** en selecteer vervolgens **Opslaan**




<!--Anchors-->
[Transparante gegevenscodering (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->
[1]: ./media/sql-server-stretch-database-encryption-tde/stretchtde1.png
[2]: ./media/sql-server-stretch-database-encryption-tde/stretchtde2.png


<!--Link references-->
