<properties
    pageTitle="Voorwaarden voor toegang tot de Azure advertentie melden API. | Microsoft Azure"
    description="Meer informatie over de vereisten voor toegang tot de Azure advertentie melden API"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a>Voorwaarden voor toegang tot de Azure advertentie melden API 

De [Azure AD melden API's](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) bieden programmatische toegang tot de gegevens via een reeks API's op basis van de REST. Uit een groot aantal programmeertalen en hulpprogramma's kunt u deze API's aanroepen.

De rapportage-API gebruikt [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) toegang tot het web API's toe te staan. 

Ter voorbereiding van de toegang tot de rapportage-API, moet u:

1. Een toepassing maken in uw huurder Azure AD 

2. De toepassing juiste machtigingen verlenen voor toegang tot de gegevens Azure AD

3. Configuratie-instellingen van uw directory verzamelen

Voor vragen, problemen of feedback, neem contact op met [AAD te melden](mailto:aadreportinghelp@microsoft.com).


## <a name="create-an-azure-ad-application"></a>Een Azure AD-toepassing maken

Configureren van de map voor toegang tot de Azure advertentie melden API, moet u inloggen om de Azure klassieke portal met een beheerdersaccount Azure abonnement die ook lid is van de rol van globale beheerder map in uw huurder Azure AD.

> [AZURE.IMPORTANT] Toepassingen die worden uitgevoerd onder referenties met 'admin' bevoegdheden als volgt kunnen zeer krachtig zijn, dus zorg ervoor dat de toepassingsgegevens ID/geheim veilig te houden.


1. Klik in de [Azure klassieke portal](https://manage.windowsazure.com)in het linkernavigatievenster op **Active Directory**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Selecteer de map uit de lijst met **active directory** .

3. Klik in het menu aan de bovenkant, **toepassingen**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Op de onderste balk, klikt u op **toevoegen**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/03.png) 

5. Op de **Wat wilt u doen?** dialoogvenster, klikt u op **het ontwikkelen van mijn organisatie toepassing toevoegen**. 

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/04.png) 


6. Voer de volgende stappen uit in het dialoogvenster **Vertel ons over uw toepassing** : 

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/05.png) 

    een. Typ in het tekstvak **naam** een naam (bv.: API Application Reporting).

    b. Selecteer de **webtoepassing en/of web-API**.

    c. Klik op **volgende**.


7. Voer de volgende stappen uit in het dialoogvenster **Eigenschappen van App** : 

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/06.png) 

    een. Typ in het tekstvak **URL - on** `https://localhost`.

    b. Typ in het tekstvak **App ID URI** ```https://localhost/ReportingApiApp```.

    c. Klik op **Voltooien**.



## <a name="grant-your-application-permission-to-use-the-api"></a>Uw toepassing machtigen voor gebruik van de API

1. Klik in de [Azure klassieke portal](https://manage.windowsazure.com/)in het linkernavigatievenster op **Active Directory**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Selecteer de map uit de lijst met **active directory** .

3. Klik in het menu aan de bovenkant, **toepassingen**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/02.png)


3. Selecteer uw nieuwe toepassing in de lijst toepassingen.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/07.png)

4. In het menu aan de bovenkant, klikt u op **configureren**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/08.png)


5. In de sectie **machtigingen voor andere toepassingen** voor de resource **Azure Active Directory** , klikt u op de vervolgkeuzelijst **Machtigingen** en selecteer **de Active directory-gegevens lezen**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/09.png)


5. Klik op **Opslaan**op de onderste balk.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/10.png)


## <a name="gather-configuration-settings-from-your-directory"></a>Configuratie-instellingen van uw directory verzamelen

In deze sectie wordt beschreven hoe u de volgende instellingen ophalen uit de map:

- Naam van het domein
- Client-ID
- Het geheim van de client

Deze waarden moet u bij het aanroepen van de API van rapportage configureren. 


### <a name="get-your-domain-name"></a>Ophalen van uw domeinnaam

1. Klik in de [Azure klassieke portal](https://manage.windowsazure.com)in het linkernavigatievenster op **Active Directory**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Selecteer de map uit de lijst met **active directory** .

3. Klik in het menu aan de bovenkant, **domeinen**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/11.png) 

4. Kopieer uw domeinnaam in de kolom **Naam van het domein** .

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/12.png) 


### <a name="get-the-applications-client-id"></a>Van de toepassing de client-ID ophalen

1. Klik in de [Azure klassieke portal](https://manage.windowsazure.com)in het linkernavigatievenster op **Active Directory**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Selecteer de map uit de lijst met **active directory** .

3. Klik in het menu aan de bovenkant, **toepassingen**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Selecteer uw nieuwe toepassing in de lijst toepassingen.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/07.png)

4. In het menu aan de bovenkant, klikt u op **configureren**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/08.png)

4. Kopieer de **Client-ID**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/13.png)


### <a name="get-the-applications-client-secret"></a>Ophalen van de toepassing client geheim

Als u client-geheim van uw toepassing, moet u een nieuwe sleutel maken en opslaan van de waarde bij het opslaan van de nieuwe sleutel, omdat het is niet mogelijk deze waarde later niet meer ophalen.

1. Klik in de [Azure klassieke portal](https://manage.windowsazure.com)in het linkernavigatievenster op **Active Directory**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Selecteer de map uit de lijst met **active directory** .

3. Klik in het menu aan de bovenkant, **toepassingen**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Selecteer uw nieuwe toepassing in de lijst toepassingen.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/07.png)

4. In het menu aan de bovenkant, klikt u op **configureren**.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/08.png)

5. Voer de volgende stappen uit in de sectie **sleutels** : 

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/14.png)

    een. Selecteer in de lijst duur een duur

    b. Klik op **Opslaan**op de onderste balk.

    ![Toepassing registreren](./media/active-directory-reporting-api-prerequisites/10.png)

    c. Kopieer de waarde van de sleutel.

## <a name="next-steps"></a>Volgende stappen

- Wilt u toegang tot de gegevens uit de Azure Active Directory API op een programmatische manier melden? Uitchecken [aan de slag met de Azure Active Directory Reporting-API](active-directory-reporting-api-getting-started.md).

- Als u meer weten wilt over Azure Active Directory reporting Zie [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).  
