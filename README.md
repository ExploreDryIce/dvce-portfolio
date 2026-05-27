# DVCE — Dynamic Value Chain Engine

> Graph-based supply chain disruption intelligence with Monte Carlo risk quantification

**[Live Demo](https://dvce-dvce.streamlit.app/)** — click to interact with the engine directly.

---

## What This Is

An agentic intelligence engine that models global supply chains as directed graphs, propagates real-world disruption signals through dependency relationships, and translates cascading impacts into actionable financial risk metrics.

**Built for**: Supply chain risk analysts, consulting engagements, strategic resilience planning.

---

## Live Demo

**[https://dvce-dvce.streamlit.app](https://dvce-dvce.streamlit.app/)**

The dashboard lets you:
- Inject disruption signals (geopolitical, climate, logistics) and watch them cascade
- See financial risk metrics (VaR, Expected Shortfall, revenue-at-risk)
- Run what-if scenarios (change supplier dependencies, see risk change)
- Identify the most critical edges via sensitivity analysis
- Analyze compound disruptions with correlation models

---

## How the Propagation Engine Works

The core algorithm uses a priority queue to guarantee optimal impact path discovery:

```python
class CascadeEngine:
    """Priority-queue propagation with temporal awareness and uncertainty."""

    def propagate(self, signal_id, origin_node_ids, severity, signal_duration_days=None):
        # Max-heap ensures highest-impact paths are explored first
        heap = []  # (-impact, counter, node_id, depth, path, cumulative_time)

        for origin_id in origin_node_ids:
            heapq.heappush(heap, (-severity, counter, origin_id, 0, [origin_id], 0.0))

        while heap:
            neg_impact, _, current_id, depth, path, cumulative_time = heapq.heappop(heap)
            current_impact = -neg_impact

            if current_id in finalized:
                continue
            finalized.add(current_id)

            for edge in graph.get_downstream_edges(current_id):
                # Attenuate impact by dependency strength at each hop
                propagated_impact = current_impact * edge["dependency_strength"]

                # Temporal: skip if signal ends before impact arrives
                new_time = cumulative_time + edge["lead_time_days"]
                if signal_duration_days and new_time > signal_duration_days:
                    continue  # Pipeline inventory absorbs it

                # Cycle detection
                if target_id in path:
                    cycles_detected.append(path + [target_id])
                    continue

                if propagated_impact > best_impact.get(target_id, 0):
                    best_impact[target_id] = propagated_impact
                    heapq.heappush(heap, (-propagated_impact, counter, target_id, ...))
```

Key properties:
- **Correctness**: Max-heap guarantees optimal paths (not BFS approximation)
- **Temporal physics**: Lead times and buffer stocks prevent false cascades
- **Uncertainty**: Monte Carlo mode perturbs edge weights 500x for confidence intervals
- **What-if**: Clone graph, modify edges, compare outcomes with ROI computation

---

## Architecture

```
Signal Ingestion -> Graph Propagation -> Metric Computation -> Adversarial Simulation
```

| Layer | Technology | Purpose |
|-------|-----------|---------|
| Graph Store | Neo4j | Supply chain topology (nodes, edges, dependencies) |
| Event Bus | NATS JetStream | Async signal distribution with local buffering |
| Object Storage | S3/MinIO | Append-only audit trail with hash-chain integrity |
| Compute | Python | Propagation engine, Monte Carlo VaR simulation |
| Orchestration | LangGraph | Multi-agent reasoning and theory refinement |
| Dashboard | Streamlit + Plotly | Interactive operator interface |

### Deployment Tiers
- **Local**: Docker Compose (Neo4j, Postgres, NATS, MinIO)
- **Gamma**: Pre-production with production constraints
- **Production**: AWS EKS + AuraDB + Aurora + S3 + Bedrock

---

## Financial Risk Quantification

```python
class FinancialRiskEngine:
    """Monte Carlo VaR with correct buffer depletion physics."""

    def assess_risk(self, propagation, time_horizon_days=30, monte_carlo_runs=500):
        # For each product line, compute:
        # - halt_probability = f(max impact on critical dependencies)
        # - days_to_stop = buffer_days / impact_score  (depletion rate)
        # - revenue_at_risk = daily_revenue * halt_prob * days_exposed

        # Monte Carlo: perturb edge weights + severity N times,
        # collect loss distribution, take quantiles for VaR/ES
        for _ in range(monte_carlo_runs):
            perturbed_result = engine.propagate(perturbed_params)
            loss_distribution.append(compute_loss(perturbed_result))

        var_95 = loss_distribution[int(0.95 * n)]
        var_99 = loss_distribution[int(0.99 * n)]
        es_97_5 = mean(loss_distribution[int(0.975 * n):])
```

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

VaR 99%: EUR 2.8B | Revenue at Risk (30d): EUR 3.1B
```

### What-If: Diversify Neon Supply
```
Modification: Reduce Ukrainian neon dependency 0.92 -> 0.50
Result: Total impact reduced by 43%
        3 nodes no longer critically affected
        ROI on 50M diversification: 18.7x
```

---

## Graph Model

18-node automotive supply chain: European OEMs (BMW, VW) through Tier-1 (Bosch, Continental, Denso), Tier-2 (TSMC, Infineon, Samsung, CATL), Tier-3 (Siltronic, SUMCO), raw materials (Ukrainian neon, Russian palladium, Chilean lithium), energy (Taiwan grid), and logistics (Rotterdam, Kaohsiung, Singapore).

---

## Technical Stats

- **59 tests** passing (propagation correctness, financial math, temporal behavior, what-if immutability)
- **< 0.12s** full test suite execution
- **500-run Monte Carlo** completes in < 1 second
- **Priority-queue traversal** guarantees O(E log V) optimal paths

---

## Built With

Python 3.11+ | Streamlit | Plotly | NetworkX | Pydantic | Neo4j | NATS JetStream | Docker

---

## Author

Solo-built engineering project demonstrating:
- Complex systems modeling and graph algorithms
- Financial risk quantification (VaR, ES, Monte Carlo simulation)
- Production-grade Python architecture (ABCs, tier-based deployment, hash-chain integrity)
- Interactive data applications
- Comprehensive testing (property-based, integration, scenario validation)

---

*Proprietary engine. Architecture and approach shown for portfolio purposes. Full implementation available on request.*
