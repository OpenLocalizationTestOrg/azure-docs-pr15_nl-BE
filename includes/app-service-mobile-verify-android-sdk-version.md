Vanwege de voortdurende ontwikkeling, kan de versie Android SDK in Android Studio niet overeen met de versie in de code. De Android SDK waarnaar wordt verwezen in deze zelfstudie is versie 23, uiterlijk op het moment van schrijven. Het versienummer toenemen naarmate er nieuwe versies van de SDK worden weergegeven, en het is raadzaam de meest recente versie beschikbaar.

Er zijn twee symptomen van niet-overeenkomende versie:

1. Bij het bouwen of het project opnieuw kunnen er foutberichten worden weergegeven als '**is niet gevonden, doel, Google Inc.:Google APIs:n**' Gradle.

2. Standaard Android-objecten in de code die moet worden opgelost op basis van `import` overzichten mogelijk foutberichten genereren.

Als een van beide worden weergegeven, de versie van de Android SDK in Android Studio is geïnstalleerd mogelijk niet overeen met het doel van de SDK van het gedownloade project.  Als u wilt controleren of de versie, de volgende wijzigingen aanbrengen:


1. Klik op **Extra**in Android Studio => **Android** => **Manager SDK**. Als u de meest recente versie van het Platform SDK niet hebt geïnstalleerd, klikt u om het te installeren. Noteer het versienummer.

2. Open het bestand in het tabblad Project Explorer onder **Scripts Gradle**, **build.gradle (modeule: app)**. Zorg ervoor dat de **compileSdkVersion** en **buildToolsVersion** zijn ingesteld op de meest recente versie SDK is geïnstalleerd. De tags uitzien als volgt:
 
            compileSdkVersion 'Google Inc.:Google APIs:23'
            buildToolsVersion "23.0.2"
    
3. In Android Studio Project Explorer met de rechtermuisknop op het projectknooppunt, kiest u **Eigenschappen**en kies **Android**in de linkerkolom. Zorg ervoor dat het **Project bouwen doel** is ingesteld op dezelfde versie als de **targetSdkVersion**SDK.

4. In Android Studio wordt het manifestbestand niet langer gebruikt om aan te geven het doel SDK en de minimale versie van SDK, in tegenstelling tot het geval met Eclips.
