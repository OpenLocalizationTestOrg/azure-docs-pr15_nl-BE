<properties 
    pageTitle="Cloud service management taken (klassiek) | Microsoft Azure" 
    description="Informatie over het beheren van cloud services in de klassieke Azure portal." 
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





# <a name="how-to-manage-cloud-services"></a>Cloud-Services beheren

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-how-to-manage-portal.md)
- [Azure klassieke portal](cloud-services-how-to-manage.md)

In het gebied van de **Cloud Services** van de klassieke Azure portal, kunt u een service of een implementatie bijwerken, bevordering van een gefaseerde implementatie voor de productie, resources koppelen aan uw service cloud zodat Zie de bronafhankelijkheden en samen de resources aanpassen en verwijderen van een cloud-service of een implementatie.


## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Procedure: werken met een cloud service rol of implementatie

Als de toepassingscode voor uw cloud-service, moet u gebruik **bijwerken** op het dashboard, **Cloud Services** pagina of pagina **exemplaren** . U kunt een enkele of alle rollen bijwerken. U moet een nieuwe servicepakket en service-configuratiebestand uploaden.

1. Klik op **bijwerken**in het [Azure klassieke portal](https://manage.windowsazure.com/)op het dashboard, **Cloud Services** pagina of pagina **exemplaren** .

    ![UpdateDeployment](./media/cloud-services-how-to-manage/CloudServices_UpdateDeployment.png)

2. Geef een naam op voor de installatie (bijvoorbeeld mycloudservice4) in **label implementatie**. Op het dashboard vindt u het label voor distributie onder **Snel starten** .

3. In het **pakket**, **Bladeren** gebruikt om te uploaden bestand (.cspkg) voor het pakket.

4. In een **configuratie**op **Bladeren klikken** voor het uploaden van het bestand van de configuratie (.cscfg).

5. **Rol**, selecteer **Alles** in als u wilt upgraden van alle rollen in de cloud-service. Een enkele rol als update wilt uitvoeren, selecteert u de rol die u wilt bijwerken. Zelfs als u een specifieke rol bij te werken, worden de updates in het configuratiebestand van de service worden toegepast op alle rollen.

6. Als de update het aantal rollen of de grootte van een functie verandert, selecteert u het selectievakje **toestaan bijwerken als rol formaat of het aantal rollen is gewijzigd** om de update te gaan. 

    Let op als u de grootte van een rol (de grootte van een virtuele machine die fungeert als host voor een exemplaar van de rol) of het aantal rollen, elk exemplaar rol (virtual machine) opnieuw afbeelding moet en lokale gegevens niet verloren.

7. Als service rollen slechts één exemplaar van de rol hebben, selecteert u het **zelfs als een of meer rollen bevatten een selectievakje één exemplaar bijwerken** zodat de upgradeprocedure kan worden voortgezet. 

    Azure alleen garantie 99.95% beschikbaarheid van de service tijdens het bijwerken van een cloud-service als elke rol ten minste twee exemplaren van rol (virtuele machines heeft). Waarmee een virtuele machine voor het verwerken van clientaanvragen terwijl het andere wordt bijgewerkt.

8. Klik op **OK** (vinkje) als u wilt beginnen met het bijwerken van de service.



## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Procedure: wisselen implementaties ter bevordering van een gefaseerde implementatie voor de productie

Gebruik **wisselen** ter bevordering van een staging-implementatie van een cloud-service voor de productie. Wanneer u een nieuwe versie van een cloud-service implementeert besluit, kunt u fase en testen van uw nieuwe versie in uw testomgeving van cloud-service terwijl uw klanten met de huidige versie in de productie. Wanneer u nu de nieuwe versie aan productie te bevorderen, kunt u **wisselen** om over te schakelen van de URL's waarmee de twee implementaties zijn gericht. 

U kunt omwisselen implementaties van de pagina **Cloud Services** of het dashboard.

1. Klik in de [Azure klassieke portal](https://manage.windowsazure.com/) **Cloud Services**.

2. Klik op de cloud-service om deze te selecteren in de lijst van cloud diensten.

3. Klik op **omwisselen**.

    Het volgende bevestigingsbericht wordt geopend.

    ![Cloud Services Swap](./media/cloud-services-how-to-manage/CloudServices_Swap.png)

4. Na het controleren van de informatie over implementatie klikt u op **Ja** om te wisselen de implementaties.

    De swap implementatie gebeurt snel omdat het enige wat verandert, de virtuele IP-adressen (VIP's is) voor de implementatie.

    Als u wilt berekenen kosten besparen, kunt u de implementatie in de testomgeving verwijderen als u zeker weet dat de nieuwe productie-implementatie wordt uitgevoerd zoals verwacht.

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Procedure: een resource koppelen aan een cloud-service

Zodat uw wolk van service afhankelijkheden van andere bronnen, kunt u een exemplaar Azure SQL-Database of een opslag-account koppelen aan de cloud-service. Koppelen en ontkoppelen van bronnen op de pagina **Gekoppelde bronnen** , en vervolgens hun gebruik in het dashboard cloud-service te controleren. Als een account gekoppelde opslag controleren ingeschakeld, kunt u totaal aantal aanvragen op het dashboard cloud-service controleren.

**Koppeling** naar een nieuwe of bestaande SQL-Database-instantie of opslag-account koppelen aan uw cloud-service gebruiken. Vervolgens kunt u de database samen met de rol van cloud-service die wordt gebruikt op de pagina **schaal** schalen. (Een account opslag wordt geschaald automatisch als het gebruik toeneemt.) Zie voor meer informatie [hoe u de schaal van een Cloud-Service en de gekoppelde bronnen](cloud-services-how-to-scale.md). 

U kunt ook bewaken, beheren en schalen van de database in het knooppunt **Databases** van de klassieke Azure portal. 

'Koppelen' van een resource in dit opzicht verbinding niet uw app met de bron. Als u een nieuwe database met de **koppeling**maakt, moet u de verbindingsreeksen toevoegen aan uw toepassingscode en vervolgens een upgrade de cloud-service. Ook moet u verbindingsreeksen toevoegen als uw app bronnen worden gebruikt in een opslag van gekoppelde account.

De volgende procedure wordt beschreven hoe u koppelt een nieuw exemplaar van SQL-Database, op een nieuwe server SQL-Database naar een cloud-service geïmplementeerd.

### <a name="to-link-a-sql-database-instance-to-a-cloud-service"></a>Een exemplaar van de SQL-Database koppelen aan een cloud-service

1. Klik in de [Azure klassieke portal](http://manage.windowsazure.com/) **Cloud Services**. Klik vervolgens op de naam van de cloud-service voor het openen van het dashboard.

2. Klik op **gekoppelde bronnen**.

    De **Gekoppelde** pagina wordt geopend.

    ![LinkedResourcesPage](./media/cloud-services-how-to-manage/CloudServices_LinkedResourcesPage.png)

3. Klik op de **koppeling van een Resource** of **koppeling**.

    De wizard **Koppeling bron** wordt gestart.

    ![Link pagina 1](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkPage1.png)

4. Klik op **een nieuwe bron maken** of **een bestaande bron Link**.

5. Kies het type resource wilt koppelen. Klik op **SQL-Database**in de [Azure klassieke portal](http://manage.windowsazure.com/). (Het voorbeeld Azure klassieke portal ondersteunt geen een opslag-account koppelen aan een cloud service.)

6. Voor het voltooien van de databaseconfiguratie, voert u de instructies in de help voor het gebied van **SQL-Databases** van de klassieke Azure portal.

    U kunt de voortgang van de gekoppelde bewerking in het berichtgebied op te volgen.

    ![Koppeling voortgang](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkProgress.png)

    Wanneer het koppelen is voltooid, kunt u de status van de gekoppelde bron op het dashboard cloud-service controleren. Zie voor informatie over het schalen van een gekoppelde SQL-Database, [de schaal van een Cloud-Service en de gekoppelde bronnen](cloud-services-how-to-scale.md).

### <a name="to-unlink-a-linked-resource"></a>Een gekoppelde bron ontkoppelen

1. Klik in de [Azure klassieke portal](http://manage.windowsazure.com/) **Cloud Services**. Klik vervolgens op de naam van de cloud-service voor het openen van het dashboard.

2. Klik op **Gekoppelde bronnen**en selecteer vervolgens de resource.

3. Klik op **koppeling verbreken**. **Klik op de bevestiging wordt gevraagd.**

    Ontkoppelen van een SQL-Database heeft geen invloed op de database of verbindingen met de database van de toepassing. U kunt nog steeds de database in de **SQL-Databases** van de klassieke Azure portal beheren.



## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Procedure: implementaties en een cloud-service verwijderen

Voordat u een service cloud verwijderen kunt, moet u elke bestaande installatie verwijderen.

Als u wilt berekenen kosten besparen, kunt u uw implementatie staging verwijderen nadat u hebt gecontroleerd dat de productie-implementatie werkt zoals verwacht. Compute gefactureerde kosten voor rol gevallen bent u zelfs als een cloud-service niet actief is.

Gebruik de volgende procedure om een distributie of uw cloud-service te verwijderen. 

1. Klik in de [Azure klassieke portal](http://manage.windowsazure.com/) **Cloud Services**.

2. Selecteer de cloud-service en klik vervolgens op **verwijderen**. (U selecteert een cloud-service zonder het dashboard te openen, klikt u ergens behalve de naam van de service cloud post.)

    Als er een implementatie in staging- of productieomgeving, ziet u een menu met opties die lijkt op de volgende onder aan het venster. Voordat u de service cloud verwijderen kunt, moet u alle bestaande installaties.

    ![Menu verwijderen](./media/cloud-services-how-to-manage/CloudServices_DeleteMenu.png)


3. Als u wilt verwijderen van een distributie, klikt u op **deployment gefaseerde installatie**of **productie-implementatie verwijderen** . Klik vervolgens op de bevestigingsprompt op **Ja**. 

4. Als u wilt verwijderen van de service cloud, Herhaal stap 3, indien nodig, de andere installatie verwijderen.

5. Als u wilt verwijderen in de cloud-service, klikt u op **verwijderen cloud-service**. Klik vervolgens op de bevestigingsprompt op **Ja**.

> [AZURE.NOTE]
> Als uitgebreide controle is geconfigureerd voor uw service cloud, worden Azure gegevens niet verwijderd de controle van uw opslag-account wanneer u de cloud-service verwijderen. U moet de gegevens handmatig te verwijderen. Zie voor informatie over waar de tabellen metrics, "hoe: toegang tot uitgebreide controle van gegevens buiten de klassieke Azure portal" in [het Monitor Cloud Services](cloud-services-how-to-monitor.md).

## <a name="next-steps"></a>Volgende stappen

 * [Algemene configuratie van de service cloud](cloud-services-how-to-configure.md).
* Meer informatie over het [implementeren van een cloud-service](cloud-services-how-to-create-deploy.md).
* Een [aangepaste domeinnaam](cloud-services-custom-domain-name.md)configureren.
* [Ssl-certificaten](cloud-services-configure-ssl-certificate.md)configureren.
