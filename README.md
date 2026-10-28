# s04-code-smells
Code smells across languages

## Group Members
- Alexander Flores
- Aayan Abdullah
- Umayr Rai

## Chosen Code Smells
1. **Mysterious Name**: a function, variable or class has a name that doesn't say what it does.
2. **Duplicated Code**: the same code structure appears in more than one place, so every change has to be repeated.
3. **Loops**: a for/while loop does filtering, transforming, and accumulating all at once so the reader has to read the whole body to understand what it does.

## 1. Mysterious Name

**Introduction:**

A mysterious name is when a variable, function, or class is named something that doesn't tell you what it does. 
Names like `x`, `tmp`, `data`, or `calc` might make sense to the person writing the code at the time, but anyone reading it later has to read the whole thing just to figure out what it's for.

For example, if a game has a function called `upd(p, d)`, you can't tell if it updates a position, a player, or something else without opening it. 
This slows everyone down and makes it easy to use the wrong variable by mistake.

**Prevention and Refactoring:**

The best way to prevent this smell is to name things after what they actually are or do, not how they work. 
A good test is: can someone understand what this function does from the name alone, without reading the body?

Martin Fowler says renaming is one of the most common refactorings he does. 
He describes three techniques for it: Change Function Declaration (p. 124) for renaming a function, Rename Variable (p. 137) for variables, and Rename Field (p. 244) for fields in a class. 
He also points out that if you can't think of a good name for something, that's usually a sign the code itself is confusing and needs to be simplified.

**Code Example: Taking Damage in a Game:**

In a small Python game I was working on, I wrote a function that handles the player getting hit. 
It takes the player's health, subtracts the damage, and makes sure health doesn't go below zero. 
It worked, but when I came back to it later I had to read the whole function to remember what it was doing, because the names didn't tell me anything.

**Before Refactoring:**
```python
def upd(h, d, a):
    n = h - (d - a)
    if n < 0:
        n = 0
    return n

h = upd(100, 30, 10)
```

What is wrong with the code?

The function is called `upd`, which could mean anything. 
The parameters are `h`, `d`, and `a`, and the result is stored in `n`. 
You can't tell this is about health and damage without reading every line and working it out. 
If someone passed the arguments in the wrong order, nothing would warn them because the names don't mean anything.

**After Refactoring:**
```python
def take_damage(health, damage, armor):
    new_health = health - (damage - armor)
    if new_health < 0:
        new_health = 0
    return new_health

health = take_damage(100, 30, 10)
```

Nothing about the logic changed, only the names. 
Now you can read `take_damage(health, damage, armor)` and know exactly what it does without looking inside. 
It's also obvious which argument is which, so it's much harder to mix them up.

**What I Learned:**
- A mysterious name doesn't break the code, but it costs time every single time someone reads it.
- Renaming is one of the easiest refactorings to do, and it's also one of the most useful.
- When I struggle to name something, it usually means the function is doing too much or I don't fully understand it yet, so that's a sign to simplify it first.
## 2. Duplicated Code

**Introduction:**

Duplicated code happens when the same or very similar code appears in multiple places. 
This often happens when developers copy and paste code to create similar features, instead of reusing existing code.

For example, if multiple levels in a game use the same code to spawn enemies, any changes to that code would have to be made in every level. 
This makes the code harder to maintain and increases the chance of mistakes.

**Prevention and Refactoring:**

One way to prevent duplicated code is to identify repeated sections and move them into a reusable function or class. 
This allows the same code to be used in multiple places without having to rewrite it each time.

Martin Fowler describes several refactoring techniques for dealing with duplicated code, including Extract Function (p. 106). 
This technique moves a section of code into its own function that can be called wherever it is needed.

In a game, for example, you could create a function that adds a certain number of enemies to a spawn queue. 
Each level could call that function with different enemy types and quantities. 
This would reduce repeated code and make it easier to maintain and add new levels.

**Code Example: Creating Enemies in a Game:**

In my CMPM 120 RPG game, I created three regular levels with different numbers of Chargers, Shooters, and Tanks. 
Each level uses three for loops to add the correct number of each enemy to a spawn queue. 
Although the enemy counts change between levels, the code structure stays the same.

This means I repeat nearly identical code in Levels 1, 2, and 3. 
If I wanted to change how enemies are added to the queue, I would have to update the code in multiple places. 
I could refactor this by creating a reusable function that adds enemies to the queue based on their type and quantity.

**Before Refactoring:**
```
// Level 1
this.spawnQueue = [];

for (let i = 0; i < 6; i++)
    this.spawnQueue.push("Charger");

for (let i = 0; i < 3; i++)
    this.spawnQueue.push("Shooter");

for (let i = 0; i < 1; i++)
    this.spawnQueue.push("Tank");

Phaser.Utils.Array.Shuffle(this.spawnQueue);
```

What is wrong with the code?

The same code is repeated in Levels 1, 2, and 3, only changing the number of each enemy. 
This creates duplicated code because each level has its own copy of the same logic. 
If I wanted to change how enemies are added to the queue, I would have to make the same changes in multiple places, which takes extra time and could lead to mistakes.

**After Refactoring:**
```
function addEnemyTypes(queue, type, count) {
    for (let i = 0; i < count; i++) {
        queue.push(type);
    }
}
```

Now each level can use the same function with different numbers.
```
// Level 1
this.spawnQueue = [];

addEnemyTypes(this.spawnQueue, "Charger", 6);
addEnemyTypes(this.spawnQueue, "Shooter", 3);
addEnemyTypes(this.spawnQueue, "Tank", 1);

Phaser.Utils.Array.Shuffle(this.spawnQueue);
```

Now levels 2 and 3 can use the same function with their own enemy counts. 
This removes the need to repeat the same logic in every level and makes the code a lot easier to maintain.
If any change is needed, we only have to update the function once.

**Python example:** the same smell in Python, the loops in level 1 and level 2 are identical except for the numbers, so the loop gets pulled into one function.

```python
# Before
# Level 1
queue = []
for i in range(6):
    queue.append("Charger")
for i in range(3):
    queue.append("Shooter")

# Level 2
queue = []
for i in range(10):
    queue.append("Charger")
for i in range(5):
    queue.append("Shooter")

# After
def add_enemies(queue, enemy_type, count):
    for i in range(count):
        queue.append(enemy_type)

# Level 1
queue = []
add_enemies(queue, "Charger", 6)
add_enemies(queue, "Shooter", 3)

# Level 2
queue = []
add_enemies(queue, "Charger", 10)
add_enemies(queue, "Shooter", 5)
```

**What I Learned:**
- Duplicated code can make a project harder to maintain because the same changes may need to be made in multiple places.
- Creating a reusable function allows me to write the logic once and use it throughout the game.
- The refactoring helps reduce duplication and makes it easier to add new levels or enemy changes in the future.

## 3. Loops
**Introduction:**
A loop smell happens when one loop does several jobs at once, like filtering, transforming, and collecting results. This usually starts with a simple loop, and then each new rule gets added inside it.

**Why it's bad:** the reader can't tell what the loop produces without reading every line, and each new rule makes the loop longer and risks changing the behavior.

**Fix:** Replace the Loop with a Pipeline (p. 231), keep each step on its own line using `filter` (keeps only the items that pass a test) and `map` (changes each item into something new), so the code reads top to bottom.

**Example:** a game wants the names of players who beat the high score of 1000. The loop checks the score, picks outs the name, and builds the list all in one block.

**JavaScript example:** the loop does three jobs in one place. If the game adds another rule, like only showing players who finished the level, the `if` gets longer and harder to read.

```javascript
// Before
const highScorers = [];

for(let i = 0; i < players.length; i++) {
    if(players[i].score > 1000) {
        highScorers.push(players[i].name);
    }
}

// After
const highScorers = players
    .filter(player => player.score > 1000)
    .map(player => player.name);
```
Now each step has its own line: keep the players who beat the high score, then get their names. `filter` and `map` each create a new array, and the final one is saved into `highScorers`. The original `players` list is never changed.

**What I learned:**
- A loop that filters, transforms, and collects at once gets harder to read with every new rule.
- A pipeline gives each step its own line, so the code can be read from top to bottom.
- Not every loop needs to change. A simple loop that only does one thing is fine as it is.


