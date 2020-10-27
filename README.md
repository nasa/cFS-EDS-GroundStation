# cFS-EDS-GroundStation

This is a Pyton based ground station that interfaces with an Electronic Data Sheets supported Core Flight Systems instance.  
Also, there are several utility python scripts that provide basic Telemetry and Telecommand functionality in a non-GUI format.

The user's manual can be found at docs/cFS-EDS-GroundStation Users Manual.docx

## Prerequisistes

  -python3-dev
  -python3-pyqt5

## Configuring and running

It is recommended to use this software in conjunction with the cfe-eds-framework repository which can be found at:
https://github.com/jphickey/cfe-eds-framework
This software package requries the EdsLib and CFE_MissionLib python modules from that repository to run.  The build process
also automatically configues these files with the defined mission name.

##

First, the following cmake variables need to be turned on for both the GroundStation or assorted utilities to work.

EDSLIB_PYTHON_BUILD_STANDALONE_MODULE:BOOL=ON
CFE_MISSIONLIB_PYTHON_BUILD_STANDALONE_MODULE:BOOL=ON
CONFIGURE_CFS_EDS_GROUNDSTATION:BOOL=ON

The first two options will compile the python bindings for EdsLib and CFE_MissionLib respectively.
The third configures the python scripts in this folder with the mission name defined in the cFS build process
and output the scripts to the ${CFS_HOME}/build/exe/host/cFS-EDS-GroundStation/ folder.

The folder where the python modules get installed is:
${CFS_HOME}/build/exe/lib/python

This folder needs to be added to the PYTHON_PATH environment variable so the modules can be imported into Python.
The folder that contains EdsLib and CFE_MissionLib .so files also needs to be added to the LD_LIBRARY_PATH
enviroment variable so Python can load these libraries.  For example the following lines can be added to ~/.bashrc

CFS_HOME = /path/to/cfs/directory
export PYTHONPATH=$PYTHONPATH:$CFS_HOME/build/exe/lib/python
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CFS_HOME/build/exe/lib

With this set the cFS-EDS-GroundStation can be run with the following command

python3 cFS-EDS-GroundStation.py

# Utility python scripts

python3 cmdUtil.py

This script runs through several prompts to send a command to a core flight instance.  The user inputs the instance name,
topic, subcommand (if applicable), payload values (if applicable), and destination IP address.  The script will create, 
fill, pack, and send a command to the desired IP address.

python3 tlm_decode.py -p <port=5021>

This script will listen in on the specified port for telemetry messages.  When messages come in they are decoded
and the contents are displayed on the screen.

python3 convert_tlm_file.py -f <filename>       or
python3 convert_tlm_file.py --file=<filename>   (recommended as this allows tab completion of file names)

This script takes the binary telementry files from the Telemetry System of the cFS-EDS-GroundStation, reads through all
of the messages in the file, and writes the decoded packet information in a csv fie of the same base name.
