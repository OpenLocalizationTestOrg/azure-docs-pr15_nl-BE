<properties
    pageTitle="Azure eindpunten"
    description="Beschrijving van de instellingen Azure-Service-eindpunt in de Toolkit Azure voor Eclips."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->

# <a name="azure-service-endpoints"></a>Azure eindpunten #

Azure service-endpoints bepalen dat of uw toepassing is geïmplementeerd en beheerd door de global Azure platform, Azure geëxploiteerd worden door 21Vianet in China of een particulier Azure platform. Het dialoogvenster **Service-Endpoints** kunt u opgeven welke service-eindpunten die u wilt gebruiken. Opent het dialoogvenster **Service-Endpoints** in Eclips, **venster**, klikt u op **Voorkeuren**, **Azure**vouwen en klik vervolgens op **Service-Endpoints**. Het veld **Actieve instellen** bepaalt u welke Azure eindpunten voor de Azure projecten in uw huidige werkruimte wordt gebruikt.

Hieronder ziet u het dialoogvenster **Service-Endpoints** .

![][ic719493]

## <a name="to-set-the-service-endpoints"></a>De eindpunten instellen ##

Klik in het dialoogvenster **Service-Endpoints** nemen een van de volgende acties:

* Als u wilt gebruiken de globale Azure platform in de vervolgkeuzelijst **Instellen voor actieve** **windowsazure.com** selecteren en klik op **OK**.
* Als u met Azure beheerd door 21Vianet in China, in de vervolgkeuzelijst **Instellen voor actieve** **windowsazure.cn (China)** selecteert en op **OK**.
* Als u een persoonlijke Azure platform gebruiken wilt:
    1. Klik op **bewerken**.
    2. Een dialoogvenster geopend waarin wordt gemeld dat het dialoogvenster **Service-eindpunten** worden gesloten, en de voorkeur sets-bestand wordt geopend. Klik op **OK**.
    3. Maak in het bestand preferencesets.xml een nieuwe `preferenceset` element. Maak voor dit nieuwe element `name`, `blob`, `management`, `portalURL` en `publishsettings` kenmerken en waarden toevoegen voor hen die overeenkomen met uw persoonlijke Azure platform. U kunt de waarden voor de bestaande `preferenceset` elementen als sjablonen. **Opmerking**: de waarde die wordt gebruikt voor de `blob` kenmerk 'blob' in de URL van de tekst moet bevatten.
    4. Opslaan en sluiten preferencesets.xml.
    5. Open opnieuw het dialoogvenster **Service-Endpoints** .
    6. Selecteer de actieve set die u hebt gemaakt in de vervolgkeuzelijst **Actief ingesteld** en op **OK**.
    7. Als u uw persoonlijke Azure platform hebt gemaakt `preferenceset` -element, kunt u de waarden die zijn toegewezen door te klikken op de knop **bewerken** in het dialoogvenster **Services eindpunt** . U kunt ook meerdere particuliere Azure platform maken `preferenceset` -elementen, als u wenst.

## <a name="see-also"></a>Zie ook ##

[Azure Toolkit voor Eclips][]

[Installatie van de Toolkit Azure voor Eclips][] 

[Een toepassing Hallo wereld maakt voor Azure in Eclips][]

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkID=699529
[Een toepassing Hallo wereld maakt voor Azure in Eclips]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installatie van de Toolkit Azure voor Eclips]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png
