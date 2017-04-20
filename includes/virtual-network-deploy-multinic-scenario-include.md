## <a name="scenario"></a>Scenario

Dit document helpt bij een implementatie met meerdere NIC's in VMs in een bepaald scenario. In dit scenario hebt u een IaaS twee lagen werkbelasting gehost in Azure. Elke laag wordt geïmplementeerd in een eigen subnet in een virtueel netwerk (VNet). De front-end laag bestaat uit verschillende webservers, gegroepeerd in een taakverdeling instellen voor maximale beschikbaarheid. De back-end-laag bestaat uit meerdere databaseservers. Deze databaseservers worden met twee NIC's, één voor de toegang tot de database, de andere voor beheer geïmplementeerd. Het scenario omvat ook netwerk-beveiligingsgroepen (NSGs) om te bepalen welk verkeer is toegestaan voor elk subnet en NIC in de implementatie. De onderstaande figuur toont de basisarchitectuur van dit scenario.  

![MultiNIC-scenario](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)

