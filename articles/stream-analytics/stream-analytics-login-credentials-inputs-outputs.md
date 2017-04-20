<properties 
    pageTitle="Stream Analytics: Aanmelden referenties voor ingangen en uitgangen draaien | Microsoft Azure" 
    description="Informatie over het bijwerken van de referenties voor de Stream Analytics ingangen en uitgangen."
    keywords="inloggegevens"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

#<a name="rotate-login-credentials-for-inputs-and-outputs-in-stream-analytics-jobs"></a>Inloggegevens voor ingangen en uitgangen in Stream Analytics projecten draaien

##<a name="abstract"></a>Abstract
Azure Stream Analytics vandaag niet mogelijk de referenties op een invoer/uitvoer vervangen terwijl de taak wordt uitgevoerd.

Terwijl Azure Stream Analytics biedt ondersteuning voor een project vanaf de laatste uitvoer wordt hervat, wilden we delen van het hele proces voor het minimaliseren van de vertraging tussen het stoppen en starten van de taak en draaien de inloggegevens.

##<a name="part-1---prepare-the-new-set-of-credentials"></a>Deel 1 - voorbereiden van de nieuwe reeks referenties:
Dit deel is van toepassing op de volgende ingangen/uitgangen:

* BLOB-opslag
* Gebeurtenis Hubs
* SQL-Database
* Tabelopslag

Gaat u verder met deel 2 voor andere ingangen /-uitgangen.

###<a name="blob-storagetable-storage"></a>Opslag/tabel-BLOB-opslag
1.  Ga naar de opslag-extensie op de Azure Management portal:  
![graphic1][graphic1]
2.  Ga naar de opslag die wordt gebruikt door uw werk en Ga in het:  
![graphic2][graphic2]
3.  Klik op de opdracht toegangstoetsen beheren:  
![graphic3][graphic3]
4.  Tussen de primaire sleutel van Access en de secundaire sneltoets, **Kies de regellijn die niet door uw werk gebruikt**.
5.  Klik op opnieuw genereren:  
![graphic4][graphic4]
6.  Kopieer de zojuist gegenereerde sleutel:  
![graphic5][graphic5]
7.  Ga verder met deel 2.

###<a name="event-hubs"></a>Gebeurtenis hubs
1.  Ga naar de Bus-Service-extensie op de Azure Management portal:  
![graphic6][graphic6]
2.  Zoek de Service Bus Namespace gebruikt door de taak en gaat u naar het:  
![graphic7][graphic7]
3.  Als uw taak gebruikmaakt van een gedeelde access-beleid op de Bus Service Namespace, Ga naar stap 6  
4.  Ga naar het tabblad gebeurtenis Hubs:  
![graphic8][graphic8]
5.  Zoek de gebeurtenis Hub gebruikt door de taak en gaat u naar het:  
![graphic9][graphic9]
6.  Ga naar het tabblad configureren:  
![graphic10][graphic10]
7.  Zoek het beleid voor gedeelde toegang gebruikt door de taak in de vervolgkeuzelijst de naam van beleid:  
![graphic11][graphic11]
8.  Tussen de primaire sleutel en de secundaire sleutel, **Kies de regellijn die niet door uw werk gebruikt**.  
9.  Klik op opnieuw genereren:  
![graphic12][graphic12]
10. Kopieer de zojuist gegenereerde sleutel:  
![graphic13][graphic13]
11. Ga verder met deel 2.  

###<a name="sql-database"></a>SQL-Database

>[AZURE.NOTE] Opmerking: u moet verbinding maken met de Service SQL-Database. We gaan om te laten zien hoe u dit moet doen met de ervaring met beheer op Azure Management portal, maar u kunt u een client-side gebruiken, zoals SQL Server Management Studio ook.

1.  Ga naar de uitbreiding van de SQL-Databases op de Azure Management portal:  
![graphic14][graphic14]
2.  Zoek de SQL-Database die wordt gebruikt door de koppeling van het project en **Klik op de server** op dezelfde regel:  
![graphic15][graphic15]
3.  Klik op de opdracht beheren:  
![graphic16][graphic16]
4.  Type Database Master:  
![graphic17][graphic17]
5.  Typ uw gebruikersnaam, wachtwoord, en klik op logboek op:  
![graphic18][graphic18]
6.  Klik op nieuwe Query:  
![graphic19][graphic19]
7.  Type in de volgende query < login_name > vervangen door uw gebruikersnaam en het vervangen van <enterStrongPasswordHere> met uw nieuwe wachtwoord:  
`CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8.  Klik op uitvoeren:  
![graphic20][graphic20]
9.  Ga terug naar stap 2 en deze keer klikt u op de database:  
![graphic21][graphic21]
10. Klik op de opdracht beheren:  
![graphic22][graphic22]
11. Typ uw gebruikersnaam, wachtwoord, en klik op aanmelden:  
![graphic23][graphic23]
12. Klik op nieuwe Query:  
![graphic24][graphic24]
13. Typ in de volgende query vervangen door een naam die u identificeren deze aanmelding in de context van deze database wilt (kunt u bieden u < login_name >, bijvoorbeeld heeft dezelfde waarde) < gebruikersnaam > en < login_name > vervangen door uw nieuwe gebruikersnaam:  
`CREATE USER <user_name> FROM LOGIN <login_name>`
14. Klik op uitvoeren:  
![graphic25][graphic25]
15. U dient nu de nieuwe gebruiker met dezelfde functies en bevoegdheden van de oorspronkelijke gebruiker had.
16. Ga verder met deel 2.

##<a name="part-2-stopping-the-stream-analytics-job"></a>Deel 2: De Stream Analytics taak stoppen
1.  Ga naar de Stream Analytics-extensie op de Azure Management portal:  
![graphic26][graphic26]
2.  Zoek uw werk en bij het:  
![graphic27][graphic27]
3.  Ga naar het tabblad invoer of de uitvoer op basis van de vraag of het draaien van de referenties op de invoer of uitvoer.  
![graphic28][graphic28]
4.  Klik op de opdracht Stop en Bevestig dat de taak is gestopt:  
![graphic29][graphic29] wacht totdat de taak stoppen.
5.  De invoer/uitvoer die u wilt draaien de referenties op en Ga naar het zoeken:  
![graphic30][graphic30]
6.  Ga verder met deel 3.

##<a name="part-3-editing-the-credentials-on-the-stream-analytics-job"></a>Deel 3: De referenties voor de Stream Analytics taak bewerken

###<a name="blob-storagetable-storage"></a>Opslag/tabel-BLOB-opslag
1.  Het veld opslag Account sleutel zoeken en de zojuist gegenereerde sleutel plakken:  
![graphic31][graphic31]
2.  Klik op de opdracht opslaan en Bevestig uw wijzigingen:  
![graphic32][graphic32]
3.  Een verbindingstest automatisch gestart wanneer u de wijzigingen opslaan, zorg ervoor dat geslaagd is.
4.  Ga verder met deel 4.

###<a name="event-hubs"></a>Gebeurtenis hubs
1.  Zoek het sleutelveld gebeurtenis Hub beleid en de zojuist gegenereerde sleutel plakken:  
![graphic33][graphic33]
2.  Klik op de opdracht opslaan en Bevestig uw wijzigingen:  
![graphic34][graphic34]
3.  Een verbindingstest automatisch gestart wanneer u de wijzigingen opslaan, zorg ervoor dat met succes is verstreken.
4.  Ga verder met deel 4.

###<a name="power-bi"></a>Power BI
1.  Klik op de vergunning vernieuwen:  
* ![graphic35][graphic35]
* Ontvangt u een bevestiging van het volgende:  
* ![graphic36][graphic36]
2.  Klik op de opdracht opslaan en Bevestig uw wijzigingen:  
![graphic37][graphic37]
3.  Een verbindingstest automatisch gestart wanneer u de wijzigingen opslaat, moet u ervoor zorgen dat heeft doorstaan.
4.  Ga verder met deel 4.

###<a name="sql-database"></a>SQL-Database
1.  De velden gebruikersnaam en wachtwoord en plak de zojuist gemaakte set met referenties in deze:  
![graphic38][graphic38]
2.  Klik op de opdracht opslaan en Bevestig uw wijzigingen:  
![graphic39][graphic39]
3.  Een verbindingstest automatisch gestart wanneer u de wijzigingen opslaan, zorg ervoor dat met succes is verstreken.  
4.  Ga verder met deel 4.

##<a name="part-4-starting-your-job-from-last-stopped-time"></a>Deel 4: De taak starten vanaf de laatst gestopt
1.  Verlaat de Input/Output:  
![graphic40][graphic40]
2.  Klik op de opdracht Start:  
![graphic41][graphic41]
3.  Kies het laatst gestopt en klik op OK:  
 ![graphic42][graphic42]
4.  Ga verder met deel 5.  

##<a name="part-5-removing-the-old-set-of-credentials"></a>Deel 5: De oude set referenties verwijderen
Dit deel is van toepassing op de volgende ingangen/uitgangen:
* BLOB-opslag
* Gebeurtenis Hubs
* SQL-Database
* Tabelopslag

###<a name="blob-storagetable-storage"></a>Opslag/tabel-BLOB-opslag
Herhaal deel 1 voor de toegangstoets die eerder werd gebruikt door de taak nu ongebruikte toegangstoets vernieuwen.

###<a name="event-hubs"></a>Gebeurtenis hubs
Deel 1 voor de sleutel die eerder werd gebruikt door de taak voor het vernieuwen van de nu niet-gebruikte toets herhalen.

###<a name="sql-database"></a>SQL-Database
1.  Ga terug naar het query-venster uit stap 7 van deel 1 en typt u de volgende query < previous_login_name > vervangen door de naam van de gebruiker die eerder werd gebruikt door uw taak:  
`DROP LOGIN <previous_login_name>`  
2.  Klik op uitvoeren:  
    ![graphic43][graphic43]  

Ontvangt u de bevestiging van het volgende: 

    Command(s) completed successfully.

## <a name="get-help"></a>Help-informatie opvragen
Probeer onze [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) voor verdere ondersteuning

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Analytics Azure Stream](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics beheer REST API: naslag](https://msdn.microsoft.com/library/azure/dn835031.aspx)


[graphic1]: ./media/stream-analytics-login-credentials-inputs-outputs/1-stream-analytics-login-credentials-inputs-outputs.png
[graphic2]: ./media/stream-analytics-login-credentials-inputs-outputs/2-stream-analytics-login-credentials-inputs-outputs.png
[graphic3]: ./media/stream-analytics-login-credentials-inputs-outputs/3-stream-analytics-login-credentials-inputs-outputs.png
[graphic4]: ./media/stream-analytics-login-credentials-inputs-outputs/4-stream-analytics-login-credentials-inputs-outputs.png
[graphic5]: ./media/stream-analytics-login-credentials-inputs-outputs/5-stream-analytics-login-credentials-inputs-outputs.png
[graphic6]: ./media/stream-analytics-login-credentials-inputs-outputs/6-stream-analytics-login-credentials-inputs-outputs.png
[graphic7]: ./media/stream-analytics-login-credentials-inputs-outputs/7-stream-analytics-login-credentials-inputs-outputs.png
[graphic8]: ./media/stream-analytics-login-credentials-inputs-outputs/8-stream-analytics-login-credentials-inputs-outputs.png
[graphic9]: ./media/stream-analytics-login-credentials-inputs-outputs/9-stream-analytics-login-credentials-inputs-outputs.png
[graphic10]: ./media/stream-analytics-login-credentials-inputs-outputs/10-stream-analytics-login-credentials-inputs-outputs.png
[graphic11]: ./media/stream-analytics-login-credentials-inputs-outputs/11-stream-analytics-login-credentials-inputs-outputs.png
[graphic12]: ./media/stream-analytics-login-credentials-inputs-outputs/12-stream-analytics-login-credentials-inputs-outputs.png
[graphic13]: ./media/stream-analytics-login-credentials-inputs-outputs/13-stream-analytics-login-credentials-inputs-outputs.png
[graphic14]: ./media/stream-analytics-login-credentials-inputs-outputs/14-stream-analytics-login-credentials-inputs-outputs.png
[graphic15]: ./media/stream-analytics-login-credentials-inputs-outputs/15-stream-analytics-login-credentials-inputs-outputs.png
[graphic16]: ./media/stream-analytics-login-credentials-inputs-outputs/16-stream-analytics-login-credentials-inputs-outputs.png
[graphic17]: ./media/stream-analytics-login-credentials-inputs-outputs/17-stream-analytics-login-credentials-inputs-outputs.png
[graphic18]: ./media/stream-analytics-login-credentials-inputs-outputs/18-stream-analytics-login-credentials-inputs-outputs.png
[graphic19]: ./media/stream-analytics-login-credentials-inputs-outputs/19-stream-analytics-login-credentials-inputs-outputs.png
[graphic20]: ./media/stream-analytics-login-credentials-inputs-outputs/20-stream-analytics-login-credentials-inputs-outputs.png
[graphic21]: ./media/stream-analytics-login-credentials-inputs-outputs/21-stream-analytics-login-credentials-inputs-outputs.png
[graphic22]: ./media/stream-analytics-login-credentials-inputs-outputs/22-stream-analytics-login-credentials-inputs-outputs.png
[graphic23]: ./media/stream-analytics-login-credentials-inputs-outputs/23-stream-analytics-login-credentials-inputs-outputs.png
[graphic24]: ./media/stream-analytics-login-credentials-inputs-outputs/24-stream-analytics-login-credentials-inputs-outputs.png
[graphic25]: ./media/stream-analytics-login-credentials-inputs-outputs/25-stream-analytics-login-credentials-inputs-outputs.png
[graphic26]: ./media/stream-analytics-login-credentials-inputs-outputs/26-stream-analytics-login-credentials-inputs-outputs.png
[graphic27]: ./media/stream-analytics-login-credentials-inputs-outputs/27-stream-analytics-login-credentials-inputs-outputs.png
[graphic28]: ./media/stream-analytics-login-credentials-inputs-outputs/28-stream-analytics-login-credentials-inputs-outputs.png
[graphic29]: ./media/stream-analytics-login-credentials-inputs-outputs/29-stream-analytics-login-credentials-inputs-outputs.png
[graphic30]: ./media/stream-analytics-login-credentials-inputs-outputs/30-stream-analytics-login-credentials-inputs-outputs.png
[graphic31]: ./media/stream-analytics-login-credentials-inputs-outputs/31-stream-analytics-login-credentials-inputs-outputs.png
[graphic32]: ./media/stream-analytics-login-credentials-inputs-outputs/32-stream-analytics-login-credentials-inputs-outputs.png
[graphic33]: ./media/stream-analytics-login-credentials-inputs-outputs/33-stream-analytics-login-credentials-inputs-outputs.png
[graphic34]: ./media/stream-analytics-login-credentials-inputs-outputs/34-stream-analytics-login-credentials-inputs-outputs.png
[graphic35]: ./media/stream-analytics-login-credentials-inputs-outputs/35-stream-analytics-login-credentials-inputs-outputs.png
[graphic36]: ./media/stream-analytics-login-credentials-inputs-outputs/36-stream-analytics-login-credentials-inputs-outputs.png
[graphic37]: ./media/stream-analytics-login-credentials-inputs-outputs/37-stream-analytics-login-credentials-inputs-outputs.png
[graphic38]: ./media/stream-analytics-login-credentials-inputs-outputs/38-stream-analytics-login-credentials-inputs-outputs.png
[graphic39]: ./media/stream-analytics-login-credentials-inputs-outputs/39-stream-analytics-login-credentials-inputs-outputs.png
[graphic40]: ./media/stream-analytics-login-credentials-inputs-outputs/40-stream-analytics-login-credentials-inputs-outputs.png
[graphic41]: ./media/stream-analytics-login-credentials-inputs-outputs/41-stream-analytics-login-credentials-inputs-outputs.png
[graphic42]: ./media/stream-analytics-login-credentials-inputs-outputs/42-stream-analytics-login-credentials-inputs-outputs.png
[graphic43]: ./media/stream-analytics-login-credentials-inputs-outputs/43-stream-analytics-login-credentials-inputs-outputs.png
 
