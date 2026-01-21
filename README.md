# aws-networking

```
| Service / Feature            | Uses public internet? | Key notes (SAA exam focus)                   |
| ---------------------------- | --------------------- | -------------------------------------------- |
| **Site-to-Site VPN**         | ✅ **Yes**             | Encrypted IPsec over internet                |
| **Direct Connect (DX)**      | ❌ **No**              | Private dedicated circuit                    |
| **VPN over Direct Connect**  | ❌ **No**              | Encrypted over private DX                    |
| **Transit Gateway (TGW)**    | ❌ **No**              | Routing hub, not transport                   |
| **TGW inter-Region peering** | ❌ **No**              | AWS private backbone                         |
| **VPC Peering**              | ❌ **No**              | Private AWS network                          |
| **BGP**                      | ❌ **No**              | Routing protocol only                        |
| **Private VIF**              | ❌ **No**              | Private IP to VPC/TGW                        |
| **Public VIF**               | ❌ **No**              | Public AWS endpoints (still private DX path) |
| **Transit VIF**              | ❌ **No**              | DX → Transit Gateway                         |
````
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

```
                 ┌───────────────────────────┐
                 │       On-premises DC       │
                 │   (Customer Gateway + BGP) │
                 └─────────────┬─────────────┘
                               │
                Internet       │ Private Circuit
          ┌──────────────┐     │     ┌──────────────┐
          │ Site-to-Site  │    │     │ Direct       │
          │ VPN (IPsec)   │    │     │ Connect      │
          └───────┬──────┘     │     └───────┬──────┘
                  │            │             │
                  │            │       Transit VIF
                  │            │             │
        ┌─────────▼─────────┐  │    ┌────────▼────────┐
        │  Transit Gateway  │◀───┼──▶│  Transit Gateway │
        │   us-east-1       │ TGW Inter-Region Peering │
        └─────────┬─────────┘       └────────┬────────┘
                  │                             │
        ┌─────────┼─────────┐         ┌─────────┼─────────┐
        │         │         │         │         │         │
     VPC-A     VPC-B     VPC-C       VPC-D     VPC-E     VPC-F
   (EC2)     (EC2)     (EC2)       (EC2)     (EC2)     (EC2)
   us-east-1                    eu-west-1
```

```
Producers (IoT / Apps / Logs)
        |
        v
Amazon Kinesis Data Firehose
 (auto-scaling, near real-time)
        |
   [Optional Lambda Transform]
        |
        v
Amazon S3
        |
   AWS Glue Crawler
        |
   Glue Data Catalog
        |
Athena / Redshift Spectrum

```
