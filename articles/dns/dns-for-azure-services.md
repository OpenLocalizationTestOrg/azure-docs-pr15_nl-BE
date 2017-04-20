<properties
  pageTitle="Azure DNS gebruiken met andere Azure services | Microsoft Azure"
  description="Wat is Azure DNS naam voor andere Azure services gebruiken?"
  services="dns"
  documentationCenter="na"
  authors="sdwheeler"
  manager="carmonm"
  editor=""
  tags="azure dns"
/>
<tags
  ms.service="dns"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
  ms.date="09/21/2016"
  ms.author="sewhee"
/>

# <a name="using-azure-dns-with-other-azure-services"></a>Azure DNS gebruiken met andere Azure services

Azure DNS is een gehoste DNS-beheer- en service voor naamomzetting. Hiermee kunt u voor het maken van openbare DNS-namen voor andere toepassingen en services die u hebt geïmplementeerd in Azure. Het is net zo eenvoudig als het toevoegen van een record van het juiste type voor uw service maken een naam voor een Azure service in uw aangepaste domein.

* Voor een dynamisch toegewezen IP-adressen, moet u een DNS CNAME-record die is toegewezen aan de DNS-naam die Azure voor uw service gemaakt. DNS-standaarden te voorkomen dat u met behulp van een CNAME-record voor de zone-top.
* Voor een statisch toegewezen IP-adressen, kunt u een DNS-A-record met een naam, inclusief de naam van een _Open domein_ op de top van de zone.

De volgende tabel geeft een overzicht van de ondersteunde typen die kunnen worden gebruikt voor verschillende Azure services. Als u in deze tabel zien kunt, ondersteunt Azure DNS alleen DNS-records voor internetgerichte netwerkbronnen. Azure DNS kan niet worden gebruikt voor naamomzetting van interne, particuliere adressen.

| Azure Service | Netwerk-Interface | Beschrijving |
|---------------|-------------------|-------------|
| Application Gateway | Front-end openbare IP | U kunt een DNS A of CNAME-record maken. |
| Taakverdeling | Front-end openbare IP | U kunt een DNS A of CNAME-record maken. Taakverdeling kan hebben een IPv6-openbare IP-adres dynamisch is toegewezen. Daarom moet u een CNAME-record voor een IPv6-adres. |
| Beheer van netwerkverkeer | Openbare naam | U kunt alleen een CNAME die is toegewezen aan de naam van de trafficmanager.net toegewezen aan uw serviceprofiel verkeer maken. Zie [hoe verkeer Manager werkt](../traffic-manager/traffic-manager-how-traffic-manager-works.md#traffic-manager-example)voor meer informatie. |
| Cloud-Service | Openbare IP | Voor een statisch toegewezen IP-adressen, kunt u een DNS-A-record maken. Voor een dynamisch toegewezen IP-adressen, moet u een CNAME-record die naar de naam _cloudapp.net verwijst_ . Dit geldt ook voor VMs in de klassieke portal gemaakt omdat ze worden geïmplementeerd als een cloud-service. Zie [een aangepaste domeinnaam in een Cloud-Services configureren](../cloud-services/cloud-services-custom-domain-name-portal.md)voor meer informatie. |
| App-Service | Externe IP-adres | U kunt een DNS-A-record maken voor externe IP-adressen. Anders moet u een CNAME-record die naar de naam azurewebsites.net verwijst. Zie voor meer informatie, [een aangepaste domeinnaam aan een Azure app toewijzen](../app-service-web/web-sites-custom-domain-name.md) |
| VMs Resource Manager | Openbare IP | VMs Resource Manager kunnen openbare IP-adressen hebben. Een VM met een openbaar IP-adres kan ook worden achter een taakverdeling. U kunt een DNS A of CNAME-record voor het openbare adres maken. Deze aangepaste naam kan worden gebruikt voor de VIP op de taakverdeling overslaan. |
| Klassieke VMs | Openbare IP | Klassieke VMs gemaakt met PowerShell of CLI kan worden geconfigureerd met een dynamische of statische (gereserveerd) virtueel adres. U kunt een CNAME DNS of record, respectievelijk maken. |
