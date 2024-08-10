# Microgrid-Energy-Optimization
A Pyomo LPP model for optimizing the energy flow in a microgrid with a Battery.

This repository contains a Pyomo-based optimization model for managing a microgrid's energy resources, including renewable energy sources, non-renewable energy sources, and battery storage. The goal of the model is to minimize the total energy cost while meeting customer demand and making optimal use of available energy resources.
Model Overview

The microgrid optimization model considers a single battery storage system that interacts with both renewable and non-renewable energy sources. The objective is to minimize the cost of energy purchased from non-renewable sources while maximizing revenue from selling energy back to the grid after satisfying customer demand.
Key Components:

    Source Nodes: Includes renewable energy sources (solar and wind generation) and a non-renewable energy source (purchased energy).
    Battery Storage: Stores energy with certain efficiency losses during charging and discharging processes.
    Sink Node: Represents the customer demand that must be met at each time period.

Model Assumptions

    Battery Retention: The battery retains its charge from one time period to the next without idle depletion.
    Energy Sources: There are two main energy sources:
        Renewable Source: Free energy from solar and wind generation.
        Non-Renewable Source: Energy purchased at the market price.
    Charging Efficiency: Charging the battery is 95% efficient, modeled as:
    actual_charge_rate=0.95×charge_rate
    actual_charge_rate=0.95×charge_rate
    Discharging Efficiency: Discharging the battery is 95% efficient, modeled as:
    actual_discharge_rate=discharge_rate0.95
    actual_discharge_rate=0.95discharge_rate​
    Renewable Energy: Renewable energy is free; there is no cost associated with using it.
    Demand Satisfaction: The grid can supply just enough energy to satisfy customer demand, no excess energy will be consumed to generate additional revenue.
    Max Charging/Discharging Rate: The maximum charging and discharging rate of the battery (250 kWh) is considered before efficiency losses.

Model Parameters

The key parameters of the model include:

    T: Set of time indices from 0 to 11.
    Max_Cap: Maximum capacity of the battery =1000kWh=1000kWh.
    Charge_Eff: Charging efficiency of the battery =0.95=0.95.
    Discharge_Eff: Discharging efficiency of the battery =0.95=0.95.
    Min_C_State: Minimum charge state of the battery =100kWh=100kWh.
    Max_C_State: Maximum charge state of the battery =950kWh=950kWh.
    Max_Charge_Discharge_Rate: Maximum charging/discharging rate of the battery =250kWh=250kWh.
    Price: The price of energy bought or sold at each time index.
    Demand: The customer demand at each time index.
    Solar_Gen: Solar energy generation at each time index.
    Wind_Gen: Wind energy generation at each time index.
    Initial_Charge: Initial charge of the battery =500kWh=500kWh.

Decision Variables

The main decision variables used in the model are:

    B_State: Battery state, representing the energy (kWh) stored in the battery at time index tt.
    Charge_R: Charging rate of the battery at time index tt.
    Discharge_R: Discharging rate of the battery at time index tt.
    Grid_I: Grid import, representing the energy (kWh) imported from all sources at time index tt.
    Grid_E: Grid export, representing the energy (kWh) supplied to customers at time index tt.
    Relative_Flow: Represents the flow of energy that is not stored in the battery and is directly supplied to the grid at time index tt.
    Charge_Binary: Binary variable used to disable simultaneous charging and discharging of the battery.
    Energy_Bought: Energy (kWh) purchased from non-renewable sources at time index tt.

Objective Function

The objective is to minimize the total cost of purchasing energy while maximizing revenue from selling energy after satisfying customer demands:
Minimize∑t∈TPricet×(Energy_Boughtt−Grid_Et)
Minimizet∈T∑​Pricet​×(Energy_Boughtt​−Grid_Et​)
Model Constraints

    Battery State Constraint: The battery's current charge state is equal to the previous charge state plus effective charging rate minus effective discharging rate.
    B_State(t)=B_State(t−1)+Charge_Eff×Charge_R(t)−Discharge_R(t)Discharge_Eff
    B_State(t)=B_State(t−1)+Charge_Eff×Charge_R(t)−Discharge_EffDischarge_R(t)​

    Minimum Battery Charge Constraint: The battery state must not fall below 100 kWh.
    B_State(t)≥Min_C_State
    B_State(t)≥Min_C_State

    Maximum Battery Charge Constraint: The battery state must not exceed 950 kWh.
    B_State(t)≤Max_C_State
    B_State(t)≤Max_C_State

    Grid Import Constraint: The energy sent to the battery plus the energy directly sent to the grid must equal the grid import energy.
    Grid_I(t)=Charge_R(t)+Relative_Flow(t)
    Grid_I(t)=Charge_R(t)+Relative_Flow(t)

    Grid Export Constraint: The grid export energy must equal the battery discharge energy plus the energy flow coming from the grid.
    Grid_E(t)=Discharge_R(t)+Relative_Flow(t)
    Grid_E(t)=Discharge_R(t)+Relative_Flow(t)

    Source Energy Constraint: The grid import energy should be less than or equal to the sum of renewable and non-renewable energy sources.
    Grid_I(t)≤Solar_Gen(t)+Wind_Gen(t)+Energy_Bought(t)
    Grid_I(t)≤Solar_Gen(t)+Wind_Gen(t)+Energy_Bought(t)

    Customer Demand Constraint: The grid export energy must satisfy the customer demands.
    Grid_E(t)=Demand(t)
    Grid_E(t)=Demand(t)

    Charge/Discharge Binary Constraints: These constraints prevent simultaneous charging and discharging of the battery.
    Charge_R(t)Max_Charge_Discharge_Rate≤Charge_Binary(t)
    Max_Charge_Discharge_RateCharge_R(t)​≤Charge_Binary(t)
    Discharge_R(t)Max_Charge_Discharge_Rate≤1−Charge_Binary(t)
    Max_Charge_Discharge_RateDischarge_R(t)​≤1−Charge_Binary(t)


    How to Run

    1. Clone the repository to your local machine:
    git clone https://github.com/buckyball99/Microgrid-Energy-Optimization.git

    2. Install the necessary Python packages

    3. Run the optimization model in a Jupyter notebook or Python script