# Mastermind Solver

Based on the board game [Mastermind][def]

# Implementation and Strategies

The solver has two main methods `adjustMoves` and `makeGuess`. Adjust moves calls the `prune` method which removes moves from the move base if they are not valid according to the black and white peg feedback received from the game.

## Random

Of the available moves, simply selected a random one.

## Most Occurences

For this attempt the alogirithm found the most occuring color within the possible moves for each of the four positions and if it was a possible move, guessed those colors for their respective spaces. The code for makeGuess in solver is as follows:
```Python
first = {}
second = {}
third = {}
fourth = {}

for move in self.moves:
    first[move[0]] = first.get(move[0], 0)
    second[move[1]] = second.get(move[1], 0)
    third[move[2]] = third.get(move[2], 0)
    fourth[move[3]] = fourth.get(move[3], 0)

one = max(first, key=first.get)
two = max(second, key=second.get)
three = max(third, key=third.get)
four = max(fourth, key=fourth.get)
potGuess = [one, two, three, four]

if potGuess in self.moves:
    return potGuess
```

## MiniMax-Shortened

For minimax I implemented a [MiniMax][def2] algorithm to decide the next move. It looks at the available moves from the adjustMoves method and finds the move that results in the smallest maximum moves after playing. It determines this by looking at all the possible responses from the game and determining what the potential move set will be for each possible response, the move set with the maximum moves is then taken and compared with all the other maxes across all the other _current_ possible moves. The minimum of these is taken. 

It slightly decreased our average completion compared to random selection but not by much.

## MiniMax-Extended

Same minimax algorithm as above but checks every single possible peg-combination as a valid move instead of just those that are potential correct answers. This means that it can make a guess that is known to not be the correct answer if it results in the smallest maximum moves after playing. Searching through all possible peg combinations each guess takes much longer to compute than the shortened minimax version. Our average guesses has increased from the shortened version but the high guesses have decreased, with the extended mini-max algorithm the maximum guesses possible are 5, this is proven in Donald Knuth's paper, [The Computer as Master Mind][def3]

# Results

For each different strategy I ran 10,000 games and took the average guesses it took to complete the games and the maximum guesses taken to win over the 10,000 games

| Strategy          | Average Guesses to Win | High |
|-------------------|:----------------------:|:----:|
| Most Occurences   |         5.7856         |   9  |
| Random Selection  |         4.6437         |   8  |
| MiniMax-Shortened |         4.4819         |   6  |
| MiniMax-Extended  |         4.7563         |   5  |

[def]: https://en.wikipedia.org/wiki/Mastermind_(board_game)
[def2]: https://en.wikipedia.org/wiki/Minimax
[def3]: http://www.cs.uni.edu/~wallingf/teaching/cs3530/resources/knuth-mastermind.pdf
