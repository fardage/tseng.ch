+++
author = "Marvin Tseng"
categories = ["AI"]
date = 2020-09-13T11:09:00Z
description = ""
draft = false
image = "/2021/02/2048_action-2.gif"
slug = "2048-ai-best-heuristics"
tags = ["AI"]
title = "🎮 2048 AI - Best Heuristics"

+++

The game 2048 was developed by Gabriele Cirulli. It's based on 1024 by Veewo Studio and conceptually similar to Threes by Asher Vollmer. Read his story of the making & rip-offs in game development here: [http://asherv.com/threes/threemails/](http://asherv.com/threes/threemails/)

![2048 AI in Action](/2048_action.gif)

In the second lab for the module "Artificial Intelligence 1" we developed a simple as well as a sophisticated AI agent to remote control the 2048 game running in the [browser](https://play2048.co). We were able to achive a massive score of **146,348.00.**

- Github: https://github.com/fardage/2048-AI

## Heuristic AI

By using simple rules/heuristics we created a simple agent that could beat a randomly playing agent. These are the things we noticed which help us to get a higher score in the game.

* When many tiles are on the board, the chance that you can’t move any more is greatly increased; thus, one goal is to have the board as empty as possible.
* If tiles of big value are at the corner of the board, they don’t block the merging of the “smaller” tiles.
* A move bringing two tiles with the same value next to each other is preferable over a move that won’t give you this advantage.

We have included all these aspects in our heuristics to score the board. (Higher is better)

1. **Snake Shape**: Enforce layout by multiplying weights to tiles

```python
def heuristic_tile_layout(board):
    rewardBoard = numpy.array([
        [2**15, 2**14, 2**13, 2**12],
        [2**8, 2**9, 2**10, 2**11],
        [2**7, 2**6, 2**5, 2**4],
        [2**0, 2**1, 2**2, 2**3]
    ])
    
    return numpy.multiply(rewardBoard,board).sum()
```

1. **Remove Tiles**: Reward for removing tiles
2. **Same Move Penalty**: Penalty for not changing board

<table>
<thead>
<tr>
<th>Type</th>
<th>Configuration</th>
<th>Score</th>
</tr>
</thead>
<tbody>
<tr>
<td>Heuristic</td>
<td>Snake Shape + Same Move Penalty + Remove Tiles</td>
<td>10,136.00</td>
</tr>
<tr>
<td>Heuristic</td>
<td>Snake Shape + Same Move Penalty + Remove Tiles</td>
<td>6,588.00</td>
</tr>
<tr>
<td>Heuristic</td>
<td>Snake Shape + Same Move Penalty + Remove Tiles</td>
<td>4,684.00</td>
</tr>
<tr>
<td>Heuristic</td>
<td>Snake Shape + Same Move Penalty + Remove Tiles</td>
<td>4,316.00</td>
</tr>
<tr>
<td>Heuristic</td>
<td>Snake Shape + Same Move Penalty + Remove Tiles</td>
<td>3,696.00</td>
</tr>
<tr>
<td>Heuristic</td>
<td>Snake Shape + Same Move Penalty + Remove Tiles</td>
<td>3,340.00</td>
</tr>
<tr>
<td>Heuristic</td>
<td>Snake Shape + Same Move Penalty + Remove Tiles</td>
<td>3,060.00</td>
</tr>
<tr>
<td>Heuristic</td>
<td>Snake Shape + Same Move Penalty + Remove Tiles</td>
<td>1,684.00</td>
</tr>
<tr>
<td>Heuristic</td>
<td>Snake Shape + Same Move Penalty + Remove Tiles</td>
<td>1,100.00</td>
</tr>
<tr>
<td>Heuristic</td>
<td>Snake Shape + Same Move Penalty + Remove Tiles</td>
<td>832.00</td>
</tr>
</tbody>
</table>

* MAX: 10,136.00
* AVG: 3,943.60

With these heuristics, we were able to achieve an average of 3'943 points and a **high score of 10'136 points**.

## Search AI

![Expectimax](/2048-expectimax.svg)

By creating an agent based on the expectimax algorithm we reached the 2048 tile 100% of the time. With our expectimax implementation, we reused our snake shape heuristic.

```python
def score_toplevel_move(move, board, depth):
    """
    Entry Point to score the first move.
    """
    global UP, DOWN, LEFT, RIGHT
    global move_args

    countZeros = 0
    nodeScore = 0
    childrenResults = []
    newboard = execute_move(move, board)

    # Stop recursion
    if board_equals(board, newboard):
        return 0
    if depth == 0:
        return ha.evaluate_board(board, newboard)

    newboards = get_all_possible_boards(newboard)

    for boardWithNewTile in newboards:
        result = [score_toplevel_move(i, board, maxDepth) for i in range(len(move_args))]
        bestmove = result.index(max(result))
        childrenResults.append(best)

    countChildren = len(childrenResults) - countZeros
    if countChildren == 0:
        return 0
    
    for node in childrenResults:
            nodeScore += node / countChildren
        
    return nodeScore

```

Other things we did:

* Calculates score + probability for possible boards per move
* Only placing 2s as opponent to speed up computation
* MAX_DEPTH: `if countFreeTiles <= 4 then 3, else 2`
* Tried Threading, but no performance gain

<table>
<thead>
<tr>
<th>Type</th>
<th>Configuration</th>
<th>Score</th>
</tr>
</thead>
<tbody>
<tr>
<td>Search</td>
<td>Snake Shape</td>
<td>146,348.00</td>
</tr>
<tr>
<td>Search</td>
<td>Snake Shape</td>
<td>128,984.00</td>
</tr>
<tr>
<td>Search</td>
<td>Snake Shape</td>
<td>124,112.00</td>
</tr>
<tr>
<td>Search</td>
<td>Snake Shape</td>
<td>79,812.00</td>
</tr>
<tr>
<td>Search</td>
<td>Snake Shape</td>
<td>76,732.00</td>
</tr>
<tr>
<td>Search</td>
<td>Snake Shape</td>
<td>72,036.00</td>
</tr>
<tr>
<td>Search</td>
<td>Snake Shape</td>
<td>67,124.00</td>
</tr>
<tr>
<td>Search</td>
<td>Snake Shape</td>
<td>58,720.00</td>
</tr>
<tr>
<td>Search</td>
<td>Snake Shape</td>
<td>52,272.00</td>
</tr>
<tr>
<td>Search</td>
<td>Snake Shape</td>
<td>50,572.00</td>
</tr>
</tbody>
</table>

* **MAX: 146,348.00**
* AVG: 85,671.20

By calculating the future possibilities and the scores, we were able to achieve excellent results.

We had a lot of fun working on this lab. By far the most difficult part was finding a good method to calculate the score of a given board.

