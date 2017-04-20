<properties
   pageTitle="Aan de slag met SQL gegevens magazijn bedreiging detectie"
   description="Om te beginnen met bedreiging detectie"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/24/2016"
   ms.author="lodipalm;sonyama;barbkess"/>


# <a name="get-started-with-threat-detection"></a>Aan de slag met bedreiging detectie

> [AZURE.SELECTOR]
- [Controle](sql-data-warehouse-auditing-overview.md)
- [Detectie van bedreiging](sql-data-warehouse-security-threat-detection.md)

## <a name="overview"></a>Overzicht

Detectie van bedreigingen detecteert afwijkende databaseactiviteiten die potentiële beveiligingsrisico's tot de database aangeeft. Detectie van bedreiging is in voorbeeld en wordt ondersteund voor SQL Data Warehouse.

Bedreiging detectie biedt een nieuwe laag van beveiliging waarmee klanten op te sporen en te reageren op potentiële bedreigingen, zoals die door middel van waarschuwingen op afwijkende activiteiten plaatsvinden. Gebruikers kunnen de verdachte gebeurtenissen met [Azure SQL gegevens magazijn controleren](sql-data-warehouse-auditing-overview.md) om te bepalen als zij uit een poging om te bekijken voortvloeien, in strijd is met of misbruik maken van gegevens in het gegevenswarehouse te verkennen.
Threat Detection kunt u eenvoudig naar adres potentiële bedreigingen voor het datawarehouse hoeft te worden een expert security of geavanceerde beveiliging bewakingssystemen beheren.

Detectie van bedreigingen detecteert bijvoorbeeld bepaalde activiteiten met een afwijkende database potentiële SQL injection pogingen die aangeeft. SQL injection is een van de algemene Web application beveiligingsproblemen op het Internet worden gebruikt voor aanvallen op gegevensgestuurde toepassingen. Aanvallers gebruikmaken van beveiligingslekken toepassing naar een schadelijke SQL-instructies in de invoervelden toepassing voor overtredingen of wijzigen van gegevens in de database invoeren.


## <a name="set-up-threat-detection-for-your-database"></a>Detectie van bedreigingen voor uw database instellen

1. Start de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

2. Ga naar het blad van de configuratie van de SQL-datawarehouse die u wilt controleren. Selecteer in de blade instellingen **controle & opsporing bedreiging**.

    ![Navigatiedeelvenster][1]

3. In de configuratie **controleren & bedreiging detectie** blade schakelen **op** controle, die de bedreiging detectie-instellingen wordt weergegeven.

    ![Navigatiedeelvenster][2]

4. **Op** bedreiging detectie inschakelen.

5. De lijst van e-mailberichten die u waarschuwingen na detectie van magazijnactiviteiten afwijkende gegevens ontvangt configureren.

6. Klik op **Opslaan** in de bladeserver **controleren & bedreiging detectie** configuratie opslaan de nieuwe of bijgewerkte controle en detectie beleid dreiging.

    ![Navigatiedeelvenster][3]


## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a>Ontdek de magazijnactiviteiten afwijkende gegevens na detectie van een verdachte gebeurtenis

1. U ontvangt een e-mailbericht na detectie van afwijkende databaseactiviteiten. <br/>
Het e-mailbericht zal informatie verschaffen over de verdachte gebeurtenis met inbegrip van de aard van de activiteiten van afwijkende, databasenaam, servernaam en de tijd van de gebeurtenis. Bovendien geven informatie over de mogelijke oorzaken en aanbevolen acties onderzoeken en het potentiële gevaar voor de database te beperken.<br/>

    ![Navigatiedeelvenster][4]

2. In de e-mail, klik u op de koppeling **Azure SQL-controle logboek** , de klassieke Azure-Portal te starten en de relevante records controleren rond de tijd van de verdachte gebeurtenis weer te geven.

    ![Navigatiedeelvenster][5]

3. Klik op de controlerecords voor meer meer informatie over de voor verdachte databaseactiviteiten zoals SQL-instructie fout reden en client-IP.

    ![Navigatiedeelvenster][6]

4. In de blade Records controleren, klikt u op **openen in Excel** opent een vooraf geconfigureerde excel-sjabloon als u wilt importeren en grondigere analyse van het controlelogboek rond de tijd van de verdachte gebeurtenis uitgevoerd.<br/>
**Opmerking:** In Excel 2010 of hoger is vereist Power Query en de instelling **Snel combineren**

    ![Navigatiedeelvenster][7]

5. De instelling **Snel combineren** - configureren In het tabblad van het lint **POWER QUERY** , selecteert u **Opties** om het dialoogvenster opties weer te geven. Selecteer de sectie Privacy en kies de tweede optie - 'De privacyniveaus negeren en de prestaties mogelijk verbeteren':

    ![Navigatiedeelvenster][8]

6. Voor het laden van controlelogboeken SQL, ervoor te zorgen dat de parameters in de instellingen correct zijn ingesteld tabblad en selecteer vervolgens het lint 'Data' en klikt u op Alles vernieuwen.

    ![Navigatiedeelvenster][9]

7. De resultaten worden weergegeven in het **SQL-controlelogboeken** blad waarin u kunt het uitvoeren van grondigere analyse van de afwijkende activiteiten die zijn aangetroffen en het effect van de beveiligingsgebeurtenis in uw toepassing.


<!--Image references-->
[1]: ./media/sql-data-warehouse-security-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-data-warehouse-security-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-data-warehouse-security-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-data-warehouse-security-threat-detection/4_td_email.png
[5]: ./media/sql-data-warehouse-security-threat-detection/5_td_audit_records.png
[6]: ./media/sql-data-warehouse-security-threat-detection/6_td_audit_record_details.png
[7]: ./media/sql-data-warehouse-security-threat-detection/7_td_audit_records_open_excel.png
[8]: ./media/sql-data-warehouse-security-threat-detection/8_td_excel_fast_combine.png
[9]: ./media/sql-data-warehouse-security-threat-detection/9_td_excel_parameters.png
