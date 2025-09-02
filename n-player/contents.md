# Many Players

One important aspect of good design is **generality**. Ask yourself: is your code written in a way that is very specific to the problem at hand, or is it flexible enough to handle new situations?

In this assignment, you will adapt your Monopoly simulation so that it can handle **any number of players**.

Start by creating a new file called `monopoly-nplayer.py` and copy your existing function `simulate_monopoly(starting_money_p1, starting_money_p2)` (along with everything else needed to run it).

### Assignment

* Modify `simulate_monopoly(starting_money_p1, starting_money_p2)` so that it works for **any number of players**.

* Instead of two separate input parameters, the function should accept a **single list** containing the starting money for each player.

* You should be able to call the function like this:

  ```python
  starting_money = [1500, 1600, 2000]  # money for [player1, player2, player3]
  simulate_monopoly(starting_money)
  ```

* The function should work for a list of any length:

  * The length of the list determines the number of players.
  * Each element in the list specifies the starting money of the corresponding player.

### Hints

* Anywhere you previously referred to `player1` and `player2`, you’ll now need to **loop over all players** instead of hard-coding.
* If your previous solution was already designed with flexibility in mind, this assignment will be straightforward. If not, you may need to restructure your code.

### Testing

* First, test your new version with exactly **two players**. It should reproduce the same results as your earlier assignment.
* Then, try running it with more players (e.g. 3 or 4). Do the results make sense?
* Can you come up with other strategies to check whether your code behaves correctly?


