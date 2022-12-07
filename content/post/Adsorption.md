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
    Q(kmol/kg) = IP_1 \frac{IP_2 P(bar) }{1+IP_2P(bar)}
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

![](/img/AdsimIAST/MixtureIsothermIAST.PNG)
