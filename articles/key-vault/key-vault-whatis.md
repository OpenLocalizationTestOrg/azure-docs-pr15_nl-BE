<properties
    pageTitle="Wat is Azure sleutel kluis? | Microsoft Azure"
    description="Azure sleutel kluis kunt u cryptografische sleutels beveiliging en geheimen die door cloud-toepassingen en services. Klanten kunnen via Azure sleutel kluis coderen sleutels en geheimen (zoals verificatiesleutels opslag account sleutels, sleutels voor gegevenscodering. PFX-bestanden en wachtwoorden) met behulp van toetsen die zijn beveiligd door hardware security modules (HSM's)."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="cabailey"/>



# <a name="what-is-azure-key-vault"></a>Wat is Azure sleutel kluis?

Azure sleutel kluis is beschikbaar in de meeste regio's. Zie de [sleutel kluis prijzen pagina](https://azure.microsoft.com/pricing/details/key-vault/)voor meer informatie.

## <a name="introduction"></a>Inleiding

Azure sleutel kluis kunt u cryptografische sleutels beveiliging en geheimen die door cloud-toepassingen en services. Met behulp van sleutel kluis, kunt u sleutels en coderen geheimen (zoals verificatiesleutels opslag account sleutels, sleutels voor gegevenscodering. PFX-bestanden en wachtwoorden) met behulp van toetsen die zijn beveiligd door hardware security modules (HSM's). U kunt importeren of het genereren van sleutels in HSM's voor toegevoegde assurance. Als u ervoor kiest om dit te doen, processen van Microsoft uw sleutels in FIPS 140-2 niveau 2 gevalideerde HSM's (hardware en firmware).  

Sleutel kluis kunt u beheer van sleutels die toegang en uw gegevens te coderen en vergemakkelijkt het beheer van sleutels. Ontwikkelaars kunnen toetsen voor het ontwikkelen en testen in minuten maken en vervolgens naadloos migreren naar productie-sleutels. Beveiligingsbeheerders kunnen verlenen (en intrekken) machtigingen voor de sleutels, indien nodig.

Gebruik de volgende tabel beter begrijpen hoe de sleutel kluis kan helpen om te voldoen aan de behoeften van ontwikkelaars en security administrators.





| Rol        | Toelichting op het probleem           | Opgelost door Azure sleutel kluis  |
| ------------- |-------------|-----|
| Ontwikkelaar voor een toepassing Azure      | "Ik wil een toepassing schrijft voor Azure die sleutels voor het ondertekenen en coderen gebruikt, maar ik wil deze sleutels worden externe van mijn toepassing, zodat de oplossing geschikt is voor een toepassing die is geografisch verspreid. <br/><br/>Ik wil ook deze toetsen en de geheimen worden beschermd zonder te hoeven schrijven van de code zelf. Ook wil deze toetsen en geheimen zijn eenvoudig te gebruiken in mijn toepassingen, met optimale prestaties." | √ Sleutels worden opgeslagen in een kluis en aangeroepen door URI als dat nodig is.<br/><br/> √ Sleutels worden gewaarborgd door Azure, met behulp van standaard-algoritmen en sleutellengten hardware security modules (HSM's).<br/><br/> √ Sleutels worden verwerkt in de HSM's die zich in de dezelfde Azure datacenters als de toepassingen bevinden. Dit biedt betere betrouwbaarheid en lagere latentie dan als de toetsen bevinden zich op een andere locatie, zoals in de lokalen.|
| Ontwikkelaar voor Software als Service (SaaS)      |"Ik wil geen verantwoordelijkheid of aansprakelijkheid voor mogelijke voor mijn klanten huurder sleutels en geheimen. <br/><br/>Ik wil de klanten hebben en hun sleutels beheren zodat ik zich concentreren kunnen op wat ik doen best, die de kern biedt software-functies doen." | √ Klanten kunnen hun eigen sleutels importeren in Azure en deze beheren. Wanneer een SaaS-toepassing cryptografische bewerkingen uitvoeren moet met behulp van hun klanten toetsen, sleutel kluis, gebeurt deze bewerkingen uit voor de toepassing. Toetsen voor de klanten niet wordt weergegeven in de toepassing.|
| Chief security officer (BBF) | "Ik wil weten dat onze toepassingen voldoen aan FIPS 140-2 niveau 2 HSM's voor veilig beheer van sleutels. <br/><br/>Ik wil controleren of mijn organisatie in de controle van de levensduur van sleutels en sleutelgebruik kunt controleren. <br/><br/>En maar we verschillende Azure services en resources gebruiken, ik wil de sleutels beheren vanaf één locatie in Azure."     |√ HSM's zijn FIPS 140-2 niveau 2 wordt gevalideerd.<br/><br/>√ Sleutel kluis is zo ontworpen dat Microsoft niet zien of uw sleutels ophalen.<br/><br/>√ in de buurt van real-time vastleggen van sleutelgebruik.<br/><br/>√ De kluis biedt één interface, ongeacht hoeveel kluizen er in Azure, welke gebieden zij ondersteuning en welke toepassingen gebruiken. |


Iemand met een abonnement op Azure kunt maken en gebruiken van sleutels kluizen. Hoewel de sleutel kluis nuttig voor ontwikkelaars en security administrators, kan deze worden geïmplementeerd en beheerd door de beheerder van een organisatie die andere Azure diensten voor een organisatie beheert. Deze beheerder wilt aanmelden met een Azure abonnement maken bijvoorbeeld een kluis voor de organisatie voor het opslaan van sleutels en vervolgens belast met operationele taken, zoals:

+ Maak of importeer een sleutel of een geheim
+ Intrekken of verwijderen van een sleutel of een geheim
+ Toestaan dat gebruikers en toepassingen toegang krijgen tot de sleutel kluis, zodat u ze kunnen beheren of gebruik de toetsen en geheimen
+ Sleutelgebruik configureren (bijvoorbeeld ondertekenen of coderen)
+ Sleutelgebruik monitor

Deze beheerder zou vervolgens ontwikkelaars URI's aan te roepen vanuit hun toepassingen, en bieden hun beveiligingsbeheerder met logboekgegevens voor sleutelgebruik. 

   ![Overzicht van Azure sleutel kluis][1]

Ontwikkelaars kunnen ook beheren de sleutels rechtstreeks met behulp van API's. Zie [de sleutel kluis developer's guide](key-vault-developers-guide.md)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Zie voor een ophalen begonnen zelfstudie voor een beheerder [Aan de slag met Azure sleutel kluis](key-vault-get-started.md).

Zie [Logboekregistratie van Azure sleutel kluis](key-vault-logging.md)voor meer informatie over het gebruik van logboekregistratie voor sleutel kluis.

Zie voor meer informatie over het gebruik van sleutels en geheimen met Azure sleutel kluis [over sleutels, geheimen, en certificaten](https://msdn.microsoft.com/library/azure/dn903623\(v=azure.1\).aspx).


<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
