<properties 
   pageTitle="Problemen oplossen met een apparaat met gedistribueerde StorSimple | Microsoft Azure"
   description="Beschrijving van het opsporen en oplossen van fouten die optreden op een StorSimple apparaat dat momenteel wordt geïmplementeerd en operationeel."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/16/2016"
   ms.author="v-sharos" />

# <a name="troubleshoot-an-operational-storsimple-device"></a>Een operationele StorSimple apparaat oplossen

## <a name="overview"></a>Overzicht

In dit artikel vindt u handig zijn voor probleemoplossing richtlijnen voor het oplossen van problemen die optreden kunnen na het apparaat StorSimple geïmplementeerd en operationeel is. Hierin worden problemen besproken, mogelijke oorzaken en aanbevolen stappen waarmee u problemen kunt oplossen die zich voordoen kunnen wanneer u Microsoft Azure StorSimple uitvoert. Deze informatie is van toepassing op zowel het fysieke apparaat voor StorSimple in ruimten en de StorSimple virtueel apparaat.

Aan het einde van dit artikel vindt u een lijst met foutcodes die tijdens de Microsoft Azure StorSimple optreden kunnen en de stappen kunt u ondernemen om de fouten op te lossen. 

## <a name="setup-wizard-process-for-operational-devices"></a>Setup-wizard proces voor actieve apparaten

U gebruikt de wizard setup ([Invoke HcsSetupWizard][1]) de configuratie controleren en corrigerende maatregelen te nemen indien nodig.

Wanneer u de wizard setup op een eerder geconfigureerd en operationeel apparaat uitvoert, verschilt de processtroom. U kunt alleen de volgende items wijzigen:

- IP-adres, subnetmasker en gateway
- Primaire DNS-server
- Primaire NTP-server
- Optionele web proxy-configuratie

De installatiewizard voert geen bewerkingen die betrekking hebben op de registratie van wachtwoord-collectie en het apparaat.

## <a name="errors-that-occur-during-subsequent-runs-of-the-setup-wizard"></a>Fouten die tijdens latere uitvoeringen van de wizard setup optreden

De volgende tabel beschrijft de fouten die kunnen optreden tijdens het uitvoeren van de wizard setup een operationele apparaat, de mogelijke oorzaken van de fouten en aanbevolen acties deze op te lossen. 

| Nr. | Foutbericht of voorwaarde | Mogelijke oorzaken | Aanbevolen actie |
|:--- |:-------------------------- |:--------------- |:------------------ |
|  1  | 350032 fout: Dit apparaat is al gedeactiveerd. | Deze fout wordt weergegeven als u de wizard setup uitvoeren op een apparaat dat wordt gedeactiveerd. | [Contact met Microsoft-ondersteuning](storsimple-contact-microsoft-support.md) voor de volgende stappen. Een gedeactiveerde apparaat kan niet worden geplaatst in de service. Een factory reset kan nodig zijn voordat het apparaat opnieuw kan worden geactiveerd. |
|  2  | Aanroepen van HcsSetupWizard: ERROR_INVALID_FUNCTION (uitzondering op HRESULT: 0x80070001) | Het bijwerken van DNS-server is mislukt. DNS-instellingen zijn algemene instellingen en via de ingeschakelde netwerkinterfaces worden toegepast. | De interface inschakelen en de DNS-instellingen opnieuw toepassen. Omdat deze instellingen gelden, kan dit naar andere interfaces ingeschakeld in het netwerk verstoren. |
|  3  | Het apparaat wordt weergegeven in de serviceportal StorSimple Manager online, maar wanneer u de minimale installatie voltooien en de configuratie op te slaan, mislukt de bewerking. | Tijdens de eerste installatie van is de webproxy niet geconfigureerd, zelfs als er een werkelijke proxyserver aanwezig is. | Gebruik de [cmdlet Test HcsmConnection] [ 2] de fout te vinden. [Contact Microsoft Support](storsimple-contact-microsoft-support.md) als u niet het probleem te verhelpen. |
|  4  | Aanroepen van HcsSetupWizard: Waarde valt niet binnen het verwachte bereik. | Een onjuist subnetmasker treedt deze fout op. Mogelijke oorzaken zijn: <ul><li> Het subnetmasker is ontbreekt of leeg.</li><li>De notatie van IPv6-voorvoegsel is onjuist.</li><li>De interface is cloud is ingeschakeld, maar de gateway is ontbreekt of is onjuist.</li></ul>Let erop dat gegevens 0 automatisch wolk is ingeschakeld als met de wizard setup geconfigureerd. | Subnetmasker 0.0.0.0 of 256.256.256.256 gebruiken om te bepalen wat het probleem, en zoek vervolgens naar de uitvoer. Geef de juiste waarden voor het subnetmasker, de gateway en de IPv6-voorvoegsel, indien nodig. |
 
## <a name="error-codes"></a>Foutcodes

Fouten worden in numerieke volgorde weergegeven.

|Foutnummer|Fouttekst of beschrijving|Actie van de gebruiker aanbevolen|
|:---|:---|:---|
|10502|Er is een fout opgetreden tijdens de toegang tot uw account opslag.|Wacht een paar minuten en probeer het opnieuw. Als de fout zich blijft voordoen, contacteer de Support van Microsoft voor volgende stappen.|
|40017|De back-upbewerking is mislukt omdat een volume dat is opgegeven in het back-beleid niet op het apparaat gevonden is.|De back-up opnieuw bewerking, als de fout zich blijft voordoen, neem contact op met Microsoft Support. voor volgende stappen.|
|40018|De back-upbewerking is mislukt omdat geen van de volumes die zijn opgegeven in het back-beleid op het apparaat is gevonden. |De back-up opnieuw bewerking, als de fout zich blijft voordoen, neem contact op met Microsoft Support. voor volgende stappen.|
|390061|Het systeem is bezet of niet beschikbaar.|Wacht een paar minuten en probeer het opnieuw. Als de fout zich blijft voordoen, contacteer de Support van Microsoft voor volgende stappen.|
|390143|Er is een fout opgetreden met foutcode 390143. (Onbekende fout).|Als de fout zich blijft voordoen, neem u contact op met Microsoft Support voor volgende stappen.|

## <a name="next-steps"></a>Volgende stappen

Als u niet aan het oplossen van het probleem, [Neem contact op met Microsoft Support](storsimple-contact-microsoft-support.md) voor hulp. 


[1]: https://technet.microsoft.com/en-us/%5Clibrary/Dn688135(v=WPS.630).aspx
[2]: https://technet.microsoft.com/en-us/%5Clibrary/Dn715782(v=WPS.630).aspx
