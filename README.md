# Converting ac (Antechamber File) to lib File

In this tutorial we are going to convert the `ac file` into a `lib file`. 

To generate the `ac file`, we will first need to use the `RESP` approach. Then, we will use the `prepgen` program from AMBER to generate the `prepi file`. In this step, we will also need to prepare the `main chain file` manually, which identifies the main atoms or backbones of a molecule.

The `main chain file` should include the following information:

    **MAIN_CHAIN:** the names of the backbone atoms
    **HEAD_NAME:** the names of the first terminal atoms
    **TAIL_NAME:** the name of the end terminal atom
    **OMIT_NAME:** the name of the atom that we want to remove from the final structure
    **PRE_HEAD_TYPE:** the atom type that connects to the first terminal atom
    **POST_TAIL_TYPE:** the atom type that connects to the end terminal atom
    **CHARGE:** the net charge of the molecule

To generate the prepi file, we can use the following command:

    prepgen -i 14l.ac -o 14l.prep -rn 14L -m 14l_mc.dat
        

Next, we can create the frcmod file to check the bond parameters for the molecule using the following commands:
  
    parmchk2 -i 14l.prep -f prepi -o 14l.frcmod -a Y -p /home/cadd/amber20_src/dat/leap/parm/gaff.dat
    parmchk2 -i 14l.prep -f prepi -o 14l.frcmod2
    grep -v "ATTN" eg.frcmod > eg.frcmod1
    
Finally, we can create the lib file using the Leap program by loading the files that we have created:

    source leaprc.gaff
    loadAmberParams 14l.frcmod1
    loadAmberParams 14l.frcmod2
    loadamberprep 14l.prep
    set 14L head 14L.1.C12
    saveoff 14L 14L.lib
    quit
    
  
**Final Output: 14L.lib**
