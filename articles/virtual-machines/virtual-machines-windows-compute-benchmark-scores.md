<properties
 pageTitle="Benchmark scores voor Windows VMs berekenen | Microsoft Azure"
 description="SPECint compute benchmark scores vergelijken voor Azure VMs met Windows Server"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="cynthn"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/22/2016"
 ms.author="cynthn"/>

# <a name="compute-benchmark-scores-for-windows-vms"></a>Benchmark scores voor Windows VMs berekenen

Volgende scores van de benchmarktest SPECInt weergeven prestaties voor Azure van krachtige VM lineup met Windows Server. COMPUTE benchmark scores zijn ook beschikbaar voor [Linux VMs](virtual-machines-linux-compute-benchmark-scores.md).



## <a name="a-series---compute-intensive"></a>A-series-computer-intensieve


Grootte | vCPUs | NUMA-knooppunten | CPU | Wordt uitgevoerd | Basistarief Gem. | STDEV
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_A8 | 8 | 1 | Intel Xeon CPU E5 2670 0 @ 2,6 GHz | 10 | 236.1 | 1.1
Standard_A9 | 16 | 2 | Intel Xeon CPU E5 2670 0 @ 2,6 GHz | 10 | 450.3 | 7.0
Standard_A10 | 8 | 1 | Intel Xeon CPU E5 2670 0 @ 2,6 GHz | 5 | 235.6 | 0,9
Standard_A11 | 16 | 2 | Intel Xeon CPU E5 2670 0 @ 2,6 GHz |7 | 454.7 | 4.8

## <a name="dv2-series"></a>Dv2-serie


Grootte | vCPUs | NUMA-knooppunten | CPU | Wordt uitgevoerd | Basistarief Gem. | STDEV
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_D1_v2 | 1 | 1 | V3 voor Intel Xeon E5-2673 @ 2,4 GHz | 83 | 36.6 | 2.6
Standard_D2_v2 | 2 | 1 | V3 voor Intel Xeon E5-2673 @ 2,4 GHz | 27 | 70,0 | 3.7
Standard_D3_v2 | 4 | 1 | V3 voor Intel Xeon E5-2673 @ 2,4 GHz | 19 | 130.5 | 4.4
Standard_D4_v2 | 8 | 1 | V3 voor Intel Xeon E5-2673 @ 2,4 GHz | 19 | 238.1 | 5.2
Standard_D5_v2 | 16 | 2 | V3 voor Intel Xeon E5-2673 @ 2,4 GHz | 14 | 460.9 | 15,4-inch
Standard_D11_v2 | 2 | 1 | V3 voor Intel Xeon E5-2673 @ 2,4 GHz | 19 | 70.1 | 3.7
Standard_D12_v2 | 4 | 1 | V3 voor Intel Xeon E5-2673 @ 2,4 GHz | 2 | 132.0 | 1.4
Standard_D13_v2 | 8 | 1 | V3 voor Intel Xeon E5-2673 @ 2,4 GHz | 17 | 235.8 | 3.8
Standard_D14_v2 | 16 | 2 | V3 voor Intel Xeon E5-2673 @ 2,4 GHz | 15 | 460.8 | 6.5


## <a name="g-series-gs-series"></a>G-serie, GS-serie


Grootte | vCPUs | NUMA-knooppunten | CPU | Wordt uitgevoerd | Basistarief Gem. | STDEV
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_G1, Standard_GS1  | 2 | 1 | V3 voor Intel Xeon E5-2698B @ 2 GHz | 31 | 71.8 | 6.5
Standard_G2, Standard_GS2 | 4 | 1 | V3 voor Intel Xeon E5-2698B @ 2 GHz | 5 | 133,4 | 13.0
Standard_G3, Standard_GS3 | 8 | 1 | V3 voor Intel Xeon E5-2698B @ 2 GHz | 6 | 242.3 | 6.0
Standard_G4, Standard_GS4 | 16 | 1 | V3 voor Intel Xeon E5-2698B @ 2 GHz | 15 | 398.9 | 6.0
Standard_G5, Standard_GS5 | 32 | 2 | V3 voor Intel Xeon E5-2698B @ 2 GHz | 22 | 762.8 | 3.7

## <a name="h-series"></a>H-serie

Grootte | vCPUs | NUMA-knooppunten | CPU | Wordt uitgevoerd | Herhalingen per seconde | STDEV
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_H8 | 8 | 1 | V3 voor Intel Xeon E5 2667 @ 3,2 GHz | 5 | 297.4 | 0,9
Standard_H16 | 16 | 2 | V3 voor Intel Xeon E5 2667 @ 3,2 GHz | 5 | 575.8 | 6,8
Standard_H8m | 8 | 1 | V3 voor Intel Xeon E5 2667 @ 3,2 GHz | 5 | 297.0 | 1.2
Standard_H16m | 16 | 2 | V3 voor Intel Xeon E5 2667 @ 3,2 GHz | 5 | 572.2 | 3.9
Standard_H16r | 16 | 2 | V3 voor Intel Xeon E5 2667 @ 3,2 GHz | 5 | 573.2 | 2.9
Standard_H16mr | 16 | 2 | V3 voor Intel Xeon E5 2667 @ 3,2 GHz | 7 | 569.6 | 2.8

## <a name="about-specint"></a>Over SPECint

Windows getallen worden berekend door de [SPECint 2006](https://www.spec.org/cpu2006/results/rint2006.html) uitgevoerd op Windows Server. SPECint is uitgevoerd met de optie basistarief (resultaten van SPECint_rate2006), met één exemplaar per core. SPECint bestaat uit 12 afzonderlijke tests, elk drie keer uitgevoerd, waarbij de gemiddelde waarde van elke test en wegen om een samengesteld score formulier. Deze proeven zijn vervolgens uitgevoerd via meerdere VMs om de gemiddelde scores weergegeven.


## <a name="next-steps"></a>Volgende stappen

* Zie voor opslagcapaciteit schijf details en aanvullende overwegingen voor het kiezen tussen VM formaten, [formaten voor virtuele machines](virtual-machines-windows-sizes.md).
