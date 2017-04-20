<properties 
    pageTitle="Cloud App Discovery registerinstellingen voor proxyservices | Microsoft Azure" 
    description="Het doel van dit onderwerp is te voorzien van de stappen die u uitvoeren moet om de vereiste poort instellen op de computers waarop de agent Cloud App Discovery." 
    services="active-directory" 
    documentationCenter="" 
    authors="markusvi" 
    manager="femila"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="markusvi"/>

# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a>Cloud App Discovery registerinstellingen voor proxyservices

De agent Cloud App detectie is standaard geconfigureerd om alleen de poorten 80 of 443 te gebruiken. Als u van plan bent over het installeren van Cloud App Discovery in een omgeving met een proxy-server die van een aangepaste poort (80 noch 443 gebruikmaakt), moet u uw agenten om deze poort te configureren. De configuratie is gebaseerd op een registersleutel.


Het doel van dit onderwerp is te voorzien van de stappen die u uitvoeren moet om de vereiste poort instellen op de computers waarop de agent Cloud App Discovery.



**Als u wilt wijzigen welke poort wordt gebruikt door de computer met de Cloud App Discovery-agent, moet u de volgende stappen uitvoeren:**


1. Start de Register-editor. <br> ![Uitvoeren](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)

2. Ga naar of de volgende registersleutel: <br> **HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud-App Discovery\Endpoint** 

3. Maak een nieuwe waarde **met meerdere tekenreeksen** , **poorten**genoemd. ![Nieuw](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)

4. U opent het dialoogvenster **Meerdere tekenreeksen bewerken** , dubbelklikt u op de waarde poorten.


5. Typ de volgende waarden en voeg alle aangepaste poorten die door uw organisatie worden gebruikt in het tekstvak waarde gegevens: <br><br>
**80** <br>
**8080** <br>
**8118** <br>
**8888** <br>
**81** <br>
**12080** <br>
**6999** <br>
**30606** <br>
**31595** <br>
**4080** <br>
**443** <br>
**1110** <br><br>
![Met meerdere tekenreeksen bewerken](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)

6. Klik op **OK** om het dialoogvenster **Met meerdere tekenreeksen bewerken** te sluiten.



**Aanvullende bronnen**


* [Hoe kan ik verzoek tot niet toegestande cloud-toepassingen die worden gebruikt binnen mijn organisatie ontdekken](active-directory-cloudappdiscovery-whatis.md) 


