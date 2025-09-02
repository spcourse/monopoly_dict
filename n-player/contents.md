# Many Players

By using a configuration dictionary and have the starting money be a list, you've started to make your code more general. Now it's time to fully commit to that effort. 

In this assignment, you will adapt your Monopoly simulation so that it can handle **any number of players**.

Start by creating a new file called `monopoly_dicts2.py` and copy your existing functions `simulate_monopoly_games(games, board_config, starting_money)` and `simulate_monopoly(board_config, starting_money)` (and all other functions required to run them) over to the new file.

### Assignment

* Modify the functions so that they work for **any number of players**.

* You should be able to call the function like this:

    board_config = {
        "lap_money": 200,      # money earned when passing Start
        "board_size": 40,      # number of spaces on the board
        "properties": {        # mapping from board position → price
            1: 60, 3: 60, 5: 200, 6: 100, 8: 100, 9: 120, 11: 140, 12: 150, 13: 140, 14: 160, 15: 200, 16: 180, 18: 180, 19: 200, 21: 220, 23: 220, 24: 240, 25: 200, 26: 260, 27: 260, 28: 150, 29: 280, 31: 300, 32: 300, 34: 320, 35: 200, 37: 350, 39: 400
        }
    }
    starting_money = [1500, 1600, 2000]  # money for [player1, player2, player3]
    number_of_games = 1000
    deltas = simulate_monopoly_games(games, board_config, starting_money)
 
  where `deltas` is a list containing the average delta (difference in amount of streets) with respect to player 1.

* The function should work for a list `starting_money` of any length:

  * The length of the list determines the number of players.
  * Each element in the list specifies the starting money of the corresponding player.

### Hints

* Anywhere you previously referred to `player1` and `player2`, you’ll now need to **loop over all players** instead of hard-coding.
* If your previous solution was already designed with flexibility in mind, this assignment will be straightforward. If not, you may need to restructure your code.

### Testing

* First, test your new version with exactly **two players**. It should reproduce the same results as your earlier assignment.
* Then, try running it with more players (e.g. 3 or 4). Do the results make sense?
* Can you come up with other strategies to check whether your code behaves correctly?




