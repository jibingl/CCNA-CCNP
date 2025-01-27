# Virtual Tunnel Interface
**Virtual Tunnel Interface (VTI)** is a logical interface supported in ASA. As an alternative to policy based
VPN, a VPN tunnel can be created between peers with Virtual Tunnel Interfaces configured. This supports
route based VPN with IPsec profiles attached to the end of each tunnel. This allows dynamic or static routes
to be used. Egressing traffic from the VTI is encrypted and sent to the peer, and the associated SA decrypts
the ingress traffic to the VTI.

Using VTI does away with the requirement of configuring static crypto map access lists and mapping them
to interfaces. You no longer have to track all remote subnets and include them in the crypto map access list.
Deployments become easier, and having static VTI which supports route based VPN with dynamic routing
protocol also satisfies many requirements of a virtual private cloud.
