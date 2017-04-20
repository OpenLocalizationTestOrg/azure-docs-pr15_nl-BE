<properties
   pageTitle="Azure Analysis Services implementeren | Microsoft Azure"
   description="Informatie over het implementeren van een model in tabelvorm op een Azure Analysis Services-server."
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

# <a name="deploy-to-azure-analysis-services"></a>Azure Analysis Services implementeren

Als u een server hebt gemaakt van uw abonnement op Azure, bent u klaar voor de implementatie van een database in tabelvorm model aan. U kunt SQL Server Data Tools (SSDT) bouwen en implementeren van een in tabelvorm modelproject waaraan u werkt. Of u kunt SQL Server Management Studio (SSMS) implementeren van een bestaande modeldatabase in tabelvorm van een Analysis Services-instantie.

## <a name="before-you-begin"></a>Voordat u begint
U moet om te beginnen:

- **Analysis Services-server** in Azure. Zie voor meer informatie, [maken een Analysis Services in Azure](analysis-services-create-server.md).
- **In tabelvorm modelproject** in SSDT of een bestaand in tabelvorm model op het niveau van de compatibiliteit van 1200 voor een Analysis Services-instantie. Nooit een gemaakt? Probeer de [zelfstudie van Adventure Works](https://msdn.microsoft.com/library/hh231691.aspx).
- **On-premises gateway** - als een of meer gegevensbronnen op ruimten in het netwerk van uw organisatie zijn, moet u een [gateway voor On-premises-gegevens](analysis-services-gateway.md)installeren. De gateway is nodig voor uw server in de cloud verbinding maken met uw gegevensbronnen op ruimten te verwerken en het vernieuwen van gegevens in het model.

## <a name="to-deploy-a-tabular-model-from-ssdt"></a>Implementatie van een model in tabelvorm uit SSDT
Implementeren van SSDT, moet dat u de [meest recente versie](https://msdn.microsoft.com/library/mt204009.aspx)is bijgewerkt op 30 September 2016 of hoger gebruikt.


> [AZURE.TIP] Voordat u implementeert, moet dat u de gegevens kunt verwerken in uw tabellen. Klik op **Model**in SSDT, > **proces** > **Alle proces**. Als de verwerking mislukt, wordt om te worden geïmplementeerd.

1. Voordat u implementeert, moet u de naam van de server ophalen. In **Azure portal** > server > **Overzicht** > de**naam van de Server**, de naam van de server te kopiëren.

    ![Naam van de server ophalen in Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

2. In SSDT > **Solution Explorer**met de rechtermuisknop op het project > **Eigenschappen**. Vervolgens in de **implementatie van** > plakt u de naam van de**Server** .   

    ![Servernaam plakken in deployment server, eigenschap](./media/analysis-services-deploy/aas-deploy-deployment-server-property.png)

3. In de **Solution Explorer**met de rechtermuisknop op **Eigenschappen**, klik op **Deploy**. Mogelijk moet u aan te melden Azure.

    ![Op server implementeren](./media/analysis-services-deploy/aas-deploy-deploy.png)

    Implementatiestatus wordt weergegeven in het venster uitvoer en distribueren.

    ![Implementatiestatus](./media/analysis-services-deploy/aas-deploy-status.png)

Dat is alles klaar is Kees!

## <a name="to-deploy-using-xmla-script"></a>Implementeren met behulp van XMLA-script
1. SSMS, met de rechtermuisknop op de database in tabelvorm model u wilt implementeren, klikt u op **Script** > **Database Script als** > **maken op**, kies een locatie.

2. De query uitvoeren op de serverexemplaar dat u implementeren wilt op. Als u naar dezelfde server, moet u ten minste de eigenschap **name** in XMLA-script.  


## <a name="but-something-went-wrong"></a>Maar iets verkeerd is gegaan

Als de implementatie mislukt bij het implementeren van metagegevens, is het waarschijnlijk omdat SSDT kan geen verbinding met uw server maken. Controleer of dat u verbinding kunt maken met de server met behulp van SSMS. Controleer vervolgens of dat de Deployment Server-eigenschap voor het project juist is.

Als de implementatie mislukt op een tabel, is het waarschijnlijk omdat uw server kan geen verbinding met een gegevensbron maken. Als de gegevensbron op locatie in het netwerk van uw organisatie, moet u een [gateway voor On-premises-gegevens](analysis-services-gateway.md)installeren.

## <a name="next-steps"></a>Volgende stappen

U hebt uw in tabelvorm model geïmplementeerd op uw server, dus nu tot stand te brengen. U kunt [een verbinding maken met SSMS](analysis-services-manage.md) beheer. En u kunt [een verbinding maken met een clienthulpprogramma voor](analysis-services-connect.md) zoals Power BI, Power BI-bureaublad of Excel en start het maken van rapporten.
