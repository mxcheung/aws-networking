# aws-networking

```
On-prem
   |
   |--- Internet ---------> Site-to-Site VPN (BGP, encrypted)
   |
   |--- Private Circuit --> Direct Connect
              |
        +-----+------+
        |            |
   Private VIF    Transit VIF
      |               |
     VPC         Transit Gateway
                       |
                 Many VPCs / Regions
```
