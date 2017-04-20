<properties
   pageTitle="Integratie van de Azure logboek Veelgestelde vragen | Microsoft Azure"
   description="Deze Veelgestelde vragen antwoorden op vragen over de integratie van de Azure logboek."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/23/2016"
   ms.author="TomSh"/>

# <a name="azure-log-integration-frequently-asked-questions-faq"></a>Integratie van de Azure logboek Veelgestelde vragen (FAQ)

Deze Veelgestelde vragen antwoorden op vragen over de integratie van de Azure log, een service waarmee ruwe logboeken van uw resources Azure integreren in uw systemen van beveiligingsinformatie en gebeurtenis Management (SIEM) op gebouwen. Deze integratie garandeert een consistente dashboard voor alle activa, in ruimten of in de cloud, zodat verzamelen, correleren, analyseren en voor beveiligingsgebeurtenissen die zijn gekoppeld aan uw toepassingen waarschuwen.

## <a name="how-can-i-see-the-storage-accounts-from-which-azure-log-integration-is-pulling-azure-vm-logs-from"></a>Hoe kan ik zien dat de opslag rekeningen waarvan is Azure logboek integratie Azure VM Logboeken uit trekken?

Voer de opdracht **lijst azlog**.

## <a name="how-can-i-update-the-proxy-configuration"></a>Hoe kan ik de proxyconfiguratie bijwerken?

Als de proxy-instellingen niet mogelijk is toegang tot opslagsystemen Azure rechtstreeks, opent u de **AZLOG. EXE. CONFIG** bestand in **c:\Program Files\Microsoft Azure logboek integratie**. Het bestand de sectie **defaultProxy** met het adres van de proxyserver van uw organisatie bijwerken. Nadat update is voltooid, stopt en start de service via de opdrachten **net stop azlog** en **net start azlog**.

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.net>
        <connectionManagement>
          <add address="*" maxconnection="400" />
        </connectionManagement>
        <defaultProxy>
          <proxy usesystemdefault="true"
          proxyaddress=http://127.0.0.1:8888
          bypassonlocal="true" />
        </defaultProxy>
      </system.net>
      <system.diagnostics>
        <performanceCounters filemappingsize="20971520" />
      </system.diagnostics>   

## <a name="how-can-i-see-the-subscription-information-in-windows-events"></a>Hoe kan ik de abonnementsgegevens in Windows-gebeurtenissen weergeven?

De **subscriptionid** aan de beschrijvende naam toevoegen tijdens het toevoegen van de bron.

    Azlog source add <sourcefriendlyname>.<subscription id> <StorageName> <StorageKey>  

De gebeurtenis XML heeft de metagegevens, zoals hieronder weergegeven, inclusief de abonnement-id.

![XML-gebeurtenis][1]

## <a name="error-messages"></a>Foutberichten

### <a name="when-running-command-azlog-createazureid-why-do-i-get-the-following-error"></a>Bij het uitvoeren van opdracht **azlog createazureid**, waarom het volgende foutbericht wordt?

Fout:

  *72f988bf-86f1-41af-91ab-2d7cd011db37-reden kan maken van de toepassing van AAD - pachters = 'Niet toegestaan' - bericht = 'Onvoldoende rechten om de bewerking te voltooien'.*

**Azlog createazureid** probeert te maken van een service principal in de Azure AD huurder voor de abonnementen waarvoor de Azure aanmelding toegang tot heeft. Als uw aanmelding Azure een gastgebruiker in die huurder Azure AD is, vervolgens de opdracht niet werkt met 'Onvoldoende machtigingen om de bewerking te voltooien'. Beheer uw account toevoegen als een gebruiker in de pachter huurder aanvragen.

### <a name="when-running-command-azlog-authorize-why-do-i-get-the-following-error"></a>Bij het uitvoeren van opdracht **azlog staan**, waarom het volgende foutbericht wordt?

Fout:

  *Waarschuwing roltoewijzing - AuthorizationFailed maken: de client janedo@microsoft.com' met object-id 'fe9e03e4-4dad-4328-910f-fd24a9660bd2' geen vergunning voor het uitvoeren van de actie 'Microsoft.Authorization/roleAssignments/write' bereik '/ abonnementen/70 d 95299-d689-4c 97-b971-0d8ff0000000'.*

**Azlog toestaan** opdracht wijst u de rol van lezer voor de Azure AD service principal (gemaakt met **Azlog, createazureid**) voor de abonnementen die. Als de aanmelding Azure een CO-beheerder of eigenaar van het abonnement is, is mislukt en het 'Verificatie is mislukt' wordt weergegeven. Azure op rollen gebaseerde toegangscontrole (RBAC) van CO-beheerder of eigenaar nodig is om deze actie te voltooien.

## <a name="where-can-i-find-the-definition-of-the-properties-in-audit-log"></a>Waar vind ik de definitie van de eigenschappen in het controlelogboek

Zie:

- [Bewerkingen met Resource Manager controleren](../resource-group-audit.md)
- [Lijst van de gebeurtenissen in een abonnement in Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931934.aspx)

## <a name="where-can-i-find-details-on-azure-security-center-alerts"></a>Waar vind ik meer informatie over meldingen van Beveiligingscentrum Azure

Zie [beheren en reageren op de beveiligingswaarschuwingen in Azure Security Center](../security-center/security-center-managing-and-responding-alerts.md).

## <a name="how-can-i-modify-what-is-collected-with-vm-diagnostics"></a>Hoe kan ik wijzigen wat door VM diagnostische gegevens worden verzameld?

[Gebruik PowerShell om diagnostiek op een virtuele machine met Windows Azure](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md) Zie voor meer informatie over het ophalen, wijzigen, en de Azure diagnostische gegevens instellen in Windows *(af)* configuratie. Hier volgt een voorbeeld:

### <a name="get-the-wad-config"></a>De configuratie af te halen

    -AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient
    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

    $xmlconfig | Out-File -Encoding utf8 -FilePath "d:\WADConfig.xml"

### <a name="modify-the-wad-config"></a>Het af-configuratie wijzigen

In het volgende voorbeeld wordt een configuratie waar alleen EventID 4624 en gebeurtenis-id 4625 worden verzameld in het logboek voor beveiligingsgebeurtenissen. Microsoft Antimalware-gebeurtenissen worden verzameld in het logboek voor systeemgebeurtenissen. Zie [gebeurtenissen verbruikt] (https://msdn.microsoft.com/library/windows/desktop/dd996910 (v=vs.85) voor meer informatie over het gebruik van XPath-expressies.

    <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>
    </WindowsEventLog>

### <a name="set-the-wad-configuration"></a>Stel de configuratie af

    $diagnosticsconfig_path = "d:\WADConfig.xml"
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName log3121 -StorageAccountKey <storage key>

Lees na het aanbrengen van wijzigingen, de opslag om ervoor te zorgen dat de juiste gebeurtenissen worden verzameld.

Als u vragen over Azure logboek integratie hebt, stuur een e-mail naar [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

<!--Image references-->
[1]: ./media/security-azure-log-integration-faq/event-xml.png
