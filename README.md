# AnalysisFunction

Analysis Function provides scripts to analyze electrophysiological data recorded with the HEKA system: the FALCON patch clamp system which combines the application of mechanical stimuli with self-sensing cantilever (Patch Clamp folder) and the two electrode voltage clamp system (TEVC folder). 

## TEVC data 
 - [How to use Scripts](#how-to-use-scripts) 
 - [testing suheaders](#####testing-suheaders)
 - [Run TEVCSelectivitySTFX.m for analysis of selectivity](#run-tEVCSelectivitySTFX.m-for-analysis-of-selectivity)

##### (works with from MATLAB_R2014b to R2020b - have not tested above)

Selectivity Study:

### DAY 0: harvest:
- harvest oocytes
- prepare injection mix and keep at -80C
	- my data are in folder /Users/Fechner/Box Sync/Fechner/TEVC-GoodmanlabBOX/Project-STFX/InjectionMixesSTFX
	- examples excel sheets for injections are in the ExampleExcelSheets folder
	- adjust comments to prepare for how many recordings and which conditions to do

### DAY 1: inject oocytes
- inject oocytes and keep at 18C in L-15 + 300 uM Amilroide

### DAY 4 to 7: recordings
- for selectivity measurements I always use the following sequences: 
	- monitor the cell with ContRamp1550 until it’s stable [Protocol Fig 1A](https://rupress.org/jgp/article/153/4/e202012655/211847/DEG-ENaC-ASIC-channels-vary-in-their-sensitivity).
	- use pgf 10ContStep, which is a combination of 10x ContRamp1550 and 3x STEPSens (Steps are similar to protocol Fig 1A, but have steps instead of ramps)
	- monitor the cell with ContRamp1550, switch solutions and monitor until it’s stable in new solutions
	- use pgf 10ContStep 
	- and the end: use ContRamp1550, switch back to NaGluSel solution, switch to 300 uM amiloride, and wash with NaGluSel solution (This last use of the protocol ContRamp1550 (or sometimes used at the beginning) with the switch to amiloride is used to measure the amplitude of the current at -85 mV and to determine the quality of the recording.  this step will be used to estemina)

### Analysis of Data with Matlab

#### Selectivity Data & Amplitude measurements:

##### Enter Meta Data

- Amplitude analysis amiloride sensitive current: Enter ContRamp1550 of amiloride block data into excel spreadsheet (only works 1 file/series per cell). They are located in the Box Folder: /Users/Fechner/Box Sync/Fechner/TEVC-GoodmanlabBOX/Project-STFX/MetaDataSTFX/MetaDelta and are called TEVCMetaSTFX111.xlsx – replace STFX111 with the current frog number. Enter last series before switching to a new solutions (the matlab code will analyze this number and the previous 2 series for replicate of 3) 
- Enter the series numbers for the 3 STEPSens replicates in TEVCMetaSTFX112-Selectivity.xlsx (replace STFX112 with current frog number) Meta data sheet found here: /Users/Fechner/Box Sync/Fechner/TEVC-GoodmanlabBOX/Project-STFX/MetaDataSTFX
- example meta data for the analysis on frog STFX112 are found in the folder ExampleExcelSheets (including example dat files 20210321 & 20210322


## How to use Scripts 

##### RUN MATLAB Code:

- Clone matlab code to your computer
	- https://github.com/wormsenseLab/AnalysisFunction.git
-	Prerequisite: follow the instruction of sigtool installation for the import.dat files [sigTOOL installation prerequisite](https://github.com/wormsenseLab/Matlab-PatchMaster)
	- copied from Sammy's readme: IMPORTANT: The ImportHEKAtoMat function (a modified version of ImportHEKA) must be placed in the sigTOOL folder: 'sigTOOL\sigTOOL NeuroscienceToolkit\File\menu_Import\group_NeuroScience File Formats'
- important functions from Sammy
	- ImportMetaData.m
	- ImportPatchData.m
	- SplitSeries.m
-  you can also find the sigtool folder and all the scripts used in the folder for the summer interns: /Users/Fechner/Box Sync/Fechner/TEVC-GoodmanlabBOX/StanfordSummerInternship/MatlabScripts
- Open Matlab
-	Type sigTOOL in command window, click continue, if a window appears (mostly grey), just close it.
-	Before starting the analysis, navigate to the folder where you want to save the Ratio-analysis files. In my case: /Users/Fechner/Box Sync/Fechner/TEVC-GoodmanlabBOX/Project-STFX/RatioSTFX


##### Run TEVCAnalyzeLoopSTFX.m for analysis of amplitude 

- First section (load dat.files)
	- which is a script from Sammy (I copied those files in a folder locally and not using the one from Github, but Github would probably be fine).
	- Navigate to the folder where the .dat files are stored, in my case: /Users/Fechner/Box Sync/Fechner/TEVC-GoodmanlabBOX/Project-STFX/datFilesSTFX
	- Loads a file structure called ephysData (explore data, contains navigation to all series etc)

- Second section (Load Meta Data TEVC)
	- Load Meta Data for amiloride sensitive amplitude measurements
	- /Users/Fechner/Box Sync/Fechner/TEVC-GoodmanlabBOX/Project-STFX/MetaDataSTFX/MetaDelta
		- e.g. the file: TEVCMetaSTFX112.xlsx (it's uploaded in the ExampleExcelSheets folder)

- Third section (Analysis Individual Recording)
	- Enter “2” behind the variable *a1* (2 is the first recording)
	- In the first round, enter 2 as well behind the variable *a2*
	- After running the code ones, you can use the variable *LastFile* behind *a2*, but it doesn’t exist in the first round yet 
	- *i* gives the column row corresponding to the MetaData + the name of the recording
	- if there is an error, the following error message appears
		- there were an error; This file will be skipped for analysis.
		- Debugging: type "name" in the command window below to figure our the file name.
		- typical error: 1) check if the file exist in the data structure ephysData on the right in Workspace.
		- 2) Verify that the name in MetaData Sheet is correct.
		- Check if you entered the correct series
	- You can create a plot for each recording by changing the variable *makePlots* from 0 to 1
	- The code creates a “single” (!) file called RatioDeltaTEVC-STFX112.txt where STFX112 is taken from the MetaData sheet entry.

##### testing suheaders

##### Run TEVCSelectivitySTFX.m for analysis of selectivity

- First section
	- Identical to amplitude section

- Second section (Load Meta Data TEVC)
	- Similar, but different MetaData sheet: /Users/Fechner/Box Sync/Fechner/TEVC-GoodmanlabBOX/Project-STFX/MetaDataSTFX e.g. the file TEVCMetaSTFX112-Selectivity.xlsx

- Third Section:
	- Same procedure as script above
	- Enter “2” behind the variable *a1* (2 is the first recording)
	- In the first round, enter 2 as well behind the variable *a2*
	- After running the code ones, you can use the variable *LastFile* behind a2, but it doesn’t exist in the first round yet 
	- It generates an individual files for each recording, e.g.: Selectivity-TEVC-STFX112023.txt

### Figure creating with Jupyter notebook:
	
[Jupyter notebooks DEG/ENaC/ASIC channels TEVC](https://github.com/wormsenseLab/JupyterNotebooksDEGENaCPharm.git)


