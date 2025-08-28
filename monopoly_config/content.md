# Monopoly Design Challenge

In this assignment, you’ll gradually extend and improve your Monopoly simulation code. You’ll start with the original program from `monopoly_realistic.py` and, step by step, add new features and refactor parts of the code.

By the end of this assignment, you’ll have a more flexible, better-structured simulation that uses dictionaries and sets to make the game configuration easier, the code cleaner, and the simulation more efficient.

> Refactoring — improving the structure of existing code without changing its behavior — can be tricky and error-prone. To avoid spending too much time debugging, you should **test your program after every small change**. Use the provided tests regularly to ensure that everything still works as expected.
>
> **We’ve set up this exercise and the accompanying tests so that you can verify your code after every step**.

### Step 0: setup

Create a new file named `monopoly_dicts.py` and copy your code from `monopoly_realistic.py`. Remove the `equilibrium()` function, as you won't need it for this assignment.

Throughout this exercise, you will be able to use the following `checkpy` command to test your code:

    checkpy monopoly_dicts

Every time you complete a step, you will pass more of the check. Running the tests after this first step should net you two green checks!

### Step 1: replacing the function headers

In this step we will introduce the `board_config` dictionary, which collects all important game settings in one place. Instead of scattering values like the money you will receive when you complete a lap or something like property prices across your code, we’ll pass them in as a single configuration dictionary:

    board_config = {
        "lap_money": 200,      # money earned when passing Start
        "board_size": 40,      # number of spaces on the board
        "properties": {        # mapping from board position → price
            1: 60, 3: 60, 5: 200, 6: 100, 8: 100, 9: 120, 11: 140, 12: 150, 13: 140, 14: 160, 15: 200, 16: 180, 18: 180, 19: 200, 21: 220, 23: 220, 24: 240, 25: 200, 26: 260, 27: 260, 28: 150, 29: 280, 31: 300, 32: 300, 34: 320, 35: 200, 37: 350, 39: 400
        }
    }

This is very common in simulation and modelling. In essence, we’re separating *configuration* (properties of the game, like: prices, board size, lap income, starting money) from the *simulation logic* (what the game does: move, buy, pay). By using this dictionary as a parameter in your simulation functions, the game becomes more flexible and easier to test. It becomes possible to swap in different configurations, for example, with a bigger board, a different lap income, or alternative property prices, without having to change the simulation logic itself.

Update the function header of the `simulate_monopoly_games()` and `simulate_monopoly()` function to accept `board_config` and `starting_money` as inputs. For now, we'll hardcode starting_money_p1 and starting_money_p2

    def simulate_monopoly_games(games, board_config, starting_money):
        ...


    def simulate_monopoly(board_config, starting_money):
        starting_money_p1 = 1500 # temporary
        starting_money_p2 = 1500 # temporary

        ...

You will also need to change the lines where `simulate_monopoly()` is called inside of your `simulate_monopoly_games()` function. You can directly pass the variables `board_config` and `starting_money` for now.

Test your code using `checkpy`. You should pass 3 more tests!

### Step 2: Integrating function parameters

In the previous step you've temporarily hardcoded the starting money for both players. Now we’ll stop hardcoding and use the values passed in. This makes it easy to run the same simulation with different initial conditions.

Use the values from the `starting_money` parameter instead of the fixed numbers from step 1. `starting_money` should be a list where each entry is the starting balance of a player, e.g. `[1500, 1500]`.

> By providing the `starting_money` as a list, you can add more players later without changing the function!

Somewhere in your code you should have defined the value for the money a player gets when they complete a lap. Replace any hardcoded “lap income” with the value from `board_config`.

Test your code using `checkpy`. You should pass 3 more tests after starting use of the `starting_money` parameter, and 3 more after starting use of the `'lap_money'` from `board_config`.

### Step 3: using sets

So far, you have probably tracked player ownership with lists. In this step, we will switch these to sets. A set automatically prevents duplicates, makes membership checks faster, and will make it easier to implement code that finds buyable properties later on.

> Remember: `{}` creates a dictionary, not a set. To create an empty set, always use `set()`.

Update your code so that each player’s properties are stored in a set. This will probably also affect your code in multiple places, so it might be more efficient to find and mark those places first with a small comment!

Test your code using `checkpy`. You should not pass more tests, but you shouldn't pass less tests either!

### Step 4: finding buyable properties

Now that properties are tracked with sets, it becomes easy to figure out which properties are still available for purchase. In this step you will implement a new function called `get_buyable_properties()`.

The `get_buyable_properties(board_properties, player_owned_properties)` function should take two arguments;

- `board_properties`: the dictionary containing all the properties that are on the board
- `player_owned_properties`: a list of sets with properties owned by each player

The first argument is directly available from `board_config['properties']`. The second argument was introduced in the previous step.

The function should return a set containing all properties that can still be bought.

You can test your function manually with a simple toy problem before using it on the entire `board_config`:

    board = {14: 100, 18: 150, 23: 200}

    print(get_buyable_properties(board, [set(), {18}]))

Should print:

    {14, 23}

After testing your function manually, test it using `checkpy`. You should pass one more test.

### Step 5: integrating properties

Up until now, some parts of your code may still rely on hardcoded values for property prices or board length. In this step you will integrate the last information stored in `board_config` so that the game no longer depends on any hardcoded values.

The dictionary `board_config` from step 1 already provides `'properties'` and `'board_size'`, a mapping from board position to purchase price and the number of spaces on the board respectively.

Start by updating your code so that movement around the board uses the value of `'board_size'` from the `board_config` dictionary. Test this first, to make sure movement still works.

Then, update your code such that whenever you need the price of a property, you look it up in `board_config['properties']`. This will probably require you to do multiple changes. Identify those first, before changing any code.

> Hint: you can use your `get_buyable_properties()` function to keep track of which properties are still for sale. Update this set whenever a property is bought. A property can only be purchased if:
> – its index is still in `buyable_properties`, and
> – the player has enough money.
>
> Also consider your loop condition: you can use the size of buyable_properties to decide when the game should end (once no properties remain).

Test your code using `checkpy`. You should pass 1 more test after using the `board_size` value, and 1 more after using the `properties`.

<!-- TODO: tests voor dit gedeelte -->

<!-- TODO: step 6, counting ring heatmap/histogram waarin je bijhoudt hoe vaak ieder vakje bezocht is + tests hiervoor?? Of misschien gewoon een voorbeeld voor hoe dit er uit komt te zien-->

<!-- TODO: afsluitend stukje -->

<!-- TODO: bonus met meer spelers, dictionary per speler. Aanmaken van deze dict in een losse functie.-->
