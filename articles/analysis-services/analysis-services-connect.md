<properties
   pageTitle="Gegevens ophalen uit Azure Analysis Services | Microsoft Azure"
   description="Informatie over verbinding maken met en gegevens ophalen uit een Analysis Services-server in Azure."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="get-data-from-azure-analysis-services"></a>Gegevens ophalen uit Azure Analysis Services
Zodra u hebt gemaakt in Azure en geïmplementeerd in tabelvorm model aan, zijn gebruikers in uw organisatie klaar om te verbinden en gebruiken van gegevens.

Azure Analysis Services ondersteunt verbindingen van clients met behulp van [objectmodellen bijgewerkt](#client-libraries); TOM, AMO, Adomd.Net of MSOLAP, via xmla naar de server te verbinden. Bijvoorbeeld Power BI, Power BI-Desktop, Excel of een clienttoepassing van derden die de objectmodellen ondersteunt.

## <a name="server-name"></a>Naam van de server
Wanneer u een Analysis Services-server in Azure maken, geeft u een unieke naam en de regio waar de server wordt gemaakt. Wanneer u de naam van de server opgeeft in een verbinding, wordt het schema voor naamgeving van server:
```
<protocol>://<region>/<servername>
```
 Wanneer het protocol is een reeks **asazure**, het gebied is de Uri van de regio waar de server is gemaakt (bijvoorbeeld voor de US West, westus.asazure.windows.net) en servernaam is de naam van uw server uniek in de regio.

## <a name="get-the-server-name"></a>De naam van de server ophalen
Voordat u verbinding maakt, moet u de naam van de server ophalen. In **Azure portal** > server > **Overzicht** > de**naam van de Server**, de naam van de hele server kopiëren. Als andere gebruikers in uw organisatie te met deze server verbinden zijn, moet u de naam van de server met hen delen. Wanneer u een servernaam opgeeft, kan het volledige pad moet worden gebruikt.

![Naam van de server ophalen in Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)


## <a name="connect-in-power-bi-desktop"></a>Verbinding in Power BI-Desktop

> [AZURE.NOTE] Deze functie is een voorbeeld.

1. Klik op **Gegevens ophalen**in [Power BI-Desktop](https://powerbi.microsoft.com/desktop/), > **Databases** > **Azure Analysis Services**.

2. In het vak **Server**de naam van de server vanaf het Klembord te plakken.

3. In de **Database**, als u de naam van de modeldatabase in tabelvorm of perspectief die u verbinding wilt maken, plak deze hier. Anders kunt u dit veld leeg laten. U kunt een database of een perspectief op het volgende scherm.

4. Laat de optie **verbinding maken met live** geselecteerd en klik op **verbinden**. Als u wordt gevraagd een rekening invoeren, Voer uw organisatie-account.

5. Vouw de server uit, in **Navigator**en vervolgens selecteert u het model of het perspectief dat u verbinding wilt maken en klik vervolgens op **verbinden**. Een enkele klik op een model of perspectief ziet u de objecten voor die weergave.


## <a name="connect-in-power-bi"></a>Verbinding in Power BI
1. Maak een bestand Power BI-bureaublad met een live verbinding met uw model op uw server.

2. Klik op **Gegevens ophalen**in [Power BI](https://powerbi.microsoft.com), > **bestanden**. Zoek en selecteer het bestand.


## <a name="connect-in-excel"></a>Verbinding maken in Excel
Verbinding maken met Azure Analysis Services-server in Excel wordt met behulp van de gegevens ophalen in Excel 2016 of Power Query in eerdere versies ondersteund. [Msolap.7-provider](https://aka.ms/msolap) is vereist. Verbinding maken met behulp van de Wizard importeren in Power Pivot wordt niet ondersteund.

1. Klik op het lint **gegevens** in Excel 2016, **Externe gegevens ophalen** > **Van andere bronnen** > **Van Analysis Services**.

2. Plakken in de Wizard Gegevensverbinding in het vak **servernaam**de naam van de server vanaf het Klembord. **Aanmeldingsreferenties**, selecteer in **de volgende gebruikersnaam en wachtwoord**, en typ de naam van de organisatie-gebruiker, bijvoorbeeld nancy@adventureworks.com, en het wachtwoord.

    ![In Excel aanmelding verbinden](./media/analysis-services-connect/aas-connect-excel-logon.png)

4. Selecteer de database en het model of het perspectief in de **Database en tabel selecteren**en klik op **Voltooien**.

    ![Verbinding maken in Excel Selecteer model](./media/analysis-services-connect/aas-connect-excel-select.png)

## <a name="connection-string"></a>Verbindingsreeks
Wanneer u verbinding maakt met Azure Analysis Services in tabelvorm objectmodel gebruiken met de volgende verbindingsreeks wordt opgemaakt:

###### <a name="integrated-azure-active-directory-authentication"></a>Geïntegreerde verificatie van Active Directory Azure
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```
Geïntegreerde verificatie van de cache van de referentie Azure Active Directory als deze beschikbaar komt ophalen. Als dat niet het geval is, wordt de Azure inlogscherm wordt weergegeven.

###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Azure Active Directory-verificatie met gebruikersnaam en wachtwoord
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

## <a name="client-libraries"></a>Clientbibliotheken
Wanneer u verbinding maakt met Azure Analysis Services vanuit Excel of andere interfaces zoals TOM, AsCmd, ADOMD.NET, wellicht moet u de laatste provider clientbibliotheken installeren. Haal de meest recente:  

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>



## <a name="next-steps"></a>Volgende stappen
[Deze server beheren](analysis-services-manage.md)
