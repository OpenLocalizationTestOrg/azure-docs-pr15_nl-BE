<properties 
    pageTitle="Een verbinding van een Azure Search indexer naar SQL Server configureren op een Azure virtuele machine | Microsoft Azure | Indexeerfuncties" 
    description="Versleutelde verbindingen inschakelen en configureren van de firewall voor verbindingen met SQL Server op een Azure virtual machine (VM) van de indexeerfunctie van Azure zoeken." 
    services="search" 
    documentationCenter="" 
    authors="jack4it" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="09/26/2016" 
    ms.author="jackma"/>

# <a name="configure-a-connection-from-an-azure-search-indexer-to-sql-server-on-an-azure-vm"></a>Een verbinding van een Azure Search indexer naar SQL Server configureren op een Azure VM

Zoals opgemerkt in [Azure SQL-Database verbinding Azure zoeken met behulp van indexeerfuncties](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md#frequently-asked-questions), maken indexeerfuncties tegen **SQL Server op Azure VMs** (of **SQL Azure VMs** voor korte) wordt ondersteund door Azure zoeken, maar er zijn een paar beveiligingsgerelateerde vereisten om te zorgen voor de eerste. 

**Taak duur:** Ongeveer 30 minuten, ervan uitgaande dat u al een certificaat geïnstalleerd op de VM.

## <a name="enable-encrypted-connections"></a>Versleutelde verbindingen inschakelen

Azure Search is gecodeerd kanaal vereist voor alle indexeerfunctie aanvragen via een openbare internetverbinding. In deze sectie worden de stappen voor het maken van dit werk.

1. Controleer de eigenschappen van het certificaat om te controleren of de dat naam van de certificaathouder is de volledig gekwalificeerde domeinnaam (FQDN) van Azure VM. U kunt een hulpprogramma zoals CertUtils of de module Certificaten om de eigenschappen weer te geven. Krijgt u de FQDN-naam van de VM service blade Essentials sectie in het veld **openbare IP-adres of DNS-naam label** in de [portal Azure](https://portal.azure.com/).

    - Voor VMs gemaakt met de nieuwe **Resource Manager** -sjabloon, de FQDN-naam is opgemaakt als `<your-VM-name>.<region>.cloudapp.azure.com`. 

    - Voor oudere VMs gemaakt als een **klassieke** VM, de FQDN-naam is opgemaakt als `<your-cloud-service-name.cloudapp.net>`. 

2. SQL Server configureren om gebruik van het certificaat met behulp van de Register-Editor (regedit). 

    Hoewel SQL Server Configuration Manager vaak voor deze taak gebruikt wordt, kunt u deze niet gebruiken voor dit scenario. Het vindt niet het geïmporteerde certificaat omdat de FQDN van de VM in Azure niet overeenkomt met de FQDN-naam bepaald door de VM (verwijst naar het domein als de lokale computer of het netwerkdomein waarvan deze lid is). Als de namen niet overeenkomen, gebruikt u regedit om op te geven van het certificaat.

    - In regedit, bladert u naar deze registersleutel: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\[MSSQL13.MSSQLSERVER]\MSSQLServer\SuperSocketNetLib\Certificate`.
     
    De `[MSSQL13.MSSQLSERVER]` onderdeel is afhankelijk van de versie-en exemplaarnaam. 

    - Stel de waarde van de sleutel van het **certificaat** aan de **vingerafdruk** van het SSL-certificaat dat u hebt geïmporteerd voor VM.

    Er zijn verschillende manieren om de vingerafdruk, sommige beter dan andere. Als u deze vanuit de module **certificaten** in MMC kopieert, zal u waarschijnlijk een onzichtbaar toonaangevende teken [zoals beschreven in dit artikel](https://support.microsoft.com/kb/2023869/), die in een fout resulteert wanneer u een verbinding probeert kiezen. Er bestaan verschillende oplossingen voor het oplossen van dit probleem. De eenvoudigste manier is via backspace en typ nogmaals het eerste teken van de vingerafdruk van het eerste teken in het veld waarde in regedit te verwijderen. Ook kunt u een ander gereedschap om te kopiëren van de vingerafdruk.

3. Machtigingen verlenen voor de service-account. 

    Controleer of de dat juiste machtigingen voor de persoonlijke sleutel van het SSL-certificaat wordt verleend door de SQL Server-serviceaccount. Als u deze stap laten corrigeren, kan SQL Server niet worden gestart. U kunt de module **certificaten** of **CertUtils** gebruiken voor deze taak.

4. De SQL Server-service opnieuw starten.

## <a name="configure-sql-server-connectivity-in-the-vm"></a>SQL Server-verbinding configureren in de VM

Na het instellen van de gecodeerde verbinding vereist door Azure zoeken, zijn er extra configuratiestappen worden uitgevoerd met SQL Server op Azure VMs intrinsieke. Als u nog niet hebt gedaan, wordt de volgende stap is om met behulp van een van deze artikelen configuratie te voltooien:

- Zie [verbinding maken met een SQL Server virtuele Machine op Azure Resource Manager gebruiken](../virtual-machines/virtual-machines-windows-sql-connect.md)voor een **Resource Manager** VM. 

- Zie voor een **klassieke** VM, [verbinding maken met een virtuele Machine op klassieke Azure in SQL Server](../virtual-machines/virtual-machines-windows-classic-sql-connect.md).

Bekijk in het bijzonder de sectie in elk artikel voor "verbinding maken via het internet".

## <a name="configure-the-network-security-group-nsg"></a>Configureer het netwerk Security Group (NSG)

Het is niet ongebruikelijk voor het configureren van de NSG en bijbehorende Azure eindpunt of lijst ACL (Access Control) om uw Azure VM toegankelijk maken voor andere partijen. Waarschijnlijk hebben dat hebt u dit nog gedaan om uw eigen toepassingslogica verbinding maken met uw SQL Azure VM. Het is niet anders voor een verbinding Azure zoeken voor uw SQL Azure VM. 

De onderstaande koppelingen geven instructies over de configuratie van NSG voor VM-implementaties. Gebruik deze instructies aan ACL een Azure SEarch eindpunt op basis van het IP-adres.

> [AZURE.NOTE] Zie voor achtergrondinformatie, [Wat is een netwerk-beveiligingsgroep?](../virtual-network/virtual-networks-nsg.md)

- Zie voor een **Resource Manager** VM, [het maken van NSGs voor ARM-implementaties](../virtual-network/virtual-networks-create-nsg-arm-pportal.md). 

- Zie voor een **klassieke** VM, [het maken van NSGs voor klassieke implementaties](../virtual-network/virtual-networks-create-nsg-classic-ps.md).

IP-adressen kan opleveren voor een aantal uitdagingen die gemakkelijk te overwinnen zijn als u op de hoogte van het probleem en mogelijke oplossingen. De resterende secties vindt u aanbevelingen voor het afhandelen van problemen met betrekking tot IP-adressen in de ACL.

#### <a name="restrict-access-to-the-search-service-ip-address"></a>Beperk de toegang tot het IP-adres van de zoekopdracht

Het wordt aangeraden dat u de toegang beperken tot het IP-adres van de zoekservice in de ACL in plaats van uw SQL Azure VMs wijd open voor verbindingsaanvragen. U kunt het IP-adres gemakkelijk uitzoeken de FQDN-naam pingen (bijvoorbeeld, `<your-search-service-name>.search.windows.net`) van de zoekservice.

#### <a name="managing-ip-address-fluctuations"></a>Beheer van IP-adres schommelingen

Als uw search-service heeft slechts één zoeken eenheid (dat wil zeggen, een replica en een partitie), verandert het IP-adres tijdens een routine wordt gestart, dat u een bestaande ACL met IP-adres van de zoekservice ongeldig.

Een manier om te voorkomen dat de volgende ODBC-fout is met meer dan één replica en één partitie in Azure zoeken. Waardoor de kosten doet toenemen, maar het is ook het IP-adres probleem opgelost. Niet in Azure zoeken wijzigen IP-adressen als er meer dan één eenheid van het zoeken.

Een tweede aanpak is om de verbinding mislukken en vervolgens de ACL's in de NSG opnieuw te configureren. Gemiddeld, kunt u IP-adressen te wijzigen om de paar weken verwachten. Voor klanten die beheerde indexering op incidentele basis, kan deze aanpak haalbaar zijn.

Een derde benadering van levensvatbare (maar niet bijzonder veilig) is geeft u het IP-adresbereik van het Azure regio waar de zoekservice is ingericht. De lijst met IP-adresbereiken waarvoor openbare IP-adressen worden toegewezen aan resources Azure is [Azure Datacenter IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653)gepubliceerd. 

#### <a name="include-the-azure-search-portal-ip-addresses"></a>Zijn de IP-adressen van Azure Search portal

Als u de Azure portal voor het maken van een indexeerfunctie moet Azure Search portal logica ook toegang tot uw SQL Azure VM tijdens het maken. Azure search portal IP-adressen kunnen u vinden door de opdracht ping `stamp2.search.ext.azure.com`.

## <a name="next-steps"></a>Volgende stappen

Met configuratie van de weg, kunt u nu een SQL-Server opgeven op Azure VM als gegevensbron voor een Azure Search indexer. [Azure SQL-Database verbinding Azure zoeken met behulp van indexeerfuncties](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md) Zie voor meer informatie.
