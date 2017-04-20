<properties
    pageTitle="Een Provider Web Apps Resource toevoegen aan Azure Stack | Microsoft Azure"
    description="Gedetailleerde richtlijnen voor het implementeren van Web Apps in Azure stapel"
    services="azure-stack"
    documentationCenter=""
    authors="ccompy, apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>

# <a name="add-a-web-apps-resource-provider-to-azure-stack"></a>Een provider Apps Web resource toevoegen aan Azure Stack

> [AZURE.NOTE] De volgende informatie geldt alleen voor Azure Stack TP1-implementaties.

Een Provider Web Apps Resource toevoegen aan Azure stapel heeft zeven stappen:

1.  Vereiste onderdelen downloaden.
2.  Certificaten worden gebruikt door Azure Stack Web Apps te maken.
3.  Gebruik het installatieprogramma te downloaden, stage en Azure Stack Web Apps installeren. 
4.  Web Apps-installatie valideren.
5.  DNS-records voor de Server voor beheer van netwerktaakverdeling en het Front-End maken.
6.  De onlangs geïmplementeerde Web Apps resource provider met ARM registreren.
7.  Test Drive de voorziening Web Apps-bron.

## <a name="download-required-components"></a>Vereiste onderdelen downloaden

1.  Download het [voorbeeld-installer Azure Stack App-Service](http://aka.ms/azasinstaller). 
2.  [Azure Stack App Service implementatie helper scripts](http://aka.ms/azashelper)downloaden. 
3.  Pak de bestanden in het zip-bestand van de helper-scripts, moet er drie scripts:
    - Maak AppServiceCerts.ps1
    - Maak AppServiceDnsRecords.ps1
    - Register AppServiceResourceProvider.ps1 

## <a name="create-certificates-to-be-used-by-azure-stack-web-apps"></a>Certificaten worden gebruikt door Azure Stack Web Apps maken

Dit eerste script werkt met de certificeringsinstantie Azure stapel 3 certificaten die nodig zijn voor het Web Apps te maken. Het script uitvoeren op de ClientVM ervoor te zorgen dat PowerShell wordt uitgevoerd als azurestack\administrator:
1.  Uitvoeren in een PowerShell-sessie die wordt uitgevoerd als **azurestack\administrator**, het script **Maken AppServiceCerts.ps1** .  Hiermee maakt u drie certificaten, in dezelfde map als de scripts die nodig zijn voor het Web Apps.
2.  Voer een wachtwoord voor de beveiliging van het pfx-bestanden en noteer deze naar wens in te voeren in het installatieprogramma Azure Stack Web Apps.

## <a name="use-the-installer-to-download-and-install-azure-stack-web-apps"></a>Gebruik het installatieprogramma downloaden en installeren van Azure Stack Web Apps

Het installatieprogramma appservice.exe wordt:
1.  De gebruiker gevraagd de Microsoft en derden gebruiksrechtovereenkomsten te accepteren.
2.  Informatie over de implementatie van Azure Stack verzamelen.
3.  Maak een blob-container in de Stack Azure opslag hebt opgegeven.
4.  Download de bestanden die nodig zijn voor de provider van de resource Azure Stack Web App installeren.
5.  Bereid de installatie voor de implementatie van de Web App resource provider in de Stack Azure-omgeving.
6.  De bestanden uploaden naar de App-opslag-account.
7.  Informatie presenteren die nodig zijn voor de sjabloon Azure Resource Manager starten.

De volgende stappen begeleiden u bij de stadia van de installatie:

>[AZURE.NOTE] Een verhoogde account (lokaal of domein administrator) moet u het installatieprogramma uitvoert. Als u zich als azurestack\azurestackuser aanmeldt, wordt u gevraagd om referenties verhoogde. 

1.  Appservice.exe als **azurestack\administrator**uitgevoerd. 
2.  Klik op **Deploy Azure Resource Manager gebruiken**.

![Azure Stack App Service technische Preview 1 Installer][1]

3.  Lees en accepteer de licentievoorwaarden voor Microsoft-Software voorlopige en klik op **volgende**.
4.  Lees en accepteer de voorwaarden van de derde partylicense en klik op **volgende**.
5.  De configuratiegegevens van het App Cloud-Service en klik op **volgende**.

![Azure Stack App Service technische Preview 1 App Cloud serviceconfiguratie][2]

6. Klik op **verbinding maken** (naast het vak Azure Stack abonnementen).

![Azure Stack App Service technische Preview 1 App Cloud Configuratiescherm Service twee][3]

7.  In het venster Azure Stack verificatie uw **Azure Active Directory Service Admin-account** en **wachtwoord**opgeven en klik vervolgens op **Aanmelden**.
**Opmerking:** Dit is de Azure Active Directory-account die u verstrekt wanneer u de stapel Azure geïmplementeerd.
    - Klik op de **Pijl-omlaag** aan de rechterkant van het vak naast **Azure Stack abonnementen** en selecteer vervolgens uw abonnement.

![Azure Stack App Service technische Preview 1 abonnement selectie][5]

8.  Klik op de **Pijl-omlaag** aan de rechterkant van het vak naast **Azure stacklocaties**.
    - Selecteer **lokale**.
9. Voer de **naam** van de beheerder.
10. Geef een **wachtwoord** voor de beheerder.
11. De **SQL Server-gegevens** en indien nodig wijzigingen aanbrengen.
12. De **Aanmeldingsaccount SysAdmin** controleren en indien nodig wijzigingen aanbrengen.
13. Voer het **Wachtwoord van de systeembeheerder**.
14. Klik op **volgende**.  Op dit moment het installatieprogramma nu controleert of de verbindingsgegevens voor SQL Server geleverd.

![Azure Stack App Service technische Preview 1 abonnement selectie][4]    

15. Klik op **Bladeren** naast het **Web Apps standaard SSL certificaat-bestand** en Ga naar de **webapps. AzureStack.Local** certificaat [eerder hebt gemaakt](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps).
16. Voer het **wachtwoord voor het certificaat** dat u instelt wanneer u de certificaten hebt gemaakt.
17. Klik op **Bladeren** naast het **Bronbestand Provider SSL certificaat** en Ga naar het beheer van **. AzureStack.Local** certificaat [eerder hebt gemaakt](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps).
18. Voer het **wachtwoord voor het certificaat** dat u instelt wanneer u de certificaten hebt gemaakt.
19. Klik op **Bladeren** naast het **Bronbestand Provider Root certificaat** en Ga naar het beheer van **. AzureStack.Local** certificaat [eerder hebt gemaakt](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps).
20. Klik op **volgende** controleert het installatieprogramma of het opgegeven wachtwoord voor het certificaat.

![Azure Stack App technische Preview 1 certificaat servicedetails][6]

De implementatie duurt ongeveer 45-60 minuten om te voltooien.

![Voortgang van Azure Stack App Service technische Preview 1 installatie][7]

21. Klik op **Afsluiten**nadat de installer met succes is voltooid.

## <a name="validate-web-apps-installation"></a>Web Apps-installatie valideren

1.  Open **Hyper-V-beheer**op uw **Hostcomputer Azure-Stack** .
2.  Zoek de **CN0 VM** en **verbinding maken** met de VM.
![Azure Stack App Service technische Preview 1 Hyper-V-beheer][8]
3.  Op het bureaublad van deze VM dubbelklikt u op het **Web Cloud Management Console**.
![Azure Stack App Service technische Preview 1 beheerconsole][9]
4.  Ga naar **Servers beheerd**.
5.  Wanneer alle machines zijn doorgaan **nu** naar de volgende stap. 
![Azure Stack App technische Preview 1 beheerde Servers servicestatus][10]

## <a name="create-dns-records-for-the-management-server-and-front-end-load-balancers"></a>DNS-records voor de Server voor documentbeheer en netwerktaakverdeling-Front-End maken
1.  Open een PowerShell-exemplaar als **azurestack\administrator**.
2.  Navigeer naar de locatie van de scripts gedownload en uitgepakt in de [vereiste stap](#Download-Required-Components).
3.  Voer het script **Maken AppServiceDnsRecords.ps1** , Hiermee worden DNS-vermeldingen, zodat de portal en web app aanvragen naar de Front-End-Servers worden doorgestuurd.  Tijdens de implementatie van Web-Apps, Netwerktaakverdeling twee Software (SLBs), ARM zijn in de netwerkprovider voor de resource gemaakt. Ze verwijzen naar de Servers voor het beheer en de front-end servers. De portal en ARM gebaseerde verzoeken de Azure Stack App resource dienstverlener gaat u naar de Server.

## <a name="register-the-newly-deployed-azure-stack-web-apps-provider-with-arm"></a>Registreren van de onlangs geïmplementeerde Azure Stack Web Apps-provider met ARM
1.  Open een PowerShell-exemplaar als **azurestack\administrator**.
2.  Navigeer naar de locatie van de scripts gedownload en uitgepakt in de [vereiste stap](#Download-Required-Components).
3.  Het **Register AppServiceResourceProvider.ps1** script uitvoeren. 

>[AZURE.NOTE] Type de gebruikersnaam en het wachtwoord **(inclusief hoofdletters en kleine letters) exact** is ingevoerd voor de **Beheerder van de Virtual Machine(s)** en **wachtwoord** -velden in de installatie of uw provider resource registreren, mislukken.

## <a name="test-drive-azure-stack-web-apps"></a>Test station Azure Stack Web Apps

Nu dat u hebt geïmplementeerd en de resource Web Apps provider geregistreerd, kunt u het testen om ervoor te zorgen dat huurders web apps kunnen implementeren.

1.  In de portal Azure Stack klikt u op Nieuw, klikt u op het Web + mobiele en Web App op.
2.  Typ een naam in het vak Web app in de blade Web App.
3.  Onder de resourcegroep, klikt u op Nieuw en typ vervolgens een naam in het vak bronnengroep. 
4.  App plan/locatie en klik op Nieuw.
5.  Typ een naam in het plan App Service in de App Service plan-blade.
6.  Klik op prijzen laag, klik op vrije gedeeld of gedeeld gedeelde Klik op selecteren, klikt u op OK en klik vervolgens op maken.
7.  In minder dan een minuut, wordt een steen voor de nieuwe web app weergegeven op het Dashboard. Klik op de tegel.
8.  Klik op Bladeren om de standaardwebsite voor deze toepassing weer te geven in de web app-blade.


**Implementeren van een website WordPress, DNN of Django (optioneel)**

1. In de stapel Azure portal, klikt u op "+" gaat implementeren op de markt Azure een Django-website en wachten op de voltooiing. Het Django webplatform wordt een bestand gebaseerde database gebruikt en hoeft niet alle providers extra bronnen zoals SQL of MySQL.  

2. Als u ook een resource MySQL provider hebt geïmplementeerd, kunt u een WordPress website van Marketplace kunt implementeren. Als de naam van de gebruiker worden ingevoerd wanneer u wordt gevraagd voor databaseparameters, *User1@Server1* (met de gebruikersnaam en de naam van de server van uw keuze).

3. Als u ook een resource-provider voor SQL Server hebt geïmplementeerd, kunt u een website van de DNN uit de markt te implementeren. Wanneer u wordt gevraagd voor databaseparameters, wordt een database kiezen in de computer met SQL Server die is verbonden met uw provider voor de resource.

## <a name="next-steps"></a>Volgende stappen

U kunt ook andere [platform als een service (PaaS)-services](azure-stack-tools-paas-services.md), zoals de [resource-provider voor SQL Server](azure-stack-sql-rp-deploy-short.md) en [MySQL resource provider](azure-stack-mysql-rp-deploy-short.md)uitproberen.

<!--Image references-->
[1]: ./media/azure-stack-webapps-deploy/AppService_exe_Start.png
[2]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep1.png
[3]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2.png
[4]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2_populated.png
[5]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2_SubscriptionSelection.png
[6]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep3_Certificates.png
[7]: ./media/azure-stack-webapps-deploy/AppService_exe_InstallationProgress.png
[8]: ./media/azure-stack-webapps-deploy/HyperV.png
[9]: ./media/azure-stack-webapps-deploy/MMC.png
[10]: ./media/azure-stack-webapps-deploy/ManagedServers.png


<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[WebAppsDeployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
