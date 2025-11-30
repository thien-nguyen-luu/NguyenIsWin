---
date: "2019-04-20"
hideAuthor: true
hideDate: true
title: 📝 Membrane Optimizer by Python
tags: ["Python","Optimization"]
---

### Context
We were asked to design a membrane system for producing biomethane from biogas. Because the system employs an Evonik membrane, we use the sizing tool provided by Evonik to determine the number of membranes and the capacity of the gas compressor. However, the Evonik tool does not include any tools to assist us in determining the most cost-effective configuration for the project (minimum capital cost + operating cost). To solve this, a tool must be created. Pythone is used to write the biomethane production membrane optimization tool. Its primary functions are as follows:
+ Auto click to control the operation of Evonik's sizing software automatically; + Record the software's parameters.
+ Record software's parameters. 
+ Use the optimizer based on the ***Genetic algorithm*** to find the best configuration (lowest cost) for a given project. 
+ Report file should be exported.

### Demo

{{< youtube id="l7As6lBf3Ok" >}}
