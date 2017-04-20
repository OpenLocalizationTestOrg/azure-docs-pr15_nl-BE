<properties
   pageTitle="Release-opmerkingen voor Azure gegevenscatalogus | Microsoft Azure"
   description="Release-opmerkingen voor Azure gegevenscatalogus."
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
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-release-notes"></a>Azure catalogus gegevens release-opmerkingen

## <a name="notes-for-the-november-20-2015-release-of-azure-data-catalog"></a>Informatie voor 20 November 2015-release van Azure gegevenscatalogus

### <a name="opening-data-sources-in-power-bi-desktop"></a>Gegevensbronnen openen in Power BI-Desktop

Wanneer u de optie 'Openen in Power BI Desktop' via de portal **Azure gegevenscatalogus** , ondervinden gebruikers een van de volgende problemen in de toepassing voor Power BI bureaublad:

- Een dialoogvenster met de titel 'Kan niet naar Document openen' wordt weergegeven.
- De Power BI Desktop-toepassing wordt geopend, maar het bestand is leeg

Voor elke situatie, kan het probleem worden opgelost door het downloaden en installeren van de meest recente versie van Power BI Desktop van [PowerBI.com](https://powerbi.com).

## <a name="notes-for-the-november-13-2015-release-of-azure-data-catalog"></a>Informatie voor 13 November 2015-release van Azure gegevenscatalogus

### <a name="registering-and-connecting-to-teradata"></a>Registreren en verbinding te maken met Teradata

Wanneer u verbinding maakt met Teradata gegevensbronnen gebruikers moeten de juiste Teradata ODBC-stuurprogramma hebt geïnstalleerd die overeenkomen met de bitness (32-bits of 64-bits) van de software wordt gebruikt.

Het meest recente [Teradata ODBC-stuurprogramma voor windows (versie 15.10)](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) vanaf deze datum ADC-versie is compatibel met Office 2013, maar niet met Office 2016.

## <a name="notes-for-the-july-13-2015-release-of-azure-data-catalog"></a>Informatie voor 13 juli 2015-release van Azure gegevenscatalogus

### <a name="registering-and-connecting-to-oracle-database"></a>Registreren en verbinding maken met een Oracle-Database

Wanneer een verbinding met gegevensbronnen voor Oracle Database gebruikers moeten beschikken over de juiste Oracle-stuurprogramma's die overeenkomen met de bitness (32-bits of 64-bits) van de software wordt gebruikt.

-   De Oracle-stuurprogramma's voor 32-bits wordt gebruikt bij het registreren van Oracle-gegevensbronnen op een computer met de 32-bits Windows
-   De 64-bits Oracle-stuurprogramma wordt gebruikt tijdens het registreren van Oracle-gegevensbronnen op een computer met 64-bits Windows
-   Wanneer zij verbinding maken met Oracle-gegevensbronnen op een computer met de 32-bits versie van Microsoft Office met behulp van Excel, wordt inclusief de 64-bits versie van Windows, het Oracle-stuurprogramma's voor 32-bits gebruikt
-   Wanneer u verbinding maakt met Oracle-gegevensbronnen op een computer met de 64-bits versie van Microsoft Office met behulp van Excel, worden de 64-bits Oracle-stuurprogramma's gebruikt

### <a name="registering-and-connecting-to-sql-server-reporting-services"></a>Registreren en verbinding te maken met SQL Server Reporting Services

Ondersteuning voor gegevensbronnen voor SQL Server Reporting Services (SSRS) is momenteel beperkt tot alleen Native-modus-servers. Ondersteuning voor SQL Server Reporting Services in SharePoint-modus wordt toegevoegd in een latere versie.

### <a name="opening-data-assets-in-excel"></a>Activa van gegevens openen in Excel

Wanneer de activa van de gegevens uit de **Catalogus met Azure gegevens** portal in Microsoft Excel opent, kunnen gebruikers een **Beveiligingsbericht van Microsoft Excel** -dialoogvenster gevraagd. Dit is standaard, verwacht gedrag en gebruikers kunnen selecteren **inschakelen** om door te gaan.

Zie [in- of uitschakelen van waarschuwingen over koppelingen en bestanden van verdachte websites](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE)voor meer informatie.

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a>Proxy- en configuratie- en bron registratie

Gebruikers kunnen een situatie waar ze aanmelden kunnen op de portal catalogus Azure-gegevens, maar wanneer ze proberen aan te melden op de data source registratie tool die zij een foutmelding dat verhindert dat ze zich aanmelden ontvangt op ondervinden.

Er zijn twee mogelijke oorzaken voor dit probleem probleem:

**Oorzaak 1: de configuratie van Active Directory Federation Services** Het hulpprogramma voor de registratie van gegevens wordt Forms-verificatie gebruikt voor het valideren van aanmeldingen van gebruikers met Active Directory. Voor een geslaagde aanmelding moet Forms-verificatie zijn ingeschakeld in het globaal beleid voor verificatie door de beheerder van Active Directory.

In sommige gevallen hebben kan deze fout gebeuren wanneer de gebruiker op het netwerk van het bedrijf, of alleen wanneer de gebruiker verbinding maakt vanaf buiten het bedrijfsnetwerk. Het globaal beleid voor verificatie kunt verificatiemethoden moeten afzonderlijk worden ingeschakeld voor het intranet en extranet-verbindingen. Aanmeldingsfouten kunnen optreden als de verificatie van formulieren is niet ingeschakeld voor het netwerk van waaruit de gebruiker verbinding maakt.

Zie [Beleid voor verificatie configureren](https://technet.microsoft.com/library/dn486781.aspx)voor meer informatie.

**Oorzaak 2: configuratie van netwerk proxy** Als het netwerk een proxyserver gebruikt, het hulpprogramma voor inschrijving niet mogelijk verbinding kunnen maken met Azure Active Directory via de proxy. Gebruikers kunnen ervoor zorgen dat het hulpprogramma voor inschrijving door het bewerken van het hulpprogramma configuratiebestand, deze sectie toe te voegen aan het bestand:


      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


Zoek het bestand RegistrationTool.exe.config en open vervolgens het hulpprogramma Windows Taakbeheer starten van het hulpprogramma voor inschrijving. Op het tabblad Details in Taakbeheer door met de rechtermuisknop op RegistrationTool.exe en bestandslocatie openen kiest in het pop-upmenu.
