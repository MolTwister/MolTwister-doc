# About MolTwister

MolTwister is an open source software package. It can aid in the construction of 
molecular systems used as input to molecular dynamics simulations. However, its
primary purpose is to provide a collection of analysis tools for postprocessing
of trajectories and data from molecular dynamics simulations. This includes
calculation of density profiles, velocity auto correlation functions, radial
distribution functions, dihedral distributions and more.

The software runs in a command line window and will act as an extension to the
ordinary command line window, running in the background. Hence, an additional 
set of commands are available for creating and analysing molecular systems from
the command line. Commands not recognized by MolTwister is sent for processing
as a regular command (e.g., vim can be executed directly from within MolTwister).
In addition to the extended command line, a 3D View window will display the
molecular system being edited or created.

The command structure in MolTwister follows a simple structure. In the source
code, each command is implemented as its own class that inherits from a base
command class. A similar structure is also possible with sub-commands. From
within the command class, access is given to the entrire state of the molecular
system, including atomic positions, masses and force-field parameters. Hence,
to implement a new command, copy a similar command class to the one that is
to be created, rename it, register it in the command pool and implement its
behavior. The class also contains the command documentation, thus providing
a complete command implementation.
