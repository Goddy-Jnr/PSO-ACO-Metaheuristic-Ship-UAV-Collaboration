# PSO-ACO-Metaheuristic-Ship-UAV-Collaboration
Bi-objective optimization of shipborne UAV location-allocation  for maritime emergency response under wind and ocean current uncertainty.

# Ship–UAV Collaborative Scheduling for Maritime Emergency Response

![![Paper- under review](https://img.shields.io/badge/Journal-Ocean%20Engineering-blue)]()
![![License](https://img.shields.io/badge/License-MIT-green)]()
![![Python](https://img.shields.io/badge/Python-3.8%2B-blue)]()

## Overview

This repository contains the implementation of a bi-objective 
optimization framework for shipborne UAV-based maritime emergency 
response. Rescue ships serve as mobile UAV launch platforms, 
enabling UAVs to penetrate hazardous zones and service multiple 
incident sites within a single deployment — reducing response 
time by **55–75%** over conventional ship-only operation.

The framework simultaneously optimizes:
- **Ship positioning** — where to pre-position rescue vessels
- **Accident-site allocation** — which ship responds to which incident
- **UAV fleet configuration** — how many UAVs each ship carries

---

## Key Features

- **Bi-objective MINLP formulation** minimizing total emergency 
  response time and system deployment cost simultaneously
- **Environmental uncertainty modeling** capturing wind-induced 
  UAV range degradation and ocean current drift on both vessel 
  navigation and UAV flight performance
- **Two-stage hybrid algorithm**:
  - Stage 1: Particle Swarm Optimization (PSO) for continuous 
    ship positioning and accident-site allocation
  - Stage 2: Ant Colony Optimization (ACO) for combinatorial 
    multi-visit UAV routing and fleet configuration
- **KDE-based hotspot prediction** from historical maritime 
  accident records (EMSA/IMO data)
- **Pareto frontier** enabling decision-makers to select 
  configurations matching operational priorities and budgets

---

## Results Summary

| Algorithm  | Mean Response Time (h) | Std Dev (h) |
|------------|----------------------|-------------|
| **PSO-ACO (ours)** | **36.85** | **1.38–1.75** |
| PSO-SA     | 38.99                | —           |
| PSO-GA     | 40.94                | —           |
| PSO-DBSCAN | 43.49                | —           |

**Key findings:**
- PSO-ACO outperforms all baselines by **5.5–15.3%**
- UAV integration reduces response time by **55–75%** vs ships only
- Ignoring environmental effects causes **14–22%** of planned 
  responses to violate critical time-window constraints
- Optimal UAV battery threshold: **130–140 min**
- Payload capacity is the dominant procurement parameter 
  (20→80 kg yields **75% rescue time reduction**)
- Flight range saturates at **30–45 km** depending on area size

---


## Experimental Setup

Four coastal instances tested:

| Instance | Area (km²) | Rescue Stations | Accident Density |
|----------|------------|-----------------|------------------|
| 1        | 110×110    | 10              | 100, 200, 400    |
| 2        | 120×120    | 20              | 100, 200, 400    |
| 3        | 130×130    | 30              | 100, 200, 400    |
| 4        | 140×140    | 40              | 100, 200, 400    |

All experiments: **30 independent runs** per configuration  
Hardware: AMD R7 5800HS, 16 GB RAM

---

## Installation

```bash
git clone https://github.com/username/ship-uav-maritime-rescue.git
cd ship-uav-maritime-rescue
pip install -r requirements.txt
```

---

## Usage

```python
# Run the full PSO-ACO framework
from src.pso import ShipPositioningPSO
from src.aco import UAVRoutingACO

# Stage 1: PSO ship positioning
pso = ShipPositioningPSO(
    n_ships=7,
    wind_speed=8.0,       # m/s
    current_speed=1.0,    # m/s
    n_particles=50,
    max_iter=200
)
X_opt, Z_opt = pso.optimize(accident_locations)

# Stage 2: ACO UAV routing
aco = UAVRoutingACO(
    ship_positions=X_opt,
    allocation=Z_opt,
    battery_capacity=130,  # minutes
    payload_max=50,        # kg
    flight_range=40        # km
)
routes, fleet_config, cost = aco.optimize()
```

---

## Citation

If you use this code in your research, please cite:

```bibtex
@article{ziedor2026shipuav,
  author    = {Ziedor, Godfred and Duo, Bin and Nartey, Obed Tettey and Liu, Tong and 
               Lin, Jie and Luo, Junsong},
  title     = {PSO--{ACO} Metaheuristic Ship--{UAV} Collaborative Scheduling for
                Maritime Emergency Response under Marine Uncertainty},
  journal   = {Ocean Engineering},
  year      = {2026 - under review},
  publisher = {Elsevier}
}
```

---

## Data Sources

- **Historical accident data**: European Maritime Safety Agency 
  (EMSA) Annual Overview of Marine Casualties and Incidents 2022
- **Environmental data**: IMO Global Integrated Shipping 
  Information System (GISIS)

---

## License

This project is licensed under the MIT License. 
See [LICENSE](LICENSE) for details.

---
