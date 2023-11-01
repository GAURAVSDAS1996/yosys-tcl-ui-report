<!---
![Yosys_TCL_UI](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/970d192f-4eee-40b2-b285-51ce1c663677)
![Yosys_TCL_UI (3)](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/4c91c984-fd74-47c7-a599-72e7f7f30594)
![Yosys_TCL_UI (2)](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/d2b2ddff-35e9-450a-b28a-8f21ae766c4d)
-->
![Yosys_TCL_UI (4)](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/ee23feab-44d4-4d7f-b5d9-fdf120a4a684)
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

![Screenshot (264)](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/dcf3a9f9-2281-4d6f-b318-a52ddea1fb7d)
![Screenshot (265)](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/6194ce14-1cf5-41c2-9de3-6938f205f912)

## Requirements

* Linux OS
* Yosys Synthesis Suite
* OpenTimer STA Tool
* TCL Matrix Package

## Usage

* Clone the repo, and in the directory in the bash terminal, run `./yosysui openMSP430_design_details.csv` and then design '*openMSP430*. Yosys synthesis and OpenTimer STA will be run, and at the end you will receive '*PRELAYOUT TIMING RESULTS*' as illustrated in the above images. (Before running, make sure to edit the procs paths in the script and the OpenTimer tool path, as well as your local paths)
* You can also type in `./yosysui -help`, wherein the command gives you a usage guide to help explore its functionalities.

***Note: The screenshots in the following sections are purely based on the 'yosysui.tcl' script that I have uploaded and are not a direct output of the code snippet in the corresponding section. The code snippets contain only the crucial portions of the 'yosysui.tcl' script required to execute the tasks mentioned in the respective sections.***

## Day 1 - Introduction to TCL and VSDSYNTH Toolbox Usage (23/08/2023)

Day 1's task is to create a command (in my case, ***yosysui***) and pass a .csv file from the UNIX shell to the TCL script, taking into consideration mainly three general scenarios from the user's point of view.

![Screenshot 2023-08-24 183526](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/a1c31fb3-a8e5-4a7e-987d-3d7e6ff4ad65)

**Review of input files provided in work directory**

![Screenshot from 2023-08-23 22-48-19](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/7dd31089-d1a4-44a2-9d31-f828af25e37c)

**Review of input file - openMSP430_design_details.csv**

![Screenshot from 2023-08-23 22-29-25](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/de69f8ea-92cc-40e6-b570-e7a364c72c04)

### Implementation

Creation of the *yosysui* command script and *yosysui.tcl* files.

![Screenshot from 2023-08-24 19-28-08](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/53d902f7-9ec4-4a8f-a2cd-683f74a7ca2f)

*yosysui.tcl*

![Screenshot from 2023-08-24 19-36-52](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/c0e5a04d-2d6b-40ea-bcd1-9bd76be60116)

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
	tclsh yosysui.tcl $1
fi
```

In my command ***yosysui***, I have implemented a total of *5 general scenarios* from the user's point of view in the bash script.

#### 1. No input file provided

![Screenshot from 2023-08-24 19-39-50](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/cb679d28-e0de-448a-ad2a-5d31031d2f8b)

#### 2. File provided exists but is not of .csv format

![Screenshot from 2023-08-24 19-41-41](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/45653ab6-96be-49f1-8af2-afcce1ee0392)

#### 3. More than one file or parameters provided

![Screenshot from 2023-08-24 19-53-06](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/8ad5345c-1fa8-4432-b618-b5a0bd5f3934)

#### 4. Provide a .csv file that does not exist

![Screenshot from 2023-08-24 19-54-26](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/96ee26cd-43ed-4056-a1ac-c42a5207542a)

#### 5. Type "-help" to find out usage

![Screenshot from 2023-08-24 19-55-32](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/86488924-9fe5-4702-a41d-3916ac7044de)
