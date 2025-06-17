# GCP Cost Overview for Distributed Ledger Technology (DLT) Nodes

## 1. Introduction and Purpose

This document provides a detailed cost overview for deploying and managing Distributed Ledger Technology (DLT) nodes on the Google Cloud Platform (GCP). The purpose is to estimate potential monthly costs for Development (Devnet), Testnet, and Production environments, considering various configurations and discount options, based on a specifically defined `standard-node` profile and pricing. This analysis aims to help in budget planning and making informed decisions regarding resource allocation and cost optimization strategies.

## 2. Assumptions

The following assumptions form the basis of the cost estimations:

*   **Region:** `us-central1` (Iowa) is used for all pricing calculations.
*   **Operating System (OS):** Standard Linux OS (e.g., Ubuntu) is assumed, with no premium OS license costs included.
*   **Node Profile (`standard-node`):**
    *   vCPUs: 4
    *   Memory: 16 GB
    *   Primary Boot Disk: 50 GB (Standard Persistent Disk - `pd-standard`)
    *   Additional Data Disk: 250 GB (Standard Persistent Disk - `pd-standard`)
*   **VM Machine Types & Hourly Rates (us-central1):**
    *   **`e2-standard-4`:**
        *   On-Demand: $0.13402284/hour
        *   1-Year CUD (blended vCPU & Memory): $0.08443432/hour
        *   3-Year CUD (blended vCPU & Memory): $0.06031032/hour
    *   **`n4-standard-4` (Alternative):**
        *   On-Demand: $0.189544/hour
        *   1-Year CUD (blended vCPU & Memory): $0.11941272/hour
        *   3-Year CUD (blended vCPU & Memory): $0.0852948/hour
*   **Storage Cost:**
    *   Total per node (50GB boot + 250GB data = 300GB `pd-standard`): **$12.00/month** (as provided).
*   **Networking:**
    *   Intra-node communication within the same zone is generally free.
    *   Inter-node communication across zones or regions, and external traffic (ingress/egress) will incur costs. A qualitative discussion is provided.
*   **Pricing Models:**
    *   On-Demand: Pay-as-you-go pricing.
    *   Committed Use Discounts (CUDs): Applied for 1-year or 3-year commitments to vCPU and memory.
*   **Currency:** All costs are in USD ($).
*   **Monthly Calculation Basis:** Costs are estimated on a monthly basis, assuming **730 hours per month** (24 hours/day * 30.44 days/month approx.).

## 3. Cost Estimation Methodology

The cost for each environment is calculated by summing the costs of:

1.  **Compute Instances:** Based on the machine type, hourly rate (On-Demand or CUD), and 730 hours per month.
    *   Monthly Compute Cost per Node = Hourly Rate * 730
2.  **Storage:** Fixed at $12.00 per node per month as per assumptions.
3.  **Total Cost per Node:** Monthly Compute Cost + Monthly Storage Cost.
4.  **Total Environment Cost:** Total Cost per Node * Number of Nodes.
5.  **Networking:** Discussed qualitatively.

Target total monthly costs for `e2-standard-4` based environments are provided and will be the basis for verification.

## 4. Detailed Cost Breakdowns

### 4.1. `standard-node` Profile Cost (per node, per month)

#### 4.1.1. E2-standard-4 (4 vCPU, 16 GB RAM)

*   **Storage Cost (fixed):** $12.00/month

*   **Compute Costs (per node, per month):**
    *   On-Demand: $0.13402284/hour * 730 hours = ~$97.8366732/month  (approx. $97.84)
    *   1-Year CUD: $0.08443432/hour * 730 hours = ~$61.6370536/month (approx. $61.64)
    *   3-Year CUD: $0.06031032/hour * 730 hours = ~$44.0265336/month (approx. $44.03)

*   **Total Cost per e2-standard-4 node per month:**
    *   On-Demand: $97.8366732 + $12.00 = **~$109.8366732**
    *   1-Year CUD: $61.6370536 + $12.00 = **~$73.6370536**
    *   3-Year CUD: $44.0265336 + $12.00 = **~$56.0265336**

#### 4.1.2. N4-standard-4 (Alternative - 4 vCPU, 16 GB RAM)

*   **Storage Cost (fixed):** $12.00/month

*   **Compute Costs (per node, per month):**
    *   On-Demand: $0.189544/hour * 730 hours = ~$138.36712/month (approx. $138.37)
    *   1-Year CUD: $0.11941272/hour * 730 hours = ~$87.1712856/month (approx. $87.17)
    *   3-Year CUD: $0.0852948/hour * 730 hours = ~$62.265204/month (approx. $62.27)

*   **Total Cost per n4-standard-4 node per month (Estimated):**
    *   On-Demand: $138.36712 + $12.00 = **~$150.36712**
    *   1-Year CUD: $87.1712856 + $12.00 = **~$99.1712856**
    *   3-Year CUD: $62.265204 + $12.00 = **~$74.265204**

### 4.2. Devnet Environment (4 Nodes)

#### 4.2.1. Using e2-standard-4 Nodes

*   **Target On-Demand Total:** ~$439.35/month
*   **Calculated On-Demand per node:** $109.8366732
*   **Calculated Total On-Demand:** 4 nodes * $109.8366732/node = **~$439.3466928/month** (Matches target of $439.35)

*   **Target 1-Year CUD Total:** ~$294.55/month
*   **Calculated 1-Year CUD per node:** $73.6370536
*   **Calculated Total 1-Year CUD:** 4 nodes * $73.6370536/node = **~$294.5482144/month** (Matches target of $294.55)

*   **Summary for Devnet (e2-standard-4):**
    *   **On-Demand: ~$439.35/month**
    *   **1-Year CUD: ~$294.55/month**

#### 4.2.2. Using n4-standard-4 Nodes (Estimated)

*   **On-Demand:** 4 nodes * $150.36712/node = **~$601.47/month**
*   **1-Year CUD:** 4 nodes * $99.1712856/node = **~$396.69/month**

#### 4.2.3. Networking Costs (Devnet)

*   Relatively low. Primarily inter-node communication for sync and consensus.
*   External traffic for developers accessing nodes or dashboards.
*   Egress costs are expected to be minimal with `pd-standard` and lower activity.
*   **Estimated:** < $30/month, but can vary.

### 4.3. Testnet Environment (7 Nodes)

#### 4.3.1. Using e2-standard-4 Nodes

*   **Target On-Demand Total:** ~$768.86/month
*   **Calculated Total On-Demand:** 7 nodes * $109.8366732/node = **~$768.8567124/month** (Matches target of $768.86)

*   **Target 1-Year CUD Total:** ~$515.46/month
*   **Calculated Total 1-Year CUD:** 7 nodes * $73.6370536/node = **~$515.4593752/month** (Matches target of $515.46)

*   **Summary for Testnet (e2-standard-4):**
    *   **On-Demand: ~$768.86/month**
    *   **1-Year CUD: ~$515.46/month**

#### 4.3.2. Using n4-standard-4 Nodes (Estimated)

*   **On-Demand:** 7 nodes * $150.36712/node = **~$1052.57/month**
*   **1-Year CUD:** 7 nodes * $99.1712856/node = **~$694.20/month**

#### 4.3.3. Networking Costs (Testnet)

*   Higher than Devnet due to more nodes and potentially more simulated transaction load.
*   Increased inter-node communication. External traffic for testing dApps, wallets, and explorers.
*   **Estimated:** $30 - $100/month, depending on testing intensity.

### 4.4. Production Environment (21 Nodes)

#### 4.4.1. Using e2-standard-4 Nodes

*   **Target On-Demand Total:** ~$2306.57/month
*   **Calculated Total On-Demand:** 21 nodes * $109.8366732/node = **~$2306.5701372/month** (Matches target of $2306.57)

*   **Target 1-Year CUD Total:** ~$1546.38/month
*   **Calculated Total 1-Year CUD:** 21 nodes * $73.6370536/node = **~$1546.3781256/month** (Matches target of $1546.38)

*   **Target 3-Year CUD Total:** ~$1176.56/month
*   **Calculated 3-Year CUD per node:** $56.0265336
*   **Calculated Total 3-Year CUD:** 21 nodes * $56.0265336/node = **~$1176.5572056/month** (Matches target of $1176.56)

*   **Summary for Production (e2-standard-4):**
    *   **On-Demand: ~$2306.57/month**
    *   **1-Year CUD: ~$1546.38/month**
    *   **3-Year CUD: ~$1176.56/month**

#### 4.4.2. Using n4-standard-4 Nodes (Estimated)

*   **On-Demand:** 21 nodes * $150.36712/node = **~$3157.71/month**
*   **1-Year CUD:** 21 nodes * $99.1712856/node = **~$2082.60/month**
*   **3-Year CUD:** 21 nodes * $74.265204/node = **~$1559.57/month**

#### 4.4.3. Networking Costs (Production)

*   Potentially significant, depending on DLT transaction volume, peer connections, and external API calls.
*   High availability often requires nodes in multiple zones or regions, increasing cross-zone/region traffic costs.
*   Egress to users (explorers, wallets, applications) and other DLT networks.
*   With smaller `pd-standard` disks, I/O related network impact might be different than with SSDs, but P2P traffic remains the primary concern.
*   **Estimated:** $100 - $700+/month. This is highly variable and needs monitoring.

## 5. Spot VMs for Production

*   **Potential Savings:** Spot VMs (using on-demand `e2-standard-4` hourly rate of $0.13402284 as a base before Spot discount) could reduce compute costs by 60-91%. For an `e2-standard-4`, the compute portion is ~$97.84/month. A 70% saving would be ~$68.49 per node.
*   **Risks:** Spot VMs can be preempted. Unsuitable for critical consensus nodes unless the DLT is highly fault-tolerant to such interruptions.
*   **Recommendation:**
    *   Consider for a small percentage of non-critical observer/RPC nodes in production (e.g., 3-5 out of 21 nodes).
    *   If 4 production nodes run on Spot VMs, potential compute savings: 4 * $68.49 = ~$273.96/month.
    *   Thorough testing is essential.

## 6. Other Cost Factors & Considerations

*   **Different Node Profiles:** Light nodes or specialized nodes would alter these costs.
*   **Right-Sizing:** Crucial. The `standard-node` profile is a baseline; individual nodes may need adjustment.
*   **Region Choice:** Costs vary between regions. `us-central1` is used here.
*   **Premium OS Licenses:** Not included.
*   **Backups and Snapshots:** For 300GB `pd-standard` disks, snapshots cost $0.0195/GB-month (for regional standard snapshots). A snapshot for one node: 300GB * $0.0195 = $5.85/snapshot/month. Regular snapshots for all production nodes (21 * $5.85) would be ~$122.85/month per full backup set.
*   **Monitoring (Cloud Monitoring):** Basic tier is free; advanced features or high volumes can add costs.
*   **Sustained Usage Discounts (SUDs):** Automatically applied to on-demand if CUDs are not used, but CUDs offer better rates.
*   **Load Balancers:** For public RPC endpoints, add ~$18/month per forwarding rule + data processed.
*   **Cloud NAT:** For private nodes needing outbound access, ~$0.0045/hour per NAT gateway + data processed.
*   **Data Transfer Costs:** Egress is key. P2P traffic for DLTs can be substantial.

## 7. Recommendations for Cost Optimization

1.  **Commit to Usage (CUDs):** Essential for production. 3-year CUDs on `e2-standard-4` offer significant savings (total production cost from ~$2307 down to ~$1177/month).
2.  **Right-Size Instances & Storage:** The current `standard-node` profile with 250GB data disk is much more cost-effective than prior 1TB SSD assumptions. Continuously verify if this size is optimal.
3.  **Optimize Storage Type:** `pd-standard` is used here for cost. If performance becomes an issue for specific nodes, consider `pd-balanced` ($0.06/GB-month) or `pd-ssd` (approx. $0.17/GB-month for zonal SSD in us-central1) selectively. For 250GB SSD, this would be $42.50/month (250GB * $0.17/GB) instead of part of the $12/month flat for pd-standard.
4.  **Network Tier Selection:** Standard Tier for external traffic if acceptable.
5.  **Monitor Network Egress:** Use VPC Flow Logs.
6.  **Leverage Spot VMs (Cautiously):** As discussed, for a subset of non-critical nodes.
7.  **Shutdown Dev/Test Resources:** If feasible.
8.  **Regular Cost Reviews:** Use GCP Billing tools.

## 8. Conclusion and Disclaimer

This document outlines estimated GCP costs for DLT node deployment based on the specified `standard-node` profile (4vCPU, 16GB RAM, 50GB boot + 250GB data `pd-standard` disks) and hourly rates. The target monthly costs have been met:

*   **Devnet (4 e2-standard-4 nodes):**
    *   On-Demand: **~$439.35/month**
    *   1-Year CUD: **~$294.55/month**
*   **Testnet (7 e2-standard-4 nodes):**
    *   On-Demand: **~$768.86/month**
    *   1-Year CUD: **~$515.46/month**
*   **Production (21 e2-standard-4 nodes):**
    *   On-Demand: **~$2306.57/month**
    *   1-Year CUD: **~$1546.38/month**
    *   3-Year CUD: **~$1176.56/month**

Alternative `n4-standard-4` instances are projected to be more expensive:
*   Production (21 n4-standard-4 nodes, 3-Yr CUD): ~$1559.57/month.

Actual costs can vary based on precise final configurations, real-time usage (especially network), GCP pricing updates, and specific data storage performance needs.

**Disclaimer:** These estimations are for planning purposes, based on the provided inputs. It is highly recommended to use the GCP Pricing Calculator for any specific project variations and to conduct a proof-of-concept to validate actual spending. Continuous monitoring and optimization are crucial.
