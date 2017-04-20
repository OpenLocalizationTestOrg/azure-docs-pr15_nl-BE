<properties
    pageTitle="Get Azure meerledige verificatie Provider gestart | Microsoft Azure"
    description="Informatie over het maken van een Provider Azure meerledige verificatie."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>



# <a name="getting-started-with-an-azure-multi-factor-auth-provider"></a>Aan de slag met een Provider Azure meerledige verificatie
Verificatie in twee stappen is standaard beschikbaar voor globale beheerders met Azure Active Directory en gebruikers van Office 365. Echter, als u wilt profiteren van de [Geavanceerde functies](multi-factor-authentication-whats-next.md) klikt u moet de volledige versie aanschaffen MVR van van Azure meerledige verificatie (gesloten).

> [AZURE.NOTE]  Een Provider Azure meerledige verificatie wordt gebruikt om te profiteren van de functies van de volledige versie van Azure MVR gesloten. Het is voor gebruikers die **geen-licenties via Azure MVR gesloten, Azure AD premie of EMS**.  Omvatten de volledige versie van Azure MVR gesloten standaard Azure MVR gesloten Azure AD Premium en EMS.  Als u licenties hebt, hoeft niet u een Provider Azure meerledige verificatie.

Een provider Azure meerledige verificatie is vereist voor het downloaden van de SDK.

> [AZURE.IMPORTANT]  Downloaden van de SDK, maken een Provider Azure meerledige verificatie zelfs als u de MVR gesloten Azure, AAD Premium of EMS-licenties hebt.  Als u een Provider Azure meerledige verificatie voor dit doel maken en al licenties hebt, moet u de Provider te maken met het model **Per gebruiker is ingeschakeld** . Vervolgens de Provider koppeling naar de map waarin de licenties van de MVR gesloten Azure, Azure AD Premium of EMS.  Dit zorgt ervoor dat je alleen krijgt de rekening hebt u meer unieke gebruikers met behulp van de SDK is dan het aantal licenties dat u bezit.


## <a name="to-create-a-multi-factor-auth-provider"></a>Voor het maken van een meerledige verificatie-Provider

Gebruik de volgende stappen voor het maken van een Provider Azure meerledige verificatie.

1. Aanmelden bij de [Azure klassieke portal](https://manage.windowsazure.com) als beheerder.
2. Selecteer **Active Directory**aan de linkerkant.
3. Selecteer op de pagina Active Directory aan de bovenkant, **Meerledige verificatieproviders**.
![Maken van een Provider van de MVR gesloten](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)
4. Aan de onderkant, klik op **Nieuw**.
![Maken van een Provider van de MVR gesloten](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)
5. Selecteer onder Services App **Meerledige verificatie Provider**
![maken van een Provider van de MVR gesloten](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)
6. Selecteer **snel tabellen maken**.
![Maken van een Provider van de MVR gesloten](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)
5. Vul de volgende velden in en selecteer **maken**.
    1. **Naam** : de naam van de Provider meerledige verificatie.
    2. **Gebruiksmodel** – of u wilt dat afzonderlijke gebruikers of betalen per controle. Kies een van de twee opties:
        - Per verificatie – inkoop model dat de kosten per verificatie. Meestal gebruikt voor scenario's waarin in de toepassing van een consument gerichte Azure meerledige verificatie.
        - Per gebruiker ingeschakeld – ingeschakeld inkoop model dat de kosten per gebruiker. Meestal gebruikt voor werknemers toegang hebben tot toepassingen zoals Office 365. Kies deze optie als u bepaalde gebruikers die al een licentie voor Azure MVR gesloten.
    2. **Directory** -huurder Azure Active Directory die de Provider meerledige verificatie is gekoppeld. Let op het volgende:
        - U hoeft niet een Azure AD-map voor het maken van een meerledige verificatie-Provider. Laat het vak leeg als u alleen de Azure meerledige verificatieserver of de SDK wilt gebruiken.
        - De Provider meerledige verificatie moet worden gekoppeld aan een map Azure AD om te profiteren van de geavanceerde functies.
        - Azure AD verbinden, AAD Sync of DirSync zijn alleen vereist als u Active Directory-omgeving op gebouwen met een directory Azure AD synchroniseert.  Als u alleen een Azure AD-map niet wordt gesynchroniseerd, is dit geen vereiste.
![Maken van een Provider van de MVR gesloten](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)
5. Nadat u op maken, de Provider meerledige verificatie wordt gemaakt en wordt er een bericht met de mededeling: **meerledige verificatie gemaakt**. Klik op **Ok**.
![Maken van een Provider van de MVR gesloten](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)
