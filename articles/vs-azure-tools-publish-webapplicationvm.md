<properties
   pageTitle="Publiceren WebApplicationVM | Microsoft Azure"
   description="Informatie over het implementeren van een webtoepassing op een virtuele machine. Dit script maakt de vereiste bronnen in uw abonnement Azure als deze nog niet bestaan."
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

# <a name="publish-webapplicationvm-windows-powershell-script"></a>Publiceren WebApplicationVM (Windows PowerShell-script)

Implementeert u een webtoepassing op een virtuele machine. Het script maakt de vereiste bronnen in uw abonnement Azure, als deze nog niet bestaan.

```
Publish-WebApplicationVM
–Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-VMPassword @{Name = "name"; Password = "password")
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

### <a name="configuration"></a>Configuratie

Het pad naar het configuratiebestand JSON die de details van de implementatie beschrijft.

|Aliassen|geen|
|---|---|
|Vereist?|True|
|Positie|met de naam|
|Standaardwaarde|geen|
|Invoer pijplijn accepteren?|False|
|Jokertekens accepteren?|False|

### <a name="subscriptionname"></a>SubscriptionName

De naam van het Azure abonnement waarin u wilt dat de virtuele machine maken.

|Aliassen|geen|
|---|---|
|Vereist?|False|
|Positie|met de naam|
|Standaardwaarde|Het eerste abonnement gebruikt in het abonnementsbestand|
|Invoer pijplijn accepteren?|False|
|Jokertekens accepteren?|False|

### <a name="webdeploypackage"></a>WebDeployPackage

Het pad naar het implementatiepakket web publiceren naar de virtuele machine. U kunt dit pakket maken met behulp van de wizard Publiceren in Visual Studio. Zie [hoe: implementatiepakket Web maken in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).

|Aliassen|geen|
|---|---|
|Vereist?|False|
|Positie|met de naam|
|Standaardwaarde|geen|
|Invoer pijplijn accepteren?|False|
|Jokertekens accepteren?|False|

### <a name="allowuntrusted"></a>AllowUntrusted

Als de waarde true is, kunt u het gebruik van certificaten die niet zijn ondertekend door een vertrouwde basiscertificeringsinstantie.

|Aliassen|geen|
|---|---|
|Vereist?|False|
|Positie|met de naam|
|Standaardwaarde|False|
|Invoer pijplijn accepteren?|False|
|Jokertekens accepteren?|False|

### <a name="vmpassword"></a>VMPassword

De referenties voor de account van de virtuele machine. Voorbeeld: VMPassword - @{Name = "admin"; Wachtwoord = "wachtwoord"}

|Aliassen|geen|
|---|---|
|Vereist?|False|
|Positie|met de naam|
|Standaardwaarde|geen|
|Invoer pijplijn accepteren?|False|
|Jokertekens accepteren?|False|

### <a name="databaseserverpassword"></a>DatabaseServerPassword

De referenties voor de SQL-database in Azure. Voorbeeld: DatabaseServerPassword - @{Name = "admin"; Wachtwoord = "wachtwoord"}

|Aliassen|geen|
|---|---|
|Vereist?|False|
|Positie|met de naam|
|Standaardwaarde|geen|
|Invoer pijplijn accepteren?|False|
|Jokertekens accepteren?|False|

### <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput

Als de waarde true is, berichten afdrukken vanuit het script naar de uitvoerstroom.

|Aliassen|geen|
|---|---|
|Vereist?|False|
|Positie|met de naam|
|Standaardwaarde|False|
|Invoer pijplijn accepteren?|False|
|Jokertekens accepteren?|False|

## <a name="remarks"></a>Opmerkingen

Dev- en testomgevingen, Zie voor een volledige uitleg over het gebruik van het script voor het maken van [Windows PowerShell-Scripts gebruiken om te publiceren op de Dev- en testomgevingen](vs-azure-tools-publishing-using-powershell-scripts.md).

Het configuratiebestand JSON geeft de details van wat er moet worden ingezet. Deze bevat de informatie die u hebt opgegeven bij het maken van het project, zoals de naam, de groep affiniteit, VHD afbeelding en grootte van de virtuele machine. Ook bevat de eindpunten op de virtuele machine, de databases worden ingericht, indien van toepassing en implementatie parameters voor websites. De volgende code toont een voorbeeld van de JSON-configuratiebestand:

```
{
    "environmentSettings": {
        "cloudService": {
            "name": "myvmname",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myvmname",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201404.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [
                    {
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [
            {
                "connectionStringName": "",
                "databaseName": "",
                "serverName": "",
                "user": "",
                "password": ""
            }
        ],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

Kunt u de JSON-configuratiebestand om te wijzigen wat wordt ingericht. Een virtuele machine en een cloud-service zijn vereist, maar de database-sectie is optioneel.
