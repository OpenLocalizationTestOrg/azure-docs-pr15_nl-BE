<properties
    pageTitle="Implementatie van grote implementaties"
    description="Informatie over het implementeren van grote implementaties met de Toolkit Azure voor Eclips."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->

# <a name="deploying-large-deployments"></a>Implementatie van grote implementaties #

Als uw distributie te groot om te worden opgenomen in de standaardmap approot is, kunt u een lokale opslag resource als de implementatie van de hoofdmap voor uw JDK en application server.

## <a name="to-use-a-local-storage-resource-as-the-deployment-root-folder-for-large-deployments"></a>Een bron van lokale opslag gebruiken als de hoofdmap van de implementatie voor grote implementaties ##

1. Maak een nieuwe lokale opslag resource. De naam van de bron is niet van belang. Opslagbronnen worden gedefinieerd op het niveau van de rol. De snelste manier om toegang tot het dialoogvenster lokale opslag, waaruit u een nieuwe resource van de lokale opslag kan maken, is met behulp van de volgende stappen: klik met de rechtermuisknop op de rol in de weergave **Project Explorer** (uw Azure projectknooppunt uitvouwen als u de functie niet ziet), **Azure**, en klik op de **Lokale opslag**. Klik op **toevoegen** om een nieuwe bron van lokale opslag maken in het dialoogvenster **Lokale opslag** .
1. Stel de gewenste grootte op ten minste 2048 MB (iets minder kan hetzelfde bestand grootte problemen veroorzaken als u zou optreden in de approot).
1. Zorg ervoor dat **de inhoud als het exemplaar van de rol gerecycled wordt schoon** wordt gecontroleerd; Dit helpt te voorkomen dat de logica voor de implementatie van opstarten worden uitgevoerd in conflicten met bestaande bestanden in de resource als het exemplaar van de rol gerecycled wordt.
1. Zorg ervoor dat de waarde van de **omgevingsvariabele opgeslagen pad na implementatie van de resource** is ingesteld op de tekenreeks **DEPLOYROOT**. Uw lokale opslag resource dialoogvenster ziet er ongeveer als volgt.
    ![][ic667943]

Ook als u **DEPLOYROOT** als de *naam* van de lokale resource gebruikt en automatisch gegenereerde naam van de omgevingsvariabele (die wordt ingesteld op **DEPLOYROOT_PATH** in dit geval) niet te wijzigen, die werkt voor uw toepassing.

Eigenschappen van [lokale opslag][]vindt meer informatie over het maken van een bron van lokale opslag.

## <a name="see-also"></a>Zie ook ##

[Azure Toolkit voor Eclips][]

[Een toepassing Hallo wereld maakt voor Azure in Eclips][]

[Installatie van de Toolkit Azure voor Eclips][] 

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkID=699529
[Een toepassing Hallo wereld maakt voor Azure in Eclips]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installatie van de Toolkit Azure voor Eclips]: http://go.microsoft.com/fwlink/?LinkId=699546
[Eigenschappen van lokale opslag]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png
