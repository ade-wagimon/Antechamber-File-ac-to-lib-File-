# Converting ac (Antechamber File) to lib File

In this tutorial, we will convert the `.ac` file into a `.lib` file.

To generate the `.ac` file, we will first use the RESP approach. Then, we will use the `prepgen` program from AMBER to generate the `.prepi` file. Additionally, we will manually prepare the main chain file, which identifies the backbone atoms of the molecule.

<img src="https://github.com/ade-wagimon/Antechamber-File-ac-to-lib-File-/blob/main/figure/14L_resp.png" width=80% height=80%>



The main chain file should contain the following information:

- **MAIN_CHAIN:** Names of the backbone atoms
- **HEAD_NAME:** Names of the first terminal atoms
- **TAIL_NAME:** Name of the end terminal atom
- **OMIT_NAME:** Name of the atom to be removed from the final structure
- **PRE_HEAD_TYPE:** Atom type connecting to the first terminal atom
- **POST_TAIL_TYPE:** Atom type connecting to the end terminal atom
- **CHARGE:** Net charge of the molecule

To generate the `.prepi` file, use the command:

```
prepgen -i 14l.ac -o 14l.prep -rn 14L -m 14l_mc.dat
```

Next, create the `.frcmod` file to verify the bond parameters for the molecule using these commands:

```
parmchk2 -i 14l.prep -f prepi -o 14l.frcmod -a Y -p /home/cadd/amber20_src/dat/leap/parm/gaff.dat
parmchk2 -i 14l.prep -f prepi -o 14l.frcmod2
grep -v "ATTN" 14l.frcmod > 14l.frcmod1
```

Finally, create the `.lib` file using the Leap program by loading the necessary files:

```
source leaprc.gaff
loadAmberParams 14l.frcmod1
loadAmberParams 14l.frcmod2
loadamberprep 14l.prep
set 14L head 14L.1.C12
saveoff 14L 14L.lib
quit
```

Final Output: `14L.lib`
