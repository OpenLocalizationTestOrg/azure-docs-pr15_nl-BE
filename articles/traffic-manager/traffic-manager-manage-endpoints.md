<properties
    pageTitle="Eindpunten in Azure verkeer beheer beheren | Microsoft Azure"
    description="Dit artikel helpt u bij het toevoegen, verwijderen, inschakelen en uitschakelen van de eindpunten van Azure verkeer Manager."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="add-disable-enable-or-delete-endpoints"></a>Toevoegen, uitschakelen, inschakelen of eindpunten verwijderen

De functie Web Apps in Azure App Service biedt al failover en round robin-verkeer routeren functionaliteit voor websites in een datacenter, ongeacht de modus van de website. Azure verkeer Manager kunt u failover en round robin-verkeer routeren voor websites en cloud services in verschillende datacentra. De eerste stap noodzakelijk is te bepalen dat de functionaliteit is het cloud-service of website eindpunt aan verkeer-beheer toevoegen.

>[AZURE.NOTE]  In dit artikel wordt uitgelegd hoe u met de klassieke portal. De klassieke Azure portal biedt alleen ondersteuning voor het maken en toewijzen van cloud-services en Web apps als eindpunten. De nieuwe [portal Azure](https://portal.azure.com) is de beste interface.

U kunt ook afzonderlijke eindpunten die deel van een serviceprofiel verkeer uitmaken uitschakelen. Het uitschakelen van een eindpunt verlaat als onderdeel van het profiel maar het profiel alsof het eindpunt is niet opgenomen in het. Deze actie is nuttig voor het tijdelijk verwijderen een eindpunt dat zich in de onderhoudsmodus of opnieuw worden geïmplementeerd. Zodra het eindpunt opnieuw actief is, kan worden ingeschakeld.

>[AZURE.NOTE] Het uitschakelen van een eindpunt heeft niets te maken met de status van de implementatie in Azure. Een gezonde eindpunt blijft omhoog en ontvangen verkeer terwijl in beheer van netwerkverkeer uitgeschakeld. Bovendien een eindpunt in één profiel uitschakelen heeft geen invloed op de status in een ander profiel.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Een cloud-service of website eindpunt toevoegen

1. Zoek het profiel verkeer Manager met de endpoint-instellingen die u wilt wijzigen in het deelvenster Beheer van verkeer in de klassieke Azure portal. U opent de pagina-instellingen, klik op de pijl rechts van de naam van het profiel.
2. Aan de bovenkant van de pagina, klikt u op de **eindpunten** om de eindpunten die al deel uitmaken van de configuratie weer te geven.
3. Klik op **toevoegen** om de toegang tot de pagina **Service-eindpunten toevoegen** onderaan de pagina. De pagina bevat standaard de cloud-services onder **Service-Endpoints**.
4. Voor cloud services, selecteert u de cloud-services in de lijst toevoegen als de eindpunten voor dit profiel. De naam van de cloud-service uit te schakelen, wordt deze verwijderd uit de lijst van eindpunten.
5. Klik op de vervolgkeuzelijst **Type Service** voor websites, en selecteer vervolgens **Web app**.
6. Selecteer de websites in de lijst om deze als eindpunten voor dit profiel toevoegen. Naam van de website uit te schakelen, wordt deze verwijderd uit de lijst van eindpunten. U kunt slechts één website per Azure datacenter (regio). Wanneer u de eerste website selecteert, worden de andere websites in het datacenter dezelfde niet beschikbaar voor selectie. Ook ziet u dat alleen standaard websites worden weergegeven.
7. Nadat u de eindpunten voor dit profiel hebt geselecteerd, klikt u op het vinkje in de rechterbenedenhoek de wijzigingen wilt opslaan.

>[AZURE.NOTE] Na het toevoegen of verwijderen van een eindpunt van een profiel met de *Failover* -verkeer routeren methode de prioriteitenlijst failover kan niet worden besteld ze zoals u dat wilt. U kunt de volgorde van de lijst van de failover-prioriteit op de pagina configuratie aanpassen. Zie voor meer informatie, [Configureer failover-verkeer routeren](traffic-manager-configure-failover-routing-method.md).

## <a name="to-disable-an-endpoint"></a>Een eindpunt uitschakelen

1. Zoek het profiel verkeer Manager met de endpoint-instellingen die u wilt wijzigen in het deelvenster Beheer van verkeer in de klassieke Azure portal. U opent de pagina-instellingen, klik op de pijl rechts van de naam van het profiel.
2. Aan de bovenkant van de pagina, klikt u op de **eindpunten** om de eindpunten die zijn opgenomen in de configuratie weer te geven.
3. Klik op het eindpunt dat u wilt uitschakelen en klik vervolgens op **uitschakelen** onder aan de pagina.
4. Clients blijven om verkeer te verzenden naar het eindpunt voor de duur van de Time-to-Live (TTL). Kunt u de TTL-waarde op de pagina configuratie van het profiel van het beheer van verkeer.

## <a name="to-enable-an-endpoint"></a>Een eindpunt inschakelen

1. Zoek het profiel verkeer Manager met de endpoint-instellingen die u wilt wijzigen in het deelvenster Beheer van verkeer in de klassieke Azure portal. U opent de pagina-instellingen, klik op de pijl rechts van de naam van het profiel.
2. Aan de bovenkant van de pagina, klikt u op de **eindpunten** om de eindpunten die zijn opgenomen in de configuratie weer te geven.
3. Klik op het eindpunt dat u wilt inschakelen en klik vervolgens op **inschakelen** onder aan de pagina.
4. Clients worden doorgestuurd naar het eindpunt ingeschakeld zoals dit door het profiel.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>Een cloud-service of website eindpunt verwijderen

1. Zoek het profiel verkeer Manager met de endpoint-instellingen die u wilt wijzigen in het deelvenster Beheer van verkeer in de klassieke Azure portal. U opent de pagina-instellingen, klik op de pijl rechts van de naam van het profiel.
2. Aan de bovenkant van de pagina, klikt u op de **eindpunten** om de eindpunten die al deel uitmaken van de configuratie weer te geven.
3. Klik op de naam van het eindpunt dat u wilt verwijderen uit het profiel op de pagina Tunneleindpunten.
4. Onderaan op de pagina, klikt u op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

* [Verkeer profielen beheren](traffic-manager-manage-profiles.md)
* [Routeringsmethoden configureren](traffic-manager-configure-routing-method.md)
* [Probleemoplossing verkeer Manager verslechterd staat](traffic-manager-troubleshooting-degraded.md)
* [Overwegingen bij prestaties van verkeer Manager](traffic-manager-performance-considerations.md)
* [Bewerkingen op verkeer Manager (REST API Reference)](http://go.microsoft.com/fwlink/p/?LinkID=313584)
