<properties 
pageTitle="Cloud Services-rol config XPath bedriegen blad | Microsoft Azure" 
description="De verschillende instellingen voor XPath kunt u in de cloud service rol config instellingen weergegeven als een omgevingsvariabele." 
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
ms.date="08/10/2016" 
ms.author="adegeo"/>

# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a>Rol van configuratie-instellingen weergeven als een omgevingsvariabele met XPath

In de cloud service werknemer of een definitiebestand voor webonderdelen rol kan runtime waarden van de systeemconfiguratie worden blootgesteld als omgevingsvariabelen. De volgende XPath-waarden worden ondersteund (die overeenkomen met de waarden van de API).

Deze XPath-waarden zijn ook beschikbaar via de bibliotheek [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) . 

## <a name="app-running-in-emulator"></a>Uitgevoerd in de emulator App

Geeft aan dat de toepassing wordt uitgevoerd in de emulator.

| Type  | Voorbeeld |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/Deployment/@emulated" |
| Code  | var x = RoleEnvironment.IsEmulated; |


## <a name="deployment-id"></a>Installatie-ID

De installatie-ID voor de instantie opgehaald.

| Type  | Voorbeeld |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/Deployment/@id" |
| Code  | var deploymentId = RoleEnvironment.DeploymentId; |


## <a name="role-id"></a>Rol-ID 

Haalt de huidige rol-ID voor de instantie.

| Type  | Voorbeeld |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@id" |
| Code  | var-id = RoleEnvironment.CurrentRoleInstance.Id; |


## <a name="update-domain"></a>Domein bijwerken

Het domein van de update van het exemplaar wordt opgehaald.

| Type  | Voorbeeld |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@updateDomain" |
| Code  | ud var = RoleEnvironment.CurrentRoleInstance.UpdateDomain; |


## <a name="fault-domain"></a>Fout met betrekking tot domein

Het domein voor fouttolerantie van de instantie wordt opgehaald.

| Type  | Voorbeeld |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@faultDomain" |
| Code  | var fd = RoleEnvironment.CurrentRoleInstance.FaultDomain; |


## <a name="role-name"></a>Rolnaam

Haalt de naam van de rol van de instanties.

| Type  | Voorbeeld |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@roleName" |
| Code  | var rname = RoleEnvironment.CurrentRoleInstance.Role.Name;  |


## <a name="config-setting"></a>Configuratie-instelling

Haalt de waarde van de opgegeven configuratie-instelling.

| Type  | Voorbeeld |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting[@name='Setting1']/@value" |
| Code  | instelling var = RoleEnvironment.GetConfigurationSettingValue("Setting1"); |
 
## <a name="local-storage-path"></a>Lokale opslag pad

Haalt het pad van de lokale opslag van het exemplaar.

| Type  | Voorbeeld |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@path" |
| Code  | var localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1"). RootPath; |


## <a name="local-storage-size"></a>Lokale opslagruimte

Haalt de grootte van de lokale opslag voor de instantie.

| Type  | Voorbeeld |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@sizeInMB" |
| Code  | var localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1"). MaximumSizeInMegabytes; |

## <a name="endpoint-protocol"></a>Eindpunt-protocol 

Haalt het eindpunt-protocol voor het exemplaar.

| Type  | Voorbeeld |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@protocol" |
| Code  | prot var RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1 ="]. Protocol; |

## <a name="endpoint-ip"></a>Eindpunt IP

Deze eigenschap haalt het eindpunt opgegeven IP-adres.

| Type | Voorbeeld |
| ----- | ---- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@address" |
| Code  | adres var RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1 ="]. IPEndpoint.Address |

## <a name="endpoint-port"></a>Poort van eindpunt 

De poort van eindpunt voor het exemplaar opgehaald.

| Type  | Voorbeeld |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@port" |
| Code  | poort var RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1 ="]. IPEndpoint.Port; |





## <a name="example"></a>Voorbeeld

Hier volgt een voorbeeld van de rol van een werknemer die wordt gemaakt van een taak is gestart met een omgevingsvariabele met de naam `TestIsEmulated` ingesteld op de [ @emulated de xpath-waarde](#app-running-in-emulator). 

```xml
<WorkerRole name="Role1">
    <ConfigurationSettings>
      <Setting name="Setting1" />
    </ConfigurationSettings>
    <LocalResources>
      <LocalStorage name="LocalStore1" sizeInMB="1024"/>
    </LocalResources>
    <Endpoints>
      <InternalEndpoint name="Endpoint1" protocol="tcp" />
    </Endpoints>
    <Startup>
      <Task commandLine="example.cmd inputParm">
        <Environment>
          <Variable name="TestConstant" value="Constant"/>
          <Variable name="TestEmptyValue" value=""/>
          <Variable name="TestIsEmulated">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
          </Variable>
          ...
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="TestConstant" value="Constant"/>
        <Variable name="TestEmptyValue" value=""/>
        <Variable name="TestIsEmulated">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
        </Variable>
        ...
      </Environment>
    </Runtime>
    ...
</WorkerRole>
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het bestand [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) .

Maak een pakket [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) .

Inschakelen, [Extern bureaublad](cloud-services-role-enable-remote-desktop.md) voor een rol.
