<properties 
    pageTitle="Meer informatie over een AS2-overeenkomst voor de Enterprise Integration Pack maken" 
    description="Informatie voor het maken van een AS2-overeenkomst voor de Enterprise Integration Pack | Microsoft Azure App Service" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-as2"></a>Integratie in de onderneming met AS2

## <a name="create-an-as2-agreement"></a>Maak een AS2-overeenkomst
Om de ondernemingsfuncties in logica apps gebruikt, maakt u overeenkomsten. 

### <a name="heres-what-you-need-before-you-get-started"></a>Hier is wat u nodig hebt voordat u begint
- Een [account voor integratie](./app-service-logic-enterprise-integration-accounts.md) in uw abonnement Azure  
- Ten minste twee [partners](./app-service-logic-enterprise-integration-partners.md) al gedefinieerd in uw account integratie  

>[AZURE.NOTE]Bij het maken van een overeenkomst, de inhoud van het bestand van de overeenkomst, moet overeenkomen met het Overeenkomstsoort.    


Nadat u [gemaakt van een account integratie](./app-service-logic-enterprise-integration-accounts.md) en [partners toegevoegd hebt](./app-service-logic-enterprise-integration-partners.md), kunt u een overeenkomst maken door de volgende stappen:  

### <a name="from-the-azure-portal-home-page"></a>Vanaf de introductiepagina van Azure portal

Nadat u zich aanmelden bij de [Azure portal](http://portal.azure.com "Azure portal"):  
1. Selecteer **Bladeren** in het menu aan de linkerkant.  

>[AZURE.TIP]Als u de koppeling **Bladeren** niet ziet, moet u mogelijk eerst het menu uitvouwen. Hiervoor selecteert u de koppeling **menu weergeven** op links boven in het menu samengevouwen.  

![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. *Integratie* typt in het vak filter zoeken en selecteer vervolgens **Integratie Accounts** in de lijst met resultaten.       
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. In de blade **Integratie Accounts** die wordt geopend, selecteert u de integratie rekening waarin u de overeenkomst maken. Als er geen integratie van alle rekeningen lijsten, [maakt u een eerste](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4.  Selecteer de tegel **overeenkomsten** . Als u de overeenkomsten naast elkaar weergeven niet ziet, moet u deze eerst toevoegen.   
![](./media/app-service-logic-enterprise-integration-agreements/agreement-1.png)   
5. Selecteer de knop **toevoegen** in het blad overeenkomsten die wordt geopend.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-2.png)  
6. Voer een **naam** voor uw overeenkomst en selecteer vervolgens de **Host Partner**, **Identiteit van de Host**, **Partner van de Gast**, **Gast identiteit**, in de overeenkomsten blade dat wordt geopend.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-3.png)  

Hier zijn een paar details die u nuttig kan zijn bij het configureren van de instellingen voor uw overeenkomst: 
  
|Eigenschap|Beschrijving|
|----|----|
|Host-Partner|Een overeenkomst moet een host en Gast-partner. De host-partner vertegenwoordigt de organisatie die de overeenkomst wordt geconfigureerd.|
|Identiteit van de host|Een id voor de host-partner. |
|Gast-Partner|Een overeenkomst moet een host en Gast-partner. De Gast partner vertegenwoordigt de organisatie die wordt zakendoen met de host-partner.|
|Identiteit van de Gast|Een id voor de Gast-partner.|
|Instellingen ontvangen|Deze eigenschappen toepassen op alle berichten die worden ontvangen door een overeenkomst|
|Instellingen voor verzenden|Deze eigenschappen toepassen op alle berichten die worden verzonden door een overeenkomst|  
We gaan verder:  
7. Selecteer **Instellingen voor ontvangen** om te configureren hoe berichten ontvangen via deze overeenkomst moeten worden verwerkt.  
 
 - Desgewenst kunt u de eigenschappen in het inkomende bericht negeren. Hiertoe selecteert u het selectievakje **overschrijven berichteigenschappen** .
  - Schakel het selectievakje **bericht moet worden ondertekend** als u dat alle inkomende berichten wilt moeten worden ondertekend. Als u deze optie selecteert, moet u ook het **certificaat** dat wordt gebruikt voor het verifiëren van de handtekening in de berichten selecteren.
  - Desgewenst kunt u berichten ook versleutelen vereisen. Schakel hiervoor het selectievakje **berichten moet worden gecodeerd** . Vervolgens moet u het **certificaat** dat wordt gebruikt voor het decoderen van binnenkomende berichten selecteren.
  - U kunt ook berichten worden gecomprimeerd vereisen. Schakel hiertoe het selectievakje **bericht moet worden gecomprimeerd** .  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-4.png)  

Zie de tabel hieronder als u meer informatie over wat de ontvangen instellingen inschakelen.  

|Eigenschap|Beschrijving|
|----|----|
|Eigenschappen van berichten opheffen|Selecteer deze optie om aan te geven dat de eigenschappen van ontvangen berichten kunnen worden overschreven |
|Bericht moet worden ondertekend.|Deze berichten digitaal wilt ondertekenen vereisen|
|Berichten moet worden gecodeerd.|Schakel dit vereisen berichten moeten worden gecodeerd. Niet-gecodeerde berichten geweigerd.|
|Bericht moet worden gecomprimeerd.|Met deze berichten wordt gecomprimeerd moet inschakelen. Niet-gecomprimeerd berichten geweigerd.|
|MDN-tekst|Dit is een standaard MDN worden verzonden naar de afzender van het bericht|
|MDN verzenden|Schakel dit toestaan MDNs moet worden verzonden.|
|Ondertekende MDN verzenden|Schakel dit vereisen MDNs moeten worden ondertekend.|
|MIC-algoritme||
|Asynchrone MDN verzenden|Deze berichten worden asynchroon verzonden moeten inschakelen.|
|URL|Dit is de URL waarnaar berichten worden verzonden.|
Nu gaan we verder:  
8. Selecteer **Instellingen voor verzenden** om te configureren hoe berichten die worden verzonden via deze overeenkomst moeten worden verwerkt.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-5.png)  

Zie de tabel hieronder als u meer informatie over wat de verzenden instellingen inschakelen.  

|Eigenschap|Beschrijving|
|----|----|
|Ondertekenen van berichten inschakelen|Schakel dit selectievakje in om alle berichten die van de overeenkomst te ondertekenen.|
|MIC-algoritme|Het algoritme wordt gebruikt in het ondertekenen van berichten selecteren|
|Certificaat|Selecteer het certificaat in het ondertekenen van berichten gebruiken|
|Codering inschakelen|Schakel dit selectievakje in voor het coderen van alle berichten die van deze overeenkomst.|
|Coderingsalgoritme|Het algoritme voor codering selecteren|
|Uitgevouwen HTTP-headers|Schakel dit selectievakje in om de HTTP-header content-type uitgevouwen tot één regel.|
|Aanvraag MDN|Schakel dit selectievakje in om aan te vragen een MDN voor alle berichten die van deze overeenkomst|
|Aanvraag MDN ondertekend|Inschakelen om aan te vragen dat alle MDNs naar deze overeenkomst verzonden worden ondertekend.|
|MDN asynchrone aanvragen|Voor het aanvragen van asynchrone MDN worden verzonden naar deze overeenkomst inschakelen|
|URL|De URL waarnaar MDNs worden verzonden|
|NRR inschakelen|Schakel dit selectievakje in om niet-afwijzing van ontvangst|
We zijn bijna klaar!  
9. Selecteer de tegel **overeenkomsten** op de integratie rekening blade en ziet u de nieuwe overeenkomst vermeld.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-6.png)

