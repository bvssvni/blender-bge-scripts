blender-bge-scripts
===================

BGE = Blender Game Engine  

Various scripts for use in the Blender Game Engine.  
![Blender](http://www.blender.org/) is an open source 3D software.  
This repository contains example scenes with internal scripts.  
All scripts are licensed under the BSP license.  

For version number and log, view the individual files.  


##Contributing

If you like to contribute with your own scripts, please follow the standard described in this section.  
For version number, the scripts uses ![Angular Versioning Notation](http://isprogrammingeasy.blogspot.no/2012/08/angular-degrees-versioning-notation.html)  

Please set the view to 'Game Logic' before commiting.  
Put a copy of the BSP license at the top of the script, beneath the version number.  
The manual of how to use the script should follow the BSP license.  

Comments format with BSD license (copy to script):

    # <name of script>.py  0.000
    # 
    # 0.003 <Version change comments>
    # 0.002
    # 0.001
    #
    
    '''
    Redistribution and use in source and binary forms, with or without
    modification, are permitted provided that the following conditions are met:
    1. Redistributions of source code must retain the above copyright notice, this
    list of conditions and the following disclaimer.
    2. Redistributions in binary form must reproduce the above copyright notice,
    this list of conditions and the following disclaimer in the documentation
    and/or other materials provided with the distribution.
    THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
    ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
    WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
    DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR
    ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
    (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
    LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND
    ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
    (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
    SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
    The views and conclusions contained in the software and documentation are those
    of the authors and should not be interpreted as representing official policies,
    either expressed or implied, of the FreeBSD Project.
    '''
    #
    # In:           <What is required to run the script, including dependices>
    # Out:          <What the script does>
    # Function:     <Why and when to use this script + comments in general>
    #

##Using Python Scripts

The scripts here uses 'game properties' extensively for settings.  
Read the script comments beneath the license.  
The document is formatted in this way:

     In: What is required to run the script.  
     Out: What the script does.  
     Function: When to use the script.  

##Running Python Scripts In BGE

Blender uses ![Python](http://www.python.org/) for programming logic at runtime in BGE.  
Python is an interpreted programming language that uses whitespace for block-syntax.  
'Whitespace' is a common word for space, tab and newline.  
Recommended setting in editor is converting tabs into 4 spaces.  

For full API, google 'Blender Python API'.  
The API might change with different versions of Blender.  

Notice that the data used by the BGE is different from the data used in design.  
The game data is organized in the module 'bge'.  

###Sensors

The sensor 'Delay' with 'Repeat' set to true sends a pulse for each update.  
Possible alternative is the 'Always' sensor, but I had troubles with making it work correctly.  

To create an initial pulse that only is triggered at start,  
use an 'Always' sensor with 'Tap' set.  

###Setting Up Controller

This can be connected to a controller that runs a Python script.  
Remember to add ".py" to your script, and set the type to "Module".  
Write the name of the script without the extension, add a dot and the function to call.  
For example:

    follow_strict.main

####AND Gate Behavior

It is a good idea to write Python scripts to behave like AND gates by default.  
The reason for this is that all Boolean expressions can be written in two layers.  
A Boolean expression is a statement that is either True or False:  

    a * b + c * !d
    
    * = AND
    + = OR
    ! = NOT
    
The first layer is the connection between sensors and controller "*".  
The second layer is the connection between controller and actuator "+".  
It is not possible to create actuators in Python, one has to use controllers.  
To emulate an OR gate you can create multiple Python controllers.  

####Interrupting Signals

The reason for making Python scripts behave like AND gates is that "A * !B" equals "A except B".  
This means A is interrupted by B, by setting the B sensor to 'Inverted'.  

Another reason is to have a standard behavior equal for all Python scripts.  

####Example

Here is an example that copies the world position of the owner of controller to the owner of actuators:  

    import bge

    def main(cont)
        # Behave like an AND gate for multiple sensors.
        positive = True
        for sens in cont.sensors:
            positive = positive and sens.positive
            
        if not positive:
            return
    
        own = cont.owner
    
        # Copy position to actuator owners.
        for act in cont.actuators:
            act.owner.worldPosition = own.worldPosition
            
Notice that we use 'return' instead of putting the rest of the code in an if-block.  
This is to save us from an extra level of indention.  
The code is easier to read if you use 'return' statements as early as possible for interruption.  

###Game Properties

The default name for a single game property interacting with a script should be:

    value

For example, one common technique for HUD controls is to use a Python script or Action actuator.  
A single value is read from the owner of the controller and used to update the graphics.  

###Print To Console On Mac

On Mac, start Blender from the Terminal to get the output from "print" statements.  
Open up the 'Terminal' application and type the following:  

    cd ../../Applications/blender.app/Contents/MacOS
    ./blender
    
Some useful functions for debugging:

    dir(obj)    Prints out functions and properties of object or module.
    
    print(msg)  Prints out a message to the console.
    
