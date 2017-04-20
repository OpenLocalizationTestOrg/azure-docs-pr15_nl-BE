<properties
    pageTitle="MongoDB installeren op een Windows VM | Microsoft Azure"
    description="Informatie over het MongoDB op een Azure VM met Windows Server 2012 R2 is gemaakt met het implementatiemodel Resource Manager te installeren."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="iainfou"/>

# <a name="install-and-configure-mongodb-on-a-windows-vm-in-azure"></a>Installeren en configureren van MongoDB op een Windows VM in Azure
[MongoDB](http://www.mongodb.org) is een populaire open-source, krachtige NoSQL database. Dit artikel helpt u bij het installeren en configureren van MongoDB op een Windows Server 2012 R2 virtuele machine (VM) in Azure. U kunt ook [MongoDB op een Linux VM in Azure installeren](virtual-machines-linux-install-mongodb.md).


## <a name="prerequisites"></a>Vereisten

Voordat u installeren en configureren van MongoDB, moet u een VM maken en in het ideale geval een schijf met gegevens aan toe te voegen. Raadpleeg de volgende artikelen een VM maken en toevoegen van een schijf met gegevens:

- [Een Windows Server-VM maken met behulp van de Azure portal](virtual-machines-windows-hero-tutorial.md) of [een Windows Server VM met Azure PowerShell maken](virtual-machines-windows-ps-create.md)
- [Toevoegen van een schijf met gegevens naar een Windows Server-VM met de Azure portal](virtual-machines-windows-attach-disk-portal.md) of [toevoegen van een schijf met gegevens naar een Windows Server-VM Azure PowerShell](https://msdn.microsoft.com/library/mt603673.aspx)
    
Om te beginnen met het installeren en configureren van MongoDB, [Meld u aan bij uw Windows Server VM](virtual-machines-windows-connect-logon.md) met behulp van extern bureaublad.


## <a name="install-mongodb"></a>MongoDB installeren

> [AZURE.IMPORTANT] MongoDB-beveiligingsfuncties, zoals verificatie en de binding tussen IP-adres, zijn standaard niet ingeschakeld. Voorzieningen voor beveiliging moeten zijn ingeschakeld voordat u MongoDB implementeert in een productieomgeving. Zie [MongoDB beveiliging en verificatie](http://www.mongodb.org/display/DOCS/Security+and+Authentication)voor meer informatie.

1. Nadat u verbinding hebt gemaakt voor uw VM met extern bureaublad, open Internet Explorer vanuit het menu **Start** op de VM.

2. **Gebruik aanbevolen beveiliging, privacy en compatibiliteitsinstellingen** selecteren wanneer Internet Explorer voor het eerst wordt geopend en klik op **OK**.

3. Configuratie van verbeterde beveiliging van Internet Explorer is standaard ingeschakeld. De website MongoDB toevoegen aan de lijst met toegestane websites:

    - Selecteer het pictogram **hulpprogramma's** in de rechterbovenhoek.
    - Selecteer het tabblad **beveiliging** in **Internet-opties**en selecteer vervolgens het pictogram **Vertrouwde websites** .
    - Klik op de knop **websites** . Toevoegen _https://\*. mongodb.org_ aan de lijst met vertrouwde sites en sluit het dialoogvenster.

    ![Beveiligingsinstellingen voor Internet Explorer configureren](./media/virtual-machines-windows-install-mongodb/configure-internet-explorer-security.png)

4. Ga naar de pagina [MongoDB - Downloads](http://www.mongodb.org/downloads) (http://www.mongodb.org/downloads).

5. Standaard moet de **Community Server** -editie en de nieuwste huidige stabiele release voor Windows Server 2008 R2, 64-bits en hoger selecteert. Om het installatieprogramma te downloaden, klikt u op **DOWNLOAD (msi)**.

    ![MongoDB-installer downloaden](./media/virtual-machines-windows-install-mongodb/download-mongodb.png)

    Nadat het downloaden voltooid is, voert u het installatieprogramma.

6. Lees en accepteer de gebruiksrechtovereenkomst. Wanneer u wordt gevraagd, selecteert u de installatie **voltooid** .

7. Klik op het laatste scherm op **installeren**.


## <a name="configure-the-vm-and-mongodb"></a>Configureer de VM en MongoDB

1. De path-variabelen worden niet bijgewerkt door het installatieprogramma van MongoDB. Zonder de MongoDB `bin` locatie in de padvariabele, moet u het volledige pad opgeven telkens wanneer u een uitvoerbaar bestand van MongoDB. De locatie aan de padvariabele toevoegen:

    - Klik met de rechtermuisknop op het menu **Start** en selecteer **systeem**.
    - Klik op **Geavanceerde instellingen**en klik vervolgens op **Omgevingsvariabelen**.
    - Onder **Systeemvariabelen** **pad**selecteren en klik vervolgens op **bewerken**.

    ![Configureer de PATH-variabelen](./media/virtual-machines-windows-install-mongodb/configure-path-variables.png)

    Het pad toevoegen aan uw MongoDB `bin` map. MongoDB wordt meestal geïnstalleerd in `C:\Program Files\MongoDB`. Controleer of het installatiepad van de VM. Het volgende voorbeeld wordt de standaard MongoDB locatie te installeren de `PATH` variabele:

    ```
    ;C:\Program Files\MongoDB\Server\3.2\bin
    ```

    > [AZURE.NOTE] Zorg ervoor dat het begin van de regel toevoegen (`;`) om aan te geven dat u wilt toevoegen aan een locatie voor uw `PATH` variabele.

2. MongoDB-gegevens en logboekbestanden mappen maken op de gegevensschijf. Selecteer in het menu **Start** **MS-DOS-Prompt**. De volgende voorbeelden maken de mappen op station F:

    ```
    mkdir F:\MongoData
    mkdir F:\MongoLogs
    ```

3. MongoDB sessie gestart met de volgende opdracht, het pad naar uw gegevens aan te passen en dienovereenkomstig mappen melden:

    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log
    ```

    Het kan enkele minuten duren voordat MongoDB journal-bestanden toewijzen en beginnen met luisteren op voor verbindingen. Alle berichten in het logboek worden doorgestuurd naar het bestand *F:\MongoLogs\mongolog.log* als `mongod.exe` server start en wijst u journal-bestanden.

    > [AZURE.NOTE] De MS-DOS-prompt blijft gericht op deze taak, terwijl de MongoDB-exemplaar wordt uitgevoerd. Sluit het opdrachtpromptvenster te gaan met MongoDB. Of MongoDB installeren als service, zoals wordt beschreven in de volgende stap.

4. Installeren voor een stabielere ervaring MongoDB, de `mongod.exe` als een service. Maken van een service betekent dat u niet hoeft te laten vanaf de opdrachtprompt uitgevoerd elke keer dat u wilt gebruiken, MongoDB. De service als volgt maken, het pad naar de mappen van uw gegevens- en logboekbestanden dienovereenkomstig aan te passen:

    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log `
        --logappend  --install
    ```

    De voorgaande opdracht maakt u een service met de naam MongoDB, met een beschrijving van de 'Mongo DB'. De volgende parameters worden ook opgegeven:

    - De `--dbpath` optie geeft u de locatie van de map data.
    - De `--logpath` optie een logboekbestand opgeven dat moet worden gebruikt omdat de actieve service geen venster met een opdrachtprompt weer te geven van de uitvoer.
    - De `--logappend` optie geeft aan dat het opnieuw opstarten van de service zorgt ervoor dat uitvoer toe te voegen aan het bestaande logboekbestand.

  De MongoDB als service wilt starten, moet u de volgende opdracht uitvoeren:

    ```
    net start MongoDB
    ```

    Zie [een Windows-Service voor MongoDB configureren](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service)voor meer informatie over het maken van de service MongoDB.

## <a name="test-the-mongodb-instance"></a>De instantie MongoDB testen

MongoDB wordt uitgevoerd als een enkel exemplaar of als service is geïnstalleerd, kunt u het maken en gebruiken van uw databases nu starten. Om te beginnen de administratieve MongoDB-shell, een ander opdrachtpromptvenster openen vanuit het menu **Start** en voer de volgende opdracht:

```
mongo  
```

Kunt u de databases met naam van de `db` opdracht. Sommige gegevens bijvoegen:

```
db.foo.insert( { a : 1 } )
```

Zoeken naar gegevens als volgt:

```
db.foo.find()
```

De uitvoer is vergelijkbaar met het volgende voorbeeld:

```
{ "_id" : "ObjectId("57f6a86cee873a6232d74842"), "a" : 1 }
```

Exit de `mongo` console als volgt:

```
exit
```

## <a name="configure-firewall-and-network-security-group-rules"></a>Firewall en netwerk-beveiligingsgroep regels configureren
MongoDB is geïnstalleerd en wordt uitgevoerd, moet u een poort in Windows Firewall openen zodat u op afstand verbinding met MongoDB maken kunt. U maakt een nieuwe binnenkomende regel voor het toestaan van TCP-poort 27017, open een administratieve PowerShell-prompt en voer de volgende opdracht:

```powerShell
New-NetFirewallRule -DisplayName "Allow MongoDB" -Direction Inbound `
    -Protocol TCP -LocalPort 27017 -Action Allow
```

U kunt ook de regel maken met behulp van de grafische beheerprogramma van **Windows Firewall met geavanceerde beveiliging** . Maak een nieuwe binnenkomende regel voor het toestaan van TCP-poort 27017.

Als dat nodig is, maakt u een regel beveiligingsgroep netwerk toegang tot MongoDB van buiten het subnet met bestaande Azure virtueel netwerk. Met behulp van de [portal Azure](virtual-machines-windows-nsg-quickstart-portal.md) of [Azure PowerShell](virtual-machines-windows-nsg-quickstart-powershell.md)kunt u de regels van de groep netwerk. Net als bij de Windows Firewall-regels, kunnen TCP-poort 27017 aan de virtuele netwerkinterface van uw VM MongoDB.

> [AZURE.NOTE] TCP-poort 27017 is de standaardpoort gebruikt door MongoDB. U kunt deze poort wijzigen met behulp van de `--port` parameter bij het starten van `mongod.exe` handmatig of via een service. Als u de poort wijzigt, moet u voor het bijwerken van de Windows Firewall en netwerk-beveiligingsgroep regels in de voorgaande stappen.


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe installeren en configureren van MongoDB op uw Windows-VM. U hebt nu toegang tot MongoDB op uw Windows-VM, door de volgende geavanceerde onderwerpen in de [documentatie van MongoDB](https://docs.mongodb.com/manual/).
