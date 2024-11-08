/*** CycleGen 1.0 *************************************************************\
 * Author: twisted_nematic57                                                  *
 * Special Thanks: Zeroko and Cemetech(.net)                                  *
 * Date: 10/27/2024                                                           *
 * License: GNU GPLv3                                                         *
 * Product Type: Script Template                                              *
\******************************************************************************/

CycleGen is a template TI-68k BASIC program that can be easily modified and run
to produce beautiful animations of 2D and 3D graphs, sort of like how you can
use a slider on Desmos to change a value and see the graph change with it.

Since the program is a ready-to-go script that can be modified just as flexibly
as any other TI-BASIC program, you can literally do anything within the main
loop that is run in between each frame render. You can do crazy things like
animating the presence of asymptotes by finding limits on each and every frame,
and even pre-rendering 3D graphs so that they may be viewed later with very
smooth framerate and zero stuttering. It also has only one assembly subprogram
dependency licensed under the GPLv2 or later, and it should work across the
entire modern 68k series (TI-89/Titanium, TI-92 Plus, and Voyage 200).

It is called CycleGen because you're supposed to use the CyclePic command (built
into the AMS) to display the generated frames as an animation after CycleGen is
done rendering them. You can also use the companion program for CycleGen, called
CycleView, to access a more user-friendly graphical interface that lets you
play, pause, and dynamically adjust speed unlike CyclePic alone. Please see
[Appendix B] for more info.

To handle file saving and memory management, all you have to do is set a couple
of flags (or leave the default ones alone) and let the program's internal code
do the rest! Options include changing file/folder names, offloading storage to
Flash memory if RAM fills up, keeping RAM free and writing every frame to Flash,
or leaving the Flash array alone at all costs in order to ensure its longevity.
Documentation for these flags can be found in [Appendix A].

Error message dialogs are designed to be as user-friendly and descriptive as
possible. In fact they are not even called errors, instead they are called
Oopsies, since that word is less intimidating to the average person and people
will therefore try with a little more determination to fix their own issues.
(Kids these days, man.)

TI calculators are not the fastest at graphing, as you may know, so the typical
run of this program may take a couple of minutes or several hours depending on
how many dimensions your graph has and what kinds of computations go on inside
the loop/during computation of points on the graph.

But even bearing a few computational limitations, I hope you'll find some good
use of this program and discover new things about graphs of functions on the
tried and true TI-68k platform with CycleGen.

 - with passion, twisted_nematic57


I. INSTALLATION

Send `ccg.89p` to your calculator with your favorite linking software. There is
 no harm in keeping it archived, and in fact I recommend archiving it.
It will be in its own folder "AAA" so that you can access it quickly from the
 VAR-LINK menu, but you can move it anywhere else you like and it should be
 fine.

Also, send `flib.89z`. It is an open-source assembly subprogram that exposes an
 interface to BASIC programs allowing manipulation of text in the calculator's
 status line, which is made use of in the script.
This variable MUST be in your `main` folder.
If you wish to view its source code, unzip `flibsrc.zip` and have at it. :)


II. USAGE

 * Copy the file into a different folder, like `main`. Ensure it is not archived
   (so you can edit it).

 * Open the newly copied file with the Program Editor.

 * The script's flag definitions will appear. Modify them as needed.
    - Reference for the flags can be found in [Appendix A].

 * A few lines below the flag definitions you will find the definitions for the
   start, step, and end flags. Modify them as needed.
    - The start flag is a real number that signifies the initial definition of
      the cycling variable in the script.
       - The "cycling variable" is used by CycleGen's main internal loop; it
         keeps track of when to stop cycling. Its name is recur and you may read
         it in your looping code, but please do not modify it.
    - The step flag is a real number that signifies the value that should be
      added to the cycling variable at the beginning of each cycle, i.e. before
      your own looping code is invoked.
    - The end flag is a real number that signifies the point where CycleGen
      should stop launching new cycles and wrap it up.
    - You may leave ONE of the above flags undefined (by commenting out the line
      it was defined on), and the script will automatically calculate the
      missing piece.

 * A couple of lines below the small hunk of code under the flag definitions,
   you will find an area where you can define your global variables.
    - These variables will be stored and operated on in the dedicated folder you
      specified with the outfldr flag.
    - The block of code in which you're supposed to define global variables is
      only run once: at the start of the script.

 * A few lines below the global variable definition area, you will find a
   comment that says "Looping code goes here". Under that comment and before the
   next hunk of code, you can place arbitrary TI-BASIC code that will be run on
   each cycle.
    - Do not modify the variables start, step, end, varcntr, recur, offset,
      memp, fmem, memthres, allowarc, and forcearc.

 * 