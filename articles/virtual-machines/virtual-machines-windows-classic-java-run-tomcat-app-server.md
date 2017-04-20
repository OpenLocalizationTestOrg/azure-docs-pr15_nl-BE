<properties
    pageTitle="Tomcat op een virtuele machine | Microsoft Azure"
    description="In deze zelfstudie worden gemaakt met het klassieke implementatiemodel bronnen gebruikt en ziet u hoe u een virtuele Windows-machine maken en configureren voor het uitvoeren van Apache Tomcat application server."
    services="virtual-machines-windows"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.workload="web"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-run-a-java-application-server-on-a-virtual-machine-created-with-the-classic-deployment-model"></a>Het uitvoeren van een Java-toepassingsserver op een virtuele machine gemaakt met het implementatiemodel klassiek

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Met Azure, kunt u een virtuele machine om server mogelijk te maken. Als voorbeeld, kan een virtuele machine op Azure worden geconfigureerd als host voor een Java-toepassingsserver zoals Apache Tomcat. Nadat u deze handleiding hebt voltooid, hebt u een goed begrip van een virtuele machine op Azure maken en configureren voor het uitvoeren van een Java-toepassingsserver.

U leert:

* Het maken van een virtuele machine met een Java Development Kit (JDK) al is geïnstalleerd.
* Informatie over het extern aan te melden bij uw virtuele machine.
* Het Java-toepassingsserver installeren op uw virtuele machine.
* Het maken van een eindpunt voor uw virtuele machine.
* Hoe u een poort openen in de firewall voor de application server.

Voor de toepassing van deze zelfstudie, wordt een Apache Tomcat application server geïnstalleerd op een virtuele machine. De voltooide installatie zal leiden tot een Tomcat installatie als volgt.

![Virtuele machine met Apache Tomcat][virtual_machine_tomcat]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Een virtuele machine maken

1. Log in om de [Azure klassieke portal](https://manage.windowsazure.com).
2. Klik op **Nieuw**, klikt u op **berekenen**, klikt u op de **virtuele machine**en klik op **Uit galerie**.
3. Selecteer in het dialoogvenster **virtuele machine afbeelding selecteren** **JDK 7 Windows Server 2012**.
Houd er rekening mee dat **JDK 6 Windows Server 2012** beschikbaar is als u verouderde toepassingen die niet kan worden uitgevoerd in JDK 7.
4. Klik op **volgende**.
5. Klik in het dialoogvenster **configuratie van de virtuele machine** :
    1. Geef een naam voor de virtuele machine.
    2. Geef de grootte moet worden gebruikt voor de virtuele machine.
    3. Voer een naam voor de beheerder in het veld **Gebruikersnaam** . Onthoud deze naam en het wachtwoord u vervolgens voert, u gebruikt deze wanneer u extern aanmelden bij de virtuele machine.
    4. Voer in het veld **Nieuw wachtwoord** een wachtwoord en voer deze opnieuw in het veld **bevestigen** . Dit is het wachtwoord voor de beheerdersaccount.
    5. Klik op **volgende**.
6. De volgende virtuele machine in het dialoogvenster **configuratie** :
    1. Voor **Cloud-service**, gebruikt de standaard **een nieuwe wolk service maken**.
    2. De waarde voor **Cloud service DNS-naam** moet uniek zijn in cloudapp.net. Indien nodig, deze waarde wijzigen zodat die Azure geeft aan dat deze uniek is.
    2. Geef een regio, affiniteit groep of virtueel netwerk. Geef een regio zoals **West VS**voor de toepassing van deze zelfstudie.
    2. Voor **Rekening van de opslag**, selecteert u **een automatisch gegenereerde opslag-account gebruiken**.
    3. Voor **Beschikbaarheid instellen**, selecteert u **(geen)**.
    4. Klik op **volgende**.
7. In het uiteindelijke dialoogvenster **virtuele machine configuratie** :
    1. Accepteer de standaardinstellingen voor het eindpunt.
    2. Klik op **Voltooien**.

## <a name="to-remotely-sign-in-to-your-virtual-machine"></a>Op afstand aan te melden bij uw virtuele machine

1. Meld u op de [Azure klassieke portal](https://manage.windowsazure.com).
2. Klik op **virtuele machines**.
3. Klik op de naam van de virtuele machine die u wilt aanmelden.
4. Nadat de virtuele machine is gestart, kan een pop-upmenu onder aan de pagina verbindingen.
5. Klik op **verbinden**.
6. Reageren op de vragen zo nodig verbinding met de virtuele machine. Dit moet leiden tot de details van de verbinding met RDP-bestand openen of opslaan. Mogelijk moet u de url: poort als het laatste deel van de eerste regel van het RDP-bestand kopiëren en plakken in een externe toepassing aanmelden.

## <a name="to-install-a-java-application-server-on-your-virtual-machine"></a>Een Java-toepassingsserver installeren op uw virtuele machine

U kunt een Java-toepassingsserver kopiëren naar uw virtuele machine of u via een installatieprogramma Java-toepassingsserver kunt installeren.

Voor de toepassing van deze zelfstudie, wordt Tomcat geïnstalleerd.

1. Wanneer u bent aangemeld met uw virtuele machine, opent u een browsersessie met [Apache Tomcat](http://tomcat.apache.org/download-70.cgi).
2. Dubbelklik op de koppeling voor **32-bits/64-bits Windows Service Installer**. Met deze techniek, wordt Tomcat geïnstalleerd als een Windows-service.
3. Wanneer dat wordt gevraagd, kiest u het installatieprogramma uitvoert.
4. Volg de aanwijzingen om installeren Tomcat vanuit de wizard **Setup van Apache Tomcat** . Voor de toepassing van deze zelfstudie, de standaardwaarden accepteren is geen probleem. Wanneer u het dialoogvenster voor het **voltooien van de Wizard Setup van Apache Tomcat** bereikt, kunt u desgewenst controleren **Apache Tomcat uitvoeren** om Tomcat nu starten. Klik op **Voltooien** om het setup-proces Tomcat.

## <a name="to-start-tomcat"></a>Tomcat starten
Als u ervoor hebt gekozen niet Tomcat uitvoeren in het dialoogvenster van **de wizard Apache Tomcat Setup** , start u door een opdrachtprompt op de virtuele machine te openen en **net start Tomcat7**.

U ziet nu Tomcat uitgevoerd als u de virtuele machine browser en <http://localhost:8080 Open>.

Overzicht Tomcat vanaf externe computers worden uitgevoerd, moet u een eindpunt maken en een poort openen.

## <a name="to-create-an-endpoint-for-your-virtual-machine"></a>Een eindpunt voor uw virtuele machine maken
1. Log in om de [Azure klassieke portal](https://manage.windowsazure.com).
2. Klik op **virtuele machines**.
3. Klik op de naam van de virtuele machine die Java-toepassingsserver wordt uitgevoerd.
4. Klik op de **eindpunten**.
5. Klik op **toevoegen**.
6. In het dialoogvenster **Add eindpunt** zorgen **toevoegen zelfstandige eindpunt** is geselecteerd en klik op **volgende**.
7. Klik in het dialoogvenster **nieuwe eindpuntdetails** :
    1. Geef een naam op voor het eindpunt; bijvoorbeeld **HttpIn**.
    2. Geef voor het protocol **TCP** op.
    3. **80** voor de openbare poort opgeven.
    4. **8080** voor de particuliere poort opgeven.
    5. Klik op de knop **Voltooien** om het dialoogvenster te sluiten. Het eindpunt wordt gemaakt.

## <a name="to-open-a-port-in-the-firewall-for-your-virtual-machine"></a>Een poort openen in de firewall voor uw virtuele machine
1. Log in om uw virtuele machine.
2. Klik op de **Start van Windows**.
3. Klik op **Configuratiescherm**.
4. Klik op **systeem en beveiliging**, klik op **Windows Firewall**en klik vervolgens op **Geavanceerde instellingen**.
5. Klik op **Binnenkomende regels**en klik vervolgens op **Nieuwe regel**.
 ![Nieuwe binnenkomende regel][NewIBRule]
6. Voor het **Regeltype** **poort**selecteren en klik vervolgens op **volgende**.
 ![Nieuwe poort voor binnenkomende regel][NewRulePort]
7. Op het scherm **protocollen en poorten** selecteert u **TCP**, **8080** opgeven als de **specifieke lokale poort**en klik vervolgens op **volgende**.
 ![Nieuwe binnenkomende regel][NewRuleProtocol]
8. Selecteer **de verbinding toestaan**op het scherm **actie** en klik op **volgende**.
 ![Nieuwe binnenkomende regelactie][NewRuleAction]
9. Controleer op het scherm **profiel** **domein**, **Private**en **Public** zijn geselecteerd en klik op **volgende**.
 ![Nieuwe binnenkomende regel profiel][NewRuleProfile]
10. Geef een naam voor de regel, bijvoorbeeld **HttpIn** (naam van de regel is niet verplicht maar komen overeen met de naam van het eindpunt), op het scherm **de naam** en klik op **Voltooien**.  
 ![Nieuwe naam voor de binnenkomende regel][NewRuleName]

Op dit moment uw website Tomcat moet worden weergegeven op een externe browser met behulp van een URL van het formulier * *http://*uw\_DNS\_naam*. cloudapp.net**waarbij ** *uw\_DNS\_naam*** de DNS-naam die u hebt opgegeven tijdens het maken van de virtuele machine.

## <a name="application-lifecycle-considerations"></a>Application lifecycle overwegingen
* U kan uw eigen toepassing webarchief (WAR) maken en toevoegen aan de map **webapps** . Bijvoorbeeld een eenvoudige Java Service pagina (JSP) dynamische webproject maken en als een WAR-bestand exporteren, de OORLOG kopiëren naar de map Apache Tomcat **webapps** op de virtuele machine, vervolgens uit te voeren in een browser.
* Standaard wanneer de Tomcat-service is geïnstalleerd, is deze ingesteld op handmatig starten. U kunt overschakelen met behulp van de module Services automatisch wordt gestart. De module Services starten door te klikken op **Windows Start**, **Systeembeheer**en vervolgens op **Services**. Dubbelklik op de **Apache Tomcat** -service en **het opstarttype** instellen op **automatisch**.

    ![Als u een service automatisch wordt gestart][service_automatic_startup]

    Het voordeel van Tomcat start automatisch met is dat wordt gestart als de virtuele machine opnieuw wordt opgestart (bijvoorbeeld nadat de software-updates opnieuw worden geïnstalleerd).

## <a name="next-steps"></a>Volgende stappen
Meer informatie over andere services (zoals opslag Azure service bus en SQL-Database), kunt u op te nemen met uw Java-toepassingen door de beschikbare informatie op de [Java Developer Center](https://azure.microsoft.com/develop/java/)bekijken.

[virtual_machine_tomcat]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/WA_VirtualMachineRunningApacheTomcat.png

[service_automatic_startup]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/WA_TomcatServiceAutomaticStart.png









[NewIBRule]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewInboundRule.png
[NewRulePort]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRulePort.png
[NewRuleProtocol]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleProtocol.png
[NewRuleAction]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleAction.png
[NewRuleName]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleName.png
[NewRuleProfile]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleProfile.png
