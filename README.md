blender-bge-scripts
===================

BGE = Blender Game Engine  

Various scripts for use in the Blender Game Engine.  
![Blender](http://www.blender.org/) is an open source 3D software.  
This repository contains example scenes with internal scripts.  
All scripts are licensed under the BSP license.  

For version number and log, view the individual files.  

Please set the view to 'Game Logic' before commiting.  
Put a copy of the BSP license at the top of the script, beneath the version number.  
The manual of how to use the script should follow the BSP license.  

For version number, the scripts uses ![Angular Versioning Notation](http://isprogrammingeasy.blogspot.no/2012/08/angular-degrees-versioning-notation.html)  

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

It is a good idea to write Python scripts to behave like AND gates by default.  
This way, one can interrupt the behavior using other sensors.
Here is an example that copies the world position of the owner of controller to the owner of actuators:

    import bge

    def main(cont)
        own = cont.owner
    
        # Behave like an AND gate.
        positive = False
        for sens in cont.sensors:
            positive = positive and sens.positive
    
        # Copy position to actuator owners.
        if sens.positive:
            for act in cont.actuators:
                act.owner.worldPosition = own.worldPosition
    
On Mac, start Blender from the Terminal to get the output from "print" statements.  
Open up the 'Terminal' application and type the following:  

    cd ../../Applications/blender.app/Contents/MacOS
    ./blender
    
Some useful functions for debugging:

    dir(obj)    Prints out functions and properties of object or module.
    
    print(msg)  Prints out a message to the console.
    
