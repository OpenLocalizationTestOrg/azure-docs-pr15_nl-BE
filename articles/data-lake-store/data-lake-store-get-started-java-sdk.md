<properties
   pageTitle="Data Lake winkel Java SDK gebruiken om toepassingen te ontwikkelen | Microsoft Azure"
   description="Azure Data Lake winkel Java SDK gebruiken om toepassingen te ontwikkelen"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/17/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-java"></a>Aan de slag met Azure Lake gegevensarchief maken met Java

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST-API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Informatie over het Azure Data Lake winkel Java SDK gebruiken voor het uitvoeren van eenvoudige bewerkingen zoals zoals mappen maakt, uploaden en downloaden van bestanden, enz. Zie voor meer informatie over gegevens Lake [Azure Lake gegevensarchief](data-lake-store-overview.md).

Toegang tot documenten van de Java SDK API voor Azure gegevensarchief Lake op [Azure Data Lake winkel Java API docs](https://azure.github.io/azure-data-lake-store-java/javadoc/).

## <a name="prerequisites"></a>Vereisten

* Java Development Kit (JDK 7 of hoger, met Java versie 1.7 of hoger)
* Azure Lake gegevensarchief account. Volg de instructies op de [aan de slag met Azure Lake gegevensarchief maken met de Portal Azure](data-lake-store-get-started-portal.md).
* [Maven](https://maven.apache.org/install.html). In deze zelfstudie wordt Maven voor bouwen en project afhankelijkheden. Hoewel het mogelijk te maken zonder gebruik van een systeem opbouwen zoals Maven of Gradle, is deze systemen maken veel gemakkelijker te beheren van afhankelijkheden.
* (Optioneel) En IDE zoals [IntelliJ IDEE](https://www.jetbrains.com/idea/download/) of [Eclips](https://www.eclipse.org/downloads/) of soortgelijke.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Hoe u verifiëren met Azure Active Directory?

In deze zelfstudie gebruiken we een geheim Azure AD toepassing client voor het ophalen van een token Azure Active Directory (service-service-verificatie). We gebruiken deze token een gegevensarchief Lake client-object voor het uitvoeren van bewerkingen van bestands- en bewerkingen maken. Voor instructies voor het verifiëren van Azure Lake gegevensarchief maken met het geheim van de client, uitvoeren we de volgende hoofdstappen:

1. Een webtoepassing Azure advertentie maken
2. De client-ID, token eindpunt voor de webtoepassing Azure AD en geheim van de client worden opgehaald.
3. Toegang voor de webtoepassing Azure AD configureren in het gegevensarchief Lake bestand of de map die u wilt openen op de Java-toepassing die u maakt.

Zie voor instructies over hoe u deze stappen uitvoert, [een Active Directory-toepassing maken](data-lake-store-authenticate-using-active-directory.md#create-an-active-directory-application).

Azure Active Directory beschikt over andere opties ook op te halen van een token. U kunt kiezen uit een aantal verschillende verificatiemechanismen op maat van uw scenario, bijvoorbeeld een toepassing wordt uitgevoerd in een browser, een toepassing die wordt gedistribueerd als een bureaubladtoepassing of een servertoepassing op locatie of in een Azure virtuele machine. U kunt ook kiezen uit verschillende soorten referenties zoals wachtwoorden, certificaten, 2-factor verificatie, enz. Bovendien kunt Azure Active Directory u uw Active Directory-gebruikers op de ruimten worden gesynchroniseerd met de cloud. Zie [Verificatie-scenario's voor Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md)voor meer informatie. 

## <a name="create-a-java-application"></a>Een Java-toepassing maken

Voorbeeld van de code beschikbaar [op GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) begeleidt u door het proces van het maken van bestanden in het archief, aaneen bestanden, downloaden van een bestand en het verwijderen van sommige bestanden in het archief. Deze sectie van het artikel leest u de belangrijkste onderdelen van de code.

1. Maak een Maven project met behulp van [archetype mvn](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) vanaf de opdrachtregel of met een IDE. Zie voor instructies voor het maken van een IntelliJ met Java-project [hier](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html). Zie voor instructies voor het maken van een project met Eclips [hier](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm). 

2. Voeg de volgende afhankelijkheden in uw **pom.xml** bestand met Maven. Toevoegen van het volgende fragment van de tekst tussen de ** \</version >** tag en de ** \</project >** code:

        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.0.4-SNAPSHOT</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>

    De eerste afhankelijkheid is het gebruik van de Data Lake winkel SDK (`azure-datalake-store`) uit de opslagplaats maven. De tweede afhankelijkheid (`slf4j-nop`) op te geven welke framework logboekregistratie voor deze toepassing is. De SDK gegevens Lake winkel gevel logboekregistratie van [slf4j](http://www.slf4j.org/) , waarin u kiezen uit een aantal populaire logboekregistratie kaders, zoals log4j kunt, logboekregistratie, logback, enz., Java wordt gebruikt of er geen logboekregistratie. In dit voorbeeld zullen we logboekregistratie uitschakelen, daarom gebruiken we de **slf4j nop** -binding. Als u andere opties voor logboekregistratie in uw app, Zie [hier](http://www.slf4j.org/manual.html#projectDep).

### <a name="add-the-application-code"></a>De toepassingscode toevoegen

Er zijn drie belangrijkste onderdelen van de code.

1. De Azure Active Directory-token verkrijgen

2. Het token gebruik te maken van een client Lake gegevensarchief.

3. Gebruik de client Lake gegevensarchief bewerkingen uit te voeren.

#### <a name="step-1-obtain-an-azure-active-directory-token"></a>Stap 1: Een Azure Active Directory-token verkrijgen.

De gegevens Lake winkel SDK vindt u handige methoden waarmee u de beveiligingstokens nodig contact opnemen met het gegevensarchief Lake-account verkrijgen. Echter, de SDK schrijft niet voor dat alleen deze methoden worden gebruikt. U kunt een andere manier voor het verkrijgen van tokens, net zoals de [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java)of uw eigen aangepaste code gebruiken.

De Data Lake winkel SDK verkrijgen van tokens voor de Active Directory-webtoepassing u eerder hebt gemaakt, gebruikt de statische methoden in `AzureADAuthenticator` klasse. **Invullen-hier** vervangen door de werkelijke waarden voor de webtoepassing Azure Active Directory.

    private static String clientId = "FILL-IN-HERE";
    private static String authTokenEndpoint = "FILL-IN-HERE";
    private static String clientKey = "FILL-IN-HERE";

    AzureADToken token = AzureADAuthenticator.getTokenUsingClientCreds(authTokenEndpoint, clientId, clientKey);

#### <a name="step-2-create-an-azure-data-lake-store-client-adlstoreclient-object"></a>Stap 2: Maak een object Azure Lake gegevensarchief client (ADLStoreClient)

Een [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) -object maken, moet u de naam van de gegevensopslag Lake en de Azure Active Directory-token die u hebt gegenereerd in de laatste stap. Houd er rekening mee dat de naam van de gegevensopslag Lake moet een volledig gekwalificeerde domeinnaam. Bijvoorbeeld vervangen door **Invullen-hier** iets zoals **mydatalakestore.azuredatalakestore.net**.

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just the account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, token);

### <a name="step-3-use-the-adlstoreclient-to-perform-file-and-directory-operations"></a>Stap 3: Gebruik de ADLStoreClient voor het uitvoeren van bewerkingen voor bestanden en mappen

De volgende code bevat een voorbeeld van de fragmenten van enkele veelvoorkomende bewerkingen. U kunt de volledige [gegevens Lake winkel Java SDK API docs](https://azure.github.io/azure-data-lake-store-java/javadoc/) van het object **ADLStoreClient** om te zien andere bewerkingen bekijken.
 
Houd er rekening mee dat bestanden gelezen uit en geschreven naar het gebruik van standaard Java stromen. Dit betekent dat u kunt lagen van de Java-streams op Lake gegevensarchief stromen om te profiteren van de standaardfunctionaliteit voor Java (bijv. afdrukken streams voor de opgemaakte uitvoer, of een van de compressie of codering streams voor extra functionaliteit op de top, enz.).

    // set file permission
    client.setPermission(filename, "744");

    // append to file
    stream = client.getAppendStream(filename);
    stream.write(getSampleContent());
    stream.close();

    // Read File
    InputStream in = client.getReadStream(filename);
    byte[] b = new byte[64000];
    while (in.read(b) != -1) {
        System.out.write(b);
    }
    in.close();

    // concatenate the two files into one
    List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
    client.concatenateFiles("/a/b/f.txt", fileList);

    //rename the file
    client.rename("/a/b/f.txt", "/a/b/g.txt");

    // list directory contents
    List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
    System.out.println("Directory listing for directory /a/b:");
    for (DirectoryEntry entry : list) {
        printDirectoryInfo(entry);
    }

    // delete directory along with all the subdirectories and files in it
    client.deleteRecursive("/a");

#### <a name="step-4-build-and-run-the-application"></a>Stap 4: Bouwen en uitvoeren van de toepassing

1. Als u wilt uitvoeren vanuit een IDE, zoeken en druk op de knop **uitvoeren** . Gebruik voor het uitvoeren van Maven, [exec: exec](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html).

2. Produceren van een zelfstandige jar die u kunt uitvoeren vanaf de opdrachtregel de jar samenstellen met alle afhankelijkheden opgenomen, met behulp van de [assembly-plugin Maven](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html). De pom.xml in de [voorbeeld-broncode op github](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) is een voorbeeld van hoe u dit doet.


## <a name="next-steps"></a>Volgende stappen

- [Beveiliging van gegevens in het gegevensarchief Lake](data-lake-store-secure-data.md)
- [Azure gegevens Lake Analytics gebruiken met Lake gegevensarchief](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Lake gegevensarchief](data-lake-store-hdinsight-hadoop-use-portal.md)
