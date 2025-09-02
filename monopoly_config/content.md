# Monopoly Design Challenge

In this assignment, you will practice working with dictionaries by gradually redesigning your Monopoly simulation. The focus of this exercise is not to expand the game with many new rules, but to apply the dictionary patterns you have just learned in a practical way. The Monopoly game is a nice example because it has lots of configuration values, property ownership to track, and events we can count. This will allow us to work through multiple dictionary patterns step by step.

You’ll start with the original program from `monopoly_realistic.py` and, step by step, add new features and refactor parts of the code.

Aside from just practicing with dictionaries, by the end of this assignment, you’ll have a more flexible, better-structured Monopoly simulation. The simulation will use dictionaries and sets to make the game configuration easier, the code cleaner, and the simulation more efficient.

> Refactoring — improving the structure of existing code without changing its behavior — can be tricky and error-prone. To avoid spending too much time debugging, you should **test your program after every small change**. Use the provided tests regularly to ensure that everything still works as expected.
>
> **We’ve set up this exercise and the accompanying tests so that you can verify your code after every step**.

### Step 0: setup

Create a new file named `monopoly_dicts.py` and copy your code from `monopoly_realistic.py`. Remove the `equilibrium()` function, as you won't need it for this assignment.

Throughout this exercise, you will be able to use the following `checkpy` command to test your code:

    checkpy monopoly_dicts

Every time you complete a step, you will pass more of the check. Running the tests after this first step should net you two green checks! <!--TODO check if this is correct -->

### Step 1: replacing the function headers

The first thing we will do is to collect important game settings into a dictionary named `board_config`. For now, copy the following dictionary into your `main()` function:

    board_config = {
        "board_size": 40,               # number of spaces on the board
        "lap_money": 200,               # money earned when passing Start
        "starting_money": [1500, 1500]  # starting money for player 1 and player 2
    }

> This is a very common pattern (see *'2. Config/attributes'* in the theory), and instead of scattering these values throughout your code, we’ll pass them in as a single configuration dictionary. In essence, we’re separating *configuration* (properties of the game, like: prices, board size, lap income, starting money) from *simulation logic* (what the game does: move, buy, pay). Using a config dictionary makes your code easier to change and test. For example, you could change the board size or give the players a different starting balance without touching the game logic at all.

Update the function header of the `simulate_monopoly_games()` and `simulate_monopoly()` function to accept `board_config` as an argument:

    def simulate_monopoly_games(games, board_config):
        ...


    def simulate_monopoly(board_config):
        ...

You will also need to update all the lines where `simulate_monopoly_games()` or `simulate_monopoly()` are called. You can directly pass both the variable `board_config`.

Now replace the hardcoded values in your code with dictionary lookups (see theory *'0. Fast search'*):
* Use the board size from the `board_config` dictionary to decide when to wrap around the board.
* Use the lap money from the `board_config` dictionary as the reward when a player completes a lap.
* Initialize player balances using the starting money from the `board_config` dictionary. This entry is a list where each element corresponds to one player’s starting balance. To set a player’s initial money, take the value from the list at the position that matches that player’s index.

Test your code using `checkpy`. You should pass 3 more tests. <!--TODO check if this is correct -->

### Step 2: properties as a nested dictionary

So far, your configuration dictionary contains simple values and a list. Next, we’ll extend it with a nested dictionary that maps board positions to property prices. Replace your `board_config` dictionary with:

    board_config = {
        "board_size": 40,               # number of spaces on the board
        "lap_money": 200,               # money earned when passing Start
        "starting_money": [1500, 1500], # starting money for player 1 and player 2
        "properties": {                 # property prices per board position
          1: 60, 3: 60, 5: 200, 6: 100, 8: 100, 9: 120,
          11: 140, 12: 150, 13: 140, 14: 160, 15: 200,
          16: 180, 18: 180, 19: 200, 21: 220, 23: 220,
          24: 240, 25: 200, 26: 260, 27: 260, 28: 150,
          29: 280, 31: 300, 32: 300, 34: 320, 35: 200,
          37: 350, 39: 400,
        }
    }

Then, update your code so that whenever you need the price of a property, you retrieve it from the properties entry in the `board_config` dictionary.

Remember that not every position on the board is a property and has a price! Any position not listed in the properties dictionary does not have an associated price, and cannot be bought.

> This gives you more practice with what we call a nested lookup, first in the config dictionary, then in the properties dictionary. We also continue the pattern from step 1: your game logic no longer depends on magic numbers, but on lookups in the config (*'0. Fast search'* and *'2. Config/attributes'*).

Test your code using `checkpy`. You should pass X more tests. <!--TODO check if this is correct -->

### Step  3: tracking ownership

To decide whether a property can still be purchased, we need to know which ones have already been bought. You may currently be keeping track of which properties each player owns with a counter or a list. We will improve this by using sets, because sets automatically prevent duplicates and make membership checks efficient. Each player will have their own set of owned properties, and together these sets show which tiles are already taken.

To make this step easier to follow, we’ll do it in two parts. First, you will write a small helper function that checks whether a property at a given position is already owned by any player. Then, you will rewrite the code tracking player ownership, and integrate the helper function into the rest of the code.

#### Step 3.1: checking ownership

Write a helper function named `is_unowned(position, owned_sets)`, where `position` is an index on the board, and `owned_sets` is a list of all player ownership sets. The function should return `True` if the position is not in any of the ownership sets, and return `False` if any player owns it.

Before continuing, test your function on a tiny example. You can use something like the test below in your file, or run it in a separate test-file with a copy of your function.

    print(is_unowned(14, [{12, 16}, {18}])) # expected: True

    print(is_unowned(18, [{12, 16}, {18}])) # expected: False

Then, test your code using `checkpy`.

#### Step 3.2: integrating ownership

Now change your code so that ownership is tracked with sets rather than counters or lists. At the start of each game, create an empty set for every player. Whenever a player buys a property, add the position of that property to their set.

> Remember: `{}` creates a dictionary, not a set. To create an empty set, always use `set()`.

With this change in place, integrate the `is_unowned()` helper function inside your game loop. Keep in mind that this function only checks whether a position is already owned; you still need to decide separately whether the position is a property and whether the player can afford it.

This completes the transition to sets for tracking ownership and integrates your helper function into the rest of the game logic. If everything is wired up correctly, purchases should now only happen when a tile is actually available.

Test your code using `checkpy`. You should pass X more tests. <!--TODO check if this is correct -->

### Checkpoint

At this point, your game should no longer depend on any hardcoded numbers for board size, lap money, starting balances, or property prices. Everything should come directly from the `board_config` dictionary. Double check that:
- movement around the board uses the value from the configuration dictionary
- rewards for completing a lap is determined by the value from the configuration dictionary
- player balances are initialized from the configuration dictionary
- property prices come from the configuration dictionary
- player properties are tracked in sets,
- player properties are tracked in sets, and the `is_unowned()` function is used to determine whether a property is not yet owned by any of the players
- all `checkpy` checks pass

All these changes make it much easier to change the configuration without touching the simulation code! This is exactly the separation of configuration and simulation logic we are aiming for.

### Step 4: counting visits

The last thing we will practice with our Monopoly simulation is the counting pattern (see theory 3. Counting). Instead of changing the rules of the game, we will use this pattern to collect statistics about how often each position on the board is visited.

To prevent this code from affecting your earlier tests, do this step in a separate file named `monopoly_visits.py`. Copy your `simulate_monopoly()` function (and any of the helper functions you need) into this file and adapt it so it also records which positions are visited during the game.

Inside this function, add a dictionary that will serve as a histogram of visits. Each time a player lands on a tile, update this dictionary so the position is counted. At the end of the game, the dictionary should map every visited position to the number of times it was landed on. Remember that Start (position 0) should also be included whenever a player passes or lands on it!

When you are confident the counting works, explore the results:

- Print a small summary, for example the five most visited positions.
- Save a bar chart of the results using `matplotlib`, with positions on the x-axis and visit counts on the y-axis. Make sure your plot is easy to read and clearly shows what the data represents. Think about how you would normally present a plot to someone else: what would make it understandable at a glance? Save your plot as `visits.png`.

This step does not include checks from `checkpy`.

<!-- TODO: afsluitend stukje? -->

<!-- TODO: bonus met meer spelers op basis van werk simon, dictionary per speler. Aanmaken van deze dict in een losse functie.-->
