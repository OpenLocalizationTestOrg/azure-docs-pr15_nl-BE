<properties
    pageTitle="Wat is een Cloud-Service-model en een pakket | Microsoft Azure"
    description="Beschrijving van de service cloud-model (.csdef, .cscfg) en in Azure-pakket (.cspkg)"
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>
<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>

# <a name="what-is-the-cloud-service-model-and-how-do-i-package-it"></a>Wat is het model van Cloud-Service en hoe ik een pakket?
Een cloud-service is gemaakt op basis van drie componenten, de definitie van service _(.csdef)_, de service config _(.cscfg)_en een service-pakket _(.cspkg)_. De **ServiceDefinition.csdef** en de **ServiceConfig.cscfg** -bestanden zijn XML- en beschrijven van de structuur van de cloud-service en hoe deze geconfigureerd; het model samen genoemd. De **ServicePackage.cspkg** is een zip-bestand dat wordt gegenereerd uit de **ServiceDefinition.csdef** en onder andere, bevat al de vereiste afhankelijkheden op basis van binaire bestanden. Azure maakt een cloud-service uit de **ServicePackage.cspkg** en de **ServiceConfig.cscfg**.

Zodra de cloud-service wordt uitgevoerd in Azure, kunt u deze configureren via het bestand **ServiceConfig.cscfg** , maar de definitie kan niet worden gewijzigd.

## <a name="what-would-you-like-to-know-more-about"></a>Wat wilt u meer weten?

* Ik wil meer weten over de bestanden [ServiceDefinition.csdef](#csdef) en [ServiceConfig.cscfg](#cscfg) .
* Ik weet al dat, geef me [enkele voorbeelden](#next-steps) van wat ik kan configureren.
* Ik wil de [ServicePackage.cspkg](#cspkg)maken.
* Ik ben met behulp van Visual Studio en wil...
    * [Maak een nieuwe wolk service][vs_create]
    * [Een bestaande cloud-service opnieuw configureren][vs_reconfigure]
    * [Een project Cloud Service implementeren][vs_deploy]
    * [Extern bureaublad in een exemplaar van de service cloud][remotedesktop]

<a name="csdef"></a>
## <a name="servicedefinitioncsdef"></a>ServiceDefinition.csdef
Het bestand **ServiceDefinition.csdef** bevat de instellingen die worden gebruikt bij een cloud-service configureren via Azure. De [Azure Service Definition Schema (.csdef bestand)](https://msdn.microsoft.com/library/azure/ee758711.aspx) biedt de toegestane indeling voor een definitiebestand. In het volgende voorbeeld ziet u de instellingen die voor de Web- en werknemer rollen kunnen worden gedefinieerd:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalHttpIn" protocol="http" />
    </Endpoints>
    <Certificates>
      <Certificate name="Certificate1" storeLocation="LocalMachine" storeName="My" />
    </Certificates>
    <Imports>
      <Import moduleName="Connect" />
      <Import moduleName="Diagnostics" />
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <LocalResources>
      <LocalStorage name="localStoreOne" sizeInMB="10" />
      <LocalStorage name="localStoreTwo" sizeInMB="10" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" />
    </Startup>
  </WebRole>

  <WorkerRole name="WorkerRole1">
    <ConfigurationSettings>
      <Setting name="DiagnosticsConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10000" />
      <InternalEndpoint name="Endpoint2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

U kunt verwijzen naar de [[Service Definition Schema]] voor een beter begrip van het XML-schema gebruikt hier, maar hier is een korte uitleg van enkele van de elementen:

**Sites**  
Bevat de definities voor websites of web-toepassingen die worden gehost in IIS7.

**InputEndpoints**  
Bevat de definities voor eindpunten die verbinding maken met de cloud-service worden gebruikt.

**InternalEndpoints**  
Bevat de definities voor eindpunten die worden gebruikt door de functie exemplaren met elkaar te communiceren.

**ConfigurationSettings**  
Bevat de definities voor de functies van een specifieke rol.

**Certificaten**  
Bevat de definities voor certificaten die nodig zijn voor een rol. In het vorige voorbeeld ziet u een certificaat dat is gebruikt voor de configuratie van verbinding Azure.

**LocalResources**  
Bevat de definities voor lokale opslag-bronnen. Een bron van lokale opslag is een gereserveerde map in het bestandssysteem van de virtuele machine waarop een exemplaar van een rol wordt uitgevoerd.

**Invoer**  
Bevat de definities voor geïmporteerde modules. In het vorige voorbeeld bevat de modules voor verbinding met extern bureaublad en Azure verbinding maken.

**Opstarten**  
Bevat de taken die worden uitgevoerd wanneer de functie wordt gestart. De taken worden gedefinieerd in een .cmd of uitvoerbaar bestand.



<a name="cscfg"></a>
## <a name="serviceconfigurationcscfg"></a>ServiceConfiguration.cscfg
De configuratie van de instellingen voor uw service cloud wordt bepaald door de waarden in het bestand **ServiceConfiguration.cscfg** . U geeft het aantal exemplaren dat u wilt implementeren voor elke rol in dit bestand. De waarden voor de configuratie-instellingen die u hebt gedefinieerd in het bestand van de definitie worden toegevoegd aan het configuratiebestand van de service. De vingerafdrukken voor het beheer van certificaten die gekoppeld aan de service cloud zijn ook toegevoegd aan het bestand. Het [Configuratieschema Azure-Service (.cscfg bestand)](https://msdn.microsoft.com/library/azure/ee758710.aspx) biedt de toegestane indeling voor een service-configuratiebestand.

Het configuratiebestand van de service niet is gecomprimeerd met de toepassing, maar naar Azure als een afzonderlijk bestand is geüpload en wordt gebruikt voor het configureren van de cloud-service. U kunt een nieuwe service-configuratiebestand zonder uw cloud-service opnieuw uploaden. De configuratiewaarden voor de cloud-service kunnen worden gewijzigd terwijl de cloud-service wordt uitgevoerd. In het volgende voorbeeld ziet u de configuratie-instellingen die kunnen worden gedefinieerd voor het Web en werknemer rollen:

```xml
<?xml version="1.0"?>
<ServiceConfiguration serviceName="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration">
  <Role name="WebRole1">
    <Instances count="2" />
    <ConfigurationSettings>
      <Setting name="SettingName" value="SettingValue" />
    </ConfigurationSettings>

    <Certificates>
      <Certificate name="CertificateName" thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
      <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption"
         thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
    </Certificates>
  </Role>
</ServiceConfiguration>
```

U kunt verwijzen naar het [Schema van Service-configuratie](https://msdn.microsoft.com/library/azure/ee758710.aspx) voor informatie over het XML-schema gebruikt hier, maar hier is een korte uitleg van de elementen:

**Exemplaren**  
Hiermee configureert u het aantal actieve exemplaren voor de rol. Om te voorkomen dat uw cloud-service mogelijk niet langer beschikbaar is tijdens de upgrade, is het aanbevolen dat u meer dan één exemplaar van uw web gerichte functies implementeren. Dit doet voldoet u aan de richtlijnen van de [Azure berekenen Service niveau overeenkomst (SLA)](http://azure.microsoft.com/support/legal/sla/), externe connectiviteit voor internetgerichte rollen 99.95% waarborgt, wanneer twee of meer exemplaren van de rol van een service worden gebruikt.

**ConfigurationSettings**  
Hiermee configureert u de instellingen voor de actieve exemplaren voor een rol. De naam van de `<Setting>` elementen moeten overeenkomen met de definities in het definitiebestand.

**Certificaten**  
Hiermee configureert u de certificaten die worden gebruikt door de service. In het vorige voorbeeld ziet u hoe het certificaat voor de module RemoteAccess definiëren. De waarde van het kenmerk *vingerafdruk* moet worden ingesteld op de blauwdruk van het certificaat te gebruiken.

<p/>

 >[AZURE.NOTE] De blauwdruk van het certificaat kan worden toegevoegd aan het configuratiebestand met behulp van een teksteditor of de waarde kan worden toegevoegd op het tabblad **certificaten** van de pagina **Eigenschappen** van de rol van Visual Studio.



## <a name="defining-ports-for-role-instances"></a>Poorten bepalen voor exemplaren van de rol
Azure kan slechts één toegangspunt tot de Webrol van een. Dit betekent dat alle verkeer vindt plaats via een IP-adres. U kunt uw websites delen van een poort door de host-header om de aanvraag naar de juiste locatie configureren. U kunt ook uw toepassingen om te luisteren naar de bekende poorten op het IP-adres.

In het volgende voorbeeld ziet u de configuratie voor de rol van een webpagina met een website en web toepassing. De website is geconfigureerd als de standaardlocatie voor de post op poort 80 en webtoepassingen worden geconfigureerd om aanvragen te ontvangen van een andere hostheader met de naam 'mail.mysite.cloudapp.net'.

```xml
<WebRole>
  <ConfigurationSettings>
    <Setting name="DiagnosticsConnectionString" />
  </ConfigurationSettings>
  <Endpoints>
    <InputEndpoint name="HttpIn" protocol="http" <mark>port="80"</mark> />
    <InputEndpoint name="Https" protocol="https" port="443" certificate="SSL"/>
    <InputEndpoint name="NetTcp" protocol="tcp" port="808" certificate="SSL"/>
  </Endpoints>
  <LocalResources>
    <LocalStorage name="Sites" cleanOnRoleRecycle="true" sizeInMB="100" />
  </LocalResources>
  <Site name="Mysite" packageDir="Sites\Mysite">
    <Bindings>
      <Binding name="http" endpointName="HttpIn" />
      <Binding name="https" endpointName="Https" />
      <Binding name="tcp" endpointName="NetTcp" />
    </Bindings>
  </Site>
  <Site name="MailSite" packageDir="MailSite">
    <Bindings>
      <Binding name="mail" endpointName="HttpIn" <mark>hostheader="mail.mysite.cloudapp.net"</mark> />
    </Bindings>
    <VirtualDirectory name="artifacts" />
    <VirtualApplication name="storageproxy">
      <VirtualDirectory name="packages" packageDir="Sites\storageProxy\packages"/>
    </VirtualApplication>
  </Site>
</WebRole>
```


## <a name="changing-the-configuration-of-a-role"></a>De configuratie van een rol wijzigen
U kunt de configuratie van de service cloud bijwerken terwijl deze wordt uitgevoerd in Azure, zonder dat de service off line nemen. Configuratie om informatie te wijzigen, kunt u een nieuwe configuratiebestand uploaden of het configuratiebestand op plaats bewerken en toepassen op uw actieve service. De volgende wijzigingen kunnen worden aangebracht in de configuratie van een service:

- **De waarden van de configuratie-instellingen wijzigen**  
Als een configuratie-instelling wijzigingen een rol exemplaar kunt de wijziging door te voeren terwijl het exemplaar online is, of het exemplaar zonder problemen te recyclen en de wijziging tijdens de sessie off line is.

- **De topologie van de service van rol exemplaren wijzigen**  
Wijzigingen in de netwerktopologie, hebben geen invloed op actieve exemplaren, behalve waar een exemplaar wordt verwijderd. Alle resterende exemplaren in het algemeen hoeft niet te worden herhaald; u kunt echter Prullenbak rol exemplaren in reactie op een topologiewijziging.

- **Vingerafdruk van het certificaat wijzigen**  
Wanneer een instantie van de functie Off line is, kunt u alleen een certificaat bijwerken. Als een certificaat is toegevoegd, verwijderd of gewijzigd terwijl een exemplaar van de rol online is, wordt Azure netjes het exemplaar offline voor het bijwerken van het certificaat en weer on line brengen nadat de wijziging voltooid is.

### <a name="handling-configuration-changes-with-service-runtime-events"></a>Wijzigingen in de configuratie met Service Runtime-gebeurtenissen afhandelen
De [Azure Runtime Library](https://msdn.microsoft.com/library/azure/mt419365.aspx) bevat de [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) -naamruimte die klassen bevat voor de interactie met de Azure-omgeving uit code die wordt uitgevoerd in een exemplaar van een rol. De klasse [RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) definieert de volgende gebeurtenissen die worden geactiveerd voor en na een wijziging in de configuratie:

- **De gebeurtenis [wijzigen](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx)**  
Dit gebeurt voordat de wijziging in de configuratie wordt toegepast op een opgegeven exemplaar van een rol waarin u de rol exemplaren nemen indien nodig.
- **Gebeurtenis [Changed](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx)**  
Deze gebeurtenis vindt plaats nadat de wijziging in de configuratie is toegepast op een opgegeven exemplaar van een rol.

> [AZURE.NOTE] Omdat het certificaat wijzigingen altijd de exemplaren van een rol offline halen, doen zij de gebeurtenissen RoleEnvironment.Changing of RoleEnvironment.Changed niet verhogen.

<a name="cspkg"></a>
## <a name="servicepackagecspkg"></a>ServicePackage.cspkg
Voor de implementatie van een toepassing als een cloud-service in Azure, moet u eerst de toepassing in de juiste indeling inpakken. Het pakketbestand maken als alternatief voor Visual Studio kunt u **CSPack** hulpprogramma voor de opdrachtregel (geïnstalleerd met de [SDK van Azure](https://azure.microsoft.com/downloads/)).

De inhoud van het definitiebestand en service-configuratiebestand **CSPack** gebruikt voor het definiëren van de inhoud van het pakket. **CSPack** genereert een application-pakketbestand (.cspkg) die u naar Azure uploaden kunt via de [portal Azure](cloud-services-how-to-create-deploy-portal.md#create-and-deploy). Standaard het pakket met de naam `[ServiceDefinitionFileName].cspkg`, maar u kunt ook een andere naam opgeven met behulp van de `/out` de optie van de **CSPack**.

**CSPack** bevindt zich in het algemeen  
`C:\Program Files\Microsoft SDKs\Azure\.NET SDK\[sdk-version]\bin\`

>[AZURE.NOTE]
CSPack.exe (op windows) is beschikbaar op de snelkoppeling naar **Microsoft Azure opdrachtprompt** die wordt geïnstalleerd met de SDK wordt uitgevoerd.  
>  
>Het programma CSPack.exe zelf Zie documentatie over alle mogelijke switches en opdrachten uitvoeren.

<p />

>[AZURE.TIP]
Uw cloud-service in de **Microsoft Azure berekenen Emulator**lokaal worden uitgevoerd, gebruikt u de optie **/copyonly** is die deze optie kopieert de binaire bestanden voor de toepassing van een directory-indeling waarmee ze kunnen worden uitgevoerd in de compute-emulator.

### <a name="example-command-to-package-a-cloud-service"></a>Van de voorbeeldopdracht om een cloud-service
Het volgende voorbeeld wordt een toepassingspakket met de gegevens voor een web-functie. De opdracht geeft het definitiebestand service te gebruiken, de map waar de binaire bestanden kunnen worden gevonden, en de naam van het pakket.

    cspack [DirectoryName]\[ServiceDefinition]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /out:[OutputFileName]

Als de toepassing zowel een Webrol en de rol van een werknemer bevat, wordt de volgende opdracht gebruikt:

    cspack [DirectoryName]\[ServiceDefinition]
           /out:[OutputFileName]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]

Waar worden de variabelen als volgt gedefinieerd:

| Variabele                  | Waarde |
| ------------------------- | ----- |
| \[DirectoryName\]         | De submap onder de hoofdmap van het project met het bestand .csdef van het project Azure.|
| \[ServiceDefinition\]     | De naam van het definitiebestand. Dit bestand is standaard de naam ServiceDefinition.csdef.  |
| \[OutputFileName\]        | De naam van het gegenereerde bestand. Dit is normaal gesproken ingesteld op de naam van de toepassing. Als er geen bestandsnaam is opgegeven, het toepassingspakket is gemaakt als \[ApplicationName\].cspkg.|
| \[Rolnaam\]              | De naam van de rol als omschreven in het definitiebestand.|
| \[RoleBinariesDirectory] | De locatie van de binaire bestanden voor de rol.|
| \[VirtualPath\]           | De fysieke mappen voor elke virtuele pad dat is gedefinieerd in de sectie Sites van de servicedefinitie van de.|
| \[PhysicalPath\]          | De fysieke mappen voor elke virtuele pad dat is gedefinieerd in het knooppunt site van de definitie van de inhoud.|
| \[RoleAssemblyName\]      | De naam van het binaire bestand voor de rol.| 


## <a name="next-steps"></a>Volgende stappen

Ik ben een cloud service-pakket maken en ik wil...

* [Extern bureaublad instellen voor een exemplaar van de service cloud][remotedesktop]
* [Een project Cloud Service implementeren][deploy]

Ik ben met behulp van Visual Studio en wil...

* [Maak een nieuwe wolk service][vs_create]
* [Een bestaande cloud-service opnieuw configureren][vs_reconfigure]
* [Een project Cloud Service implementeren][vs_deploy]
* [Extern bureaublad instellen voor een exemplaar van de service cloud][vs_remote]

[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop.md
[vs_remote]: ../vs-azure-tools-remote-desktop-roles.md
[vs_deploy]: ../vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md
[vs_reconfigure]: ../vs-azure-tools-configure-roles-for-cloud-service.md
[vs_create]: ../vs-azure-tools-azure-project-create.md
