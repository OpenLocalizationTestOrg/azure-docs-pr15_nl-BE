<properties
   pageTitle="Eindpunten in Azure verkeer beheer beheren | Microsoft Azure"
   description="Dit artikel helpt u bij het toevoegen, verwijderen, inschakelen en uitschakelen van de eindpunten van Azure verkeer Manager."
   services="traffic-manager"
   documentationCenter=""
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/17/2016"
   ms.author="sewhee" />

# <a name="add-disable-enable-or-delete-endpoints"></a>Toevoegen, uitschakelen, inschakelen of eindpunten verwijderen

De functie Web Apps in Azure App Service biedt al failover en round robin-verkeer routeren functionaliteit voor websites in een datacenter, ongeacht de modus van de website. Azure verkeer Manager kunt u failover en round robin-verkeer routeren voor websites en cloud services in verschillende datacentra. De eerste stap noodzakelijk is te bepalen dat de functionaliteit is het cloud-service of website eindpunt aan verkeer-beheer toevoegen.

>[AZURE.NOTE] Als de eindpunten met de Azure klassieke portal kunt u externe locaties of verkeer profielen toevoegen. U moet de REST API- [Definitie maken](http://go.microsoft.com/fwlink/p/?LinkId=400772) of Windows PowerShell [Add-AzureTrafficManagerEndpoint](http://go.microsoft.com/fwlink/p/?LinkId=400774).

U kunt ook afzonderlijke eindpunten die deel van een serviceprofiel verkeer uitmaken uitschakelen. Eindpunten zijn zowel cloud services en websites. Het uitschakelen van een eindpunt verlaat als onderdeel van het profiel maar het profiel alsof het eindpunt is niet opgenomen in het. Deze actie is erg handig voor het tijdelijk verwijderen van een eindpunt dat zich in de onderhoudsmodus of opnieuw worden geïmplementeerd. Zodra het eindpunt opnieuw actief is, kan worden ingeschakeld.

>[AZURE.NOTE] Het uitschakelen van een eindpunt heeft niets te maken met de status van de implementatie in Azure. Een gezonde eindpunt blijven omhoog en ontvangen verkeer terwijl in beheer van netwerkverkeer uitgeschakeld. Bovendien een eindpunt in één profiel uitschakelen heeft geen invloed op de status in een ander profiel.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Een cloud-service of website eindpunt toevoegen


1. In het deelvenster Beheer van verkeer in de klassieke Azure portal naar het profiel dat verkeer Manager met de endpoint-instellingen die u wilt wijzigen en klik vervolgens op de pijl rechts van de naam van het profiel. Hiermee opent u de pagina-instellingen voor het profiel.
2. Aan de bovenkant van de pagina, klikt u op de **eindpunten** om de eindpunten die al deel uitmaken van de configuratie weer te geven.
3. Klik op **toevoegen** om de toegang tot de pagina **Service-eindpunten toevoegen** onderaan de pagina. De pagina bevat standaard de cloud-services onder **Service-Endpoints**.
4. Voor cloud services, selecteert u de cloud-services in de lijst om ze als eindpunten voor dit profiel. De naam van de cloud-service uit te schakelen, wordt deze verwijderd uit de lijst van eindpunten.
5. Klik op de vervolgkeuzelijst **Type Service** voor websites, en selecteer vervolgens **Web app**.
6. Selecteer de websites in de lijst om deze als eindpunten voor dit profiel toevoegen. Naam van de website uit te schakelen, wordt deze verwijderd uit de lijst van eindpunten. Opmerking u kunt slechts één website per Azure datacenter (regio) selecteren. Als u een website in een datacenter dat fungeert als host voor meerdere websites wanneer u de eerste website selecteert, worden in het datacenter dezelfde anderen niet beschikbaar voor selectie. Ook ziet u dat alleen standaard websites worden weergegeven.
7. Nadat u de eindpunten voor dit profiel hebt geselecteerd, klikt u op het vinkje in de rechterbenedenhoek de wijzigingen wilt opslaan.

>[AZURE.NOTE] Als u de *Failover* -verkeer routeren methode na het toevoegen of verwijderen van een eindpunt gebruikt, moet u de failover-lijst met prioriteiten op de pagina configuratie zodat de failover-order die u wilt gebruiken voor de configuratie aanpassen. Zie voor meer informatie, [Configureer failover-verkeer routeren](traffic-manager-configure-failover-routing-method.md).

## <a name="to-disable-an-endpoint"></a>Een eindpunt uitschakelen

1. In het deelvenster Beheer van verkeer in de klassieke Azure portal naar het profiel dat verkeer Manager met de endpoint-instellingen die u wilt wijzigen en klik vervolgens op de pijl rechts van de naam van het profiel. Hiermee opent u de pagina-instellingen voor het profiel.
2. Aan de bovenkant van de pagina, klikt u op de **eindpunten** om de eindpunten die zijn opgenomen in de configuratie weer te geven.
3. Klik op het eindpunt dat u wilt uitschakelen en klik vervolgens op **uitschakelen** onder aan de pagina.
4. Verkeer wordt gestopt die doorloopt tot aan het eindpunt op basis van de DNS-Time-to-Live (TTL) geconfigureerd voor de domeinnaam van het beheer van verkeer. Kunt u de TTL-waarde van de pagina configuratie van het profiel van het beheer van verkeer.

## <a name="to-enable-an-endpoint"></a>Een eindpunt inschakelen

1. In het deelvenster Beheer van verkeer in de klassieke Azure portal naar het profiel dat verkeer Manager met de endpoint-instellingen die u wilt wijzigen en klik vervolgens op de pijl rechts van de naam van het profiel. Hiermee opent u de pagina-instellingen voor het profiel.
2. Aan de bovenkant van de pagina, klikt u op de **eindpunten** om de eindpunten die zijn opgenomen in de configuratie weer te geven.
3. Klik op het eindpunt dat u wilt inschakelen en klik vervolgens op **inschakelen** onder aan de pagina.
4. Verkeer wordt gestart die aan de service opnieuw als gedicteerde door het profiel.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>Een cloud-service of website eindpunt verwijderen


1. In het deelvenster Beheer van verkeer in de klassieke Azure portal naar het profiel dat verkeer Manager met de endpoint-instellingen die u wilt wijzigen en klik vervolgens op de pijl rechts van de naam van het profiel. Hiermee opent u de pagina-instellingen voor het profiel.
2. Aan de bovenkant van de pagina, klikt u op de **eindpunten** om de eindpunten die al deel uitmaken van de configuratie weer te geven.
3. Klik op de naam van het eindpunt dat u wilt verwijderen uit het profiel op de pagina Tunneleindpunten.
4. Onderaan op de pagina, klikt u op **verwijderen**.

>[AZURE.NOTE] U kunt externe locaties of verkeer profielen niet verwijderen als de klassieke Azure portal met eindpunten. U moet Windows PowerShell gebruiken. Zie [Software-AzureTrafficManagerEndpoint](https://msdn.microsoft.com/library/dn690251.aspx)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- [Failover-methode configureren](traffic-manager-configure-failover-routing-method.md)
- [Methode round robin configureren](traffic-manager-configure-round-robin-routing-method.md)
- [Prestaties routeringsmethode configureren](traffic-manager-configure-performance-routing-method.md)
- [Probleemoplossing verkeer Manager verslechterd staat](traffic-manager-troubleshooting-degraded.md)
- [Bewerkingen op verkeer Manager (REST API Reference)](http://go.microsoft.com/fwlink/p/?LinkID=313584)
