# Agnirath_Solar_Strategy_2026
Repo about the final challenge of agnirath strategy team.
# Agnirath Solar Strategy Challenge - Day 3

## Executive Summary
This repository contains the optimized race strategy for Day 3 of the Sasol Solar Challenge. The primary objective was to maximize total distance while adhering to a strict 5:00 PM cutoff and a 20% State of Charge (SOC) safety floor. 

After performing a constrained optimization, the final strategy focuses on completing **12 loops**, resulting in a total distance of **610 km**. This was identified as the "Efficiency Sweet Spot" for the vehicle's specific aerodynamic profile.

---

## Strategy Parameters
| Parameter | Value |
| :--- | :--- |
| **Total Distance** | 610.00 km |
| **Cruising Speed ($v_{opt}$)** | 81.33 km/h |
| **Optimal Loops ($n_{opt}$)** | 12 |
| **Final SOC** | 38.64% |
| **Arrival Time** | 17:00:00 (Sharp) |

---

## ⚙️ Vehicle Constants
The physics engine utilized the following rigid parameters for all simulations:
* **Total Mass:** 250 kg
* **Aerodynamic Drag ($C_dA$):** 0.0795
* **Rolling Resistance ($C_{rr}$):** 0.005140
* **Battery Capacity:** 5000 Wh
* **Starting SOC:** 95% (4750 Wh)

---

## 🧠 Strategic Logic

### 1. The Time Constraint
To maximize loops, we first calculated the minimum speed required to finish 12 loops within the 9-hour window (8:00 AM - 5:00 PM). Accounting for the 30-minute control stop and 60 minutes of cumulative loop stops (5 min per loop), the driving time available is 7.5 hours.
$$v_{min} = \frac{190 + (12 \times 35)}{7.5} = 81.33 \text{ km/h}$$

### 2. The Energy Trade-off
While the vehicle has the power to drive faster, the cubic relationship between velocity and aerodynamic drag ($P \propto v^3$) makes a 13th loop non-viable under current environmental conditions. 
- **12 Loops:** Finishes with **38.64% SOC**, providing a healthy buffer for unexpected weather or Day 4 requirements.
- **13 Loops:** Requires a speed of ~87 km/h, which would have depleted the battery below the 20% safety limit.

---

## 📁 Repository Structure
- **/code**: Contains the Jupyter Notebook (`.ipynb`) with the SLSQP optimizer.
- **/data**: Includes `simulation_results.csv`, the minute-by-minute telemetry.
- **/plots**: Includes `final_soc_plot.png` showing the battery profile.

---

## 🛠️ How to Run
1. Ensure `pandas`, `numpy`, and `scipy` are installed.
2. Upload `route_environment.csv` to the working directory.
3. Run all cells in `LastChallenge.ipynb` to regenerate the telemetry and plots.

we can also go with 13th loop but then soc will be less then 20% but we can recharge our battery after finishing the race 50 min early and getting SOC>20% in that case but, is it allowed or not was not known by me hence i chose to complete only 12 loops.
