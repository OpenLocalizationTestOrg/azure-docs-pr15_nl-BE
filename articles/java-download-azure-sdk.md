<properties 
    pageTitle="De Azure SDK voor Java downloaden" 
    description="Informatie over het downloaden van de Azure SDK voor Java, met voorbeeldcode voor Maven projecten en eenvoudige installatiestappen voor de Azure Tookit voor Eclips." 
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

# <a name="download-the-azure-sdk-for-java"></a>De Azure SDK voor Java downloaden

Dit artikel bevat instructies voor het downloaden en installeren van de bibliotheken Azure voor Java.

**Opmerking:** De bibliotheken Azure voor Java worden verdeeld onder de [Apache License, versie 2.0][license].

## <a name="azure-libraries-for-java---manual-download"></a>Azure Libraries voor Java - handmatig downloaden

Klik op <http://go.microsoft.com/fwlink/?LinkId=690320> om te downloaden van een ZIP-bestand dat alle bibliotheken en alle afhankelijkheden bevat om handmatig de Azure Libraries voor Java.

Nadat u het zip-bestand naar uw computer hebt gedownload, pak de inhoud en een van de volgende opties om de JAR-bestanden toevoegen aan uw project:

* De JAR-bestanden importeren in uw project Java in Eclips.

* Configureer het **Pad maken** voor uw project Java in Eclips ook het pad naar de JAR-bestanden.

Zie voor gedetailleerde informatie over het instellen van build paden in Eclips [Java-pad maken] in het artikel op de website Eclips.

**Opmerking:** Zie het bestand ThirdPartyNotices.txt in de ZIP voor licentie- en andere informatie en license.txt.

## <a name="azure-libraries-for-java---building-with-maven"></a>Azure Libraries voor Java - bouwen met Maven

### <a name="step-1---set-up-your-project-to-use-maven-for-build"></a>Stap 1 - uw project instellen voor gebruik van Maven voor build

Maven projecten maken in Eclips waarmee de Azure libraries voor Java, volgens de instructies in de [Aan de slag met Azure Management Libraries voor Java] [ maven-getting-started] artikel. 

### <a name="step-2---configure-your-maven-settings-with-the-requisite-dependencies"></a>Stap 2: de Maven-instellingen configureren met de vereiste afhankelijkheden

Nadat uw project is geconfigureerd voor het gebruik van Maven voor build, kunt u toevoegen de vereiste afhankelijkheden aan uw pom.xml bestand met behulp van de syntaxis, zoals in het volgende voorbeeld. Houd er rekening mee dat u niet wilt toevoegen om afhankelijkheid die wordt vermeld in het volgende voorbeeld. u hoeft alleen de specifieke afhankelijkheden waarvoor u uw project toevoegen.

> [AZURE.NOTE] Binnen elk `<version>` element in het volgende voorbeeld wordt de 'n.n.n' tijdelijke aanduidingen vervangen door in dit voorbeeld geldige versienummers die kan worden verkregen uit de [Opslagplaats van Azure bibliotheken op Maven].

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-compute</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-network</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-sql</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-storage</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-websites</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-media</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-serviceruntime</artifactId>
        <version>n.n.n</version>
    </dependency>

## <a name="installing-the-azure-toolkit-for-eclipse"></a>Installatie van de Toolkit Azure voor Eclips

Deze sectie bevat algemene instructies voor het installeren van de Toolkit Azure voor Eclips; Zie de [installatie van de Toolkit Azure voor Eclips]voor gedetailleerde instructies.

### <a name="prerequisites"></a>Vereisten

1. Windows operting-systemen die worden vermeld in het artikel [Wat is nieuw in de Toolkit Azure voor Eclips] .
1. Macintosh- of Linux operting systemen die zijn vermeld in het artikel [Wat is nieuw in de Toolkit Azure voor Eclips] .
1. Voor ontwikkelaars van Java EE, IDE-Eclips Indigo of hoger. Dit kan worden gedownload van <http://www.eclipse.org/downloads/>.

### <a name="basic-installation-steps"></a>Eenvoudige installatiestappen

1. Selecteer in het menu **Help** in Eclips, **Nieuwe Software installeren**.
1. Geef de locatie van site <http://dl.microsoft.com/eclipse> en druk op **Enter**.
1. Selecteer de items worden geïnstalleerd en klik op **Voltooien**.

De Toolkit Azure voor Eclips gebruikt de meest recente versie van de SDK Azure. Dit kan worden gedownload via het Web Platform Installer (WebPI) op <http://go.microsoft.com/fwlink/?LinkID=252838>. Echter, als u niet geïnstalleerd hebt, bij het maken van uw eerste Azure-implementatieproject de Azure Toolkit voor Eclips wordt automatisch geïnstalleerd de juiste versie van de SDK Azure.

## <a name="see-also"></a>Zie ook

[Azure Toolkit voor Eclips]

[Installatie van de Toolkit Azure voor Eclips] 

[Een toepassing Hallo wereld maakt voor Azure in Eclips]

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure bibliotheken opslagplaats op Maven]: http://go.microsoft.com/fwlink/?LinkID=286274
[Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkID=699529
[Een toepassing Hallo wereld maakt voor Azure in Eclips]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installatie van de Toolkit Azure voor Eclips]: http://go.microsoft.com/fwlink/?LinkId=699546
[Java-bouwpad]: http://help.eclipse.org/luna/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2Freference%2Fref-properties-build-path.htm
[license]: http://www.apache.org/licenses/LICENSE-2.0.html
[maven-getting-started]: http://go.microsoft.com/fwlink/?LinkID=622998
[zip-download]: http://go.microsoft.com/fwlink/?LinkId=690320
[Wat is nieuw in de Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkId=690333
