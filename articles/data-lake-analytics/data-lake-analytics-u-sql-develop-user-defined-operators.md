<properties 
   pageTitle="U SQL door gebruiker gedefinieerde operators voor Azure gegevens Lake Analytics projecten ontwikkelen | Azure" 
   description="Informatie over het door gebruiker gedefinieerde operators worden gebruikt en opnieuw worden gebruikt in gegevens Lake Analytics projecten te ontwikkelen. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>


# <a name="develop-u-sql-user-defined-operators-for-azure-data-lake-analytics-jobs"></a>Door de gebruiker gedefinieerd U SQL-operators voor Azure gegevens Lake Analytics taken ontwikkelen

Informatie over het door gebruiker gedefinieerde operators worden gebruikt en opnieuw worden gebruikt in gegevens Lake Analytics projecten te ontwikkelen. U zal een aangepaste namen converteren-operator ontwikkelen.

##<a name="prerequisites"></a>Vereisten

- Visual Studio 2015, Visual Studio 2013 werken 4 of Visual Studio 2012 met Visual C++ is geïnstalleerd 
- Microsoft Azure SDK voor .NET versie 2.5 of hoger.  Installeren met het installatieprogramma van de Web-platform.
- Gegevens Lake Analytics-account.  Zie [Aan de slag met Azure gegevens Lake Analytics met Azure Portal](data-lake-analytics-get-started-portal.md).
- De zelfstudie [aan de slag met Azure gegevens Lake Analytics U SQL Studio](data-lake-analytics-u-sql-get-started.md) doorlopen.
- Verbinding maken met Azure, Zie [aan de slag met Azure gegevens Lake Analytics U SQL Studio](data-lake-analytics-u-sql-get-started.md#connect-to-azure). 
- Uploaden van de brongegevens, kunt u [aan de slag met Azure gegevens Lake Analytics U SQL Studio](data-lake-analytics-u-sql-get-started.md#upload-source-data-files). 

## <a name="define-and-use-user-defined-operator-in-u-sql"></a>Definiëren en door gebruiker gedefinieerde operator in U SQL gebruiken

**Maken en indienen van een taak U SQL** 

1. Klik op **Nieuw**in het menu **bestand** en klik vervolgens op **Project**.
2. Selecteer het type **Project U SQL** .

    ![nieuwe U SQL Visual Studio-project](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. Klik op **OK**. Visual studio maakt een oplossing met een Script.usql-bestand.
4. In **Solution Explorer**Script.usql vouwen en dubbelklik vervolgens op **Script.usql.cs**.
5. Plak de volgende code in het bestand:

        using Microsoft.Analytics.Interfaces;
        using System.Collections.Generic;
        
        namespace USQL_UDO
        {
            public class CountryName : IProcessor
            {
                private static IDictionary<string, string> CountryTranslation = new Dictionary<string, string>
                {
                    {
                        "Deutschland", "Germany"
                    },
                    {
                        "Schwiiz", "Switzerland"
                    },
                    {
                        "UK", "United Kingdom"
                    },
                    {
                        "USA", "United States of America"
                    },
                    {
                        "中国", "PR China"
                    }
                };
        
                public override IRow Process(IRow input, IUpdatableRow output)
                {
        
                    string UserID = input.Get<string>("UserID");
                    string Name = input.Get<string>("Name");
                    string Address = input.Get<string>("Address");
                    string City = input.Get<string>("City");
                    string State = input.Get<string>("State");
                    string PostalCode = input.Get<string>("PostalCode");
                    string Country = input.Get<string>("Country");
                    string Phone = input.Get<string>("Phone");
        
                    if (CountryTranslation.Keys.Contains(Country))
                    {
                        Country = CountryTranslation[Country];
                    }
                    output.Set<string>(0, UserID);
                    output.Set<string>(1, Name);
                    output.Set<string>(2, Address);
                    output.Set<string>(3, City);
                    output.Set<string>(4, State);
                    output.Set<string>(5, PostalCode);
                    output.Set<string>(6, Country);
                    output.Set<string>(7, Phone);
        
                    return output.AsReadOnly();
                }
            }
        }

5. Open Script.usql en plak het volgende U SQL-script:

        @drivers =
            EXTRACT UserID      string,
                    Name        string,
                    Address     string,
                    City        string,
                    State       string,
                    PostalCode  string,
                    Country     string,
                    Phone       string
            FROM "/Samples/Data/AmbulanceData/Drivers.txt"
            USING Extractors.Tsv(Encoding.Unicode);
        
        @drivers_CountryName =
            PROCESS @drivers
            PRODUCE UserID string,
                    Name string,
                    Address string,
                    City string,
                    State string,
                    PostalCode string,
                    Country string,
                    Phone string
            USING new USQL_UDO.CountryName();    
        
        OUTPUT @drivers_CountryName
            TO "/Samples/Outputs/Drivers.csv"
            USING Outputters.Csv(Encoding.Unicode);

6. In **Solution Explorer**, klikt u met de rechtermuisknop op **Script.usql**en klik op **Script maken**.
6. In de **Solution Explorer**, klikt u met de rechtermuisknop op **Script.usql**en klik op **Script indienen**.
7. Als u nog niet hebt verbinding met uw abonnement Azure, worden u vragen om uw Azure accountreferenties opgeven.
7. Klik op **indienen**. Resultaten van indienen en de taakkoppeling zijn beschikbaar in het venster wanneer het verzenden is voltooid.
8. U moet klikt u op de knop Vernieuwen om de status van de meest recente zien en het scherm te vernieuwen.

**Voor een overzicht van de uitvoer van de taak**

1. In **Server Explorer** **Azure**uitvouwen, vouw **Gegevens Lake Analytics**Vouw uw account gegevens Lake Analytics, vouw **Opslag rekeningen**, met de rechtermuisknop op de standaard opslag en klik vervolgens op **Explorer**. 
2. Vouw van monsters, uitgangen uit en dubbelklik **Stuurprogramma's.csv**.


##<a name="see-also"></a>Zie ook

- [Aan de slag met Data Lake Analytics met PowerShell](data-lake-analytics-get-started-powershell.md)
- [Aan de slag met Data Lake Analytics met behulp van de portal Azure](data-lake-analytics-get-started-portal.md)
- [Meer hulpmiddelen voor Visual Studio gebruiken voor het ontwikkelen van toepassingen U SQL](data-lake-analytics-data-lake-tools-get-started.md)
