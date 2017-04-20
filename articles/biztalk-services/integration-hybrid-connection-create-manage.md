<properties 
    pageTitle="Hybride verbindingen maken en beheren | Microsoft Azure" 
    description="Informatie over hoe een hybride verbinding maken, beheren van de verbinding en de hybride Verbindingsbeheer installeren. MAB, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="ccompy"/>


# <a name="create-and-manage-hybrid-connections"></a>Hybride verbindingen maken en beheren


## <a name="overview-of-the-steps"></a>Overzicht van de stappen
1. Een hybride verbinding maken door te voeren van de **hostnaam** of **FQDN-naam** van de resource op de ruimten in uw particuliere netwerk.
2. Uw Azure web apps of Azure mobiele apps koppelen aan de hybride-verbinding.
3. De hybride Connection Manager installeren op de resource in de lokalen en verbinding maken met de specifieke hybride verbinding. De Azure portal biedt een ervaring met één klik installeren en aansluiten.
4. Hybride verbindingen en hun verbindingssleutels beheren.

In dit onderwerp worden de volgende stappen uit. 

> [AZURE.IMPORTANT] Het is een eindpunt hybride verbinding instellen met een IP-adres. Als u een IP-adres gebruikt, moet u mogelijk of bereikt mogelijk niet de resource op gebouwen, afhankelijk van de client. De hybride verbinding is afhankelijk van de client een DNS-zoekopdracht uitvoeren. In de meeste gevallen is de __client__ de toepassingscode. Als de client geen een DNS-lookup voert (het probeert niet om op te lossen het IP-adres als een domeinnaam (x.x.x.x)), en vervolgens het verkeer niet verzonden via de verbinding hybride.
>
> Bijvoorbeeld (in pseudo-code) kunt u **10.4.5.6** definiëren als de host op ruimten:
> 
> **In het volgende scenario werkt:**  
> `Application code -> GetHostByName("10.4.5.6") -> Resolves to 127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on-prem host`
> 
> **In het volgende scenario werkt niet:**  
> `Application code -> Connect("10.4.5.6") -> ?? -> No route to host`


## <a name="CreateHybridConnection"></a>Een hybride verbinding maken

Een hybride verbinding kan worden gemaakt in de Azure portal Web Apps **of** met behulp van BizTalk-Services. 

**Hybride verbindingen met behulp van Web Apps maken**, Zie [Azure Web Apps verbinding maken met een On-Premises-bron](../app-service-web/web-sites-hybrid-connection-get-started.md). U kunt ook de hybride Connection Manager (HCM) installeren in uw web-app, de voorkeur verdient. 

**Hybride verbindingen in BizTalk-Services maken**:

1. Log in om de [Azure klassieke portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. In het linkernavigatievenster **BizTalk-Services** selecteren en selecteer vervolgens uw BizTalk-Service. 

    Als u een bestaande BizTalk-Service niet hebt, kunt u [een BizTalk-Service maken](biztalk-provision-services.md).
3. Selecteer het tabblad **Hybride verbindingen** :  
![Tabblad hybride-verbindingen][HybridConnectionTab]

4. Selecteer **een hybride verbinding maken** of klik op de knop **toevoegen** in de taakbalk. Voer de volgende gegevens:

    Eigenschap | Beschrijving
--- | ---
Naam | De naam van de hybride moet uniek zijn en mag niet dezelfde naam als de BizTalk-Service. U kunt een willekeurige naam opgeven maar met het doel ervan. Voorbeelden zijn:<br/><br/>Salarissen,*SQL Server*<br/>SupplyList*SharepointServer*<br/>Klanten*OracleServer*
Host-naam | Voer de volledige hostnaam, de naam van de host of het IPv4-adres van de resource in de lokalen. Voorbeelden zijn:<br/><br/>mySQLServer<br/>*mySQLServer*. *Domein*. corp.*uwbedrijf*.com<br/>*myHTTPSharePointServer*<br/>*myHTTPSharePointServer*. *uw bedrijf*.com<br/>10.100.10.10<br/><br/>Als u het IPv4-adres gebruikt, houd er rekening mee dat uw code met client- of het IP-adres mogelijk niet opgelost. Zie de belangrijke opmerking aan het begin van dit onderwerp.
Poort | Voer het poortnummer van de resource op gebouwen. Als u Web Apps gebruikt, voert u bijvoorbeeld poort 80 of poort 443. Als u SQL Server gebruikt, voert u de poort 1433.

5. Schakel het selectievakje in om de installatie te voltooien. 

#### <a name="additional"></a>Extra

- Meerdere hybride verbindingen kunnen worden gemaakt. Zie de [BizTalk-Services: edities grafiek](biztalk-editions-feature-chart.md) voor het aantal toegestane verbindingen. 
- Elke hybride verbinding wordt gemaakt met een paar verbindingstekenreeksen: toepassing toetsen die verzenden en On-premises sleutels die LUISTEREN. Elk paar heeft een primaire en een secundaire sleutel. 


## <a name="LinkWebSite"></a>Koppel uw Azure App Service Web App of mobiele App

Om een Web App of mobiele App in Azure App-Service een koppeling naar een bestaande verbinding met hybride, selecteert u **Gebruik een bestaande verbinding met hybride** in de blade hybride verbindingen. Zie [toegang op-premises bronnen met behulp van hybride verbindingen in Azure App-Service](../app-service-web/web-sites-hybrid-connection-get-started.md).

## <a name="InstallHCM"></a>Installeer het Verbindingsbeheer hybride on-premises

Nadat een hybride verbinding is gemaakt, installeert de hybride Connection Manager op de bron in de lokalen. Worden kan gedownload vanaf uw Azure web apps of de BizTalk-Service. Stappen voor BizTalk-Services: 

1. Log in om de [Azure klassieke portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. In het linkernavigatievenster **BizTalk-Services** selecteren en selecteer vervolgens uw BizTalk-Service. 
3. Selecteer het tabblad **Hybride verbindingen** :  
![Tabblad hybride-verbindingen][HybridConnectionTab]
4. Selecteer in de taakbalk **On-Premises Setup**:  
![On-Premises Setup][HCOnPremSetup]
5. Selecteer **installeren en configureren** uit te voeren of de hybride Connection Manager op het systeem op locatie te downloaden. 
6. Schakel het selectievakje in om de installatie te starten. 

<!--
You can also download the Hybrid Connection Manager MSI file and copy the file to your on-premises resource. Specific steps:

1. Copy the on-premises primary Connection String. See [Manage Hybrid Connections](#ManageHybridConnection) in this topic for the specific steps.
2. Download the Hybrid Connection Manager MSI file. 
3. On the on-premises resource, install the Hybrid Connection Manager from the MSI file. 
4. Using Windows PowerShell, type: 
> Add-HybridConnection -ConnectionString “*Your On-Premises Connection String that you copied*” 
--> 

#### <a name="additional"></a>Extra
- Hybride Connection Manager kan worden geïnstalleerd op de volgende besturingssystemen:

    - Windows Server 2008 R2 (.NET Framework 4.5 + en Windows Management Framework 4.0 + vereist)
    - Windows Server 2012 (Windows Management Framework 4.0 + vereist)
    - Windows Server 2012 R2


- Na de installatie van Verbindingsbeheer hybride, gebeurt het volgende: 

    - De hybride verbinding op Azure wordt automatisch geconfigureerd voor het gebruik van de verbindingsreeks van de primaire toepassing. 
    - De On-Premises resource wordt automatisch geconfigureerd voor het gebruik van de primaire On-Premises-verbindingsreeks.

- De hybride Connection Manager moet een verbindingsreeks op ruimten is geldig voor verificatie gebruiken. De Azure Web Apps of mobiele Apps moet de verbindingsreeks van een geldige aanvraag voor een vergunning gebruiken.
- Door een ander exemplaar van de hybride-Verbindingsbeheer installeren op een andere server kunt u de schaal hybride verbindingen. Configureer de listener op lokalen voor het gebruik van hetzelfde adres als de eerste op gebouwen-listener. In dit geval is het verkeer willekeurig verdeelde (round robin) tussen de listeners actief op gebouwen. 


## <a name="ManageHybridConnection"></a>Hybride verbindingen beheren
Voor het beheren van uw hybride verbindingen, kunt u het volgende doen:

- De Azure portal gebruiken en gaat u met uw BizTalk-Service. 
- [REST API's](http://msdn.microsoft.com/library/azure/dn232347.aspx)gebruiken.

#### <a name="copyregenerate-the-hybrid-connection-strings"></a>Kopie/opnieuw genereren de hybride verbindingsreeksen

1. Log in om de [Azure klassieke portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. In het linkernavigatievenster **BizTalk-Services** selecteren en selecteer vervolgens uw BizTalk-Service. 
3. Selecteer het tabblad **Hybride verbindingen** :  
![Tabblad hybride-verbindingen][HybridConnectionTab]
4. De hybride verbinding selecteren. Selecteer in de taakbalk, **Verbinding beheren**:  
![Opties beheren][HCManageConnection]

    **Verbinding beheren** geeft een overzicht van de toepassing en On-Premises verbindingsreeksen. U kunt de verbindingsreeksen kopiëren of opnieuw genereren van de toegangstoets gebruikt in de verbindingsreeks. 

    **Als u het opnieuw genereren**, de toegang tot gedeelde sleutel in de verbindingsreeks gebruikt wordt gewijzigd. Voer de volgende handelingen uit:
    - Selecteer in de Azure klassieke portal **Sync sleutels** in de toepassing Azure.
    - De **On-Premises Setup**opnieuw uitvoeren. Wanneer u de On-Premises Setup opnieuw uitvoert, wordt de resource op ruimten automatisch geconfigureerd voor het gebruik van de bijgewerkte primaire verbindingsreeks.


#### <a name="use-group-policy-to-control-the-on-premises-resources-used-by-a-hybrid-connection"></a>Gebruik Groepsbeleid om te bepalen op ruimten bronnen gebruikt door de verbinding van een hybride

1. Download de [Beheersjablonen hybride Connection Manager](http://www.microsoft.com/download/details.aspx?id=42963).
2. Pak de bestanden.
3. Op de computer waarop het groepsbeleid wordt gewijzigd, het volgende doen:  

    - Kopie de. ADMX-bestanden naar de map *%WINROOT%\PolicyDefinitions* .
    - Kopie de. ADML bestanden naar de map *%WINROOT%\PolicyDefinitions\en-us* .

Eenmaal zijn gekopieerd, kunt u Groepsbeleid-Editor om het beleid te wijzigen.




## <a name="next"></a>Volgende

[Verbinding met de bron van een On-Premises Azure Web Apps](../app-service-web/web-sites-hybrid-connection-get-started.md)  
[Verbinding maken met de SQL-Server on-premises vanuit Azure Web Apps](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)   
[Hybride verbindingen-overzicht](integration-hybrid-connection-overview.md)


## <a name="see-also"></a>Zie ook

[REST-API voor het beheren van Services op Microsoft Azure BizTalk](http://msdn.microsoft.com/library/azure/dn232347.aspx)  
[BizTalk-Services: Edities grafiek](biztalk-editions-feature-chart.md)  
[Een BizTalk-Service met Azure klassieke portal maken](biztalk-provision-services.md)  
[BizTalk-Services: Tabbladen voor het Dashboard, de Monitor en de schaal](biztalk-dashboard-monitor-scale-tabs.md)


[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
