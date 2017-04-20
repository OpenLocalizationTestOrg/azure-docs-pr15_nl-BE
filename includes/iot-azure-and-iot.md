
# <a name="azure-and-internet-of-things"></a>Azure en Internet van dingen

Welkom bij Microsoft Azure en het Internet van dingen (IoT). In dit artikel wordt een IoT solution-architectuur die beschrijft de algemene kenmerken van een IoT-oplossing die u kan implementeren met behulp van Azure services geïntroduceerd. IoT oplossingen moeten beveiligen, bidirectionele communicatie tussen apparaten mogelijk nummering in de miljoenen en een oplossing voor back-end. Bijvoorbeeld, een oplossing voor back-end geautomatiseerde, voorspellende analytics gebruiken om inzichten uit de stroom van uw apparaat naar cloud-gebeurtenis.

Azure IoT Hub is een belangrijke bouwsteen wanneer deze IoT oplossingsarchitectuur met Azure services te implementeren. IoT-Suite biedt volledige, end-to-end-implementaties van deze architectuur voor specifieke scenario's voor IoT. Bijvoorbeeld: 

- De oplossing voor *externe controle* kunt u de status van apparaten zoals automaten. 
- De oplossing voor *preventief onderhoud* kunt u anticiperen op behoeften van onderhoud van apparaten zoals pompen in externe reageert stations en niet-geplande uitvaltijd te voorkomen.

## <a name="iot-solution-architecture"></a>Oplossingsarchitectuur IoT

In het volgende diagram ziet u een typische IoT solution-architectuur. Het diagram omvat niet de namen van eventuele specifieke Azure services, maar beschrijft de belangrijkste elementen in een algemene oplossingsarchitectuur voor IoT. In deze architectuur verzamelen IoT apparaten van gegevens die ze naar een cloud-gateway verzenden. De cloud-gateway maakt de gegevens beschikbaar voor verwerking door een andere backend-services waar gegevens voor andere toepassingen van bedrijfs of menselijke exploitanten door middel van een dashboard of een andere presentatie-apparaat is geleverd.

![Oplossingsarchitectuur IoT][img-solution-architecture]

> [AZURE.NOTE] Zie de [Microsoft Azure IoT referentiearchitectuur]voor uitvoerige informatie van IoT-architectuur,[lnk-refarch].

### <a name="device-connectivity"></a>Apparaat verbinding

In deze oplossingsarchitectuur IoT verzenden apparaten telemetrie zoals metingen van de sensor van een station reageert, naar een eindpunt van de wolk voor opslag en verwerking. In een scenario voor preventief onderhoud, de back-end de stroom van de sensorgegevens gebruiken om te bepalen wanneer een specifieke pomp vereist onderhoud. Apparaten kunnen ook ontvangen en reageren op opdrachten van de wolk naar apparaat door het lezen van berichten vanuit het eindpunt van een wolk. Bijvoorbeeld in het geval van preventief onderhoud kan de oplossing voor back-end opdrachten verzenden naar andere pompen in het station reageert om te beginnen met ' omleiding ' stromen net voordat het onderhoud moet worden betaald om te controleren of dat de engineer onderhoud kan aan de slag na aankomst.

Een van de grootste uitdagingen die IoT projecten is een betrouwbare en veilige wijze apparaten aansluiten op de oplossing voor back-end. IoT apparaten hebben verschillende kenmerken in vergelijking met andere clients zoals browsers en mobiele toepassingen. IoT apparaten:

- Zijn vaak embedded-systemen met geen menselijke operator.
- Kan worden geïmplementeerd in externe locaties waar fysieke toegang duur is.
- Mogelijk alleen bereikbaar via de oplossing voor back-end. Er is geen andere manier om te communiceren met het apparaat.
- Mogelijk beperkt vermogen en verwerkingsbronnen.
- Mogelijk verbinding met het netwerk af en toe, langzaam en duur.
- Wellicht moet u eigen, aangepaste en branchespecifieke toepassingsprotocollen gebruiken.
- U kunt maken met behulp van een grote reeks populaire hardware- en software platforms.

Naast de vereisten, moet ook een oplossing IoT leveren schaal, beveiliging en betrouwbaarheid. De resulterende reeks vereisten voor connectiviteit is moeilijk en tijdrovend te implementeren met behulp van traditionele technologieën zoals web- en messaging makelaars. Azure IoT Hub en de IoT apparaat SDK's gemakkelijker implementeren van oplossingen die voldoen aan deze vereisten.

Een apparaat kan communiceren rechtstreeks met een wolk gateway eindpunt, of als het apparaat niet kan een van de communicatieprotocollen die door de wolk-gateway ondersteunt, verbinding kan maken via een tussenliggende gateway. Bijvoorbeeld de [Azure IoT-protocol gateway] [ lnk-protocol-gateway] protocol vertaling kunt uitvoeren als de apparaten kunnen de IoT Hub ondersteunde protocollen niet gebruiken.

### <a name="data-processing-and-analytics"></a>Gegevensverwerking en -analyse

Een oplossing voor back-end IoT is in de cloud, waar de meeste van de verwerking van gegevens plaatsvindt zoals filteren en aggregeren Telemetrie en het bewerkingsplan aan andere diensten. De IoT oplossing weer beëindigen:

- Telemetrie op schaal van uw apparatuur ontvangt en bepaalt hoe worden verwerkt en die gegevens opslaan. 
- Mogelijk kunt u opdrachten naar apparaat verzenden vanuit de cloud.
- Biedt mogelijkheden voor registratie apparaat waarmee u apparaten verstrekken en om te bepalen welke apparaten mogen verbinding maken met uw infrastructuur.
- Kunt u de status van uw apparaten bijhouden en controleren van hun activiteiten.

Het scenario van preventief onderhoud, de oplossing voor back-end historische telemetriegegevens worden opgeslagen. De back-end kunt deze gegevens gebruiken voor het identificeren van patronen die aangeven onderhoud op een specifieke pomp verschuldigd is.

IoT oplossingen kunnen automatische feedback loops opnemen. Bijvoorbeeld kunt een analytics-module in de back-end identificeren van telemetrie die de temperatuur van een bepaald apparaat hoger dan de normale operationele niveaus is. De oplossing kan vervolgens een opdracht sturen naar het apparaat, instructie corrigerende maatregelen nemen.

### <a name="presentation-and-business-connectivity"></a>Presentatie en business connectivity

De presentatie en business connectivity laag kan eindgebruikers communiceren met de IoT-oplossing en de apparaten. Hiermee kunnen gebruikers kunnen bekijken en analyseren van de gegevens verzameld van hun apparaten. Deze weergaven kunnen de vorm van dashboards of BI-rapporten die zowel historische gegevens kunnen worden weergegeven of in de buurt van real-time gegevens krijgen. Bijvoorbeeld een operator in de status van bepaalde reageert station controleren en Zie geen waarschuwingen gegenereerd door het systeem. Deze laag kan ook de integratie van de IoT oplossing voor back-end met bestaande line-of-business-toepassingen te koppelen in de bedrijfsprocessen van de onderneming of workflows. Bijvoorbeeld, kunt de oplossing voor preventief onderhoud integreren met een planningssysteem dat het boeken van een engineer om naar een station reageert wanneer wordt vastgesteld dat de oplossing een pomp vereist onderhoud.

![IoT oplossing dashboard][img-dashboard]

[img-solution-architecture]: ./media/iot-azure-and-iot/iot-reference-architecture.png
[img-dashboard]: ./media/iot-azure-and-iot/iot-suite.png

[lnk-machinelearning]: http://azure.microsoft.com/documentation/services/machine-learning/
[Azure IoT Suite]: http://azure.microsoft.com/solutions/iot
[lnk-protocol-gateway]:  ../articles/iot-hub/iot-hub-protocol-gateway.md
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
