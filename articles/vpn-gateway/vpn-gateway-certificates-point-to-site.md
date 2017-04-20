<properties 
   pageTitle="Zelfondertekende certificaten maken voor punt-tot-Site, virtuele netwerk cross-premises verbindingen via makecert | Microsoft Azure"
   description="Dit artikel bevat stappen voor het gebruik van makecert zelfondertekende certificaten maken op Windows 10."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/22/2016"
   ms.author="cherylmc" />

# <a name="working-with-self-signed-certificates-for-point-to-site-connections"></a>Werken met zelfondertekende certificaten voor punt-naar-Site-verbindingen

Dit artikel helpt u bij het maken van een zelfondertekend certificaat gebruiken **makecert**en genereer vervolgens certificaten uit. De stappen zijn geschreven voor makecert op Windows 10. MakeCert is gevalideerd voor certificaten die compatibel met P2S verbindingen zijn maken. 

Voor P2S verbindingen, verdient de voorkeur voor certificaten gebruiken uw bedrijfsoplossing certificaat, zorg ervoor dat de client om certificaten te verlenen aan de algemene indeling van de waarde 'name@yourdomain.com', in plaats van de 'domeinnaam\gebruikersnaam NetBIOS-notatie.

Als u een bedrijfsoplossing niet hebt, is een zelf-ondertekend certificaat nodig P2S clients verbinding maken met een virtueel netwerk. We zijn op de hoogte dat makecert is afgeschaft, maar nog steeds een geldige methode is voor het maken van zelfondertekende certificaten die compatibel met P2S verbindingen zijn. We een andere oplossing voor het maken van zelfondertekende certificaten ook werken, maar op dit moment makecert verdient de voorkeur.

## <a name="create-a-self-signed-certificate"></a>Een zelfondertekend certificaat maken

MakeCert is één manier om een zelfondertekend certificaat maken. De volgende stappen helpt u bij het maken van een zelfondertekend certificaat met makecert. Deze stappen zijn niet specifiek deployment-model. Ze zijn geldig voor Resource Manager en de klassieke.

### <a name="to-create-a-self-signed-certificate"></a>Een zelfondertekend certificaat maken

1. Vanaf een computer met Windows 10, download en installeer de [Windows Software Development Kit (SDK) voor Windows 10](https://dev.windows.com/en-us/downloads/windows-10-sdk).

2. Na de installatie kunt u het hulpprogramma makecert.exe Dit pad vinden: C:\Program Files (x86) \Windows Kits\10\bin\<arch >. 
        
    Voorbeeld:`C:\Program Files (x86)\Windows Kits\10\bin\x64`

3. Maak vervolgens en installeert u een certificaat in het persoonlijke certificaatarchief op de computer. In het volgende voorbeeld wordt een bijbehorende *CER* -bestand dat u naar Azure uploadt bij het configureren van P2S gemaakt. De volgende opdracht als administrator uitvoeren. *ARMP2SRootCert* en *ARMP2SRootCert.cer* vervangen door de naam die u wilt gebruiken voor het certificaat.<br><br>Het certificaat bevindt zich in uw certificaten - Huidige gebruiker\Persoonlijk\Certificaten.

        makecert -sky exchange -r -n "CN=ARMP2SRootCert" -pe -a sha1 -len 2048 -ss My "ARMP2SRootCert.cer"


###  <a name="rootpublickey"></a>De openbare sleutel ophalen

De openbare sleutel voor het basiscertificaat is geüpload als onderdeel van de configuratie van de VPN-Gateway voor punt-naar-Site-verbindingen naar Azure.

1. Als u een cer-bestand van het certificaat, **certmgr.msc**te openen. Met de rechtermuisknop op het certificaat zelfondertekend root, klik op **alle taken**en klik vervolgens op **exporteren**. Hiermee opent u de **Wizard Certificaat exporteren**.

2. Klik op **volgende**in de Wizard, selecteert u **Nee, de persoonlijke sleutel niet exporteren**en klik op **volgende**.

3. Klik op de pagina **Bestandsindeling voor Export** selecteren **Base-64 encoded X.509 (. (CER).** Klik vervolgens op **volgende**. 

4. Op het **te exporteren bestand**, **Blader** naar de locatie waarnaar u het certificaat exporteren. Naam voor **de naam van**het certificaatbestand. Klik vervolgens op **volgende**.

5. Klik op **Voltooien** om het exporteren van het certificaat.

 
### <a name="export-the-self-signed-certificate-optional"></a>(Optioneel) zelf-ondertekend certificaat exporteren

U kunt zelf-ondertekend certificaat exporteren en veilig opslaan. Als moet worden, u kunt later op een andere computer installeren en meer clientcertificaten genereren of een andere cer-bestand exporteren. Elke computer met een clientcertificaat dat is geïnstalleerd en die is ook geconfigureerd met de juiste VPN-client instellingen verbinding met het virtuele netwerk via P2S maken kunnen. U wilt daarom, om ervoor te zorgen dat certificaten worden gegenereerd en alleen geïnstalleerd wanneer nodig en zelf-ondertekend certificaat veilig opgeslagen.

Om een zelf-ondertekend certificaat exporteren als een .pfx, selecteert u het basiscertificaat en volgt u dezelfde stappen zoals beschreven in het [exporteren van een certificaat](#clientkey) exporteren.

## <a name="create-and-install-client-certificates"></a>Maken en installeren van clientcertificaten

U installeren geen zelf-ondertekend certificaat rechtstreeks op de clientcomputer. U moet een clientcertificaat van een zelf-ondertekend certificaat te genereren. U en vervolgens exporteren en dit certificaat installeren op de clientcomputer. De volgende stappen zijn niet specifiek deployment-model. Ze zijn geldig voor Resource Manager en de klassieke.

### <a name="part-1---generate-a-client-certificate-from-a-self-signed-certificate"></a>Deel 1 – een clientcertificaat van een zelfondertekend certificaat te genereren

De volgende stappen kunt u doorlopen een manier om een clientcertificaat van een zelfondertekend certificaat te genereren. U kunt meerdere clientcertificaten kan genereren uit hetzelfde certificaat. Elk clientcertificaat kan vervolgens worden geëxporteerd en geïnstalleerd op de clientcomputer. 

1. Open een opdrachtprompt als administrator op dezelfde computer waarmee u het zelf-ondertekende certificaat maken.

2. In dit voorbeeld wordt 'ARMP2SRootCert' verwijst naar het zelfondertekend certificaat die u hebt gegenereerd. 
    - *'ARMP2SRootCert'* op de naam van de zelf-ondertekende basis die u het clientcertificaat van genereert wijzigen. 
    - Wijzig *ClientCertificateName* in de naam die u een certificaat wilt te genereren. 


    Wijzigen en voer het voorbeeld voor het genereren van een clientcertificaat. Als u zonder het te wijzigen in het volgende voorbeeld wordt uitgevoerd, is het resultaat een certificaat met de naam ClientCertificateName in uw persoonlijke certificatenarchief die is gegenereerd op basis van het basiscertificaat ARMP2SRootCert.

        makecert.exe -n "CN=ClientCertificateName" -pe -sky exchange -m 96 -ss My -in "ARMP2SRootCert" -is my -a sha1

4. Alle certificaten zijn opgeslagen in uw 'Certificaten - Huidige gebruiker\Persoonlijk\Certificaten' opslaan op uw computer. U kunt meerdere certificaten indien nodig op basis van deze procedure genereren.

### <a name="clientkey"></a>Deel 2 - een clientcertificaat exporteren

1. Als u wilt exporteren een clientcertificaat, **certmgr.msc**te openen. Met de rechtermuisknop op het certificaat dat u wilt exporteren, klikt u op **alle taken**en klik vervolgens op **exporteren**. Hiermee opent u de **Wizard Certificaat exporteren**.

2. Klik op **volgende**in de Wizard en vervolgens selecteert u **Ja, de persoonlijke sleutel exporteren**en klik op **volgende**.

3. Klik op de pagina **Bestandsindeling voor Export** kunt u de standaardinstellingen ingeschakeld laten. Klik vervolgens op **volgende**. 
 
4. Op de pagina **beveiliging** moet u de persoonlijke sleutel te beveiligen. Als u een wachtwoord gebruikt, moet u registreren of het wachtwoord die u hebt ingesteld voor dit certificaat. Klik vervolgens op **volgende**.

5. Op het **te exporteren bestand**, **Blader** naar de locatie waarnaar u het certificaat exporteren. Naam voor **de naam van**het certificaatbestand. Klik vervolgens op **volgende**.

6. Klik op **Voltooien** om het exporteren van het certificaat.  

### <a name="part-3---install-a-client-certificate"></a>Deel 3 – een certificaat installeren

Elke client die u verbinden met het virtuele netwerk wilt met een punt-naar-Site verbinding moet een certificaat geïnstalleerd hebben. Dit certificaat is het vereiste pakket voor VPN-configuratie. De volgende stappen wordt u stapsgewijs door het certificaat handmatig installeren.

1. Zoek en het *.pfx* -bestand kopiëren naar de clientcomputer. Dubbelklik op het *PFX* -bestand te installeren op de clientcomputer. Laat u de **Locatie** als de **Huidige gebruiker**en klik op **volgende**.

2. Op het **bestand** te importeren pagina, niet wijzigen. Klik op **volgende**.

3. Klik op de pagina **beveiliging van persoonlijke sleutel** het wachtwoord voor het certificaat worden ingevoerd als u gebruikt, of Controleer of de beveiligings-principal die het installeren van het certificaat juist is en klik op **volgende**.

4. Op de pagina **Certificaatarchief** de standaardlocatie laten staan en klik op **volgende**.

5. Klik op **Voltooien**. Op de **Beveiligingswaarschuwing** voor installatie van het certificaat, klikt u op **Ja**. Het certificaat wordt nu geïmporteerd.

## <a name="next-steps"></a>Volgende stappen

Doorgaan met de configuratie van punt-naar-Site. 

- **Resource Manager** distributie model Zie [configureren een punt-naar-Site verbinding met een VNet met PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md). 
- **Klassieke** deployment model Zie [configureren een Point-to-Site VPN-verbinding met een VNet met de klassieke portal](vpn-gateway-point-to-site-create.md).