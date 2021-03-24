---
title: "First Simulation"
excerpt: ""
collection: blogs
date: 2019-06-25
---

# First Simulation of Kremer-Grest Polymer in LAMMPS
## Introduction to Simulation Protocols
The simulation is usually divided in *four* main steps. These steps could vary depending on what the simulation’s objective is. In a typical case, where the objective is to study the dynamics of polymer melt, the steps to follow are: *Generation*, *Equilibration*, *Production*, and *Analysis*.

#### Generation: 
This step takes a random configuration of the system and let it pseudo-equilibrate for certain amount of time at certain thermodynamic conditions. The main objective here is to obtain a “non-artificial” configuration that is going to function as a starting point for the equilibration.

#### Equilibration
This step is going to take the configurations created in generation to thermodynamic equilibrium by letting them run for certain amount of time. This simulation time depends on polymer chemistry, thermoenvironment etc.

#### Production

Once system has gone through equilibration, it is assumed that the configurations obtained as a result (in the form of restart files) are in thermodynamic equilibrium (for now). Then, the goal in *Production* step is to generate all the data needed to compute the desired properties. For this purpose, this step takes the final configurations obtained in equilibration and let them run for at least the time given in equilibration at the same thermodynamic state, output the positions of each frame of the particles at specific time-steps in a trajectory file. 

#### Analysis
This step takes the trajectory files generated at production and run different analysis on them. Usually, the properties obtained are the structure factor or radial distribution function(rdf), end-to-end distance, radius gyration, the mean square displacement, end-end vector autocorrelation function and so on. By using the data it is possible to corroborate that the time given during equilibration step was enough to take the system to thermodynamic equilibrium. If this is not this case, simulations should be restarted from *equilibration* step to run a longer time for a equilibrated system

## Typical Workflow
1. Creation of folder structures(example of mine)
	* lammps_inputs: store input files for different steps
	* generation
		* log
		* restart
		* screen
		* trajetcory
		* submission_files 
	* equilibration
		* log
		* restart
		* screen
		* trajetcory
		* submission_files 
	* production
		* log
		* restart
		* screen
		* trajetcory
		* submission_files 
	* analysis([SMMSAT](https://github.com/Chenghao-Wu/SMMSAT))

2. Building LAMMPS Input File 
	* As seen in the example of next section

3. Building Initial Configuration(Here packmol as example)
	* firstly, create (type, x, y, z) for single chain
	* then, excute packmol to randomly put the chain into a fixed simulation box
	* now, positions of total chains of polymer are obtained from the output of packmol
	* create topology (bonds, angles, dihedrals etc.) for the system

4. Scripting lammps input files according to the objects of expriements

5. Performing post-analysis on the trajectory files

6. Organizing and plotting results 

## Example: Simulation of Kremer-Grest Polymer Melts
* Polymer Model
	* [Kremer-Grest bead-spring model](https://aip.scitation.org/doi/10.1063/1.458541)
	* bonded potential: finitely extensible nonlinear elastic(FENE)
$$ E_{FENE}=-k(\frac{R_0^2}{2})\,ln(1-(\frac{r}{R}^2))+4\epsilon[(\frac{\sigma}{r})^{12}-(\frac{\sigma}{r})^6] +\epsilon\,\,\,\,\,\,\,\,\,\,\,\, k=30, R_0=1.5, \sigma=1.0,\epsilon=1.0 $$
	* pair potential: 12-6 Lennard-Jones(LJ)
$$ E_{LJ}= 4\epsilon[(\frac{\sigma}{r})^{12}-(\frac{\sigma}{r})^6 ] \,\,\,\,\,\,\,\,\,\,\,\,\sigma=1.0,\epsilon=1.0,r_{cut}=2.5\sigma$$
* LAMMPS input file:
	* ./examples/polymer/Kremer-Grest 
		* generation.inp
		* equilibration.inp
		* production.inp
* LAMMPS data file:
	* ./examples/polymer/Kremer-Grest 
		* KG.data
* cluster submission file:
	* it is created by your self according to the variables in input file
	* ```man sniffa``` check manual of slurm command for submitting jobs on sniffa

* Analysis
	* target propertis: 
		1. radius gyration
		2. end-to-end distance
		3. mean-square internal distance
		3. mean-square displacements
			* for chain inner beads, end beads and all chain beads
		4. end-end vector autocorrelation function 
