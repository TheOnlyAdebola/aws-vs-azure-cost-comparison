# aws-vs-azure-cost-comparison
Cloud economics project comparing AWS and Azure hosting costs

## 1. AWS Linux Estimate
- **Instance**: t3.small, 2 vCPU, 2 GB RAM
- **Storage**: 32 GB gp3 EBS
- **Data Transfer**: 100 GB Internet Egress
- **Total**: **$29.53/month**

File: `aws-estimate.csv`  
Screenshot: `screenshots/aws-t3small.png`

## 2. Azure Linux VM Estimate
- **Instance**: B2s, 2 vCPU, 4 GB RAM
- **Storage**: 32 GB Standard SSD
- **Data Transfer**: 100 GB Internet Egress
- **Total**: **$46.24/month**

File: `azure-linux-estimate.xlsx`  
Screenshot: `screenshots/azure-linux-b2s.png`

## 3. Azure Windows + Hybrid Benefit
| Component | Detail | Cost |
| --- | --- | --- |
| **Instance** | B1ms, 1 vCPU, 2 GB RAM + Azure Hybrid Benefit | ~$9.27 |
| **Storage** | 32 GB Standard SSD | ~$2.40 |
| **Data Transfer** | 100 GB Internet Egress | ~$13.47 |
| **Total** | | **~$25.14/month** |

File: `azure-windows-hybrid-estimate.xlsx`  
Screenshot: `screenshots/azure-windows-hybrid.png`

## 4. Inter-Zone Data Transfer Costs

Networking fees for traffic between availability zones/regions:

| Provider | Same Region, Inter-Zone | Cross-Region |
| --- | --- | --- |
| **AWS** | $0.01 per GB | $0.02 per GB |
| **Azure** | $0.02 per GB | $0.02 per GB |

**Impact**: For 100GB of inter-zone traffic monthly, AWS costs $1.00 vs Azure $2.00. AWS is 50% cheaper for multi-AZ architectures within a region.

## 5. Discount Mechanisms

### AWS Savings Plans
- **Commitment**: 1 or 3 year term, hourly spend commitment
- **Discount**: Up to 72% off On-Demand pricing  
- **Flexibility**: Applies across EC2 instance families, regions, and to Lambda/Fargate

### Azure Reserved Instances + Hybrid Benefit
- **Reserved Instances**: 1 or 3 year term for specific VM size, up to 72% off
- **Azure Hybrid Benefit**: Use existing Windows Server licenses to save up to 40% on OS costs
- **Combined**: RI + AHUB can cut Windows VM costs by 80%+ vs pay-as-you-go

## 6. Cost Comparison Summary

| Scenario | Winner | Monthly Cost | Key Reason |
| --- | --- | --- | --- |
| **Linux Workload** | **AWS** | $29.53 vs $46.24 | t3.small is 36% cheaper, AWS egress fees lower |
| **Windows Workload** | **Azure** | $25.14 vs $29.53+ | Hybrid Benefit eliminates Windows license fees |
| **Multi-AZ Architecture** | **AWS** | $1/100GB vs $2/100GB | Cheaper inter-zone data transfer |

## 7. Cost-Optimization Strategies

1. **Use ARM/Graviton processors**: Migrate from x86 to AWS Graviton3 `t4g.small` or Azure Ampere Altra `B2ps_v2`. ARM instances are 20-40% cheaper than equivalent x86 for web workloads with same performance.

2. **Leverage 1-year commitment discounts**: For steady-state production workloads, use AWS Compute Savings Plans or Azure Reserved Instances. Saves 40-60% vs pay-as-you-go. For Windows workloads, always combine Azure RI with Hybrid Benefit for maximum savings.
