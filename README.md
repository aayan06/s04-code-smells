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

Duplicated code is when the same chunk of code shows up in more than one place. Sometimes it’s an exact copy, sometimes it’s close with a few lines changed, but the same idea is written out twice.
Why it’s bad: you have to read both copies to see if they’re actually the same, and if you change one you have to remember to change the other. Forget one and now you have a bug that’s really hard to track down.
Fix: Extract Function (p. 106), Slide Statements (p. 223), or Pull Up Method (p. 350).
Example: say you have two functions that both get the customer name, build a greeting, and format the date before doing their own thing. If the date format changes you have to fix it in both. Pulling those lines into one function like build_header(customer, date) means there’s only one spot to change. This happens in Python, JavaScript, Java, anything, since copy paste works everywhere.
Things I found interesting in the reading:
Fowler doesn’t hedge on this one, he says if you see the same structure twice you can be sure the code will be better if you combine it. Most other smells he’s more “it depends” about.
Sometimes the first step isn’t extracting anything, it’s just moving lines around (Slide Statements) so the duplicate parts are next to each other. The duplication can be hidden by the order the code is in.
He mentions in the Large Class section that when a class has too many fields, duplicated code “cannot be far behind.” So seeing a lot of copies is usually a sign something bigger needs to be split up.

## 3. Loops

A loop code smell happens when a for or while loop tries to do too many things at once, like checking numbers, changing values, and adding them together. 

**Why it's bad:** When a loop does too many things, it's harder to understand, find mistakes, and make corrections or changes later.

**Fix:** Replace Loop with Pipeline (p. 231). This means breaking a complicated loop into smaller steps so each step has one clear job. 

**Example:** Imagine a loop that goes through a list of orders, checks which ones are completed, adds up their totals, and stores the results can be difficult to follow. Instead, you could use separate operations to filter completed orders, extract their totals, and calculate the sum. In Python, for example, using methods like filter, map, and reduce can make the code more readable. The goal isn't to get rid of every loop, but to make sure the logic is easy to understand.

**Things I found interesting in the reading:**
- Fowler points out that loops can become harder to understand when the combine several different operations into one block of code.
- I found it interesting that breaking a loop into smaller steps, can make the code almost explain itself because each operation tells you exactly what its doing instead of having you figure it out from the loop body.
- Sometimes a few separate operations are easier to understand than one big loop because each step has a clear purpose. Refactoring loops is about making the code easier to read and maintain, not just making it shorter. 
