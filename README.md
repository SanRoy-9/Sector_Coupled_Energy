# Sector-Coupled-Urban-Energy-System-Simulation
Physics-based simulation of a sector-coupled urban energy system integrating PV, battery storage, heat pump, and thermal tank over a full-year hourly horizon. Evaluates cost, peak load, self-consumption, and CO₂ emissions under rule-based and optimization dispatch to quantify flexibility and decarbonization potential.

# Berlin-Urban-Energy-Simulation  
---

## Overview

This project presents a structured computational framework for modeling a **sector-coupled urban energy system** representative of a residential building or small district-scale infrastructure in Berlin.

The model integrates:

- An **electric subsystem** consisting of base electrical demand, photovoltaic (PV) generation, battery storage, and grid exchange.
- A **heating subsystem** consisting of space heating demand, heat pump conversion, and a thermal storage tank.
- **External signals**, including time-of-use (TOU) electricity tariffs and time-varying grid CO₂ intensity.

The simulation operates on an **hourly time grid over one full year (8760 hours)**. The framework is designed to evaluate physical consistency, techno-economic performance, environmental impact, and flexibility potential under decarbonization scenarios.

The objective is not merely to simulate energy flows, but to provide a **quantitative and reproducible environment** for analyzing sector coupling, storage dynamics, and system-level trade-offs in urban decarbonization pathways.

---

## Problems Addressed (Real-World Motivation)

Urban energy systems undergoing electrification face multiple interdependent challenges:

### 1. Electrification of Heating and Sector Coupling

Heat pumps increase electricity demand, particularly during winter periods. This coupling introduces:

- Seasonal amplification of electricity demand  
- Increased exposure to peak grid stress  
- Potential mismatch between renewable generation and heating demand  

Understanding the dynamic interdependence between heat demand, renewable production, storage systems, and grid interaction is essential for sustainable urban planning.

---

### 2. Peak Load and Distribution Grid Stress

Electrified heating can intensify:

- Evening peak demand  
- Winter peak demand  
- Transformer and feeder congestion  

Storage coordination and optimized dispatch strategies are required to quantify peak shaving potential and assess grid-supportive behavior.

---

### 3. Exposure to Time-Varying Electricity Pricing

Time-of-use tariffs create heterogeneous cost signals across the day. Without coordinated control:

- Heat pump operation may coincide with high-price hours  
- Electrification may increase annual cost despite decarbonization  

This project evaluates the economic impact of flexible storage dispatch under TOU pricing structures.

---

### 4. Temporal Variability of Carbon Intensity

Grid carbon intensity varies hourly and seasonally. Electrification reduces emissions only if electricity consumption aligns with lower-carbon hours.

The framework quantifies:

- Hourly and cumulative emissions  
- Seasonal concentration effects  
- Carbon intensity of supplied energy  

---

### 5. Storage Investment and System Design Uncertainty

Battery and thermal storage sizing decisions require:

- Diminishing return analysis  
- Sensitivity evaluation  
- Peak reduction quantification  

The simulation enables systematic parameter variation to assess performance trade-offs.

---

## System Model

### Electric Subsystem

At each timestep, the electricity balance includes:

- PV generation  
- Base electrical load  
- Heat pump electrical consumption  
- Battery charging and discharging  
- Grid import and export  

Battery state-of-charge (SOC) dynamics are governed by:

Charging:
\[
SOC_{t+1} = SOC_t + \eta_{ch} P_{ch} \Delta t
\]

Discharging:
\[
SOC_{t+1} = SOC_t - \frac{P_{dis} \Delta t}{\eta_{dis}}
\]

Constraints enforced:
- SOC bounds  
- Maximum charge/discharge power  
- Efficiency losses  

---

### Heating Subsystem

Space heating demand is approximated via a heat-loss formulation:

\[
Q_{heat} = UA \cdot (T_{indoor} - T_{outdoor})
\]

Heat pump heat production is modeled as:

\[
Q_{hp} = COP(T_{out}) \cdot P_{hp}
\]

Thermal tank dynamics incorporate:

- Heat inflow from the heat pump  
- Heat outflow to meet demand  
- Standby heat losses  

Tank energy bounds are enforced at every timestep.

---

### External Signals

The model incorporates time-dependent economic and environmental signals:

- TOU import price profile  
- Feed-in tariff for exports  
- Hourly grid CO₂ intensity  

Operational cost per hour:

\[
Cost_t = Import_t \cdot Price_{import,t} - Export_t \cdot Price_{export}
\]

CO₂ emissions per hour:

\[
CO2_t = Import_{kWh,t} \cdot \frac{CO2_{grid,t}}{1000}
\]

---

## Configuration

The framework is fully parameterized to allow systematic experimentation.

### Battery Parameters
- Energy capacity (kWh)  
- Power limits (kW)  
- Charge/discharge efficiency  
- SOC bounds  

### Heat Pump Parameters
- Maximum electrical input  
- Minimum and maximum COP  

### Thermal Tank Parameters
- Energy capacity (kWh_th)  
- Minimum allowable energy  
- Standby loss rate  

### Economic Parameters
- TOU price profile  
- Export tariff  

### Carbon Parameters
- Seasonal and diurnal intensity model  
- Optional CO₂ penalty in optimization objective  

### Optimization Parameters
- Rolling-horizon length  
- Objective weighting  

---

## Results & Expected Behavior

### Baseline Case (No Storage)

- Elevated annual cost  
- Higher peak grid imports  
- Limited PV self-consumption  
- No load-shifting capability  

### Rule-Based Dispatch

- Increased PV self-consumption  
- Reduced peak imports relative to baseline  
- Electrified heating shifts electricity demand seasonally  
- Emissions concentrated in winter and evening peak hours  

### Optimization-Based Dispatch

- Reduced annual cost under TOU pricing  
- Improved peak reduction performance  
- Potential CO₂ reduction when aligned with low-carbon hours  
- Provides theoretical performance benchmark  

---

## Findings

- Sector coupling significantly reshapes urban electricity demand profiles.
- Thermal storage enables temporal shifting of heat pump operation.
- Battery storage increases PV self-consumption and system autonomy.
- Emission concentration is seasonally dominated by winter heating demand.
- Optimization establishes an upper bound on achievable cost and peak reduction.
- Electrification alone does not guarantee emission reduction; timing alignment is critical.
- The numerical framework remains stable over full-year simulation horizons.

---

## Reproducibility

- Synthetic input profiles are generated using fixed random seeds.
- Simulation outputs are deterministic for fixed parameter sets.
- All time-series data are exported in structured CSV format.
- All figures are generated programmatically and saved automatically.

Required dependencies:
- Python 3.x  
- NumPy  
- Pandas  
- Matplotlib  
- SciPy (for optimization module)  

---

## Limitations

- Synthetic demand and PV data (no measured Berlin datasets)
- Simplified COP model (no part-load or defrost modeling)
- Simplified building thermal model (UA approximation)
- Linearized tank-loss representation in optimization module
- No electric vehicle integration
- No multi-building aggregation
- No distribution network constraint modeling

---

## Conclusion

This project demonstrates the structured development of a sector-coupled urban energy system simulation integrating:

- Multi-energy balance modeling  
- Storage state dynamics  
- Heat pump thermodynamic approximation  
- Time-series economic evaluation  
- Carbon accounting with variable intensity  
- Constrained optimization benchmarking  

The framework provides a coherent computational environment for evaluating decarbonization strategies in urban energy systems and supports rigorous quantitative analysis of flexibility, cost, and environmental performance.

---

## Future Work

Potential extensions include:

- Integration of real Berlin load and PV datasets  
- Incorporation of measured German grid CO₂ intensity signals  
- Electric vehicle charging integration  
- Demand response modeling  
- Multi-objective optimization (cost–CO₂ Pareto analysis)  
- Monte Carlo uncertainty quantification  
- Higher-fidelity RC building thermal models  
- Storage capacity optimization  
- Inclusion of grid-level constraints  

---

## Academic Context

This work reflects a structured integration of:

- Energy systems engineering  
- Thermodynamic modeling  
- Storage and state evolution dynamics  
- Techno-economic analysis  
- Carbon accounting methodologies  
- Constrained linear optimization  

It demonstrates the translation of physical and economic system models into a stable computational framework capable of full-year simulation and rigorous performance evaluation.

The project provides a foundation for advanced research in sustainable energy systems, energy transition modeling, and urban decarbonization analysis.
