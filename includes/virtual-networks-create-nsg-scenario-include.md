## <a name="scenario"></a>Scenario

Dit document om beter illustreren hoe u NSGs maakt, gebruikt het volgende scenario.

![VNet-scenario](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

In dit scenario maakt u een NSG voor elk subnet in het virtuele netwerk **TestVNet** , zoals hieronder beschreven: 

- **NSG-FrontEnd**. Front-end voor het NSG wordt toegepast op het subnet *FrontEnd* en bevatten twee regels:  
    - **rdp-regel**. Deze regel kunt RDP-verkeer naar het subnet *FrontEnd* .
    - **web-regel**. Deze regel kunt HTTP-verkeer naar het subnet *FrontEnd* .
- **NSG-BackEnd**. De back-end NSG wordt toegepast op het subnet van de *back-end* en bevatten twee regels: 
    - **sql-regel**. Deze regel kunt SQL-verkeer alleen van het subnet *FrontEnd* .
    - **web-regel**. Deze regel al dan niet toestaan van dat alle internet verkeer afhankelijk van het subnet van de *back-end* .

De combinatie van deze regels een DMZ-achtige scenario maken, waar de back-end-subnet kan alleen binnenkomend verkeer ontvangen voor SQL uit het subnet-front-end, en heeft geen toegang tot het Internet, terwijl het subnet-front-end kan met het Internet communiceren en binnenkomende HTTP-aanvragen ontvangen.
 
