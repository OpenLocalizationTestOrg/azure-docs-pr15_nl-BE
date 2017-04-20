<properties 
    pageTitle="Naam uitgever en de sleutel van de uitgevende instelling in BizTalk-Services | Microsoft Azure" 
    description="Informatie over het ophalen van de uitgever naam en sleutel van de uitgevende instelling voor Service Bus of Access Control (ACS) in BizTalk-Services. MAB, WABS" 
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
    ms.date="08/15/2016" 
    ms.author="mandia"/>




# <a name="biztalk-services-issuer-name-and-issuer-key"></a>BizTalk-Services: Naam van de uitgevende instelling en de sleutel van de uitgevende instelling

Azure BizTalk-Services wordt gebruikt voor de naam van de uitgevende instelling Bus en sleutel van de verlener, Access Control uitgever en de naam sleutel van de uitgevende instelling. Met name:

Taak | Welke naam uitgever en de sleutel van de uitgevende instelling
--- | ---
Implementatie van uw toepassing vanuit Visual Studio | Toegang tot de uitgevende instelling besturingselementnaam en de sleutel van de uitgevende instelling
De Portal Azure BizTalk-Services configureren | Toegang tot de uitgevende instelling besturingselementnaam en de sleutel van de uitgevende instelling
Relais LOB maken met de BizTalk-Adapter Services in Visual Studio | Service Bus uitgever naam en sleutel van de uitgevende instelling

In dit onderwerp vindt u de stappen voor het ophalen van de naam van de uitgevende instelling en de sleutel van de uitgevende instelling. 

## <a name="access-control-issuer-name-and-issuer-key"></a>Toegang tot de uitgevende instelling besturingselementnaam en de sleutel van de uitgevende instelling
De uitgevende instelling naam van Access Control en de sleutel van de uitgevende instelling worden gebruikt door het volgende:

- Uw toepassing Azure BizTalk-Service is gemaakt in Visual Studio: als u wilt implementeren met succes uw BizTalk-Service application in Visual Studio op Azure, u de sleutel van de uitgevende instelling en de uitgevende instelling naam van Access Control. 
- De Azure BizTalk-serviceportal: Wanneer u een BizTalk-Service maken en openen van de BizTalk-Services Portal, de uitgevende instelling naam van Access Control en de sleutel van de uitgevende instelling worden automatisch geregistreerd voor uw implementaties met dezelfde waarden voor toegangsbeheer.

### <a name="to-copy-and-paste-the-access-control-issuer-name-and-issuer-key"></a>Kopiëren en plakken van de uitgevende instelling naam van Access Control en de sleutel van de uitgevende instelling

1. Log in om de [Azure klassieke portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Selecteer in het linkernavigatievenster **BizTalk-Services**.
3. Selecteer uw BizTalk-Service. 
4. Selecteer **Verbindingsgegevens** in de taakbalk. De Namespace van de Control Access standaard uitgevende instelling (naam uitgever) en standaard sleutel (uitgever) worden weergegeven en kan worden gekopieerd en geplakt.  

Samenvatting:  
Naam van de uitgevende instelling standaard uitgever =  
Sleutel van de uitgevende instelling standaardsleutel =


Ook kunt u **Open ACS Management Portal** om de waarden van het toegangsbeheer:

1. Log in om de [Azure klassieke portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Selecteer in het linkernavigatievenster **BizTalk-Services**.
3. Selecteer uw BizTalk-Service.
4. Klik op de knop gegevens en selecteert u **Openen ACS Management Portal**.
5. Selecteer in de Portal onder **Service-instellingen**, **Service-identiteiten**. Uw Service-identiteit, de waarde van de uitgevende instelling naam van Access-besturingselement worden weergegeven. Selecteer de koppeling-id voor een overzicht van het wachtwoord, de waarde van de sleutel van de uitgevende instelling wordt. De waarden kunnen worden gekopieerd.<br/><br/>
Bijvoorbeeld in **Service-identiteiten**ziet u 'eigenaar'. "Eigenaar" is de naam van uw Access Control-uitgever. Wanneer u klikt op de koppeling 'eigenaar', ziet u het **wachtwoord**. Wanneer u klikt op de koppeling "Wachtwoord", ziet u de waarde. Deze waarde wachtwoord is uw sleutel Access Control verlener.  

Samenvatting:  
Uitgevende instelling naam = naam van Service-identiteit  
Uitgevende instelling Key = wachtwoord

U kunt ook **Active Directory** voor het ophalen van de toegangsbeheer-waarden selecteren in het linkernavigatievenster. 

> [AZURE.IMPORTANT]Wanneer een besturingselement toegang Namespace wordt gemaakt wordt met behulp van **Active Directory**, de identiteit van een Service **niet** automatisch gemaakt. Wanneer u een BizTalk-Service, een Access Control-Namespace inrichten-id met de naam 'eigenaar' (naam van de uitgever) wachtwoord (uitgever sleutel), en symmetrische sleutel worden automatisch gemaakt.<br /> 
[Hoe: ACS Management-Service gebruiken om Service identiteiten configureren](http://go.microsoft.com/fwlink/p/?LinkID=303942) vindt u meer informatie over Access Control-Service identiteiten.


## <a name="service-bus-issuer-name-and-issuer-key"></a>Service Bus uitgever naam en sleutel van de uitgevende instelling
Service Bus uitgever naam en sleutel van de uitgevende instelling worden gebruikt door BizTalk-Adapter Services. In de BizTalk-Services-project in Visual Studio kunt u de BizTalk-Adapter Services verbinding maken met een systeem van lokalen Line of Business (LOB). Als u verbinding wilt maken de Relay LOB en voert u de details van uw LOB-systeem. Wanneer u dit doet, voert u ook de uitgevende instelling naam van Service Bus en de sleutel van de uitgevende instelling.

### <a name="to-retrieve-the-service-bus-issuer-name-and-issuer-key"></a>Voor het ophalen van de uitgevende instelling naam van Service Bus en de sleutel van de uitgevende instelling

1. Log in om de [Azure klassieke portal](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. Selecteer in het linkernavigatievenster **Service Bus**.
3. Selecteer de naamruimte. Selecteer **Verbindingsgegevens**in de taakbalk. Hier worden de **Uitgevende instelling standaard** (naam uitgever) en **Standaard Key** (sleutel van de uitgevende instelling). De waarden kunnen worden gekopieerd.  

Samenvatting:  
Naam van de uitgevende instelling standaard uitgever =  
Sleutel van de uitgevende instelling standaardsleutel =

## <a name="next"></a>Volgende
Extra onderwerpen over Azure BizTalk-Services:

-  [De Azure BizTalk-Services SDK installeren](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
-  [Zelfstudie: Azure BizTalk-Services](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
-  [Hoe kan ik starten via de Azure BizTalk-Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
-  [Azure BizTalk-Services](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>


## <a name="see-also"></a>Zie ook
-  [Procedure: ACS Management-Service gebruiken om Service identiteiten configureren](http://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
- [BizTalk-Services: Ontwikkelaar, Basic, Standard en Premium-edities grafiek](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk-Services: Met behulp van Azure klassieke portal inrichten](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk-Services: Status grafiek inrichten](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [BizTalk-Services: Tabbladen voor het Dashboard, de Monitor en de schaal](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk-Services: Back-up en terugzetten](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk-Services: beperken](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
 
