<properties 
    pageTitle="Een certificaat toevoegen aan de Java-CA-archief | Microsoft Azure" 
    description="Informatie over het toevoegen van een certificate authority (CA) certificaat naar de winkel Java CA-certificaat (cacerts) voor Twilio service of Azure Service Bus." 
    services="" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="adding-a-certificate-to-the-java-ca-certificates-store"></a>Een certificaat toevoegen aan het archief met Java CA certificaten
De volgende stappen laten zien hoe een certificate authority (CA) certificaat toevoegen aan de Java-CA-certificaatarchief (cacerts). In het voorbeeld gebruikt wordt voor het CA-certificaat vereist door de service Twilio. Informatie later in het onderwerp wordt beschreven hoe het CA-certificaat installeren voor de Azure Service Bus. 

U kunt hulpprogramma Keytool gebruikt het CA-certificaat voor het comprimeren van de JDK en deze toe te voegen aan het project Azure **approot** map toevoegen of een taak Azure opstarten met hulpprogramma Keytool gebruikt om toe te voegen van het certificaat kan worden uitgevoerd. In dit voorbeeld wordt ervan uitgegaan dat u een CA-certificaat voor de JDK wordt ingepakt wilt toevoegen. Ook een specifieke CA-certificaat wordt gebruikt in het voorbeeld, maar de stappen voor het verkrijgen van een andere CA-certificaat en vervolgens te importeren in het archief van cacerts zou vergelijkbaar zijn.

## <a name="to-add-a-certificate-to-the-cacerts-store"></a>Een certificaat toevoegen aan het archief met cacerts

1. Voer de volgende om te zien welke certificaten zijn geïnstalleerd bij een opdrachtprompt die is ingesteld op een van de JDK **jdk\jre\lib\security** map:

    `keytool -list -keystore cacerts`

    Wordt u gevraagd om het wachtwoord van de winkel. Het standaardwachtwoord is **changeit**. (Als u wilt dat het wachtwoord te wijzigen, Zie de documentatie bij het hulpprogramma Keytool gebruikt op <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.) In dit voorbeeld wordt ervan uitgegaan dat het certificaat met MD5-vingerafdruk 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 niet wordt vermeld, en die u wilt importeren (dit certificaat wordt door de service de API Twilio nodig).
2. Het certificaat uit de lijst met [Basiscertificaten GeoTrust](http://www.geotrust.com/resources/root-certificates/)vermelde certificaten verkrijgen. Klik met de rechtermuisknop op de koppeling voor het certificaat met serienummer 35:DE:F4:CF en opslaan in de map **jdk\jre\lib\security** . Ten behoeve van dit voorbeeld, is opgeslagen in een bestand met de naam **Equifax\_Secure\_certificaat\_Authority.cer**.
3. Importeer het certificaat via de volgende opdracht:

    `keytool -keystore cacerts -importcert -alias equifaxsecureca -file Equifax_Secure_Certificate_Authority.cer`

    Wanneer u wordt gevraagd dit certificaat vertrouwen als het certificaat MD5-vingerafdruk 67:CB:9 D is: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4, reageren door **y**te typen.
4. Voer de volgende opdracht om dat het CA-certificaat is geïmporteerd:

    `keytool -list -keystore cacerts`

5. De JDK ZIP en toe te voegen aan het project Azure **approot** map.

Zie <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>voor informatie over het hulpprogramma Keytool gebruikt.

## <a name="azure-root-certificates"></a>Azure basiscertificaten

De toepassingen die gebruikmaken van Azure services (zoals Azure Service Bus) moeten de Baltimore CyberTrust basiscertificaat vertrouwt. (Vanaf 15 April 2013 Azure begon met de migratie van GTE CyberTrust globale Root naar de hoofdmap van de CyberTrust Baltimore. Deze migratie heeft enkele maanden te voltooien.)

Het certificaat mogelijk al geïnstalleerd in uw winkel cacerts Baltimore zo denken de **hulpprogramma Keytool gebruikt-lijst** de opdracht eerst om te zien of deze al bestaat.

Als u toevoegen van de Baltimore CyberTrust Root wilt, heeft het 02:00:00:b9 serienummer en SHA1-vingerafdruk d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2 c: 78:db:28:52:ca:e4:74. Het kan worden gedownload van <https://cacert.omniroot.com/bc2025.crt>, opgeslagen in een lokaal bestand met de extensie **.cer**en vervolgens geïmporteerd met behulp van **hulpprogramma Keytool gebruikt** , zoals hierboven is weergegeven.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de basiscertificaten gebruikt door Azure [Azure Root certificaat migratie](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx).

Zie voor meer informatie over Java, [Java Developer Center](/develop/java/).
