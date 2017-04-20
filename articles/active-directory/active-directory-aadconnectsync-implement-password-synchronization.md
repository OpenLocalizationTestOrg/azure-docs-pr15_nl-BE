<properties
    pageTitle="Implementeren van Wachtwoordsynchronisatie met Azure AD verbinden sync | Microsoft Azure"
    description="Geeft informatie over hoe Wachtwoordsynchronisatie werkt en hoe u deze inschakelen."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="markusvi;andkjell"/>


# <a name="implementing-password-synchronization-with-azure-ad-connect-sync"></a>Implementeren van Wachtwoordsynchronisatie met Azure verbinden met AD-synchronisatie
In dit onderwerp vindt u informatie die u nodig hebt voor het synchroniseren van uw wachtwoorden van een in de lokalen Active Directory (AD) naar een cloud-gebaseerde Azure Active Directory (AD Azure).

## <a name="what-is-password-synchronization"></a>Wat is de Wachtwoordsynchronisatie
De kans dat u bent niet aan het werk als gevolg van een vergeten wachtwoord is gerelateerd aan het aantal verschillende wachtwoorden hoeft te onthouden. De meer wachtwoorden hoeft te onthouden, des te groter de kans op een vergeten. Vraag naar de meeste bronnen van helpdesk vragen en gesprekken over het opnieuw instellen van wachtwoorden en andere problemen met wachtwoord.

Synchronisatie van wachtwoorden is een functie voor het synchroniseren van wachtwoorden van een Active Directory op de ruimten in een cloud-gebaseerde Azure Active Directory (AD Azure).
Deze functie kunt u aan te melden Azure Active Directory-services (zoals Office 365, Microsoft Intune, CRM Online en Azure AD Domain Services) met behulp van het wachtwoord dat u gebruikt voor het aanmelden bij uw Active Directory op gebouwen.

![Wat is Azure AD verbinden](./media/active-directory-aadconnectsync-implement-password-synchronization/arch1.png)

Vermindert het aantal wachtwoorden moeten gebruikers met één behouden blijven, kunt u synchronisatie van wachtwoorden:

- De productiviteit van uw gebruikers verbeteren
- Verlagen van uw helpdesk  

Ook als u [**Federation met AD FS**](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect)gebruiken selecteert, kunt u desgewenst inschakelen Wachtwoordsynchronisatie als een back-up voor het geval uw AD FS-infrastructuur is mislukt.

Wachtwoordsynchronisatie is een uitbreiding van de synchronisatiefunctie directory is geïmplementeerd door sync Azure AD verbinden. Als u Wachtwoordsynchronisatie in uw omgeving, moet u:

- Installeer AD Azure verbinding  
- Directory-synchronisatie tussen uw op locatie configureren AD en Azure Active Directory
- Wachtwoordsynchronisatie inschakelen

Zie voor meer informatie [uw identiteiten op ruimten met Azure Active Directory integreren](active-directory-aadconnect.md)

> [AZURE.NOTE] Zie voor meer informatie over Active Directory Domain Services die zijn geconfigureerd voor synchronisatie met FIPS en wachtwoord [Password Sync en FIPS](#password-synchronization-and-fips).

## <a name="how-password-synchronization-works"></a>Hoe werkt Wachtwoordsynchronisatie
Wachtwoorden worden opgeslagen in de vorm van een hash-waarde representatie van het wachtwoord van de gebruiker van de service Active Directory-domein. Een hash-waarde is een gevolg van een wiskundige eenrichtingsfunctie (de '*hash-algoritme*'). Er is geen methode om het resultaat van een unilaterale functie om de versie van een wachtwoord in onbewerkte tekst te herstellen. U kan niet een hash van het wachtwoord kunt aanmelden bij uw netwerk op gebouwen.

Als u uw wachtwoord, haalt sync Azure AD verbinden de wachtwoordhash van uw van Active Directory op de ruimten. Verwerking van extra beveiliging wordt toegepast op de hash van het wachtwoord voordat deze wordt gesynchroniseerd met de verificatieservice van Azure Active Directory. Wachtwoorden worden gesynchroniseerd op per gebruiker en in chronologische volgorde.

De werkelijke overdracht van het synchronisatieproces wachtwoord is vergelijkbaar met de synchronisatie van gebruikersgegevens, zoals de weergavenaam of e-mailadressen. Wachtwoorden worden echter vaker dan de map standard synchronisatievenster voor andere kenmerken gesynchroniseerd. Elke 2 minuten wordt uitgevoerd door het synchronisatieproces wachtwoord. De frequentie van dit proces kunt u niet wijzigen. Bij het synchroniseren van een wachtwoord, overschrijft het bestaande wachtwoord in de cloud.

De eerste keer de functie wachtwoord synchronisatie is ingeschakeld, voert een initiële synchronisatie van wachtwoorden van gebruikers in het bereik. U kunt een subset van wachtwoorden te synchroniseren niet expliciet definiëren.

Wanneer u een wachtwoord in de lokalen, wordt het bijgewerkte wachtwoord gesynchroniseerd, meestal binnen een paar minuten.
De synchronisatiefunctie wachtwoord worden automatisch synchronisatiepogingen mislukte pogingen. Als een fout tijdens een poging om te synchroniseren van een wachtwoord optreedt, wordt een fout in de logboeken vastgelegd.

De synchronisatie van een wachtwoord heeft geen gevolgen voor de gebruiker die momenteel is aangemeld.
De huidige sessie van de cloud-service is niet direct beïnvloed door gesynchroniseerde wachtwoord wijzigen dat optreedt terwijl u bent aangemeld bij een cloud service. Wanneer de cloud-service, moet u opnieuw te identificeren moet u uw nieuwe wachtwoord op te geven.

> [AZURE.NOTE] Wachtwoord synchronisatie wordt alleen ondersteund voor de gebruiker van het type object in Active Directory. Het wordt niet ondersteund voor het type van het object iNetOrgPerson.

### <a name="how-password-synchronization-works-with-azure-ad-domain-services"></a>Hoe werkt Wachtwoordsynchronisatie met Azure AD Domain Services
U kunt ook de functie wachtwoord synchronisatie voor het synchroniseren van uw wachtwoorden op gebouwen op de [Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-overview.md). In dit scenario kunt u de Azure AD Domain Services voor de verificatie van gebruikers in de cloud met alle methoden die beschikbaar zijn in uw bedrijf op AD. De ervaring van dit scenario is vergelijkbaar met het gebruik van Active Directory Migration Tool (ADMT) in een omgeving op gebouwen.

### <a name="security-considerations"></a>Beveiligingsoverwegingen
Bij het synchroniseren van wachtwoorden, wordt de versie met tekst zonder opmaak van uw wachtwoord wordt niet blootgesteld aan de functie wachtwoord synchronisatie naar Azure AD, of een van de bijbehorende services.

Ook behoeft niet in Active Directory in de lokalen voor het opslaan van het wachtwoord in een omkeerbaar gecodeerde vorm. Een overzicht van de Active Directory-wachtwoord-hash wordt gebruikt voor de overdracht tussen de ruimten op AD en Azure Active Directory. De samenvatting van de hash van het wachtwoord worden niet gebruikt voor toegang tot bronnen in uw omgeving op gebouwen.

### <a name="password-policy-considerations"></a>Beleid voor wachtwoorden, overwegingen
Er zijn twee soorten beleid met wachtwoorden die worden beïnvloed door Wachtwoordsynchronisatie inschakelen:

1. Wachtwoordbeleid complexiteit
2. Verloopbeleid wachtwoord

**Wachtwoordbeleid complexiteit**  
Wanneer u wachtwoordsynchronisatie inschakelen, overschrijven de instellingen voor wachtwoordbeleid complexiteit in uw Active Directory op de ruimten complexiteit beleid in de cloud voor gesynchroniseerde gebruikers. Alle geldige wachtwoorden van uw Active Directory op ruimten kunt u toegang krijgen tot AD Azure services.

> [AZURE.NOTE] Wachtwoorden voor gebruikers die zijn gemaakt, rechtstreeks in de cloud worden nog steeds het wachtwoordbeleid als omschreven in de cloud.

**Verloopbeleid wachtwoord**  
Als een gebruiker binnen het bereik van Wachtwoordsynchronisatie is, wordt het wachtwoord van de wolk*Nooit verlopen'*instellen.
U kunt aanmelden bij uw wolk diensten met gesynchroniseerde wachtwoord is verlopen in uw omgeving op lokalen blijven. Uw wachtwoord cloud wordt bijgewerkt zodra die u het wachtwoord in de omgeving van gebouwen wijzigen.

### <a name="overwriting-synchronized-passwords"></a>Overschrijven van wachtwoorden gesynchroniseerd
Een beheerder kan handmatig opnieuw instellen van uw wachtwoord met behulp van Windows PowerShell.

In dit geval het nieuwe wachtwoord het wachtwoord gesynchroniseerd overschrijft en alle wachtwoordbeleid worden gedefinieerd in de wolk worden toegepast op het nieuwe wachtwoord.

Als u uw wachtwoord in de lokalen, het nieuwe wachtwoord wordt gesynchroniseerd met de cloud en overschrijft het wachtwoord handmatig bijgewerkt.

## <a name="enabling-password-synchronization"></a>Wachtwoordsynchronisatie inschakelen
Wachtwoordsynchronisatie wordt automatisch ingeschakeld bij het installeren van Azure AD verbinding maken met de **Express instellingen**. Zie [aan de slag met Azure AD verbinden via een express-instellingen](./connect/active-directory-aadconnect-get-started-express.md)voor meer informatie.

Als u aangepaste instellingen gebruiken bij het installeren van Azure AD verbinden, moet u Wachtwoordsynchronisatie op de gebruikerspagina aanmelden inschakelen. Zie [aangepaste installatie van Azure AD verbinding maken](./connect/active-directory-aadconnect-get-started-custom.md)voor meer informatie.

![Wachtwoordsynchronisatie inschakelen](./media/active-directory-aadconnectsync-implement-password-synchronization/usersignin.png)

### <a name="password-synchronization-and-fips"></a>Synchronisatie van wachtwoorden en FIPS
Als uw server is vergrendeld volgens de Federal Information Processing Standard (FIPS), is vervolgens MD5 uitgeschakeld.

**Om MD5 voor wachtwoordsynchronisatie inschakelen, moet u de volgende stappen uitvoeren:**

1. Ga naar **%programfiles%\Azure AD Sync\Bin**.
2. Open **miiserver.exe.config**.
3. Ga naar het knooppunt **configuratie/runtime** (aan het einde van het bestand).
4. Het volgende knooppunt toevoegen:`<enforceFIPSPolicy enabled="false"/>`
5. Sla uw wijzigingen.

Voor een verwijzing naar is dit knipsel hoe het eruit moet:

```
    <configuration>
        <runtime>
            <enforceFIPSPolicy enabled="false"/>
        </runtime>
    </configuration>
```

Zie voor meer informatie over beveiliging en FIPS [AAD Password-Sync, codering en FIPS-naleving](https://blogs.technet.microsoft.com/enterprisemobility/2014/06/28/aad-password-sync-encryption-and-fips-compliance/)

## <a name="troubleshooting-password-synchronization"></a>Problemen met Wachtwoordsynchronisatie oplossen
Als de wachtwoorden niet kunnen worden gesynchroniseerd als verwacht, is het voor alle gebruikers of voor een subset van gebruikers.

- Als er een probleem met de afzonderlijke objecten, Zie vervolgens [problemen met één object dat wachtwoorden niet worden gesynchroniseerd](#troubleshoot-one-object-that-is-not-synchronizing-passwords).
- Als er een probleem waarbij er geen wachtwoorden worden gesynchroniseerd, Zie [problemen met problemen waar geen wachtwoorden worden gesynchroniseerd](#troubleshoot-issues-where-no-passwords-are-synchronized).

### <a name="troubleshoot-one-object-that-is-not-synchronizing-passwords"></a>Een object dat niet wachtwoorden synchroniseert oplossen
U kunt eenvoudig wachtwoord synchronisatieproblemen oplossen aan de hand van de status van een object.

Start **Active Directory: gebruikers**en Computers. Zoek de gebruiker en controleer of **dat gebruiker moet wachtwoord bij volgende aanmelding wijzigen** is uitgeschakeld.
![Active Directory productieve wachtwoorden](./media/active-directory-aadconnectsync-implement-password-synchronization/adprodpassword.png)  
Als deze optie is ingeschakeld, vraagt u de gebruiker aanmelden en het wachtwoord te wijzigen. Tijdelijke wachtwoorden worden niet gesynchroniseerd met Azure AD.

Als u denkt dat het correct in Active Directory, wordt de volgende stap gaat u als volgt de gebruiker in de synchronisatie-engine. Door de gebruiker uit Active Directory voor ruimten naar Azure AD, ziet u of er een omschrijving van de fout op het object is.

1. Start de **[Service Synchronisatiebeheer](active-directory-aadconnectsync-service-manager-ui.md)**.
2. Klik op **verbindingslijnen**.
3. Selecteer de de gebruiker zich in de **Active Directory Connector** .
4. Selecteer **Search Connector ruimte**.
5. Zoek de gebruiker die u zoekt.
6. Selecteer het tabblad **lineage** en zorg ervoor dat ten minste één regel met Sync **Password Sync** als **de waarde True**geeft. In de standaardconfiguratie, de naam van de regel Sync is **In van AD - AccountEnabled van de gebruiker**.  
    ![Informatie over een gebruiker Lineage](./media/active-directory-aadconnectsync-implement-password-synchronization/cspasswordsync.png)  
7. Vervolgens [voert u de gebruiker](active-directory-aadconnectsync-service-manager-ui-connectors.md#follow-an-object-and-its-data-through-the-system) via de metaverse aan de ruimte Azure AD-Connector. De connector space-object moet een uitgaande regel hebben met **Password Sync** ingesteld op **True**. In de standaardconfiguratie is de naam van de regel sync **Out naar AAD - gebruiker toevoegen**.  
    ![Connector ruimte eigenschappen van een gebruiker](./media/active-directory-aadconnectsync-implement-password-synchronization/cspasswordsync2.png)  
8. Overzicht van de details van de synchronisatie van wachtwoord van het object voor de afgelopen week, klikt u op **logbestand...**.  
    ![Details van object](./media/active-directory-aadconnectsync-implement-password-synchronization/csobjectlog.png)  

De statuskolom kan de volgende waarden hebben:

Status | Beschrijving
---- | -----
Succes | Wachtwoord is gesynchroniseerd.
FilteredByTarget | Wachtwoord is ingesteld op **gebruiker moet wachtwoord bij volgende aanmelding wijzigen**. Wachtwoord is niet gesynchroniseerd.
NoTargetConnection | Geen object in de metaverse of in de ruimte Azure AD-connector.
SourceConnectorNotPresent | Geen object gevonden in de ruimte op de lokalen Active Directory connector.
TargetNotExportedToDirectory | Het object in de ruimte Azure AD connector is nog niet geëxporteerd.
MigratedCheckDetailsForMoreInfo | Logboekvermelding is gemaakt voordat de build 1.0.9125.0 en wordt weergegeven in de oude staat.

### <a name="troubleshoot-issues-where-no-passwords-are-synchronized"></a>Oplossen van problemen waar geen wachtwoorden worden gesynchroniseerd
Start het script wordt uitgevoerd in de sectie [de status van de synchronisatie-instellingen voor wachtwoord ophalen](#get-the-status-of-password-sync-settings). Het biedt u een overzicht van de configuratie van wachtwoord synchronisatie.  
![PowerShell script uitvoer van wachtwoord synchronisatie-instellingen](./media/active-directory-aadconnectsync-implement-password-synchronization/psverifyconfig.png)  
Als de functie is niet ingeschakeld in Azure AD of de synchronisatiestatus van het kanaal niet is ingeschakeld, voert u de wizard verbinding maken installeren. **Aanpassen: opties voor synchronisatie** selecteren en deselecteren wachtwoord synchronisatie. Deze wijziging wordt de functie tijdelijk uitgeschakeld. Vervolgens wordt de wizard opnieuw uitvoeren en sync wachtwoord opnieuw in te schakelen. Voer het script opnieuw om te controleren of de configuratie correct is.

Als u het script ziet dat is er geen hartslag, vervolgens het script uitvoeren in [een volledige synchronisatie van alle wachtwoorden Trigger](#trigger-a-full-sync-of-all-passwords). Dit script kan ook worden gebruikt om andere scenario's waarbij de configuratie correct is, maar wachtwoorden worden niet gesynchroniseerd.

#### <a name="get-the-status-of-password-sync-settings"></a>De status van de synchronisatie-instellingen voor wachtwoord ophalen

```
Import-Module ADSync
$connectors = Get-ADSyncConnector
$aadConnectors = $connectors | Where-Object {$_.SubType -eq "Windows Azure Active Directory (Microsoft)"}
$adConnectors = $connectors | Where-Object {$_.ConnectorTypeName -eq "AD"}
if ($aadConnectors -ne $null -and $adConnectors -ne $null)
{
    if ($aadConnectors.Count -eq 1)
    {
        $features = Get-ADSyncAADCompanyFeature -ConnectorName $aadConnectors[0].Name
        Write-Host
        Write-Host "Password sync feature enabled in your Azure AD directory: "  $features.PasswordHashSync
        foreach ($adConnector in $adConnectors)
        {
            Write-Host
            Write-Host "Password sync channel status BEGIN ------------------------------------------------------- "
            Write-Host
            Get-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector.Name
            Write-Host
            $pingEvents =
                Get-EventLog -LogName "Application" -Source "Directory Synchronization" -InstanceId 654  -After (Get-Date).AddHours(-3) |
                    Where-Object { $_.Message.ToUpperInvariant().Contains($adConnector.Identifier.ToString("D").ToUpperInvariant()) } |
                    Sort-Object { $_.Time } -Descending
            if ($pingEvents -ne $null)
            {
                Write-Host "Latest heart beat event (within last 3 hours). Time " $pingEvents[0].TimeWritten
            }
            else
            {
                Write-Warning "No ping event found within last 3 hours."
            }
            Write-Host
            Write-Host "Password sync channel status END ------------------------------------------------------- "
            Write-Host
        }
    }
    else
    {
        Write-Warning "More than one Azure AD Connectors found. Please update the script to use the appropriate Connector."
    }
}
Write-Host
if ($aadConnectors -eq $null)
{
    Write-Warning "No Azure AD Connector was found."
}
if ($adConnectors -eq $null)
{
    Write-Warning "No AD DS Connector was found."
}
Write-Host
```

#### <a name="trigger-a-full-sync-of-all-passwords"></a>Een volledige synchronisatie van alle wachtwoorden activeren
U kunt een volledige synchronisatie van alle wachtwoorden met het volgende script activeren:

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"
$aadConnector = "<CASE SENSITIVE AAD CONNECTOR NAME>"
Import-Module adsync
$c = Get-ADSyncConnector -Name $adConnector
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1
$c.GlobalParameters.Remove($p.Name)
$c.GlobalParameters.Add($p)
$c = Add-ADSyncConnector -Connector $c
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $false
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $true
```

## <a name="next-steps"></a>Volgende stappen

* [Azure AD verbinding Sync: Synchronisatie-opties aanpassen](active-directory-aadconnectsync-whatis.md)
* [Integratie van uw identiteiten op ruimten met Azure Active Directory](active-directory-aadconnect.md)
