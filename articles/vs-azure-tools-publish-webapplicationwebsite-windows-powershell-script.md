<properties
   pageTitle="Publiceren WebApplicationWebSite (Windows PowerShell-script) | Microsoft Azure"
   description="Informatie over het publiceren van een webproject op een Azure website. Dit script maakt de vereiste bronnen in uw abonnement Azure als deze nog niet bestaan."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="publish-webapplicationwebsite-windows-powershell-script"></a>Publiceren WebApplicationWebSite (Windows PowerShell-script)

##<a name="syntax"></a>Syntaxis

Een webproject publiceert naar een Azure website. Het script maakt de vereiste bronnen in uw abonnement Azure, als deze nog niet bestaan.

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a>Configuratie

Het pad naar het configuratiebestand JSON die de details van de implementatie beschrijft.

|Parameter|Standaardwaarde|
|---|---|
|Aliassen|geen|
|Vereist?|True|
|Positie|met de naam|
|Standaardwaarde|geen|
|Invoer pijplijn accepteren?|False|
|Jokertekens accepteren?|False|

## <a name="subscriptionname"></a>SubscriptionName

De naam van het Azure abonnement dat u wilt maken van de website in.

|Parameter|Standaardwaarde|
|---|---|
|Aliassen|geen|
|Vereist?|False|
|Positie|met de naam|
|Standaardwaarde|geen|
|Invoer pijplijn accepteren?|False|
|Jokertekens accepteren?|False|

## <a name="webdeploypackage"></a>WebDeployPackage

Het pad naar het web te publiceren naar de website pakket. U kunt dit pakket maken met behulp van de wizard Publiceren in Visual Studio. Zie [aan de slag met Azure Cloud Services en ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089)voor meer informatie.

|Parameter|Standaardwaarde|
|---|---|
|Aliassen|geen|
|Vereist?|False|
|Positie|met de naam|
|Standaardwaarde|geen|
|Invoer pijplijn accepteren?|False|
|Jokertekens accepteren?|False|

## <a name="databaseserverpassword"></a>DatabaseServerPassword

De gebruikersnaam en het wachtwoord voor de SQL-database in Azure.

|Parameter|Standaardwaarde|
|---|---|
|Aliassen|geen|
|Vereist?|False|
|Positie|met de naam|
|Standaardwaarde|geen|
|Invoer pijplijn accepteren?|False|
|Jokertekens accepteren?|False|

## <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput

Als de waarde true is, berichten afdrukken vanuit het script naar de uitvoerstroom.

|Parameter|Standaardwaarde|
|---|---|
|Aliassen|geen|
|Vereist?|False|
|Positie|met de naam|
|Standaardwaarde|False|
|Invoer pijplijn accepteren?|False|
|Jokertekens accepteren?|False|

## <a name="remarks"></a>Opmerkingen

Dev- en testomgevingen, Zie voor een volledige uitleg over het gebruik van het script voor het maken van [Windows PowerShell-Scripts gebruiken om te publiceren op de Dev- en testomgevingen](vs-azure-tools-publishing-using-powershell-scripts.md).

Het configuratiebestand JSON geeft de details van wat er moet worden ingezet. Deze bevat de informatie die u hebt opgegeven tijdens het maken van het project, zoals de naam en de gebruikersnaam voor de website. Bevat ook de database naar de voorziening, indien van toepassing. De volgende code toont een voorbeeld van de JSON-configuratiebestand:

    {
        "environmentSettings": {
            "webSite": {
                "name": "WebApplication10554",
                "location": "West US"
            },
            "databases": [
                {
                    "connectionStringName": "DefaultConnection",
                    "databaseName": "WebApplication10554_db",
                    "serverName": "iss00brc88",
                    "user": "sqluser2",
                    "password": "",
                    "edition": "",
                    "size": "",
                    "collation": "",
                    "location": "West US"
                }
            ]
        }
    }

Kunt u de JSON-configuratiebestand om te wijzigen wat wordt geïmplementeerd. Een sectie van de webSite is vereist, maar de database-sectie is optioneel.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie [Publiceren-WebApplicationVM (Windows PowerShell-script)](vs-azure-tools-publish-webapplicationvm.md)
