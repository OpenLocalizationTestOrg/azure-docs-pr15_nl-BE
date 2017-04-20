<properties 
    pageTitle="Bekende netwerken | Microsoft Azure" 
    description="Bekende netwerken configureren, kunt u voorkomen dat IP-adressen die eigendom zijn van uw organisatie deel uit van de Sign-ins uit verschillende gebieden en Sign-ins van IP-adressen met verdachte activiteiten verslagen." 
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
    ms.date="10/10/2016" 
    ms.author="markvi"/>

# <a name="known-networks"></a>Bekende netwerken


U kunt Azure Active Directory-toegang en gebruiksrapporten krijgen inzicht in de integriteit en beveiliging van de directory van uw organisatie. Met deze informatie kunt een directory admin beter bepalen waar mogelijke beveiligingsrisico's kunnen liggen, zodat zij adequaat plannen kunnen die risico's te beperken.

Het is mogelijk dat de rapporten '*Sign-ins uit verschillende gebieden*' en '*Sign-ins van IP-adressen met verdachte activiteiten*' markeert IP-adressen die het eigendom zijn van uw organisatie. 

Dit kan bijvoorbeeld gebeuren wanneer: 

- Een gebruiker in uw office heeft zich aangemeld op afstand naar uw datacenter in San Francisco Boston activeert het rapport "Log-ins uit verschillende gebieden" 

- Een gebruiker van uw organisatie wil aanmelding enkele keren met een onjuist wachtwoord triggers het rapport "Ins ondertekenen van IP-adressen met verdachte activiteiten" 

Om te voorkomen dat deze gevallen misleidende beveiligingsrapporten genereren, bekende IP-adresbereiken toe te voegen aan de lijst met het openbare IP-adres van uw organisatie.    


###<a name="to-add-your-organizations-public-ip-address-ranges-perform-the-following-steps"></a>Als u wilt toevoegen van uw organisatie openbare IP-adresbereiken, kunt u de volgende stappen uitvoeren: 

1.  Aanmelding bij de [Azure management portal](https://manage.windowsazure.com).

2.  Klik in het linkerdeelvenster op **Active Directory**. <br><br>![De werking van detectie van Cloud App](./media/active-directory-known-networks/known-netwoks-01.png)

3.  Selecteer in het tabblad **map** de map.

4.  In het menu aan de bovenkant, klikt u op **configureren**. <br><br>![De werking van detectie van Cloud App](./media/active-directory-known-networks/known-netwoks-02.png)

5.  Op het tabblad configureren, gaat u naar **uw organisaties openbare IP-adresbereiken** <br><br>![De werking van detectie van Cloud App](./media/active-directory-known-networks/known-netwoks-03.png)

6.  Klik op **bekende IP-adresbereiken toevoegen**.

7.  De adresbereiken toevoegen in het dialoogvenster dat verschijnt en klik vervolgens op de knop controleren wanneer u klaar bent. <br><br>![De werking van detectie van Cloud App](./media/active-directory-known-networks/known-netwoks-04.png)


**Aanvullende bronnen**


* [En het gebruik van access-rapporten weergeven](active-directory-view-access-usage-reports.md)
* [Meld u invoegtoepassingen van IP-adressen met verdachte activiteiten](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [Sign-ins uit verschillende gebieden](active-directory-reporting-sign-ins-from-multiple-geographies.md)


