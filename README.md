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

A mysterious name is when a variable or function is named something like `x`, `tmp`, or `calc` so you can't tell what it does without reading the whole thing. Fowler says this is the first smell to fix because good names make code readable on their own.

**Why it's bad:** you waste time figuring out what the code means, and short names get mixed up easily.

**Fix:** Change Function Declaration (p. 124), Rename Variable (p. 137), or Rename Field (p. 244).

**Example:** a function called calc(d, r) could mean anything, but order_total(unit_price, quantity) tells you exactly what it does without reading the body. The same idea works in any language like Python, JavaScript, or Java. The fix is just renaming x, tmp, or proc to what the thing actually is.

**Things I found interesting in the reading:**
- Fowler and Beck say naming is one of the two hard things in programming, and that renaming is probably the refactoring they do *most often*.
- People avoid renaming because it feels like it's not worth the effort, but a good name can save hours of confusion later on.
- The coolest point: if you can't come up with a good name for something, that usually means the design itself is confusing. So struggling with a name is a hint to simplify the code, not just pick a better word.

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

**What I Learned:**
- Duplicated code can make a project harder to maintain because the same changes may need to be made in multiple places.
- Creating a reusable function allows me to write the logic once and use it throughout the game.
- The refactoring helps reduce duplication and makes it easier to add new levels or enemy changes in the future.
