# boggle-ish
Creates a number-based Boggle board, then finds its solutions

This design process has been a very long, but enjoyable, series of epiphanies.  I basically completed the problem twice, using two completely different methods.  I’m going to explain the important decisions that I made, including why I decided to start over and why this was a good idea.  
I’ve made games in Java before, so I decided early-on that I would use this language again.  My first realization was that it would be extremely helpful to make each tile on the board its own object with x and y coordinates as well as a value; this way I could compare them to each other when checking for solutions.
I hit a roadblock when trying to figure out how to find all the solutions.  This is where I made a poor decision on my first try and where I came back to on my second try.  I wanted to make a sort of AI that could find all the solutions, but couldn’t come up with a way to execute this.  At some point, it hit me that I could just compute every single combination of the numbers on the board, and then screen these based on the requirements of having adjacent cells, adding up to the area of the board, not repeating cells, etc.  This approach works for a 3x3, but not if you scale the program up (as I discovered later on).
One major bug I ran into is when I check if cells are adjacent to each other.  My original method was just to check if each cell in a chain is connected to at least one other tile.  This is fine, except for when you have 4 or more cells in the chain.  For example, when checking the numbers 3420, the 3 and 4 could be adjacent to each other, but not near the 2 and 0 which are adjacent to each other.  So, the program would say that 3 is attached to 4, 4 to 3, 2 to 0, and 0 to 2, thinking this is a viable path based on the fact that each cell is connected to at least one other cell.  I realized that another way to check if a chain is viable is that the number of cells that must have at least TWO connections is the length of the chain minus two (minus two, because the endpoints only need one connection, but every other tile must be connected on two ends).  So, I then created an int array of the number of connections each tile in a chain has, and make sure at least size() – 2 elements are greater than or equal to 2, and that none of the elements are zero.  If none of that made sense, it doesn’t matter, because it all changed anyway.
At this point, I thought I was done and focused on the visuals for a bit.  From the beginning, I wanted to make a JavaFX GUI, just because I think they’re nicely minimalistic and I have some experience with them.  One major component that I thought would be cool, and that I knew would also be helpful for debugging, is highlighting each solution path on the board.  I did this very carefully and it surprisingly worked; I’m most proud of myself for making this aspect of the program, because it was difficult to map a string with “3+6=9” to the corresponding buttons on the physical board.
After finishing the GUI, I noticed that some long solution chains were not being screened properly, an issue I thought I had fixed earlier.  This is a recurring problem.  I’m also becoming wary of all these screening tests I’m adding in to account for the 4x4, because I don’t know if they are affecting my already-working 3x3 (they were).
I restarted the program and decided to model it after the depth-first search algorithm.  This is for navigating all the paths in a tree.  I figured that I could treat each tile on the board as the root of its own tree, and recursively traverse the possible paths leading out of it, using sum == area as an end case.  Each tree is non-binary, so there are more than two ways to go at each point, which means that a loop is necessary for iterating through each root’s children.  This method significantly increases the efficiency of my program, because when the program knows a path won’t lead to a solution, it won’t continue on that path and won’t need to calculate any further.  Now, I can actually do a 5x5 (although this should generally be avoided due to the amount of computation required), whereas my first program froze every time I tried.  I think it would scale easier if the range of numbers (0, 9) was also scaled, because the 2x2 hardly ever has a solution and the 4x4 always has many solutions, but for now I’m very happy with my program.

