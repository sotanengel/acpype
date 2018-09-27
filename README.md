# ACPYPE

**AnteChamber PYthon Parser interfacE**

A tool based in Python to use Antechamber to generate topologies for chemical
compounds and to interface with others python applications like CCPN or ARIA.

acpype is pronounced as "ace + pipe"

Topologies files to be generated so far: CNS/XPLOR, GROMACS, CHARMM and AMBER.

NB: Topologies generated by acpype/Antechamber are based on General Amber Force
Field (GAFF) and should be used only with compatible forcefields like AMBER and
its variant.

Several flavours of AMBER FF are ported already for GROMACS (see ffamber:
http://ffamber.cnsm.csulb.edu/) as well as to XPLOR/CNS (see xplor-nih:
http://ambermd.org/xplor-nih.html) and CHARMM.

This code is released under GNU General Public License V3.

###### NO WARRANTY AT ALL!!!

It was inspired by:

- amb2gmx.pl (Eric Sorin, David Mobley and John Chodera)
  and depends on Antechamber and Openbabel

- YASARA Autosmiles:
  http://www.yasara.org/autosmiles.htm (Elmar Krieger)

- topolbuild (Bruce Ray)

- xplo2d (G.J. Kleywegt)

For Non-uniform 1-4 scale factor conversion (e.g. if using GLYCAM06), please cite:

> Austen Bernardi, Roland Faller, Dirk Reith, Karl N. Kirschner, "Conversion of
GLYCAM06 Force-Field Parameters from AMBER to GROMACS Topologies"
submitted for review, April 2018."

For Antechamber, please cite:
> 1.  Wang, J., Wang, W., Kollman P. A.; Case, D. A. "Automatic atom type and
    bond type perception in molecular mechanical calculations". Journal of
    Molecular Graphics and Modelling , 25, 2006, 247260.
> 2.  Wang, J., Wolf, R. M.; Caldwell, J. W.; Kollman, P. A.; Case, D. A.
    "Development and testing of a general AMBER force field". Journal of
    Computational Chemistry, 25, 2004, 1157-1174.

If you use this code, I am glad if you cite:

> SOUSA DA SILVA, A. W. & VRANKEN, W. F.
> ACPYPE - AnteChamber PYthon Parser interfacE. Manuscript submitted.

> BATISTA, P. R.; WILTER, A.; DURHAM, E. H. A. B. & PASCUTTI, P. G. Molecular
Dynamics Simulations Applied to the Study of Subtypes of HIV-1 Protease.
Cell Biochemistry and Biophysics, 44, 395-404, 2006.

Alan Wilter Sousa da Silva, D.Sc.
Bioinformatician, UniProt, EMBL-EBI
Hinxton, Cambridge CB10 1SD, UK.
http://www.ebi.ac.uk/~awilter

alanwilter _at_ gmail _dot_ com

#### How To Use ACPYPE

##### Introduction

To run *acpype* with its all functionalities, you need *ANTECHAMBER* from package
[http://amber.scripps.edu/#AmberTools AmberTools] and
[http://openbabel.org/wiki/Main_Page Open Babel] if your input files are of PDB
format.

However, if one wants *acpype* just to emulate *amb2gmx.pl*, one needs nothing
at all but *[http://www.python.org Python]*.

At the moment, *acpype* is for download via git:
```bash
git clone https://github.com/alanwilter/acpype.git
```

Yet, it is possible to install using Anaconda package Python 3.6:

```bash
conda install -c acpype -c openbabel acpype
```
##### To Test

At folder *acpype/test*, type:
```bash
../acpype.py -i FFF.pdb
```

It'll create a folder called *FFF.acpype*, and inside it one may find topology
files for GROMACS and CNS/XPLOR.

To get help and more information, type:
```bash
../acpype.py -h
```

##### To Install

At folder acpype, type:
```bash
  ln -s $PWD/acpype.py /usr/local/bin/acpype
```

And re-login or start another shell session.

##### To Verify with GMX
```bash
cd FFF.acpype/
grompp -c FFF_GMX.gro -p FFF_GMX.top -f em.mdp -o em.tpr
mdrun -v -deffnm em
# And if you have VMD
vmd em.gro em.trr
```

##### For MD, do:
GROMACS < v.4.5
```bash
grompp -c em.gro -p FFF_GMX.top -f md.mdp -o md.tpr
mdrun -v -deffnm md
vmd md.gro md.trr
```

GROMACS > v.5.0
```bash
gmx grompp -c em.gro -p FFF_GMX.top -f md.mdp -o md.tpr
mdrun -v -deffnm md
vmd md.gro md.trr
```
###### With openmpi, for a dual core
```bash
GROMACS < v.4.5
grompp -c FFF_GMX.gro -p FFF_GMX.top -f em.mdp -o em.tpr
om-mpirun -n 2 mdrun_mpi -v -deffnm em
grompp -c em.gro -p FFF_GMX.top -f md.mdp -o md.tpr
om-mpirun -n 2 mdrun_mpi -v -deffnm md
vmd md.gro md.trr
```
GROMACS > v.5.0
```bash
grompp -c FFF_GMX.gro -p FFF_GMX.top -f em.mdp -o em.tpr
gmx mdrun -ntmpi 2 -v -deffnm em

```

#### To Emulate amb2gmx.pl

For any given *prmtop* and *inpcrd* files (outputs from AMBER LEaP), type:

```bash
acpype -p FFF_AC.prmtop -x FFF_AC.inpcrd
```

The output files `FFF_GMX.gro` and `FFF_GMX.top` will be generated at the same
folder of the input files.

#### To Verify with CNS/XPLOR
At folder *FFF.acpype*, type:

```bash
cns < FFF_CNS.inp
```
#### To Verify with NAMD

  * see [TutorialNAMD]
