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



## 3. Loops

A loop code smell happens when a for or while loop tries to do too many things at once, like checking numbers, changing values, and adding them together. 

** Why it's bad:** When a loop does too many things, it's harder to understand, find mistakes, and make corrections or changes later.

**Fix:** Replace Loop with Pipeline (p. 231). This means breaking a complicated loop into smaller steps so each step has one clear job. 

**Example:** Imagine a loop that goes through a list of orders, checks which ones are completed, adds up their totals, and stores the results can be difficult to follow. Instead, you could use separate operations to filter completed orders, extract their totals, and calculate the sum. In Python, for example, using methods like filter, map, and reduce can make the code more readable. The goal isn't to get rid of every loop, but to make sure the logic is easy to understand.

**Things I found interesting in the reading:**
- Fowler points out that loops can become harder to understand when the combine several different operations into one block of code.
- I found it interesting that breaking a loop into smaller steps, can make the code almost explain itself because each operation tells you exactly what its doing instead of having you figure it out from the loop body.
- Sometimes a few separate operations are easier to understand than one big loop because each step has a clear purpose. Refactoring loops is about making the code easier to read and maintain, not just making it shorter. 
