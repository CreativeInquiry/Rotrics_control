# Rotrics Dexarm Python/Serial API
---
This is where the python API using the pyserial library is stored. If you want to handle sending G-Code over serial you can use the Python API in the "api" folder with instructions below. 

Python is not the only method to communicate with the Dexarm over serial, you can write in any language you are comfortable by convert the python classes to your chosen language. You can also use a port monitoring software like CoolTerm to send individual G-Code strings. See [THESE](https://manual.rotrics.com/get-start/drawing-and-writing/generate-writing-drawing-g-code-with-inkscape) docs for Inkscape and Illustrator plugins

### API Usage
1.  Install Python 3.7 or Higher
2.  Create a venv in the directory you intend to work `python -m venv <env_name>` and replace <env_name> with something like `myVenv`
3.  In terminal activate the venv. Mac:`$ source myvenv/bin/activate` || Windows Powershell: `venv\Scripts\Activate.ps1`
4.  With venv activated `pip install pyserial'
5.  Use this import line to get the  `from pydexarm_sfci import Dexarm` at the top of your script in order to use the serial communication.

Below is an example script using the api's classes 
```
from pydexarm_sfci import Dexarm
import time

'''windows'''
dexarm = Dexarm(port="COM3")
'''mac & linux'''
# device = Dexarm(port="/dev/tty.usbmodem3086337A34381")

dexarm.go_home()

dexarm.move_to(50, 300, 0)
dexarm.move_to(50, 300, -50)
dexarm.move_to(50, 300, 0)
dexarm.move_to(-50, 300, 0)
dexarm.move_to(-50, 300, -50)

dexarm.go_home()

'''DexArm sliding rail Demo'''
'''
dexarm.conveyor_belt_forward(2000)
time.sleep(20)
dexarm.conveyor_belt_backward(2000)
time.sleep(10)
dexarm.conveyor_belt_stop()
'''

'''DexArm sliding rail Demo'''
'''
dexarm.go_home()
dexarm.sliding_rail_init()
dexarm.move_to(None,None,None,0)
dexarm.move_to(None,None,None,100)
dexarm.move_to(None,None,None,50)
dexarm.move_to(None,None,None,200)
'''
dexarm.close()
```
> The pydexarm_sfci is an adaptation of [the official](https://github.com/Rotrics-Dev/DexArm_API) Python API with a few tweaks for readability. For instance the "dealy_s" becomes delay.

### (X,Y,Z) Min-Max
These are the max and min positions in mm that the robot can traverse, and they are fed into the `.move_to()` function as-is. If you send a command out-of range, the machine will skip that command.
- X: -300 - 300mm
- Y: 230 - 380mm
- Z: -40 - 40mm

### Sliding rail
Note the structure of the Sliding rail demo, by sending the `None` value to (x,y,z), the arm doesn't recieve any new locations to move. The hidden, fourth field is a location from 1-1000mm for the rail to move. The `.sliding_rail_init()` must have been called before a rail position is given.

### Fast Move To
To move at the maximum jog speed for the robot and rail, use the `.fast_move_to` function instead.