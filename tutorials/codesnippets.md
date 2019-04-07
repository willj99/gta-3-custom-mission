# Contents
**CLEO Technique Snippets**
1. Creating FXT Keys
2. Binding a health bar to an actor 
3. Checking if player is on foot and within a sphere's radius
4. Bind marker to location
5. Bind marker to actor
6. Bind marker to pickup
7. Load/Unload Special Actors

# Creating FXT Keys
FXT keys are strings which can be used in game, the FXT format is part of the CLEO library. A key is the ID of a string. They are formatted like this.

        KEY_01 This is FXT key number 1
**An FXT key can have a maximum of 7 characters. [KEY_01]**

## How to create an .FXT file, and create a key (GTA III).
You don't need any software for this, just make sure the game you're working with (GTA 3, VC, SA) has the CLEO library installed. 

- Open up notepad to get started.
- To begin, we'll create a key that overrides an original game key, so that we know it is working without coding the key into the game.
- Type this:
        
        ATUTOR This is a test key. 
- Save the .fxt file with whatever name you like, just make sure the extension is fxt. Save it into your cleo directory in a folder called CLEO_TEXT
- Run the game and get in an ambulance.

We can use our own custom keys when creating scripts and missions. 

## Advantages
The creation of FXT keys is simple and effective. FXT keys are useful because:
- You don't have to overwrite the original game's string keys.
- Easier to edit, don't require special software.
- Part of the CLEO library.

# Binding a health bar to an actor
This tutorial assumes you know how to create an actor within the game's world in GTA III. 

In this example our loaded actor is #LI_MAN1. 

        :initiate_actor // This label creates our actor.
        wait 0
        0247: request_model #LI_MAN1 // Request actor model
        038B: load_requested_model // loads the model
        009A: 0@ = create_actor 12 #LI_MAN1 at 974.8594 -743.8564 14.9727 // create actor, store info in local var 0@
        jump @initiate_health // once code finishes move to the next label.

        :initiate_health //03C4: creates health bar assigned to global variable $HBAR
        03C4: set_status_text_to $HBAR 1 'BARNAME' // 'BARNAME' is the fxt key. e.g.: Enemy
        jump @check_health

        :check_health
        while true // While label is active/true
        wait 0
        0226: $HBAR = actor 0@ health // $HBAR is equivalent to LI_MAN1's health
        if 
            $HBAR < 1 // If his health bar reaches 0
        then
            0151: remove_status_text $HBAR // Remove the bar
            jump @ending // Jump to last label with no operations.
        end

        :ending
        0000: NOP
        
# Checking if the player is within a sphere radius, while on-foot. 
This can be achieved through two ways, we can continually check if the player is in the radius, if not the game won't pass to the next label.

        :create_sphere
        03BC: 01@ = create_sphere 974.8594 -743.8564 14.9727 1.5
        jump @check
        
        :check
        while true
            wait 0
            if
                00FF: actor $PLAYER_ACTOR 0 974.8594 -743.8564 14.9727 radius 2.0 2.0 2.0
            then
                jump @cond_passed
            end
        end   

        :cond_passed 
        0000: NOP
        03BD: destroy_sphere 01@

Or, we can do this through a low-level style syntax like Rockstar. E.g.:

        :oursphere
        03BC: 01@ = create_sphere 974.8594 -743.8564 14.9727 1.5
        jump @condition

        :condition
        wait 0
        if 
            00FF: actor $PLAYER_ACTOR 0 974.8594 -743.8564 14.9727 radius 1.5 1.5 1.5
        jf @condition_notpassed // jf statement means jump if false
        jump @condition_passed

        :condition_notpassed
        wait 0
        00BC: text_highpriority 'GET_IN' 5000 ms 1 // Get in the radius.
        jump @condition

        :condition_passed
        0000: NOP
        03BD: destroy_sphere 01@

It's clear to see the top one is more concise and easier to read. For instances like getting out of a required vehicle, lower level syntax techniques are more suitable.

# Binding markers to locations

# Binding markers to actors

# Binding markers to pickups

# Loading and unloading special actors
