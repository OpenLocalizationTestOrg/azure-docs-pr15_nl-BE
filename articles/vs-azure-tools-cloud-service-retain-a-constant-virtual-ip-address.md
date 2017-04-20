<properties
   pageTitle="Het behouden van constante virtueel IP-adres voor een cloud service | Microsoft Azure"
   description="Leren om ervoor te zorgen dat het virtuele IP-adres (VIP) van uw Azure cloud-service niet wijzigen."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-retain-a-constant-virtual-ip-address-for-a-cloud-service"></a>Het behouden van constante virtueel IP-adres voor een cloud-service

Als u een cloud-service die wordt gehost in Azure, is het mogelijk om ervoor te zorgen dat het virtuele IP-adres (VIP) van de service niet wijzigen. Domain Name System (DNS) veel domain management services gebruiken voor het registreren van domeinnamen. DNS werkt alleen als de VIP hetzelfde blijft. Kunt u de **Wizard publiceren** in Azure Tools om ervoor te zorgen dat de VIP van uw cloud-service niet wijzigen wanneer u deze bijwerkt. Zie [een aangepaste domeinnaam voor een Azure cloud-service configureren](./cloud-services/cloud-services-custom-domain-name.md)voor meer informatie over het gebruik van beheer van DNS-domein voor cloud services.

## <a name="publishing-a-cloud-service-without-changing-its-vip"></a>Een cloud-service te publiceren zonder de VIP

De VIP van een cloud-service wordt toegewezen wanneer u eerst het implementeert voor Azure in een bepaalde omgeving, zoals de productie-omgeving. De VIP niet gewijzigd tenzij u expliciet de implementatie verwijderen of impliciet door het implementatieproces van de update wordt verwijderd. Als u wilt behouden in de VIP, verwijdert u niet uw implementatie en u moet ook zorgen dat Visual Studio automatisch uw distributie niet verwijderen. Door de implementatie-instellingen op te geven in de **Publish Wizard**, waar verschillende implementatieopties ondersteunt, kunt u het gedrag bepalen. Kunt u een nieuwe installatie of een update-implementatie incrementele of gelijktijdig zijn kan, en beide soorten updates installeren de VIP blijft behouden. Zie voor definities van deze verschillende soorten implementatie [Azure Application Wizard publiceren](vs-azure-tools-publish-azure-application-wizard.md).  Bovendien kunt u bepalen of de vorige installatie van een cloud-service wordt verwijderd als er een fout optreedt. De VIP kan onverwacht worden gewijzigd als u niet juist die optie ingesteld.

### <a name="to-update-a-cloud-service-without-changing-its-vip"></a>Een cloud service bijwerken zonder de VIP

1. Nadat u ten minste één keer uw cloud-service implementeert, opent u het snelmenu voor het knooppunt voor uw project Azure en kies vervolgens **publiceren**. De wizard **Publiceren Azure-toepassing** wordt weergegeven.

1. Kies degene die u wilt implementeren in de lijst met abonnementen, en klik op **volgende** . De pagina **Instellingen** van de wizard wordt weergegeven.

1. Controleer of de naam van de cloud-service die u distribueren wilt, het **milieu**, de **Configuratie maken**en de **Configuratie van de Service** zijn alle juiste op het tabblad **Algemene instellingen** .

1. Op het tabblad **Geavanceerde instellingen** , Controleer of de opslag en het label implementatie juist is, dat de **implementatie mislukt verwijderen** is uitgeschakeld en dat het selectievakje **implementatie** bijwerken is geselecteerd. Als u het selectievakje bijwerken van **implementatie** , zorgt u ervoor dat uw implementatie niet verwijderd en uw VIP niet verloren wanneer u de toepassing opnieuw publiceert. De **implementatie op het selectievakje mislukte verwijderen**uit te schakelen, zorgt u ervoor dat uw VIP niet verloren is als er een fout optreedt tijdens de implementatie.

1. Om extra te geven hoe u wilt dat de rollen worden bijgewerkt, kiest u de koppeling **Instellingen** naast het vak van de **implementatie** en kies vervolgens de incrementele update of de optie voor het gelijktijdig bijwerken in het dialoogvenster Instellingen voor **implementatie bijwerken** . Als u incrementele update, elk exemplaar is bijgewerkt na elkaar, zodat de toepassing altijd beschikbaar is. Als u gelijktijdig bijwerken kiest, worden alle exemplaren tegelijk bijgewerkt. Gelijktijdige updates is sneller, maar de service mogelijk niet beschikbaar tijdens het bijwerken.

1. Als u tevreden met uw instellingen bent, klik op **volgende** .

1. Controleer de instellingen op de pagina **Overzicht** van de wizard en klik vervolgens op de knop **publiceren** .

  >[AZURE.WARNING] Als de installatie mislukt, moet u waarom er geen adres en implementeren zo spoedig mogelijk, om te voorkomen dat het verlaten van uw cloud-service in een beschadigde toestand verkeert.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over publiceren naar Azure vanuit Visual Studio, [de wizard Publiceren Azure-toepassing](vs-azure-tools-publish-azure-application-wizard.md).
