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

> A 5 day TCL training workshop by VSD using open-source EDA tools Yosys and Opentimer and using TCL script to generate a report from a design, wherein the input is design file paths in .csv format to the TCL program. The ultimate objective by final day is to give design details, namely paths of design data, to the "TCL BOX", which is the UI being designed, which runs the design in Yosys and Opentimer open-source EDA tools and generates report of the design.

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

## Day 1 - Introduction to TCL and VSDSYNTH Toolbox Usage (01/11/2023)

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
		echo "USAGE:  ./gdas_synth <csv file>"
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

## Day 2 - Variable Creation and Processing Constraints from CSV (02/11/2023)

Day 2's task is to basically write the TCL code in *gdas_synth.tcl* for variable creation, file/directory existence check, and the processing of the constraints csv file to convert it into format[1] (which is the constraints format taken as input by Yosys tool) as well as into SDC (Synopsys Design Constraints) format (which is the industry standard format).

![sc14](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/48527a0e-f656-4542-b60f-9461715de44a)


**Review of input file - openMSP430_design_constraints.csv**

![sc15](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/5ca439d2-79c8-4786-bf54-33e5b3ad4a96)

### Implementation

I have successfully completed Day 2 tasks, namely variable creation, file and directory existence checks, and the processing of the constraints csv file.

**gdas_synth.tcl snapshot**

![fp1](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/c4877493-9358-4e8d-9cc8-f0a6d4ef2af8)

#### Variable Creation

I have auto-created the variables (*have used special condition to identify design name*) from the csv file by converting it into a matrix and then to an array (*also added command to capture the start time of the script so that it can be used to calculate runtime at the end*). The basic code of the same and a screenshot of the terminal with several "puts" printing out the variables are shown below.

*Code*

```tcl
# Capturing start time of the script
set start_time [clock clicks -microseconds]

# Variable Creation
# -----------------
# Setting CLI argument to variable where argv is TCL builtin variable containing CLI arguments as list
set dcsv [lindex $argv 0]

# csv file ti matrix processing package
package require csv
package require struct::matrix

# Initialisation of a matrix "m"
struct::matrix m

# Opening design details csv to file handler "f"
set f [open $dcsv]

# Parsing csv data to matrix "m"
csv::read2matrix $f m , auto

# Closing design details csv
close $f

# Storing number of rows and columns of matrix to variables
set ncdcsv [m columns]
set nrdcsv [m rows]

# Convertion of matrix to array "des_arr(column,row)"
m link des_arr

# Auto variable creation and data assignment
set i 0
while {$i < $nrdcsv} {
	puts "\nInfo: Setting $des_arr(0,$i) as '$des_arr(1,$i)'"
	if { ![string match "*/*" $des_arr(1,$i)] && ![string match "*.*" $des_arr(1,$i)] } {
		set [string map {" " "_"} $des_arr(0,$i)] $des_arr(1,$i)
	} else {
		set [string map {" " "_"} $des_arr(0,$i)] [file normalize $des_arr(1,$i)]
	}
	set i [expr {$i+1}]
}
```

*Screenshot*

![sc11](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/af239a76-6578-45ce-b23c-2a6721561dc4)

#### File / Directory Existence Check

I have written the code to check the existence of all files and directories wherein the program exits in case they are not found since their existence is crucial for the program to move further except for the output directory, which is created if it does not exist. The basic code of the same and screenshots of the terminal demonstrating the functionality, namely one showing the creation of a new output directory and another in which an output directory exists but a constraints file does not exist, are shown below.

*Code*

```tcl
# File/Directory existence check
# ------------------------------
# Checking if output directory exists if not creates one
if { ![file isdirectory $Output_Directory] } {
	puts "\nInfo: Cannot find output directory $Output_Directory. Creating $Output_Directory"
	file mkdir $Output_Directory 
} else {
	puts "\nInfo: Output directory found in path $Output_Directory"
}

# Checking if netlist directory exists if not exits
if { ![file isdirectory $Netlist_Directory] } {
	puts "\nError: Cannot find RTL netlist directory in path $Netlist_Directory. Exiting..."
	exit
} else {
	puts "\nInfo: RTL netlist directory found in path $Netlist_Directory"
}

# Checking if early cell library file exists if not exits
if { ![file exists $Early_Library_Path] } {
	puts "\nError: Cannot find early cell library in path $Early_Library_Path. Exiting..."
	exit
} else {
	puts "\nInfo: Early cell library found in path $Early_Library_Path"
}

# Checking if late cell library file exists if not exits
if { ![file exists $Late_Library_Path] } {
	puts "\nError: Cannot find late cell library in path $Late_Library_Path. Exiting..."
	exit
} else {
	puts "\nInfo: Late cell library found in path $Late_Library_Path"
}

# Checking if constraints file exists if not exits
if { ![file exists $Constraints_File] } {
	puts "\nError: Cannot find constraints file in path $Constraints_File. Exiting..."
	exit
} else {
	puts "\nInfo: Constraints file found in path $Constraints_File"
}
```

*Screenshots*

![sc13](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/79689396-6c17-4b2e-b027-73c64d273010)
![sc12](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/4f67438a-2ccf-4a2f-a59c-9c1b4d04d0e3)

#### Processing of the constraints openMSP430_design_constraints.csv file

The file was successfully processed and converted into a matrix, and the rows and columns count were extracted, as well as the starting rows of clocks, inputs, and outputs. The basic code of the same and a screenshot of the terminal with several "puts" printing out the variables are shown below.

*Code*

```tcl
# Constraints csv file data processing for convertion to format[1] and SDC
# ------------------------------------------------------------------------
puts "\nInfo: Dumping SDC constraints for $Design_Name"
::struct::matrix m1
set f1 [open $Constraints_File]
csv::read2matrix $f1 m1 , auto
close $f1
set nrconcsv [m1 rows]
set ncconcsv [m1 columns]
# Finding row number starting for CLOCKS section
set clocks_start [lindex [lindex [m1 search all CLOCKS] 0] 1]
# Finding column number starting for CLOCKS section
set clocks_start_column [lindex [lindex [m1 search all CLOCKS] 0] 0]
# Finding row number starting for INPUTS section
set inputs_start [lindex [lindex [m1 search all INPUTS] 0] 1]
# Finding row number starting for OUTPUTS section
set outputs_start [lindex [lindex [m1 search all OUTPUTS] 0] 1]
```

*Screenshot*

<!---
![Screenshot from 2023-08-26 00-09-40](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/7114a964-971f-46ee-b6a8-831ea83c0145)
-->
![sc17](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/cd2cc548-1ac1-41c1-8835-7b6e98785e88)

## Day 3 - Processing Clock and Input Constraints from CSV and dumping SDC (04/11/2023)

Day 3's task is to basically process constraints in a csv file for clocks and inputs and dump SDC commands into a .sdc file with actual processed data. It involves several matrix search algorithms and also an algorithm to identify inputs that are buses and bits differently.

**Review of input file - openMSP430_design_constraints.csv**

![sc18](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/e9b24ee4-275b-423e-84bd-de85b5de8313)


### Implementation

I have successfully completed Day 3 tasks, namely processing constraints in a csv file for clocks and inputs and dumping SDC commands into a .sdc file with actual processed data.

#### Processing of the constraints .csv file for CLOCKS and dumping SDC commands to .sdc

I have successfully processed the csv file for CLOCKS data and dumped clock-based SDC commands (*with unique clock names by adding "_yui" to the SDC create_clock command*) to .sdc file. The basic code of the same and screenshots of the terminal with several "puts" printing out the variables and user debug information as well as output .sdc are shown below.

*Code*

```tcl
# Convertion of constraints csv file to SDC
# -----------------------------------------
# CLOCKS section
# Finding column number starting for clock latency in CLOCKS section only
set clocks_erd_start_column [lindex [lindex [m1 search rect $clocks_start_column $clocks_start [expr {$ncconcsv-1}] [expr {$inputs_start-1}] early_rise_delay] 0 ] 0 ]
set clocks_efd_start_column [lindex [lindex [m1 search rect $clocks_start_column $clocks_start [expr {$ncconcsv-1}] [expr {$inputs_start-1}] early_fall_delay] 0 ] 0 ]
set clocks_lrd_start_column [lindex [lindex [m1 search rect $clocks_start_column $clocks_start [expr {$ncconcsv-1}] [expr {$inputs_start-1}] late_rise_delay] 0 ] 0 ]
set clocks_lfd_start_column [lindex [lindex [m1 search rect $clocks_start_column $clocks_start [expr {$ncconcsv-1}] [expr {$inputs_start-1}] late_fall_delay] 0 ] 0 ]

# Finding column number starting for clock transition in CLOCKS section only
set clocks_ers_start_column [lindex [lindex [m1 search rect $clocks_start_column $clocks_start [expr {$ncconcsv-1}] [expr {$inputs_start-1}] early_rise_slew] 0 ] 0 ]
set clocks_efs_start_column [lindex [lindex [m1 search rect $clocks_start_column $clocks_start [expr {$ncconcsv-1}] [expr {$inputs_start-1}] early_fall_slew] 0 ] 0 ]
set clocks_lrs_start_column [lindex [lindex [m1 search rect $clocks_start_column $clocks_start [expr {$ncconcsv-1}] [expr {$inputs_start-1}] late_rise_slew] 0 ] 0 ]
set clocks_lfs_start_column [lindex [lindex [m1 search rect $clocks_start_column $clocks_start [expr {$ncconcsv-1}] [expr {$inputs_start-1}] late_fall_slew] 0 ] 0 ]

# Finding column number starting for frequency and duty cycle in CLOCKS section only
set clocks_freq_start_column [lindex [lindex [m1 search rect $clocks_start_column $clocks_start [expr {$ncconcsv-1}] [expr {$inputs_start-1}] frequency] 0 ] 0 ]
set clocks_dc_start_column [lindex [lindex [m1 search rect $clocks_start_column $clocks_start [expr {$ncconcsv-1}] [expr {$inputs_start-1}] duty_cycle] 0 ] 0 ]

# Creating .sdc file with design name in output directory and opening it in write mode
set sdc_file [open $Output_Directory/$Design_Name.sdc "w"]

# Setting variables for actual clock row start and end
set i [expr {$clocks_start+1}]
set end_of_clocks [expr {$inputs_start-1}]

puts "\nInfo-SDC: Working on clock constraints....."

# while loop to write constraint commands to .sdc file
while { $i < $end_of_clocks } {
	# create_clock SDC command to create clocks
	puts -nonewline $sdc_file "\ncreate_clock -name [concat [m1 get cell 0 $i]_yui] -period [m1 get cell $clocks_freq_start_column $i] -waveform \{0 [expr {[m1 get cell $clocks_freq_start_column $i]*[m1 get cell $clocks_dc_start_column $i]/100}]\} \[get_ports [m1 get cell 0 $i]\]"

	# set_clock_transition SDC command to set clock transition values
	puts -nonewline $sdc_file "\nset_clock_transition -min -rise [m1 get cell $clocks_ers_start_column $i] \[get_clocks [m1 get cell 0 $i]\]"
	puts -nonewline $sdc_file "\nset_clock_transition -min -fall [m1 get cell $clocks_efs_start_column $i] \[get_clocks [m1 get cell 0 $i]\]"
	puts -nonewline $sdc_file "\nset_clock_transition -max -rise [m1 get cell $clocks_lrs_start_column $i] \[get_clocks [m1 get cell 0 $i]\]"
	puts -nonewline $sdc_file "\nset_clock_transition -max -fall [m1 get cell $clocks_lfs_start_column $i] \[get_clocks [m1 get cell 0 $i]\]"

	# set_clock_latency SDC command to set clock latency values
	puts -nonewline $sdc_file "\nset_clock_latency -source -early -rise [m1 get cell $clocks_erd_start_column $i] \[get_clocks [m1 get cell 0 $i]\]"
	puts -nonewline $sdc_file "\nset_clock_latency -source -early -fall [m1 get cell $clocks_efd_start_column $i] \[get_clocks [m1 get cell 0 $i]\]"
	puts -nonewline $sdc_file "\nset_clock_latency -source -late -rise [m1 get cell $clocks_lrd_start_column $i] \[get_clocks [m1 get cell 0 $i]\]"
	puts -nonewline $sdc_file "\nset_clock_latency -source -late -fall [m1 get cell $clocks_lfd_start_column $i] \[get_clocks [m1 get cell 0 $i]\]"

	set i [expr {$i+1}]
}
```

*Screenshots*

![sc20](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/67a821bf-8269-4558-80ee-40875968b2de)
![sc21](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/2669e9eb-1087-4276-bbe2-01a5aca614ce)


*openMSP430.sdc*

![sc19](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/ba4fc5f8-1244-40c4-955e-701cfd32ac50)

#### Processing of the constraints .csv file for INPUTS and dumping SDC commands to .sdc

I have successfully processed the csv file for INPUTS data as well as differentiated bit and bus inputs and dumped input-based SDC commands to .sdc file. The basic code of the same and screenshots of the terminal with several "puts" printing out the variables and user debug information as well as output .sdc are shown below.

*Code*

```tcl
# INPUTS section
# Finding column number starting for input clock latency in INPUTS section only
set inputs_erd_start_column [lindex [lindex [m1 search rect $clocks_start_column $inputs_start [expr {$ncconcsv-1}] [expr {$outputs_start-1}] early_rise_delay] 0 ] 0 ]
set inputs_efd_start_column [lindex [lindex [m1 search rect $clocks_start_column $inputs_start [expr {$ncconcsv-1}] [expr {$outputs_start-1}] early_fall_delay] 0 ] 0 ]
set inputs_lrd_start_column [lindex [lindex [m1 search rect $clocks_start_column $inputs_start [expr {$ncconcsv-1}] [expr {$outputs_start-1}] late_rise_delay] 0 ] 0 ]
set inputs_lfd_start_column [lindex [lindex [m1 search rect $clocks_start_column $inputs_start [expr {$ncconcsv-1}] [expr {$outputs_start-1}] late_fall_delay] 0 ] 0 ]

# Finding column number starting for input clock transition in INPUTS section only
set inputs_ers_start_column [lindex [lindex [m1 search rect $clocks_start_column $inputs_start [expr {$ncconcsv-1}] [expr {$outputs_start-1}] early_rise_slew] 0 ] 0 ]
set inputs_efs_start_column [lindex [lindex [m1 search rect $clocks_start_column $inputs_start [expr {$ncconcsv-1}] [expr {$outputs_start-1}] early_fall_slew] 0 ] 0 ]
set inputs_lrs_start_column [lindex [lindex [m1 search rect $clocks_start_column $inputs_start [expr {$ncconcsv-1}] [expr {$outputs_start-1}] late_rise_slew] 0 ] 0 ]
set inputs_lfs_start_column [lindex [lindex [m1 search rect $clocks_start_column $inputs_start [expr {$ncconcsv-1}] [expr {$outputs_start-1}] late_fall_slew] 0 ] 0 ]

# Finding column number starting for input related clock in INPUTS section only
set inputs_rc_start_column [lindex [lindex [m1 search rect $clocks_start_column $inputs_start [expr {$ncconcsv-1}] [expr {$outputs_start-1}] clocks] 0 ] 0 ]

# Setting variables for actual input row start and end
set i [expr {$inputs_start+1}]
set end_of_inputs [expr {$outputs_start-1}]

puts "\nInfo-SDC: Working on input constraints....."
puts "\nInfo-SDC: Categorizing input ports as bits and bussed"

# while loop to write constraint commands to .sdc file
while { $i < $end_of_inputs } {
	# Checking if input is bussed or not
	set netlist [glob -dir $Netlist_Directory *.v]
	set tmp_file [open /tmp/1 w]
	foreach f $netlist {
		set fd [open $f]
		while { [gets $fd line] != -1 } {
			set pattern1 " [m1 get cell 0 $i];"
			if { [regexp -all -- $pattern1 $line] } {
				set pattern2 [lindex [split $line ";"] 0]
				if { [regexp -all {input} [lindex [split $pattern2 "\S+"] 0]] } {
					set s1 "[lindex [split $pattern2 "\S+"] 0] [lindex [split $pattern2 "\S+"] 1] [lindex [split $pattern2 "\S+"] 2]"
					puts -nonewline $tmp_file "\n[regsub -all {\s+} $s1 " "]"
				}
			}
		}
	close $fd
	}
	close $tmp_file
	set tmp_file [open /tmp/1 r]
	set tmp2_file [open /tmp/2 w]
	puts -nonewline $tmp2_file "[join [lsort -unique [split [read $tmp_file] \n]] \n]"
	close $tmp_file
	close $tmp2_file
	set tmp2_file [open /tmp/2 r]
	set count [llength [read $tmp2_file]]
	close $tmp2_file
	if {$count > 2} {
		set inp_ports [concat [m1 get cell 0 $i]*]
	} else {
		set inp_ports [m1 get cell 0 $i]
	}

	# set_input_transition SDC command to set input transition values
	puts -nonewline $sdc_file "\nset_input_transition -clock \[get_clocks [m1 get cell $inputs_rc_start_column $i]\] -min -rise -source_latency_included [m1 get cell $inputs_ers_start_column $i] \[get_ports $inp_ports\]"
	puts -nonewline $sdc_file "\nset_input_transition -clock \[get_clocks [m1 get cell $inputs_rc_start_column $i]\] -min -fall -source_latency_included [m1 get cell $inputs_efs_start_column $i] \[get_ports $inp_ports\]"
	puts -nonewline $sdc_file "\nset_input_transition -clock \[get_clocks [m1 get cell $inputs_rc_start_column $i]\] -max -rise -source_latency_included [m1 get cell $inputs_lrs_start_column $i] \[get_ports $inp_ports\]"
	puts -nonewline $sdc_file "\nset_input_transition -clock \[get_clocks [m1 get cell $inputs_rc_start_column $i]\] -max -fall -source_latency_included [m1 get cell $inputs_lfs_start_column $i] \[get_ports $inp_ports\]"

	# set_input_delay SDC command to set input latency values
	puts -nonewline $sdc_file "\nset_input_delay -clock \[get_clocks [m1 get cell $inputs_rc_start_column $i]\] -min -rise -source_latency_included [m1 get cell $inputs_erd_start_column $i] \[get_ports $inp_ports\]"
	puts -nonewline $sdc_file "\nset_input_delay -clock \[get_clocks [m1 get cell $inputs_rc_start_column $i]\] -min -fall -source_latency_included [m1 get cell $inputs_efd_start_column $i] \[get_ports $inp_ports\]"
	puts -nonewline $sdc_file "\nset_input_delay -clock \[get_clocks [m1 get cell $inputs_rc_start_column $i]\] -max -rise -source_latency_included [m1 get cell $inputs_lrd_start_column $i] \[get_ports $inp_ports\]"
	puts -nonewline $sdc_file "\nset_input_delay -clock \[get_clocks [m1 get cell $inputs_rc_start_column $i]\] -max -fall -source_latency_included [m1 get cell $inputs_lfd_start_column $i] \[get_ports $inp_ports\]"

	set i [expr {$i+1}]
}
```

*Screenshots*

![sc22](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/fcdf8c5d-1c1e-4818-8ca4-fbb9c31b111c)
![sc24](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/1682c902-6962-49a5-80e8-e556a4446c49)
![sc23](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/ccdb0499-4c9b-4022-b99c-91058940f9ca)

*openMSP430.sdc*

![sc25](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/4e4d6e98-1089-4276-a55f-9442adc215ca)

/tmp/1 and /tmp/2 file screenshots for bit port

*/tmp/1*

![sc26](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/3d3a6113-4dd8-4c7f-a0c9-aa6314f9e463)

*/tmp/2*

![sc27](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/bc763938-a76a-43e3-8262-5616376d5bf9)

# Day 4 - Complete Scripting and Yosys Synthesis Introduction (05/11/2023)

Day 4's tasks included the output section processing and dumping of the SDC file, sample Yosys synthesis using example memory and explanation, Yosys hierarchy check, and its error handling.

**Review of input file - openMSP430_design_constraints.csv**

![Screenshot from 2023-08-29 15-51-51](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/a0251a70-a667-4c03-99cb-430d0cec1ef8)

### Implementation

I have successfully completed Day 4 tasks, namely processing constraints csv file for outputs and dumping SDC commands to .sdc file with actual processed data; learning sample memory synthesis and its memory write and read processes; dumping the hierarchy check Yosys script; and writing code handling errors in hierarchy check.

#### Processing of the constraints .csv file for OUTPUTS and dumping SDC commands to .sdc

I have successfully processed the csv file for OUTPUTS data as well as differentiated bit and bus outputs and dumped output-based SDC commands to .sdc file. The basic code of the same and screenshots of the terminal with several "puts" printing out the variables and user debug information as well as output .sdc are shown below.

*Code*

```tcl
# OUTPUTS section
# Finding column number starting for output clock latency in OUTPUTS section only
set outputs_erd_start_column [lindex [lindex [m1 search rect $clocks_start_column $outputs_start [expr {$ncconcsv-1}] [expr {$nrconcsv-1}] early_rise_delay] 0 ] 0 ]
set outputs_efd_start_column [lindex [lindex [m1 search rect $clocks_start_column $outputs_start [expr {$ncconcsv-1}] [expr {$nrconcsv-1}] early_fall_delay] 0 ] 0 ]
set outputs_lrd_start_column [lindex [lindex [m1 search rect $clocks_start_column $outputs_start [expr {$ncconcsv-1}] [expr {$nrconcsv-1}] late_rise_delay] 0 ] 0 ]
set outputs_lfd_start_column [lindex [lindex [m1 search rect $clocks_start_column $outputs_start [expr {$ncconcsv-1}] [expr {$nrconcsv-1}] late_fall_delay] 0 ] 0 ]

# Finding column number starting for output related clock in OUTPUTS section only
set outputs_rc_start_column [lindex [lindex [m1 search rect $clocks_start_column $outputs_start [expr {$ncconcsv-1}] [expr {$nrconcsv-1}] clocks] 0 ] 0 ]

# Finding column number starting for output load in OUTPUTS section only
set outputs_load_start_column [lindex [lindex [m1 search rect $clocks_start_column $outputs_start [expr {$ncconcsv-1}] [expr {$nrconcsv-1}] load] 0 ] 0 ]

# Setting variables for actual input row start and end
set i [expr {$outputs_start+1}]
set end_of_outputs [expr {$nrconcsv-1}]

puts "\nInfo-SDC: Working on output constraints....."
puts "\nInfo-SDC: Categorizing output ports as bits and bussed"

# while loop to write constraint commands to .sdc file
while { $i < $end_of_outputs } {
	# Checking if input is bussed or not
	set netlist [glob -dir $Netlist_Directory *.v]
	set tmp_file [open /tmp/1 w]
	foreach f $netlist {
		set fd [open $f]
		while { [gets $fd line] != -1 } {
			set pattern1 " [m1 get cell 0 $i];"
			if { [regexp -all -- $pattern1 $line] } {
				set pattern2 [lindex [split $line ";"] 0]
				if { [regexp -all {output} [lindex [split $pattern2 "\S+"] 0]] } {
					set s1 "[lindex [split $pattern2 "\S+"] 0] [lindex [split $pattern2 "\S+"] 1] [lindex [split $pattern2 "\S+"] 2]"
					puts -nonewline $tmp_file "\n[regsub -all {\s+} $s1 " "]"
				}
			}
		}
	close $fd
	}
	close $tmp_file
	set tmp_file [open /tmp/1 r]
	set tmp2_file [open /tmp/2 w]
	puts -nonewline $tmp2_file "[join [lsort -unique [split [read $tmp_file] \n]] \n]"
	close $tmp_file
	close $tmp2_file
	set tmp2_file [open /tmp/2 r]
	set count [llength [read $tmp2_file]]
	close $tmp2_file
	if {$count > 2} {
		set op_ports [concat [m1 get cell 0 $i]*]
	} else {
		set op_ports [m1 get cell 0 $i]
	}

	# set_output_delay SDC command to set output latency values
	puts -nonewline $sdc_file "\nset_output_delay -clock \[get_clocks [m1 get cell $outputs_rc_start_column $i]\] -min -rise -source_latency_included [m1 get cell $outputs_erd_start_column $i] \[get_ports $op_ports\]"
	puts -nonewline $sdc_file "\nset_output_delay -clock \[get_clocks [m1 get cell $outputs_rc_start_column $i]\] -min -fall -source_latency_included [m1 get cell $outputs_efd_start_column $i] \[get_ports $op_ports\]"
	puts -nonewline $sdc_file "\nset_output_delay -clock \[get_clocks [m1 get cell $outputs_rc_start_column $i]\] -max -rise -source_latency_included [m1 get cell $outputs_lrd_start_column $i] \[get_ports $op_ports\]"
	puts -nonewline $sdc_file "\nset_output_delay -clock \[get_clocks [m1 get cell $outputs_rc_start_column $i]\] -max -fall -source_latency_included [m1 get cell $outputs_lfd_start_column $i] \[get_ports $op_ports\]"

	# set_load SDC command to set load values
	puts -nonewline $sdc_file "\nset_load [m1 get cell $outputs_load_start_column $i] \[get_ports $op_ports\]"

	set i [expr {$i+1}]
}

close $sdc_file
puts "\nInfo-SDC: SDC created. Please use constraints in path $Output_Directory/$Design_Name.sdc"
```

*Screenshots*

![sc28](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/3c7a08e7-20dd-4dac-ab99-2bdc5741fbdd)
![sc29](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/36b81d6e-ebd4-4abe-b924-e3c364546ee3)
![sc30](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/a0fa3524-0cb7-43c5-a6c1-b3dc29be92e9)
![sc31](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/d947f25f-249e-4dd9-bf5c-1281734f4d35)
![sc32](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/7cd4fefe-867a-49ca-8a0c-510747b967ad)


*openMSP430.sdc*

![sc33](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/944e623a-ef7b-415d-910d-93483c0b52de)

/tmp/1 and /tmp/2 files similar to input ports

#### Memory module yosys synthesis and explanation

The verilog code *memory.v* for a single-bit address and single-bit data memory unit is given below.

*Code*

```verilog
module memory (CLK, ADDR, DIN, DOUT);

parameter wordSize = 1;
parameter addressSize = 1;

input ADDR, CLK;
input [wordSize-1:0] DIN;
output reg [wordSize-1:0] DOUT;
reg [wordSize:0] mem [0:(1<<addressSize)-1];

always @(posedge CLK) begin
	mem[ADDR] <= DIN;
	DOUT <= mem[ADDR];
	end

endmodule
```

The basic Yosys script *memory.ys* to run this and obtain a gate-level netlist and 2D representation of the memory module in gate components is provided below.

*Script*

```tcl
# Reding the library
read_liberty -lib -ignore_miss_dir -setattr blackbox /home/kunalg/Desktop/work/openmsp430/openmsp430/osu018_stdcells.lib
# Reading the verilog
read_verilog memory.v
synth top memory
splitnets -ports -format ___
dfflibmap -liberty /home/kunalg/Desktop/work/openmsp430/openmsp430/osu018_stdcells.lib
opt
abc -liberty /home/kunalg/Desktop/work/openmsp430/openmsp430/osu018_stdcells.lib
flatten
clean -purge
opt
clean
# Writing the netlist
write_verilog memory_synth.v
# Representation of netlist with it's components
show
```

The output view of netlist from the code is shown below.
![v1](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/c01a456e-5bb4-4a3b-9e8d-fc9f0d615304)

*Memory write process explained in following images using truth table*

Basic illustration of the write process

![v2](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/e737e333-786b-4104-ab18-36b713427a23)

Before first rising edge of the clock

![v3](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/4432b9be-6222-4d2a-81c8-ad9aa20b3b98)

After first rising edge of the clock - write process done

![v4](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/bca2741e-fdb0-4104-a848-aea4b662de51)

*Memory read process explained in following images using truth table*

Basic illustration of the read process

![v5](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/9f44875b-192f-47ae-9994-ebcfff55d925)

After first rising edge and before second rising edge of the clock

![v6](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/2b96b838-163b-4ccb-a43f-7788f592a1e0)

After second rising edge of the clock - read process done

![v7](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/779b5136-1c6c-41f6-bfd0-728f66de5155)

#### Hierarchy check script dumping

I have successfully written the code for dumping the hierarchy check script. The basic code of the same and screenshots of the terminal with several "puts" printing out the variables and user debug information as well as output .hier.ys are shown below.

*Code*

```tcl
# Hierarchy Check
# ---------------
puts "\nInfo: Creating hierarchy check script to be used by Yosys"
set data "read_liberty -lib -ignore_miss_dir -setattr blackbox ${Late_Library_Path}"
set filename "$Design_Name.hier.ys"
set fileId [open $Output_Directory/$filename "w"]
puts -nonewline $fileId $data
set netlist [glob -dir $Netlist_Directory *.v]
foreach f $netlist {
	puts -nonewline $fileId "\nread_verilog $f"
}
puts -nonewline $fileId "\nhierarchy -check"
close $fileId
```

*Screenshots*

![sp1](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/80023026-e01e-418f-bfbb-849a952b2a17)
![sp2](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/48fa0997-5ab4-4d96-95c0-1c38e8fb9f1b)
![sp3](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/caf855d5-a6d6-4296-9bd2-2064e5416c8e)

*openMSP430.hier.ys*

![sp4](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/199eeeb8-5d47-4259-a587-4ea0096035b8)

#### Hierarchy Check Run & Error Handling

I have successfully written the code for hierarchy check error handling in case any error pops up during hierarchy check run in Yosys and *exits if hierarchy check fails*. The basic code of the same and screenshots of the terminal with several "puts" printing out the variables and user debug information are shown below.

*Code*

```tcl
# Hierarchy check error handling
# Running hierarchy check in yosys by dumping log to log file and catching execution message
set error_flag [catch {exec yosys -s $Output_Directory/$Design_Name.hier.ys >& $Output_Directory/$Design_Name.hierarchy_check.log} msg]
if { $error_flag } {
	set filename "$Output_Directory/$Design_Name.hierarchy_check.log"
	# EDA tool specific hierarchy error search pattern
	set pattern {referenced in module}
	set count 0
	set fid [open $filename r]
	while { [gets $fid line] != -1 } {
		incr count [regexp -all -- $pattern $line]
		if { [regexp -all -- $pattern $line] } {
			puts "\nError: Module [lindex $line 2] is not part of design $Design_Name. Please correct RTL in the path '$Netlist_Directory'"
			puts "\nInfo: Hierarchy check FAIL"
		}
	}
	close $fid
	puts "\nInfo: Please find hierarchy check details in '[file normalize $Output_Directory/$Design_Name.hierarchy_check.log]' for more info. Exiting..."
	exit
} else {
	puts "\nInfo: Hierarchy check PASS"
	puts "\nInfo: Please find hierarchy check details in '[file normalize $Output_Directory/$Design_Name.hierarchy_check.log]' for more info"
}
```

*Screenshots*

![pass1](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/36ef7140-7440-41a2-b62c-c04325a38964)

*openMSP430.hierarchy_check.log*

![pass2](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/fe462613-23a1-4828-b741-ba25ef9df740)


![error1](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/42fdbdaf-6677-422a-9e5e-074d95a1e99b)

*openMSP430.hierarchy_check.log*

![error2](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/3b27a9e7-86fc-417b-a02e-95f297c7ee19)

## Day 5 - Advanced Scripting Techniques and Quality of Results (QoR) Generation (06/11/2023)

Day 5's tasks are to run main synthesis in Yosys, learn about procs and use them at the application level, create commands, and write necessary files required for the OpenTimer tool, such as .conf - .spef - .timing, write an OpenTimer script, run an OpenTimer STA, and collect the required data to form QoR from .results file generated from OpenTimer STA run and finally print the collected data in a tool-standard QoR output format.

### Implementation

I have successfully coded all the required elements to achieve Day 5 tasks, and all the details of the sub-tasks achieved are shown below.

#### Main Yosys synthesis script dumping

I have successfully written the code for the main Yosys synthesis script .ys file and dumped the script. The basic code of the same and screenshots of the terminal with several "puts" printing out the variables and user debug information are shown below.

*Code*

```tcl
# Main Synthesis Script
# ---------------------
puts "\nInfo: Creating main synthesis script to be used by Yosys"
set data "read_liberty -lib -ignore_miss_dir -setattr blackbox ${Late_Library_Path}"
set filename "$Design_Name.ys"
set fileId [open $Output_Directory/$filename "w"]
puts -nonewline $fileId $data
set netlist [glob -dir $Netlist_Directory *.v]
foreach f $netlist {
	puts -nonewline $fileId "\nread_verilog $f"
}
puts -nonewline $fileId "\nhierarchy -top $Design_Name"
puts -nonewline $fileId "\nsynth -top $Design_Name"
puts -nonewline $fileId "\nsplitnets -ports -format ___\ndfflibmap -liberty ${Late_Library_Path} \nopt"
puts -nonewline $fileId "\nabc -liberty ${Late_Library_Path}"
puts -nonewline $fileId "\nflatten"
puts -nonewline $fileId "\nclean -purge\niopadmap -outpad BUFX2 A:Y -bits\nopt\nclean"
puts -nonewline $fileId "\nwrite_verilog $Output_Directory/$Design_Name.synth.v"
close $fileId
puts "\nInfo: Synthesis script created and can be accessed from path $Output_Directory/$Design_Name.ys"
```
*Screenshots*

![final30](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/01788acc-0a83-41be-9121-d29ae19b5767)
![final31](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/692473f9-1017-4d57-9bff-1315a2c1205b)
![final32](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/5094d12c-857a-4bf8-bc6e-5ac69a2179a4)

*openMSP430.ys*

![final27](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/8011366a-ce45-4f3d-9a36-ee9a64bdf896)

#### Running main synthesis script & error handling

I have successfully written the code for running the main Yosys synthesis script and exiting if errors are found. The basic code and screenshots of the terminal are shown below.

*Code*

```tcl
puts "\nInfo: Running synthesis..........."
# Main synthesis error handling
# Running main synthesis in yosys by dumping log to log file and catching execution message
if { [catch {exec yosys -s $Output_Directory/$Design_Name.ys >& $Output_Directory/$Design_Name.synthesis.log} msg] } {
	puts "\nError: Synthesis failed due to errors. Please refer to log $Output_Directory/$Design_Name.synthesis.log for errors. Exiting...."
	exit
} else {
	puts "\nInfo: Synthesis finished successfully"
}
puts "\nInfo: Please refer to log $Output_Directory/$Design_Name.synthesis.log"
```

*Screenshots*

![final25](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/c14a4bf0-bf3e-4b38-b738-a2b2dd5c0a24)

*openMSP430.synthesis.log*

![final26](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/e0092b06-ead3-4645-b6bc-cfd7a4b1a2f7)

#### Editing .synth.v to be usable by OpenTimer

I have successfully written the code to edit the main synthesis output netlist .synth.v to make it usable for OpenTimer and other STA and PnR needs by replacing lines with "*" as a word and by removing "\" from any and all lines that have it. The basic code of the same and screenshots of the terminal with several "puts" printing out the variables and user debug information are shown below.

*Code*

```tcl
# Editing .synth.v to be usable by Opentimer
# ------------------------------------------
set fileId [open /tmp/1 "w"]
puts -nonewline $fileId [exec grep -v -w "*" $Output_Directory/$Design_Name.synth.v]
close $fileId
set output [open $Output_Directory/$Design_Name.final.synth.v "w"]
set filename "/tmp/1"
set fid [open $filename r]
while { [gets $fid line] != -1 } {
	puts -nonewline $output [string map {"\\" ""} $line]
	puts -nonewline $output "\n"
}
close $fid
close $output
puts "\nInfo: Please find the synthesized netlist for $Design_Name at below path. You can use this netlist for STA or PNR"
puts "\nPath: $Output_Directory/$Design_Name.final.synth.v"
```

*Screenshots*

![final24](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/1aeb7374-21bc-44fa-a19a-809971da265c)

*openMSP430.synth.v*

![final23](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/3d25d771-f682-4bde-af66-f06dc23470cd)


*openMSP430.final.synth.v*

![final22](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/ec722b5c-f61d-4b93-8a94-492cfa3ce9d0)

#### World of Procs (TCL Procedure)

Procs can be used to create user-defined commands, as shown below. I have successfully written the code for all the procs. The basic codes of all the procs and screenshots of the terminal with several "puts" printing out the variables and user debug information for the 'set_multi_cpu_usage' and the 'read_sdc' procs are shown below.

##### reopenStdout.proc

This proc redirects the 'stdout' screen log to the file in the proc's argument.

*Code*

```tcl
#!/bin/tclsh

# proc to redirect screen log to file
proc reopenStdout {file} {
    close stdout
    open $file w       
}
```

##### set_multi_cpu_usage.proc

This proc outputs multiple threads of the CPU usage command required for the OpenTimer tool.

*Code*

```tcl
#!/bin/tclsh

proc set_multi_cpu_usage {args} {
        array set options {-localCpu <num_of_threads> -help "" }
        while {[llength $args]} {
                switch -glob -- [lindex $args 0] {
                	-localCpu {
				set args [lassign $args - options(-localCpu)]
				puts "set_num_threads $options(-localCpu)"
			}
                	-help {
				set args [lassign $args - options(-help) ]
				puts "Usage: set_multi_cpu_usage -localCpu <num_of_threads> -help"
				puts "\t-localCpu - To limit CPU threads used"
				puts "\t-help - To print usage"
                      	}
                }
        }
}
```

*Screenshots*

![final21](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/a832ef3c-6704-4998-bd05-89fad4208272)

##### read_lib.proc

This proc outputs commands to read early and late libraries required for the OpenTimer tool.

*Code*

```tcl
#!/bin/tclsh

proc read_lib args {
	# Setting command parameter options and its values
	array set options {-late <late_lib_path> -early <early_lib_path> -help ""}
	while {[llength $args]} {
		switch -glob -- [lindex $args 0] {
		-late {
			set args [lassign $args - options(-late) ]
			puts "set_late_celllib_fpath $options(-late)"
		      }
		-early {
			set args [lassign $args - options(-early) ]
			puts "set_early_celllib_fpath $options(-early)"
		       }
		-help {
			set args [lassign $args - options(-help) ]
			puts "Usage: read_lib -late <late_lib_path> -early <early_lib_path>"
			puts "-late <provide late library path>"
			puts "-early <provide early library path>"
			puts "-help - Provides user deatails on how to use the command"
		      }	
		default break
		}
	}
}
```

##### read_verilog.proc

This proc outputs commands to read the synthesised netlist required for the OpenTimer tool.

*Code*

```tcl
#!/bin/tclsh

# Proc to convert read_verilog to OpenTimer format
proc read_verilog {arg1} {
	puts "set_verilog_fpath $arg1"
}
```

##### read_sdc.proc

This proc outputs commands to read constraints .timing file required for the OpenTimer tool. This procs converts SDC file contents to .timing file format for use by the OpenTimer tool, and the conversion code is explained stage by stage with sufficient screenshots.

###### Converting 'create_clock' constraints

Initially, the proc takes the SDC file as an input argument or parameter and processes the 'create_clock' constraints part of SDC.

*Code*

```tcl
#!/bin/tclsh

proc read_sdc {arg1} {

# 'file dirname <>' to get directory path only from full path
set sdc_dirname [file dirname $arg1]
# 'file tail <>' to get last element
set sdc_filename [lindex [split [file tail $arg1] .] 0 ]
set sdc [open $arg1 r]
set tmp_file [open /tmp/1 w]

# Removing "[" & "]" from SDC for further processing the data with 'lindex'
# 'read <>' to read entire file
puts -nonewline $tmp_file [string map {"\[" "" "\]" " "} [read $sdc]]     
close $tmp_file

# Opening tmp file to write constraints converted from generated SDC
set timing_file [open /tmp/3 w]

# Converting create_clock constraints
# -----------------------------------
set tmp_file [open /tmp/1 r]
set lines [split [read $tmp_file] "\n"]
# 'lsearch -all -inline' to search list for pattern and retain elementas with pattern only
set find_clocks [lsearch -all -inline $lines "create_clock*"]
foreach elem $find_clocks {
	set clock_port_name [lindex $elem [expr {[lsearch $elem "get_ports"]+1}]]
	set clock_period [lindex $elem [expr {[lsearch $elem "-period"]+1}]]
	set duty_cycle [expr {100 - [expr {[lindex [lindex $elem [expr {[lsearch $elem "-waveform"]+1}]] 1]*100/$clock_period}]}]
	puts $timing_file "\nclock $clock_port_name $clock_period $duty_cycle"
}
close $tmp_file
```


###### Converting 'set_clock_latency' constraints

Processes 'set_clock_latency' constraints part of SDC.

*Code*

```tcl
# Converting set_clock_latency constraints
# ----------------------------------------
set find_keyword [lsearch -all -inline $lines "set_clock_latency*"]
set tmp2_file [open /tmp/2 w]
set new_port_name ""
foreach elem $find_keyword {
        set port_name [lindex $elem [expr {[lsearch $elem "get_clocks"]+1}]]
	if {![string match $new_port_name $port_name]} {
        	set new_port_name $port_name
        	set delays_list [lsearch -all -inline $find_keyword [join [list "*" " " $port_name " " "*"] ""]]
        	set delay_value ""
        	foreach new_elem $delays_list {
        		set port_index [lsearch $new_elem "get_clocks"]
        		lappend delay_value [lindex $new_elem [expr {$port_index-1}]]
        	}
		puts -nonewline $tmp2_file "\nat $port_name $delay_value"
	}
}

close $tmp2_file
set tmp2_file [open /tmp/2 r]
puts -nonewline $timing_file [read $tmp2_file]
close $tmp2_file
```

###### Converting 'set_clock_transition' constraints

Processes 'set_clock_transition' constraints part of SDC.

*Code*

```tcl
# Converting set_clock_transition constraints
# -------------------------------------------
set find_keyword [lsearch -all -inline $lines "set_clock_transition*"]
set tmp2_file [open /tmp/2 w]
set new_port_name ""
foreach elem $find_keyword {
        set port_name [lindex $elem [expr {[lsearch $elem "get_clocks"]+1}]]
        if {![string match $new_port_name $port_name]} {
		set new_port_name $port_name
		set delays_list [lsearch -all -inline $find_keyword [join [list "*" " " $port_name " " "*"] ""]]
        	set delay_value ""
        	foreach new_elem $delays_list {
        		set port_index [lsearch $new_elem "get_clocks"]
        		lappend delay_value [lindex $new_elem [expr {$port_index-1}]]
        	}
        	puts -nonewline $tmp2_file "\nslew $port_name $delay_value"
	}
}

close $tmp2_file
set tmp2_file [open /tmp/2 r]
puts -nonewline $timing_file [read $tmp2_file]
close $tmp2_file
```

###### Converting 'set_input_delay' constraints

Processes 'set_input_delay' constraints part of SDC.

*Code*

```tcl
# Converting set_input_delay constraints
# --------------------------------------
set find_keyword [lsearch -all -inline $lines "set_input_delay*"]
set tmp2_file [open /tmp/2 w]
set new_port_name ""
foreach elem $find_keyword {
        set port_name [lindex $elem [expr {[lsearch $elem "get_ports"]+1}]]
        if {![string match $new_port_name $port_name]} {
                set new_port_name $port_name
        	set delays_list [lsearch -all -inline $find_keyword [join [list "*" " " $port_name " " "*"] ""]]
		set delay_value ""
        	foreach new_elem $delays_list {
        		set port_index [lsearch $new_elem "get_ports"]
        		lappend delay_value [lindex $new_elem [expr {$port_index-1}]]
        	}
        	puts -nonewline $tmp2_file "\nat $port_name $delay_value"
	}
}
close $tmp2_file
set tmp2_file [open /tmp/2 r]
puts -nonewline $timing_file [read $tmp2_file]
close $tmp2_file
```


###### Converting 'set_input_transition' constraints

Processes 'set_input_transition' constraints part of SDC.

*Code*

```tcl
# Converting set_input_transition constraints
# -------------------------------------------
set find_keyword [lsearch -all -inline $lines "set_input_transition*"]
set tmp2_file [open /tmp/2 w]
set new_port_name ""
foreach elem $find_keyword {
        set port_name [lindex $elem [expr {[lsearch $elem "get_ports"]+1}]]
        if {![string match $new_port_name $port_name]} {
                set new_port_name $port_name
        	set delays_list [lsearch -all -inline $find_keyword [join [list "*" " " $port_name " " "*"] ""]]
        	set delay_value ""
        	foreach new_elem $delays_list {
        		set port_index [lsearch $new_elem "get_ports"]
        		lappend delay_value [lindex $new_elem [expr {$port_index-1}]]
        	}
        	puts -nonewline $tmp2_file "\nslew $port_name $delay_value"
	}
}

close $tmp2_file
set tmp2_file [open /tmp/2 r]
puts -nonewline $timing_file [read $tmp2_file]
close $tmp2_file
```

###### Converting 'set_output_delay' constraints

Processes 'set_output_delay' constraints part of SDC.

*Code*

```tcl
# Converting set_output_delay constraints
# ---------------------------------------
set find_keyword [lsearch -all -inline $lines "set_output_delay*"]
set tmp2_file [open /tmp/2 w]
set new_port_name ""
foreach elem $find_keyword {
        set port_name [lindex $elem [expr {[lsearch $elem "get_ports"]+1}]]
        if {![string match $new_port_name $port_name]} {
                set new_port_name $port_name
        	set delays_list [lsearch -all -inline $find_keyword [join [list "*" " " $port_name " " "*"] ""]]
        	set delay_value ""
        	foreach new_elem $delays_list {
        		set port_index [lsearch $new_elem "get_ports"]
        		lappend delay_value [lindex $new_elem [expr {$port_index-1}]]
        	}
        	puts -nonewline $tmp2_file "\nrat $port_name $delay_value"
	}
}

close $tmp2_file
set tmp2_file [open /tmp/2 r]
puts -nonewline $timing_file [read $tmp2_file]
close $tmp2_file
```

###### Converting 'set_load' constraints

Processes 'set_load' constraints part of SDC. And with that, all SDC constarints are processed, so we close the /tmp/3 file containing all processed data for now.

*Code*

```tcl
# Converting set_load constraints
# -------------------------------
set find_keyword [lsearch -all -inline $lines "set_load*"]
set tmp2_file [open /tmp/2 w]
set new_port_name ""
foreach elem $find_keyword {
        set port_name [lindex $elem [expr {[lsearch $elem "get_ports"]+1}]]
        if {![string match $new_port_name $port_name]} {
                set new_port_name $port_name
        	set delays_list [lsearch -all -inline $find_keyword [join [list "*" " " $port_name " " "*" ] ""]]
        	set delay_value ""
        	foreach new_elem $delays_list {
        	set port_index [lsearch $new_elem "get_ports"]
        	lappend delay_value [lindex $new_elem [expr {$port_index-1}]]
        	}
        	puts -nonewline $timing_file "\nload $port_name $delay_value"
	}
}
close $tmp2_file
set tmp2_file [open /tmp/2 r]
puts -nonewline $timing_file  [read $tmp2_file]
close $tmp2_file

# Closing tmp file after writing constraints converted from generated SDC
close $timing_file
```

*Screenshots*

*/tmp/3*

![sp5](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/323298c6-ca44-43bc-af66-734ace679692)

###### Expanding the bussed input and output ports

The /tmp/3 file contains bussed ports as <port_name>*, which is expanded to each bit, and single-bit port lines are untouched. This new content is dumped to .timing file, and then the proc exits by giving output the OpenTimer command to access this .timing file.

*Code*

```tcl
# Expanding the bussed input and output ports to it's individual bits and writing final .timing file for OpenTimer
set ot_timing_file [open $sdc_dirname/$sdc_filename.timing w]
set timing_file [open /tmp/3 r]
while { [gets $timing_file line] != -1 } {
        if {[regexp -all -- {\*} $line]} {
                set bussed [lindex [lindex [split $line "*"] 0] 1]
                set final_synth_netlist [open $sdc_dirname/$sdc_filename.final.synth.v r]
                while { [gets $final_synth_netlist line2] != -1 } {
                        if {[regexp -all -- $bussed $line2] && [regexp -all -- {input} $line2] && ![string match "" $line]} {

                        	puts -nonewline $ot_timing_file "\n[lindex [lindex [split $line "*"] 0 ] 0 ] [lindex [lindex [split $line2 ";"] 0 ] 1 ] [lindex [split $line "*"] 1 ]"

                        } elseif {[regexp -all -- $bussed $line2] && [regexp -all -- {output} $line2] && ![string match "" $line]} {

                        	puts -nonewline $ot_timing_file "\n[lindex [lindex [split $line "*"] 0 ] 0 ] [lindex [lindex [split $line2 ";"] 0 ] 1 ] [lindex [split $line "*"] 1 ]"

                        }
                }
        } else {
        	puts -nonewline $ot_timing_file  "\n$line"
        }
}
close $timing_file
puts "set_timing_fpath $sdc_dirname/$sdc_filename.timing"

}
```

*Screenshots*

![final16](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/f24be25f-8ca3-42b6-8050-5130a0e1fb62)

*/tmp/3*

![final17](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/e5a74d82-b986-469c-b9f8-5355d534eb78)

*openMSP430.timing*

![final15](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/90953e3a-4544-4c88-a5b0-dea07f5fe500)

#### Using the procs to write .conf

I have successfully written the code to use these procs on an application level and create some portion of the .conf configuration file required for the OpenTimer tool. The basic code of the same and screenshots of terminal and .conf are shown below.

*Code*

```tcl
# Preparation of .conf for OpenTimer STA
# --------------------------------------
# Procs used below \/
puts "\nInfo: Timing Analysis Started...."
puts "\nInfo: Initializing number of threads, libraries, sdc, verilog netlist path..."
# Sourcing required procs
source /home/vsduser/vsdsynth/procs/reopenStdout.proc
source /home/vsduser/vsdsynth/procs/set_multi_cpu_usage.proc
source /home/vsduser/vsdsynth/procs/read_lib.proc
source /home/vsduser/vsdsynth/procs/read_verilog.proc
source /home/vsduser/vsdsynth/procs/read_sdc.proc
# Writing command required for OpenTimer tool to .conf file by closing and redirecting 'stdout' to a file
reopenStdout $Output_Directory/$Design_Name.conf
set_multi_cpu_usage -localCpu 4
read_lib -early $Early_Library_Path
read_lib -late $Late_Library_Path
read_verilog $Output_Directory/$Design_Name.final.synth.v
read_sdc $Output_Directory/$Design_Name.sdc
# Reopening 'stdout' to bring back screen log
reopenStdout /dev/tty
# Closing .conf file opened by 'reopenStdout' proc
close $Output_Directory/$Design_Name.conf
```

*Screenshots*
![final13](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/aa3351a0-dbcf-4f55-b379-27a99c17ce21)
![final14](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/7ecbe832-334f-48bc-81af-5b1e2c14d8ad)
![final12](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/66286d46-ce5e-41ad-9c98-79cb6f0d77f8)

*openMSP430.conf*

![final11](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/55c4eaf3-d133-40a9-9d54-54f122023d7b)

#### Preparation of rest of .conf & .spef files for OpenTimer STA

I have successfully written the code to write .spef *with the current date and time in the spef code* and to append the rest of the portion of .conf file. The basic code of the same and screenshots of terminal, .conf and .spef are shown below.

*Code*

```tcl
# Preparation of .conf & .spef for OpenTimer STA
# ----------------------------------------------
# Continue to write .conf and also write a .spef
# Writing .spef
set enable_prelayout_timing 1
if {$enable_prelayout_timing == 1} {
	puts "\nInfo: enable_prelayout_timing is $enable_prelayout_timing. Enabling zero-wire load parasitics"
	set spef_file [open $Output_Directory/$Design_Name.spef w]
	puts $spef_file "*SPEF \"IEEE 1481-1998\" "
	puts $spef_file "*DESIGN \"$Design_Name\" "
	puts $spef_file "*DATE \"[clock format [clock seconds] -format {%a %b %d %I:%M:%S %Y}]\" "
	puts $spef_file "*VENDOR \"TAU 2015 Contest\" "
	puts $spef_file "*PROGRAM \"Benchmark Parasitic Generator\" "
	puts $spef_file "*VERSION \"0.0\" "
	puts $spef_file "*DESIGN_FLOW \"NETLIST_TYPE_VERILOG\" "
	puts $spef_file "*DIVIDER / "
	puts $spef_file "*DELIMITER : "
	puts $spef_file "*BUS_DELIMITER \[ \] "
	puts $spef_file "*T_UNIT 1 PS "
	puts $spef_file "*C_UNIT 1 FF "
	puts $spef_file "*R_UNIT 1 KOHM "
	puts $spef_file "*L_UNIT 1 UH "
	close $spef_file
}

# Appending to .conf file
set conf_file [open $Output_Directory/$Design_Name.conf a]
puts $conf_file "set_spef_fpath $Output_Directory/$Design_Name.spef"
puts $conf_file "init_timer "
puts $conf_file "report_timer "
puts $conf_file "report_wns "
puts $conf_file "report_worst_paths -numPaths 10000 "
close $conf_file
```

*Screenshots*

![final9](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/40173e7b-5fc7-48be-83f9-6a321d45d9e6)

*openMSP430.spef*

![final8](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/15592a35-7e0b-492a-8203-bdd104650f43)

*openMSP430.conf*

![final7](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/9bc9c181-d206-4104-8ed2-8fcc2b4e8153)
<!---
![Screenshot from 2023-08-29 18-49-34](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/bc1a6e67-1962-4412-ade9-3519e78efb97)
-->

#### STA using OpenTimer

I have successfully written the code to run STA on OpenTimer and capture its runtime. The basic code of the same and screenshots of the terminal with several "puts" printing out the variables and user debug information are shown below.

*Code*

```tcl
# Static Timing Analysis using OpenTimer
# --------------------------------------
# Running STA on OpenTimer and dumping log to .results and capturing runtime
set tcl_precision 3
set time_elapsed_in_us [time {exec /home/kunalg/Desktop/tools/opentimer/OpenTimer-1.0.5/bin/OpenTimer < $Output_Directory/$Design_Name.conf >& $Output_Directory/$Design_Name.results}]
set time_elapsed_in_sec "[expr {[lindex $time_elapsed_in_us 0]/1000000}]sec"
puts "\nInfo: STA finished in $time_elapsed_in_sec seconds"
puts "\nInfo: Refer to $Output_Directory/$Design_Name.results for warnings and errors"
```

*Screenshots*

![final6](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/3bdeceb7-f10d-4d56-a7b8-75ca08546d48)

*openMSP430.results*

![final5](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/da53015c-d9a4-4c67-9a2f-f00b70e12a43)
<!---
![Screenshot from 2023-08-29 19-00-18](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/32eaa113-437c-40b9-b08e-ca4e1b263acb)
![Screenshot from 2023-08-29 19-00-58](https://github.com/fayizferosh/yosys-tcl-ui-report/assets/63997454/094ed944-018c-4428-bc1f-f95cdc425e05)
-->

#### Data collection from .results file and other sources for QoR

I have successfully written the code to collect all required data for specific codes and *have also collected total .tcl script runtime*. The basic code of the same and screenshots of the terminal with several "puts" printing out the variables and user debug information are shown below.

*Code*

```tcl
# Find worst output violation
set worst_RAT_slack "-"
set report_file [open $Output_Directory/$Design_Name.results r]
set pattern {RAT}
while { [gets $report_file line] != -1 } {
	if {[regexp $pattern $line]} {
		set worst_RAT_slack "[expr {[lindex $line 3]/1000}]ns"
		break
	} else {
		continue
	}
}
close $report_file

# Find number of output violation
set report_file [open $Output_Directory/$Design_Name.results r]
set count 0
while { [gets $report_file line] != -1 } {
	incr count [regexp -all -- $pattern $line]
}
set Number_output_violations $count
close $report_file

# Find worst setup violation
set worst_negative_setup_slack "-"
set report_file [open $Output_Directory/$Design_Name.results r]
set pattern {Setup}
while { [gets $report_file line] != -1 } {
	if {[regexp $pattern $line]} {
		set worst_negative_setup_slack "[expr {[lindex $line 3]/1000}]ns"
		break
	} else {
		continue
	}
}
close $report_file

# Find number of setup violation
set report_file [open $Output_Directory/$Design_Name.results r]
set count 0
while { [gets $report_file line] != -1 } {
	incr count [regexp -all -- $pattern $line]
}
set Number_of_setup_violations $count
close $report_file

# Find worst hold violation
set worst_negative_hold_slack "-"
set report_file [open $Output_Directory/$Design_Name.results r]
set pattern {Hold}
while { [gets $report_file line] != -1 } {
	if {[regexp $pattern $line]} { 
		set worst_negative_hold_slack "[expr {[lindex $line 3]/1000}]ns"
		break
	} else {
		continue
	}
}
close $report_file

# Find number of hold violation
set report_file [open $Output_Directory/$Design_Name.results r]
set count 0
while {[gets $report_file line] != -1} {
	incr count [regexp -all - $pattern $line]
}
set Number_of_hold_violations $count
close $report_file

# Find number of instance
set pattern {Num of gates}
set report_file [open $Output_Directory/$Design_Name.results r]
while {[gets $report_file line] != -1} {
	if {[regexp -all -- $pattern $line]} {
		set Instance_count [lindex [join $line " "] 4 ]
		break
	} else {
		continue
	}
}
close $report_file

# Capturing end time of the script
set end_time [clock clicks -microseconds]

# Setting total script runtime to 'time_elapsed_in_sec' variable
set time_elapsed_in_sec "[expr {($end_time-$start_time)/1000000}]sec"
```

*Screenshots*

![final4](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/8578f15d-bc70-439e-8785-f157a7fd5f5a)

#### QoR (Quality of Results) Generation

I have successfully written the code for QoR generation. The basic code and screenshots of the terminal are shown below.

*Code*

```tcl
# Quality of Results (QoR) generation
puts "\n"
puts "                                                           ****PRELAYOUT TIMING RESULTS****\n"
set formatStr {%15s%14s%21s%16s%16s%15s%15s%15s%15s}
puts [format $formatStr "-----------" "-------" "--------------" "---------" "---------" "--------" "--------" "-------" "-------"]
puts [format $formatStr "Design Name" "Runtime" "Instance Count" "WNS Setup" "FEP Setup" "WNS Hold" "FEP Hold" "WNS RAT" "FEP RAT"]
puts [format $formatStr "-----------" "-------" "--------------" "---------" "---------" "--------" "--------" "-------" "-------"]
foreach design_name $Design_Name runtime $time_elapsed_in_sec instance_count $Instance_count wns_setup $worst_negative_setup_slack fep_setup $Number_of_setup_violations wns_hold $worst_negative_hold_slack fep_hold $Number_of_hold_violations wns_rat $worst_RAT_slack fep_rat $Number_output_violations {
	puts [format $formatStr $design_name $runtime $instance_count $wns_setup $fep_setup $wns_hold $fep_hold $wns_rat $fep_rat]
}
puts [format $formatStr "-----------" "-------" "--------------" "---------" "---------" "--------" "--------" "-------" "-------"]
puts "\n"
```

*Screenshots*

![final1](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/9e0fc4fb-81e9-4203-90f0-9ef8bbed9bda)
![final2](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/60718d38-66ce-46c5-ad94-0e1e845e73f4)
![final3](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/a5de37ec-cefc-492c-ab1b-930d3d8515e5)

# Acknowledgements

* [Kunal Ghosh](https://github.com/kunalg123), Co-founder, VSD Corp. Pvt. Ltd.
