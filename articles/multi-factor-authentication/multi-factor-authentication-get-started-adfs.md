<properties
    pageTitle="Azure MVR gesloten en AD FS | Microsoft Azure"
    description="Dit is de pagina Azure meerledige verificatie die wordt beschreven hoe u aan de slag met Azure MVR gesloten en AD FS."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na" ms.topic="get-started-article"
    ms.date="10/17/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-azure-multi-factor-authentication-and-active-directory-federation-services"></a>Aan de slag met Azure meerledige verificatie en Active Directory Federation Services



<center>![Wolk](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

Als uw organisatie heeft uw Active Directory op ruimten met Azure Active Directory met behulp van AD FS federatieve, zijn er twee opties voor het gebruik van Azure meerledige verificatie.

- Beveiligde cloud resources Azure meerledige verificatie of Active Directory Federation Services gebruiken
- Cloud en op ruimten bronnen met Azure meerledige verificatieserver beveiligen

De volgende tabel bevat een overzicht van de ervaring van controle tussen de bronnen met Azure meerledige verificatie en AD FS

|Ervaring van controle - Apps in bladeren|Ervaring van controle - Browser gebaseerde toepassingen
:------------- | :------------- | :------------- |
Azure AD bronnen met Azure meerledige verificatie beveiligen |<li>De eerste verificatiestap wordt uitgevoerd met behulp van AD FS op gebouwen.</li> <li>De tweede stap is een telefonische cloud-verificatie uitgevoerd.</li>|Eindgebruikers kunnen app wachtwoorden gebruiken om aan te melden.
Beveiligen van Azure AD bronnen met behulp van Active Directory Federation Services |<li>De eerste verificatiestap wordt uitgevoerd met behulp van AD FS op gebouwen.</li><li>De tweede stap is uitgevoerd op ruimten door de claim behouden blijft.</li>|Eindgebruikers kunnen app wachtwoorden gebruiken om aan te melden.

Voorbehoud met wachtwoorden app voor federatieve gebruikers:

- App-wachtwoorden worden gecontroleerd met behulp van verificatie van de wolk, zodat ze federation overslaan. Federatie alleen actief wordt gebruikt bij het instellen van een wachtwoord app.
- Client Access Control-instellingen op de ruimten worden niet gehonoreerd door app wachtwoorden.
- U verliest op ruimten verificatie-faciliteit voor app-wachtwoorden.
- Account uitschakelen/verwijderen duurt tot drie uur voor directory-synchronisatie uitschakelen/verwijderen van wachtwoorden in de identiteit van de cloud app vertragen.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor informatie over het instellen van Azure meerledige verificatie of de Server Azure meerledige verificatie met AD FS:

- [Secure cloud resources met Azure meerledige verificatie en AD FS](multi-factor-authentication-get-started-adfs-cloud.md)
- [Veilige cloud en op ruimten bronnen met Azure meerledige verificatieserver met Windows Server 2012 R2 AD FS](multi-factor-authentication-get-started-adfs-w2k12.md)
- [Cloud en op ruimten bronnen met Azure meerledige verificatieserver met AD FS 2.0 beveiligen](multi-factor-authentication-get-started-adfs-adfs2.md)
