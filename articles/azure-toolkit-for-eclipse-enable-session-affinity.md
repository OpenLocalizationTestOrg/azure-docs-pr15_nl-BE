<properties
    pageTitle="Sessie-affiniteit met de Toolkit Azure voor Eclips inschakelen"
    description="Informatie over het inschakelen van de affiniteit van sessie met de Toolkit Azure voor Eclips."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->

# <a name="enable-session-affinity"></a>Affiniteit van sessie inschakelen #

Binnen de Azure Toolkit voor Eclips kunt u HTTP-sessie affiniteit of "sticky sessies', voor de rollen. De volgende afbeelding toont de **Taakverdeling** eigenschappen dialoogvenster gebruikt de functie sessie-affiniteit in te schakelen:

![][ic719492]

## <a name="to-enable-session-affinity-for-your-role"></a>Sessie-affiniteit voor uw rol inschakelen ##

1. Klik met de rechtermuisknop op de rol in de Projectverkenner van de Eclips **Azure**en klik vervolgens op **Load Balancing**.
1. In het dialoogvenster **Eigenschappen voor netwerktaakverdeling van WorkerRole1** :
    1. Controleer **Schakel HTTP-sessie affiniteit (sticky sessies) voor deze rol.**
    1. Voor **Input eindpunt te gebruiken**, selecteert u een eindpunt invoer te gebruiken, bijvoorbeeld **http (openbare: 80 privé: 8080)**. Uw toepassing moet dit eindpunt gebruiken als de HTTP-eindpunt. U kunt meerdere endpoints voor uw rol inschakelen, maar kunt u slechts één van beide sticky sessies te ondersteunen.
    1. Uw toepassing opnieuw opbouwt.

Eenmaal is ingeschakeld, hebt u meer dan één exemplaar van de rol, blijven HTTP-aanvragen die afkomstig zijn van een bepaalde client worden verwerkt door het exemplaar met dezelfde rol.

De Eclips Toolkit maakt dit mogelijk door een speciale toepassing aanvragen routering (ARR) genoemd in elk van de exemplaren van de rol van IIS-module te installeren. ARR HTTP-aanvragen naar de juiste rol instantie wordt omgeleid. Het geselecteerde eindpunt de toolkit automatisch geconfigureerd zodat het binnenkomend HTTP-verkeer eerst gerouteerd naar het ARR-software. De toolkit maakt ook een nieuwe interne eindpunt dat de Java-server is geconfigureerd om naar te luisteren. Dat is het eindpunt door ARR gebruikt het HTTP-verkeer met de geschikte rol exemplaar worden omgeleid. Op deze manier elke instantie rol bij de implementatie van meerdere instanties fungeert als een reverse-proxy voor alle andere instanties, sticky sessies inschakelen.

## <a name="notes-about-session-affinity"></a>Opmerkingen over sessie affiniteit ##

* Affiniteit sessie werkt niet in de compute-emulator. De instellingen kunnen worden toegepast in de compute-emulator geen conflicten ontstaan met uw bouwproces of berekenen emulator worden uitgevoerd, maar de functie zelf niet werkt in de compute-emulator.
* Inschakelen van de affiniteit van de sessie leidt tot een verhoging van de schijfruimte ingenomen door de implementatie in Azure, als aanvullende software wordt gedownload en geïnstalleerd in de exemplaren van uw rol als de service wordt gestart in de Azure cloud.
* De tijd voor elke rol zal langer duren.
* Interne eindpunt, functioneren als een rerouter verkeer zoals hierboven vermeld, worden added.ss

Zie [sessiegegevens onderhouden met sessie-affiniteit][]voor een voorbeeld van het beheren van sessiegegevens bij sessie affiniteit is ingeschakeld.

## <a name="see-also"></a>Zie ook ##

[Azure Toolkit voor Eclips][]

[Een toepassing Hallo wereld maakt voor Azure in Eclips][]

[Installatie van de Toolkit Azure voor Eclips][] 

[Het beheren van sessiegegevens met affiniteit sessie][]

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkID=699529
[Een toepassing Hallo wereld maakt voor Azure in Eclips]: http://go.microsoft.com/fwlink/?LinkID=699533
[Het beheren van sessiegegevens met affiniteit sessie]: http://go.microsoft.com/fwlink/?LinkID=699539
[Installatie van de Toolkit Azure voor Eclips]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png
