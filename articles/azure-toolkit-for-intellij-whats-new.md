<properties
    pageTitle="Wat is nieuw in de Azure Toolkit voor IntelliJ | Microsoft Azure"
    description="Meer informatie over de nieuwste functies in de Azure-Toolkit voor IntelliJ."
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
    ms.date="08/26/2016" 
    ms.author="robmcm;asirveda;martinsawicki"/>

# <a name="whats-new-in-the-azure-toolkit-for-intellij"></a>Wat is nieuw in de Azure Toolkit voor IntelliJ

## <a name="azure-toolkit-for-intellij-releases"></a>Azure Toolkit voor IntelliJ Releases

Dit artikel bevat informatie over de verschillende versies en de meest recente updates voor de Azure Toolkit voor IntelliJ.

> [AZURE.NOTE] Er is ook een Azure-Toolkit voor de Eclips IDE. Zie voor meer informatie [Azure Toolkit voor Eclips].

### <a name="august-26-2016"></a>26 augustus 2016

De Toolkit Azure voor IntelliJ - augustus 2016 release bevat de volgende uitbreidingen:

* **Aangepaste JDK verdelingen**. De Toolkit Azure voor IntelliJ ondersteunt nu opgeven en implementeren van een willekeurige versie van de JDK aan uw Azure WebApp container:
  - Naast de JDKs geleverd door Azure, kunt u ook kiezen uit een ruime keuze aan OpenJDK Zulu versies beschikbaar gesteld op Azure Azul systemen.
  - Als u deze als een ZIP-bestand naar uw account opslag uploaden, kunt u uw eigen JDK distributie opgeven.
* **Verbeteringen in de weergave van de Verkenner Azure**:
  - Ondersteuning voor virtuele Machine beheer met behulp van de nieuwe Resource Manager model van Azure: u kunt weergeven, maken en verwijderen van resource manager gebaseerde virtuele machines zonder de IDE.
  - Ondersteuning voor opslag blob accountbeheer van Azure Resource Manager, die een aanvulling vormt op de bestaande functionaliteit voor het beheren van accounts 'klassieke' opslag gebruiken.
* **Microsoft JDBC-stuurprogramma 6.0 voor SQL Server**. Deze update bevat de meest recente stuurprogramma voor JDBC voor Microsoft SQL Server (v6.0), die is opgenomen als een bibliotheek die u gemakkelijk aan uw Java projecten toevoegen kunt, waardoor de oudere versie vervangen.

### <a name="june-29-2016"></a>29 juni 2016

De Toolkit Azure voor IntelliJ - juni 2016 release bevat de volgende uitbreidingen:

* **Java 8 vereiste**. De Toolkit Azure voor IntelliJ moet nu Java 8, hoewel deze vereiste alleen voor de toolkit is-toepassingen kunnen blijven gebruiken alle versies van Java die worden ondersteund door Azure.
* **Ondersteuning voor de nieuwste Java-JDKs**. De nieuwste versies van de Java-JDKs worden nu ondersteund door de Azure-Toolkit voor IntelliJ.
* **Ondersteuning voor Azure SDK v2.9.1**. De meest recente versie van de SDK Azure is nu de minimale spelen voor de Azure Toolkit voor IntelliJ.
* **Geïntegreerde monsters**. De Toolkit Azure voor IntelliJ biedt nu verschillende voorbeeldtoepassingen waarmee ontwikkelaars aan de slag.
* **Integratie van HDInsight gereedschap**. Hulpprogramma's van Azure-HDInsight zijn nu gebundeld met de Toolkit Azure voor IntelliJ. Zie voor meer informatie [HDInsight extra Plugin voor IntelliJ].
* **Externe foutopsporing van Java Web Apps**. De Toolkit Azure voor IntelliJ biedt nu ondersteuning voor foutopsporing op afstand van Java web apps Azure App-service.

### <a name="april-12-2016"></a>12 april 2016

De Toolkit Azure voor IntelliJ - April 2016 release bevat de volgende uitbreidingen:

* **Ondersteuning voor Azure SDK v2.9.0**. De meest recente versie van de SDK Azure is nu de minimale spelen voor de Azure Toolkit voor IntelliJ.
* **Diverse gebruiksvriendelijkheid, alertheid en prestaties verbeteringen die betrekking hebben op ondersteuning van Azure Web App**. Een aantal prestatieverbeteringen in hoe de Toolkit met Azure resultaat communiceert in een meer responsieve gebruikersinterface.
* **Mogelijkheid tot het verwijderen van een bestaande webtoepassing container in Azure uit binnen IntelliJ**. De Toolkit Azure voor IntelliJ kunt u nu een bestaande webpagina Azure container zonder IntelliJ verwijderen.

## <a name="see-also"></a>Zie ook ##

Zie de volgende koppelingen voor meer informatie over de Azure Toolkits voor Java IDEs:

- [Azure Toolkit voor Eclips]
  - [Installatie van de Toolkit Azure voor Eclips]
  - [Maak een Hello World Web App voor Azure in Eclips]
  - [Wat is nieuw in de Azure Toolkit voor Eclips]
- [Azure Toolkit voor IntelliJ]
  - [Installeren van de Azure Toolkit voor IntelliJ]
  - [Maak een Hello World Web App voor Azure in IntelliJ]
  - *Wat is nieuw in de Azure Toolkit voor IntelliJ (dit artikel)*

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center].

<!-- URL List -->

[Azure Toolkit voor Eclips]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit voor IntelliJ]: ./azure-toolkit-for-intellij.md
[Maak een Hello World Web App voor Azure in Eclips]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Maak een Hello World Web App voor Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installatie van de Toolkit Azure voor Eclips]: ./azure-toolkit-for-eclipse-installation.md
[Installeren van de Azure Toolkit voor IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Wat is nieuw in de Azure Toolkit voor Eclips]: ./azure-toolkit-for-eclipse-whats-new.md
[What's New in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547

[HDInsight extra Plugin voor IntelliJ]: ./hdinsight/hdinsight-apache-spark-intellij-tool-plugin.md
