# The Strategist's Crucible: Solar Car Race Optimization

## Project Overview
This project focuses on the strategy and performance analysis of a solar-powered vehicle competing in a 9-hour race from Sasolburg to Zeerust. The primary objective is to maximize the distance covered (Total Loops) within the strict time window of 8:00 AM to 5:00 PM, while ensuring the battery never drops below a 20% State of Charge (SOC).

---

## Technical Architecture

### Phase 1: The Cartographer (Route Generation)
In this phase, we modeled the high-fidelity terrain for the base route and the repeated loops.
* **Base Route:** 297 km from Sasolburg to Zeerust, including the rolling hills of the South African Highveld.
* **Hills & Slopes:** We implemented a sinusoidal altitude model to simulate varying slopes, which is critical for calculating gravity's effect ($F = mg \sin\theta$) and regenerative braking.
* **Resolution:** The route is sampled every 50 meters to ensure precision in power calculations.

### Phase 2: The Strategist (Constraint Optimization)
We developed an optimization engine to find the "Sweet Spot" between speed, time, and energy consumption.
* **Objective:** Maximize Loops ($N$) while finishing the final loop as close to 5:00 PM (17:00) as possible.
* **Constraint 1 (Time):** A 30-minute mandatory control stop and 5-minute stops before every loop.
* **Constraint 2 (Energy):** A hard floor of 20% SOC.
* **Logic:** By increasing the loop count and decreasing the speed to use the full 9-hour window, we significantly reduced aerodynamic drag losses, which increase with the cube of velocity ($v^3$).

### Phase 3: The Analyst (Performance Simulation)
We moved from a simplified model to a high-fidelity physics simulation.
* **Inertia ($F=ma$):** We integrated the energy cost of overcoming the car's mass during acceleration.
* **Trapezoidal Ramping:** Instead of digital speed jumps, the car now realistically accelerates/decelerates at a constant $0.5\ m/s^2$.
* **Visual Deliverables:** Generation of Velocity, SOC, and Acceleration profiles plotted against the race timeline.

---

## Strategy Summary

| Parameter | Value |
| :--- | :--- |
| **Completed Loops** | 11 Loops |
| **Total Distance** | 682 km (297km Base + 385km Loops) |
| **Optimal Velocity** | 89.93 km/h |
| **Finish Time** | 17:00:00 (Perfect Time Utilization) |
| **Safety Buffer** | ~38% Final SOC (Above 20% limit) |

---

## Strategic Deep-Dive: Critical Questions

### 1. Why is 12 Loops not feasible?
While 12 loops is theoretically possible within the 9-hour time window (requiring a speed of 95.60 km/h), it fails the **Energy Constraint**.
* **Aerodynamic Wall:** Increasing the speed from 89.93 km/h to 95.60 km/h is a ~6% increase in speed, but it results in a ~20% increase in power consumption due to the cubic drag law ($P \propto v^3$).
* **Energy Deficit:** At 95.60 km/h, the total mechanical energy required to push through the air exceeds the sum of the 5kWh battery capacity and the solar energy gathered during the race.
* **Simulation Result:** Our Phase 2 optimizer flagged 12 loops as "FAILED" because the minimum SOC dipped to approximately 9.6%, violating the 20% safety regulation.

### 2. Why is the Final SOC around 50% in Phase 3 (instead of 38%)?
You may notice a discrepancy between the Phase 2 estimate (38%) and the Phase 3 graph (~50%). This is not an error; it is the result of **higher simulation fidelity**.
* **Acceleration Ramping:** In Phase 2, we assumed the car was at 89.93 km/h for the entire distance. In Phase 3, the car takes time to speed up (Trapezoidal Ramping). During these acceleration phases, the car is moving at lower average speeds, which drastically reduces air resistance losses during those segments.
* **Stationary Solar Gain:** Phase 3 precisely logs the solar energy gained during the 30-minute Zeerust stop and the 55 minutes of total loop stops (11 loops x 5 mins). During these **85 minutes**, the car consumes 0W but continues to gather ~1000W of solar power, acting as a significant recharge period.
* **Regenerative Braking:** Phase 3's terrain-aware model recovers up to 70% of energy during downhill sections of the base route, a factor that was simplified in the initial Phase 2 estimates.

---
