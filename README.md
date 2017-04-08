![alt text](https://github.com/rb-kuddai/cav/blob/master/cav_hw_anim.gif)

# Computer Animation and Visualisation Assessment 1 - Skinning
Ruslan Burakov, student id: s1569105

## Note
The only extra include which I use is sstream (must be included in c++ standard library). 
If something is not running properly, please email (s1569105@ed.ac.uk) me because it was running ok 
on DICE and probably there is something wrong with the current user interaction. 

## Implemented features
* Skinning with Linear Blending
* Animation Keyframe Interpolation
* Walking and Running Animations Motion Blending
* User Interaction/Keyframing

All of my code (except joint transforms in the skeleton class) is located in the Skinning.cpp. I compute 
walk and run animations global transforms in advance in order to save CPU (and local rest pose transforms, 
as well). The code section responsible for that starts with "UTILS TO STORE WALK AND RUN TRANSFORMS IN ADVANCE". 

After that, code for "Skinning with Linear Blending" is located under "MESH WEIGHT LINEAR BLENDING PART". 
I normalise blending weights so they sum to one.

Animation Keyframe Interpolation code is located under "UTILS FOR LINEAR INTERPOLATION" and is used 
in the skeleton rendering ("SKELETON DRAWING FUNCTIONS") and mesh rendering. 

Blending between walking and running animations code is under "WALK AND RUN BLENDING PART". Blending 
between walking and running animations is done based on the paper:
> Kovar, Lucas, and Michael Gleicher. "Flexible automatic motion blending with registration curves." Proceedings of 
> the 2003 ACM SIGGRAPH/Eurographics symposium on Computer animation. Eurographics Association, 2003.

From this paper, I use the fact that in our case walking and running animations have the same root 
transform. My slope limit parameter to compute optimal time wrapping is equal to 2. If you run the code
via ./skinning in console, then the distance between frames of walking and running clips will be printed 
(note: this is a big table. The full screen terminal window in the Drill Hall DICE machine was able to fit it). 

After that, I find the increasing sequence of frames with the lowest distances (to sync footsteps, for example) 
and prune it in order to find a seamless animation loop (looped motion). 

User interaction features are spread across many functions but most of them are located in the KeyEvent 
handler. Basically, a user can control the animation speed, mixture the ratio between walking and running, choose a 
particular frame, switch between animations, and skeleton or mesh rendering. See more in controls section.

Different small utils were implemented to display user hints (code under "UTILS FOR FORMATTING/DISPLAYING").

Also, a small initial memory leak was fixed (in the original code provided to us the walk_animation 
wasn't released).

## Controls (keyboard)
##### Switching between animation clips
* r - switch to running animation (default one)
* w - switch to walking animation
* b - switch to blending between walking and running animations
##### Choosing the view mode
* s - switch to skeleton view
* m - switch to mesh view (default one)
##### Controlling frames
* f - switch frame mode. Either animation live (default one) or keyframe mode
* j - decrease the animation speed or choose a previous frame depending on the frame mode
* k - increase animation speed or choose next frame depending on frame mode
##### Controlling Walking and Running Animations Motion Blending
The following changes are visible only when blending between walking and running animations is 
chosen (b is pressed on the keyboard):
* z - change animation closer to walking
* x - change animation closer to running
##### Animation Keyframe Interpolation
In order to see the difference, the animations speed must be decreased (j on keyboard) to 5, for example.
* i - enable (default one) or disable Animation Keyframe Interpolation. 


## Running. Suggested workflow for grading.
##### Note
Just in case, ensure that Caps Lock is disabled. It should work with Caps Lock as well (in the code, I use 
cases for both lower and upper letters) but I didn't test it enough with Caps Lock enabled. If you are 
somehow, feel lost in different modes, try to restart the program or look into the control section for 
guidance (or email me).

Run ./skinning

You should see the character running with text "Default Mode" in the left bottom corner. 

Try to switch between different animation clips by pressing 'r' or 'w' on the keyboard (don't press 'b' 
yet, we will reach this stage later).  Try different view modes by pressing 's' or 'm'.

Restore to running animation again (press 'r') and mesh view (press 'm'). 

Next. In order to see the difference between Animation Keyframe Interpolation ON and OFF, decrease the animation 
speed to 5 by pressing 'j' (if you decreased too much, just press 'k'). Disable or Enable  Animation Keyframe Interpolation by pressing 'i'. 

Restore Animation Keyframe Interpolation to ON again. Increase the animation speed to 40 again by pressing 'k'. 

Press 'b' to see the blending between walking and running animations. Try different blending ratios by pressing 
'z' or 'x'. You should see how character movements are gradually changing between running and walking. In order 
to see the changes more smoothly, try to switch into the skeleton view mode (press 's'). After that, staying in the blended 
walk/run clip  press 'f' to enable the keyframe mode. Change frames by pressing 'j' or 'k'. You should see different 
animations keyframes. For some fixed frame try to change blending ratios once more by pressing 'z' or 'x' in order 
to see the change in more details. Try to switch back to a mesh view (press 'm') and see the difference ('z' or 'x') 
for some fixed frame.

Finally, try to play with different modes (press 'f' once more to enable live). Look into the control section 
for guidance. 


