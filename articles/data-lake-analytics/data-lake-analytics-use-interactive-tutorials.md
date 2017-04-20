<properties 
   pageTitle="Informatie over gegevens Lake Analytics en U-SQL de Azure Portal interactieve zelfstudies met | Azure" 
   description="Snel starten met een leren gegevens Lake Analytics en U SQL. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>


# <a name="use-azure-data-lake-analytics-interactive-tutorials"></a>Interactieve handleiding Azure gegevens Lake Analytics gebruiken

De Portal Azure biedt een interactieve zelfstudie voor u aan de slag met Data Lake Analytics. In dit artikel wordt beschreven hoe u de zelfstudie voor het analyseren van Logboeken website doorlopen.


>[AZURE.NOTE]Zie [Logboeken voor website analyseren met gegevens Lake Analytics](data-lake-analytics-analyze-weblogs.md)via dezelfde zelfstudie met behulp van Visual Studio.
>Meer interactieve zelfstudie moet worden toegevoegd aan de portal.


Zie voor andere zelfstudie te kiezen:

- [Aan de slag met Data Lake Analytics met Azure Portal](data-lake-analytics-get-started-portal.md)
- [Aan de slag met Data Lake Analytics met Azure PowerShell](data-lake-analytics-get-started-powershell.md)
- [Aan de slag met Data Lake Analytics met .NET SDK](data-lake-analytics-get-started-net-sdk.md)
- [U SQL scripts schrijven met behulp van hulpmiddelen voor gegevens Lake voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md) 

**Vereisten**

Voordat u deze zelfstudie hebt u het volgende:

- **A gegevens Lake Analytics-account**.  Zie [Aan de slag met Azure gegevens Lake Analytics met Azure Portal](data-lake-analytics-get-started-portal.md).

##<a name="create-data-lake-analytics-account"></a>Gegevens Lake Analytics-account maken 

Voordat u alle taken kunt uitvoeren, moet u een account gegevens Lake Analytics hebben.

Elke account gegevens Lake Analytics heeft een [Gegevensarchief voor Lake Azure](../data-lake-store/data-lake-store-overview.md) account afhankelijkheid.  Deze account is de standaardaccount Lake gegevensopslag genoemd.  Kunt u de account Lake gegevensarchief vooraf of wanneer u uw gegevens Lake Analytics-account maken. In deze zelfstudie maakt u het gegevensarchief Lake rekening met de Analytics-account

**Een Data Lake Analytics-account maken**

1. Aanmelden bij de [Azure Portal](https://portal.azure.com/signin/index/?Microsoft_Azure_Kona=true&Microsoft_Azure_DataLake=true&hubsExtension_ItemHideKey=AzureDataLake_BigStorage%2cAzureKona_BigCompute).
2. Klik op **Microsoft Azure** in de linkerbovenhoek om de StartBoard te openen.
3. Klik op de tegel **Marketplace** .  
3. **Azure gegevens Lake Analytics** typt in het zoekvak in **Alles** blade en druk op **ENTER**. **Azure gegevens Lake Analytics** zien u in de lijst.
4. Klik op **Azure gegevens Lake Analytics** uit de lijst.
5. Klik op **maken** op de onderkant van het blad.
6. Typ of Selecteer de volgende opties:

    ![Azure gegevens Lake Analytics portal blade](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)

    - **Naam**: de naam van de Analytics-account.
    - **Gegevensarchief Lake**: elke gegevens Lake Analytics account een afhankelijke gegevensarchief Lake-account heeft. De gegevens Lake Analytics-account en de afhankelijke Lake gegevensarchief account moeten zich bevinden in dezelfde Azure Datacenter. Volg de instructies voor het maken van een nieuw gegevensarchief Lake-account of een bestaande selecteren.
    - **Abonnement**: Kies de Azure abonnement gebruikt voor het Analytics-account.
    - **Resourcegroep**. Selecteer een bestaande brongroep van Azure of een nieuwe maken. Toepassingen zijn gewoonlijk opgebouwd uit veel onderdelen, bijvoorbeeld een webtoepassing, database, databaseserver, opslag- en 3e partij diensten. Azure Resource Manager (ARM) kunt u werken met de resources in uw toepassing als een groep, een resourcegroep Azure genoemd. U kunt implementeren, bijwerken, controleren of alle bronnen voor uw toepassing in een enkele, gecoördineerde bewerking te verwijderen. U een sjabloon gebruiken voor de implementatie en de sjabloon die voor verschillende omgevingen, zoals het testen, ontwikkel- en productiecomputers kunt werken. U kunt de facturering voor uw organisatie verduidelijken door de samengevouwen kosten voor de hele groep weer te geven. Zie [Azure Resource Manager-overzicht](azure-resource-manager/resource-group-overview.md)voor meer informatie. 
    - **Locatie**. Selecteer een Azure Datacenter voor de account gegevens Lake Analytics. 
7. **Pin om Startboard te**selecteren. Dit is vereist voor deze zelfstudie te volgen.
8. Klik op **maken**. Het gaat u naar de portal StartBoard. Een nieuwe tegel wordt toegevoegd aan de introductiepagina met het label 'Deploying Azure gegevens Lake Analytics' weergegeven. Het duurt even voor het maken van een account gegevens Lake Analytics. Wanneer de account wordt gemaakt, wordt de rekening op een nieuwe blade geopend in de portal.

    ![Azure gegevens Lake Analytics portal blade](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-blade.png)

##<a name="run-website-log-analysis-interactive-tutorial"></a>De interactieve zelfstudie Website logboekanalyse uitvoeren

**De Website Log Analytics interactieve zelfstudie openen**

1. Klik op **Microsoft Azure** in het linkermenu voor het openen van de StartBoard van de Portal.
2. Klik op de tegel die is gekoppeld aan uw account gegevens Lake Analytics.
3. Klik op **verkennen interactieve handleiding** van de **Essentials** -balk.

    ![Lake Data Analytics interactieve zelfstudies](./media/data-lake-analytics-use-interactive-tutorials/data-lake-analytics-explore-interactive-tutorials.png)

4. Als er een oranje waarschuwing met de melding 'monsters niet instellen, klik hier...', klikt u op **Voorbeeldgegevens kopiëren** om de voorbeeldgegevens kopiëren naar de standaardaccount Lake gegevensarchief. De interactieve zelfstudie moet de gegevens uit te voeren.
5. Klik op **Website logboek Analytics**van de bladeserver **Interactieve zelfstudies** . De zelfstudie voor opent de portal in een nieuwe portal blade.
5. **1 Inleiding** op en volg de instructies

##<a name="see-also"></a>Zie ook

- [Overzicht van Microsoft Azure gegevens Lake Analytics](data-lake-analytics-overview.md)
- [Aan de slag met Data Lake Analytics met Azure Portal](data-lake-analytics-get-started-portal.md)
- [Aan de slag met Data Lake Analytics met Azure PowerShell](data-lake-analytics-get-started-powershell.md)
- [U SQL scripts schrijven met behulp van hulpmiddelen voor gegevens Lake voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Met behulp van Azure gegevens Lake Analytics Website-logboekbestanden analyseren](data-lake-analytics-analyze-weblogs.md)
