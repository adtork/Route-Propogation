# Route-Propogation

In this artcile, we are going to talk about route propogation using route tables with gateways and why its important, and how it affects workloads. Simply put, route propogation or gateway route propgation is the ability of the Virtual Network gateway to push and propgate learned routes to remote network interfaces. We are going to look at a simple Hub+Spoke topology with an AzFW in the hub, peered with two spokes and IPSEC VPN to OnPrem(My Laptop) with Unfi Router.  

# Topology

![image](https://user-images.githubusercontent.com/55964102/199131381-bdaecdd3-899f-48da-97b4-787d7258568f.png)

