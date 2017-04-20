<properties
    pageTitle="Sjablonen met Visual Studio in Azure stapel implementeren | Microsoft Azure"
    description="Informatie over het distribueren van sjablonen met Visual Studio in Azure stapel."
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>Distribueren van sjablonen in Azure Stack met behulp van Visual Studio

Gebruik Visual Studio Azure Resource Manager templates implementeren op de Stack Azure Implementatiemodel.

Resource Manager templates implementeren en inrichten van de middelen voor de toepassing in een enkele, gecoördineerde werking.

1.  Open de Visual Studio 2015 Update 1.

2.  Klik op **bestand**, klikt u op **Nieuw**en klik in het dialoogvenster **Nieuw Project** **Resourcegroep Azure**.

3.  Voer een **naam** voor het nieuwe project en klik vervolgens op **OK**.

4.  Klik op de **virtuele Windows-computer**in het dialoogvenster **Azure sjabloon selecteren** en klik op **OK**.

  In het nieuwe project ziet u een lijst met de beschikbare sjablonen Vouw het knooppunt **sjablonen** in het deelvenster **Solution Explorer** .

5.  In het deelvenster **Solution Explorer** met de rechtermuisknop op de naam van uw project, **distribueren**, klik op **Nieuwe distributie**.

6.  Selecteer in het dialoogvenster **Deploy resourcegroep** in de vervolgkeuzelijst **abonnement** uw abonnement op Microsoft Azure Stack.

7.  Kies een bestaande resourcegroep in de lijst **Groep** of maak een nieuwe.

8.  Kies een locatie in de lijst **bron locatie** en klik vervolgens op **distribueren**.

9.  Voer waarden in voor de parameters (die verschillen per sjabloon) in het dialoogvenster **Parameters bewerken** en klik vervolgens op **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

[Distribueren van sjablonen met de opdrachtregel](azure-stack-deploy-template-command-line.md)
