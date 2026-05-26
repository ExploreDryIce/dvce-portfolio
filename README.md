# DVCE — Dynamic Value Chain Engine

> Graph-based supply chain disruption intelligence with Monte Carlo risk quantification

## What This Is

An agentic intelligence engine that models global supply chains as directed graphs, propagates real-world disruption signals through dependency relationships, and translates cascading impacts into actionable financial risk metrics.

**Built for**: Supply chain risk analysts, consulting engagements, strategic resilience planning.

---

## Core Capabilities

### Signal Propagation Engine
- Priority-queue (max-impact-first) traversal guarantees optimal path discovery
- Temporal awareness: lead times, signal duration, pipeline inventory absorption
- Cycle detection with amplification modeling

### Financial Risk Quantification
- Monte Carlo VaR (95%, 99%) and Expected Shortfall
- Revenue-at-risk by product line with buffer depletion physics
- Mitigation scenario ROI computation

### What-If Analysis
- Clone graph, modify edges, compare baseline vs. modified outcomes
- Sensitivity analysis: identify which edges matter most
- Prescriptive recommendations with cost-benefit quantification

### Compound Signal Analysis
- Multiple simultaneous disruptions with configurable correlation models
- Independent, correlated, and additive combination strategies

### Interactive Dashboard
- 6-page Streamlit application with Plotly visualizations
- Real-time propagation heatmaps on network graphs

---

## Architecture

| Layer | Technology | Purpose |
|-------|-----------|---------|
| Graph Store | Neo4j | Supply chain topology |
| Event Bus | NATS JetStream | Async signal distribution with buffering |
| Object Storage | S3/MinIO | Append-only audit trail with hash-chain integrity |
| Compute | Python + NumPy/SciPy | Propagation engine, Monte Carlo |
| Orchestration | LangGraph | Multi-agent reasoning |
| Dashboard | Streamlit + Plotly | Operator interface |

### Deployment Tiers
- **Local**: Docker Compose (Neo4j, Postgres, NATS, MinIO)
- **Gamma**: Pre-production with production constraints
- **Production**: AWS EKS + AuraDB + Aurora + S3 + Bedrock

---

## Sample Output

### Neon Shortage Propagation (severity=0.75)

```
Origin: raw-neon-ukraine
  tier2-tsmc-hsinchu        impact=0.69  hop=1  time=30d
  tier2-infineon-dresden    impact=0.66  hop=1  time=14d
  tier1-bosch-stuttgart     impact=0.66  hop=2  time=212d
  tier1-continental-hanover impact=0.46  hop=2  time=104d
  oem-bmw-munich            impact=0.56  hop=3  time=226d
  oem-vw-wolfsburg          impact=0.47  hop=3  time=224d

Total Impact: 4.11 | VaR 99: EUR 2.8B | Revenue at Risk 30d: EUR 3.1B
```

### What-If: Diversify Neon Supply

```
Modification: Reduce Ukrainian neon dependency 0.92 to 0.50
Result: Total impact reduced by 43 percent
        3 nodes no longer critically affected
        ROI on 50M diversification investment: 18.7x
```

---

## Technical Highlights

- **Correctness**: Max-heap traversal guarantees optimal impact paths
- **Uncertainty**: Monte Carlo with configurable edge weight noise produces confidence intervals
- **Temporal Physics**: Lead times and buffer stocks modeled correctly
- **Prescriptive**: What-if analysis with ROI on mitigations
- **Tested**: 59 tests covering propagation, financial math, temporal behavior

---

## Graph Model

18-node automotive supply chain: European OEMs (BMW, VW) with full dependency tree through Tier-1 (Bosch, Continental, Denso), Tier-2 (TSMC, Infineon, Samsung, CATL), Tier-3 (Siltronic, SUMCO), raw materials (Ukrainian neon, Russian palladium, Chilean lithium), energy (Taiwan grid), and logistics hubs (Rotterdam, Kaohsiung, Singapore).

---

## Author

Solo-built engineering project demonstrating:
- Complex systems modeling and graph algorithms
- Financial risk quantification (VaR, ES, Monte Carlo)
- Production-grade Python architecture (ABCs, tier-based deployment, hash-chain integrity)
- Interactive data applications (Streamlit + Plotly)
- Comprehensive testing (property-based, integration, scenario validation)

*Proprietary. Architecture shown for portfolio purposes. Full implementation available on request.*