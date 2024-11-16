/*** CycleGen 1.1 *************************************************************\
 * Author: twisted_nematic57                                                  *
 * Special Thanks: Zeroko and Cemetech(.net)                                  *
 * Date: 11/15/2024 [MM-DD-YYYY]                                              *
 * License: GNU GPLv3 (or later) - see LICENSE                                *
 * Product Type: Script Template                                              *
 * Platform: TI-89 Titanium: TI-BASIC                                         *
\******************************************************************************/

CycleGen is a template TI-89 Titanium BASIC program that is designed to be
easily modified and run to produce beautiful animations of 2D and 3D graphs,
sort of like how you can use a slider on Desmos to change a value and see the
graph change with it.

Since the program is a ready-to-go script that can be modified just as flexibly
as any other TI-BASIC program, you can literally do anything within the main
loop that is run in between each frame render. You can do crazy things like
animating the presence of asymptotes by finding limits on each and every frame,
and even pre-rendering 3D graphs so that they may be viewed later with very
smooth framerate and zero stuttering. It also has only one assembly subprogram
dependency licensed under the GPLv2 or later.

It is called CycleGen because you're supposed to use the CyclePic command (built
into the AMS) to display the generated frames as an animation after CycleGen is
done rendering them.

To handle file saving and memory management, all you have to do is set a couple
of flags (or leave the default ones alone) and let the program's internal code
do the rest! Options include changing file/folder names and precise management
of the limited-capacity RAM and limited-write Flash chip.

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

All files being referred to in this section are in the `distribution` folder.

Send `ccg.89p` to your calculator with your favorite linking software. I
 recommend archiving it.
It will be in its own folder "AAA" so that you can access it quickly from the
 VAR-LINK menu, but you can put it anywhere else you like and it should be fine.

Also, send `Flib.89z`. Leave it archived.
It is an open-source assembly subprogram that exposes an interface to BASIC
 scripts allowing manipulation of text in the calculator's status line, which is
 made use of in CycleGen.
This variable MUST be in your `main` folder.
If you wish to view its source code, unzip `flibsrc.zip` and have at it. :)
The license included with Flib is in `flib-license`, and also inside the ZIP
 itself (in `GPL.txt`).


II. DETAILS

 * The cycling architecture is built in a way that is primarily centered around
   interpolating a variable between two endpoints and producing images of graphs
   at every keyframe.
 * At the start of the script, you define flags. Flags change the behavior of
   script by telling it how to manage memory, format output, etc. You also
   define global variables used in your own calculations.
 * The user-defined "looping code" is a chunk of TI-BASIC code that is executed
   once per cycle before graphing begins. ANYTHING that can be done in a
   standalone BASIC script can be done in this loop, as long as it does not
   modify CycleGen's internal variables. (See line 152 of this document.)
 * After the user-defined looping code is done running, CycleGen makes the
   calculator regenerate the graph and save a screenshot.


III. USAGE

 * Copy the file into a different folder, like `main`. Ensure the copy is not
   archived (so you can edit it).

 * Open the newly copied file with the Program Editor.

 * The script's flag definitions will appear. These are sort of like "settings"
   that are accounted for in the script's internal code to alter its behavior.
   Modify them as needed.
    - `outfldr`: A 1 - 8 character string that specifies the name of the folder
                 the frames should be saved to. (Your global variables will be
                 stored and looping code will be run in this folder.)
    - `varprfx`: A 1 - 5 character string that specifies the prefix of the saved
                 pictures' names. E.g., a `varprfx` of "pic" will cause the
                 output sequence of frames to be named "pic1", "pic2", "pic3",
                 etc.
    - `ovrwprev`: If set to 1, allows the script to overwrite pre-existing
                  frames, if there are any. If set to 0 and there are frames
                  with names that match the ones that need to be saved, the
                  script will exit with an error.
    - `allowarc`: If set to 1, allows the script to write frames to Flash memory
                  if available RAM is insufficient. If set to 0, then the script
                  will exit with an error if enough RAM is not available.
    - `forcearc`: If set to 1, makes the script write frames to Flash memory
                  regardless of the amount of available RAM. Takes precedence
                  over `allowarc`.
    - `memthres`: A positive real integer (between 8192 and 192512, inclusive)
                  that denotes the minimum amount of RAM, in kilobytes, that
                  CycleGen must leave free after executing. As a general rule of
                  thumb, one should have at least 8192K free to avoid running
                  out of memory when saving frames or calculating points on the
                  graph(s).
    - `maxsteps`: A positive real integer between 1 and 999 that signifies the
                  maximum number of frames CycleGen will generate.

 * Below the first block of flag definitions, you’ll find settings for the
 `start`, `step`, and `end` flags. These control the behavior of the dynamic
 variable, which progresses from the starting value to the ending value in
 increments of `step`. Modify them as needed.
    - `start`: A real number that signifies the initial definition of the
               dynamic variable in the script.
    - `step`: A real number that signifies the value that should be added to the
              dynamic variable at the beginning of each cycle, i.e. before your
              own looping code is invoked.
    - `end`: A real number that signifies the value of the dynamic variable at
             the last rendered frame.
    - You may leave ONE of the above flags undefined (by commenting out the line
      it was defined on), and the script will automatically calculate the
      missing piece.
    - The above flags are there only to make the script easier to use for you.
      You still have the freedom to manipulate your own variables however you
      like in your looping code. (See line 152 of this document to get to know
      the few variable-related restrictions you have.)
    - The dynamic variable starts from `start` + `step` (and not `start` itself)
      because incrementation occurs before graphing. However, it will equal
      `end` at the last frame.

 * A couple of lines below the small chunk of code under the flag definitions,
   you will find an area where you can define your global variables.
    - These variables will be stored and operated on in the dedicated folder you
      specified with the `outfldr` flag.
    - The block of code in which you're supposed to define global variables is
      only run once: at the start of the script.

 * A few lines below the global variable definition area, you will find a
   comment that says "Looping code goes here". Under that comment and before the
   next chunk of code, you can place arbitrary TI-BASIC code that will be run on
   each cycle.
    - Do not modify the variables `start`, `step`, `end`, `recur`, `memp`,
      `fmem`, `memthres`, `overwprev`, `allowarc`, `forcearc`, and `maxsteps`.

 * Go back to the Home Screen and execute the modified version of CycleGen you
   just closed. To fully initialize, CycleGen will take a couple seconds, and
   promptly begin rendering cycles.
    - The status bar will contain a message saying how many frames are to be
      rendered and how many have been rendered so far.

 * After CycleGen has finished, it will return to the Home Screen and exit.

 * To view the rendered frames as an animation:
    - Switch to the folder where you saved the frames.
    - On the Home Screen, type `CyclePic "str",num,delay,times,reverse`, where:
       - `str` is the prefix string of the pictures you specified in the script.
       - `num` is the number of frames you want to play back. (So if num=50,
               CyclePic will play back frames 1-50. There is no way to change
               the starting point.)
       - `delay` is the amount of time, in seconds, each frame is displayed. I
                 recommend a value of 0.1, generally.
       - `times` is the number of times the animation should repeat.
       - `reverse`, if set to -1, will play back the animation in reverse when
                    it has finished playing. If this one is left undefined and
                    the animation has to play again, it will start from the
                    beginning.
    - You can press the ON key at any time during playback of the animation to
      stop it.


IV. EXAMPLE

Flags:
  "ctmp"→outfldr
  "p"→varprfx
  1→ovrwprev
  1→allowarc
  0→forcearc
  8192→memthres
  10→maxsteps

  0→start
  0.3→step
  [End undefined]

Global variables:
  start→q

Looping code:
  q+step→q
  PxlText string(q),69,0

The above example will make CycleGen output 10 frames in a folder called `ctmp`
and add a "p" character before the frame numbers in their filenames. It will
overwrite any previous frames, if they exist. The script will not run without
<8192K of RAM free; and if it does run out of RAM while cycling, it will archive
all frames past the point where RAM capacity was exhausted. However, it will
make as much use of available RAM as possible.

There will be a global variable `q` inside the folder `ctmp`. It is initially
defined as the `start` flag.

In the looping code, the step value is added to `q` and `q`'s value is displayed
on the graph screen. Then, graphing occurs and the frame is saved.

After all frames have been saved, CycleGen returns to the `main` folder and
exits.

Other examples may be found in the `examples` directory. They are in the form
of video files showing off the content of the scripts, then generating and
playing back the animations.


V. CHANGELOG (LATEST-FIRST)

 * CycleGen 1.1: Bugfixes
    - CODE [#1]: Rework the core `for(` loop to work on internally defined
                 integers instead of user-defined floats.
    - DOC: Revise formatting and informational errors in documentation (mostly
           relating to the header and `memthres` flag's behavior)
    - DOC: Add more info about `start`,`step`,`end` flags and clarify their
           behavior
    - CODE: Automatically delete `start`,`step`,`end` flags upon initialization
            (fixes issues with re-runs after incomplete runs)

 * CycleGen 1.0: Initial release