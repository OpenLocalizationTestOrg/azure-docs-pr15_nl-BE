<properties 
    pageTitle="Windows-verificatie en Azure meerledige verificatieserver"
    description="Dit is de pagina Azure meerledige verificatie die helpt bij het implementeren van Windows-verificatie en Azure meerledige verificatieserver."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="windows-authentication-and-azure-multi-factor-authentication-server"></a>Windows-verificatie en Azure meerledige verificatieserver

De sectie Windows-verificatie kan de beheerder inschakelen en configureren van Windows-verificatie voor een of meer toepassingen.  Hier volgt een lijst met dingen bedenken voor het instellen van Windows-verificatie.

-  opnieuw opstarten nodig is voordat de Azure meerledige verificatie voor Terminal Services toegepast worden.
-  Als 'Match Azure meerledige verificatie vereisen gebruikers' is ingeschakeld en u zich niet in de lijst met gebruikers, is het niet mogelijk aan te melden bij de computer na opnieuw opstarten.
-  Vertrouwde IP-adressen is afhankelijk van of de toepassing de client-IP-met de verificatie kan bieden. Op dit moment alleen Terminal Services wordt ondersteund.  







>[AZURE.NOTE]Deze functie wordt niet ondersteund voor veilige Terminal Services op Windows Server 2012 R2.




## <a name="to-secure-an-application-with-windows-authentication-use-the-following-procedure"></a>Beveiliging van een toepassing met Windows-verificatie, moet u de volgende procedure gebruiken.

1. Klik op het pictogram van Windows-verificatie op de Server Azure meerledige verificatie.
![Windows-verificatie](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)
2. Schakel het selectievakje inschakelen Windows-verificatie. Dit selectievakje is standaard uitgeschakeld.
3. Op het tabblad toepassingen kan de beheerder een of meer toepassingen voor Windows-verificatie configureren.
4. Selecteer een server of toepassing: opgeven of de servertoepassing is ingeschakeld. Klik op OK.
5. Klik op toevoegen... de knop.
6. Het tabblad vertrouwde IP-adressen kunt u overslaan Azure meerledige verificatie voor Windows-sessies uit specifieke IP-adressen. Bijvoorbeeld als werknemers met de toepassing van het kantoor en thuis, u mogelijk wilt behouden hun telefoon Azure meerledige verificatie terwijl bij het Bureau wordt opgeroepen. Voor deze, zou u het office-subnet opgeven als de vertrouwde IP-adressen.
7. Klik op toevoegen... de knop.
8. Selecteer één IP als u wilt overslaan van één IP-adres.
9. IP-bereik selecteren als u een hele IP-adresbereik overslaan. Voorbeeld van de 10.63.193.1-10.63.193.100.
10. Subnet selecteren als u een bereik van IP-adressen met subnetmasker notatie opgeven. Voer de begindatum van het subnet-IP en kies het juiste netmasker in de lijst.
11. Klik op OK.
