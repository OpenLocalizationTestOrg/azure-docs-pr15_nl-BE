<properties
   pageTitle="Verificatie configureren met Amazon webservices | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe maken en valideren van een referentie (AWS) voor runbooks in automatisering van Azure AWS bronnen beheren."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="AWS-verificatie configureren (AWS)"/>
<tags
   ms.service="automation"
   ms.workload="tbd"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.date="09/12/2016"
   ms.author="magoedte"/>

# <a name="authenticate-runbooks-with-amazon-web-services"></a>Runbooks met Amazon webservices verifiëren
Veelvoorkomende taken automatiseren met bronnen in de Amazon Web Services (AWS) kan worden uitgevoerd met de runbooks voor automatisering in Azure.  U kunt veel taken in met behulp van automatisering runbooks net als met resources in Azure AWS automatiseren.  Vereist zijn twee dingen:

* Een abonnement (AWS) en een set referenties.  De AWS toegangssleutel en geheime sleutel.  Lees het artikel [Met behulp van AWS referenties](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html)voor meer informatie.
* Een abonnement op Azure en automatisering account.  Lees het artikel [Configureren Azure uitvoeren als-Account](../automation/automation-sec-configure-azure-runas-account.md)voor meer informatie over het instellen van een account Azure automatisering.  

Als u wilt verifiëren met (AWS), moet u een set referenties voor de verificatie van uw runbooks uitvoeren van automatisering Azure AWS. Als u al een automatisering-account gemaakt en u gebruiken wilt voor verificatie met AWS, kunt u de stappen in de volgende sectie.  Als u een account voor runbooks gericht op AWS resources toegewezen, moet u maakt eerst een nieuwe [account automatisering uitvoeren als](../automation/automation-sec-configure-azure-runas-account.md) (de optie voor het maken van een service principal overslaan) en volg de onderstaande stappen.

## <a name="configure-automation-account"></a>Automatisering-account configureren
Voor het automatiseren van Azure AWS communiceert, moet u eerst uw referenties (AWS) ophalen en deze opslaan als activa in Azure automatisering.  De volgende stappen beschreven in het document AWS [Toegangstoetsen beheren voor uw Account (AWS)](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) een toegangstoets maken en kopiëren van de **Sleutel-ID van Access** en de **Toegangstoets geheim** (eventueel het ergens veilig opslaan van uw bestand downloaden).

Nadat u hebt gemaakt en uw beveiligingssleutels AWS gekopieerd, moet u een referentie actief maken met een account met Azure automatisering veilig opslaan en ze verwijzen met de runbooks.  Volg de stappen in de sectie **voor het maken van een nieuwe referentie** in de [activa van de referentie in Azure automatisering](../automation/automation-certificates.md/###To create a new credential with the Azure portal) artikel en voer de volgende gegevens:

1. Voer in het vak **naam** **AWScred** of een geschikte waarde na uw naamgevingsstandaarden.  
2. Typ in het vak **gebruikersnaam** uw **Toegangs-ID** en de **Toegangstoets geheim** in het vak **wachtwoord** en **wachtwoord bevestigen** .   

## <a name="next-steps"></a>Volgende stappen

- Het artikel oplossing [automatiseren implementatie van een VM in Amazon Web Services](../automation/automation-scenario-aws-deployment.md) voor meer informatie over het maken van runbooks voor het automatiseren van taken in de AWS bekijken.
