# Counting visits

The last thing we will practice with our Monopoly simulation is the counting pattern (see theory *'3. Counting'*). Instead of changing the rules of the game, we will use this pattern to collect statistics about how often each position on the board is visited.

To prevent this code from affecting your earlier tests, do this step in a separate file named `monopoly_visits.py`. Copy your `simulate_monopoly()` function into the new file, along with any helper functions that it directly calls. _You don’t need to copy everything — only the pieces required to make this function run on its own._

Inside this function, add a dictionary that will serve as a histogram of visits. Each time a player lands on a tile, update this dictionary so the position is counted. At the end of the game, the dictionary should map every visited position to the number of times it was landed on. Remember that _start_ (position 0) should also be included whenever a player lands on it!

When you are confident the counting works, explore the results:

- Print a small summary, for example the five most visited positions.
- Save a bar chart of the results using `matplotlib`, with positions on the x-axis and visit counts on the y-axis. Make sure your plot is easy to read and clearly shows what the data represents. Think about how you would normally present a plot to someone else: what would make it understandable at a glance? Save your plot as `visits.png`.

## Straight to jail

As an extra twist, and to make the plot more interesting, let’s add one special rule: if a player lands on position 30, they must immediately go to jail on position 10. This means that instead of staying on 30, their position is set to 10 for the next turn.

Implement this rule in a function named `simulate_monopoly_jail()`. Don’t forget to update the visit counts for both tiles! The player landed on 30, but is then moved to 10. This way your histogram will reflect both the chance of hitting the “Go to jail” tile and the fact that jail itself is visited more often.

Save your plot as `jail_visits.png`. Does this affect the simulation in a way you would expect?
