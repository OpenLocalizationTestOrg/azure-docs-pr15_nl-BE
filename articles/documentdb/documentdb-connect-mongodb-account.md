<properties 
    pageTitle="Verbinding maken met een account DocumentDB met ondersteuning voor MongoDB | Microsoft Azure" 
    description="Informatie over hoe u verbinding met een account met DocumentDB met ondersteuning voor MongoDB, nu beschikbaar voor preview-protocol. Verbinding maken met behulp van de verbindingsreeks MongoDB." 
    keywords="mongodb-verbindingsreeks"
    services="documentdb" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="anhoh"/>

# <a name="how-to-connect-to-a-documentdb-account-with-protocol-support-for-mongodb"></a>Verbinding maken met een account DocumentDB met ondersteuning voor MongoDB

Informatie over verbinding maken met een account Azure DocumentDB met ondersteuning voor MongoDB met behulp van standaard MongoDB URI indeling van de verbindingsreeks.  

## <a name="get-the-accounts-connection-string-information"></a>Verbindingsinformatie van de account ophalen

1. Log in op de [Azure Portal](https://portal.azure.com)in een nieuw venster.
2. Klik op de **Verbindingsreeks**in de balk **Navigatie links** van de Blade-Account. Om te navigeren naar de **Account Blade**, op de Jumpbar klikt u op **Meer Services**op **DocumentDB (NoSQL)** en selecteert u de account DocumentDB met ondersteuning voor MongoDB-protocol.

    ![Schermafdruk van het blad van alle instellingen](./media/documentdb-connect-mongodb-account/SettingsBlade.png)

3. De **Verbindingsinformatie** blade wordt geopend en alle informatie verbinding maken met de account met behulp van een stuurprogramma voor MongoDB, inclusief de verbindingsreeks van een vooraf gebouwd is.

    ![Schermafdruk van de connection string blade](./media/documentdb-connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a>Vereisten voor verbinding string

Het is belangrijk te weten dat DocumentDB standaard MongoDB URI indeling van de verbindingsreeks, met een aantal specifieke vereisten ondersteunt: DocumentDB accounts vereisen verificatie en veilige communicatie via SSL.  Indeling van de verbindingsreeks is dus:

    mongodb://username:password@host:port/[database]?ssl=true

Waarin de waarden van deze tekenreeks in de verbindingsreeks blade hierboven beschikbaar zijn.

- Gebruikersnaam (vereist)
    - De naam DocumentDB
- Wachtwoord (vereist)
    - Wachtwoord DocumentDB
- Host (vereist)
    - FQDN-naam van de DocumentDB-account
- Port (vereist)
    - 10250
- Database (optioneel)
    - De standaarddatabase gebruikt door de verbinding
- SSL = true (vereist)

Stel de account in de bovenstaande verbindingsinformatie weergegeven.  Er is een geldige verbindingsreeks:
    
    mongodb://contoso123:<password@contoso123.documents.azure.com:10250/mydatabase?ssl=true

## <a name="connecting-with-the-c-driver-for-mongodb"></a>Verbinding maken met het C#-stuurprogramma voor MongoDB
Zoals reeds vermeld vereisen alle DocumentDB accounts verificatie en veilige communicatie via SSL. Terwijl MongoDB URI indeling van de verbindingsreeks een ssl ondersteunt = true queryreeksparameter, werken met de MongoDB C#-stuurprogramma vereist het gebruik van het object MongoClientSettings bij het maken van een MongoClient.  Gezien de bovenstaande gegevens, het volgende stukje code laat zien hoe verbinding maken met de account en het werken met de database 'Taken'.

            MongoClientSettings settings = new MongoClientSettings();
            settings.Server = new MongoServerAddress("contoso123.documents.azure.com", 10250);
            settings.UseSsl = true;
            settings.SslSettings = new SslSettings();
            settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;

            MongoIdentity identity = new MongoInternalIdentity("Tasks", "contoso123");
            MongoIdentityEvidence evidence = new PasswordEvidence("<password>");

            settings.Credentials = new List<MongoCredential>()
            {
                new MongoCredential("SCRAM-SHA-1", identity, evidence)
            };
            MongoClient client = new MongoClient(settings);
            var database = client.GetDatabase("Tasks",);
    

## <a name="next-steps"></a>Volgende stappen


- Meer informatie over hoe te [gebruiken MongoChef](documentdb-mongodb-mongochef.md) met een DocumentDB-account met protocol ondersteunen voor MongoDB.
- DocumentDB met ondersteuning voor MongoDB [monsters](documentdb-mongodb-samples.md)verkennen.

 
