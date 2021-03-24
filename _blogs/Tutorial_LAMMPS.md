---
title: "Portfolio item number 2"
excerpt: "Short description of portfolio item number 1<br/><img src='/images/500x300.png'>"
collection: blogs
---

# LAMMPS
## Introduction
### What is LAMMPS?
* It is an acronym Large-scale Atomic/Molecular Massively Parallel Simulator
* It is a classical molecular dynamics code with a focus on materials modeling; Open source, portable C++;

### What can LAMMPS do?

* Molecular Dynamics(MD) Simulations, Non-equilibrium MD, Energy minimization, Monte Carlo(MC) Simulations
* Particle Simulations at varying length and time scales: 
  * electrons $\Rightarrow$ atomistic $\Rightarrow$ coarse-grained $\Rightarrow$ continuum
* Spatial-decomposition of simulation domain for parallelism
* GPU and OpenMP enhanced
* Can be coupled to other scales: QM, kMC, FE, CFD, ...

### Where can we learn LAMMPS?
* [Manual](https://lammps.sandia.gov/doc/Manual.html)
	* Introductions, Commands, Packages, Acceleratings
	* Howto, Modifying, Errors 
* Alphabetized command list: one doc page per command
	* https://lammps.sandia.gov/doc/commands_list.html
* Web site: [LAMMPS](http://lammps.sandia.gov)
	* Pictures, Movies - examples of others work
	* Papers - find a paper similar to what you want to model
	* Workshops - slides from LAMMPS simulation talks 
* [Mail list](https://lammps.sandia.gov/mail.html): search it, post to it

## Practical
### Structure of typical input scripts
1. Units and atom style
2. Create simulation box and atoms
	* region, create box, create atoms, region commands
		* lattice command vs box units
	* read data command
		* data file is a text file
		* look at examples/polymer/Kremer-Grest/KG.data
		* see read data doc page for full syntax  
3. Define groups
4. Set attributes of atoms: mass, velocity
5. Pair style for atom interactions
6. Fixes for time integration and constraints
7. Computes for diagnostics
8. Output: thermo, dump, restart
9. Run or minimize
10. Rinse and repeat (script executed one command at a time)

### Defining variables in input scripts
* Styles: index, loop, equal, atom, ...
	* variable x index run1 run2 run3 run4
	* variable x loop 100
	* variable x equal trap(f_JJ[3])*${scale}
	* variable x atom -(c_p[1]+c_p[2]+c_p[3])/(3*vol) 
* Formulas can be complex
	* see [variable](https://lammps.sandia.gov/doc/variable.html)
	* thermo keywords (temp, press, ...)
	* math operators & functions (sqrt, log, cos, ...)
	* group and region functions (count, xcm, fcm, ...)
	* various special functions (min, ave, trap, stride, stagger, ...)
	* per-atom vectors (x, vx, fx, ...)
	* output from computes, fixes, other variables 
* Formulas can be time- and/or spatially-dependent

### Using variables in input scripts
* Substitute in any command via $x or ${myVar}
* Can define them as command-line arguments 
	* ```% lmp linux -v myTemp 350.0 < in.polymer ```
* Use in next command to increment a variable
	* with jump command to create loops
* Many commands allow them as arguments
	* ```fix addforce 0.0 v_fy 1.0```
	* ```dump_ modify every v_count```
	* ```region sphere 0.0 0.0 0.0 v_radius```
* Allows time- and spatially-dependent commands

### Power tools for input scripts
* Filename options:
	* ```read data data.protein.gz```
	* ```read restart old.restart.*```
* If/then/else via ```if``` command
* Insert another script via ```include``` command
	* useful for long list of parameters
* Looping via next and jump commands
	* easy to run incrementally and stop when condition met
	* see examples on [jump](https://lammps.sandia.gov/doc/jump.html) command doc page 
* Invoke a shell command or external program
	* ```shell cd subdir1```
	* ```shell my analyze out.file $n ${param}```

### Debugging an input script
* Firstly find Error Info in screen file(always output screen file for your simulations)
	* ```% lmp linux -echo screen < in.polymer```
* For input, setup, run-time errors ...
	* search [Error](https://lammps.sandia.gov/doc/Errors_messages.html) for text of error message  
	* also for warnings, they are usually important
	* if specific input command causes problems, look for IMPORTANT NOTE info on doc page
	* look in the source code file at the line number
* Search the mail list, others may have similar problem
* Remember: an input script is like a program( test from easy stuff to the complicated)
	* start with small systems
	* start with one processor
	* turn-on complexity one command at a time
	* monitor thermo output, viz the results (use dump image)
* Also check thermodynamic output:
	* Look for blow-ups or NaNs, print every step if necessary 
* Debug by visualization - what does your system look like?
	* Normally visualize with VMD with LAMMPS trajectory file

### Build Initial Configurations
* In general it can be a hard problem!
* Molecular topology is an input to LAMMPS
* Packages for building initial configurations
	* [PACKMAL](http://m3g.iqm.unicamp.br/packmol/home.shtml) 
	* [moltemplate](https://moltemplate.org/)
	* http://lammps.sandia.gov/prepost.html
* If necessary, be willing to write system-building scripts by yourself
* Before running simulation of initial configurations, attention should be paid:
  * try to use smaller timeunit than conventional one of this model
  * try to run simulations under high temperature first
  * try to run simulations under NVT ensemble first and then run under NPT ensemble with different pressure 

### Setting Force Field
#### Pair Styles
* A pair style can be true pair-wise or many-body
	* LJ, Coulombic, Buckingham, Morse, Yukawa, ...
* EAM, Tersoff, REBO, ReaxFF, ...
* Bond/angle/dihedral/improper styles = permanent bonds
* Variants optimized for GPU and many-core
	* GPU, USER-CUDA, USER-OMP packages
	* lj/cut, lj/cut/gpu, lj/cut/cuda, lj/cut/omp
	* see [Accelerate](https://lammps.sandia.gov/doc/Speed.html)
* Coulomb interactions included in pair style
	* lj/cut, lj/cut/coul/cut, lj/cut/coul/wolf, lj/cut/coul/long 

#### Categories of Force Field
* All-atom: OPLS, CHARMM, AMBER, etc
* Charged systems:
	* pair lj/cut/coul/cut, lj/cut/coul/long + kspace style
* UA: pair lj, pair coul, bond/angle/dihedral harmonic, etc
* Coarse-grained
	* FENE, DPD, SDK, granular, SPH, peri, colloid, lubricate, brownian, FLD
* Aspherical
	* gayberne, resquared, line, tri
* Tabulated (e.g. force matching)
	* pair table, bond table, angle table, etc
* Reactive: ReaxFF, COMB, AIREBO, other bond-order models
* Hybrid systems: pair hybrid and hybrid/overlay
	* polymers on metal surface
	* polymers with nano-particles
	* solid-solid interface between 2 materials

### Fixes
* Most flexible feature in LAMMPS, allows control of what happens when within each timestep and you choose what group of atoms to apply fix to
* [Doc_fixes](https://lammps.sandia.gov/doc/fixes.html)

### Computes
* Calculate some property of system, on the fly
* computes store their results
* other commands invoke them and use the results, e.g. thermo output, dumps, fixes
* Output of computes:
	* global vs per-atom vs local
	* scalar vs vector vs array
	* extensive vs intensive values
* Examples:
	* temp & pressure = global scalar or vector
	* pe/atom = potential energy per atom (vector)
	* displace/atom = displacement per atom (array)
	* pair/local & bond/local = per-neighbor or per-bond info
* Many computes are useful with averaging fixes:
	* fix ave/time, fix ave/spatial, fix ave/atom
	* fix ave/histo, fix ave/correlate 
* [Doc_computes](https://lammps.sandia.gov/doc/computes.html)

### Thermo Output
* One line of output every N timesteps to screen and log file
* Any scalar can be output:
	* dozens of keywords: temp, pyy, eangle, lz, cpu
	* any output of a compute or fix: c_ID, f_ID[N], c_ID[N][M]
	* fix ave/time stores time-averaged quantities
	* equal-style variable: v_MyVar
	* one value from atom-style variable: v_xx[N]
	* any property for one atom: q, fx, quat, etc
	* useful for debugging or post-analysis
* [Doc_thermo](https://lammps.sandia.gov/doc/thermo_style.html)

### Dump output
* Snapshot of per-atom values every N timesteps
* Styles
	* atom, custom (both native LAMMPS)
		* VMD will auto-read if file named *.lammpstraj
	* xyz for coords only
	* cfg for AtomEye
	* DCD, XTC for CHARMM, NAMD, GROMACS
		* useful for back-and-forth runs and analys
* Two additional styles
	* local: per-neighbor, per-bond, etc info
	* image: instant JPG/PPM picture, rendered in parallel
* Any per-atom quantity can be output
	* dozens of keywords: id, type, x, xs, xu, mux, omegax, ...
	* any output of a compute or fix: f_ID, c _D[M]
	* atom-style variable: v_foo
* Additional options:
	* control which atoms by group or region
	* control which atoms by threshold
		* dump modify thresh c_pe > 3.0
	* text or binary or gzipped
	* one big file or per snapshot or per proc
	* see dump_modify fileper or nfile
* [Doc_dump](https://lammps.sandia.gov/doc/dump.html)