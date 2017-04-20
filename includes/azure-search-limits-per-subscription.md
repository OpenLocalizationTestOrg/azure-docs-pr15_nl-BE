U kunt meerdere services binnen een abonnement, ingericht in een specifieke laag, alleen beperkt door het aantal services dat is toegestaan in elke laag een maken. U kan bijvoorbeeld maximaal 12 in de Basic laag en een andere 12 diensten in de laag S1 binnen het hetzelfde abonnement maken. Zie voor meer informatie over lagen, [Kies een SKU of niveaus voor Azure Search](../articles/search/search-sku-tier.md).

Maximale service grenzen kunnen op verzoek worden verhoogd. Neem contact op met ondersteuning van Azure als u meer diensten binnen het hetzelfde abonnement nodig.

Resource|Gratis|Basic|S1|S2|S3 |S3 HD <sup>1</sup>
---|---|---|---|----|---|----
Maximum services |1 |12 |12  |6 |6 |6 
Maximumschaal in SU <sup>2</sup>|N.V.T. <sup>3</sup>|SU 3 <sup>4</sup> |36 SU|36 SU|36 SU|12 SU, 3 SU <sup>5</sup>

<sup>1</sup> S3 HD ondersteunt geen [indexeerfuncties](../articles/search/search-indexer-overview.md) op dit moment. 

<sup>2</sup> eenheden zoeken (SU) zijn factureerbare eenheden per service, als een *replica* of een *partitie*toegewezen. U moet beide resources voor opslag, indexering en querybewerkingen. Zie voor meer informatie over geldige combinaties die onder de maximale grenzen blijven, [schaal resource niveaus voor query- en werkbelasting](../articles/search/search-capacity-planning.md). 

<sup>3</sup> gratis is gebaseerd op gedeelde bronnen worden gebruikt door meerdere abonnees. In deze laag zijn er geen specifieke resources voor een individuele abonnee. Om deze reden is maximumschaal gemarkeerd als niet van toepassing.

<sup>4</sup> basic heeft één vaste partitie. In deze laag, worden extra SUs gebruikt voor het toewijzen van meer replica's voor een standaardwerkbelasting toegenomen query.

<sup>5</sup> HD S3 heeft een andere toewijzing structuur van toegestane combinaties. U kunt een maximum van 12 hebben voor replica's. Voor partities is het maximum 3.




