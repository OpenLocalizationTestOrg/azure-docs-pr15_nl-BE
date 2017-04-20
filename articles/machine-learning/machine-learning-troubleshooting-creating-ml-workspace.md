<properties
    pageTitle="Problemen oplossen: Maken en koppelen aan een werkruimte Machine Learning | Microsoft Azure"
    description="Oplossingen voor veelvoorkomende problemen bij het maken en verbinding maken met een werkruimte Azure Machine Learning"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/09/2016"
    ms.author="garye"/>


# <a name="troubleshooting-guide-create-and-connect-to-an-machine-learning-workspace"></a>Handleiding voor het oplossen van problemen: maken en verbinding maakt met een Machine Learning-werkruimte

Deze handleiding bevat oplossingen voor een aantal uitdagingen vaak aangetroffen bij het instellen van werkruimten Azure Machine Learning.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a>De eigenaar van de werkruimte

Wanneer u een nieuwe Machine Learning werkruimte maakt, de ID die u in het veld eigenaar van de WERKRUIMTE invoert moet een geldige Microsoft-account (voorheen Windows Live ID), bijvoorbeeld, john-contoso@live.com of john-contoso@hotmail.com. Een niet-Microsoft-account, zoals uw zakelijke e-mailaccount kan niet. Als u wilt een gratis Microsoft-account, gaat u naar [www.live.com](http://www.live.com).

Houd er rekening mee dat de account die u gebruikt voor aanmelding op de Azure portal voor het maken van de werkruimte is niet automatisch gemachtigd te *openen* die werkruimte, tenzij u die account als de eigenaar opgeven. Om een werkruimte opent in Machine Learning Studio, u moet zijn aangemeld bij het Microsoft-Account die is gedefinieerd als de eigenaar van de werkruimte, of moet u een uitnodiging ontvangen van de eigenaar te nemen aan de werkruimte. Van de klassieke Azure portal kunt u echter *beheren* van de werkruimte, inclusief de mogelijkheid de eigenaar wijzigen en toegang configureren.

Zie [een Azure Machine Learning werkruimte beheren]voor meer informatie over het beheren van een werkruimte.

[Een werkruimte Azure Machine Learning beheren]: machine-learning-manage-workspace.md

## <a name="allowed-regions"></a>Toegestane gebieden

Machine Learning is momenteel beschikbaar in een beperkt aantal regio's. Als uw abonnement niet een van deze regio's bevat, ziet u het foutbericht, "U hebt geen abonnementen in de toegestane gebieden."

Volg de aanwijzingen om uw aanvraag te verzenden om te vragen dat een gebied worden toegevoegd aan uw abonnement, **Contact met Microsoft ondersteuning** van het klassieke Portal Azure en kies **Factuuradres** als het probleemtype.

![Neem contact op met Microsoft support][screen1]

## <a name="storage-account"></a>Opslag account

De Machine Learning-service moet een account opslagruimte voor het opslaan van gegevens. U kunt een bestaande account voor de opslag of kunt u een nieuwe account voor de opslag bij het maken van de nieuwe Machine Learning werkruimte (als u over quota voor het maken van een nieuwe opslag-account).

<!-- These instructions no longer work, but I'm not sure what to replace them with
To see if you can create a new storage account, in the Classic Portal, go to **Settings** and then click **Usage**.
-->

![Werkruimte maken][screen2]

Nadat de nieuwe Machine Learning-werkruimte wordt gemaakt, kunt u aanmelden bij Machine Learning Studio met behulp van het Microsoft-account dat u hebt opgegeven als de eigenaar van de werkruimte. Als u het foutbericht 'Werkruimte niet gevonden' (vergelijkbaar met de volgende schermafdruk), gebruikt u de volgende stappen uw browsercookies te verwijderen.

![Werkruimte niet gevonden][screen3]

**Browsercookies verwijderen**

Als u Internet Explorer gebruikt, klikt u op de knop **Extra** in de rechterbovenhoek en selecteer **Internet-opties**.  

![Internet-opties][screen4]

Klik op het tabblad **Algemeen** **verwijderen...**

![Tabblad Algemeen][screen5]

Zorg ervoor dat **Cookies en websitegegevens** is geselecteerd in het dialoogvenster **Browsegeschiedenis verwijderen** en klikt u op **verwijderen**.

![Cookies verwijderen][screen6]

Nadat u de cookies hebt verwijderd, start de browser opnieuw en Ga naar de pagina [Microsoft Azure Machine Learning](https://studio.azureml.net) . Wanneer u wordt gevraagd om een gebruikersnaam en wachtwoord, Voer hetzelfde Microsoft-account dat u hebt opgegeven als de eigenaar van de werkruimte.

Ons doel is de Machine leerervaring zo weinig mogelijk te maken. Boek eventuele opmerkingen en problemen bij het [forum Azure Machine Learning](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) op helpt ons u beter van dienst.

[screen1]:media/machine-learning-troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/machine-learning-troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/machine-learning-troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/machine-learning-troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/machine-learning-troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/machine-learning-troubleshooting-creating-ml-workspace/screen6.png
