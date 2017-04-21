Het is mogelijk een URI MongoLab in uw code plakken, kunt u het beste in de omgeving voor het gemak van beheer te configureren. Op deze manier als de URI wordt gewijzigd, kunt u deze bijwerken via de Portal Azure zonder de code.


1. Selecteer in de Portal Azure **Web Apps**.
1. Klik op de naam van de web-app in de lijst van Web Apps.  
![WebAppEntry][entry-website]  
Het Web App dashboard wordt weergegeven.

1. Klik op **configureren** in de menubalk.  
![WebAppDashboardConfig][focus-mongolab-websitedashboard-config]

1. Ga naar de sectie verbindingsreeksen.  
![WebAppConnectionStrings][focus-mongolab-websiteconnectionstring]

1. Voer voor de **naam**MONGOLAB_URI.
1. Voor **waarde**, plak de verbindingsreeks die is verkregen in de vorige sectie.
1. Selecteer **aangepast** in de vervolgkeuzelijst **Type** (in plaats van de standaard **SQLAzure**).
1. Klik op de werkbalk op **Opslaan** .  
![SaveWebApp][button-website-save]

**Opmerking:** Azure voegt de **CUSTOMCONNSTR\_ ** de code boven de verwijzingen naar deze variabele, daarom is het voorvoegsel **CUSTOMCONNSTR\_MONGOLAB_URI.**

[entry-website]: ./media/howto-save-connectioninfo-mongolab/entry-website.png
[focus-mongolab-websitedashboard-config]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websitedashboard-config.png
[focus-mongolab-websiteconnectionstring]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websiteconnectionstring.png
[button-website-save]: ./media/howto-save-connectioninfo-mongolab/button-website-save.png
