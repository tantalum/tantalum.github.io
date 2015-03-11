---
layout: post
title: MiniMax and Tic-Tac-Toe
---

## What it is

Lets start this post of with some techno-babble. Minimax is a depth-first search algorithm to find the least-lost strategy for zero-sum two person turn based games.

A zero-sum game is a game where, if one player loses, than another player must win the same number of points and vice-versa. For example chess is a zero-sum game because if one player wins (Lets say winning a score of +1) than the other player must lose (getting a score of -1), or the game is a draw and both players get a score of 0. We don't give players that lose a score of 0 and the winner +1 because that would mean than one player won the game and the other player is at a draw, which is impossible in chess. The same goes for other games such as tic-tac-toe, Go, and four in a row. Another set of zero sum games are games where the total number of points is constant (think poker where players can't buy chips) and if someone wins (a bet) than the rest of the players must lose points (money) equal to the number of points won. 

Depth-first search means that we follow the algorithm all the way down until the end, and then start moving backwards, looking for the best move as we go. The reason why we have to follow the algorithm all the way to the end will become clear soon.

<!--more-->

## How it works

Now that we understand some of the techno-babble, lets start talking about how Minimax works. Minimax is based on the assumption that your opponent will make the move that will maximize his/her/its potential score, thus minimizing your potential score (zero-sum, remember) and you want to do the same to your opponent.

So what you do with Minimax is look forward at the potential moves up to a point. Because each move has a number of possible next-moves, we'll end up with a tree if possible moves. Then, starting at the end of tree (depth-first) we give each of the leaf moves a score. After that we move up the tree and if the branch we are on is our turn we give it a score that is equal to the maximum score of the sub-moves (maximize our score), otherwise we give the branch a score that is equal to the minimum score of the sub-moves (i.e. assume the opponent will maximize his score). Finally, when we reach the top we pick the move with the highest score (it's our turn so we maximize the score). A typical implementation of Minimax will look something like this:

```
function score(game):
	if game.winner == me: return 1
	elif game.winner != me: return -1 #<-- remember, only two players
	else: return 0 # Game is a draw

function max(game):
	if game.over:
		return score(game)
	
	best_score = -1
	for move in game.possible_moves():
		new_game = game.copy().apply_move(move)
		score = mini(new_game)
		if score > best_score:
			best_score = score
	return best_score

function mini(game):
	if game.over:
		return score(game)
	
	worst_score = 1
	for move in game.possible_moves():
		new_game = game.copy().apply_move(move)
		score = max(new_game)
		if score < worst_score:
			worst_score = score
	return worst_score
```

If you look a little bit at mini() and max() you'll see that they are both basically the same function. Most papers and articles will have separate mini and max functions that look pretty much like what we just wrote. However, when comes to code, you want to try and not repeat yourself. So what were gonna do is combine the mini and max functions into a single function:

```
function minimax(game):
	if game.over:
		return score(game)
	
	best_score = -1
	multiplyer = -1
	if game.player == me:
		multiplyer = 1 #<-- Make everthing the same as Max if it's our turn
	
	for move in game.possible_moves():
		new_game = game.copy().apply_move(move)
		score = multiplyer*minimax(new_game)
		if score > best_score:
			best_score = score

	return multiplyer*best_score
```

## Tic-Tac-Toe

Tic Tac Toe is a extremely popular example when it comes to showcasing the Minimax algorithm for two reasons:

1. It's a zero-sum two player game.
2. The number of possible moves in relatively small, so we can follow the algorithm all the way to the end of the game.

Those two criterian make Tic Tac Toe a perfect candidate for Minimax. Because it is possible to calculate all the possible moves for any Tic Tac Toe game, Minimax gurantees that the game will at least end in a draw. In other more complicated games, like Chess and Go, it is not possible to calculate all the possible moves. In those more complex games (Where it is not possible to calculate all the possible moves) the algrithm is designed to follow the possible moves up to a specific "Event Horizon" and assign each leaf-state a hurisitc based score. 

Hopefully, it's obvious how to apply MiniMax to Tic-Tac-Toe. In case it isn't, the first step is evaluate the score for a winner. If we win we get a score of +1, of we loose a score of -1, otherwise the game is a draw and the score is 0. Simple enough:

```java
	private int evaluateWinner(Cell winner) {
		if((winner == null) || (winner == Cell.EMPTY)) {
			return 0;
		} else if(winner == team) {
			return 1;
		} else {
			return -1;
		}
	}
```

To kick things off with MiniMax, we're gonna go through each possible move and find the move that will give us the best score:

```java
	@Override
	public Location makeMove() throws TicTacToeException {
		List<Location> possibleLocations = game.board.emptySlots();
		
		float thisScore;
		float bestScore = -1;
		Location bestMove = null;
		TicTacToeBoard currentBoard;
		
		for(Location l : possibleLocations) {
			currentBoard = game.board.copy();
			currentBoard.makeMove(team, l);
			thisScore = minimax(currentBoard, TicTacToeBoard.oppositePlayer(team));
			if(thisScore >= bestScore) {
				bestScore = thisScore;
				bestMove = l;
			}
		}
		
		return bestMove;
	}
	
```

And the meat of the algorithm is the `minimax` function we described earlier:

```java
  	private int minimax(TicTacToeBoard board, Cell thisTeam) throws TicTacToeException {
		int maxScore = -1;
		int multiplyer = 1;
		if(thisTeam != team) {
			multiplyer = -1;
		}
		Cell winner = board.winner();
		if(winner != null) {
			return evaluateWinner(winner);
		}
		
		List<Location> possibleMoves = board.emptySlots();
		int thisScore;
		TicTacToeBoard currentBoard;
		
		for(Location l: possibleMoves) {
			currentBoard = board.copy();
			currentBoard.makeMove(thisTeam, l);
			thisScore = multiplyer*minimax(currentBoard, TicTacToeBoard.oppositePlayer(thisTeam));
			if(thisScore >= maxScore) {
				maxScore = thisScore;
			}
		}
		return multiplyer*maxScore;
	}
```

And that's it! Wrap all that in a neat little class with the blanks filled in and you have an AI Tic-Tac-Toe player that will never lose.

The complete implementation of this code is on [github](https://github.com/tantalum/tictactoe). The code is written in Java.


