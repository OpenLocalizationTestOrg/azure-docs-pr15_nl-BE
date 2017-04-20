Resource|Standaardlimiet
---|---
Aantal accounts opslag per abonnement|200<sup>1</sup>
TB per account opslag|500 TB
Het maximumaantal containers blob BLOB's, bestandsshares, tabellen, wachtrijen, entiteiten of berichten per account opslag|Alleen de limiet is de account van 500 TB opslagcapaciteit
Maximale grootte van een blob één container, de tabel of de wachtrij|500 TB
Max aantal blokken in een blok blob of blob toevoegen|50.000
Maximale grootte van een blok in een blok blob of blob toevoegen|4 MB
Maximale grootte van een blok blob of blob toevoegen|50.000 x 4 MB (ong. 195 GB) 
Maximale grootte van een pagina blob |1 TB
Maximale grootte van een Tabelentiteit|1 MB
Het maximumaantal eigenschappen in de tabel-entiteit|252
Maximale grootte van een bericht in een wachtrij|64 KB
Maximale grootte van een gedeeld bestand|5 TB
Maximale grootte van een bestand in een bestandsshare|1 TB
Het maximumaantal bestanden in een gedeeld bestand|Alleen de limiet is de totale capaciteit van 5 TB van de bestandsshare
Max 8 KB IOP's per aandeel|1000
Het maximumaantal bestanden in een gedeeld bestand|Alleen de limiet is de totale capaciteit van 5 TB van de bestandsshare
Het maximumaantal containers blob BLOB's, bestandsshares, tabellen, wachtrijen, entiteiten of berichten per account opslag|Alleen de limiet is de account van 500 TB opslagcapaciteit
Het maximumaantal opgeslagen toegangsbeleid per container, bestandsshare, tabel of wachtrij|5
Totaal aanvragen snelheid (ervan uitgaande dat de grootte van objecten in 1KB) per account opslag|Maximaal 20.000 IOP's entiteiten per seconde of berichten per seconde
Doel de doorvoer voor één blob|Maximaal 60 MB per seconde of maximaal 500 aanvragen per seconde
Doel de doorvoer voor één wachtrij (berichten 1 KB)|Maximaal 2000 berichten per seconde
Doel doorvoer voor één tabel partitie (entiteiten 1 KB)|Tot 2000 entiteiten per seconde
De doorvoer van het doel voor één bestandsshare|Maximaal 60 MB per seconde
Max ingress<sup>2</sup> per account opslag (ons regio's)|10 Gbps als GRS/ZRS<sup>3</sup> ingeschakeld, 20 Gbps voor LRS
Max egress<sup>2</sup> per account opslag (ons regio's)|20 Gbps als RA-GRS/GRS/ZRS<sup>3</sup> ingeschakeld, 30 GB/s voor LRS
Max ingress<sup>2</sup> per account opslag (Europese en Aziatische gebieden)|5 Gbps als GRS/ZRS<sup>3</sup> ingeschakeld, 10 Gbps voor LRS
Max egress<sup>2</sup> per account opslag (Europese en Aziatische gebieden)|10 Gbps als RA-GRS/GRS/ZRS<sup>3</sup> ingeschakeld, 15 GB/s voor LRS

<sup>1</sup> Dit omvat zowel Standard en Premium accounts voor opslag. Als u accounts voor meer dan 200 opslag vereist, moet u een aanvraag tot en met [Ondersteuning van Azure](https://azure.microsoft.com/support/faq/). Het team Azure opslag controleert uw business case en maximaal 250 opslag rekeningen kan goedkeuren. 

<sup>2</sup> *Ingress* verwijst naar alle gegevens die worden verzonden naar een opslag (aanvragen). *Egress* verwijst naar alle gegevens (antwoorden) wordt ontvangen van een rekening voor opslag.  

<sup>3</sup> Azure Storage replicatie-opties zijn beschikbaar:

- **RA GRS**: geo-redundante opslag lezen toegang. Als RA GRS is ingeschakeld, zijn doelen voor de secundaire locatie egress gelijk aan die voor de primaire locatie.
- **GRS**: Geo-redundante opslag. 
- **ZRS**: Zone redundante opslag. Alleen beschikbaar voor BLOB's blokkeren. 
- **LRS**: lokaal redundante opslag. 

