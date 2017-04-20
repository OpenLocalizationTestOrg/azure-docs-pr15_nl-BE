<properties
   pageTitle="Wat zijn Service Fabric toepassing en service-beveiligingsbeleid | Microsoft Azure"
   description="Een overzicht van het uitvoeren van een Service Fabric-toepassing onder systeem en voor lokale beveiligingsaccounts, met inbegrip van het SetupEntry punt waarin een toepassing een geprivilegieerde actie laten uitvoeren moet voordat deze begint"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/22/2016"
   ms.author="mfussell"/>

# <a name="configure-security-policies-for-your-application"></a>Beveiligingsbeleid voor uw toepassing configureren
Azure Service Fabric biedt de mogelijkheid om beveiligde toepassingen in het cluster met verschillende gebruikersaccounts. Fabric-service beveiligt ook de middelen die door de toepassingen worden gebruikt bij de implementatie van de account van de gebruiker, zoals bestanden, mappen en certificaten. Hierdoor waarop toepassingen worden uitgevoerd, zelfs in een gedeelde, gehoste omgeving beveiligen tegen elkaar. 

Standaard Service Fabric-toepassingen worden uitgevoerd onder de account die compatibel is met het proces van Fabric.exe. Fabric-service biedt ook de mogelijkheid om toepassingen uitvoeren in een lokale gebruikersaccount of een lokale systeemaccount in het manifest van de toepassing opgegeven. Ondersteund systeem van de lokale zijn account voor **lokalegebruiker**, **NetworkService**, **LocalService**en **LocalSystem**.

 Wanneer Fabric-Service wordt uitgevoerd op een Server in uw datacenter met behulp van het zelfstandige installatieprogramma voor Windows, kunt u Active Directory (AD)-domeinaccounts.

Groepen kunnen worden gedefinieerd en gemaakt, zodat een of meer gebruikers kunnen worden toegevoegd aan elke groep moeten worden beheerd samen. Dit is handig als er meerdere gebruikers voor verschillende ingangspunten zijn en moeten zij bepaalde algemene bevoegdheden die beschikbaar op het groepeerniveau van de zijn hebben.

## <a name="configure-the-policy-for-service-setupentrypoint"></a>Configureer het beleid voor de service SetupEntryPoint

Zoals beschreven in het [toepassingsmodel](service-fabric-application-model.md) van is de **SetupEntryPoint** een beschermde post die wordt uitgevoerd met dezelfde referenties als Service Fabric (doorgaans de account *NetworkService* ) voordat een ingangspunt. Het uitvoerbare bestand opgegeven met **EntryPoint** is meestal de langdurige ServiceHost, met een afzonderlijke installatie ingangspunt wordt vermeden dat de ServiceHost uitvoerbaar bestand uitvoeren met hoge bevoegdheden voor langere tijd. Het uitvoerbare bestand opgegeven met **EntryPoint** wordt uitgevoerd nadat de **SetupEntryPoint** met succes wordt afgesloten. Het resulterende proces is gecontroleerd en opnieuw gestart, opnieuw beginnen met **SetupEntryPoint**, als het ooit wordt beëindigd of vastloopt.

Hier volgt een eenvoudige service manifest voorbeeld ziet u de SetupEntryPoint en de belangrijkste EntryPoint voor de service.

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="Config" Version="1.0.0" />
</ServiceManifest>
~~~

### <a name="configure-the-policy-using-a-local-account"></a>Met een lokale account-beleid configureren

Nadat u de service kunt u een ingangspunt setup hebt geconfigureerd, kunt u de machtigingen die wordt uitgevoerd onder in het manifest van de toepassing. In het volgende voorbeeld ziet u hoe de service worden uitgevoerd onder de machtigingen voor de gebruikersaccount beheerder configureren.

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupAdminUser">
            <MemberOf>
               <SystemGroup Name="Administrators" />
            </MemberOf>
         </User>
      </Users>
   </Principals>
</ApplicationManifest>
~~~

Maak eerst een **beveiligings-Principals** sectie met de gebruikersnaam van een, zoals SetupAdminUser. Hiermee wordt aangegeven dat de gebruiker lid van de groep Administrators systeem is.

Configureer een beleid voor deze principal van toepassing op **SetupEntryPoint**in de sectie **ServiceManifestImport** . Deze vertelt Service Fabric dat wanneer het bestand **MySetup.bat** wordt uitgevoerd, RunAs met administrator-bevoegdheden worden moet. Gezien het feit dat er een beleid *niet* toegepast op de belangrijkste toegangspunt, de code in de **MyServiceHost.exe** wordt uitgevoerd onder de account **Netwerkservice** -systeem. Dit is de standaardaccount die alle ingangspunten van de service worden uitgevoerd.

Nu we het bestand MySetup.bat toegevoegd aan de Visual Studio-project voor het testen van de administrator-bevoegdheden. Klik met de rechtermuisknop op de service-project in Visual Studio en een nieuw bestand met de naam MySetup.bat toevoegen.

Controleer vervolgens dat het bestand MySetup.bat is opgenomen in het servicepakket. Standaard is het niet. Selecteer het bestand, klik met de rechtermuisknop om het snelmenu en kies **Eigenschappen**. Controleer of **kopiëren naar de uitvoermap** is ingesteld **als nieuwere**kopiëren in het dialoogvenster Eigenschappen. Zie de volgende schermafdruk.

![CopyToOutput van Visual Studio voor het batchbestand SetupEntryPoint][image1]

Nu opent u het bestand MySetup.bat en voeg de volgende opdrachten:

~~~
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set to > out.txt
echo %TestVariable% >> out.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
~~~

Vervolgens bouwen en implementeren de oplossing op een cluster van plaatselijke ontwikkeling.  Nadat de service is gestart, zoals wordt weergegeven in de Service Fabric Explorer, kunt u zien dat de MySetup.bat met succes is uitgevoerd in een op twee manieren. Open een PowerShell-opdrachtprompt en typ:

~~~
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
~~~

Noteer de naam van het knooppunt waarop de service is geïmplementeerd en is gestart in de Explorer Fabric-Service bijvoorbeeld knooppunt 2. Ga vervolgens naar de map application exemplaar werk naar het bestand out.txt wordt de waarde van **TestVariable**. Voor het voorbeeld als deze service is geïmplementeerd op knooppunt 2 en u gaat naar dit pad voor de **MyApplicationType**:

~~~
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
~~~

###  <a name="configure-the-policy-using-local-system-accounts"></a>Het lokale systeemaccounts met beleid configureren
Vaak is het raadzaam om het opstartscript met een lokale systeemaccount in plaats van een beheerdersaccount zoals voorgaande worden uitgevoerd. De RunAs actief werkt beleid als beheerders meestal niet goed omdat de machines hebben gebruikers toegang tot Gebruikersaccountbeheer (UAC) is standaard ingeschakeld. In dergelijke gevallen **de aanbeveling is de SetupEntryPoint uitgevoerd als LokaalSysteem in plaats van een lokale gebruiker toegevoegd aan de groep administrators**. In het volgende voorbeeld ziet u het instellen van de SetupEntryPoint kan worden uitgevoerd als LokaalSysteem.

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupLocalSystem" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupLocalSystem" AccountType="LocalSystem" />
      </Users>
   </Principals>
</ApplicationManifest>
~~~

##  <a name="launch-powershell-commands-from-a-setupentrypoint"></a>PowerShell-opdrachten uit een SetupEntryPoint starten
PowerShell uitvoeren vanaf het punt **SetupEntryPoint** , kunt u in een batch-bestand die naar een bestand met PowerShell verwijst **PowerShell.exe** uitvoeren. Eerst een PowerShell-bestand toevoegt aan het project service, zoals **MySetup.ps1**. Vergeet niet de eigenschap *kopiëren als nieuwere* zodanig instellen dat het bestand ook in het servicepakket opgenomen is. In het volgende voorbeeld ziet u een voorbeeldbatchbestand starten PowerShell bestand met de naam MySetup.ps1 waarin een systeemomgevingsvariabele **TestVariable**genoemd.


MySetup.bat voor het starten van PowerShell-bestand.

~~~
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
~~~

Voeg de volgende in te stellen een systeemomgevingsvariabele die is gedefinieerd in het bestand PowerShell:

~~~
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
~~~

**Opmerking:** Standaard als het batchbestand wordt uitgevoerd gekeken naar de toepassingsmap die **werken** voor bestanden genoemd. In dit geval, wanneer MySetup.bat wordt uitgevoerd willen we dit de MySetup.ps1 vinden in dezelfde map is de map application **code pakket** . Instellen als u wilt wijzigen in deze map, de map zoals in de volgende.

~~~
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    </ExeHost>
</SetupEntryPoint>
~~~

## <a name="using-console-redirection-for-local-debugging"></a>Met behulp van consoleomleiding voor lokale foutopsporing
Soms is het handig om te zien van de console-uitvoer van het uitvoeren van een script voor foutopsporing. U kunt een console-Omleidingsbeleid van de uitvoer wordt geschreven naar een bestand instellen om dit te doen. De uitvoer in een bestand wordt geschreven naar de map application **log** op het knooppunt waarop de toepassing wordt geïmplementeerd en uitgevoerd (Zie waar vindt u dit in het voorgaande voorbeeld) genoemd.

**Opmerking: nooit** het omleidingsbeleid console gebruiken in een toepassing die is geïmplementeerd in de productie, omdat dit invloed op de toepassing failover hebben kan. **Alleen** Gebruik dit voor plaatselijke ontwikkeling en foutopsporing.  

In het volgende voorbeeld ziet u de console-omleiding met een FileRetentionCount-waarde instellen.

~~~
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
~~~

Als u het bestand MySetup.ps1 om te schrijven een opdracht **Echo** nu wijzigt, wordt dit schrijven naar het uitvoerbestand voor foutopsporing.

~~~
Echo "Test console redirection which writes to the application log folder on the node that the application is deployed to"
~~~

**Zodra u het script hebt opgespoord, onmiddellijk verwijderen van dit beleid van console-omleiding**

## <a name="configure-policy-for-service-code-packages"></a>Beleid configureren voor code-servicepakketten 
In de voorgaande stappen hebt u gezien hoe RunAs beleid toepassen op SetupEntryPoint. We gaan naar een beetje dieper in het maken van verschillende beveiligings-principals die als service-beleid kunnen worden toegepast.

### <a name="create-local-user-groups"></a>Lokale groepen maken
Gebruikersgroepen worden gedefinieerd en gemaakt die een of meer gebruikers worden toegevoegd aan een groep. Dit is vooral handig als er meerdere gebruikers voor verschillende ingangspunten zijn en zij nodig hebben om bepaalde algemene bevoegdheden die beschikbaar op het groepeerniveau van de zijn. In het volgende voorbeeld ziet u een lokale groep met de naam **LocalAdminGroup** met administrator-bevoegdheden. Twee gebruikers, Customer1 en Customer2, worden leden van deze lokale groep gemaakt.

~~~
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
     </Membership>
   </Group>
 </Groups>
  <Users>
     <User Name="Customer1">
        <MemberOf>
           <Group NameRef="LocalAdminGroup" />
        </MemberOf>
     </User>
    <User Name="Customer2">
      <MemberOf>
        <Group NameRef="LocalAdminGroup" />
      </MemberOf>
    </User>
  </Users>
</Principals>
~~~

### <a name="create-local-users"></a>Lokale gebruikers maken
U kunt een lokale gebruiker die kan worden gebruikt voor het beveiligen van een service in de toepassing maken. Wanneer een type **lokalegebruiker** is opgegeven in de sectie beveiligings-principals van het manifest van de toepassing, Service Fabric wordt gemaakt van lokale gebruikersaccounts op computers waarop de toepassing wordt gedistribueerd. Standaard hoeft deze accounts niet dezelfde namen als die welke in het manifest van de toepassing (bijvoorbeeld Customer3 in het volgende voorbeeld). Ze worden dynamisch gegenereerd en willekeurige wachtwoorden.

~~~
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
~~~

<!-- If an application requires that the user account and password be same on all machines (for example, to enable NTLM authentication), the cluster manifest must set NTLMAuthenticationEnabled to true. The cluster manifest must also specify an NTLMAuthenticationPasswordSecret that will be used to generate the same password across all machines.

<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
-->

### <a name="assign-policies-to-the-service-code-packages"></a>Beleid toewijzen aan de code-servicepakketten
In de sectie **RunAsPolicy** voor een **ServiceManifestImport** wordt de account in de sectie beveiligings-principals die moet worden gebruikt om een pakket code uitvoeren. Pakketten van het servicemanifest code wordt gekoppeld aan gebruikersaccounts in de sectie beveiligings-principals. U kunt dit opgeven voor de installatie of de belangrijkste ingangspunten of kunt u `All` op beide toepassen. In het volgende voorbeeld ziet u verschillende soorten beleid wordt toegepast:

~~~
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
~~~

Als **EntryPointType** niet is opgegeven, wordt standaard ingesteld op EntryPointType = "Main". Opgeven **SetupEntryPoint** is vooral handig als u een bepaalde bewerking hoge bevoegdheid setup met een systeemaccount worden uitgevoerd. De werkelijke code kunt uitvoeren onder een account van de laagste bevoegdheid.

### <a name="apply-a-default-policy-to-all-service-code-packages"></a>Een standaardbeleid toepassen op alle code servicepakketten
De sectie **DefaultRunAsPolicy** wordt gebruikt om aan te geven een standaardgebruikersaccount voor alle pakketten voor code die geen specifieke **RunAsPolicy** gedefinieerd. Als de meeste van de pakketten code is opgegeven in het servicemanifest van de wordt gebruikt door een toepassing worden uitgevoerd onder dezelfde gebruiker moet, kan de toepassing een standaardbeleid RunAs met die account alleen definiëren. In het volgende voorbeeld geeft aan dat als een pakket code niet **RunAsPolicy** dat wordt opgegeven, het pakket code moet worden uitgevoerd onder de **MyDefaultAccount** die is opgegeven in de sectie beveiligings-principals.

~~~
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
~~~
### <a name="using-an-active-directory-domain-group-or-user"></a>Met behulp van een gebruiker of groep voor Active Directory-domein
Voor Service Fabric geïnstalleerd op Windows Server met het zelfstandige installatieprogramma voor kunt u, de service onder de referenties voor een gebruiker in Active Directory (AD) of groepsaccount uitvoeren. Opmerking: Dit is Active Directory lokale binnen uw domein en niet met Azure Active Directory (AAD). U kunt andere bronnen in het domein (bijvoorbeeld gedeelde bestanden) die u hebt gemachtigd benaderen met behulp van een domeingebruiker of een groep.

Het volgende voorbeeld wordt een AD-gebruiker *TestUser* aangeroepen met hun domeinwachtwoord gecodeerd met behulp van een certificaat genoemd *MyCert*. U kunt de `Invoke-ServiceFabricEncryptText` Powershell-opdracht voor het maken van de geheime gecodeerde tekst. Raadpleeg dit artikel [geheimen in Service Fabric-toepassingen beheren](service-fabric-application-secret-management.md) voor meer informatie over. De persoonlijke sleutel van het certificaat voor het decoderen van het wachtwoord moet worden geïmplementeerd op de lokale computer in een out-of-band-methode (in Azure dit via de bronnenbeheerder is). Vervolgens, wanneer Fabric-Service het servicepakket naar de computer implementeert, is het kunnen decoderen het geheim en samen met de naam van de gebruiker worden geverifieerd met AD onder deze referenties worden uitgevoerd.

~~~
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
~~~


## <a name="assign-securityaccesspolicy-for-http-and-https-endpoints"></a>SecurityAccessPolicy voor HTTP- en HTTPS-eindpunten toewijzen
Als u een RunAs-beleid toepassen op een service, servicemanifest verklaart eindpunt resources met het HTTP-protocol, moet u een **SecurityAccessPolicy** om ervoor te zorgen dat poorten toegewezen aan deze eindpunten correct toegangsbeheer voor de RunAs-gebruikersaccount die door de service wordt uitgevoerd onder vermeld. Anders **http.sys** heeft geen toegang tot de service en krijgt u fouten met aanroepen van de client. In het volgende voorbeeld wordt de account Customer3 geldt voor een eindpunt genaamd **ServiceEndpointName**, volledige toegangsrechten geven.

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
~~~

Voor het eindpunt HTTPS hebt u ook om aan te geven van de naam van het certificaat om terug te keren naar de client. U kunt dit doen met behulp van **EndpointBindingPolicy**, met het certificaat dat is gedefinieerd in een sectie van de certificaten in het manifest van de toepassing.

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
~~~


## <a name="a-complete-application-manifest-example"></a>Een voorbeeld van de volledige toepassing manifest
Het volgende manifest van de toepassing bevat veel van de andere instellingen:

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application3Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="Stateless1_InstanceCount" DefaultValue="-1" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
         <RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup" />
        <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
         <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
        <!--EndpointBindingPolicy is needed the EndpointName is secured with https -->
        <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
     </Policies>
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="Stateless1">
         <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
   <Principals>
      <Groups>
         <Group Name="LocalAdminGroup">
            <Membership>
               <SystemGroup Name="Administrators" />
            </Membership>
         </Group>
      </Groups>
      <Users>
         <User Name="LocalAdmin">
            <MemberOf>
               <Group NameRef="LocalAdminGroup" />
            </MemberOf>
         </User>
         <!--Customer1 below create a local account that this service runs under -->
         <User Name="Customer1" />
         <User Name="MyDefaultAccount" AccountType="NetworkService" />
      </Users>
   </Principals>
   <Policies>
      <DefaultRunAsPolicy UserRef="LocalAdmin" />
   </Policies>
   <Certificates>
     <EndpointCertificate Name="Cert1" X509FindValue="FF EE E0 TT JJ DD JJ EE EE XX 23 4T 66 "/>
  </Certificates>
</ApplicationManifest>
~~~


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

* [Het application-objectmodel begrijpen](service-fabric-application-model.md)
* [Bronnen opgeven in een servicemanifest](service-fabric-service-manifest-resources.md)
* [Een toepassing](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
