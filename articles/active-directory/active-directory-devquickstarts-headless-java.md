<properties
    pageTitle="Azure AD Java aan de slag | Microsoft Azure"
    description="Het bouwen van een Java-opdrachtregel app die gebruikers in een API toegang ondertekent."
    services="active-directory"
    documentationCenter="java"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
  ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="using-java-command-line-app-to-access-an-api-with-azure-ad"></a>Met behulp van Java opdrachtregel App voor toegang tot een met AD Azure-API

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD maakt het eenvoudig en ongecompliceerd voor het uitbesteden van uw web app identiteitsbeheer, bieden één aanmeldings- en afmeldingstijden met slechts een paar regels code.  In Java web apps, kunt u dit doen met behulp van de Microsoft implementatie van de Gemeenschap op basis van beschikbare resources ADAL4J.

  Hier gebruiken we de ADAL4J aan:
- De gebruiker inloggen op de app met Azure AD als de id-provider.
- Sommige informatie weergeven over de gebruiker.
- De gebruiker van de toepassing ondertekenen.

Hiervoor moet u naar:

1. Een toepassing met Azure Active Directory registreren
2. Uw app instellen voor gebruik van de bibliotheek ADAL4J.
3. De ADAL4J bibliotheek gebruiken voor het uitgeven van aanmeldings- en afmeldingstijden aanvragen naar Azure AD.
4. Gegevens over de gebruiker afdrukken.

Ophalen is gestart, [het skelet app downloaden](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) of [het voltooide monster](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\/archive/complete.zip).  U moet ook een huurder Azure AD waarin om uw toepassing te registreren.  Als u geen sjabloon hebt, [Lees hoe u een](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>1. een toepassing met Azure Active Directory registreren
Als u wilt dat uw app om gebruikers te verifiëren, moet u eerst registreren van een nieuwe toepassing in uw huurder.

- Aanmelden bij de Azure Management Portal.
- Klik in de nav links op **Active Directory**.
- Selecteer de huurder waar u wilt dat om de toepassing te registreren.
- Klik op het tabblad **toepassingen** en klik op toevoegen in de onderste lade.
- Volg de aanwijzingen op het scherm en maak een nieuwe **webtoepassing en/of WebAPI**.
    - De **naam** van de toepassing wordt de toepassing voor eindgebruikers beschrijven
    - De **URL-aanmelding** is de basis-URL van uw app.  Het skelet van de standaardwaarde is `http://localhost:8080/adal4jsample/`.
    - De **URI voor App-ID** is een unieke id voor uw toepassing.  Het Verdrag is met `https://<tenant-domain>/<app-name>`, bv.`http://localhost:8080/adal4jsample/`
- Als u de registratie hebt voltooid, zal AAD toewijzen uw app een unieke client-id.  Moet u deze waarde in de volgende secties, dus kopiëren vanaf het tabblad configureren.

Eenmaal in de portal voor uw app een **Geheim toepassing** voor uw toepassing maken en kopieer deze naar beneden.  U moet het kort.


## <a name="2-set-up-your-app-to-use-adal4j-library-and-prerequisites-using-maven"></a>2. instellen uw app voor ADAL4J-bibliotheek en de voorwaarden voor Maven
We zullen hier ADAL4J voor het gebruik van het verificatieprotocol OpenID verbinding configureren.  ADAL4J wordt gebruikt voor aanmeldings- en afmeldingstijden aanvragen en informatie over de gebruiker, onder andere gebruikerssessies beheren.

-   In de hoofdmap van het project openen/maken `pom.xml` en zoek de `// TODO: provide dependencies for Maven` en vervangen door het volgende:

```Java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>public-client-adal4j-sample</artifactId>
    <packaging>jar</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>public-client-adal4j-sample</name>
    <url>http://maven.apache.org</url>
    <properties>
        <spring.version>3.0.5.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>adal4j</artifactId>
            <version>1.1.2</version>
        </dependency>
        <dependency>
            <groupId>com.nimbusds</groupId>
            <artifactId>oauth2-oidc-sdk</artifactId>
            <version>4.5</version>
        </dependency>
        <dependency>
            <groupId>org.json</groupId>
            <artifactId>json</artifactId>
            <version>20090211</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.5</version>
        </dependency>
</dependencies>
    <build>
        <finalName>public-client-adal4j-sample</finalName>
        <plugins>
                <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>1.2.1</version>
            <configuration>
                <mainClass>PublicClient</mainClass>
            </configuration>
        </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>install</id>
                        <phase>install</phase>
                        <goals>
                            <goal>sources</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.5</version>
                <configuration>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
        </configuration>
      </plugin>
      <plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-assembly-plugin</artifactId>
  <configuration>
    <archive>
      <manifest>
        <mainClass>PublicClient</mainClass>
      </manifest>
    </archive>
  </configuration>
</plugin>
        </plugins>
    </build>

</project>


```



## <a name="3-create-the-java-publicclient-file"></a>3. het java-PublicClient-bestand maken

Zoals hierboven aangegeven, we gebruiken de Graph API om gegevens over de gebruiker is aangemeld. Dit is gemakkelijk voor ons moet maken we zowel een voor een **Directory-Object** als een afzonderlijk bestand voor de **gebruiker** , zodat het patroon OO van Java kan worden gebruikt.

1. Maak een bestand met de naam `DirectoryObject.java` die we gebruiken voor het opslaan van gegevens over een DirectoryObject (u kunt gerust dit om later te gebruiken voor andere grafiek query's kunt u doen). U kunt knippen en plakken dit uit het volgende:

```Java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import javax.naming.ServiceUnavailableException;

import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;

public class PublicClient {

    private final static String AUTHORITY = "https://login.microsoftonline.com/common/";
    private final static String CLIENT_ID = "2a4da06c-ff07-410d-af8a-542a512f5092";

    public static void main(String args[]) throws Exception {

        try (BufferedReader br = new BufferedReader(new InputStreamReader(
                System.in))) {
            System.out.print("Enter username: ");
            String username = br.readLine();
            System.out.print("Enter password: ");
            String password = br.readLine();

            AuthenticationResult result = getAccessTokenFromUserCredentials(
                    username, password);
            System.out.println("Access Token - " + result.getAccessToken());
            System.out.println("Refresh Token - " + result.getRefreshToken());
            System.out.println("ID Token - " + result.getIdToken());
        }
    }

    private static AuthenticationResult getAccessTokenFromUserCredentials(
            String username, String password) throws Exception {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(AUTHORITY, false, service);
            Future<AuthenticationResult> future = context.acquireToken(
                    "https://graph.windows.net", CLIENT_ID, username, password,
                    null);
            result = future.get();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }
}


```


##<a name="compile-and-run-the-sample"></a>Compileren en uitvoeren van het monster

Weer uit naar de hoofdmap en de volgende opdracht uit te maken van het monster u plaatsen samen met `maven`. Dit wordt gebruikt de `pom.xml` bestand dat u hebt geschreven voor afhankelijkheden.

`$ mvn package`

U hebt nu een `adal4jsample.war` bestand in uw `/targets` directory. U kunt implementeren die in uw container Tomcat en Ga naar de URL 

`http://localhost:8080/adal4jsample/`


> [AZURE.NOTE] 
Het is heel eenvoudig te implementeren, een OORLOG met de meest recente Tomcat servers. Eenvoudig navigeren naar `http://localhost:8080/manager/` en volg de instructies over het uploaden van uw '' adal4jsample.war' bestand. Het autodeploy wordt u met het juiste eindpunt.

##<a name="next-steps"></a>Volgende stappen

Gefeliciteerd! U hebt nu een werkende Java-toepassing die de mogelijkheid heeft om gebruikers te verifiëren, veilig gebruik OAuth 2.0 Web-API's aanroepen en elementaire informatie over de gebruiker.  Als u nog niet gedaan hebt, is nu de tijd voor het vullen van de huurder met sommige gebruikers.

Voor een verwijzing naar kunt het ingevulde voorbeeld (zonder de waarden van de systeemconfiguratie) [wordt geleverd als een .zip hier](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip)of u klonen het van GitHub:

```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```

