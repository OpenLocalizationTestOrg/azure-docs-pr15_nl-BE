<properties
 pageTitle="IoT Hub HA en DR | Microsoft Azure"
 description="Beschrijving van functies die helpen bij het maximaal beschikbare IoT oplossingen bouwen met disaster recovery mogelijkheden."
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="02/03/2016"
 ms.author="elioda"/>

# <a name="iot-hub-high-availability-and-disaster-recovery"></a>IoT Hub hoge beschikbaarheid en disaster recovery

Als een Azure service biedt IoT Hub ontslagen op het regioniveau Azure zonder extra werk nodig is voor de oplossing met hoge beschikbaarheid (HA). Azure biedt bovendien een aantal functies die u helpen om oplossingen te bouwen met mogelijkheden voor disaster recovery (Debet) of beschikbaarheid van cross-regio als nodig is. Ontwerpen en voorbereiden van uw oplossingen om te profiteren van deze DR functies als u de globale wilt bieden, cross-regio voor maximale beschikbaarheid apparaten of gebruikers. [Azure Business Continuity technische richtsnoeren](../resiliency/resiliency-technical-guidance.md) beschreven de ingebouwde functies in Azure voor bedrijfscontinuïteit en DR. Het papier [noodherstel en hoge beschikbaarheid voor Azure toepassingen][] bevat architectuur op strategieën voor Azure toepassingen HA en DR.

## <a name="azure-iot-hub-dr"></a>Azure IoT Hub DR
IoT Hub implementeert naast intra-regio HA, failover-mechanismen voor noodherstel waarvoor geen tussenkomst van de gebruiker. IoT Hub DR zelf wordt gestart en heeft een herstel tijd doelstelling (RTO) 2-26 uur en de volgende herstel punt doelstellingen (productoutput).

| Functionaliteit | VRIJGEGEVEN PRODUCTIEORDER |
| ------------- | --- |
| Beschikbaarheid van de service voor register- en bewerkingen | Mogelijk verlies van CName |
| Id-gegevens in het register van apparaat-id | 0-5 minuten gegevensverlies |
| Apparaat-wolk-berichten | Alle ongelezen berichten gaan verloren |
| Bewerkingen berichten controleren | Alle ongelezen berichten gaan verloren |
| Cloud-to-device-berichten | 0-5 minuten gegevensverlies |
| Cloud-to-device-feedback-wachtrij | Alle ongelezen berichten gaan verloren |

## <a name="regional-failover-with-iot-hub"></a>Regionale failover met IoT Hub

Een volledige behandeling van implementatietopologieën voor in IoT oplossingen is buiten het bestek van dit artikel, maar voor hoge beschikbaarheid en noodherstel we het implementatiemodel *regionale failover* worden beschouwd.

De oplossing voor back-end hoofdzakelijk op één locatie van het datacenter wordt uitgevoerd in een regionale failover-model, maar een extra IoT hub en back-end worden ingezet in een ander datacenter locatie voor failover-doeleinden, voor het geval de IoT hub in het primaire datacenter een stroomstoring ondergaat of de verbinding met het netwerk van het apparaat naar de primaire datacenter op wordt onderbroken. Apparaten gebruiken een secundaire service-eindpunt wanneer de primaire gateway kan niet worden bereikt. Met een cross-regio uitvalbeveiligingsmogelijkheden kan de beschikbaarheid van de oplossing na de beschikbaarheid van een bepaalde regio worden verbeterd.

Op een hoog niveau, een model voor regionale failover implementeren met IoT Hub, moet u de volgende.

* **Een secundaire hub IoT en routering logica apparaat**: in het geval van een verstoring van de service in uw regio voor primaire apparaten moeten starten, verbinding maken met uw secundaire regio. Gezien de staat bewust aard van de meeste diensten die betrokken zijn, is het gebruikelijk dat beheerders voor het starten van het proces tussen regio failover-oplossing. De beste manier om te communiceren het nieuwe endpoint apparaten, zonder daarbij de controle van het proces, is ze een *concierge* service voor de huidige actieve eindpunt regelmatig te controleren. De concierge-service is een eenvoudige webtoepassing die wordt gerepliceerd en bewaard kunnen worden bereikt met behulp van DNS-omleiding technieken (bij voorbeeld met [Azure verkeer Manager][]).
* **Identiteit register replicatie** - om te worden gebruikt, de secundaire IoT hub alle apparaat-id's die verbinding met de oplossing maken kunnen moet bevatten. De oplossing moet back-ups geo gerepliceerd van de apparaat-id's te behouden en te uploaden naar de secundaire IoT hub voordat u overschakelt van het actieve eindpunt voor de apparaten. De exportfunctie van apparaat identiteit van IoT Hub is erg handig in deze context. Zie [IoT Hub Developer Guide - identiteit register][]voor meer informatie.
* **Logica samen** - als de primaire regio weer beschikbaar is, moet de status en de gegevens die zijn gemaakt in de secundaire site gemigreerd ongedaan maken op de primaire regio. Dit is voornamelijk betrekking heeft op de apparaat-id's en toepassing meta-gegevens, die moeten worden samengevoegd met de primaire IoT-hub en andere toepassingsspecifieke winkels in de regio voor primaire. U kunt deze stap vereenvoudigen, meestal verdient u bewerkingen idempotency is ingeschakeld. Deze neveneffecten niet alleen van eventuele consistente distributie van gebeurtenissen, maar ook van duplicaten of aflevering uit van de volgorde van gebeurtenissen wordt geminimaliseerd. Bovendien moet de toepassingslogica zodanig zijn dat tolereren mogelijke inconsistenties of 'iets' niet op datum staat. Dit is vanwege de extra tijd die nodig is om het systeem te 'retoucheren' op basis van herstel punt doelstellingen (vrijgegeven Productieorder).

## <a name="next-steps"></a>Volgende stappen

Klik op deze koppelingen voor meer informatie over Azure IoT Hub:

- [Aan de slag met IoT Hubs (zelfstudie)][lnk-get-started]
- [Wat is Azure IoT Hub?][]

[Noodherstel en hoge beschikbaarheid voor Azure toepassingen]: ../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md
[Azure Business Continuity Technical Guidance]: https://azure.microsoft.com/documentation/articles/resiliency-technical-guidance/
[Azure verkeer Manager]: https://azure.microsoft.com/documentation/services/traffic-manager/
[IoT Hub Developer Guide - identiteit register]: iot-hub-devguide-identity-registry.md

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[Wat is Azure IoT Hub?]: iot-hub-what-is-iot-hub.md
