
<properties 
    pageTitle="Opties voor de migratie van Azure RemoteApp | Microsoft Azure" 
    description="Meer informatie over de opties voor de migratie van Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/06/2016" 
    ms.author="elizapo" />

# <a name="options-for-migrating-out-of-azure-remoteapp"></a>Opties voor de migratie van Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt stopgezet. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Als u gestopt Azure RemoteApp door de [aankondiging van de pensioen](https://go.microsoft.com/fwlink/?linkid=821148) -of omdat u uw beoordeling hebt voltooid, moet u van Azure RemoteApp migreren naar een andere app-service. Er zijn twee verschillende benaderingen voor de migratie: een zelfbeheerde (infrastructuur wordt vaak genoemd als een Service [IaaS]) implementatie of een volledig beheerde (vaak genoemd Platform als Service) of de Software als een Service [PaaS/SaaS] bieden. 

IaaS zelf is een doe-implementatie die wordt beheerd, geëxploiteerd en die eigendom zijn van u rechtstreeks op virtuele machines (VMs) of fysieke systemen geïmplementeerd. Aan de andere kant een volledig beheerde PaaS/SaaS biedt is vergelijkbaar met een Azure RemoteApp - partner met een laag service op een oplossing voor externe toegang dat zorgt voor operationele en onderhoud, terwijl u, als de klant, worden sommige afbeeldingen en app beheer.

Lees verder voor meer informatie en voorbeelden van de verschillende opties voor hosting.    

## <a name="self-managed-iaas-solutions"></a>Zelf beheerde (IaaS)-oplossingen

### <a name="rds-on-iaas"></a>**RDS op IaaS** 
U kunt een eigen sessie implementatie op basis van extern bureaublad-Services (in Windows Server) met behulp van RemoteApp- en desktopcomputers op-premises implementeren of in een gehoste omgeving (like op Azure VMs). RDS op IaaS implementaties zijn het meest geschikt voor klanten die bekend zijn met en waarvan de bestaande technische expertise met RDS-implementaties. 

> [AZURE.NOTE]
> U moet het Volume Licensing met Software Assurance (SA) voor licenties voor clienttoegang RDS gebruiken deze optie.

Implementatie van RDS in Azure VMs is eenvoudiger dan ooit wanneer u met de implementatie en het corrigeren van sjablonen (Lees een [Overzicht](https://blogs.technet.microsoft.com/enterprisemobility/2015/07/13/azure-resource-manager-template-for-rds-deployment/) en vervolgens [gaat ze](https://aka.ms/rdautomation)). Hoewel er meer aanpassingen en configuraties, kunt u met behulp van de [Automatische schaling script](https://gallery.technet.microsoft.com/scriptcenter/Automatic-Scaling-of-9b4f5e76)elastische schaal mogelijkheden met Azure klassieke model implementatiebronnen (geen Model met Azure resources) in Azure RemoteApp krijgen. Als u RDS op Azure VMs implementeert, is ondersteuning beschikbaar via [Azure ondersteunen](https://azure.microsoft.com/support/plans/), de dezelfde ondersteuningsmedewerkers die u met RemoteApp-Azure ondersteund. U kunt ophalen kostenramingen op basis van uw bestaande gebruik contact opnemen met [Ondersteuning van Azure](https://azure.microsoft.com/support/plans/)of kunt u berekeningen uitvoeren zelf met een spoedig worden vrijgegeven kostencalculator.  Met de N-reeks VMs (momenteel in privé preview) kunt u ook vGPU - toevoegen hoort meer over het toevoegen van vGPU en het [RDS-verbeteringen in Windows Server 2016 gebruik](https://myignite.microsoft.com/videos/2794) te maken van onze Ignite-sessie.   

We hebben een stapsgewijze implementatiehandleidingen voor [Windows Server 2012 R2](http://aka.ms/rdsonazure) en [Windows Server 2016](http://aka.ms/rdsonazure2016) om te helpen bij de implementatie. Bekijk de [Extern bureaublad-blog](https://blogs.technet.microsoft.com/enterprisemobility/?product=windows-server-remote-desktop-services) voor het laatste nieuws.
 
### <a name="citrix-on-iaas"></a>**Citrix op IaaS** 
Een native Citrix implementatie van XenApp op basis van een sessie of XenDesktop kan worden geïmplementeerd in ruimten of binnen een gehoste omgeving (zoals op Azure VMs). 

Raadpleeg de stapsgewijze implementatiehandleiding, [Citrix XA 7.6 op Azure](http://www.citrixandmicrosoft.com/Documents/Citrix-Azure Deployment Guide-v.1.0.docx), voor meer informatie. Lees meer over [Citrix op Azure](http://www.citrixandmicrosoft.com/Solutions/AzureCloud.aspx), met inbegrip van een rekenmachine voor prijs. Ook vindt u een [Citrix contact](http://citrix.com/English/contact/index.asp) te bespreken de opties met.

## <a name="fully-managed-paassaas-offerings"></a>Volledig beheerde (PaaS/SaaS)-aanbiedingen

### <a name="citrix-cloud"></a>**Citrix Cloud** 
[Citrix bestaande cloud oplossing](https://www.citrix.com/products/citrix-cloud/), conceptueel dezelfde Citrix XenApp Express. Citrix biedt een [promotie van 50% korting](https://www.citrix.com/blogs/2016/10/03/special-promotion-for-microsoft-azure-remoteapp-customers/) voor bestaande klanten die Azure RemoteApp. 

### <a name="citrix-xenapp-express-in-tech-preview"></a>**Citrix XenApp Express (in de tech preview)**
[Registreren voor de tech preview](http://now.citrix.com/remoteapp), en bekijk de [Ignite-sessies](https://myignite.microsoft.com/videos/2792) (vanaf 20:30 minuut). XenApp Express is conceptueel gelijk aan Citrix wolk behalve omvat vereenvoudigd beheer UI en andere functies en mogelijkheden die vergelijkbaar met Azure RemoteApp zijn. 

Meer informatie over de [Citrix XenApp Express](http://now.citrix.com/remoteapp).   

### <a name="citrix-service-provider-program"></a>**Citrix Service Provider-programma** 
Citrix Service Provider wordt eenvoudig voor serviceproviders leveren de eenvoud van virtuele cloud computing met SMB's, bieden ze de diensten die ze in een eenvoudige, "pay-as-you-go"-model willen. Citrix-serviceproviders groeien hun bedrijven Microsoft SPLA en hun investeringen RDS platform uitbreiden met een apparaat, overal toegang tot, de meest uitgebreide ondersteuning voor toepassingen, een rijke ervaring, beveiliging en verbeterde schaalbaarheid. Op zijn beurt Citrix-aanbieders trekken meer abonnees, de klanttevredenheid verhogen en hun operationele kosten te verlagen. [Meer informatie](http://www.citrix.com/products/service-providers.html) of [een partner zoeken](https://www.citrix.com/buy/partnerlocator.html).

### <a name="microsoft-hosted-service-provider"></a>**Microsoft gehoste serviceprovider** 
Hosting-partners bieden meestal een volledig beheerde Windows-bureaublad gehost en application service, waaronder de Azure resources, besturingssystemen, toepassingen en de partner-helpdesk beheren de licentieovereenkomsten met Microsoft en andere softwareleveranciers niet alleen een gebruiksrechtovereenkomst voor Service Provider om directe verkoop van abonnee toegang licentie (SAL). De volgende gegevens bevatten details en contactgegevens voor enkele van de hosters die gespecialiseerd zijn in hun migratie Azure RemoteApp-klanten te helpen. Bekijk [de huidige lijst met serviceproviders die worden gehost](http://aka.ms/rdsonazurecertified) die de RDS op IaaS leren en de evaluatie is voltooid.  

#### <a name="aspex"></a>**ASPEX**
[ASPEX](http://www.aspex.be/en) is gespecialiseerd in de ISV's overstappen naar de Cloud en ISV' te optimaliseren, hun huidige instellingen van de wolk. ASPEX biedt een breed scala van beheerde services, devops en adviesdiensten.  

Primaire locatie: Antwerpen, België

Bewerkingsregio: West-Europa

Partner-status: [Zilver](https://partnercenter.microsoft.com/pcv/solution-providers/aspex_9397f5dd-ebdd-405b-b926-19a5bda61f7a/cfe00bac-ea36-4591-a60b-ec001c4c3dff)

Microsoft Cloud serviceprovider: Ja

RemoteApp- en bureaubladverbindingen sessie-oplossingen bieden: Ja, beide

Azure RemoteApp-migratie oplossingen: Ja, [meer informatie](https://www.aspex.be/en/azure-remote-apps)

**Contactpersoon:**

- Telefoon: +3232202198
- E-mail:[info@aspex.be](mailto:info@aspex.be)
- Web: [http://cloud.aspex.be/contact-ara-0](http://cloud.aspex.be/contact-ara-0)

#### <a name="conexlink-platform-name-mycloudit"></a>**Conexlink (de naam van het Platform: MyCloudIT)**
[MyCloudIT](http://www.mycloudit.com) is een platform voor automatisering van IT-bedrijven te vereenvoudigen, te optimaliseren en de schaal van migratie en de bezorging van externe bureaubladen, externe toepassingen en infrastructuur in het Microsoft Azure Cloud. 

Het platform MyCloudIT distributietijd 95%, Azure kosten met 30%, en de hele IT-infrastructuur van de client worden verplaatst naar de cloud in een kwestie van enkele toetsaanslagen. Partners kunnen klanten nu beheren vanuit één globale dashboard, eindgebruikers over de hele wereld als nooit tevoren en inkomsten groeien zonder extra overhead of uitgebreide Azure training toe te voegen.  

Primaire locatie: Dallas, TX, Verenigde Staten

Bewerkingsregio: wereldwijd

Partner-status: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/conexlink_4298787366/843036_1?k=Conexlink)

Microsoft Cloud serviceprovider: Ja

RemoteApp- en bureaubladverbindingen sessie-oplossingen bieden: Ja, beide

Azure RemoteApp-migratie oplossingen: Ja, [meer informatie](https://mycloudit.com/remote-app-microsoft/)

**Contactpersoon:**
- Brian Garoutte, Adjunct-Directeur Business Development

   Telefoon: 972-218-0741

   E-mail:[brian.garoutte@conexlink.com](mailto:brian.garoutte@conexlink.com)

#### <a name="acuutech"></a>**Acuutech**
[Acuutech](http://www.acuutech.com) is gespecialiseerd in het gehoste-desktopoplossingen bieden volledige bureaublad en ISV-toepassingen bieden ervaringen gebaseerd op Microsoft-technologie om een algemene basis client van Azure en hun eigen datacenters.

Primaire locatie: Londen, UK; Singapore; Houston, TX

Bewerkingsregio: wereldwijd

Partner-status: Gold

Microsoft Cloud serviceprovider: Ja

RemoteApp- en bureaubladverbindingen sessie-oplossingen bieden: Ja, beide

Azure RemoteApp-migratie oplossingen: Ja, [meer informatie](http://www.acuutech.com/ara-migration/)

**Contactpersoon:**

- Verenigd Koninkrijk:

  5/6 York House, Langston weg,

  Loughton, Essex IG10 3TQ
  
  Telefoon: + 44 (0) 20 8502 2155
 
- Singapore:

  100 Cecil Street, #09-02 
  
  De hele wereld, Singapore, 069532
  
  Telefoon: + 65 6709 4933
 
- Noord-Amerika: 

  3601 S. Sandman St.
  
  Suite-200, Houston, TX 77098
  
  Telefoon: +1 713 691 0800

## <a name="need-more-help"></a>Meer hulp nodig?
Toch moet helpen kiezen of nog verdere vragen hebt? Een van de volgende methoden gebruiken om Help-informatie opvragen. 

1.  Een e-mail sturen naar [arainfo@microsoft.com](mailto:arainfo@microsoft.com).
2.  Neem contact op met [ondersteuning van Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Start door het openen van een [Azure geval ondersteunen](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
3.  Bel ons. [Zoeken naar een lokaal nummer van de verkoop](https://azure.microsoft.com/overview/sales-number/).
