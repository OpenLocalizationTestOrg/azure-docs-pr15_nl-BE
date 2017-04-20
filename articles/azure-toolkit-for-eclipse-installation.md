<properties
    pageTitle="Installatie van de Toolkit Azure voor Eclips | Microsoft Azure"
    description="Informatie over het installeren van de Toolkit Azure voor Eclips."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->

# <a name="installing-the-azure-toolkit-for-eclipse"></a>Installatie van de Toolkit Azure voor Eclips

De Toolkit Azure voor Eclips biedt sjablonen en functies die u kunt u gemakkelijk maken, ontwikkelen, testen en implementeren van Azure-toepassingen met behulp van de Eclips-ontwikkelomgeving. De Toolkit Azure voor Eclips is een Open Source project, waarvan de broncode beschikbaar onder de MIT-licentie van de site van het project op GitHub op de volgende URL is:

<https://github.com/Microsoft/Azure-Tools-for-Java>

De volgende stappen laten zien hoe de Azure Toolkit voor Eclips installeren.

[AZURE.INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a>De Toolkit Azure voor Eclips installeren

1. Eclips starten.

1. Als Eclips wordt geopend, klikt u op het menu **Help** en klik vervolgens op **Nieuwe Software installeren**, zoals in de volgende afbeelding.

    ![Installatie van de Toolkit Azure voor Eclips][01]

1. Typ in het dialoogvenster **Beschikbare Software** binnen het tekstvak **werken met** **http://dl.microsoft.com/eclipse** , gevolgd door de **Enter** -toets.

1. In het venster **naam** **Azure Toolkit voor Eclips**controleren en schakel het selectievakje **alle sites tijdens bijwerken Contact om te zoeken naar de vereiste software installeren**. Het scherm moet worden weergegeven met de volgende strekking:

    ![Installatie van de Toolkit Azure voor Eclips][02]

1. Als u de **Toolkit voor Eclips Azure**uitvouwt, ziet u de volgende items:

    * **Toepassing inzichten Plugin voor Java**: dit onderdeel kunt u gebruiken van Azure telemetrie logboekregistratie en analysis services voor uw toepassingen en exemplaren van de server.
    * **Azure Access Control Services-Filter**: dit onderdeel biedt ondersteuning voor het verifiëren van toepassingsgebruikers met Azure ACS, single sign-on scenario's inschakelen en logica van de identiteit van de toepassing externalizing.
    * **Algemene invoegtoepassing Azure**: dit onderdeel bevat de algemene functionaliteit die nodig is voor andere onderdelen van de toolkit.
    * **Azure Explorer voor Eclips**: dit onderdeel bevat de algemene functionaliteit die nodig is voor andere onderdelen van de toolkit.
    * **Azure Plugin voor Java Eclips**: dit onderdeel biedt ondersteuning voor het ontwikkelen van projecten die helpen bij het ontwikkelen, testen en implementeren van Java-toepassingen voor het Microsoft Azure cloud in Eclips en via de opdrachtregel.
    * **Azure Web Apps Plugin met Java**: dit onderdeel biedt ondersteuning voor het implementeren van Java-toepassingen naar Microsoft Azure Web App containers.
    * **Microsoft JDBC-stuurprogramma 4.2 voor SQL Server**: dit onderdeel biedt JDBC API voor SQL Server en Microsoft Azure SQL-Database voor Java Platform Enterprise Edition 8.
    * **Pakket voor Apache Qpid Client Libraries voor JMS**: dit onderdeel voorziet in het clientonderdeel JMS uit het project Apache Qpid uw toepassing gebruik van AMQP in Azure Microsoft messaging inschakelen.
    * **Pakket voor Microsoft Azure bibliotheken voor Java**: dit onderdeel biedt API's voor toegang tot Microsoft Azure services, zoals opslag, bus service, service runtime, enz.

1. Klik op **volgende**. (Als u ongebruikelijke vertragingen ondervinden bij de installatie van de toolkit, ervoor te zorgen dat **alle sites tijdens bijwerken Contact om te zoeken naar de vereiste software installeren** ingeschakeld is.)

1. Klik op **volgende**in het dialoogvenster **Details installeren** .

    ![Installatiedetails bekijken][03]

1. Lees de voorwaarden van de licentieovereenkomsten in het dialoogvenster **Licenties bekijken** . Als u de voorwaarden van de gebruiksrechtovereenkomst accepteert, klikt u op **ik ga akkoord met de voorwaarden van de gebruiksrechtovereenkomst** en klik vervolgens op **Voltooien**. (De resterende stappen wordt ervan uitgegaan dat u de voorwaarden van de gebruiksrechtovereenkomst accepteert. Als u de voorwaarden van de gebruiksrechtovereenkomst niet accepteert, sluit u het installatieproces.)

    ![Bekijk licenties][04]

    Eclips zal downloaden en installeren van de vereiste pakketten.

    ![Voortgang van de installatie][05]

1. Als wordt gevraagd opnieuw opstarten Eclips om installatie te voltooien, klikt u op **Ja**.

    ![Prompt starten][06]

## <a name="see-also"></a>Zie ook

Zie de volgende koppelingen voor meer informatie over de Azure Toolkits voor Java IDEs:

- [Azure Toolkit voor Eclips]
  - *Installatie van de Toolkit Azure voor Eclips (dit artikel)*
  - [Maak een Hello World Web App voor Azure in Eclips]
  - [Wat is nieuw in de Azure Toolkit voor Eclips]
- [Azure Toolkit voor IntelliJ]
  - [Installeren van de Azure Toolkit voor IntelliJ]
  - [Maak een Hello World Web App voor Azure in IntelliJ]
  - [Wat is nieuw in de Azure Toolkit voor IntelliJ]

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center].

<!-- URL List -->

[Azure Toolkit voor Eclips]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit voor IntelliJ]: ./azure-toolkit-for-intellij.md
[Maak een Hello World Web App voor Azure in Eclips]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Maak een Hello World Web App voor Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installeren van de Azure Toolkit voor IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Wat is nieuw in de Azure Toolkit voor Eclips]: ./azure-toolkit-for-eclipse-whats-new.md
[Wat is nieuw in de Azure Toolkit voor IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png

