# List of commands

## cd

*Change directory (MolTwister implementation)*

````text
Usage: cd <destination directory>

Changes directory to <destination directory>. It is possible to specify
directory names containing spaces using the '\' character. For example,
'cd /Users/John\ Doe/' would change to the directory '/Users/John Doe'
To change directory to MolTwister shortcut N, type 'cd [N]' and hit enter.
Shortcuts can be edited directly by accessing 'MolTwister.shortcuts',
located under the home directoy of your computer. The contents of this
file is structured as follows:

	#Default
	<default directory at startup>
	#n
	<shortcut 1>
	     .
	     .
	     .
	<shortcut n>

where n is the number of shortcuts available in the file.
````

## lsm

*Show current directory contents*

````text
Usage: lsm

Shows the contents of the current directory in the same way as the 'ls'
command in Linux, but will also show the directory shortcuts that are
available to the MolTwister 'cd' command. Go to the help pages for the
MolTwister 'cd' command for a further explanation on shortcuts. However,
the 'lsm' command does not accept any of theswitches available for the
Linux 'ls' command.
````

## ll

*Shortcut for the Unix 'ls -lha' command*

````text
Usage: ll

This is a shortcut for the 'ls -lha' command (only applicable on Unix based systems).
````

## load

*Load data into MolTwister*

````text
Usage: load <data type> <file path>

Load data into MolTwister. The allowed data types are:

xyz :        XYZ-coordinate files (including XYZ files containing
             several frames). Bonds can be ignored using 'ignore' (
             see genbonds). Use 'frame <frame>' to select frame to use
             as basis for generating the bonds. Default is frame zero.
             The order of the additional arguments is 'ignore', 'frame'.
dcd :        DCD-coordinate files. Bonds can be ignored using 'ignore' (
             see genbonds). Use 'frame <frame>' to select frame to use
             as basis for generating the bonds. Default is frame zero.
             Use 'stride <stride>' to only load every <stride> frame.
             The order of the additional arguments is 'ignore', 'frame', 'stride'.
pdb :        PDB-coordinate files. Bonds can be ignored using 'ignore'.
             To avoid query about delete ('del'), append ('app') or cancel ('can'),
             use 'noquery <response>' with the appropriate response.
mtt :        MolTwister trajectory file.
script :     MolTwister script containing a sequence of MolTwister commands,
             each formated as: exec("<MolTwister command>");. For example,
             exec("add atom C at 0 0 0"); would add a C atom at (0, 0, 0)
masscharge : Load charges and masses into the current molecular structure. In
             the file each line defines a charge/mass in two possible ways:
                   * ID <ID, e.g. H, C, C2, or similar> <partial charge> <mass>
                   * AtInd <Index of atom> <partial charge> <mass>
qepos :      XYZ-position files (*.pos) from QuantumEspresso. Note that in
             this case <file path> = <*.pos file> <input file>

             If no data type is selected MolTwister will try to recognize
             the file format from the file extension.
python :     Execute a python script. How to access MolTwister from a Python
             is documented under the 'mtpython' command. However, when loading
             a script, only the contents of 'mtpython {<contents>}' is to be
             included in the Python script.
````

## genbonds

*Generate bonds between atoms*

````text
Usage: genbonds [minr <minimum R>] [pbcdetect] [verbose] [atomicunwrap]

Generate bonds between atoms. A bond is defined as any bond length R that
satisfies 'minimum R' < r < r1+r2+0.4. In addition, the number of bonds that
are connected to C, N, P, S atoms are restricted to 4. The default minimum
bond length limit is 0.8AA. The parmeters inside square brackets are optional.
If the verbose keyword is used, then each detected bond will be displayed as
they are detected.

Using the pbcdetect keyword results in bonds across periodic images defined
by the current PBCs (i.e. Periodic Boundary Conditions). 'atomicunwrap'
results in molecules diveded by the PBCs being wrapped to one side of the
periodic boundary that divide the molecule.

To ignore bonds to atoms use 'ignore' followed by a comma separated list of
atoms (without any space).
````

## list

*List atomic properties*

````text
Usage: list <filter>

Lists atomic properties based on the specified filter. The allowed filters
are:

       * all                    :   List all
       * mol <N>                :   List atoms in molecule N
       * ff                     :   List only force-field parameters
       * latexff [longtable]    :   Make LaTeX force-field tables. 'longtable'
                                    creates tables across several pages
````

## autoscale

*Autoscale the 3D view window*

````text
Usage: autoscale

Autoscale the 3D view by searching for the boundary of all atoms inside the
3D view and subsequently adjusting the viewing frustum to these boundaries.
The camera is placed along the x-axis and will point towards the center of
the located atomic boundaries.
````

## del

*Delete atoms, bonds, etc.*

````text
Usage: del <specifier>

Delete atoms, bonds, angles and dihedrals. The allowed specifiers are:

        * atom <N>             :   Delete atom index <N>
        * atomname <name>      :   Delete atoms with name <name>
        * sel                  :   Delete selected atoms
        * bond <N1> <N2>       :   Delete bond <N1>-<N2>
        * bonds <1> <2>        :   Delete bonds <1>-<2>, where <1>, <2> are atom names.
                                   If <2>=* then all bonds to <1> are deleted
        * mdnonbonded <index>  :   The 'list ff' command yields list of indices
        * mdbond <index>       :   The 'list ff' command yields list of indices
        * mdangle <index>      :   The 'list ff' command yields list of indices
        * mddihedral <index>   :   The 'list ff' command yields list of indices
        * ff                   :   Delete force-field (m, Q, mobility, non-bonded, bonds, angles etc.)
````

## copy

*Copy atoms, molecules, etc.*

````text
Usage: copy <specifier>

Make several copies of atoms, or collection of atoms. The allowed specifiers are:

        * atoms <nx> <ny> <nz> <dx> <dy> <dz> <index1> ... <indexN>        : Copy <ni> times
          with distance <di> between each copy, where i is x,y or z.
        * names <nx> <ny> <nz> <dx> <dy> <dz> <name1> ... <nameN>          : Copy based on names.
        * all <nx> <ny> <nz> <dx> <dy> <dz>                                : Copy all.
        * sel <nx> <ny> <nz> <dx> <dy> <dz>                                : Copy only selected.
        * resnames <nx> <ny> <nz> <dx> <dy> <dz> <resname1> ... <resnameN> : Copy based on resnames.
````

## add

*Add atom to system*

````text
Overview of commands of the form 'add <sub command>':

add atom <ID> [atomlabel <name> <x-displ> <y-displ> <z-displ> <r>, <g>, <b>] at <x> <y> <z> [cubecpy <nx> <ny> <nz> <dx> <dy> <dz>, spherecpy <N> <R> [random]]
add atom <ID> [atomlabel <name> <x-displ> <y-displ> <z-displ> <r>, <g>, <b>] from atom <n> [bondlabel <name> <x-displ> <y-displ> <z-displ> <r>, <g>, <b>] dist <d> [cubecpy...]
add atom <ID> [atomlabel <name> <x-displ> <y-displ> <z-displ> <r>, <g>, <b>] from atom <n> [bondlabel <name> <x-displ> <y-displ> <z-displ> <r>, <g>, <b>] dir <dx> <dy> <dz> dist <d> [cubecpy...]
add atom <ID> [atomlabel <name> <x-displ> <y-displ> <z-displ> <r>, <g>, <b>] from bond <n1> <n2> [bondlabel <name> <x-displ> <y-displ> <z-displ> <r>, <g>, <b>] angle <angle> <dih> dist <d> [cubecpy...]
add atom <ID> [atomlabel <name> <x-displ> <y-displ> <z-displ> <r>, <g>, <b>] from angle <n1> <n2> <n3> [bondlabel <name> <x-displ> <y-displ> <z-displ> <r>, <g>, <b>] angle <angle> <dih> dist <d> [cubecpy...]
add atoms <ID> sel <dx> <dy> <dz>
add mdnonbonded <ID1> <ID2> <FF-type> <parameters for given FF-type>
add mdbond <ID1> <ID2> <FF-type> [all_r_less_than <radius>, mol_r_less_than <radius>, only_visible_bonds, only_14_bonds] <parameters for given FF-type>
add mdangle <ID1> <ID2> <ID3> <FF-type> [all_r_less_than <radius>, mol_r_less_than <radius>, only_visible_bonds] <parameters for given FF-type>
add mddihedral <ID1> <ID2> <ID3> <ID4> <FF-type> [all_r_less_than <radius>, mol_r_less_than <radius>, only_visible_bonds] <parameters for given FF-type>

To get more information about a <sub command>, type 'help add <sub command>

````

### atom

````text
Overview of commands of the form 'add atom':

add atom <ID> [atomlabel <name> <x-displ> <y-displ> <z-displ> <r>, <g>, <b>] at <x> <y> <z> [cubecpy <nx> <ny> <nz> <dx> <dy> <dz>, spherecpy <N> <R> [random]]
add atom <ID> [atomlabel <name> <x-displ> <y-displ> <z-displ> <r>, <g>, <b>] from atom <n> [bondlabel <name> <x-displ> <y-displ> <z-displ> <r>, <g>, <b>] dist <d> [cubecpy...]
add atom <ID> [atomlabel <name> <x-displ> <y-displ> <z-displ> <r>, <g>, <b>] from atom <n> [bondlabel <name> <x-displ> <y-displ> <z-displ> <r>, <g>, <b>] dir <dx> <dy> <dz> dist <d> [cubecpy...]
add atom <ID> [atomlabel <name> <x-displ> <y-displ> <z-displ> <r>, <g>, <b>] from bond <n1> <n2> [bondlabel <name> <x-displ> <y-displ> <z-displ> <r>, <g>, <b>] angle <angle> <dih> dist <d> [cubecpy...]
add atom <ID> [atomlabel <name> <x-displ> <y-displ> <z-displ> <r>, <g>, <b>] from angle <n1> <n2> <n3> [bondlabel <name> <x-displ> <y-displ> <z-displ> <r>, <g>, <b>] angle <angle> <dih> dist <d> [cubecpy...]

In general, ID could for example be C, H, or O (i.e. carbon, hydrogen, or
oxygen). It is also possible to give names such as C1 or C2. As long as
the name contains a C it is recognized as a carbon atom. Similarly, any
name containing O will be recognized as oxygen, etc.

The 'atomlabel' keyword can be used to add a label with any <name>, without white
characters, that can be displayed a displacement (<x-disp>, <y-disp>, <z-displ>) away
from the atomic position, with the color {<r>, <g>, <b>}. Similarly, a label can be
added relative to each bond center by applying the 'bondlabel' keyword.

When atoms are added they attain a new index starting at index 0. The list
of atomic indices are obtained through the 'list' command.
Versions of the add command:

 1) Add atom at the coordinate (<x>, <y>, <z>). Grid copy makes <na> copies separated
    by a distance <da>, where a is x, y, and z. Sphere-copy places <N> atoms at surface
    of a sphere of radius R, approx. equidistant., unless 'random' is specified.

 2) Add atom a distance <d> in the y-direction from the atom with index <n>.

 3) Add atom a distance <d> in the (<dx>, <dy>, <dz>)-direction from the atom
    with index <n>.

 4) Add atom with index n3, such that the angle <n1>-<n2>-<n3> is <angle>
    degrees, with the <n2>-<n3> bond being rotated <dih> around the <n1>-<n2>
    bond (automatically using the x-, y-, or z- axis as reference). The bond
    length <n2>-<n3> is set to <d>.

 5) Add atom with index n4, such that the angle <n2>-<n3>-<n4> is <angle>
    degrees, with the <n1>-<n2>-<n3>-<n4> dihedral set to <dih> degrees.
    The bond length <n3>-<n4> is set to <d>.

````

### atoms

````text
Overview of commands of the form 'add atoms':

add atoms <ID> sel <dx> <dy> <dz>

In general, ID could for example be C, H, or O (i.e. carbon, hydrogen, or
oxygen). It is also possible to give names such as C1 or C2. As long as
the name contains a C it is recognized as a carbon atom. Similarly, any
name containing O will be recognized as oxygen, etc.

When atoms are added they attain a new index starting at index 0. The list
of atomic indices are obtained through the 'list' command.

The above command will add one atom of type <ID> a distance <dx> <dy> <dz> away
from each selected atom.

````

### mdnonbonded

````text
Overview of commands of the form 'add mdnonbonded':

add mdnonbonded <ID1> <ID2> <FF-type> <parameters for given FF-type>

ID1 and ID2 identifiy the atom types where a non-bonded interaction is to
be defined (e.g., H, O, C, O5). The force field type, FF-type, is idefined by
a string. The possible strings are listed below, together with how 'parameters
for given FF-type' is defined for each string.

Possible <FF-type> <parameters for given FF-type> to apply:
* LJ <Lennard-Jones epsilon> <Lennard-Jones sigma> [alpha <Lennard-Jones alpha>]
* LJ1208 <Lennard-Jones epsilon> <Lennard-Jones sigma> [alpha <Lennard-Jones alpha>]
* Buck <Buckingham A> <Buckingham rho> <Buckingham C>
* LJBuck 0 <Lennard-Jones epsilon> <Lennard-Jones sigma>
* LJBuck 1 <Buckingham A> <Buckingham rho> <Buckingham C>


````

### mdbond

````text
Overview of commands of the form 'add mdbond':

add mdbond <ID1> <ID2> <FF-type> [all_r_less_than <radius>, mol_r_less_than <radius>, only_visible_bonds, only_14_bonds] <parameters for given FF-type>

ID1 and ID2 identifiy the atom types where a bond interaction is to be
defined (e.g., H, O, C, O5). The force field type, FF-type, is idefined by
a string. The possible strings are listed below, together with how 'parameters
for given FF-type' is defined for each string.

Possible bond definitions to apply:
* all_r_less_than <r> - yields bonds even between atoms not close enough to define a molecule (if the r-criteria is satisfied)
* mol_r_less_than <r> - yields bonds only between atoms close enough to define a molecule (if the r-criteria is satisfied)
* only_visible_bonds - yields bonds only where bonds are visible in the 3D view
* only_14_bonds - yields bonds only between 1-4 interactions

Possible <FF-type> <parameters for given FF-type> to apply:
* Harm <Harmonic k> <Harmonic r_0>
* Morse <Morse D> <Morse alpha> <Morse r_0>
* LJC <Lennard-Jones epsilon> <Lennard-Jones sigma> <scale>


````

### mdangle

````text
Overview of commands of the form 'add mdangle':

add mdangle <ID1> <ID2> <ID3> <FF-type> [all_r_less_than <radius>, mol_r_less_than <radius>, only_visible_bonds] <parameters for given FF-type>

ID1, ID2 and ID3 identifiy the atom types where an angular interaction is to be
defined (e.g., H, O, C, O5). The force field type, FF-type, is idefined by
a string. The possible strings are listed below, together with how 'parameters
for given FF-type' is defined for eaah string.

Possible bond definitions to apply:
* all_r_less_than <r> - yields bonds even between atoms not close enough to define a molecule (if the r-criteria is satisfied)
* mol_r_less_than <r> - yields bonds only between atoms close enough to define a molecule (if the r-criteria is satisfied)
* only_visible_bonds - yields bonds only where bonds are visible in the 3D view

Possible <FF-type> <parameters for given FF-type> to apply:
* Harm <Harmonic k> <Harmonic theta0(deg)>
* Class2 <Ea.theta0> <Ea.K2> <Ea.K3> <Ea.K4> <Ebb.M> <Ebb.r1> <Ebb.r2> <Eba.N1> <Eba.N2> <Eba.r1> <Eba.r2>


````

### mddihedral

````text
Overview of commands of the form 'add mddihedral':

add mddihedral <ID1> <ID2> <ID3> <ID4> <FF-type> [all_r_less_than <radius>, mol_r_less_than <radius>, only_visible_bonds] <parameters for given FF-type>

ID1, ID2, ID3 and ID4 identifiy the atom types where a dihedral interaction is to be
defined (e.g., H, O, C, O5). The force field type, FF-type, is idefined by
a string. The possible strings are listed below, together with how 'parameters
for given FF-type' is defined for eaah string.

Possible bond definitions to apply:
* all_r_less_than <r> - yields bonds even between atoms not close enough to define a molecule (if the r-criteria is satisfied)
* mol_r_less_than <r> - yields bonds only between atoms close enough to define a molecule (if the r-criteria is satisfied)
* only_visible_bonds - yields bonds only where bonds are visible in the 3D view

Possible <FF-type> <parameters for given FF-type> to apply:
* Fourier4t <V1> <V2> <V3> <V4>
* Harm <Harmonic K> <Harmonic D> <Harmonic N>


````

## var

*Define a variable*

````text
Usage: var <type+value> <variable name>

Define a variable that, for example, can be used (in some situations)
instead of atomic indices. <type+value> can be one of the following:

  * atom <N>                     : Define atomic variable with index <N>
  * bond <N1> <N2>               : Define bond variable between index <N1>
                                   and index <N2>
  * angle <N1> <N2> <N3>         : Define angle variable between index <N1>,
                                   <N2> and <N3>
  * dihedral <N1> <N2> <N3> <N4> : Define dihedral variable between index
                                   <N1>, <N2>, <N3> and <N4>

Defining variables can be useful for commands such as 'mod', 'measure',
and 'gauss9', to avoid having to remember atomic indices.
````

## varlist

*List all variables defined by 'var'*

````text
Usage: varlist

List all variables that have been defined using the 'var command.
The variable names, their type and their content will be listed.
````

## mod

*Modify settings, parameters and states*

````text
Overview of commands of the form 'mod <sub command>':

mod angle id <atom index 1> <atom index 2> <atom index 3> to <angle>
mod angle var <variable name> to <angle>
mod atomname <atom ID to change> <atom ID to change to> [within <domain>]
mod atompos id <atom index> to <x> <y> <z>
mod atompos id <atom index> by <x> <y> <z>
mod atompos id <atom index> geomcent <x> <y> <z>
mod atompos id <atom index> flip <flip axis>
mod atompos var <variable name> to <x> <y> <z>
mod atompos var <variable name> by <x> <y> <z>
mod atompos var <variable name> geomcent <x> <y> <z>
mod atompos var <variable name> flip <flip axis>
mod atompos all to <x> <y> <z>
mod atompos all by <x> <y> <z>
mod atompos all geomcent <x> <y> <z>
mod atompos all flip <flip axis>
mod atompos sel to <x> <y> <z>
mod atompos sel by <x> <y> <z>
mod atompos sel geomcent <x> <y> <z>
mod atompos sel flip <flip axis>
mod bondlength id <atom index 1> <atom index 2> to <dist>
mod bondlength var <variable name> to <dist>
mod charge id <atom index> to <partial charge>
mod charge var <variable name> to <partial charge>
mod charge name <atom ID> to <partial charge>
mod dihedral id <atom index 1> <atom index 2> <atom index 3> <atom index 4> to <angle>
mod dihedral var <variable name> to <angle>
mod mass id <atom index> to <atomic mass>
mod mass var <variable name> to <atomic mass>
mod mass name <atom ID> to <atomic mass>
mod mobility id <atom index> to <mobility>
mod mobility var <variable name> to <mobility>
mod mobility name <atom ID> to <mobility>
mod mobility sel to <mobility>
mod resname id <atom index> to <resname>
mod resname var <variable name> to <resname>
mod resname name <atom ID> to <resname>
mod resname resname <resname> to <resname>
mod resname molecule <molecule index> to <resname>
mod resname atomnames <atom ID1> <atom ID2> ... <atom IDn> to <resname>
mod rotatesel <angle> <pos. of rotation vector> <rotation vector>
mod sigma id <atom index> to <sigma>
mod sigma var <variable name> to <sigma>
mod userdefpbc <x low> <x high> <y low> <y high> <z low> <z high>

To get more information about a <sub command>, type 'help mod <sub command>

````

### angle

````text
Overview of commands of the form 'mod angle':

mod angle id <atom index 1> <atom index 2> <atom index 3> to <angle>
mod angle var <variable name> to <angle>

Modifies the angle, given by
* the three atom indices <atom index 1> to <atom index 3>
* the atom indices contained in the variable <variable name>
to the angle given by <angle>, which is given in degrees.

````

### atomname

````text
Overview of commands of the form 'mod atomname':

mod atomname <atom ID to change> <atom ID to change to> [within <domain>]

Renames atom ID (e.g., H, O, C7) from <atom ID to change> to <atom ID to change to>, either
for all atoms of the loaded system, if 'within' is not specified, or for the atoms within the
given <domain>.

The <domain> parameter consists of a string without space, where several
conditions are specified, separated by boolean operators. The boolean
operators that can be applied are:
* not: !
* and: &
* and: *
* or: |
* or: +
* xor: ^
Each condition is on x, y or z and can be of the following types
* equal: =
* not equal: !=
* greater than: >
* greater than or equal: >=
* less than: <
* less than or equal: <=
For example,
* x>5
* x>5&x<10
* (x>5&x<10)|(x>20&x<23)
* !(x>5&x<10)

````

### atompos

````text
Overview of commands of the form 'mod atompos':

mod atompos id <atom index> to <x> <y> <z>
mod atompos id <atom index> by <x> <y> <z>
mod atompos id <atom index> geomcent <x> <y> <z>
mod atompos id <atom index> flip <flip axis>
mod atompos var <variable name> to <x> <y> <z>
mod atompos var <variable name> by <x> <y> <z>
mod atompos var <variable name> geomcent <x> <y> <z>
mod atompos var <variable name> flip <flip axis>
mod atompos all to <x> <y> <z>
mod atompos all by <x> <y> <z>
mod atompos all geomcent <x> <y> <z>
mod atompos all flip <flip axis>
mod atompos sel to <x> <y> <z>
mod atompos sel by <x> <y> <z>
mod atompos sel geomcent <x> <y> <z>
mod atompos sel flip <flip axis>

Modifies the atomic positions of one or more atoms. The atoms to be moved can be specified by
* id <atom index>
* var <variable name>, where variable name contains atom index
* all, which signifies all loaded or created atoms
* sel, which signifies all selected atoms
The relative motion of the translations can be one of the following
* to <x> <y> <z>, which will move all chosen atoms to (<x>, <y>, <z>)
* by <x> <y> <z>, which will translate all chosen atoms by <x>, <y> and <z> along the x-, y-
                  and z-axis, respectively
* geomcent <x> <y> <z>, which will translate all chosen atoms such that geometric center of
                        their bounding box is positioned at (<x>, <y>, <z>)
* flip <flip axis>, where <flip axis> is either x, y, or z, which will flip all the chosen
                    atoms are mirrored across the selected axis around the bounding box center
                    of the chosen atoms

````

### bondlength

````text
Overview of commands of the form 'mod bondlength':

mod bondlength id <atom index 1> <atom index 2> to <dist>
mod bondlength var <variable name> to <dist>

Modifies the distance between two atoms, typically a bond, given by
* the two atom indices <atom index 1> and <atom index 3>
* the atom indices contained in the variable <variable name>
to the distance given by <dist>.

````

### charge

````text
Overview of commands of the form 'mod charge':

mod charge id <atom index> to <partial charge>
mod charge var <variable name> to <partial charge>
mod charge name <atom ID> to <partial charge>

Modifies the charge of atoms given by
* the atom index, <atom index>
* the atom index contained in the variable <variable name>
* all atoms with atom ID = <atom ID> (e.g., O, H, C7)
to the charge given by <partial charge>.

````

### dihedral

````text
Overview of commands of the form 'mod dihedral':

mod dihedral id <atom index 1> <atom index 2> <atom index 3> <atom index 4> to <angle>
mod dihedral var <variable name> to <angle>

Modifies the dihedral angle, given by
* the three atom indices <atom index 1> to <atom index 4>
* the atom indices contained in the variable <variable name>
to the dihedral angle given by <angle>, which is given in degrees.

````

### mass

````text
Overview of commands of the form 'mod mass':

mod mass id <atom index> to <atomic mass>
mod mass var <variable name> to <atomic mass>
mod mass name <atom ID> to <atomic mass>

Modifies the atomic mass of atoms given by
* the atom index, <atom index>
* the atom index contained in the variable <variable name>
* all atoms with atom ID = <atom ID> (e.g., O, H, C7)
to the mass given by <atomic mass>.

````

### mobility

````text
Overview of commands of the form 'mod mobility':

mod mobility id <atom index> to <mobility>
mod mobility var <variable name> to <mobility>
mod mobility name <atom ID> to <mobility>
mod mobility sel to <mobility>

Modifies the atomic mobility of atoms given by
* the atom index, <atom index>
* the atom index contained in the variable <variable name>
* all atoms with atom ID = <atom ID> (e.g., O, H, C7)
* all atoms contained in the current visible selection ('sel')
to the mobility given by <mobility>, which can be
either 'mobile' (e.g., included in MD integrations)
or 'fixed' (e.g., not included in MD integrations).

````

### resname

````text
Overview of commands of the form 'mod resname':

mod resname id <atom index> to <resname>
mod resname var <variable name> to <resname>
mod resname name <atom ID> to <resname>
mod resname resname <resname> to <resname>
mod resname molecule <molecule index> to <resname>
mod resname atomnames <atom ID1> <atom ID2> ... <atom IDn> to <resname>

Modifies the resname of atoms given by
* the atom index, <atom index>
* the atom index contained in the variable <variable name>
* all atoms with atom ID = <atom ID> (e.g., O, H, C7)
* all atoms with resname = <resname>
* all atoms within molecular index = <molecule index>
* all atoms with atom IDs, <atom ID1> through <atom IDn>
to the resname given by <resname>.

````

### rotatesel

````text
Overview of commands of the form 'mod rotatesel':

mod rotatesel <angle> <pos. of rotation vector> <rotation vector>

Rotates the current selection <angle> degrees around the vector <rotation vector>,
which is positioned at <pos. of rotation vector>. The position and vector are formatted
as three numbers, <x> <y> <z>, separated by space.

````

### sigma

````text
Overview of commands of the form 'mod sigma':

mod sigma id <atom index> to <sigma>
mod sigma var <variable name> to <sigma>

Changes the van der Waals radius, denoted as sigma, to the value stated by <sigma>.
This is not to be confued with the Lennard-Jones sigma for the assigned force fields.
The van der Waals radius is used in calculations such as for iso surfaces describing
a surface plot surrounding a molecule or atomic cluster.

The atom index to receive the new value of sigma is identified by
* directly assigning the atom index, <atom index>
* a variable, <variable name>, containing the atom index

````

### userdefpbc

````text
Overview of commands of the form 'mod userdefpbc':

mod userdefpbc <x low> <x high> <y low> <y high> <z low> <z high>

Sets a user defined periodic boundary condition (PBC). Note that the
user defined PBC will not automatically be applied, but needs to be
activated through the 'set' command.

````

## set

*Set various properties of MolTwister and the loaded systems*

````text
Usage: set projection <projection>
       set fullscreen <fullscreen>
       set userdefpbc <userdefpbc>
       set bondacrosspbc <bondacrosspbc>
       set redrawlimit <num atoms>
       set fog <fog>
       set usevdwradius <usevdwradius>
       set vdwscalefactor <vdwscalefactor>
       set labels <labels>
       set labelsfontsize <fontsize>
       set backgroundcolor <r> <g> <b>
       set backgroundcolor default

This command will set various properties of MolTwister or its loaded systems.

The <projection> can be either 'perspective' or 'ortho'.

The <fullscreen> parameter can be either 'on' or 'off'.

The <userdefpbc> parameter can be either 'on' or 'off'. If 'on', the user defined
periodic boundary conditions (PBC) set by the 'mod userdefpbc' command is applied.
If 'off', the PBC will be collected elsewhere.

The <bondacrosspbc> parameter can be either 'on' or 'off'. If 'on', bonds acrsss
PBCs will be visible in the 3D view.

The 'redrawlimit' sub-command will specify that, during rotation / panning / scaling
of the scene, scenes with more than <num atoms> atoms will only be displayed with the
axis system. All atoms will be redrawn once the corresponding mouse button is released.

The <fog> can be either 'on' or 'off'.

The <usevdwradius> can be either 'on' or 'off'. If 'on' the atoms are drawn using the
built in van der Waals radius multiplied by <vdwscalefactor> (default 1).

The <labels> can be either 'on' or 'off'. If 'on', the atom and bond labels defined in the
'add atom' command will be displayed.

The <fontsize> can be either 10, 12 or 18.

The color values <r> <g> <b> (red, green and blue) must be between 0 and 1.
````

## get

*Get various properties of MolTwister and the loaded systems*

````text
Usage: get <args>

Retrieve various properties of the system. <args> can be:

       * atomtypes                 :   List all atom types present in system
       * mdinconsistency           :   Check if any inconsistencies in MD force-field can be found
       * bondinfo <ind 1> <ind 2>  :   Get a list of bonds connected to atom indices 1 and 2
       * userdefpbc                :   Get user defined PBCs
       * gpuinfo                   :   Check if CUDA is available and return info
````

## sel

*Create a selection of atoms*

````text
Usage: sel atom <atom index>
       sel atomname <atom ID (e.g., O, H, C7)> [within <condition>]
       sel all [within <domain>]
       sel none

This command will create an atomic selection, where the selected atoms will be highlighted
in the 3D view. The first time a selection is done, the selection is done. The second time
the same selection is done, the selection is unselected, except for 'sel all' and 'sel none'.

The <domain> parameter consists of a string without space, where several
conditions are specified, separated by boolean operators. The boolean
operators that can be applied are:
* not: !
* and: &
* and: *
* or: |
* or: +
* xor: ^
Each condition is on x, y or z and can be of the following types
* equal: =
* not equal: !=
* greater than: >
* greater than or equal: >=
* less than: <
* less than or equal: <=
For example,
* x>5
* x>5&x<10
* (x>5&x<10)|(x>20&x<23)
* !(x>5&x<10)
````

## print

*Print information (e.g., atom pos. in different file formats)*

````text
Usage: print <content>

Prints, to screen, or file if piped ('>'), the specified contents, where <contents> can be any of the following
* xyz [currentframe] [atomicunits]: atomic IDs and positions in the XYZ-file format
* xyz [frame <frame index>] [atomicunits]: atomic IDs and positions in the XYZ-file format
* pdb [currentframe]: atomic IDs and positions in the PDB-file format (Protein Data Bank file format)
* pdb [frame <frame index>]: atomic IDs and positions in the PDB-file format (Protein Data Bank file format)
* mtt [bondsfromff] [bondacrosspbc] [currentframe] [binary]: atomic IDs and positions in the MolTwister file format
* mtt [bondsfromff] [bondacrosspbc] [frame <frame index>] [binary]: atomic IDs and positions in the MolTwister file format
* version: version of the software
* mixff resname <resname 1> resname <resname 2> <mixing-rule>: finds a mixed set of force field parameters between atoms in
                                                               <resname 1> and <resname 2> and outputs a list of mixed
                                                               parameters in the form of MolTwister exec() commands, that
                                                               constitutes a script that sets up the mixing force field
* mixff atomnames <list of atom IDs 1> atomnames <list of atom IDs 2> <mixing-rule>: finds a mixed set of force field parameters
                                                                                     between atoms in <list of atom IDs 1> and
                                                                                     <list of atom IDs 2> and outputs a list
                                                                                     of mixed parameters in the form of MolTwister
                                                                                     exec() commands, that constitutes a script
                                                                                     that sets up the mixing force field

If 'atomicunits' is not specified, then the positions are given in Aangstroms. If 'bondsfromff' is specified,
bonds are taken from the given force field bonds, otherwise bonds are calculated based on atomic separations
and will be identical to the bonds visible in the 3D view. If 'bondacrosspbc' is not specified, then the bonds
will not stretch across the periodic boundary conditions (PBC). The 'binary' keyword will invoke a binary
version of the file format, if available. If neither 'currentframe' or 'frame' is specified, the default frame
to print is the frame at index 0. The <mixing-rule> parameter depends on the force fields to be mixed. In
some cases it is not possible to choose the rule (the parameter will be ignored), but in others it is either
'aritmetic' or 'geometric'. Note that the <list of atom IDs X> consists of a comma separated list of atom IDs
(e.g., H, O, C7) without any space.
````

## measure

*Perform a measurement*

````text
Overview of commands of the form 'measure <sub command>':

measure angle id <atom index 1> <atom index 2> <atom index 3>
measure angle var <variable name>
measure atompos id <atom index>
measure atompos var <variable name>
measure atompos sel
measure bondcount <atom ID>
measure bondlength id <atom index 1> <atom index 2>
measure bondlength var <variable name>
measure bondlengthdyn <DCD filename> <frame from> <frame to> <atom indices list (each pair defines a bond)>
measure bondsep id <atom index 1> <atom index 2>
measure bondsep var <variable name>
measure center sel
measure charge sel
measure charge tot
measure coulombenergy single [1to4coeffs <coeff 1> <coeff 2> <coeff 3> <coeff 4>]
measure coulombenergy dihedralrot <dihedral> rot <start angle> <end angle> <angular step size> [1to4coeffs <coeff 1> <coeff 2> <coeff 3> <coeff 4>]
measure coulombenergy anglerot <angle> rot <start angle> <end angle> <angular step size> [1to4coeffs <coeff 1> <coeff 2> <coeff 3> <coeff 4>]
measure coulombenergy bondstretch <bond> stretch <start dist> <end dist> <dist step size> [1to4coeffs <coeff 1> <coeff 2> <coeff 3> <coeff 4>]
measure coulombenergy allframes [1to4coeffs <coeff 1> <coeff 2> <coeff 3> <coeff 4>]
measure coulombpotential single at <x> <y> <z>
measure coulombpotential dihedralrot at <x> <y> <z> <dihedral> rot <angle start> <angle end> <angular step size>
measure count sel
measure count tot
measure dihedral <dihedral>
measure overlap <within> sel
measure pbc [<frame index>]
measure radiusofgyration sel

To get more information about a <sub command>, type 'help measure <sub command>

````

### angle

````text
Overview of commands of the form 'measure angle':

measure angle id <atom index 1> <atom index 2> <atom index 3>
measure angle var <variable name>

Measures the angle between three atoms (in degrees) by
* defining three atom indices directly by using the 'id' keyword
* obtaining atom indices from a variable by using the 'var' keyword

Output:
Theta(<atom index 1>, <atom index 2>, <atom index 3>) = <angle in degrees>

````

### atompos

````text
Overview of commands of the form 'measure atompos':

measure atompos id <atom index>
measure atompos var <variable name>
measure atompos sel

Measures the position of a single atom by
* defining the atom index directly by using the 'id' keyword
* obtaining atom indiex from a variable by using the 'var' keyword
* obtaining multiple atom indices from a selection by using the 'sel' keyword

Output (single atom measurement):
t(<x>, <y>, <z>)
Output (multiple atom measurement):
1. [<atom index>, <atom ID>, <resname>]: (<x>, <y>, <z>)
2. [<atom index>, <atom ID>, <resname>]: (<x>, <y>, <z>)
     .
     .
     .
N. [<atom index>, <atom ID>, <resname>]: (<x>, <y>, <z>)
where N is the number of selected atoms.

````

### bondcount

````text
Overview of commands of the form 'measure bondcount':

measure bondcount <atom ID>

Counts bonds connected to a given atom type, specified by <atom ID> (e.g., H, O, C7), and
presents a histogram that shows the number of atoms (with atom ID) with 0, 1, 2, .., >=9
bond connections.

Output:
1. n<=0 n=1 n=2 n=3 n=4 n=5 n=6 n=7 n=8 n>=9
2. <count n<=0> <count n=1> <count n=2> <count n=3> <count n=4> <count n=5> <count n=6> <count n=7> <count n=8> <count n>=9>

````

### bondlength

````text
Overview of commands of the form 'measure bondlength':

measure bondlength id <atom index 1> <atom index 2>
measure bondlength var <variable name>

Measures the length between two atoms by
* defining two atom indices directly by using the 'id' keyword
* obtaining atom indices from a variable by using the 'var' keyword

Output:
R(<atom index 1>, <atom index 2>) = <distance>

````

### bondlengthdyn

````text
Overview of commands of the form 'measure bondlengthdyn':

measure bondlengthdyn <DCD filename> <frame from> <frame to> <atom indices list (each pair defines a bond)>

Measures distances between atom pairs defined in <atom indices list>, which is comma
separated with no spaces alowed. E.g., <atom indices list>=0,3,5,6 will yield a
measurement of distances between atoms 0 and 3, as well as betwen atoms 5 and 6. More
distances are added by extending the list. The distances are measured from the DCD
file, <DCD filename>, for each frame between <frame from> and <frame to>. Thus, a
distance plot, as function of time, is provided for each selected sistance to measure.

Output:
1. <distance 1> <distance 2> ... <distance n>
2. <distance 1> <distance 2> ... <distance n>
         .
         .
         .
N. <distance 1> <distance 2> ... <distance n>
where N is the number of selected frames from the DCD file and n is the number of pairs
of atoms in <atom indices list>, where the order is the same as in <atom indices list>.

````

### bondsep

````text
Overview of commands of the form 'measure bondsep':

measure bondsep id <atom index 1> <atom index 2>
measure bondsep var <variable name>

Measures the number of bondsbetween two atoms by
* defining two atom indices directly by using the 'id' keyword
* obtaining atom indices from a variable by using the 'var' keyword

Output:
* BondSep(<atom index 1>, <atom index 2>) = <distance>
or
* BondSep(<atom index 1>, <atom index 2>) > 4
if the number of bonds exceeds 4.

````

### center

````text
Overview of commands of the form 'measure center':

measure center sel

Measures the geometric center of the collection of atoms defined by
* the visual selection of atoms, by using the 'sel' keyword

Output:
(<x_center>, <y_center>, <z_center>)

````

### charge

````text
Overview of commands of the form 'measure charge':

measure charge sel
measure charge tot

Measures the total charge of the collection of atoms defined by
* the visual selection of atoms, by using the 'sel' keyword
* all the atoms, by using the 'tot' keyword
If 'sel' or 'tot' is not specified, 'tot' is assumed.

Output:
Qsum = <summed charge>

````

### coulombenergy

````text
Overview of commands of the form 'measure coulombenergy':

measure coulombenergy single [1to4coeffs <coeff 1> <coeff 2> <coeff 3> <coeff 4>]
measure coulombenergy dihedralrot <dihedral> rot <start angle> <end angle> <angular step size> [1to4coeffs <coeff 1> <coeff 2> <coeff 3> <coeff 4>]
measure coulombenergy anglerot <angle> rot <start angle> <end angle> <angular step size> [1to4coeffs <coeff 1> <coeff 2> <coeff 3> <coeff 4>]
measure coulombenergy bondstretch <bond> stretch <start dist> <end dist> <dist step size> [1to4coeffs <coeff 1> <coeff 2> <coeff 3> <coeff 4>]
measure coulombenergy allframes [1to4coeffs <coeff 1> <coeff 2> <coeff 3> <coeff 4>]

Measures the Coulomb energy using one of the following choices
* 'single':      sum Coulomb energies between all atoms, N, by (i=0..N, j=i+1..N)
                 with output 'Etot = <energy> kJ/mol'.
* 'dihedralrot': performs a 'single' for dihedral angles, specified by the
                 <dihedral>, <start angle>, <end angle>, <angular step size>
                 parameters. The output is a two column space sepatated list
                 with header 'Angle [deg] Etot [kJ/mol]'.
* 'anglerot':    performs a 'single' for molecular angles, specified by the
                 <angle>, <start angle>, <end angle>, <angular step size>
                 parameters. The output is a two column space sepatated list
                 with header 'Angle [deg] Etot [kJ/mol]'.
* 'bondstretch': performs a 'single' for bond stretching, specified by the
                 <bond>, <start dist>, <end dist>, <dist step size>
                 parameters. The output is a two column space sepatated list
                 with header 'Dist [AA] Etot [kJ/mol]'.
* 'allframes':   performs a 'single' for all the loaded frames and produces
                 a two column output of the form 'Frame Etot [kJ/mol].'

The <dihedral> parameter specifies the dihedral to rotate and can be formatted
as shown below.
* id <atom index 1> <atom index 2> <atom index 3> <atom index 4>
* var <variable name>

The <angle> parameter specifies the angle to rotate and can be formatted as
shown below.
* id <atom index 1> <atom index 2> <atom index 3>
* var <variable name>

The <bond> parameter specifies the bond to stretch and can be formatted as
shown below.
* id <atom index 1> <atom index 2>
* var <variable name>
	
The '1to4coeffs' keyword can be applied to assign different weights to the
Coulomb energy calculations that are separated with 1, 2, 3 and 4 bonds,
respectively, by assigning values to <coeff 1> through <coeff 4>.

````

### coulombpotential

````text
Overview of commands of the form 'measure coulombpotential':

measure coulombpotential single at <x> <y> <z>
measure coulombpotential dihedralrot at <x> <y> <z> <dihedral> rot <angle start> <angle end> <angular step size>

Measures the Coulomb potential, at a given point, of a test charge, +1,
using one of the following choices
* 'single':      find coulomb energy at <x> <y> <z>, with output 'Utot =
                  <energy> kJ/C'.
* 'dihedralrot': find coulomb energy at <x> <y> <z> for several dihedral
                 rotations between <angle start> and <angle end> with steps
                 <angular step size> (in degrees), where the rotation is
                 applied to the specified dihedral, <dihedral>.

The <dihedral> parameter specifies the dihedral to rotate and can be formatted
as shown below.
* id <atom index 1> <atom index 2> <atom index 3> <atom index 4>
* var <variable name>

````

### count

````text
Overview of commands of the form 'measure count':

measure count sel
measure count tot

Counts number of atoms. Either the total count can be measured, using 'tot',
or only the selected atoms can be measured, using 'sel'. If 'tot' or 'sel' are
not specified, the default behavior is 'tot'.

Output:
Nsum = <atom count>

````

### dihedral

````text
Overview of commands of the form 'measure dihedral':

measure dihedral <dihedral>

Measures the dihedral angle, given by the <dihedral> parameter. This
parameter specifies the dihedral to rotate and can be formatted
as shown below.
* id <atom index 1> <atom index 2> <atom index 3> <atom index 4>
* var <variable name>

Output:
Phi(<dih. index 1>, <dih. index 2>, <dih. index 3>, <dih. index 4>) = <angle>
where <angle> is given in degrees.

````

### overlap

````text
Overview of commands of the form 'measure overlap':

measure overlap <within> sel

Counts number of overlaping atoms within the current frame by counting the
number of atoms that are closer than the distance <within>. The count is
done for all atoms. Specifying the 'sel' keyword will select one atom of
every overlapping pair of atoms.

Output:
1. Total number of overlaps within <within> = N
2.
3. Atom i  Atom j  Type i  Type j  r_ij
4. ---------------------------------------------------------------------------
5. <index i> <index j> <atom ID i> <atom ID j> <distance from i to j>
6. <index i> <index j> <atom ID i> <atom ID j> <distance from i to j>
       .
       .
       .
N+4. <index i> <index j> <atom ID i> <atom ID j> <distance from i to j>
where N is the number of identified overlaps.

````

### pbc

````text
Overview of commands of the form 'measure pbc':

measure pbc [<frame index>]

Measures the current periodic bounary conditions of frame <frame index>.
The frame index is, by default, the current frame index (i.e., the visible
frame), in case <frame index> is omitted.

Output:
1. x = [<x low>, <x high>]
2. x = [<y low>, <y high>]
3. x = [<z low>, <z high>]
If the PBC is user defined, this will be notified by the message 'User
defined PBC!', in addition to the above.

````

### radiusofgyration

````text
Overview of commands of the form 'measure radiusofgyration':

measure radiusofgyration sel

Measures the radius of gyration for the collection of atoms defined by
* the visual selection of atoms, by using the 'sel' keyword
If 'sel' is not specified, 'sel' is assumed.

Note that atomic masses must be loaded for this command to work!

Output:
Rgyr = <radius of gyration>

````

## calculate

*Perform a calculation*

````text
Overview of commands of the form 'calculate <sub command>':

calculate com <selection>
calculate densitymap <DCD filename> <stride> <span> <last frame to load> <num bins> <atoms (comma sep, no space)> <direction>
calculate densityprofile <DCD filename> <frame from> <frame to> <atoms (comma sep, no space)> <vec. from> <vec. to> <num bins> [within simbox <vec. low> <vec. high>, within cylinder <radius>]
calculate dihedraldistr <DCD filename> <frame from> <frame to> <num bins> indices <index 1> <index 2> <index 3> <index 4>
calculate dihedraldistr <DCD filename> <frame from> <frame to> <num bins> atomtypes <atom ID 1> <atom ID 2> <atom ID 3> <atom ID 4> <bond cutoff r> [ignorepbc]
calculate dihedraldistrcom <DCD filename> <frame from> <frame to> <num bins> <num COM bins> <COM dir> <COM range> indices <index 1> <index 2> <index 3> <index 4> [ignorepbc] indices <M> <COM mol index 1> ... <COM mol index M>
calculate dihedraldistrcom <DCD filename> <frame from> <frame to> <num bins> <num COM bins> <COM dir> <COM range> indices <index 1> <index 2> <index 3> <index 4> [ignorepbc] atomtypes <M> <COM mol atom ID 1> ... <COM mol atom ID M>
calculate dihedraldistrcom <DCD filename> <frame from> <frame to> <num bins> <num COM bins> <COM dir> <COM range> atomtypes <atom ID 1> <atom ID 2> <atom ID 3> <atom ID 4> <bond cutoff r> [ignorepbc] indices <M> <COM mol index 1> ... <COM mol index M>
calculate dihedraldistrcom <DCD filename> <frame from> <frame to> <num bins> <num COM bins> <COM dir> <COM range> atomtypes <atom ID 1> <atom ID 2> <atom ID 3> <atom ID 4> <bond cutoff r> [ignorepbc] atomtypes <M> <COM mol atom ID 1> ... <COM mol atom ID M>
calculate dipmom <DCD filename> <frame index> <list of atomic IDs (comma sep., no space)> [chargedmol]
calculate dipmomperturbcharge <DCD filename> <frame index> <list of atomic IDs> <positive charges> <negative charges> <num perturbations> <delta Q> [chargedmol]
calculate dipmomperturbzpos <DCD filename> <frame index> <list of atomic IDs> <atoms to displace> <delta Z> <num perturbations> [chargedmol]
calculate dipmomperturbzposexchange <DCD filename> <frame index> <list of atomic IDs> <positive charges> <negative charges> <delta Z pos. charges> <delta Z neg. charges> [chargedmol]
calculate dipmomprofile <DCD filename> <frame from> <frame to> <molecule resname> <x-dir.vec> <y-dir.vec> <z-dir.vec> <num. bins> <min. dist.> <max. dist.> [chargedmol] [forcecomplete <num. atoms in complete molecule>]
calculate distprobabilitycom <DCD filename> <frame from> <frame to> <num dist. bins> <start dist. range> <end dist. range> <num COM bins> <COM direction> <start COM range> <end COM range> <atom IDs from> <atom IDs to> indices <M> <COM base index 1> ... <COM base index M> [ignorepbc]
calculate distprobabilitycom <DCD filename> <frame from> <frame to> <num dist. bins> <start dist. range> <end dist. range> <num COM bins> <COM direction> <start COM range> <end COM range> <atom IDs from> <atom IDs to> atomtypes <M> <COM base ID 1> ... <COM base ID M> [ignorepbc]
calculate energybetween nonbonded <comma sep. list of atom IDs> <FF index (zero based)>
calculate energybetween coulomb <comma sep. list of atom IDs>
calculate energybetween bond <comma sep. list of atom IDs> <FF index (zero based)>
calculate energybetween angle <comma sep. list of atom IDs> <FF index (zero based)>
calculate energybetween dihedral <comma sep. list of atom IDs> <FF index (zero based)>
calculate energyoftranslation <xs> <ys> <zs> <xe> <ye> <ze> <steps>
calculate fft <ASCII file with 1 or more columns> <index of columns with real numbers> <index of column with imaginary numbers> <direction> [zeropad]
calculate forcebetween nonbonded <comma sep. list of atom IDs> <FF index (zero based)> [numerical]
calculate forcebetween coulomb <comma sep. list of atom IDs>
calculate forcebetween bond <comma sep. list of atom IDs> <FF index (zero based)> [numerical]
calculate forcebetween angle <comma sep. list of atom IDs> <FF index (zero based)> [numerical]
calculate forcebetween dihedral <comma sep. list of atom IDs> <FF index (zero based)> [numerical]
calculate hbondcount <DCD filename> <frame from> <frame to> stride <stride> <M> <h-bond crit 1> ... <h-bond crit M> [pbcfromvisual] [searchtype <type>] [nopbc]
calculate hbondcount <DCD filename> <frame from> <frame to> <M> <h-bond crit 1> ... <h-bond crit M> [pbcfromvisual] [searchtype <type>] [nopbc]
calculate loading <DCD filename> <frame from> <frame to> <atom IDs to find loading for> <lower vector - loading region> <upper vector - loading region>
calculate msd <DCD filename> <frame from> <frame to> resname <resname> [usegeomcent] [numshiftsint0 <num shifts>]
calculate paircorrelation <DCD filename> <frame from> <frame to> <atom ID 1> <atom ID 2> <num bins> <min. dist> <max. dist> [ignorepbc] [ignoredistbelow <dist>]
calculate potenergymap <DCD filename> <frame from> <frame to> <atom IDs> <list of applied force fields> <Nx> <Ny> <cutting plane> <cutting plane pos>
calculate qbal <group of atoms to modify>
calculate vacf <DCD filenmae> <frame from> <frame to> <time step (fs)> <VACF length> name <atom IDs (comma sep., no space)>
calculate vacf <DCD filenmae> <frame from> <frame to> <time step (fs)> <VACF length> sel
calculate vdos <DCD filename> <frame from> <num. bits> <time step (fs)> name <atom IDs (comma sep., no space)>
calculate vdos <DCD filename> <frame from> <num. bits> <time step (fs)> sel
calculate volumefromdensity <target density in kg/m^3> <atomic masses in g/mol> <num. molecules>

To get more information about a <sub command>, type 'help calculate <sub command>

````

### com

````text
Overview of commands of the form 'calculate com':

calculate com <selection>

Calculates the center of mass (COM) of the specified selection. The possible selections are:
* sel - calculate the COM of the selected atoms.

````

### densitymap

````text
Overview of commands of the form 'calculate densitymap':

calculate densitymap <DCD filename> <stride> <span> <last frame to load> <num bins> <atoms (comma sep, no space)> <direction>

Calculates density maps of a specified atomic selection. Each density map that is generated will be
a count taken over <span> frames of the specified DCD file, centered at an index, t. The index, t,
runs over all frames of the DCD file, up to <last frame to load>, where each index is separated by <stride>
frames. I.e., the number of generated density maps are approximately <last frame to load> / <stride>.
Each map is generated by specifying a plane, given by <direction> in {xy, yz, zx}, for which all atoms
are projected into. The atoms specified by the comma separated list of atom IDs (i.e., <atoms>, s.a.,
O, H, C5, etc.) are counted within every bin of the defined plane, where each side of the plane are
divided into <num bins> equisized bins. The output is formatted as described below.

First a header, consisting of three lines are output:
1. x(t,n,val) y(t,n,val) xbin(t,n,val) ybin(t,n,val) bincnt(t,n,val) ...
2. <t>        <t>        <t>           <t>           <t>             ...
3. <Nx>       <Ny>       <num bins>^2  <num bins>^2  <num bins>^2    ...
The first line describes each column that will follow in the data section, located immediately below the
header. The ellipsis (...) denotes that the columns are repeated for each output frame, <t>, which is
counted over <span>/2 on each side. Note that if the span is outside the available frames of the DCD
file, then these frames are simply ignored. The number of points (i.e., the length of the data columns
below) are given by <Nx> and <Ny>., while the total number of bins in the specified plane is given by the
square of <num bins>. The data section is formatted as:
4. <x> <y> <x center pos. of bin containing (x,y)> <y center pos. of bin containing (x,y)> <bin count of bin containing (x,y) ...
5. <x> <y> <x center pos. of bin containing (x,y)> <y center pos. of bin containing (x,y)> <bin count of bin containing (x,y) ...
                                    .
                                    .
                                    .
N+3. <x> <y> <x center pos. of bin containing (x,y)> <y center pos. of bin containing (x,y)> <bin count of bin containing (x,y) ...
where N is the number of selected atoms.

````

### densityprofile

````text
Overview of commands of the form 'calculate densityprofile':

calculate densityprofile <DCD filename> <frame from> <frame to> <atoms (comma sep, no space)> <vec. from> <vec. to> <num bins> [within simbox <vec. low> <vec. high>, within cylinder <radius>]

Calculates the density profile from an atomic selection, sepcified by the list of atom IDs,
<atoms>, s.a., O, H, C5, etc (no cammas). The atomic positions are taken from the specified
DCD file, from <frame from> to <frame to>. The density profile is calculated along the vector
V = <vec. to> - <vec. from>, where <vec. from> and <vec. to>, both are specified on the form
<x> <y> <z>. The density profile is divided into <num bins> between these two points. Optionally,
the 'within' keyword can be used to specify a cutoff for which atoms are not included into the
density profile. If simbox is used, then atoms outside the lower corner <vec. low> and the upper
corner <vec. high> (both of the form <x> <y> <z>), are not counted. If cylinder is used, then all
atoms outside the cylinder of radius <radius> with its core along V are ignored. If within is not
specified, then all atoms are counted. The output is specified below.

Note that there are three types of calculated densities from this command, these are
* Particle density
* Mass density
* Charge density

The output is as follows, starting with a single header line,
1. abs_x abs_y abs_z rel_x rel_y rel_z #/frm m/frm Q/frm
2. <x abs> <y <bs> <z abs> <x rel> <y rel> <z rel> <particle density> <mass density> <charge density>
3. <x abs> <y <bs> <z abs> <x rel> <y rel> <z rel> <particle density> <mass density> <charge density>
                 .
                 .
                 .
N+1. <x abs> <y <bs> <z abs> <x rel> <y rel> <z rel> <particle density> <mass density> <charge density>
where N is the number of bins. Relative positions (rel) are relative to <vec. from>, while absolute
positions (abs) are relative to the simulation box. Mass density is in [g/mol], while charge density
is in units of partial charges.

````

### dihedraldistr

````text
Overview of commands of the form 'calculate dihedraldistr':

calculate dihedraldistr <DCD filename> <frame from> <frame to> <num bins> indices <index 1> <index 2> <index 3> <index 4>
calculate dihedraldistr <DCD filename> <frame from> <frame to> <num bins> atomtypes <atom ID 1> <atom ID 2> <atom ID 3> <atom ID 4> <bond cutoff r> [ignorepbc]

Calculates the dihedral distribution, based on the contents of the <DCD filename> DCD file,
from frame index <frame from> to frame index <frame to>. The number of bins to divide the full
360 degree angle into is defined by <num bins>. It is possible to either define four atom
indices (zero based) and hence study the conformational distributions of a single dihedral
over time, or to specify four atom IDs (e.g., H, O, C5) and study a specific type of dihedral
over time (i.e., several dihedrals of the same type, per frame). In the latter case, one also
need to specify the bond cutoff radius, <bond cutoff r>, which defines how long a bond should
be before it is no longer considered a bond. By default, bonds reach across periodic boundaries
(PBC), but it is possible to only consider bonds within the PBC by specifying 'ignorepbc'. The
output from the calculation is given below.

The output starts with the following header:
1. Num Bins: <applied number of bins>
2. From frame: <starting frame index>
3. To frame: <to frame index>
4. Bond cutoff: <applied bond cutoff>
5. Dihedral: <specified dihedral that was studied>
6. Index Angle Tot.Count Count/Frm

Following the header is the data section:
7. <bin index> <angle in degrees> <number of entries in this bin> <number of entries per frame in this bin>
8. <bin index> <angle in degrees> <number of entries in this bin> <number of entries per frame in this bin>
                      .
                      .
                      .
N+6. <bin index> <angle in degrees> <number of entries in this bin> <number of entries per frame in this bin>
where N is the number of bins.

````

### dihedraldistrcom

````text
Overview of commands of the form 'calculate dihedraldistrcom':

calculate dihedraldistrcom <DCD filename> <frame from> <frame to> <num bins> <num COM bins> <COM dir> <COM range> indices <index 1> <index 2> <index 3> <index 4> [ignorepbc] indices <M> <COM mol index 1> ... <COM mol index M>
calculate dihedraldistrcom <DCD filename> <frame from> <frame to> <num bins> <num COM bins> <COM dir> <COM range> indices <index 1> <index 2> <index 3> <index 4> [ignorepbc] atomtypes <M> <COM mol atom ID 1> ... <COM mol atom ID M>
calculate dihedraldistrcom <DCD filename> <frame from> <frame to> <num bins> <num COM bins> <COM dir> <COM range> atomtypes <atom ID 1> <atom ID 2> <atom ID 3> <atom ID 4> <bond cutoff r> [ignorepbc] indices <M> <COM mol index 1> ... <COM mol index M>
calculate dihedraldistrcom <DCD filename> <frame from> <frame to> <num bins> <num COM bins> <COM dir> <COM range> atomtypes <atom ID 1> <atom ID 2> <atom ID 3> <atom ID 4> <bond cutoff r> [ignorepbc] atomtypes <M> <COM mol atom ID 1> ... <COM mol atom ID M>

Calculates the dihedral distribution, based on the contents of the <DCD filename> DCD file,
from frame index <frame from> to frame index <frame to>. The number of bins to divide the full
360 degree angle into is defined by <num bins>. It is possible to either define four atom
indices (zero based) and hence study the conformational distributions of a single dihedral
over time, or to specify four atom IDs (e.g., H, O, C5) and study a specific type of dihedral
over time (i.e., several dihedrals of the same type, per frame). In the latter case, one also
need to specify the bond cutoff radius, <bond cutoff r>, which defines how long a bond should
be before it is no longer considered a bond. By default, bonds reach across periodic boundaries
(PBC), but it is possible to only consider bonds within the PBC by specifying 'ignorepbc'.

In the output from this calculation, the bins will be identified by the center of mass (COM)
positions of the molecules containing the dihedral angles that are counted. Thus, the COM molecule
must be identified, as well as the range of COM position that are to be binned and the number of
such bins. This is done through either specifying the 'indices' or 'atomtypes', together with the
number of indices or molecules, <M>, and the indices, <COM mol index i>, or atom IDs (such as H,
O, C6), <COM mol atom ID i>. The COM bins are defined by <num COM bins> <COM range> (of the form
<start pos> <end pos>), as well as the axis along which to perform binning <COM dir> in {x, y, z}.
	
The output from the calculation is given below.

The output starts with the following header:
1. Num angle Bins: <applied number of bins>
2. Num COM Bins: <applied number of bins>
3. Start COM range: <applied start pos>
4. End COM range: <applied end pos>
5. COM range direction (0=x,1=y,2=z): <applied direction>
6. From frame: <starting frame index>
7. To frame: <to frame index>
8. Bond cutoff: <applied bond cutoff>
9. Dihedral: <specified dihedral that was studied>
10. COMPos Angle Tot.Count Normalized

Following the header is the data section:
11. <COM pos> <angle in degrees> <number of entries in this bin> <normalized num. entries, max is unity>
12. <COM pos> <angle in degrees> <number of entries in this bin> <normalized num. entries, max is unity>
                      .
                      .
                      .
Na*Nb+10. <COM pos> <angle in degrees> <number of entries in this bin> <normalized num. entries, max is unity>
where Na is the number of COM bins and Nb is the number of angle bins (i.e., this constitutes.
a surface plot with angle along one axis and molecular COM along the other).


````

### dipmom

````text
Overview of commands of the form 'calculate dipmom':

calculate dipmom <DCD filename> <frame index> <list of atomic IDs (comma sep., no space)> [chargedmol]

Calculates the dipole moment of the selected molecules <list of atomic IDs>. The
dipolemoment is averaged based on all the defined molecules in frame index given by
<frame index>. Either one can choose a DCD file through <DCD filename> as input, or
it is possible to use the loaded frames as input by letting <DCD filename> =
__fromloadedframes__. The dipole moment expression for neutral molecules are used as
default. By sepcifying 'chargedmol', the dipole moment expression for charged molecules
is employed. The output is defined below.

Output:
Px Py Pz
<dipole moment x-component> <dipole moment y-component> <dipole moment z-component>

````

### dipmomperturbcharge

````text
Overview of commands of the form 'calculate dipmomperturbcharge':

calculate dipmomperturbcharge <DCD filename> <frame index> <list of atomic IDs> <positive charges> <negative charges> <num perturbations> <delta Q> [chargedmol]

Calculates the dipole moment of the selected molecules <list of atomic IDs> (comma
separated, no space). The dipolemoment is averaged based on all the defined molecules
in frame index given by <frame index>. The DCD file, <DCD filename>, is used as input.
The dipole moment expression for neutral molecules are used as default. By sepcifying
'chargedmol', the dipole moment expression for charged molecules is employed. The dipole
moment is perturbed through <num perturbations> steps, where the positive and negative
charges are all increased and decreased, respectively, by <delta Q> spread over the
respective groups of charges, thus preserving charge neutrality. The greatest perturbation
is calculated first. Note that the lists of positive and negative charges are lists of
atomic IDs, such as H, O and C7 (comma separated, no space).

Output:
1. QPerturb+ QPerturb- Px Py Pz
2. <positive perturbation> <negative perturbation> <dipole moment x-component> <dipole moment y-component> <dipole moment z-component>
3. <positive perturbation> <negative perturbation> <dipole moment x-component> <dipole moment y-component> <dipole moment z-component>
                 .
                 .
                 .
N+1. <positive perturbation> <negative perturbation> <dipole moment x-component> <dipole moment y-component> <dipole moment z-component>
where N is the number of perturbations.

````

### dipmomperturbzpos

````text
Overview of commands of the form 'calculate dipmomperturbzpos':

calculate dipmomperturbzpos <DCD filename> <frame index> <list of atomic IDs> <atoms to displace> <delta Z> <num perturbations> [chargedmol]

Calculates the dipole moment of the selected molecules <list of atomic IDs> (comma
separated, no space). The dipolemoment is averaged based on all the defined molecules
in frame index given by <frame index>. The DCD file, <DCD filename>, is used as input,
or the loaded frames if <DCD filename> = __fromloadedframes__. The dipole moment expression
for neutral molecules are used as default. By sepcifying 'chargedmol', the dipole moment
expression for charged molecules is employed. The dipole moment is perturbed through
<num perturbations> steps, where the <atomd to displace> (comma separated, no space)
are all displaced by the z-position <delta Z>. Note that <atoms to displace> is a lists of
atomic IDs, such as H, O and C7.

Output:
1. Step Px Py Pz Rel.z.pos
2. <step> <dipole moment x-component> <dipole moment y-component> <dipole moment z-component> <Z pos displacement>
3. <step> <dipole moment x-component> <dipole moment y-component> <dipole moment z-component> <Z pos displacement>
                 .
                 .
                 .
N+1. <step> <dipole moment x-component> <dipole moment y-component> <dipole moment z-component> <Z pos displacement>
where N is the number of perturbations.

````

### dipmomperturbzposexchange

````text
Overview of commands of the form 'calculate dipmomperturbzposexchange':

calculate dipmomperturbzposexchange <DCD filename> <frame index> <list of atomic IDs> <positive charges> <negative charges> <delta Z pos. charges> <delta Z neg. charges> [chargedmol]

Calculate dipole moment while incrementally increasing the number of ion-swaps to
perform. Swapping the ions will naturally keep the system charge neutral. The ion
indices to exchange are chosen at random. I.e., in the first iteration a random
ion-pair is chosen. Then, in the next iteration, the first pair remains the same
while the second pair is randomly chosen. An ion pair is constructed by first
selecting an index from the positive ions and then finding the negative ion
which is the closest to the chosen positive ion. The the dipole moment is calculated
for the selected molecules <list of atomic IDs> (comma separated, no space). The
dipolemoment is averaged based on all the defined molecules in frame index given by
<frame index>. The DCD file, <DCD filename>, is used as input, or the loaded frames if
<DCD filename> = __fromloadedframes__. The dipole moment expression for neutral molecules
are used as default. By sepcifying 'chargedmol', the dipole moment expression for charged
molecules is employed. The lists of positive and negative charges are lists of atomic IDs,
such as H, O and C7 (comma separated, no space), which defines groups of positive and
negative charges that can form the aforementioned ion pairs to swap (i.e., ions are picked
from these groups). The <delta Z pos. charges> and <delta Z neg. charges> parameters
define the z-axis displacement to perform on the positive and negative charges,
respectively.

Output:
1. #Exchanges Px Py Pz Exch.index
2. <number of performed exchanges> <dipole moment x-component> <dipole moment y-component> <dipole moment z-component> <Index of the positive ion>
3. <number of performed exchanges> <dipole moment x-component> <dipole moment y-component> <dipole moment z-component> <Index of the positive ion>
                 .
                 .
                 .
N+1. <number of performed exchanges> <dipole moment x-component> <dipole moment y-component> <dipole moment z-component> <Index of the positive ion>
N+2.             .
N+3.             .
             <N atomic system configurations, for each exchange step that has been performed, in the xyz-file format>
M.               .
M+1.             .
where N is the number of perturbations.

````

### dipmomprofile

````text
Overview of commands of the form 'calculate dipmomprofile':

calculate dipmomprofile <DCD filename> <frame from> <frame to> <molecule resname> <x-dir.vec> <y-dir.vec> <z-dir.vec> <num. bins> <min. dist.> <max. dist.> [chargedmol] [forcecomplete <num. atoms in complete molecule>]

Calculates the dipole moment of the selected molecules from <molecule resname> (string
with resname). The dipolemoment is averaged based on all the defined molecules
in frames <frame from> to <frame to>. The DCD file, <DCD filename>, is used as input.
	
The dipole moment expression for neutral molecules are used as default. By sepcifying
'chargedmol', the dipole moment expression for charged molecules is employed. A profile
of the dipolemoment, for the defined molecule type, is created along the unit vector
created from the vector (<x-dir.vec>, <y-dir.vec>, <z-dir.vec>) from the distance <min.
dist.> to the distance <max. dist.>, where the distances are molecular center of mass
(COM) distances that are projected along the specified direction unit vector.The
projected COM for each molecule where the dipole moment is calculated will be binned into
<num. bins> bins between the specified minimum and maximum distances. Each bin will contain
an average dipole moment from all molecules belonging to that bin. By default, the algorithm
will not check if a molecule is wrapped accross periodic boundaries, thus resulting in
incomplete molecules being processed. This can be fixed by applying the 'forcecomplete'
keyword with the number of atoms to expect in a molecule, <num. atoms in complete molecule>.
Molecules that do not conform to the specified atom count will not be included in the
dipole moment profile.

Output:

1. From frame: <first frame to include in average>
2. To frame: <last frame to include in average>
3. Resname: <resname for selected molecules>
4. Range: [<min. dist>, <max. dist>]
5. Num. bins: <num. bins>
6. Rc projection: <UnitVec(<x-dir.vec>, <y-dir.vec>, <z-dir.vec>)>
7. Distance Count P_x P_y P_z P_abs
8. <projected COM distance for bin (center of bin)> <num bin entries> <dipole moment x-component> <dipole moment y-component> <dipole moment z-component> <absolute dipole moment>
9. <projected COM distance for bin (center of bin)> <num bin entries> <dipole moment x-component> <dipole moment y-component> <dipole moment z-component> <absolute dipole moment>
                 .
                 .
                 .
N+7. <projected COM distance for bin (center of bin)> <num bin entries> <dipole moment x-component> <dipole moment y-component> <dipole moment z-component> <absolute dipole moment>
where N is the number of bins.

````

### distprobabilitycom

````text
Overview of commands of the form 'calculate distprobabilitycom':

calculate distprobabilitycom <DCD filename> <frame from> <frame to> <num dist. bins> <start dist. range> <end dist. range> <num COM bins> <COM direction> <start COM range> <end COM range> <atom IDs from> <atom IDs to> indices <M> <COM base index 1> ... <COM base index M> [ignorepbc]
calculate distprobabilitycom <DCD filename> <frame from> <frame to> <num dist. bins> <start dist. range> <end dist. range> <num COM bins> <COM direction> <start COM range> <end COM range> <atom IDs from> <atom IDs to> atomtypes <M> <COM base ID 1> ... <COM base ID M> [ignorepbc]

Calculates a two dimensional surface plot with the molecular center of mass (COM) along
one axis. The other axis is the length between atoms of the set defined by <atom IDs from>
(comma separated, no space) and the set defined by <atom IDs to>. Each of the axis are
binned according to <num COM bins> and <num dist. bins>, respectively. The range of the
two axis are defined by [<start COM range>, <end COM range>] and by [<start dist. range>,
<end dist. range>], respectively. The <COM direction> can be either x, y or z and defines
which of the components of the COM vector are to be binned. The set of atoms that is used
as basis for the COM calculations are defined by specifying the number of atoms, <M>,
followed by the atom indices <COM base index i>, if 'indices' is specified, or by the
atom IDs (e.g., H, O, C7) <COM base ID i>, if 'atomtypes' is specified. By default the
periodic boundary conditions (PBC) are taken into account (i.e., distances are measured
across PBC boundaries. However, PBCs can be ignored by using the 'ignorepbc' keyword.
	
Trajectories used as input is specified by <DCD filename>, where all frames from <frame from>
to <frame to> are used in the averaging (i.e., contributing to counts in the 2D grid of
specified bins).
	
Output:
1. Num distance bins: <num dist. bins>
2. Start dist. range: <start dist. range>
3. End dist. range: <end dist. range>
4. Num COM bins: <num COM bins>
5. Start COM range: <start COM range>
6. End COM range: <end COM range>
7. COM range direction (0=x,1=y,2=z): <COM direction>
8. From frame: <frame from>
9. To frame: <frame to>
10. Dist from: <atom IDs from>
11. Dist to: <atom IDs to>
12. COMPos Dist Tot.Count Normalized
13. <COM positio binn> <distance between atoms bin> <total count in bin> <normalized count, max set to unity>
14. <COM positio binn> <distance between atoms bin> <total count in bin> <normalized count, max set to unity>
                .
                .
                .
Na*Nb+12. <COM positio binn> <distance between atoms bin> <total count in bin> <normalized count, max set to unity>
where Na and Nb are the number of COM bins and distance bins, respectively.

````

### energybetween

````text
Overview of commands of the form 'calculate energybetween':

calculate energybetween nonbonded <comma sep. list of atom IDs> <FF index (zero based)>
calculate energybetween coulomb <comma sep. list of atom IDs>
calculate energybetween bond <comma sep. list of atom IDs> <FF index (zero based)>
calculate energybetween angle <comma sep. list of atom IDs> <FF index (zero based)>
calculate energybetween dihedral <comma sep. list of atom IDs> <FF index (zero based)>

Calculates the nonbonded, Coulomb or bond energy (in kJ/mol) betwee two atoms, based on the
configured force fields, or the angular energy between three atoms or the dihedral energy
between four atoms. The energies are calculated based on the atoms shown in the current frame.
The indices of the various force fields that have been created or loaded is available through
the 'list' command. An index into these lists, for the appropriate force field type, is specified
through the <FF index (zero based)> parameter. The energy is calculated between the atoms given
in <comma sep. list of atom IDs> (e.g., H, O, C7, where the list must be enetered withour spece).

````

### energyoftranslation

````text
Overview of commands of the form 'calculate energyoftranslation':

calculate energyoftranslation <xs> <ys> <zs> <xe> <ye> <ze> <steps>

Calculates the total non-bonded energy of the system shown in the current frame for repeated
translations of the selected atoms in the current frame. The selected atoms are translated along
from start vector, (<xs>, <ys>, <zs>) to the end vector, (<xe> <ye> <ze>), in <step> steps. The
non-bonded force fields must be loaded or created before running this command.

Output:
1. Position ENon-bond
2. <x> <y> <z> <total system energy (kJ/mol)>
3. <x> <y> <z> <total system energy (kJ/mol)>
              .
              .
              .
N+1. <x> <y> <z> <total system energy (kJ/mol)>
where N is the number of steps. The position (<x>, <y>, <z>) are the positions between the
start vector and the end vector.

````

### fft

````text
Overview of commands of the form 'calculate fft':

calculate fft <ASCII file with 1 or more columns> <index of columns with real numbers> <index of column with imaginary numbers> <direction> [zeropad]

Calculates the FFT (Fast Fourier Transform) of a list of complex numbers, taken from an ASCII file
containing several coulumns of data (separated using one or more space characters). The columns are
chosen by their zero base indices. One or two columns can be selected, one for the real part of the
complex numners and one for the imaginary part. In case of only real numbers, then the imaginary
column index can be set to -1. The <direction> parameter can be either 'fwd' or 'rev', depending on
if a forward or reverse Fourier transform is to be calculated, respectively.

By invoking 'zeropad' the input dataset is zero padded to achieve a size 2^M, where M is an integer.

Output:
1. Re Im
2. <transformed real number> <transformed imaginary number>
3. <transformed real number> <transformed imaginary number>
            .
            .
            .
N+1. <transformed real number> <transformed imaginary number>
where N is the number of entries in the FFT transformed dataset.

````

### forcebetween

````text
Overview of commands of the form 'calculate forcebetween':

calculate forcebetween nonbonded <comma sep. list of atom IDs> <FF index (zero based)> [numerical]
calculate forcebetween coulomb <comma sep. list of atom IDs>
calculate forcebetween bond <comma sep. list of atom IDs> <FF index (zero based)> [numerical]
calculate forcebetween angle <comma sep. list of atom IDs> <FF index (zero based)> [numerical]
calculate forcebetween dihedral <comma sep. list of atom IDs> <FF index (zero based)> [numerical]

Calculates the nonbonded, Coulomb or bond force (in kJ/(mol Angstrom)) betwee two atoms, based on
the configured force fields, or the angular force between three atoms or the dihedral force
between four atoms. The forces are calculated based on the atoms shown in the current frame.
The indices of the various force fields that have been created or loaded is available through
the 'list' command. An index into these lists, for the appropriate force field type, is specified
through the <FF index (zero based)> parameter. The force is calculated between the atoms given
in <comma sep. list of atom IDs> (e.g., H, O, C7, where the list must be enetered withour spece).
By sepcifying 'numerical', the forces are calculated using numerical differentiation.

````

### hbondcount

````text
Overview of commands of the form 'calculate hbondcount':

calculate hbondcount <DCD filename> <frame from> <frame to> stride <stride> <M> <h-bond crit 1> ... <h-bond crit M> [pbcfromvisual] [searchtype <type>] [nopbc]
calculate hbondcount <DCD filename> <frame from> <frame to> <M> <h-bond crit 1> ... <h-bond crit M> [pbcfromvisual] [searchtype <type>] [nopbc]

Calculates hydrogen bond statistics from frames <frame from> to <frame to> within the DCD file specified
by <DCD filenmae>. <M> hydrogen bond criteria are specified in the <h-bond crit i> parameters. Each such
parameter is given as a comma separated list with no spaces in-between. There are 7 entries in the comma
separated lists
* the donor atom to consider (e.g., O, O4, C7)
* the hydrogen atom to consider
* the acceptor atom to consider
* the distance criteria between donor and hydrogen
* the distance criteria between hydrogen and acceptor
* the distance criteria between donor and acceptor
* the angle criteria, donor-hydrogen-acceptor
Both distance and angular criteria can be specified as '-', which means they are ignored. It is possible to
load every <stride> frame from the DCD file, which can be useful for large DCD files. By default, the periodic
boundary conditions (PBC) are taken from each frame in the DCD file. However, by specifying 'pbcfromvisual'
the PBC is collected from the one active in the 3D view. Using 'nopbc' ignores PBCs. By default, the bonds
are searched by considereing only intermolecular hydrogen bonds. By using the 'searchtype' parameter, it is
possible to specify
* <type>=intermolecular: only consider intermolecular bonds
* <type>=intramolecular: only consider intramolecular bonds
* <type>=allhbonds: consider all types of bonds

Output:
1. [1] D:<donor> H:<hydrogen> A:<acceptor> d-h:<dist.crit. donor-hydrogen> h-a:<dist.crit. hydrogen-acceptor> d-a:<dist.crit. donor-accepror> d-h...a:<angle crit>
2. [2] D:<donor> H:<hydrogen> A:<acceptor> d-h:<dist.crit. donor-hydrogen> h-a:<dist.crit. hydrogen-acceptor> d-a:<dist.crit. donor-accepror> d-h...a:<angle crit>
            .
            .
            .
N. [N] D:<donor> H:<hydrogen> A:<acceptor> d-h:<dist.crit. donor-hydrogen> h-a:<dist.crit. hydrogen-acceptor> d-a:<dist.crit. donor-accepror> d-h...a:<angle crit>
N+1. Index Tot.count Num.Donors Num.Accept. Num.Conn.Don. Num.Conn.Acc. Frame FirstDon. FirstHydr. FirstAcc
N+2. <index> <tot.count> <num. donors> <num. acceptors> <num h-bonds conn to donors> <num h-bonds conn to acceptors> <frame index> <first donor in frame> <first hydrogen in frame> <first acceptor in frame>
N+3. <index> <tot.count> <num. donors> <num. acceptors> <num h-bonds conn to donors> <num h-bonds conn to acceptors> <frame index> <first donor in frame> <first hydrogen in frame> <first acceptor in frame>
            .
            .
            .
N+K+1. <index> <tot.count> <num. donors> <num. acceptors> <num h-bonds conn to donors> <num h-bonds conn to acceptors> <frame index> <first donor in frame> <first hydrogen in frame> <first acceptor in frame>
where N is the number of hydrogen bond criteria and K is the number of analyzed frames.

````

### loading

````text
Overview of commands of the form 'calculate loading':

calculate loading <DCD filename> <frame from> <frame to> <atom IDs to find loading for> <lower vector - loading region> <upper vector - loading region>

Calculates the loading curve from <frame from> to <frame to> of the atoms specified in
<atom IDs to find loading for> within the DCD file given by <DCD filename>. For each
frame, which is one point on the curve, the number of atoms from the list of atom IDs
(comma separated list without spaces, e.g., H,O,C7) that are within the loadin region
are counted. The loading region is specified by a lower and upper vector, which both
are entered as three numbers, <x> <y> <z>, separated by a space.

Output:
1. <list of atoms for which loading is calculated>
2. <loading frame 1>
3. <loading frame 2>
      .
      .
      .
N+1. <loading frame N>
where N is the number of included frames.

````

### msd

````text
Overview of commands of the form 'calculate msd':

calculate msd <DCD filename> <frame from> <frame to> resname <resname> [usegeomcent] [numshiftsint0 <num shifts>]

Calculates the mean square displacement (MSD) of the molecules that has the resname <resname>
between frames <frame from> to <frame to>. By default, the MSD is calculated based on the
center of mass of the tracked molecules (which requires masses of the atoms to be loaded).
However, by using the 'usegeomcent' keyword, all masses are set to unity, thus resulting
in the geometric center of the molecules being used as basis for the MSD.

To enable better MSD averaging, the 'numshiftsint0' keyword kan be used to specify <num
shifts> number of shifts to apply to the starting frame, t0. Hence, if <num shifts>=0, no
shifts are applied, if <num shifts>=10, then the MSD is calculated 10 times but with the
starting frame being moved 0, 1, 2 and up to 10 steps. The average MSD is calculated between
all the shifted calculations.

Output:
1. Num. shifts in t0 = <num shifts>
2. From frame = <frame from>
3. To frame = <frame to>
4. Index MSD
5. <index> <MSD>
6. <index> <MSD>
          .
          .
          .
N+4. <index> <MSD>
where N is the number of points in the MSD curve.

````

### paircorrelation

````text
Overview of commands of the form 'calculate paircorrelation':

calculate paircorrelation <DCD filename> <frame from> <frame to> <atom ID 1> <atom ID 2> <num bins> <min. dist> <max. dist> [ignorepbc] [ignoredistbelow <dist>]

Calculates the pair correlation function, averaged over frames <frame from> to <frame to>,
taken from the DCD file <DCD filename>. Pair correlation is calculated between atoms with
ID <atom ID 1> and <atom ID 2> (e.g., H, O, C7). The pair correlation function is calculated
for distances between <min. dist> and <max. dist>, divided into <num bins>. By default periodic
boundary conditions (PBCs) are used, thus letting the distance measurements go across these
boundaries. To prevent distance measurements across PBCs, use the 'ignorepbc' keyword. If it
is desirable to only include distances above a distance criteria, <dist>, in the pair correlation
function, the 'ignoredistbelow' keyword can be applied.

Ouptut:
1. From frame: <frame from>
2. To frame: <frame to>
3. Pair: <atom ID1>-<atom ID2>
4. Range: [<min. dist>, <max. dist>]
5. Num. bins: <num bins>
6. Ignore dist. below: <dist>
7. Use PBC: yes/no
8.
9. Distance Count PairCorr RDF IdGas.RDF
10. <distance> <num. atoms in bin> <pair correlation value> <radial distribution function (RDF) value> <ideal gas RDF value>
11. <distance> <num. atoms in bin> <pair correlation value> <radial distribution function (RDF) value> <ideal gas RDF value>
        .
        .
        .
N+9. <distance> <num. atoms in bin> <pair correlation value> <radial distribution function (RDF) value> <ideal gas RDF value>
where N is the number of applied bins.

````

### potenergymap

````text
Overview of commands of the form 'calculate potenergymap':

calculate potenergymap <DCD filename> <frame from> <frame to> <atom IDs> <list of applied force fields> <Nx> <Ny> <cutting plane> <cutting plane pos>

Calculates a 2D potential energy map by utilizing a test particle of charge +1, averaged over
frames from <frame from> to <frame to> in the DCD file specified by <DCD filename>. Only the
atoms listed in <atom IDs> (e.g., H, O, C7) yield a contribution to the map. The <atom IDs>
list is to be comma separated without any spaces. The <list of applied forcefields> can be
* coulomb
This list is also to be comma separated, without any spaces. <Nx> and <Ny> defines the number
of bins to include in the 2D map. The 2D map is calculated for a <cutting plane> which can be
* XY
* YZ
* ZX
where the cutting plane position is <cutting plane pos>. If XY, then the cutting plane position
is along Z, if YZ, then it is along X and if ZX, then it is along Y.

Output:
1. PlanePrinted : <cutting plane>(<0 if XY, 1 if YZ and 2 if ZX)
2. Selection : <atom ID 1> <atom ID 2> ... <atom ID n>
3. Size : (<Nx>, <Ny>)
4. DensityData={{<point x>, <point y>, <pot. E>}, {<point x>, <point y>, <pot. E>}, ..., {<point x>, <point y>, <pot. E>}};
where <point x> and <point y> are points in the defined cutting plane.

````

### qbal

````text
Overview of commands of the form 'calculate qbal':

calculate qbal <group of atoms to modify>

Given a loaded dataset, this command will calculate scaling factors for the atoms within the given set of
atoms to modify. The <group of atoms to modify> parameter can be one of the following:
* sel - the group of atoms to modify is the current visual selection
The calculated scaling factors for the charges are such that, if applied to the corresponding charges, the
total charge of the loaded system is charge balanced. Moreover, the scaling factors are minimized, such that
they are as small as possible (given the usual L2-norm).

More detailed information about calculating the scaling factors is given in Refs. 1 and 2 (see below).
	
Output:
1. Charge 'Multiply with' 'Old charge' 'New charge' 'Change in %'
2. <charge type> <correction factor> <old charge value> <charge value after multiplication with correction factor> <change in % from old charge>
3. <charge type> <correction factor> <old charge value> <charge value after multiplication with correction factor> <change in % from old charge>
         .
         .
         .
N+1. <charge type> <correction factor> <old charge value> <charge value after multiplication with correction factor> <change in % from old charge>
where N is the number of charge types (e.g., H, O, C8) within the group of atoms to modify.


[1] Olsen, R. and Kvamme, B. (2019) ‘Effects of glycol on adsorption dynamics of idealized water droplets on LTA‐3A zeolite surfaces’, AIChE Journal, 65(5), p. e16567. doi: 10.1002/aic.16567.
[2] Olsen, R. et al. (2016) ‘Effects of Sodium Chloride on Acidic Nanoscale Pores Between Steel and Cement’, The Journal of Physical Chemistry C, 120(51), pp. 29264–29271. doi: 10.1021/acs.jpcc.6b10043.

````

### vacf

````text
Overview of commands of the form 'calculate vacf':

calculate vacf <DCD filenmae> <frame from> <frame to> <time step (fs)> <VACF length> name <atom IDs (comma sep., no space)>
calculate vacf <DCD filenmae> <frame from> <frame to> <time step (fs)> <VACF length> sel

Calculates the velocity auto correlation function (VACF) for the DCD file, <DCD filename>,
between the start frame, <frame from>, to the frame <frame to>. The time step, <time step>,
given in units of femtoseconds (fs), is used to obtain numerically calculated velocities
from pairs of atomic position originating from two adjacent time frames. The <VACF length>
parameter is the length of the VACF, in number of time steps, and must be smaller or equal
to D = <frame to> - <frame from>. If <VACF length> < D, then a VACF is calculated for each
starting time t0 from t0=<frame from> to t0=<frame to> (where indices outside the number
of frames are ignored). All collected VACF are then averaged. The VACF is only calculated
for a given selection of atoms. Either, based on the atom 'name', where a list, <atom IDs>,
is supplied (e.g., H, O, C7), or based on the visual selection of atoms, achieved through
the 'sel' keyword.

Output:
1. t vacf norm(vacf)
2. <VACF time point in fs> <VACF value> <VACF value, normalized to the first value>
3. <VACF time point in fs> <VACF value> <VACF value, normalized to the first value>
       .
       .
       .
N+1. <VACF time point in fs> <VACF value> <VACF value, normalized to the first value>
where N is the number of points selected in the VACF (<VACF length>).

````

### vdos

````text
Overview of commands of the form 'calculate vdos':

calculate vdos <DCD filename> <frame from> <num. bits> <time step (fs)> name <atom IDs (comma sep., no space)>
calculate vdos <DCD filename> <frame from> <num. bits> <time step (fs)> sel

Calculates the vibrational density of states (VDOS) for the DCD file, <DCD filename>,
between the start frame, <frame from>, to the frame <frame from>+2^<num. bits>. The time
step, <time step>, given in units of femtoseconds (fs), is used to obtain numerically
calculated velocities that are needed within the calculations. VDOS is only calculated for a
given selection of atoms. Either, based on the atom 'name', where a list, <atom IDs>,
is supplied (e.g., H, O, C7), or based on the visual selection of atoms, achieved through
the 'sel' keyword.

Output:
1. omegaomega[rad/fs] vdos
2. <angular frequency> <VDOS value (probability)>
3. <angular frequency> <VDOS value (probability)>
       .
       .
       .
N+1. <angular frequency> <VDOS value (probability)>
where N is the length of the VDOS function.

````

### volumefromdensity

````text
Overview of commands of the form 'calculate volumefromdensity':

calculate volumefromdensity <target density in kg/m^3> <atomic masses in g/mol> <num. molecules>

Given a target density in kg/m^3, a list of atomic masses (comma separated with no space) for
a single atom in the system, as well as the number of such molecules in the system, the required
volume in Angstrom^3 is calculated, in addition to the required side length required if the system
was cubic.

````

## gauss9

*Operations related to the Gaussian9 SW package*

````text
Usage: gauss9 dihedralrot id <atom index 1> <atom index 2> <atom index 3> <atom index41> rot <start angle> <end angle> <angular step size> qmspec <base set> <charge> <spin multiplicity> [relaxed]
       gauss9 dihedralrot var <variable name> rot <start angle> <end angle> <angular step size> qmspec <base set> <charge> <spin multiplicity> [relaxed]
       gauss9 anglerot id <atom index 1> <atom index 2> <atom index 3> rot <start angle> <end angle> <angular step size> qmspec <base set> <charge> <spin multiplicity> [relaxed]
       gauss9 anglerot var <variable name> rot <start angle> <end angle> <angular step size> qmspec <base set> <charge> <spin multiplicity> [relaxed]
       gauss9 bondstretch id <atom index 1> <atom index 2> stretch <start dist> <end dist> <dist step size> qmspec <base set> <charge> <spin multiplicity> [relaxed]
       gauss9 bondstretch var <variable name> stretch <start dist> <end dist> <dist step size> qmspec <base set> <charge> <spin multiplicity> [relaxed]
       gauss9 genxyzfrominput <gaussian input file name>
       gauss9 genoptefromoutput <gaussian output file name>
       gauss9 genoptxyzfromoutput <gaussian output file name>
       gauss9 genxyzfromoutput <gaussian output file name>

This command can execute several operations that are useful for the Gaussian9 software package. These are listed in the following:
* dihedralrot: will generate a Gaussian9 input script that executes electronic structure calculations on the loaded system
               for each rotation of a dihedral, either specified through 4 atomic indices or through the same indices stored
               in a variable.
* anglerot:    will generate a Gaussian9 input script that executes electronic structure calculations on the loaded system
               for each rotation of an angle, either specified through 3 atomic indices or through the same indices stored
               in a variable.
* bondstretch: will generate a Gaussian9 input script that executes electronic structure calculations on the loaded system
               for each atomic distance separation, either specified through 2 atomic indices or through the same indices
               stored in a variable.
* genxyzfrominput: Extracts the atom names and coordinates from a Gaussian9 input file and outputs a XYZ-file formatted text.
* genoptefromoutput: Extracts the optimization energies from a Gaussian9 output file and produces a list of these.
* genoptxyzfromoutput: Extracts the optimized structures from a Gaussian9 output file and produces XYZ-formatted text outputs.
* genxyzfromoutput: Extracts the input structure from a Gaussian9 output file and produces XYZ-formatted text outputs.
````

## lammps

*Operations related to the LAMMPS SW package*

````text
Usage: lammps genff
       lammps gendata [bondacrosspbc]

This command provides features that relate to version 5 Sep 2014 version of the LAMMPS MD
simulator, as well as other versions that support the same input / output structure.

The 'genff' sub-command will create a LAMMPS text that sets up the specified force field.
This should be piped to a file (i.e.,genff > <ff file>).

The 'gendata' sub-command will create a LAMMPS text that sets up all atomic positions, atomic IDs,
bond definitions, angle definitions, etc. This should be piped to a file (i.e.,gendata >
<data file>). To make sure bonds are created across PBCs, use the 'bondacrosspbc' keyword.

The generated input files can now be used as a basis for creating the final input files. The
<ff file> and <data file> should be used as input to the main input file for LAMMPS.
````

## hoomdblue

*Operations related to the HOOMD-blue SW package*

````text
Usage: hoomdblue genff
       hoomdblue gendata [bondacrosspbc]
       hoomdblue genrun <data file> <ff file> <temperature (K)> <time step (fs)> [enable12and13]
       hoomdblue atmtolj <value in atm units>
       hoomdblue fstolj <value in fs units>
       hoomdblue Ktolj <value in K units>

This command provides features that relate to version 1.3.1 of the HOOMD-blue MD simulator,
as well as other versions that support the same input / output structure. The 'atmtolj',
'fstolj' and 'Ktolj' sub-commands are used to convert values given in atm, fs and K,
respectiviely, to the Lennard-Jones units used by the sub-commands that generate inpu files
for HOOMD-blue.

The 'genff' sub-command will create a HOOMD-blue Python text that sets up the specified force
field. This should be piped to a Python file (i.e.,genff > <ff file>.py).

The 'gendata' sub-command will create an XML text that sets up all atomic positions, atomic IDs,
bond definitions, angle definitions, etc. This should be piped to a XML file (i.e.,gendata >
<data file>.xml). To make sure bonds are created across PBCs, use the 'bondacrosspbc' keyword.

Once 'genff' and 'gendata' has generated the <ff file>.py and <data file>.xml, the 'genrun'
sub-command can be applied to generate a Python run script for HOOMD-blue. This should be piped
to a Python file (i.e., genrun <data file> <ff file> <temp> <step> > run.py). To enable 1-2 and
1-3 interactions in HOOMD-blue, use the 'enable12and13' keyword.

The generated input files can now be used as a basis for creating the final input files.
````

## mmol

*Apply operations to *.mmol type files*

````text
Usage: mmol toscript [noself] [mixgeom] <molfile 1> <molfile 2> ... <molfile N> > <script file to generate>

This command imports a series of *.mmol files, which are molecular definition
files defined within the MDynaMix MD software package, and converts them to a
series of MolTwister exec(), script instructions that will set up the force
fields defined within the *.mmol files. The list of input molfiles (*.mmol)
is terminated by the pipe symbol, '>'.

Self interactions can be exculded in the MolTwister instructions by appyling the
'noself' keyword. By default, mixing of short range interaction parameters is done
by using arithmetic mixing rules. However, by employing the 'mixgeom' keyword,
geometric mixing rules will be applied.
````

## dcd

*Perform a DCD file operation*

````text
Overview of commands of the form 'dcd <sub command>':

dcd atomicunwrap <DCD filename> [atomnames <atom ID list>] [pbcfromvisual]
dcd header <DCD filename>
dcd numcoordinates <DCD filename> <record index>
dcd numrecords <DCD filename>
dcd readcoordinate <DCD filename> <record index> <coordinate index>
dcd readrecord <DCD filename> <record index>
dcd unwrap <DCD filename> [pbcfromvisual]
dcd wrap <DCD filename> [pbcfromvisual]

To get more information about a <sub command>, type 'help dcd <sub command>

````

### atomicunwrap

````text
Overview of commands of the form 'dcd atomicunwrap':

dcd atomicunwrap <DCD filename> [atomnames <atom ID list>] [pbcfromvisual]

Loads a DCD file, <DCD filename>, and performs an atomic unwrap across the
periodic boundaries (i.e., PBC). The PBC is taken from the DCD file by default.
However, by specifying the 'pbcfromvisual' keyword, the PBC can be taken from
the applied visual PBC. It is also possible to select a subsection of the atoms
stored in the original DCD file by applying the 'atomnames' keyword with a list
of atom IDs (e.g., H, O, C7), <atom ID list>, which is comma separated with no
space. The output is a DCD file with the name
* <DCD filename (without extension)>_mtatunwrap.dcd

Note that an 'unwrap' will move the atomic coordinates across the PBC if it
is discovered that the coordinate crosses it between two time steps (thus leading
to a complete unfolding of the PBC. However, an 'atomic unwrap' will unwrap
frame-by-frame, based on the molecular definitions. Hence, only molecules that
wrap across the PBC will be unwrapped so as to be un-divided, but outside the
PBC. Hence. an atomic unwrap requires only one frame, while unwrap needs more.

````

### header

````text
Overview of commands of the form 'dcd header':

dcd header <DCD filename>

Prints the DCD file header from <DCD filename>.

Output:
ID = <ID>
NSets = <number of sets>
InitStep = <init step>
WrtFreq = <Write frequency>
TimeStep = <time step>
DescriptA = <descript A>
DescriptB = <descript B>
NAtoms = <number of atoms>

````

### numcoordinates

````text
Overview of commands of the form 'dcd numcoordinates':

dcd numcoordinates <DCD filename> <record index>

Prints the number of coordinates in record <record index>
of the DCD file, <DCD filename>.

Output:
Coordinate count = <coordinate count>

````

### numrecords

````text
Overview of commands of the form 'dcd numrecords':

dcd numrecords <DCD filename>

Prints the number of recoords within the DCD file, <DCD filename>.

Output:
Num records = <record count>

````

### readcoordinate

````text
Overview of commands of the form 'dcd readcoordinate':

dcd readcoordinate <DCD filename> <record index> <coordinate index>

Prints the coordinate at record index <record index>, coordinate index
<coordinate index> within the DCD file, <DCD filename>.

Output:
Coordinate = (<x>, <y>, <z>)

````

### readrecord

````text
Overview of commands of the form 'dcd readrecord':

dcd readrecord <DCD filename> <record index>

Prints all the coordinates at record index <record index>
within the DCD file, <DCD filename>.

Output:
{{<x1>, <y1>, <z1>}, {<x2>, <y2>, <z2>}, ..., {<xN>, <yN>, <zN>}}
where N is the number of coordinates within the given record.

````

### unwrap

````text
Overview of commands of the form 'dcd unwrap':

dcd unwrap <DCD filename> [pbcfromvisual]

Loads a DCD file, <DCD filename>, and performs an unwrap across the periodic
boundaries (i.e., PBC). The PBC is taken from the DCD file by default. However,
by specifying the 'pbcfromvisual' keyword, the PBC can be taken from the applied
visual PBC. The output is a DCD file with the name
* <DCD filename (without extension)>_mtunwrap.dcd

Note that an 'unwrap' will move the atomic coordinates across the PBC if it
is discovered that the coordinate crosses it between two time steps (thus leading
to a complete unfolding of the PBC. However, an 'atomic unwrap' will unwrap
frame-by-frame, based on the molecular definitions. Hence, only molecules that
wrap across the PBC will be unwrapped so as to be un-divided, but outside the
PBC. Hence. an atomic unwrap requires only one frame, while unwrap needs more.

````

### wrap

````text
Overview of commands of the form 'dcd wrap':

dcd wrap <DCD filename> [pbcfromvisual]

Loads a DCD file, <DCD filename>, and performs a wrap across the periodic boundaries
(i.e., PBC). The PBC is taken from the DCD file by default. However, by specifying
the 'pbcfromvisual' keyword, the PBC can be taken from the applied visual PBC.
The output is a DCD file with the name
* <DCD filename (without extension)>_mtwrap.dcd

````

## mtpython

*Runs Python script input, with MolTwister spesific functions*

````text
Usage: mtpython {<Python line of code>}

Any Python line of code can be executed, one by one. For example, the sequence:

mtpython {a = 5}
mtpython {b = 20}
mtpython {print("Output: %f" % (a+b))}

would produce the output 'Output: 25.000000'. Note that Python 'import' will load
the specified library and will be available for the next use of 'mtpython'.
	
By importing the 'moltwister' library (e.g., import moltwister as mt), several
Python functions will be made available that can query / manipulate the state of
MolTwister. These are as follows

	mt_exec(<moltwister command>) : result as string
	Executes a moltwister command. For example, mt_exec("list all"), which
	will return result of 'list all'.

	mt_get_num_atoms() : result as int
	Returns the number of atoms presently loaded or added.

	mt_get_atom_pos(atomIndex:integer, axisIndex:integer) : result as integer
	Returns the atom position of a given atom index and a given axis (0=x, 1=y, 2=z).

	mt_get_atom_type(atomIndex:integer) : result as string
	Returns the atom type of the atom at the given atom index.

	mt_get_atom_mass(atomIndex:integer) : result as integer
	Returns the assigned atomic mass of the atom at the given atom index.

	mt_get_atom_charge(atomIndex:integer) : result as integer
	Returns the assigned atomic charge of the atom at the given atom index.

	mt_get_atom_resname(atomIndex:integer) : result as string
	Returns the assigned resname of the atom at the given atom index.

	mt_get_atom_molindex(atomIndex:integer) : result as integer
	Returns the assigned molecular index of the atom at the given atom index.

	mt_is_atom_sel(atomIndex:integer) : result as boolean
	Returns true if the atom at the given atom index is selected, else it returns false.

	mt_begin_progress(progBarDescription:string) : no result
	Shows initial progress bar (in the command line shell) with given text.

	mt_update_progress(step:integer, totalSteps:integer) : no result
	Updates progress bar according to given step information.

	mt_end_progress() : no result
	Finishes progress bar, showing as 100 percent complete.
	
An example sequence could be:

mtpython {import moltwister as mt}
mtpython {print("Num atoms: %f" % mt.mt_get_num_atoms())}
mtpython {print("%s" % mt.mt_exec("list all"))}

Note that all the above examples are written in the language of Python version 3.0
and may not be compatible with earlier versions of Python.
	
It is also possible to write a python script and then load this script using
the 'load python <name of script>' command, where only the <Python line of code>
parts of the mtpython commands are included within the script.
````

## moldyn

*Invoke a molecular dynamics simulation*

````text
Overview of commands of the form 'moldyn <sub command>':

moldyn run [cpu]
moldyn ff bondforceprofile <ff index> <profile start r> <profile end f> <num points in profile>
moldyn ff angleforceprofile <ff index> <profile start theta> <profile end theta> <num points in profile>
moldyn ff dihedralforceprofile <ff index> <profile start theta> <profile end theta> <num points in profile>
moldyn ff nonbondforceprofile <ff index> <profile start r> <profile end f> <num points in profile>
moldyn cfg get
moldyn cfg set <parameter> <value>

To get more information about a <sub command>, type 'help moldyn <sub command>

````

### run

````text
Overview of commands of the form 'moldyn run':

moldyn run [cpu]

This command will run a molecular dynamics (MD) simulation. If the 'cpu' keyword is included, the
simulation will be forced to be executed on the CPU. If not, an attempt will be made to run the
simulation on GPU. If this does not succeed (e.g., if the software is not compiled to run on GPU)
the simulation will be executed on CPU.

````

### ff

````text
Overview of commands of the form 'moldyn ff':

moldyn ff bondforceprofile <ff index> <profile start r> <profile end f> <num points in profile>
moldyn ff angleforceprofile <ff index> <profile start theta> <profile end theta> <num points in profile>
moldyn ff dihedralforceprofile <ff index> <profile start theta> <profile end theta> <num points in profile>
moldyn ff nonbondforceprofile <ff index> <profile start r> <profile end f> <num points in profile>

The molecular dynamics algorithm will first create a list of points based on the force-field
potential expressions. This list of points will be interpolated during the simulations to
calculate the forces that need to be appiled to each atom in the system.
	
This command will output the list of points that make up both the potential (in kJ/mol) and
the force (in kJ/(mol*AA)). Depending on the type of potential we query (bond, angle, dihedral
or non-boned), the start and end points to output must be specified, together with the desired
number of points. The units for bond and non-bonded profiles are in Angstrom, while for angle
and dihedral profiles they are in degrees. The force-field index, <ff index>, can be found
through the 'list ff' command.

````

### cfg

````text
Overview of commands of the form 'moldyn cfg':

moldyn cfg get
moldyn cfg set <parameter> <value>

This command can be used both to set and get configurations for the molecular
dynamics (MD) simulator. If 'get' is called, the available parameters are listed
together with their current values. The first word in each listed line (followed
by '=') is the value to be handed to <parameter> when calling 'set'. In the list
obtained from 'get' it has also been made clear how <value> for the corresponding
parameter should be formatted.

````

