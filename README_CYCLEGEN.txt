/*** CycleGen 2.0 *************************************************************\
 * Author:         twisted_nematic57                                          *
 * Special Thanks: Zeroko and Cemetech(.net)                                  *
 * Date:           12/29/2024 [MM-DD-YYYY]                                    *
 * License:        GNU GPLv3 (or later) - see LICENSE                         *
 * Product Type:   BASIC program                                              *
 * Platform:       TI-89 Titanium - can be ported to other Motorola 68000     *
                   based TI calculators with ease.                            *
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

A bundled program called CycleView allows you to play back the generated frames
as an animation and scrub through individual frames. The operating system's
built-in CyclePic command can also be used, but it has some limitations. See
README_CYCLEVIEW.txt for more information and comparisons between CycleView and
CyclePic.

To handle file saving and memory management, all you have to do is set a couple
of flags (or leave the default ones alone) and let the program's internal code
do the rest! Options include changing file/folder names and precise management
of the limited-capacity RAM and limited-write Flash chip. Thanks to new features
in Version 2.0, you can also choose to render as many frames as will fit in
either or both forms of memory.

Error message dialogs are designed to be as user-friendly and descriptive as
possible. In fact, they are not even called errors; instead they are called
Oopsies, since that word is less intimidating to the average person and people
will therefore try with a little more determination to fix their own issues.
(Kids these days, man.) See Section IV for documentation on the various causes
and solutions of each Oopsie situation.

TI calculators are not the fastest at graphing, as you may know, so the typical
run of this program may take a couple of minutes or several hours depending on
various factors like the number of dimensions in the graph, computational
complexity in the main loop, and the number of frames to be rendered.

But even bearing a few computational limitations, I hope you'll find some good
use of this program and discover new things about graphs of functions on the
tried and true TI-68k platform with CycleGen.

 - created and refined with passion.


I. INSTALLATION

All files being referred to in this section are in the `distribution` folder.

Send `ccg.89p` to your calculator with your favorite linking software. It should
 be archived.
It will be in its own folder `AAA` so that you can access it quickly from the
 VAR-LINK menu, but it's fine to store it anywhere.

Also, send `Flib.89z`. Leave it archived.
It is an open-source assembly subprogram that exposes an interface to BASIC
 scripts allowing manipulation of text in the calculator's status line, which is
 made use of in CycleGen.
This variable MUST be in the `main` folder.
If you wish to view its source code, unzip `flibsrc.zip` and have at it. :)
The license included with Flib is in `flib-license`, and also inside the ZIP
 itself (in `GPL.txt`).


II. PROGRAM FLOW

The program is centered around interpolating a variable between two endpoints
and producing images of graphs at every keyframe.

 * At the start of the script, you define _flags_. Flags change the behavior of
   script by telling it how it should manage memory, format output, etc. You
   also define global variables used in your own calculations.
 * The user-defined "looping code" is a chunk of TI-BASIC code that is executed
   once per cycle before graphing begins. ANYTHING that can be done in a
   standalone BASIC script can be done in this loop, as long as it does not
   modify CycleGen's internal variables. (See line 186 of this document.)
 * After the user-defined looping code is done running, CycleGen makes the
   calculator regenerate the graph and save a screenshot.
 * The screenshots are saved in the format that animation player programs
   expect: a string prefix, followed by an integer that denotes the frame number
   (to signify the screenshot's "place" in the animation).


III. USAGE

 * Make a copy of the `ccg` variable. Ensure the copy is not archived.

 * Open the newly copied file with the Program Editor.

 * The script's flag definitions will appear. These are settings that are
   accounted for in the script's internal code to alter its behavior. Modify
   them as needed.
    - `outfldr`: A 1 - 8 character string that specifies the name of the folder
                 the frames should be saved to. (Your global variables will be
                 stored and looping code will be run in this folder.)
    - `varprfx`: A 1 - 5 character string that specifies the prefix of the saved
                 pictures' names. E.g., a `varprfx` of "pic" will cause the
                 output sequence of frames to be named "pic1", "pic2", "pic3",
                 etc.
    - `ovrwprev`: If set to 1, this allows the script to overwrite pre-existing
                  frames, if there are any. If set to 0 and there are frames
                  with names that match the ones that need to be saved, the
                  script will exit with an error.
    - `allowarc`: If set to 1, allows the script to write frames to Flash memory
                  if available RAM is insufficient. If set to 0, then the script
                  will exit with an error if enough RAM is not available.
                   - The Flash chip in the TI-89 Titanium is SLC, meaning it
                     should endure at least 100,000 writes to every cell.
                     However, keep in mind that not all cells are ultimately
                     used to store variables, as the OS is rarely overwritten
                     and wear leveling may not be implemented.
                   - Within reason, you should be able to safely set this flag
                     to 1 for many thousands of runs; but ALWAYS CONSIDER
                     setting it to 0 if it is feasible to do so.
    - `forcearc`: If set to 1, makes the script write frames to Flash memory
                  regardless of the amount of available RAM. Takes precedence
                  over `allowarc`.
                   - This flag should only be set to 1 when there is very little
                     RAM free, or if it absolutely must be preserved for another
                     use.
    - `fillmem`: If set to 1, this flag makes the script fill available memory
                 with as many frames as will fit. `allowarc` and `forcearc` are
                 accounted for; therefore this flag can let CycleGen fill up
                 RAM, Flash/Archive, or both.
                  - This flag assumes that all images will have a size of 1547
                    bytes, which is true on the TI-89 Titanium in full-screen
                    mode. This value is hardcoded in the script; it is not
                    designed to be used in split-screen modes.
    - `memthres`: A positive integer (>= 8192) that denotes the minimum amount
                  of RAM, in bytes, that CycleGen must leave free after
                  terminating. As a general rule of thumb, one should have at
                  least 8192 bytes free to avoid running out of memory when
                  saving frames or calculating points on the graph(s).
                   - Keep in mind that generated frames are always saved to RAM
                     first, and then they're archived. This is just due to
                     limitations in the calculator's OS.
    - `arcthres`: Basically `memthres` but for Archive/Flash space, with one
                  exception: this flag is ignored when `fillmem` is set to 0.
    - `maxsteps`: A positive integer between 1 and 999 that signifies the number
                  of frames CycleGen should generate.
                   - If `fillmem` is set to 1, this flag has no effect.

 * Below the first block of flag definitions, you'll find settings for the
   `start`, `step`, and `end` flags. These control the behavior of the dynamic
   variable, which progresses from the starting value to the ending value in
   increments of `step`. (The dynamic variable is expected to be defined and
   managed by the user in the looping code.) Modify them as needed.
    - `start`: A number that signifies the starting value of the interpolation.
    - `step`: A number that signifies the interpolation step.
    - `end`: A number that signifies the ending value of the interpolation.
    - You may leave ONE of the above flags undefined by commenting out the line
      it was defined on, and the script will automatically calculate the missing
      piece.
    - The above flags are there only to make the script easier to use for you.
      You still have the freedom to manipulate your own variables however you
      like in your looping code. (See line 186 of this document to get to know
      the few variable-related restrictions you have.)
    - The dynamic variable on the first frame should equal `start` + `step` (but
      not `start` itself) because incrementation occurs before graphing.
      However, it should equal `end` at the last frame.

 * A couple of lines below the small chunk of code under the flag definitions,
   you will find an area where you can define your global variables.
    - These variables will be stored and operated on in the dedicated folder you
      specified with the `outfldr` flag.
    - The block of code in which you're supposed to define global variables is
      only run once: at the start of the script.
    - Just like the looping code, this is a regular block of TI-BASIC. You can
      change graphing behavior, set modes, define equations to be graphed,
      change visual styles, etc.

 * A few lines below the global variable definition area, you will find a
   comment that says "Looping code goes here". Under that comment and before the
   next chunk of code, you can place arbitrary TI-BASIC code that will be run on
   each cycle.
    - Do not modify the variables `memp`, `maxsteps`, `recur`, `varprfx`,
      `memthres`, `allowarc`, `forcearc`, or `err`.
       ~ If any of the above are modified, the program may behave erratically.
         It can result in anything from weird-looking output to crashes.
         However, no serious damage should occur to the calculator's OS or
         filesystem.

 * Go back to the Home Screen and execute the modified version of CycleGen you
   just closed. CycleGen will take a second to fully initialize, and promptly
   begin rendering cycles.
    - The status bar will contain information about the number of frames that
      have already been generated out of how many it will generate in total.

 * After CycleGen has finished generating frames, the calculator will return to
   the Home Screen and terminate the program.

 * To view the rendered frames as an animation, refer to README_CYCLEVIEW.txt
   for information on how to use CycleView or consult the TI-89 Titanium User
   Manual for documentation on the CyclePic instruction.

DISCLAIMER: Treating the program in ways that deviate from the specification
above may cause unexpected and erratic behavior. No damage should ever occur to
your calculator by using this program, but the author(s) of this program do not
claim liability for ANYTHING should undesirable events occur as a result of the
distribution, storage, compression, or execution of this program.


IV. OOPSIE DOCUMENTATION

Each cause/fix pair is matched by numbering. For Oopsies that only have one
reasonable cause, there may be multiple independent fixes, and vice versa.

 * #1 and #5: Not enough RAM is free in the system.
    - CAUSE: 1) you may have too many variables unarchived; 2) too many Flash
             applications installed; 3) something may have sprung a memory leak.
    - FIX: 1) Archive more variables; 2) uninstall some Flash applications; 3)
           reset the calculator's RAM after archiving the script.

 * #2: `maxsteps` flag is <= 1.
    - CAUSE: 1) you manually set it too low; 2) there was not enough RAM or
             Flash space to store more than 1 frame.
    - FIX: 1) manually set the variable to a higher value; 2) free up some RAM
           or Flash space, depending on the values of `allowarc`/`forcearc`.

 * #3: newly generated frames cannot be saved due to a variable name clash.
    - CAUSE: some variables under the exact same name and folder already exist
             in the calculator's filesystem, and `ovrwprev`=0, disallowing the
             script from removing them.
    - FIX: 1) manually delete the old variables; 2) change `varprfx`; 3) change
           `outfldr`; 4) set `ovrwprev` to 1.

 * #4 and #6: a frame could not be archived.
    - CAUSE: 1) there is not enough Flash space to save the frame; 2) the
             calculator's batteries are too low to complete a Flash write.
    - FIX: 1) free up some Flash space; 2) replace or charge the batteries.


V. CHANGELOG (LATEST-FIRST)

 * CycleGen 2.0: Can take full advantage of all available memory; More
                 case-specific messages in status bar after imperfect
                 termination; Bundles CycleView
    - CODE: add fillmem flag, add code to handle it
    - CODE: introduce arcthres flag
       ~ does not have an effect when fillmem=0, it's just illogical
    - CODE: add error-specific messages to status bar after imperfect (not
            normal) exit
    - CODE/DOC: fix issue showing recommended free RAM values to be 1024x their
                true size
    - DOC: reword some phrases for more clarity
    - DOC: remove examples, too tedious
    - MISC: add plaintext form of source code for curious people without TI-89Ts
            or emulators

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