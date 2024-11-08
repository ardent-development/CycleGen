# To-Do List

## CycleGen

- [X] Test memthres flag
- [X] Revise Oopsie dialogs
- [X] Test maxsteps flag
- [X] Remove the autocompute flag and automatically detect if it's required
- [X] Extend the autocompute system to include auto-calculation of step value based on start, end, and maxsteps
- [X] Create an offset flag (output var name numbers added to offset on StoPic)
- [X] Test offset flag
- [X] Restructure the beginning of the script to make it more user-friendly
- [X] Remove declarations of unused variables (oops)
- [ ] Finish documentation

## CycleView

- [ ] Do initial RplcPic test (check: is this feasible?)
- [ ] Toolbar implementation
  - [ ] File
    - [ ] New (clears the graph screen, lets user 'bring their own' pics)
    - [ ] Open... (dialog for opening an existing script and figuring out details from there)
    - [ ] About... (dialog showing about screen: CycleGen & CycleView, publication date, free RAM, License, Ardent Development, license)
  - [ ] Settings
    - [ ] Playback speed... (Changes how fast pics should be cycled)
    - [ ] EOS [end-of-sequence] Handling
      - [ ] Stop (just stop playback at end)
      - [ ] Loop (play again from beginning)
      - [ ] Bounceback (play in reverse order when at either end)
    - [ ] Manual Sequence Override... (...)
