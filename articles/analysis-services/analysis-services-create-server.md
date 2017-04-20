<properties
   pageTitle="Een Analysis Services-server maken in Azure | Microsoft Azure"
   description="Informatie over het maken van een exemplaar van de Analysis Services-server in Azure."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="create-an-analysis-services-server"></a>Een Analysis Services-server maken
Dit artikel helpt u bij het maken van een nieuwe resource van de Analysis Services-server in uw abonnement Azure.

## <a name="before-you-begin"></a>Voordat u begint
U moet om te beginnen:

- **Azure abonnement**: Bezoek [Azure gratis](https://azure.microsoft.com/offers/ms-azr-0044p/) een account aanmaken.
- **Resourcegroep**: een resourcegroep die u al hebt of [een nieuwe maken](../azure-resource-manager/resource-group-overview.md).

> [AZURE.NOTE] Maken van een Analysis Services-server kan leiden tot een nieuwe service te factureren. Voor meer informatie, Zie de prijzen van Analysis Services.

## <a name="create-an-analysis-services-server"></a>Een Analysis Services-server maken

1. Log in om de [Azure portal](https://portal.azure.com).

2. Klik op **+ Nieuw** > **Intelligence + analytics** > **Analysis Services**.

3. In de **Analysis Services** -blade Vul de vereiste velden in en drukt u vervolgens op **maken**.

    ![Server maken](./media/analysis-services-create-server/aas-create-server-blade.png)

    - **Naam**: Typ een unieke naam die wordt gebruikt om te verwijzen naar de server.

    - **Abonnement**: het abonnement op deze server te stuklijsten selecteren.

    - **Resourcegroep**: dit zijn containers ontworpen voor het beheren van een verzameling bronnen Azure. Voor meer informatie, Zie [resourcegroepen](../resource-group-overview.md).

    - **Locatie**: de server fungeert als host voor deze Azure datacenter locatie. Kies een locatie dichtst bij uw grootste gebruikersgroep.

    - **Prijzen laag**: Selecteer een laag prijzen. Tabelvorm 100 GB modellen worden ondersteund. U kunt de prijzen laag later altijd wijzigen.

4. Klik op **maken**.

Maken gewoonlijk duurt minder dan een minuut; vaak een paar seconden. Als u **toevoegen aan de Portal**hebt geselecteerd, gaat u naar uw portal naar de nieuwe server wordt weergegeven. Of Ga naar **meer services** > **Analysis Services** te zien als de server klaar is. Als u niet de lijst te vernieuwen.

 ![Dashboard](./media/analysis-services-create-server/aas-create-server-dashboard.png)


## <a name="next-steps"></a>Volgende stappen
Als u de server hebt gemaakt, kunt u op het [implementeren van een model](analysis-services-deploy.md) met SSDT of met SSMS.

Als u een model implementatie van uw server verbinding met gegevensbronnen voor bedrijfsruimten, moet u een [On-premises gegevensgateway](analysis-services-gateway.md) installeren op een computer in uw netwerk.
