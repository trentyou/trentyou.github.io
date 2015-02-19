---
layout: post
title: Using a Tree To Solve Sudoku
published: true
---

2/19/2015

In the last few weeks I've been working on rewriting my [sudoku solver](https://github.com/trentyou/Sudoku) in Objective-C and adding a GUI. Something else I've been working on is updating the guessing algorithm that runs after all the possible logical moves have been made.


<div style="text-align:center" markdown ="1">
![](/images/sudokubeforesolved.png)
</div>


In my old guessing algorithm, it would pick a random number from one of the possible answers in an empty cell and repeat until it realizes it made an incorrect guess or the puzzle was solved. 

While this algorithm worked, it could be really slow for some of the harder puzzles. The reason for this was that there was a huge number of possibilities for the correct answer in those puzzles, and my algorithm didn't keep track of the paths that were already guessed. Due to this, the algorithm would often repeat its guesses and waste a lot of time solving the harder puzzles. 

To fix this, I came up with a different algorithm for guessing numbers that would keep track of what had already been guessed. The algorithm would be organized in a tree, with the possible answers from the first empty cell acting as the first level in the tree, and the next empty cells as the next levels in the tree. The algorithm would do a traversal similar to a pre-order tree traversal by drilling down to the left most nodes first and generating the nodes as it went along. 

If it encountered no possible answers for the next level in the tree (meaning an incorrect guess), it would first try shifting over to a sibling node. If the node had no siblings, it would move back to a point in the tree that did have a sibling and try again. The previously traversed tree paths would lose the pointer to that path and be deallocated in order to save memory.

If the algorithm managed to reach the last empty cell at the bottom of the tree, the puzzle was solved. 


<div style="text-align:center" markdown ="1">
![](/images/sudokusolving_post6.gif)
</div>


This algorithm _on average_ is much faster than my previous algorithm, because there's a limit to how long it could take to solve a puzzle. The old algorithm did not have a limit to the time to solve a puzzle, as the guess was random and could theoretically never make the correct guess, especially with so many possible answers. 

[Rewritten Sudoku Solver in Objective-C](https://github.com/trentyou/SudokuSolver-iOS)

