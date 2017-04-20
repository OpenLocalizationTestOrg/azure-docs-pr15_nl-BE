<properties
    pageTitle="Werken met de vorderingen op de hoogte van de toepassingen in de toepassingsproxy"
    description="Wordt beschreven hoe aan de slag te kunnen met AD-toepassingsproxy Azure."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>



# <a name="working-with-claims-aware-apps-in-application-proxy"></a>Werken met de vorderingen op de hoogte van de toepassingen in de toepassingsproxy

Vorderingen op de hoogte apps uitvoeren een omleiding naar de Security Token Service (STS), die op zijn beurt vraagt om referenties van de gebruiker in ruil voor een token voor de gebruiker omleiden naar de toepassing. Als u wilt dat de toepassingsproxy voor het werken met deze omleidingen, de volgende stappen dienen te worden genomen.

## <a name="prerequisites"></a>Vereisten
Voordat u deze procedure uitvoert, zorg dat de STS de vorderingen op de hoogte app wordt omgeleid naar buiten uw netwerk op ruimten beschikbaar.

## <a name="azure-classic-portal-configuration"></a>Azure klassieke configuratie van portal

1. Publiceer de toepassing volgens de instructies die worden beschreven in [de toepassingen publiceren met de Proxy](active-directory-application-proxy-publish.md).
2. De vorderingen op de hoogte app selecteren in de lijst met toepassingen en klik op **configureren**.
3. Als u **Passthrough** als **Methode voor vooraf-verificatie**kiest, moet u **HTTPS** selecteert als uw **Externe URL** -schema.
4. Als u **Active Directory Azure** als **Methode voor vooraf-verificatie**kiest, selecteert u **geen** als **Interne methode voor verificatie**.


## <a name="adfs-configuration"></a>AD FS-configuratie

1. Open beheer van AD FS.
2. Ga naar de **Partij vertrouwt vertrouwen**, klik met de rechtermuisknop op de toepassing die u wilt publiceren met de toepassingsproxy en kies **Eigenschappen**.  
  ![Gebruikmakende partij vertrouwt Klik met de rechtermuisknop op de naam van de app - screentshot](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  
3. Selecteer op het tabblad **eindpunten** onder **type eindpunt** **WS-Federation**.
4. Onder de **Vertrouwde URL** de URL die u hebt ingevoerd in de toepassingsproxy onder **Externe URL** en klik op **OK**.  
  ![Toevoegen van een Endpoint - waarde vertrouwde URL - screenshot](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## <a name="see-also"></a>Zie ook

- [Uitgeven van toepassingen met de toepassingsproxy](active-directory-application-proxy-publish.md)
- [Eenmalige inschakelen](active-directory-application-proxy-sso-using-kcd.md)
- [Oplossen van problemen met de toepassingsproxy](active-directory-application-proxy-troubleshoot.md)
- [Native Clienttoepassingen communiceren met toepassingen van proxy inschakelen](active-directory-application-proxy-native-client.md)

Bekijk de [toepassingsproxy blog](http://blogs.technet.com/b/applicationproxyblog/) voor het laatste nieuws en updates
