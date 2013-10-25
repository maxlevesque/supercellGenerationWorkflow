supercellGenerationWorkflow
===========================

My workflow to efficiently generate initial configurations for molecular dynamics or Monte Carlos simulations


We illustrate the workflow below, with the building of a supercell of pure ScCl3.

## Target density
The density of the liquid may be quickly estimated with Wolfram Alpha. In effect, the quantity of importance is not the density, but the molecular volume of ScCl3:

http://www.wolframalpha.com/input/?i=molar+volume+of+ScCl3+in+Angstrom^3+per+molecule

ScCl3 has 105.1 Angstroms^3 per molecule.

## How many molecules do you want in your supercell?
Nobody can answer but you. I'll say 10 only for my purpose today.

## Deduce dimensions of the supercell

10 molecules times 105.1 Angstroms^3 per molecule = 1051 Angstroms^3 = 1.051 nm^3. For the sake of simplicity and numerical efficiency of my simulations (Ewald summations are more efficient in cubic cells), I want a cubic supercell. Its length is thus 1051^(1/3) Angstroms = 10.167 Angstroms.

## What is the geometry of your molecule (ScCl3 here)

In Mathematica (9+?), type:
```
ChemicalData["ScCl3", "MoleculePlot", "LongDescription"]
ChemicalData["ScCl3", "MoleculePlot"]
ChemicalData["ScCl3", "VertexTypes", "LongDescription"]
ChemicalData["ScCl3", "VertexTypes"] // TableForm
ChemicalData["ScCl3", "AtomPositions", "LongDescription"]
ChemicalData["ScCl3", "AtomPositions"] // TableForm
```

The answer is a figure of the molecule, plus:
```
"list of atom types at graph vertices"
Sc
Cl
Cl
Cl

"list of 3D coordinates of atoms (in picometers)"
{
 {-2.3977, -9.5735, 9.9297},
 {167.16, -29.684, 144.28},
 {34.595, 138.13, -143.27},
 {-199.36, -98.874, -10.935}
}
```

We now have the type and coordinates of all atoms of the molecule.

## We now prepare PACKMOL: input file to define ScCl3

L. Martinez, R. Andrade, E. G. Birgin, J. M. Martinez, 
PACKMOL: A package for building initial configurations for molecular dynamics simulations. 
Journal of Computational Chemistry, 30:2157-2164,2009.


http://www.ime.unicamp.br/~martinez/packmol/

PACKMOL needs a xyz file (it may be a pdb) that contains exactly what we just extracted:
```
$ cat ScCl3.xyz
4
ScCl3
Sc    1.976023 1.904265 2.099297
Cl    3.6716 1.70316 3.4428   
Cl    2.34595 3.3813 0.5673
Cl    0.0064 1.01126 1.89065
```

## PACKMOL: the input file defining the supercell

```
$ cat ScCl3.inp
#
# Supercell of 10 ScCl3 in a cubic cell of length 10.167 Angstrom
#

# All atoms from diferent molecules will be at least 2.0 Angstroms apart
# from each other at the solution.

tolerance 2.0

# The files are in the simple Molden-xyz format

filetype xyz

# The name of the output file.

output ScCl3_cell.xyz

# number of molecules of ScCl3 (as define in an external .xyz file)

structure ScCl3.xyz 
  number 10 
  inside cube 0. 0. 0. 10.167
end structure
```

A file ScCl3_cell.xyz has been generated. It is a cubic cell of 10.167 Ang that contains 10 molecules of ScCl3.
No atom have closest nearest neighbor than 2 Ang.
