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
```

The answer is 

## Use PACKMOL

http://www.ime.unicamp.br/~martinez/packmol/

