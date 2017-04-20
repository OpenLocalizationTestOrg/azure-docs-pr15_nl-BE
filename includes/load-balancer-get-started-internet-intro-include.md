Azure-taakverdeling is een taakverdeling laag-4 (TCP, UDP). De taakverdeling biedt een hoge beschikbaarheid door binnenkomend verkeer over gezonde service-exemplaren in de cloud-services of virtuele machines in een load balancer verdelen. Azure taakverdeling kan ook presenteren die diensten op meerdere poorten of meerdere IP-adressen.

U kunt een load balancer te configureren:

* Binnenkomende Internet-verkeer saldo op virtuele machines (VMs) worden geladen. We noemen een load balancer in dit scenario een [internetgerichte taakverdeling](../articles/load-balancer/load-balancer-internet-overview.md).
* Belasting saldo verkeer tussen VMs in een virtueel netwerk (VNet), tussen VMs in cloud-services of tussen computers op gebouwen en VMs in een virtueel netwerk meerdere ruimten. We noemen een load balancer in dit scenario een [interne taakverdeling (ILB)](../articles/load-balancer/load-balancer-internal-overview.md).
* Extern verkeer doorsturen naar een bepaald exemplaar van de VM.
