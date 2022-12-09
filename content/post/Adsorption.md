---
date: "2022-11-21"
tags:
- Model
- Data
title: How to use Aspen Adsim for the IAST calculation ?
mathjax: true
---

### What is IAST ?

-   IAST stands for **Ideal adsorbed solution theory.**

-   For complex mixtures with multiple components, the Ideal Adsorbed Solution (IAS) Theory has recently gained traction. This method can predict the adsorption equilibria of components in a gas mixture. All that is required are adsorption equilibrium constants for pure components at the same temperature and on the same adsorbent.

-   The model considers the mixed adsorbate phase to be an ideal solution when combined with the gas phase. To derive the fundamental equations of thermodynamic equilibrium, the Gibbs method, which is used for vapor-liquid equilibria but also applies to gas-adsorbed phase equilibria, is used.

-   At first glance, perfect behavior during the adsorbed phase appears unlikely. Binary and ternary mixtures on activated carbons, zeolites, and silica gel are a few examples of systems where IAS theory predictions and experimental data agree well.

-   Some quality articles on IAST.

    -   Place Holder 1

    -   Place Holder 2

    -   Place Holder 3

    -   Place Holder 4

### Thermodynamic Framework

-   The following is a summary of that article's contents.

-   The solid is assumed to be inert to possess a specific surface area identical for all adsorbates.

-   Differential of the Helmholtz energy:

$$dA^a = -S^adT + \sigma d\mathcal{A}+\sum_i\mu_i^adn_i^a+\mu_s^adn_s^a 
\qquad (1)$$

-   i solute

-   s solvent

-   $\sigma$ interfacial tension

-   $\mathcal{A}$ area of the solution-solid interface

-   By Euler's theorem, Equation (1) can be intergrated to give:

    $$
    A^a = \sigma\mathcal{A}+\sum_i\mu_i^an_i^a + \mu_s^an_s^a \qquad(2)
    $$

-   Differentiation of equation (2)

    $$
    dA^a = \mathcal{A}d\sigma+\sigma d\mathcal{A}+\sum_i\mu_i^adn_i^a + \sum_in_i^a d\mu_i^a + \mu_s^adn_s^a + n_s^ad\mu_s^a \qquad(3)
    $$

-   Subtraction (3) from (1)

    $$
    0 = \mathcal{A}d\sigma + \sum_in_i^a d\mu_i^a + n_s^ad\mu_s^a + S^adT \qquad(4)
    $$

-   Assume the isothermal process

    $$
    0 = \mathcal{A}d\sigma + \sum_in_i^a d\mu_i^a + n_s^ad\mu_s^a \qquad(5)
    $$

-   We anticipate the result that at equilibrium the chemical potentials of the adsorbed and liquid phases are identical. Hence the isothermal Gibbs-Duhem equation can be written as

$$\sum_ic_id\mu_i^a+c_sd\mu_s^a = 0 \qquad(6)$$

-   $c_i$ and $c_s$ are the bulk liquid concentrations of solute **i** and solvent **s** in moles per unit volume.

-   To be continue

### A Case study: Predict the isotherm of a methane-ethane mixture in a nanoporous material, IRMOF-1.

The data presented in this section is drawn from the article [pyIAST: Ideal adsorbed solution theory (IAST) Python package](https://www.sciencedirect.com/science/article/pii/S0010465515004403?via%3Dihub). Aspen Adsim is used instead of the tool mentioned in the article to perform the calculation.

**Why should we use Aspen Adsim?** Aspen Adsim already offered a very good model and solver for adsorption simulation. Therefore, we should not reinvent the wheel.

Using the grand-canonical Monte Carlo algorithm, the author simulated pure-component ethane and methane adsorption isotherms at 298 K.

![Pure-component methane and ethane adsorption isotherms -- the amount of gas adsorbed as a function of pressure-- in metal-organic framework IRMOF-1. Simulated data.](/img/AdsimIAST/SingleIsotherm.PNG)

*Figure 1. Pure-component methane and ethane adsorption isotherms -- the amount of gas adsorbed as a function of pressure-- in metal-organic framework IRMOF-1. Simulated data.*

To validate the Aspen Adsim calculation, we compare the IAST calculation with binary-component grand-canonical Monte Carlo simulations of methane/ethane adsorption at various ethane molar fractions $(y_{C_2H_6})$.

![](/img/AdsimIAST/MixtureIsotherm.PNG)*Figure 2. Methane and ethane adsorption in IRMOF-1 in the presence of a mixture of methane and ethane at 65.0 bar and 298 K. The x-axis shows the composition of ethane in the mixture. The data points are from binary grand-canonical Monte Carlo simulations;*

The author provides a csv file in [Github](https://github.com/CorySimon/pyIAST) with data for both single and mixture isotherms.

#### Use Aspen Adsim for isotherm curve fitting.

-   We can use the **Static isotherm model** to fit the experiment data to isotherm model.

    ![](/img/AdsimIAST/SingleIsothermAdsim.png)

-   We can set the isotherm model by double click on model icon. For isotherm estimation, the compound is set to **"set"** (Don't use physical properties). The isotherm Langmuir 1 is written as:

    $$
    Q(kmol/kg) =\frac{IP_1 P(bar) }{1+IP_2P(bar)}
    $$

    ![](/img/AdsimIAST/IsothermModel.png)

-   Tool \> Estimation. On the tab estimated variables, we draged the $IP_1, IP_2$ from B1 block to the Estimation window.

![](/img/AdsimIAST/Estimation.png)

-   Next, we added experiment data points. Click the Steady state experiments tab. Here we have several experiment is already added. If you want to add a new experiment point, click the New button. Another window will be showed, from here, you can add pressure (as fixed variable) and loading (as measured variable) for gas phase. Pressure (P) is obtained from Specify window of B1. Loading (W) is obtained from Result window of B1.

    ![](/img/AdsimIAST/AddExperiment.png)

-   Then we click run and see the statistical test result.

    ![](/img/AdsimIAST/FittingResult.png)

-   The $IP_1 = 6.27\times10^{-4}$ and $IP_2 = 2.149\times10^{-2}$ , and the model can explain 99.9% variation from the data. Let's plot the model and the data together.

![](/img/AdsimIAST/SingleIsothermFit.PNG)

#### Use Aspen Adsim for IAST calculation.

-   Open the new Aspen Adsim workplace.

    ![](/img/AdsimIAST/IAST%20Cal/BlankSimulation.PNG)

-   Add compound list to simulation, noting that this time, each compound's physical and chemical properties are derived from Aspen Properties.

    ![](/img/AdsimIAST/IAST%20Cal/AddCompound.png)

-   This is the result of adding an Aspen Properties compound. Don't forget to add the available compound to Default component list.

    ![](/img/AdsimIAST/IAST%20Cal/AddCompound2.png)

-   The next step is to build the single bed model. We are only interested in the calculation of the gas-liquid equilibrium (expressed as an isotherm) in Aspen Adsim; therefore, other aspects of the process, such as kinetics, will be overlooked. Usually we can not directly build a functional bed from zero since I personally find the solver in Aspen Adsim is super unstable. The trick here is to built everything step-by-step. So we first build the non-bed model. Click View \> Model Libraries, and dont forget to change **Connection** to **gas_Material_Connection.**

    ![](/img/AdsimIAST/IAST%20Cal/AddNonBed1.png)

-   Let's build a non-bed model.

    ![](/img/AdsimIAST/IAST%20Cal/AddNonBed2.png)

-   Connect all the unit with **gas_Material_Connection.** Initialize the lower and upper gas tanks. Set the gas feed pressure to be higher than the gas product pressure; I would recommend 3 bar for the feed and 1 bar for the product. The simulation is then prepared for execution.

    ![](/img/AdsimIAST/IAST%20Cal/AddNonBed3.png)

-   Now, we can replace the gas valve in the middle with the real gas bed. Now the next step is how can we config the bed.

    ![](/img/AdsimIAST/IAST%20Cal/AddNonBed4.png)

-   Regarding the configuration of the bed, you can leave everything at its default value. Since only IAST calculation is of interest, only the isotherm tab is modified. Isotherm is now I.A.S. Langmuir 1 (in the case you want to calculate IAST base on Langmuir Isotherm). For the gas phase, the dependence of the isotherm will be partial pressure. Now we can specify the bed for adding isotherm parameters, which we found on previous section by fitting single compound isotherm.

    ![](/img/AdsimIAST/IAST%20Cal/ConfigBedIAST.png)

    ![](/img/AdsimIAST/IAST%20Cal/SpecifyBedIAST.png)

-   Let's run it. Before we can run the simulation, we need to initialize our bed.

    ![](/img/AdsimIAST/IAST%20Cal/RunSim.png)

-   If the simulation run successfully, we can start to play with the valve to set the correct pressure for our system. You can view the pressure at both ends of the bed (gas tank) to monitor the pressure in the bed. Here, the pressure in the bed is around 3 bar at the end of column and 1 bar at the top of column.

    ![](/img/AdsimIAST/IAST%20Cal/PressureBed.png)

-   That's is NOT GOOD. When it comes to numerical method, Aspen uses a finite difference method to solve the mass and heat balance along the axial of the column. They discretize the axial into a set of points, each of which is referred to as a node. Because the pressure at each node varies, so does the loading at each node.

    ![](/img/AdsimIAST/IAST%20Cal/FixBedFDM.png)

-   So our objective is to reduce the pressure difference between each node. The first step we can do is, decrease the CV of the product line and increase the CV of feed line.![](/img/AdsimIAST/IAST%20Cal/AdjustPressure1.png)

-   That's much better. Another way that you can improve it by decrease the bed length. Let's decrease it from 1m to 0.1m.

    ![](/img/AdsimIAST/IAST%20Cal/AdjustPressure2.png)

To operate at pressure we want, increase the pressure from Feed stream.

![](/img/AdsimIAST/IAST%20Cal/AdjustPressure3.png)

-   Allow the simulation to run until the Bed inventory is stable. It means that the simulation has reached an equilibrium state.

    ![](/img/AdsimIAST/IAST%20Cal/Equilibrium.png)

-   The loading can be calculate by the equation below:

    $$
    Q(kmol/kg) = \frac{Bed Inventory(kmol)}{Hb\times\pi\left(\frac{Db}{4}\right)^2\times RHOs}
    $$

![](/img/AdsimIAST/IAST%20Cal/Calculate.png)

-   For the next point: We change the molar fraction of ethane \> wait the simulation reach equilibrium \> calculate loading. Then we plot the IAST isotherm and *binary grand-canonical Monte Carlo simulations together.*

![](/img/AdsimIAST/MixtureIsothermIAST.PNG)
