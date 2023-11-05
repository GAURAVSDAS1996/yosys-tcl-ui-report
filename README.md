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

![sc16](https://github.com/GAURAVSDAS1996/screenshots/assets/76874646/86812cf5-e5b8-4d26-8bcf-da40fc8b8356)

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

