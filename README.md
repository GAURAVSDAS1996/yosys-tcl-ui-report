<!---
![Yosys_TCL_UI](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/970d192f-4eee-40b2-b285-51ce1c663677)
![Yosys_TCL_UI (3)](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/4c91c984-fd74-47c7-a599-72e7f7f30594)
![Yosys_TCL_UI (2)](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/d2b2ddff-35e9-450a-b28a-8f21ae766c4d)
-->
![YOSYS TCL UI](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/6e91fc1d-5630-4f9b-a0ad-b7619b486d1a)

# YOSYS TCL UI REPORT

![Static Badge](https://img.shields.io/badge/OS-linux-orange)
![Static Badge](https://img.shields.io/badge/EDA%20Tools-Yosys%2C_OpenTimer-navy)
![Static Badge](https://img.shields.io/badge/languages-verilog%2C_bash%2C_TCL-crimson)
![GitHub last commit](https://img.shields.io/github/last-commit/fayizferosh/yosys-tcl-ui-report)
![GitHub language count](https://img.shields.io/github/languages/count/fayizferosh/yosys-tcl-ui-report)
![GitHub top language](https://img.shields.io/github/languages/top/fayizferosh/yosys-tcl-ui-report)
![GitHub repo size](https://img.shields.io/github/repo-size/fayizferosh/yosys-tcl-ui-report)
![GitHub code size in bytes](https://img.shields.io/github/languages/code-size/fayizferosh/yosys-tcl-ui-report)
![GitHub repo file count (file type)](https://img.shields.io/github/directory-file-count/fayizferosh/yosys-tcl-ui-report)
<!---
Comments
-->

> 5 day TCL training workshop by VSD using Yosys and Opentimer open-source EDA tools and TCL to generate a report from a design, wherein the input is design file paths in .csv format to the TCL program. The final objective by day 5 is to give design details, namely paths of design data, to the "TCL BOX", which is the UI being designed, which runs the design in Yosys and Opentimer open-source EDA tools and returns a report of the design.

![sc1](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/5296fc27-8416-4ec0-9f68-e6cd2d4fe088)
![sc2](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/29525e33-1ed0-4d4e-b498-456a40f5249a)

## Requirements

* Linux OS
* Yosys Synthesis Suite
* OpenTimer STA Tool
* TCL Matrix Package

## Usage

* Clone the repo, and in the directory in the bash terminal, run `./gdas_synth openMSP430_design_details.csv` and then design '*openMSP430*. Yosys synthesis and OpenTimer STA will be run, and at the end you will receive '*PRELAYOUT TIMING RESULTS*' as illustrated in the above images. (Before running, make sure to edit the procs paths in the script and the OpenTimer tool path, as well as your local paths)
* You can also type in `./gdas_synth -help`, wherein the command gives you a usage guide to help explore its functionalities.

***Note: The screenshots in the following sections are purely based on the 'gdas_synth.tcl' script that I have uploaded and are not a direct output of the code snippet in the corresponding section. The code snippets contain only the crucial portions of the 'gdas_synth.tcl' script required to execute the tasks mentioned in the respective sections.***

## Day 1 - Introduction to TCL and VSDSYNTH Toolbox Usage (23/08/2023)

Day 1's task is to create a command (in my case, ***yosysui***) and pass a .csv file from the UNIX shell to the TCL script, taking into consideration mainly three general scenarios from the user's point of view.

![Screenshot 2023-08-24 183526](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/a1c31fb3-a8e5-4a7e-987d-3d7e6ff4ad65)

**Review of input files provided in work directory**

![Screenshot from 2023-08-23 22-48-19](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/7dd31089-d1a4-44a2-9d31-f828af25e37c)

**Review of input file - openMSP430_design_details.csv**

![Screenshot from 2023-08-23 22-29-25](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/de69f8ea-92cc-40e6-b570-e7a364c72c04)

### Implementation

Creation of the *gdas_synth* command script and *gdas_synth.tcl* files.

![sc10](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/e170330d-410a-422a-9606-99fb712752d8)

*gdas_synth.tcl*

![sc4](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/cf60b6bb-e75e-4611-90bc-1ac205648c2c)

The basic structure of bash code used for the implementation of general scenarios is shown below.

```bash
# Tool Initialisation
if [ $# -eq 0 ]; then
	echo "Info: Please provide the csv file"
	exit 1
elif [ $# -gt 1 ] && [ $1 != *.csv ]; then
	echo "Info: Please provide only one csv file"
	exit 1
else
	if [[ $1 != *.csv ]] && [ $1 != "-help" ]; then
		echo "Info: Please provide a .csv format file"
		exit 1
	fi
fi

if [ ! -e $1 ] || [ $1 == "-help" ]; then
	if [ $1 != "-help" ]; then
		echo "Error: Cannot find csv file $1. Exiting..."
		exit 1
	else
		echo "USAGE:  ./yosysui <csv file>"
		echo
		echo "        where <csv file> consists of 2 columns, below keyword being in 1st column and is Case Sensitive. Please request Fayiz for sample csv file."
		echo
		echo "        <Design Name> is the name of top level module."
		echo
		echo "        <Output Directory> is the name of output directory where you want to dump synthesis script, synthesized netlist and timing reports."
		echo
		echo "        <Netlist Directory> is the name of directory where all RTL netlist are present."
		echo
		echo "        <Early Library Path> is the file path of the early cell library to be used for STA."
		echo
		echo "        <Late Library Path> is file path of the late cell library to be used for STA."
		echo
		echo "        <Constraints file> is csv file path of constraints to be used for STA."
		echo
	fi
else
	echo "Info: csv file $1 accepted"
	tclsh gdas_synth.tcl $1
fi
```

In my command ***gdas_synth***, I have implemented a total of *5 general scenarios* from the user's point of view in the bash script.

#### 1. No input file provided

![sc5](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/c69be564-40e8-4e5c-956b-76a9a4417443)

#### 2. File provided exists but is not of .csv format

![sc6](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/861c3eb7-b475-4f2f-b987-34c5a4be727b)

#### 3. More than one file or parameters provided

![sc7](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/e44fc301-ee41-4ae3-bc0e-0e1e00471487)

#### 4. Provide a .csv file that does not exist

![sc8](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/afb87688-c31d-478b-adf0-db3636a7b868)


#### 5. Type "-help" to find out usage

![sc9](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/7a534182-1a26-4868-a264-3be4bf0d7727)
