# Microgrid Optimization Model

This repository contains the implementation of a Microgrid Optimization Model designed to manage energy sources, battery storage, and grid interactions with the goal of minimizing costs while meeting customer energy demands.

## Model Overview

The model optimizes the energy flow in a microgrid system consisting of renewable energy sources (solar and wind), non-renewable energy sources, and battery storage. The primary objectives are to minimize the cost of purchasing non-renewable energy and to maximize revenue from selling energy while ensuring that customer demand is met.

## Model Assumptions

1. **Battery Efficiency**:
   - The battery retains its charge from one time state to the next, with no idle depletion.
   - The charging efficiency is \( 95\% \), meaning the actual charge rate is \( 0.95 \times \text{charge\_rate} \).
   - The discharging efficiency is \( 95\% \), meaning the actual discharge rate is \( \frac{\text{discharge\_rate}}{0.95} \).

2. **Energy Sources**:
   - Two source nodes: one supplying renewable energy (free) and the other supplying energy at the market price.

3. **Cost Minimization**:
   - The model prioritizes minimizing costs, which means the grid imports only enough energy to satisfy demand.

4. **Battery Constraints**:
   - The maximum charging and discharging rate is \( 250 \text{ kWh} \), considered before efficiency losses.
   - The battery has a minimum charge state of \( 100 \text{ kWh} \) and a maximum charge state of \( 950 \text{ kWh} \).

## Model Parameters

- **T**: Set of time indices from \( 0 \) to \( 11 \).
- **Max\_Cap**: Maximum capacity of the battery = \( 1000 \text{ kWh} \).
- **Charge\_Eff**: Charging efficiency of the battery = \( 0.95 \).
- **Discharge\_Eff**: Discharging efficiency of the battery = \( 0.95 \).
- **Min\_C\_State**: Minimum charge the battery can hold = \( 100 \text{ kWh} \).
- **Max\_C\_State**: Maximum charge the battery can hold = \( 950 \text{ kWh} \).
- **Max\_Charge\_Discharge\_Rate**: Maximum energy that can be charged or discharged = \( 250 \text{ kWh} \).
- **Price**: The price matrix for energy bought or sold at each time index.
- **Demand**: Customer demand at each time index.
- **Solar\_Gen**: Solar energy generation at each time index.
- **Wind\_Gen**: Wind energy generation at each time index.
- **Initial\_Charge**: Initial charge of the battery = \( 500 \text{ kWh} \).

## Model Decision Variables

- **B\_State**: Energy (kWh) stored by the battery at time index \( t \).
- **Charge\_R**: Energy (kWh) used to charge the battery at time index \( t \).
- **Discharge\_R**: Energy (kWh) discharged from the battery at time index \( t \).
- **Grid\_I**: Energy (kWh) imported into the grid at time index \( t \).
- **Grid\_E**: Energy (kWh) exported to customers to satisfy demand at time index \( t \).
- **Relative\_Flow**: Energy (kWh) imported but not stored in the battery, instead sent directly to grid export.
- **Charge\_Binary**: Binary variable restricting simultaneous charging and discharging of the battery.
- **Energy\_Bought**: Non-renewable energy (kWh) purchased at each time index \( t \).

## Objective Function

The objective is to minimize the total cost of purchasing energy while maximizing the revenue from selling energy that meets customer demand.

<!-- $$\text{Minimize} \quad \sum_{t \in T} \text{Price}_t \times \left(\text{Energy\_Bought}_t - \text{Grid\_E}_t\right)$$ -->
$$\text{Minimize} \quad \sum_{t \in T} \text{Price}_t \times (\text{EnergyBought}_t - \text{GridE}_t)$$

## Model Constraints

1. **Battery Constraint**:
   $$
   \text{B\_State}(t) = \text{B\_State}(t-1) + \text{Charge\_Eff} \times \text{Charge\_R}(t) - \frac{\text{Discharge\_R}(t)}{\text{Discharge\_Eff}} \quad \text{for all } t \in \{0, 1, \dots, 11\}
   $$

2. **Minimum Battery State**:
   $$
   \text{B\_State}(t) \geq \text{Min\_C\_State} \quad \text{for all } t \in \{0, 1, \dots, 11\}
   $$

3. **Maximum Battery State**:
   $$
   \text{B\_State}(t) \leq \text{Max\_C\_State} \quad \text{for all } t \in \{0, 1, \dots, 11\}
   $$

4. **Grid Import Constraint**:
   $$
   \text{Grid\_I}(t) = \text{Charge\_R}(t) + \text{Relative\_Flow}(t) \quad \text{for all } t \in \{0, 1, \dots, 11\}
   $$

5. **Grid Export Constraint**:
   $$
   \text{Grid\_E}(t) = \text{Discharge\_R}(t) + \text{Relative\_Flow}(t) \quad \text{for all } t \in \{0, 1, \dots, 11\}
   $$

6. **Source Energy Constraint**:
   $$
   \text{Grid\_I}(t) \leq \text{Solar\_Gen}(t) + \text{Wind\_Gen}(t) + \text{Energy\_Bought}(t) \quad \text{for all } t \in \{0, 1, \dots, 11\}
   $$

7. **Customer Demand Constraint**:
   $$
   \text{Grid\_E}(t) = \text{Demand}(t) \quad \text{for all } t \in \{0, 1, \dots, 11\}
   $$

8. **Charge Binary Constraint**:
   $$
   \frac{\text{Charge\_R}(t)}{\text{Max\_Charge\_Discharge\_Rate}} \leq \text{Charge\_Binary}(t) \quad \text{for all } t \in \{0, 1, \dots, 11\}
   $$

9. **Discharge Binary Constraint**:
   $$
   \frac{\text{Discharge\_R}(t)}{\text{Max\_Charge\_Discharge\_Rate}} \leq 1 - \text{Charge\_Binary}(t) \quad \text{for all } t \in \{0, 1, \dots, 11\}
   $$

## How to Use

1. Clone the repository:
    ```bash
    git clone https://github.com/buckyball99/Microgrid-Energy-Optimization.git
    cd Microgrid-Energy-Optimization
    ```

2. Install the necessary dependencies:
    ```bash
    pip install -r requirements.txt
    ```

3. Run the optimization model

4. Analyze the results:
    The results will be saved as output files, and relevant plots will be generated to visualize the energy flow, costs, and renewable energy utilization.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
