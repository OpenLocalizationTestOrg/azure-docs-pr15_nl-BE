<properties
  pageTitle="Azure IoT Suite Veelgestelde vragen | Microsoft Azure"
  description="Veelgestelde vragen voor IoT Suite"
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="09/26/2016"
  ms.author="araguila"/>
   
# <a name="frequently-asked-questions-for-iot-suite"></a>Veelgestelde vragen voor IoT Suite

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Wat is het verschil tussen een resourcegroep in Azure portal verwijderen en klikt u op verwijderen op een vooraf geconfigureerde oplossing in azureiotsuite.com?

- Als u de vooraf geconfigureerde oplossing in [azureiotsuite.com][lnk-azureiotsuite], verwijdert u alle bronnen die zijn ingericht bij het maken van de vooraf geconfigureerde oplossing; Als u extra resources toegevoegd aan de resourcegroep, worden deze ook verwijderd. 

- Als u de resourcegroep in de [Azure portal]verwijderen[lnk-azure-portal], verwijdert u alleen de resources in die resourcegroep; u moet ook de Azure Active Directory-toepassing die is gekoppeld aan de vooraf geconfigureerde oplossing in [Azure klassieke portal]verwijderen[lnk-classic-portal].

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Hoeveel exemplaren van IoT Hub kan ik creëren in een abonnement 

Tien. Kunt u een [ticket voor ondersteuning van Azure] [ link-azuresupportticket] te verhogen van deze limiet, maar standaard, u kunt alleen inrichten tien IoT Hubs per abonnement, zoals wordt beschreven in [Azure abonnementen][link-azuresublimits]. Aangezien elke vooraf geconfigureerde oplossing een nieuwe IoT-Hub bepalingen, kunt u daarom alleen maximaal tien vooraf geconfigureerde oplossingen in een bepaald abonnement inrichten. 

### <a name="how-many-documentdb-instances-can-i-provision-in-a-subscription"></a>DocumentDB hoe vaak kan ik creëren in een abonnement

Vijftig. Kunt u een [ticket voor ondersteuning van Azure] [ link-azuresupportticket] te verhogen van deze limiet, maar standaard kunt u alleen vijftig exemplaren van DocumentDB per abonnement inrichten. 

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Hoeveel vrije Bing Maps-API's kan ik in een abonnement inrichten?

Twee. In een Azure-abonnement kunt u alleen de twee interne transacties niveau 1 Bing Maps voor Enterprise plannen. De oplossing voor externe controle is van plan met interne transacties niveau 1 standaard ingericht. Hierdoor kunt u maximaal twee externe controle kan worden uitgevoerd in een abonnement zonder wijzigingen alleen inrichten.

### <a name="i-have-a-remote-monitoring-solution-deployment-with-a-static-map-how-do-i-add-an-interactive-bing-map"></a>Ik heb een externe controle implementatie van oplossingen met een statische kaart, hoe voeg ik een interactieve plattegrond met Bing? 
1. De Bing Maps-API voor Enterprise QueryKey ophalen van [Azure portal][lnk-azure-portal]: 
 1. Ga naar de resourcegroep waarbij de Bing Maps-API voor onderneming in de [Azure portal is][lnk-azure-portal].
 2. Klik op alle instellingen, vervolgens Sleutelbeheer. 
 3. U ziet twee sleutels: MasterKey en QueryKey. Kopieer de waarde voor QueryKey.

     > [AZURE.NOTE] Heb je nog een Bing Maps-API voor rekening van de onderneming? Maken in de [portal Azure] [ lnk-azure-portal] door te klikken op + nieuwe, zoeken naar Bing Maps API voor onderneming en volg de aanwijzingen voor het maken van.

2. De nieuwste code halen uit de [Azure-IoT-afstandsbediening-controle op][lnk-remote-monitoring-github].

3. Uitvoeren van een lokale of commandline implementatierichtlijnen in de map /docs/ in de bibliotheek na implementatie van de wolk. 

4. Nadat u een lokale hebt uitgevoerd of cloud implementatie, kijk in de hoofdmap van de *. user.config-bestand gemaakt tijdens de implementatie. Open dit bestand in een teksteditor. 

5. Wijzig de volgende regel om de waarde die u hebt gekopieerd uit de QueryKey: 
   
  `<setting name="MapApiQueryKey" value="" />`

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>Kan ik een vooraf geconfigureerde oplossing als ik Microsoft Azure voor DreamSpark maken?
U kunt op dit moment een vooraf geconfigureerde oplossing maken met een [Microsoft Azure voor DreamSpark] [ lnk-dreamspark] account. U kunt echter een [gratis proefversie voor Azure-account] maken[ lnk-30daytrial] in een paar minuten in waarmee u een vooraf geconfigureerde oplossing.

### <a name="how-do-i-delete-an-aad-tenant"></a>Hoe verwijder ik een huurder AAD

Blog van Eric Golpe van [scenario van het verwijderen van een advertentie Azure huurder]boeken Zie[lnk-delete-aad-tennant].

## <a name="next-steps"></a>Volgende stappen

U kunt ook een aantal andere functies en mogelijkheden van de IoT Suite vooraf geconfigureerde oplossingen verkennen:

- [Preventief onderhoud vooraf geconfigureerde oplossing, overzicht][lnk-predictive-overview]
- [Beveiliging van de grond omhoog IoT][lnk-security-groundup]

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-security-groundup]: securing-iot-ground-up.md

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[link-azuresublimits]: https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/#iot-hub-limits
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx
