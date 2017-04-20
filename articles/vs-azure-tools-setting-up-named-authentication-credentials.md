<properties
   pageTitle="Met het instellen van de naam verificatiereferenties | Microsoft Azure"
   description="Meer informatie over hoe om referenties op te geven die Visual Studio kunt gebruiken voor verificatie aanvragen Azure een toepassing uitgeven aan Azure vanuit Visual Studio of een bestaande cloud-service controleren... "
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="setting-up-named-authentication-credentials"></a>Benoemde verificatiereferenties instellen

Een toepassing uitgeven aan Azure vanuit Visual Studio of om een bestaande cloud-service te controleren, moet u de referenties die Visual Studio gebruiken kunt voor het verifiëren van aanvragen voor Azure opgeven. Er zijn verschillende plaatsen in Visual Studio, waar je in deze referenties op te geven. U kunt vanuit de Server Explorer bijvoorbeeld opent het snelmenu voor het knooppunt **Azure** en kies **verbinding maken met Azure**. Wanneer u zich aanmeldt, de abonnementsgegevens die is gekoppeld aan uw account Azure is beschikbaar in Visual Studio en er is niets meer hoeft te doen.

Extra Azure biedt ook ondersteuning voor een oudere manier van de referenties van de met behulp van het abonnementsbestand (.publishsettings). Dit onderwerp beschrijft deze methode, wordt nog steeds ondersteund in Azure SDK 2.2.

De volgende items zijn vereist voor verificatie naar Azure.

- Uw abonnement-ID

- Een geldige X.509 v3-certificaat

>[AZURE.NOTE] De lengte van de sleutel in het X.509 v3-certificaat moet ten minste 2048 bits. Azure weigert een certificaat dat niet aan deze eis voldoen of die niet geldig is.

Visual Studio gebruikt uw abonnements-ID samen met de certificaatgegevens als referenties. De juiste referenties worden in het abonnementsbestand (.publishsettings), met een openbare sleutel voor het certificaat vermeld. Het abonnementsbestand bevat referenties voor meer dan één abonnement.

Zoals verderop in dit onderwerp wordt uitgelegd, kunt u de abonnementsgegevens van het dialoogvenster **Nieuw/bewerken abonnement** bewerken.

U kunt als u wilt voor het maken van een certificaat, raadpleegt u de instructies in het [maken en uploaden van een certificaat voor Azure](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) en het certificaat handmatig aan de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885)te uploaden.

>[AZURE.NOTE] Deze referenties die Visual Studio is vereist voor het beheren van uw cloud-services zijn niet dezelfde referenties die zijn vereist voor het verifiëren van een verzoek aan de Azure storage-services.

## <a name="modify-or-export-authentication-credentials-in-visual-studio"></a>Wijzigen of het exporteren van verificatiereferenties in Visual Studio

U kunt instellen, wijzigen of exporteren van uw verificatiereferenties in het dialoogvenster **Nieuw abonnement** dat wordt weergegeven als u een van de volgende handelingen uitvoeren:

- In **Server Explorer**, open het snelmenu voor het knooppunt **Azure** **Abonnementen beheren**te kiezen, klik op het tabblad **certificaten** en klik op de knop **Nieuw** of **bewerken** .

- Wanneer u een Azure cloud-service van de wizard **publiceren Azure publiceert** , **beheren** in de lijst **Kies uw abonnement** kiezen en vervolgens kiest u het tabblad certificaten en klik vervolgens op de knop **Nieuw** of **bewerken** .

De volgende procedure wordt ervan uitgegaan dat in het dialoogvenster **Nieuw abonnement** geopend is.

### <a name="to-set-up-authentication-credentials-in-visual-studio"></a>Voor het instellen van verificatiereferenties in Visual Studio

1. **Selecteer een bestaand certificaat** voor, verificatielijst, kiest u een certificaat.

1. Klik op de knop **kopiëren van het volledige pad** . Het pad van het certificaat (CER-bestand) wordt gekopieerd naar het Klembord.

    >[AZURE.IMPORTANT] Voor het publiceren van uw Azure vanuit Visual Studio-toepassing, moet u dit certificaat uploaden de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885).

1. Het certificaat uploaden naar de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885):

    1. Kies de koppeling Azure Portal.

         De [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885) wordt geopend.

    1. Aanmelden bij de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885)en kies vervolgens de knop **Cloud Services** .

    1. Kies de cloud-service dat u interesseert.

        Hiermee opent u de pagina voor de service.

    1. Klik op het tabblad **certificaten** op **uploaden** .

    1. Plak het volledige pad van het .cer-bestand dat u zojuist hebt gemaakt en voer vervolgens het wachtwoord die u hebt opgegeven.
