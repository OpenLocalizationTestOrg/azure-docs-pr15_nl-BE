Azure zal bepalen welke versie van Python gebruiken voor de virtuele omgeving met de volgende prioriteit:

1. versie die is opgegeven in runtime.txt in de hoofdmap
1. opgegeven door de instelling in de configuratie van web app Python versie (de **Instellingen** > blade**Toepassingsinstellingen** voor uw web app in Azure Portal)
1. Python-2.7 is de standaardinstelling als geen van de bovenstaande zijn opgegeven.

Geldige waarden voor de inhoud van 

    \runtime.txt

zijn:

- Python-2.7
- Python-3.4

Als de micro versie (derde cijfer) is opgegeven, wordt genegeerd.
