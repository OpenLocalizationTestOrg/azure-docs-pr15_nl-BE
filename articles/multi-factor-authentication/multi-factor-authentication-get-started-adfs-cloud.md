<properties
    pageTitle="Resources met Azure MVR gesloten en AD FS beveiligde cloud"
    description="Dit is de pagina Azure meerledige verificatie die wordt beschreven hoe u aan de slag met Azure MVR gesloten en AD FS in de cloud."
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

# <a name="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs"></a>Beveiligen van bronnen met Azure meerledige verificatie en AD FS cloud

Als uw organisatie met Azure Active Directory federated is, Azure meerledige verificatie of gebruiken Active Directory Federation Services voor het beveiligen van bronnen die worden gebruikt door AD Azure. Gebruik de volgende procedures voor het beveiligen van Azure Active Directory-bronnen met Azure meerledige verificatie of Active Directory Federation Services.

## <a name="secure-azure-ad-resources-using-ad-fs"></a>Azure AD bronnen met AD FS beveiligde

Beveiliging van uw wolk resource, eerst een account voor gebruikers inschakelen en vervolgens een regel vorderingen instellen. Volg deze procedure om de stappen doorlopen:

1. Gebruik de stappen die worden beschreven in [Turn-on meerledige verificatie voor gebruikers](active-directory/multi-factor-authentication-get-started-cloud.md#turn-on-multi-factor-authentication-for-users) een account in te schakelen.
2. Start de console Beheer van AD FS.
![Wolk](./media/multi-factor-authentication-get-started-adfs-cloud/adfs1.png)
3. Ga naar de **Partij vertrouwt vertrouwen** en met de rechtermuisknop op het vertrouwen partij vertrouwt. Selecteer **bewerken-Claim regels...**
4. Klik op **regel... toevoegen**
5. In de vervolgkeuzelijst **vorderingen met behulp van een aangepaste regel verzenden** en klik op **volgende**.
6. Voer een naam voor de regel van de claim.
7. Onder de aangepaste regel: Voeg de volgende tekst:

    ```
    => issue(Type = "http://schemas.microsoft.com/claims/authnmethodsreferences", Value = "http://schemas.microsoft.com/claims/multipleauthn");
    ```

    Bijbehorende claim:

    ```
    <saml:Attribute AttributeName="authnmethodsreferences" AttributeNamespace="http://schemas.microsoft.com/claims">
    <saml:AttributeValue>http://schemas.microsoft.com/claims/multipleauthn</saml:AttributeValue>
    </saml:Attribute>
    ```

8. Klik op **OK** en vervolgens **Voltooien**. Sluit de console Beheer van AD FS.

Gebruikers kunnen Vul het aanmelden met behulp van de methode op ruimten (zoals een smartcard).

## <a name="trusted-ips-for-federated-users"></a>Vertrouwde IP-adressen voor federatieve gebruikers
Vertrouwde IP-adressen kunnen beheerders omloopleiding uit twee stappen bestaande controle voor bepaalde IP-adressen, of voor federatieve gebruikers die aanvragen die afkomstig zijn van binnen hun eigen intranet. In de volgende secties wordt beschreven hoe Azure meerledige verificatie vertrouwde IP-adressen configureren met federatieve gebruikers en de omloopleiding uit twee stappen bestaande controle wanneer een aanvraag afkomstig van binnen een intranet federatieve gebruikers is. Dit wordt bereikt door AD FS met een Pass Through-query of filter een binnenkomende claim sjabloon met het claimtype binnen bedrijfsnetwerk configureren.

In dit voorbeeld wordt de Office 365 voor onze vertrouwen partij vertrouwt.

### <a name="configure-the-ad-fs-claims-rules"></a>De AD FS-claims regels configureren

Het eerste wat dat we moeten doen is voor het configureren van de AD FS-claims. We gaan maken vorderingen op twee regels, één voor het claimtype binnen bedrijfsnetwerk en een extra om onze gebruikers ingelogd te houden.

1. Open beheer van AD FS.
2. Selecteer aan de linkerkant, **Vertrouwen partij vertrouwt**.
3. Klik met de rechtermuisknop op **Microsoft Office 365 identiteit Platform** en selecteer **Bewerken Claim regels...** 
 ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)
4. Klik op de regels voor uitgifte transformeren, **regel toevoegen.** 
 ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)
5. De transformatie Claim regel Wizard toevoegen **doorheen of een binnenkomende Claim Filter** selecteren in de vervolgkeuzelijst en klik op **volgende**.
![Wolk](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)
6. In het vak naast de naam van Claim regel, geef de regel een naam. Bijvoorbeeld: InsideCorpNet.
7. In de vervolgkeuzelijst naast het binnenkomende claimtype, selecteert u **In bedrijfsnetwerk**.
![Wolk](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)
8. Klik op **Voltooien**.
9. Klik op de regels voor uitgifte transformeren, **Regel toevoegen**.
10. De transformatie Claim regel Wizard toevoegen, **verzenden vorderingen met behulp van een aangepaste regel** selecteert in de vervolgkeuzelijst en klik op **volgende**.
11. In het vak onder de naam van Claim: Voer *Houden gebruikers ingelogd*.
12. Geef in het vak aangepast regel:

        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
![Wolk](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip5.png)
13. Klik op **Voltooien**.
14. Klik op **toepassen**.
15. Klik op **Ok**.
16. Sluit beheer van AD FS.



### <a name="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users"></a>Configureren van Azure meerledige verificatie vertrouwde IP-adressen met federatieve gebruikers
Nu dat de claims uitgevoerd worden, kunnen we vertrouwde IP-adressen configureren.

1. Log in om de [Azure klassieke portal](https://manage.windowsazure.com).
2. Aan de linkerkant, klikt u op **Active Directory**.
3. Selecteer onder map, de map waar u vertrouwde IP-adressen instellen.
4. Klik op de map die u hebt geselecteerd, klikt u op **configureren**.
5. Klik op **service-instellingen beheren**in de sectie meerledige verificatie.
6. Selecteer op de pagina instellingen van de Service onder vertrouwde IP-adressen, **meerdere-factor-verificatie overslaan voor aanvragen van federatieve gebruikers op mijn intranet.** 
 ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip6.png)
7. Klik op **Opslaan**.
8. Nadat de updates zijn toegepast, klikt u op **sluiten**.


Dat is alles. Federatieve gebruikers van Office 365 hoeven nu alleen te gebruiken MVR gesloten wanneer een aanvraag afkomstig van buiten het bedrijfsintranet is.
