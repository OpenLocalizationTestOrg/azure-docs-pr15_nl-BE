<properties 
    pageTitle="Algemene beheertaken voor cloud service | Microsoft Azure" 
    description="Informatie over het beheren van services in de portal Azure cloud. Deze voorbeelden gebruiken de Azure portal." 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/02/2016"
    ms.author="adegeo"/>


# <a name="how-to-manage-cloud-services"></a>Cloud-Services beheren

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-how-to-manage-portal.md)
- [Azure klassieke portal](cloud-services-how-to-manage.md)

Uw cloud-service wordt op het gebied van **Cloud-Services (klassiek)** van de Azure portal beheerd. Dit artikel bevat enkele veelgebruikte acties als wanneer u nemen bij het beheren van uw cloud-services. Waaronder de bijwerken, verwijderen, schalen en bevordering van een gefaseerde implementatie voor de productie.

Meer informatie over het schalen van uw cloud-service is beschikbaar [hier](cloud-services-how-to-scale-portal.md).

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Procedure: werken met een cloud service rol of implementatie

Als de toepassingscode voor uw cloud-service, moet u **werken** op gebruiken de cloud service blade. U kunt een enkele of alle rollen bijwerken. Als u wilt bijwerken, kunt u een nieuw servicepakket of -service-configuratiebestand uploaden.

1. Selecteer in de [portal Azure][]cloud service die u wilt bijwerken. Deze stap opent u de bladeserver cloud service-exemplaar.

2. In het blad, klikt u op de knop **bijwerken** .

    ![Knop bijwerken](./media/cloud-services-how-to-manage-portal/update-button.png)

3. De implementatie bijwerken met een nieuwe service-pakketbestand (.cspkg) en service-configuratiebestand (.cscfg).

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. **Eventueel** bijwerken het label voor de implementatie en de opslag-account. 

5. Als u geen rollen hebt slechts één exemplaar van de rol, selecteer het **implementeren, zelfs als een of meer rollen een enkel exemplaar bevatten** waarmee de upgradeprocedure kan worden voortgezet. 

    Azure alleen garantie 99.95% beschikbaarheid van de service tijdens het bijwerken van een cloud-service als elke rol ten minste twee exemplaren van rol (virtuele machines heeft). Met twee exemplaren van de rol, wordt één virtuele machine clientaanvragen verwerkt, terwijl het andere wordt bijgewerkt.

6. Controleer de **implementatie starten** als u wilt dat de update is toegepast nadat het uploaden van het pakket is voltooid.

7. Klik op **OK** om te beginnen met het bijwerken van de service.



## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Procedure: wisselen implementaties ter bevordering van een gefaseerde implementatie voor de productie

Wanneer u besluit een nieuwe versie van het werkgebied een cloud-service, implementeren en testen van de nieuwe versie in uw testomgeving van cloud-service. Met de **wisselen** kunt u de URL's die de twee implementaties zijn gericht en bevordering van een nieuwe versie aan productie. 

U kunt omwisselen implementaties van de pagina **Cloud Services** of het dashboard.

1. Selecteer in de [portal Azure][]cloud service die u wilt bijwerken. Deze stap opent u de bladeserver cloud service-exemplaar.

2. In het blad, klikt u op de knop **wisselen** .

    ![Cloud Services Swap](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. Het volgende bevestigingsbericht wordt geopend.

    ![Cloud Services Swap](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Na het controleren van de informatie over implementatie klikt u op **OK** om te wisselen de implementaties.

    De swap implementatie gebeurt snel omdat het enige wat verandert, de virtuele IP-adressen (VIP's is) voor de implementatie.

    Als u wilt berekenen kosten besparen, kunt u de staging-implementatie verwijderen nadat u hebt gecontroleerd dat de productie-implementatie werkt zoals verwacht.

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Procedure: een resource koppelen aan een cloud-service

De Azure portal wordt niet resources aan elkaar koppelen zoals de huidige Azure klassieke portal biedt. In plaats daarvan aanvullende bronnen op dezelfde resourcegroep wordt gebruikt door de Service Cloud implementeren.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Procedure: implementaties en een cloud-service verwijderen

Voordat u een service cloud verwijderen kunt, moet u elke bestaande installatie verwijderen.

Als u wilt berekenen kosten besparen, kunt u de staging-implementatie verwijderen nadat u hebt gecontroleerd dat de productie-implementatie werkt zoals verwacht. Je krijgt de rekening voor de kosten berekenen voor gedistribueerde rol exemplaren die zijn gestopt.

Gebruik de volgende procedure om een distributie of uw cloud-service te verwijderen. 

1. Selecteer in de [portal Azure][]cloud service die u wilt verwijderen. Deze stap opent u de bladeserver cloud service-exemplaar.

2. In het blad, klikt u op de knop **verwijderen** .

    ![Cloud Services Swap](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. U kunt de hele cloud-service verwijderen door **Cloud-service en de distributie** te controleren of kies de **productie-implementatie** of de **Staging-implementatie**.

    ![Cloud Services Swap](./media/cloud-services-how-to-manage-portal/delete-blade.png) 

4. Klik op de knop **verwijderen** onder in het venster.

5. Als u wilt verwijderen in de cloud-service, klikt u op **verwijderen cloud-service**. Klik vervolgens op de bevestigingsprompt op **Ja**.

> [AZURE.NOTE]
> Wanneer een cloud-service wordt verwijderd en uitgebreid toezicht is geconfigureerd, moet u de gegevens handmatig verwijderen uit uw account opslag. Zie [Dit](cloud-services-how-to-monitor.md) artikel voor meer informatie over waar de tabellen met statistieken.

[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a>Volgende stappen

* [Algemene configuratie van de service cloud](cloud-services-how-to-configure-portal.md).
* Meer informatie over het [implementeren van een cloud-service](cloud-services-how-to-create-deploy-portal.md).
* Een [aangepaste domeinnaam](cloud-services-custom-domain-name-portal.md)configureren.
* [Ssl-certificaten](cloud-services-configure-ssl-certificate-portal.md)configureren.