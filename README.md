# Vroomba: The Magical Cleaning Robot

Congratulations on making it to the end of the YSC2229 module!

This final project is designed for you to demonstrate all your acquired
algorithmic wrangling skills, while attacking a complex optimisation problem,
allowing for multiple possible solutions. I hope, you will enjoy this
experience.

This is a team project. When done, submit your solution to the project by creating a
[tagged release on GitHub](https://help.github.com/en/github/administering-a-repository/creating-releases)
and providing a link to it on Canvas.

## Project Structure

The code is split into three uneven parts.

* `lib` is a collection of the data structures and algorithms we were
  studying in this class. Feel free to use any of them. In general,
  you are not expected to modify any files in this folder, although
  you should feel free to do so, if you think it's necessary. Please,
  remark on your changes in your report.

* `runners` is a folder with a single runner file needed to start your
  suite for Vroomba exercises (more details below). Some template code
  is provided there, and you are expected to extend it to match the
  functionality below. Please, keep the amount of conceptual code
  there minimal and instead put it in the files in the `vroomba`
  folder.

* `vroomba` is a folder, where most of your development will take
  place. It contains a number of files, determining your work split
  and, to some extent, sub-tasks. Some data type definitions and
  functions are provided there as templates. Feel free to add whatever
  definitions you consider necessary. You can also add more files to
  this folder.

In addition, the folder `resources` has several non-code files for you
to try your implementation.

## Your Tasks

The intuition for the project is explained in the 
[lecture notes](https://ilyasergey.net/YSC2229/final.html), and
this document focuses on the technical specification.

Your ultimate goal in this endeavour is to implement a tool suite for
solving Vroomba navigation problems for arbitrary rooms. The suite is
delivered as the executable runner, which, once compiled (into
`bin/vroomba`) can be run from the terminal in multiple execution
modes, depending on the first provided command-line argument. These
are the runner modes and their corresponding additional arguments:

* ```bin/vroomba solve input_file output_file```

  In this mode, the runner takes a file with the path `input_file`
  containing a number of room encodings, each on a new line, and
  produces a file `output_file` of solutions (move strings), one per
  line.

  An example input file with 10 medium-size problems can be found in
  `resources/rooms.txt`. A result of solving it should be a file with
  10 strings of Vroomba moves (i.e., each string is something like
  `WWDDDDDSSSAAADDDWWWA` but probably longer).

  As another examples the files `basic.txt` and `basic.sol` under
  `resources` show an examples of a valid input/output with a single
  room.

  **Your solver must be fast**: it should take seconds (not even tens of
    seconds) on the problems of the size of those from `rooms.txt`. 

* ```bin/vroomba check input_file solutions_file```

  In this mode, the runner takes a file with the path `input_file`
  with rooms and a file with the path `solutions_file` containing
  solutions and matches, line by line, that each solution in
  `solutions_file` is a valid solution to a room in `input_file` from
  the corresponding line. Empty lines at the end and at the middle of
  the files should be ignored.
  
  This mode should output to the terminal the statistics for the
  solution for the corresponding problems. For instance for a pair
  4-line room/solution files, where the solutions for rooms 1, 2, and
  4 are correct, and the one for 3 is wrong, the output should be
  as follows:

  ```
  1: 241
  2: 23
  3: Fail
  4: 12
  ```
  
  In the successful cases, the numbers (e.g., `241`) denote the length
  of the list of moves in the solution. You can use this mode to check
  your solution produced by the `solve` mode.


* ```bin/vroomba generate num size output_file```

  This mode generate `num` rooms of the size `size` (the span along
  either of the dimensions) and write them into `output_file`, one per
  line. The result file should have a similar format as
  `resources/roooms.txt`.
  

* ```bin/vroomba play input_file output_file```

  This mode reads all the rooms from the file `input_file` and starts
  a series of "games". Each game allows the user to solve the room by
  navigating Vroomba in a room, recording the user's moves in the
  background. Once a room is fully "sweeped", a next room from the
  sequence is displayed, if any. Once all games are complete, the
  resulting sequences of the moves for each room are written to the
  `output_file`. The resulting should be possible to check against
  `input_file` via the `check` mode of the runner.

  The user should use the keys `w`, `a`, `s` and `d` to control the
  movements of Vroomba on a screen and `q` to exit the game (in this
  case the results of all complete games should be saved to the file).

  There is no specific requirement on how the game should look. The
  design of the user interface and possible enhancements are left up
  to your discretion.

A large part of this project is reserved for testing the various
subroutines of your suite (testing of the graphical mode should be
done manually). Some initial tests are provided in the files under the
`vroomba` folder, commented out. Make sure that they pass once your
implementations are complete. Add more tests checking various
properties of our room representations.

## Tips and Hints

### Workload split

This is a complex project and the good separation of tasks is a key to
frustration-free in-team communication. To begin the project, discuss
the following components as a team:

* What should be the type `room` in `Rooms.ml` and how it's going to
  be treated.

* What should be the type `state` in `RoomChecker.ml` and how the
  checker and the solver are going to use its values.

After that, depending on a number of students in your team, you can
split the design and implementation workload based on the files in the `vroomba`
folder. I suggest the following strategies, but please, feel free to
adapt them for your team.

**2-person team:**

1. Test room generation (`RoomGenerator.ml`); solving rooms (`RoomSolver.ml`).
2. Task room parsing (`Rooms.ml`); testing room solutions
   (`RoomChecker.ml`); game rendering (`RoomRendering.ml`).

**3-person team:**

1. Task room parsing (`Rooms.ml`); solving rooms (`RoomSolver.ml`).
2. Test room generation (`RoomGenerator.ml`); testing room solutions
   (`RoomChecker.ml`).
3. Game rendering (`RoomRendering.ml`).

### Implementation gotchas

* Vroomba cannot reach the floor surface "around the corners". That is, if you
  cannot build a segment that `does not touch walls or corners` between the
  center of Vroomba's position square and some square S in its reach, it won't
  be able to clean S (unless it moves closer to it). For instance, Vroomba ``V``
  cannot clear the square ``S`` in the image below, because of the wall ``W``,
  so it needs to move up to reach ``S``:
  ```
   [ ][S]
   [V][W]
  ```
 
* For the gamification part, make sure to revise
  [this simple project](https://github.com/ysc2229/ocaml-graphics-demo)
  and salvage whatever you consider useful for your needs.

* Useful string-processing functions are available in the library
  files `ReadingFiles.ml`. Also, check how the graphs are parsed.

* You can enhance the "game" part of the project to visualise and animate your
  solutions. This is not required for completion, but since you'll
  probably end up doing it anyway, use the following runner mode:

  ```
  bin/vroomba render input_file output_file
  ```

  where `input_file` and `output_file` provide the rooms/solutions
  correspondingly. Once done with each individual solution, stop the
  animation, and proceed to the next one from the file if the key `n`
  is pressed. Feel free to play with the delays to choose the optimal
  animation mode.

* Even before you implement the advanced visualiser for your problems,
  you might want to be able to print a given `state` of your solution
  checker in the terminal, similarly to how we displayed chess boards
  in the 8-queen problem when studying backtracking.

* Since the move sequences for Vroomba are not coordinate-specific but
  are position-relative, your `room` data type does not have to keep
  the exact coordinates of the room polygon in space. Use this fact to represent
  it via a convenient data structure.

* Many aspects of this project are open-ended and you are encouraged
  to reflect on how you can improve on the project part that you are in
  charge of. For instance:
  
  * What could be a strategy to find better solutions for Vroomba for arbitrary rooms?
  * How to generate more "interesting" rooms for testing?
  * What could make the game even more fun to play?

* If, in your visualisation, the frame is "flickering" with every change, it is
  most probably due to your implementation having to redraw it from scratch every
  time. This makes the user experience not so pleasant. To avoid this, consider
  redrawing only *the part of the frame affected by the latest changes* in the room's
  state.
  
* Make sure that all your components: generator, solver, checker, were tested in
  tandem, viciously. For the purpose of testing, I will compile a test suite 
  containing a number of rooms generated by each of the teams' implementations and use it for
  testing *all* submitted solvers. **Feel free to exchange text files with randomly generated room descriptions** 
  (but not your program code!) **with other teams** to make sure your solver is
  bullet-proof with regard to others' inputs, not just your own.

### Comparing the quality of the solutions

Since solving Vroomba navigation is an NP-hard problem (it is from the
same class of algorithmic problems as SAT), you are not expected to
deliver *the best* solutions for any room, only a correct one.
However, it's always interesting to see if we can do better.

To stimulate the research on better solving strategies, there will be
an informal competition amongst the teams, with the ranks allocated
according to the overall better algorithmically obtained results for
the ten special rooms (in `resources/rooms.txt`) from the file above.
The winners will be announced on Canvas and will be granted the right
to openly brag about it. No additional points will be awarded for
winning the competition.

## Assessment and Grading

This is how I am going to test your implementation of this project:

1. Running your solver on the provided rooms
2. Checking *my* solution for provided rooms using *your* checker
3. Checking *your* solution for provided rooms using *my* checker
4. Generating your own rooms (2 rooms, 30x30 size); visually assessing their quality.
5. Solving your generated rooms using your solver
6. Checking solution for your generated rooms using your checker
7. Checking solution for your generated rooms using my checker
8. Messing up your solution (removing steps) for your generated rooms and checking it using your checker (it should fail)
9. Playing the game on your generated rooms obtaining complete solutions. 
10. Visually assessing the quality of the UI and UX.
11. Checking the solutions obtained manually via the game, using your checker.
12. Running your solver on random rooms from other teams; testing your solutions using my checker.

Make sure to try the same steps before submitting!

I will be checking GitHub repositories to ensure that all members of the team
have contributed a substantial amount of non-trivial code to the implementation.
In the case if one's contributions are considered minor, a penalty will be applied
to their score for this task.

**Points for the task**

* Room Solver:
  * correctness: 2
  * performance: 2
* Room Generator: 2
  * correctness
  * aesthetic properties of the generated rooms
* Room Checker:
  * correctness & performance: 2
* Tests for Generator/Solver/Checker: 1
* Rendering and gamification: 3
  * performance of the animation rendering 
  * good use of geometric scaling
  * correctness of the representation
  * overall user experience


**Good luck!**
