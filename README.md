# Route-Propogation

In this artcile, we are going to talk about route propogation when using gateways and why its important, and how it affects workloads. Simply put, route propogation or gateway route propgation is the ability of the Virtual Network Gateway (VNG) to push and propgate learned routes to remote network interfaces. We are going to look at a simple Hub+Spoke topology with an AzFW in the hub, peered with two spokes and IPSEC VPN to OnPrem(My Laptop) with Unfi Router.  

# Topology

![image](https://user-images.githubusercontent.com/55964102/199131381-bdaecdd3-899f-48da-97b4-787d7258568f.png)

# Toggling GW Route Propogation On/Off on the Route Table

By Default the Azure platform will inject the default system routes into the vNIC along with any other learned routes, vnet peering in our example and on-prem ranges for both hub and spoke since we are using "remote gateway" and "allow gateway transit"

The problem arrises we have specific UDRs for the prefixes we want to connect to, **and** gateway route propogation is enabled on the route table affecting those same prefixes. What it does under the hood is push both routes are pushed, but the non UDR routes become invalid:

![image](https://user-images.githubusercontent.com/55964102/199133012-d126b557-bd4c-42dd-bbda-f71d203b000b.png)

![image](https://user-images.githubusercontent.com/55964102/199133181-5b45c933-849b-4fa6-b352-13e06f078e42.png)

Ping in this case from my on-prem laptop to both spoke vms and hub vm works but this is not best practice. As we can see both routes are injected on the vNIC. When using custom UDRs with a route table, its best to **not** propogate gateway routes and let the UDRs do their job, that way you don't have conflicting routes. With GW route propogation turned off. we only see the UDRs injected on the vNIC:

![image](https://user-images.githubusercontent.com/55964102/199133773-53aeab3e-914c-4fde-a460-a5ca8b2c29a5.png)

![image](https://user-images.githubusercontent.com/55964102/199133838-a99c781a-917f-468c-9feb-39b8385bb5a2.png)

![image](https://user-images.githubusercontent.com/55964102/199133858-4f6ac495-76b0-4b3a-813d-a8365bcff293.png)

# Conclusion

This behavior also holds true for BGP learned routes. If you don't want remote prefixes learned and propogated through the GW to their destination, make sure to toggle off gateway route propogation as this can cause routing conflicts and undesriable affects. 



