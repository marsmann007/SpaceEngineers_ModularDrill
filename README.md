# SpaceEngineers_ModularDrill
 
<img src="DrillLoop.gif" alt="Drill Loop" width="400"/>

"Small" state machine for an automated drill platform  
Commands: without <> symbols:  
<toggle=on> or <toggle=off> ... turns the process on or off

## For the interested: (probably just future me)

The code looks bigger than it is - I could have hardcoded all steps seperatily into the main() routine,
but I decided it would be more readable and better to make my own state machine class.  
With this class it becomes really simple adding or removing states. Every state consists of a condition
that has to be met in order to perform this step and an Execution method, the condition will be checked every time the script
gets executed. If the condition method returns true the state will execute the code in the Perform() block.  
Afterward the programm will move on to the next state and check its condition.  
(There are acctually two seperate state loops in this script - one for inserting new modules and
drilling - and the other one just for building/welding a new module; both loops will wait for the other
one to finish before continuing)


I also made my own modified classes for the Pistons, MergeBlocks and the Connectors.  
Why?  
I wasn't really satisfied with the base classes and I had a few problems with them
(of course my class is build upon the classes of the developers)
For example the merge block has a bollean property that is called "IsConnected".  
If this property is true one would assume the merge block is really connected... but that's not
the case. It takes a few milliseconds for the block to connect even if the property is already set.  
So in my modified class I added a delay to this property.

The Connector class I made is only to make the drilling process a bit more beatiful. 
Acctually there was another reason why I made this class. If a reference to a connector gets saved,
it only lasts as long as the connector isn't connected or disconnected. As soon as we disconnect it
the reference will be invalid and if we try to use it again an exception will occur - took me some time
to find this out :/  
So my class will now save just the id (and the name) of the block and every time we want to send commands
to it we will create a new reference.  
Now to the other part why this class was acctually a good thing to make:  
The connector can already merge with another connector as soon as there is defined minimum distance between them,
they don't have to physically touch each other and that's what I dislike. I don't want to see floating modules
on my drill. Gotta stay realistic :)  

The Piston class makes life a lot easier. I can move the piston to a defined distance with a defined
speed with one command. The class handles everything else for me, like checking if the piston should
be retracted or extendet to reach the destination.
