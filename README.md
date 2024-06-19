
**Chess Multiplayer**

**Problem Definition:** 

The focus of this project is to develop an advanced chess AI that offers experienced players a robust and challenging experience. While the game will have multiple difficulty levels including a random player, a central problem is devising a "pro" difficulty setting where the AI's decision-making prioritizes aggressive checkmate-oriented play while demonstrating an understanding of long-term positional considerations. To effectively solve this problem, it's crucial to characterize the nuances of this playstyle. This characterization includes:



* Initial State: Defining the typical starting conditions for a "pro" level game. Are there specific opening preferences, or should the AI be adaptable? Should the difficulty setting account for a player's skill level or previous game results? I made it adaptable.
* Goal States: Specifying what constitutes a win for the AI. Does this always mean delivering checkmate, or could it include scenarios where a significant material or positional advantage is sufficient? While the ultimate goal is checkmate, AI also gets significant utility by posing a threat or killing other pieces.
* Obstacles: Identifying challenges in generating this playstyle. This includes balancing aggression with sound strategic principles, managing risk, and accurately evaluating threats posed by the human player.
* Scale: Determining the scope of the problem. Does the "pro" AI need to excel across all phases of the game (opening, endgame), or are specific areas of strength acceptable?

**Why AI is a Good Fit:**

	With AI, we can use various search algorithms and equip shortcuts like heuristics or use alpha-beta pruning, which will save computational time. We can modify such heuristics to tune the AI to be very accurate or very fast.



**Solution Specification:** 

My goal is to build a chess game where humans can play against each other or face an AI opponent with varying difficulty levels. AI decision-making is where I've focused most of my effort.

**How I Evaluate Positions:**

At the core of my AI is my evaluate_board function. I primarily focus on material advantage – counting up the piece values for each side (with different weights) (Chess Programming Wiki, n.d.). I also used different "positional tables" for the start and the end of the game. These are like maps for each piece type, where squares have different scores depending on how favorable they are. This helps the AI understand which squares are valuable for its pieces. I used the tables from Hartikka (2017). 

For the Pro player, it goes beyond just these things and adds King safety and passed pawn threat.

**King Safety**: The player analyzes how well-protected its own king is and how vulnerable the opponent's king is. This encourages direct attacks to weaken the opponent's king position.

**Passed Pawn Threat:** The AI identifies passed pawns (those without opposing pawns blocking their advance) and prioritizes their creation and promotion. This is strategically sound, especially in the endgame where a passed pawn can be decisive.

**How I Choose Moves:** \
	I use the Minimax algorithm to explore the game tree. Let's say it's the AI's turn. I generate all legal moves, and then, for each one, I imagine the opponent's best response, then my best move after that, and so on. I do this for a certain "depth" – the number of moves ahead I explore (which is a parameter and different values are used for different difficulty levels). However, with a high depth level (>4), the chess becomes quite slow with generating moves. Minimax gives each possible future board a score/utility based on the evaluation function. The AI will pick the move leading to the best utility value/ score. \
	To make this search faster, I used Alpha-Beta pruning. With this optimization we can "prune" (disregard) entire branches of the game tree if it becomes clear they won't lead to a better outcome than something the AI has already found. \


**Difficulty Levels:** \
There are four difficulty levels for the AI.



* Easy: It chooses a random move among all possible valid moves from the current state.
* Medium: It uses the piecewise heuristics but searches for the depth of 2.
* Challenging: It also uses the piecewise heuristics but searches for the depth of 4.
* Pro: It searches for the depth of 4. In addition to the piecewise heuristics, this mode accounts for king safety and passed pawns.

**Analysis of Solution:**

**Table 1._ Game results for different player types. The moves indicate the number of piece moves taken per player before the game ends._**


<table>
  <tr>
   <td>S.N.
   </td>
   <td>Player 1
   </td>
   <td>Player 2
   </td>
   <td>Results
   </td>
   <td>Moves
   </td>
  </tr>
  <tr>
   <td>1
   </td>
   <td>Challenging-ai
   </td>
   <td>Easy-ai
   </td>
   <td>Player 1 won
   </td>
   <td>21
   </td>
  </tr>
  <tr>
   <td>2
   </td>
   <td>Challenging-ai
   </td>
   <td>Medium-ai
   </td>
   <td>Player 1 won
   </td>
   <td>60
   </td>
  </tr>
  <tr>
   <td>3
   </td>
   <td>Medium-player
   </td>
   <td>Easy-ai
   </td>
   <td>Player 1 won
   </td>
   <td>20
   </td>
  </tr>
  <tr>
   <td>4
   </td>
   <td>Easy-ai
   </td>
   <td>Easy-ai
   </td>
   <td>Draw
   </td>
   <td>126
   </td>
  </tr>
  <tr>
   <td>5
   </td>
   <td>Pro-ai
   </td>
   <td>Easy-ai
   </td>
   <td>Player 1 Won
   </td>
   <td>14
   </td>
  </tr>
  <tr>
   <td>6
   </td>
   <td>Pro-ai
   </td>
   <td>Medium-ai
   </td>
   <td>Player 1 won
   </td>
   <td>11
   </td>
  </tr>
</table>





  I played chess against different players and got the results in Table 1. I did not use human players as a test case since I do not have people with a Standard Elo rating available for testing. However, just playing the AI players against each other, it is clear that the different modes' winning probabilities are in ascending order:



However, the game becomes extremely slow when two Pro-ai players are playing against each other. 

**Further Improvements:**



* Investigating more nuanced positional factors like pawn structure and relative position of multiple pieces to opposite King. Current heuristics only consider the position of one piece at a time. However, we can get checkmate after with multiple pieces on the attack.
* Using machine learning to train the evaluation function will lead to optimal moves.
* Creating a list of the best early and end-of-the-game trees would help save computation time. The board position is simple in such cases with fewer branches for the tree.
* Potentially using the Monte Carlo Tree Search (MCTS) algorithm which uses random sampling and statistical evaluation rather than searching the entire search space hence saving time (Jain, 2023).



**References:** 


    Chess Programming Wiki. (n.d.). _Simplified Evaluation Function - Chessprogramming wiki_. Chess Programming Wiki. Retrieved April 19, 2024, from https://www.chessprogramming.org/Simplified_Evaluation_Function


    Hartikka, L. (2017, March 30). _A step-by-step guide to building a simple chess AI_. freeCodeCamp. Retrieved April 19, 2024, from https://www.freecodecamp.org/news/simple-chess-ai-step-by-step-1d55a9266977/


    Jain, S. (2023, May 23). _ML | Monte Carlo Tree Search (MCTS)_. GeeksforGeeks. Retrieved April 19, 2024, from https://www.geeksforgeeks.org/ml-monte-carlo-tree-search-mcts/


    Python-Chess. (n.d.). _python-chess: a chess library for Python_. python-chess Documentation. Retrieved April 15, 2024, from https://python-chess.readthedocs.io/en/latest/index.html
